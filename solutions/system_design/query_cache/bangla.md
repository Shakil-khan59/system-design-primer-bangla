# Design a key-value cache to save the results of the most recent web server queries

*নোট: এই document [system design topics](../../bangla.md#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **User** একটি search request পাঠায় resulting in a cache hit
* **User** একটি search request পাঠায় resulting in a cache miss
* **Service** উচ্চ প্রাপ্যতা রয়েছে

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
    * Popular queries প্রায় সবসময় cache এ থাকা উচিত
    * কীভাবে expire/refresh করতে হবে তা নির্ধারণ করতে হবে
* Cache থেকে serving করা fast lookups প্রয়োজন
* Machines এর মধ্যে low latency
* Cache এ limited memory
    * কী রাখতে/remove করতে হবে তা নির্ধারণ করতে হবে
    * Millions of queries cache করতে হবে
* 10 million users
* প্রতি মাসে 10 billion queries

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* Cache ordered list of key: query, value: results store করে
    * `query` - 50 bytes
    * `title` - 20 bytes
    * `snippet` - 200 bytes
    * Total: 270 bytes
* প্রতি মাসে 2.7 TB cache data যদি সমস্ত 10 billion queries unique হয় এবং সব store করা হয়
    * প্রতি search 270 bytes * প্রতি মাসে 10 billion searches
    * Assumptions state limited memory, কীভাবে contents expire করতে হবে তা নির্ধারণ করতে হবে
* প্রতি সেকেন্ডে 4,000 requests

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/KqZ3dSx.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: User sends a request resulting in a cache hit

Popular queries read latency কমাতে এবং **Reverse Index Service** এবং **Document Service** overload করা এড়াতে Redis বা Memcached এর মতো একটি **Memory Cache** থেকে serve করা যেতে পারে। Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=../../bangla.md#latency-numbers-every-programmer-should-know>1</a></sup>

যেহেতু cache এর limited capacity রয়েছে, আমরা older entries expire করতে একটি least recently used (LRU) approach ব্যবহার করব।

* **Client** **Web Server** এ একটি request পাঠায়, একটি [reverse proxy](../../bangla.md#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Query API** server এ forward করে
* **Query API** server নিম্নলিখিত কাজগুলো করে:
    * Query parse করে
        * Markup remove করে
        * Text terms এ ভেঙে দেয়
        * Typos fix করে
        * Capitalization normalize করে
        * Query boolean operations ব্যবহার করতে convert করে
    * Query match করে এমন content এর জন্য **Memory Cache** check করে
        * যদি **Memory Cache** এ একটি hit থাকে, **Memory Cache** নিম্নলিখিত কাজগুলো করে:
            * Cached entry এর position LRU list এর front এ update করে
            * Cached contents return করে
        * অন্যথায়, **Query API** নিম্নলিখিত কাজগুলো করে:
            * Query match করে এমন documents খুঁজে পেতে **Reverse Index Service** ব্যবহার করে
                * **Reverse Index Service** matching results rank করে এবং top ones return করে
            * Titles এবং snippets return করতে **Document Service** ব্যবহার করে
            * Contents সহ **Memory Cache** আপডেট করে, entry কে LRU list এর front এ placing করে

#### Cache implementation

Cache একটি doubly-linked list ব্যবহার করতে পারে: new items head এ যোগ করা হবে যখন expire করার items tail থেকে remove করা হবে। আমরা প্রতিটি linked list node এ fast lookups এর জন্য একটি hash table ব্যবহার করব।

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

**Query API Server** implementation:

```python
class QueryApi(object):

    def __init__(self, memory_cache, reverse_index_service):
        self.memory_cache = memory_cache
        self.reverse_index_service = reverse_index_service

    def parse_query(self, query):
        """Remove markup, break text into terms, deal with typos,
        normalize capitalization, convert to use boolean operations.
        """
        ...

    def process_query(self, query):
        query = self.parse_query(query)
        results = self.memory_cache.get(query)
        if results is None:
            results = self.reverse_index_service.process_search(query)
            self.memory_cache.set(query, results)
        return results
```

**Node** implementation:

```python
class Node(object):

    def __init__(self, query, results):
        self.query = query
        self.results = results
```

**LinkedList** implementation:

```python
class LinkedList(object):

    def __init__(self):
        self.head = None
        self.tail = None

    def move_to_front(self, node):
        ...

    def append_to_front(self, node):
        ...

    def remove_from_tail(self):
        ...
```

**Cache** implementation:

```python
class Cache(object):

    def __init__(self, MAX_SIZE):
        self.MAX_SIZE = MAX_SIZE
        self.size = 0
        self.lookup = {}  # key: query, value: node
        self.linked_list = LinkedList()

    def get(self, query)
        """Get the stored query result from the cache.

        Accessing a node updates its position to the front of the LRU list.
        """
        node = self.lookup[query]
        if node is None:
            return None
        self.linked_list.move_to_front(node)
        return node.results

    def set(self, results, query):
        """Set the result for the given query key in the cache.

        When updating an entry, updates its position to the front of the LRU list.
        If the entry is new and the cache is at capacity, removes the oldest entry
        before the new entry is added.
        """
        node = self.lookup[query]
        if node is not None:
            # Key exists in cache, update the value
            node.results = results
            self.linked_list.move_to_front(node)
        else:
            # Key does not exist in cache
            if self.size == self.MAX_SIZE:
                # Remove the oldest entry from the linked list and lookup
                self.lookup.pop(self.linked_list.tail.query, None)
                self.linked_list.remove_from_tail()
            else:
                self.size += 1
            # Add the new key and value
            new_node = Node(query, results)
            self.linked_list.append_to_front(new_node)
            self.lookup[query] = new_node
```

#### When to update the cache

Cache আপডেট করা উচিত যখন:

* Page contents পরিবর্তিত হয়
* Page remove করা হয় বা একটি নতুন page যোগ করা হয়
* Page rank পরিবর্তিত হয়

এই cases handle করার সবচেয়ে straightforward way হল কেবল একটি max time set করা যে একটি cached entry update হওয়ার আগে cache এ থাকতে পারে, সাধারণত time to live (TTL) হিসাবে উল্লেখ করা।

Tradeoffs এবং alternatives এর জন্য [When to update the cache](../../bangla.md#when-to-update-the-cache) দেখুন। উপরের approach [cache-aside](../../bangla.md#cache-aside) বর্ণনা করে।

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/4j99mhe.png)

**গুরুত্বপূর্ণ: শুধু initial design থেকে final design এ সরাসরি jump করবেন না!**

State করুন আপনি 1) **Benchmark/Load Test**, 2) **Profile** bottlenecks এর জন্য 3) alternatives এবং trade-offs evaluate করার সময় bottlenecks address করবেন এবং 4) repeat করবেন। [Design a system that scales to millions of users on AWS](../scaling_aws/bangla.md) দেখুন initial design iteratively scale করার একটি sample হিসাবে।

