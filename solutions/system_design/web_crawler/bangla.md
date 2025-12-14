# Design a web crawler

*নোট: এই document [system design topics](../../bangla.md#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **Service** urls এর একটি list crawls করে:
    * Words থেকে search terms ধারণকারী pages এর reverse index generate করে
    * Pages এর জন্য titles এবং snippets generate করে
        * Title এবং snippets static, তারা search query এর উপর ভিত্তি করে পরিবর্তিত হয় না
* **User** একটি search term inputs করে এবং crawler generate করা titles এবং snippets সহ relevant pages এর একটি list sees করে
    * শুধুমাত্র এই use case এর জন্য high level components এবং interactions sketch করুন, depth এ যাওয়ার প্রয়োজন নেই
* **Service** উচ্চ প্রাপ্যতা রয়েছে

#### Out of scope

* Search analytics
* Personalized search results
* Page rank

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
    * কিছু searches খুব popular, যখন অন্যরা শুধুমাত্র একবার executed হয়
* শুধুমাত্র anonymous users support করুন
* Search results generate করা দ্রুত হওয়া উচিত
* Web crawler একটি infinite loop এ stuck হওয়া উচিত নয়
    * Graph একটি cycle ধারণ করলে আমরা একটি infinite loop এ stuck হয়ে যাই
* 1 billion links crawl করতে হবে
    * Freshness নিশ্চিত করতে pages regularly crawl করা প্রয়োজন
    * গড়ে সপ্তাহে একবার refresh rate, popular sites এর জন্য আরও frequent
        * প্রতি মাসে 4 billion links crawled
    * প্রতি web page গড় stored size: 500 KB
        * Simplicity এর জন্য, changes নতুন pages হিসাবে count করুন
* প্রতি মাসে 100 billion searches

Exercise more traditional systems এর ব্যবহার - [solr](http://lucene.apache.org/solr/) বা [nutch](http://nutch.apache.org/) এর মতো existing systems ব্যবহার করবেন না।

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* প্রতি মাসে 2 PB stored page content
    * প্রতি page 500 KB * প্রতি মাসে 4 billion links crawled
    * 3 বছরে 72 PB stored page content
* প্রতি সেকেন্ডে 1,600 write requests
* প্রতি সেকেন্ডে 40,000 search requests

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/xjdAAUv.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: Service crawls a list of urls

আমরা ধরে নেব আমাদের একটি initial list `links_to_crawl` রয়েছে যা initially overall site popularity এর উপর ভিত্তি করে ranked। যদি এটি একটি reasonable assumption না হয়, আমরা popular sites দিয়ে crawler seed করতে পারি যা outside content এ link করে যেমন [Yahoo](https://www.yahoo.com/), [DMOZ](http://www.dmoz.org/), ইত্যাদি।

আমরা processed links এবং তাদের page signatures store করতে একটি table `crawled_links` ব্যবহার করব।

আমরা `links_to_crawl` এবং `crawled_links` একটি key-value **NoSQL Database** এ store করতে পারি। `links_to_crawl` এ ranked links এর জন্য, আমরা page links এর ranking maintain করতে sorted sets সহ [Redis](https://redis.io/) ব্যবহার করতে পারি। আমাদের [SQL বা NoSQL বেছে নেওয়ার use cases এবং tradeoffs](../../bangla.md#sql-or-nosql) নিয়ে আলোচনা করা উচিত।

* **Crawler Service** একটি loop এ নিম্নলিখিত কাজগুলো করে প্রতিটি page link process করে:
    * Crawl করার জন্য top ranked page link নেয়
        * Similar page signature সহ একটি entry এর জন্য **NoSQL Database** এ `crawled_links` check করে
            * যদি আমাদের একটি similar page থাকে, page link এর priority reduce করে
                * এটি আমাদের একটি cycle এ যাওয়া থেকে প্রতিরোধ করে
                * Continue করুন
            * অন্যথায়, link crawl করে
                * একটি [reverse index](https://en.wikipedia.org/wiki/Search_engine_indexing) generate করতে **Reverse Index Service** queue এ একটি job যোগ করে
                * একটি static title এবং snippet generate করতে **Document Service** queue এ একটি job যোগ করে
                * Page signature generate করে
                * **NoSQL Database** এ `links_to_crawl` থেকে link remove করে
                * **NoSQL Database** এ `crawled_links` এ page link এবং signature insert করে

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

`PagesDataStore` **Crawler Service** এর মধ্যে একটি abstraction যা **NoSQL Database** ব্যবহার করে:

```python
class PagesDataStore(object):

    def __init__(self, db);
        self.db = db
        ...

    def add_link_to_crawl(self, url):
        """Add the given link to `links_to_crawl`."""
        ...

    def remove_link_to_crawl(self, url):
        """Remove the given link from `links_to_crawl`."""
        ...

    def reduce_priority_link_to_crawl(self, url)
        """Reduce the priority of a link in `links_to_crawl` to avoid cycles."""
        ...

    def extract_max_priority_page(self):
        """Return the highest priority link in `links_to_crawl`."""
        ...

    def insert_crawled_link(self, url, signature):
        """Add the given link to `crawled_links`."""
        ...

    def crawled_similar(self, signature):
        """Determine if we've already crawled a page matching the given signature"""
        ...
```

`Page` **Crawler Service** এর মধ্যে একটি abstraction যা একটি page, তার contents, child urls, এবং signature encapsulate করে:

```python
class Page(object):

    def __init__(self, url, contents, child_urls, signature):
        self.url = url
        self.contents = contents
        self.child_urls = child_urls
        self.signature = signature
```

`Crawler` **Crawler Service** এর মধ্যে main class, `Page` এবং `PagesDataStore` দিয়ে composed।

```python
class Crawler(object):

    def __init__(self, data_store, reverse_index_queue, doc_index_queue):
        self.data_store = data_store
        self.reverse_index_queue = reverse_index_queue
        self.doc_index_queue = doc_index_queue

    def create_signature(self, page):
        """Create signature based on url and contents."""
        ...

    def crawl_page(self, page):
        for url in page.child_urls:
            self.data_store.add_link_to_crawl(url)
        page.signature = self.create_signature(page)
        self.data_store.remove_link_to_crawl(page.url)
        self.data_store.insert_crawled_link(page.url, page.signature)

    def crawl(self):
        while True:
            page = self.data_store.extract_max_priority_page()
            if page is None:
                break
            if self.data_store.crawled_similar(page.signature):
                self.data_store.reduce_priority_link_to_crawl(page.url)
            else:
                self.crawl_page(page)
```

### Handling duplicates

আমাদের careful হতে হবে web crawler একটি infinite loop এ stuck না হয়, যা ঘটে যখন graph একটি cycle ধারণ করে।

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

আমরা duplicate urls remove করতে চাই:

* ছোট lists এর জন্য আমরা `sort | unique` এর মতো কিছু ব্যবহার করতে পারি
* 1 billion links crawl করার সাথে, আমরা শুধুমাত্র frequency 1 আছে এমন entries output করতে **MapReduce** ব্যবহার করতে পারি

```python
class RemoveDuplicateUrls(MRJob):

    def mapper(self, _, line):
        yield line, 1

    def reducer(self, key, values):
        total = sum(values)
        if total == 1:
            yield key, total
```

Duplicate content detect করা আরও complex। আমরা page এর contents এর উপর ভিত্তি করে একটি signature generate করতে পারি এবং similarity এর জন্য সেই দুটি signature compare করতে পারি। কিছু potential algorithms হল [Jaccard index](https://en.wikipedia.org/wiki/Jaccard_index) এবং [cosine similarity](https://en.wikipedia.org/wiki/Cosine_similarity)।

### Determining when to update the crawl results

Pages regularly crawl করা প্রয়োজন freshness নিশ্চিত করতে। Crawl results একটি `timestamp` field থাকতে পারে যা শেষবার একটি page crawl করা হয়েছিল তা নির্দেশ করে। একটি default time period পরে, বলুন এক সপ্তাহ, সমস্ত pages refresh করা উচিত। Frequently updated বা আরও popular sites shorter intervals এ refresh করা যেতে পারে।

যদিও আমরা analytics এর details এ dive করব না, আমরা একটি particular page update হওয়ার আগে mean time নির্ধারণ করতে কিছু data mining করতে পারি, এবং সেই statistic ব্যবহার করে page কতবার re-crawl করতে হবে তা নির্ধারণ করতে পারি।

আমরা একটি `Robots.txt` file support করারও বেছে নিতে পারি যা webmasters কে crawl frequency control দেয়।

### Use case: User inputs a search term and sees a list of relevant pages with titles and snippets

* **Client** **Web Server** এ একটি request পাঠায়, একটি [reverse proxy](../../bangla.md#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Query API** server এ forward করে
* **Query API** server নিম্নলিখিত কাজগুলো করে:
    * Query parse করে
        * Markup remove করে
        * Text terms এ ভেঙে দেয়
        * Typos fix করে
        * Capitalization normalize করে
        * Query boolean operations ব্যবহার করতে convert করে
    * Query match করে এমন documents খুঁজে পেতে **Reverse Index Service** ব্যবহার করে
        * **Reverse Index Service** matching results rank করে এবং top ones return করে
    * Titles এবং snippets return করতে **Document Service** ব্যবহার করে

আমরা একটি public [**REST API**](../../bangla.md#representational-state-transfer-rest) ব্যবহার করব:

```
$ curl https://search.com/api/v1/search?query=hello+world
```

Response:

```
{
    "title": "foo's title",
    "snippet": "foo's snippet",
    "link": "https://foo.com",
},
{
    "title": "bar's title",
    "snippet": "bar's snippet",
    "link": "https://bar.com",
},
{
    "title": "baz's title",
    "snippet": "baz's snippet",
    "link": "https://baz.com",
},
```

Internal communications এর জন্য, আমরা [Remote Procedure Calls](../../bangla.md#remote-procedure-call-rpc) ব্যবহার করতে পারি।

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/bWxPtQA.png)

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
* [NoSQL](../../bangla.md#nosql)
* [Consistency patterns](../../bangla.md#consistency-patterns)
* [Availability patterns](../../bangla.md#availability-patterns)

কিছু searches খুব popular, যখন অন্যরা শুধুমাত্র একবার executed হয়। Popular queries response times কমাতে এবং **Reverse Index Service** এবং **Document Service** overload করা এড়াতে Redis বা Memcached এর মতো একটি **Memory Cache** থেকে serve করা যেতে পারে। **Memory Cache** unevenly distributed traffic এবং traffic spikes handle করতেও উপযোগী। Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=../../bangla.md#latency-numbers-every-programmer-should-know>1</a></sup>

নিম্নে **Crawling Service** এ আরও কয়েকটি optimizations রয়েছে:

* Data size এবং request load handle করতে, **Reverse Index Service** এবং **Document Service** সম্ভবত sharding এবং federation এর heavy use করতে হবে।
* DNS lookup একটি bottleneck হতে পারে, **Crawler Service** তার নিজের DNS lookup রাখতে পারে যা periodically refreshed হয়
* **Crawler Service** একবারে অনেক open connections রাখে performance উন্নত করতে এবং memory usage কমাতে পারে, [connection pooling](https://en.wikipedia.org/wiki/Connection_pool) হিসাবে উল্লেখ করা
    * [UDP](../../bangla.md#user-datagram-protocol-udp) এ switching performance boost করতে পারে
* Web crawling bandwidth intensive, high throughput sustain করার জন্য যথেষ্ট bandwidth আছে তা নিশ্চিত করুন

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

