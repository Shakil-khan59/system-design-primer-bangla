# Design the data structures for a social network

*নোট: এই document [system design topics](../../bangla.md#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন。
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **User** কাউকে search করে এবং searched person এ shortest path sees করে
* **Service** উচ্চ প্রাপ্যতা রয়েছে

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
    * কিছু searches অন্যরা শুধুমাত্র একবার executed হওয়ার তুলনায় আরও popular
* Graph data একটি single machine এ fit হবে না
* Graph edges unweighted
* 100 million users
* User প্রতি 50 friends average
* প্রতি মাসে 1 billion friend searches

Exercise more traditional systems এর ব্যবহার - [GraphQL](http://graphql.org/) বা [Neo4j](https://neo4j.com/) এর মতো graph-specific solutions ব্যবহার করবেন না

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* 5 billion friend relationships
    * 100 million users * user প্রতি 50 friends average
* প্রতি সেকেন্ডে 400 search requests

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/wxXyq2J.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: User searches for someone and sees the shortest path to the searched person

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

Millions of users (vertices) এবং billions of friend relationships (edges) এর constraint ছাড়া, আমরা এই unweighted shortest path task একটি general BFS approach দিয়ে solve করতে পারি:

```python
class Graph(Graph):

    def shortest_path(self, source, dest):
        if source is None or dest is None:
            return None
        if source is dest:
            return [source.key]
        prev_node_keys = self._shortest_path(source, dest)
        if prev_node_keys is None:
            return None
        else:
            path_ids = [dest.key]
            prev_node_key = prev_node_keys[dest.key]
            while prev_node_key is not None:
                path_ids.append(prev_node_key)
                prev_node_key = prev_node_keys[prev_node_key]
            return path_ids[::-1]

    def _shortest_path(self, source, dest):
        queue = deque()
        queue.append(source)
        prev_node_keys = {source.key: None}
        source.visit_state = State.visited
        while queue:
            node = queue.popleft()
            if node is dest:
                return prev_node_keys
            prev_node = node
            for adj_node in node.adj_nodes.values():
                if adj_node.visit_state == State.unvisited:
                    queue.append(adj_node)
                    prev_node_keys[adj_node.key] = prev_node.key
                    adj_node.visit_state = State.visited
        return None
```

আমরা সমস্ত users একই machine এ fit করতে পারব না, আমাদের [shard](../../bangla.md#sharding) users **Person Servers** জুড়ে করতে হবে এবং একটি **Lookup Service** দিয়ে access করতে হবে।

* **Client** **Web Server** এ একটি request পাঠায়, একটি [reverse proxy](../../bangla.md#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Search API** server এ forward করে
* **Search API** server request **User Graph Service** এ forward করে
* **User Graph Service** নিম্নলিখিত কাজগুলো করে:
    * Current user এর info stored আছে এমন **Person Server** খুঁজে পেতে **Lookup Service** ব্যবহার করে
    * Current user এর `friend_ids` list retrieve করতে appropriate **Person Server** খুঁজে পায়
    * Current user কে `source` হিসাবে এবং current user এর `friend_ids` কে প্রতিটি `adjacent_node` এর ids হিসাবে ব্যবহার করে একটি BFS search run করে
    * একটি প্রদত্ত id থেকে `adjacent_node` পেতে:
        * **User Graph Service** *আবার* প্রদত্ত id match করে এমন `adjacent_node` কোন **Person Server** store করে তা নির্ধারণ করতে **Lookup Service** এর সাথে communicate করতে হবে (optimization এর potential)

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

**Note**: Error handling simplicity এর জন্য নিচে excluded। জিজ্ঞাসা করুন যদি আপনার proper error handing code করা উচিত।

**Lookup Service** implementation:

```python
class LookupService(object):

    def __init__(self):
        self.lookup = self._init_lookup()  # key: person_id, value: person_server

    def _init_lookup(self):
        ...

    def lookup_person_server(self, person_id):
        return self.lookup[person_id]
```

**Person Server** implementation:

```python
class PersonServer(object):

    def __init__(self):
        self.people = {}  # key: person_id, value: person

    def add_person(self, person):
        ...

    def people(self, ids):
        results = []
        for id in ids:
            if id in self.people:
                results.append(self.people[id])
        return results
```

**Person** implementation:

```python
class Person(object):

    def __init__(self, id, name, friend_ids):
        self.id = id
        self.name = name
        self.friend_ids = friend_ids
```

**User Graph Service** implementation:

```python
class UserGraphService(object):

    def __init__(self, lookup_service):
        self.lookup_service = lookup_service

    def person(self, person_id):
        person_server = self.lookup_service.lookup_person_server(person_id)
        return person_server.people([person_id])

    def shortest_path(self, source_key, dest_key):
        if source_key is None or dest_key is None:
            return None
        if source_key is dest_key:
            return [source_key]
        prev_node_keys = self._shortest_path(source_key, dest_key)
        if prev_node_keys is None:
            return None
        else:
            # Iterate through the path_ids backwards, starting at dest_key
            path_ids = [dest_key]
            prev_node_key = prev_node_keys[dest_key]
            while prev_node_key is not None:
                path_ids.append(prev_node_key)
                prev_node_key = prev_node_keys[prev_node_key]
            # Reverse the list since we iterated backwards
            return path_ids[::-1]

    def _shortest_path(self, source_key, dest_key, path):
        # Use the id to get the Person
        source = self.person(source_key)
        # Update our bfs queue
        queue = deque()
        queue.append(source)
        # prev_node_keys keeps track of each hop from
        # the source_key to the dest_key
        prev_node_keys = {source_key: None}
        # We'll use visited_ids to keep track of which nodes we've
        # visited, which can be different from a typical bfs where
        # this can be stored in the node itself
        visited_ids = set()
        visited_ids.add(source.id)
        while queue:
            node = queue.popleft()
            if node.key is dest_key:
                return prev_node_keys
            prev_node = node
            for friend_id in node.friend_ids:
                if friend_id not in visited_ids:
                    friend_node = self.person(friend_id)
                    queue.append(friend_node)
                    prev_node_keys[friend_id] = prev_node.key
                    visited_ids.add(friend_id)
        return None
```

আমরা একটি public [**REST API**](../../bangla.md#representational-state-transfer-rest) ব্যবহার করব:

```
$ curl https://social.com/api/v1/friend_search?person_id=1234
```

Response:

```
{
    "person_id": "100",
    "name": "foo",
    "link": "https://social.com/foo",
},
{
    "person_id": "53",
    "name": "bar",
    "link": "https://social.com/bar",
},
{
    "person_id": "1234",
    "name": "baz",
    "link": "https://social.com/baz",
},
```

Internal communications এর জন্য, আমরা [Remote Procedure Calls](../../bangla.md#remote-procedure-call-rpc) ব্যবহার করতে পারি।

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/cdCv5g7.png)

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

400 *average* read requests per second (peak এ higher) constraint address করতে, person data Redis বা Memcached এর মতো একটি **Memory Cache** থেকে serve করা যেতে পারে response times কমাতে এবং downstream services এ traffic কমাতে। এটি বিশেষভাবে উপযোগী হতে পারে যারা succession এ multiple searches করে এবং যারা well-connected।

নিম্নে আরও optimizations রয়েছে:

* Subsequent lookups দ্রুত করতে **Memory Cache** এ complete বা partial BFS traversals store করুন
* Subsequent lookups দ্রুত করতে একটি **NoSQL Database** এ offline batch compute তারপর complete বা partial BFS traversals store করুন
* একই **Person Server** এ hosted friend lookups একসাথে batching করে machine jumps কমাতে
    * [Shard](../../bangla.md#sharding) **Person Servers** location দ্বারা এটি আরও উন্নত করতে, কারণ friends সাধারণত একে অপরের কাছাকাছি থাকে
* একই সময়ে দুটি BFS searches করুন, একটি source থেকে শুরু করে, এবং একটি destination থেকে, তারপর দুটি paths merge করুন
* বড় সংখ্যক friends সহ people থেকে BFS search শুরু করুন, কারণ তারা current user এবং search target এর মধ্যে [degrees of separation](https://en.wikipedia.org/wiki/Six_degrees_of_separation) সংখ্যা কমাতে likely
* User জিজ্ঞাসা করার আগে time বা number of hops এর উপর ভিত্তি করে একটি limit set করুন যদি তারা continue searching করতে চায়, কারণ searching কিছু ক্ষেত্রে একটি considerable amount of time নিতে পারে
* [Neo4j](https://neo4j.com/) এর মতো একটি **Graph Database** বা [GraphQL](http://graphql.org/) এর মতো একটি graph-specific query language ব্যবহার করুন (যদি **Graph Databases** ব্যবহার করা থেকে প্রতিরোধ করার কোনো constraint না থাকে)

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