Initial design এর সাথে আপনি যে bottlenecks এর মুখোমুখি হতে পারেন এবং আপনি কীভাবে প্রতিটি address করতে পারেন তা নিয়ে আলোচনা করা গুরুত্বপূর্ণ। উদাহরণস্বরূপ, একাধিক **Web Servers** সহ একটি **Load Balancer** যোগ করা দ্বারা কী issues address করা হয়? **CDN**? **Master-Slave Replicas**? প্রতিটির জন্য alternatives এবং **Trade-Offs** কী?

আমরা design complete করতে এবং scalability issues address করতে কিছু components পরিচয় করাব। Internal load balancers clutter কমাতে দেখানো হয়নি।

*আলোচনা repeat করা এড়াতে*, main talking points, tradeoffs, এবং alternatives এর জন্য নিম্নলিখিত [system design topics](../../bangla.md#index-of-system-design-topics) দেখুন:

* [DNS](../../bangla.md#domain-name-system)
* [Load balancer](../../bangla.md#load-balancer)
* [Horizontal scaling](../../bangla.md#horizontal-scaling)
* [Web server (reverse proxy)](../../bangla.md#reverse-proxy-web-server)
* [API server (application layer)](../../bangla.md#application-layer)
* [Cache](../../bangla.md#cache)
* [Consistency patterns](../../bangla.md#consistency-patterns)
* [Availability patterns](../../bangla.md#availability-patterns)

### Expanding the Memory Cache to many machines

Heavy request load এবং প্রয়োজনীয় large amount of memory handle করতে, আমরা horizontally scale করব। আমাদের **Memory Cache** cluster এ ডেটা কীভাবে store করতে হবে তার উপর আমাদের তিনটি main options রয়েছে:

* **Cache cluster এ প্রতিটি machine এর নিজের cache আছে** - Simple, যদিও এটি likely একটি low cache hit rate এর ফলাফল দেবে।
* **Cache cluster এ প্রতিটি machine এর cache এর একটি copy আছে** - Simple, যদিও এটি memory এর inefficient use।
* **Cache [sharded](../../bangla.md#sharding) cache cluster এ সমস্ত machines জুড়ে** - আরও complex, যদিও এটি likely best option। আমরা `machine = hash(query)` ব্যবহার করে query এর cached results কোন machine এ থাকতে পারে তা নির্ধারণ করতে hashing ব্যবহার করতে পারি। আমরা likely [consistent hashing](../../bangla.md#under-development) ব্যবহার করতে চাই।

## Additional talking points

> সমস্যা scope এবং remaining time এর উপর নির্ভর করে dive into করার জন্য additional topics।

### SQL scaling patterns

* [Read replicas](../../bangla.md#master-slave-replication)
* [Federation](../../bangla.md#federation)
* [Sharding](../../bangla.md#sharding)
* [Denormalization](../../bangla.md#denormalization)
* [SQL Tuning](../../bangla.md#sql-tuning)

#### NoSQL

* [Key-value store](../../bangla.md#key-value-store)
* [Document store](../../bangla.md#document-store)
* [Wide column store](../../bangla.md#wide-column-store)
* [Graph database](../../bangla.md#graph-database)
* [SQL vs NoSQL](../../bangla.md#sql-or-nosql)

### Caching

* কোথায় cache করতে হবে
    * [Client caching](../../bangla.md#client-caching)
    * [CDN caching](../../bangla.md#cdn-caching)
    * [Web server caching](../../bangla.md#web-server-caching)
    * [Database caching](../../bangla.md#database-caching)
    * [Application caching](../../bangla.md#application-caching)
* কী cache করতে হবে
    * [Caching at the database query level](../../bangla.md#caching-at-the-database-query-level)
    * [Caching at the object level](../../bangla.md#caching-at-the-object-level)
* কখন cache update করতে হবে
    * [Cache-aside](../../bangla.md#cache-aside)
    * [Write-through](../../bangla.md#write-through)
    * [Write-behind (write-back)](../../bangla.md#write-behind-write-back)
    * [Refresh ahead](../../bangla.md#refresh-ahead)

### Asynchronism and microservices

* [Message queues](../../bangla.md#message-queues)
* [Task queues](../../bangla.md#task-queues)
* [Back pressure](../../bangla.md#back-pressure)
* [Microservices](../../bangla.md#microservices)

### Communications

* Tradeoffs নিয়ে আলোচনা করুন:
    * Clients এর সাথে external communication - [HTTP APIs following REST](../../bangla.md#representational-state-transfer-rest)
    * Internal communications - [RPC](../../bangla.md#remote-procedure-call-rpc)
* [Service discovery](../../bangla.md#service-discovery)

### Security

[security section](../../bangla.md#security) দেখুন।

### Latency numbers

[Latency numbers every programmer should know](../../bangla.md#latency-numbers-every-programmer-should-know) দেখুন।

### Ongoing

* Bottlenecks আসার সাথে সাথে address করতে আপনার system benchmark এবং monitor করা চালিয়ে যান
* Scaling একটি iterative process

