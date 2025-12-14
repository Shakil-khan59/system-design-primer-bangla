# Design Pastebin.com (or Bit.ly)

*নোট: এই document [system design topics](../../bangla.md#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

**Design Bit.ly** - একটি অনুরূপ প্রশ্ন, তবে pastebin original unshortened url এর পরিবর্তে paste contents সংরক্ষণ করতে প্রয়োজন।

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **User** একটি text block enters করে এবং একটি randomly generated link পায়
    * Expiration
        * Default setting expire হয় না
        * Optionally একটি timed expiration set করতে পারে
* **User** একটি paste এর url enters করে এবং contents views করে
* **User** anonymous
* **Service** pages এর analytics tracks করে
    * Monthly visit stats
* **Service** expired pastes delete করে
* **Service** উচ্চ প্রাপ্যতা রয়েছে

#### Out of scope

* **User** একটি account এর জন্য register করে
    * **User** email verify করে
* **User** একটি registered account এ log in করে
    * **User** document edit করে
* **User** visibility set করতে পারে
* **User** shortlink set করতে পারে

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
* একটি short link follow করা দ্রুত হওয়া উচিত
* Pastes শুধুমাত্র text
* Page view analytics realtime হওয়ার প্রয়োজন নেই
* 10 million users
* প্রতি মাসে 10 million paste writes
* প্রতি মাসে 100 million paste reads
* 10:1 read to write ratio

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* প্রতি paste size
    * প্রতি paste 1 KB content
    * `shortlink` - 7 bytes
    * `expiration_length_in_minutes` - 4 bytes
    * `created_at` - 5 bytes
    * `paste_path` - 255 bytes
    * total = ~1.27 KB
* প্রতি মাসে 12.7 GB নতুন paste content
    * প্রতি paste 1.27 KB * প্রতি মাসে 10 million pastes
    * 3 বছরে ~450 GB নতুন paste content
    * 3 বছরে 360 million shortlinks
    * ধরে নিন বেশিরভাগ existing ones এ updates এর পরিবর্তে নতুন pastes
* গড়ে প্রতি সেকেন্ডে 4 paste writes
* গড়ে প্রতি সেকেন্ডে 40 read requests

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/BKsBnmG.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: User enters a block of text and gets a randomly generated link

আমরা একটি [relational database](../../bangla.md#relational-database-management-system-rdbms) একটি large hash table হিসাবে ব্যবহার করতে পারি, generated url কে paste file ধারণকারী file server এবং path এ mapping করে।

একটি file server manage করার পরিবর্তে, আমরা Amazon S3 এর মতো একটি managed **Object Store** বা একটি [NoSQL document store](../../bangla.md#document-store) ব্যবহার করতে পারি।

একটি relational database একটি large hash table হিসাবে কাজ করার একটি alternative, আমরা একটি [NoSQL key-value store](../../bangla.md#key-value-store) ব্যবহার করতে পারি। আমাদের [SQL বা NoSQL বেছে নেওয়ার tradeoffs](../../bangla.md#sql-or-nosql) নিয়ে আলোচনা করা উচিত। নিম্নলিখিত আলোচনা relational database approach ব্যবহার করে।

* **Client** **Web Server** এ একটি create paste request পাঠায়, একটি [reverse proxy](../../bangla.md#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Write API** server এ forward করে
* **Write API** server নিম্নলিখিত কাজগুলো করে:
    * একটি unique url generate করে
        * **SQL Database** এ duplicate খুঁজে url unique কিনা check করে
        * যদি url unique না হয়, এটি another url generate করে
        * যদি আমরা একটি custom url support করি, আমরা user-supplied ব্যবহার করতে পারি (এছাড়াও duplicate check করুন)
    * **SQL Database** `pastes` table এ save করে
    * **Object Store** এ paste data save করে
    * url ফেরত দেয়

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

`pastes` table নিম্নলিখিত structure থাকতে পারে:

```
shortlink char(7) NOT NULL
expiration_length_in_minutes int NOT NULL
created_at datetime NOT NULL
paste_path varchar(255) NOT NULL
PRIMARY KEY(shortlink)
```

Primary key `shortlink` column এর উপর ভিত্তি করে set করা একটি [index](../../bangla.md#use-good-indices) তৈরি করে যা database uniqueness enforce করতে ব্যবহার করে। আমরা `created_at` এ একটি additional index তৈরি করব lookups দ্রুত করতে (entire table scan করার পরিবর্তে log-time) এবং ডেটা memory এ রাখতে। Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=../../bangla.md#latency-numbers-every-programmer-should-know>1</a></sup>

Unique url generate করতে, আমরা করতে পারি:

* User এর ip_address + timestamp এর [**MD5**](https://en.wikipedia.org/wiki/MD5) hash নিন
    * MD5 একটি widely used hashing function যা একটি 128-bit hash value তৈরি করে
    * MD5 uniformly distributed
    * Alternatively, আমরা randomly-generated data এর MD5 hash নিতে পারি
* MD5 hash কে [**Base 62**](https://www.kerstner.at/2012/07/shortening-strings-using-base-62-encoding/) encode করুন
    * Base 62 `[a-zA-Z0-9]` এ encode করে যা urls এর জন্য ভালো কাজ করে, special characters escape করার প্রয়োজন দূর করে
    * Original input এর জন্য শুধুমাত্র একটি hash result রয়েছে এবং Base 62 deterministic (randomness জড়িত নয়)
    * Base 64 আরেকটি popular encoding কিন্তু urls এর জন্য issues প্রদান করে কারণ additional `+` এবং `/` characters
    * নিম্নলিখিত [Base 62 pseudocode](http://stackoverflow.com/questions/742013/how-to-code-a-url-shortener) O(k) time এ চলে যেখানে k হল digits সংখ্যা = 7:

```python
def base_encode(num, base=62):
    digits = []
    while num > 0
      remainder = modulo(num, base)
      digits.push(remainder)
      num = divide(num, base)
    digits = digits.reverse
```

* Output এর প্রথম 7 characters নিন, যা 62^7 possible values এর ফলাফল দেয় এবং 3 বছরে 360 million shortlinks এর আমাদের constraint handle করার জন্য যথেষ্ট হওয়া উচিত:

```python
url = base_encode(md5(ip_address+timestamp))[:URL_LENGTH]
```

আমরা একটি public [**REST API**](../../bangla.md#representational-state-transfer-rest) ব্যবহার করব:

```
$ curl -X POST --data '{ "expiration_length_in_minutes": "60", \
    "paste_contents": "Hello World!" }' https://pastebin.com/api/v1/paste
```

Response:

```
{
    "shortlink": "foobar"
}
```

Internal communications এর জন্য, আমরা [Remote Procedure Calls](../../bangla.md#remote-procedure-call-rpc) ব্যবহার করতে পারি।

### Use case: User enters a paste's url and views the contents

* **Client** **Web Server** এ একটি get paste request পাঠায়
* **Web Server** request **Read API** server এ forward করে
* **Read API** server নিম্নলিখিত কাজগুলো করে:
    * Generated url এর জন্য **SQL Database** check করে
        * যদি url **SQL Database** এ থাকে, **Object Store** থেকে paste contents fetch করে
        * অন্যথায়, user এর জন্য একটি error message ফেরত দেয়

REST API:

```
$ curl https://pastebin.com/api/v1/paste?shortlink=foobar
```

Response:

```
{
    "paste_contents": "Hello World"
    "created_at": "YYYY-MM-DD HH:MM:SS"
    "expiration_length_in_minutes": "60"
}
```

### Use case: Service tracks analytics of pages

যেহেতু realtime analytics একটি requirement নয়, আমরা কেবল **Web Server** logs **MapReduce** করে hit counts generate করতে পারি।

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

```python
class HitCounts(MRJob):

    def extract_url(self, line):
        """Extract the generated url from the log line."""
        ...

    def extract_year_month(self, line):
        """Return the year and month portions of the timestamp."""
        ...

    def mapper(self, _, line):
        """Parse each log line, extract and transform relevant lines.

        Emit key value pairs of the form:

        (2016-01, url0), 1
        (2016-01, url0), 1
        (2016-01, url1), 1
        """
        url = self.extract_url(line)
        period = self.extract_year_month(line)
        yield (period, url), 1

    def reducer(self, key, values):
        """Sum values for each key.

        (2016-01, url0), 2
        (2016-01, url1), 1
        """
        yield key, sum(values)
```

### Use case: Service deletes expired pastes

Expired pastes delete করতে, আমরা কেবল **SQL Database** scan করতে পারি সমস্ত entries এর জন্য যাদের expiration timestamp current timestamp এর চেয়ে পুরানো। সমস্ত expired entries তারপর table থেকে delete (বা expired হিসাবে marked) করা হবে।

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/4edXG0T.png)

**গুরুত্বপূর্ণ: শুধু initial design থেকে final design এ সরাসরি jump করবেন না!**

State করুন আপনি এটি iteratively করবেন: 1) **Benchmark/Load Test**, 2) **Profile** bottlenecks এর জন্য 3) alternatives এবং trade-offs evaluate করার সময় bottlenecks address করুন, এবং 4) repeat করুন। [Design a system that scales to millions of users on AWS](../scaling_aws/bangla.md) দেখুন initial design iteratively scale করার একটি sample হিসাবে।

Initial design এর সাথে আপনি যে bottlenecks এর মুখোমুখি হতে পারেন এবং আপনি কীভাবে প্রতিটি address করতে পারেন তা নিয়ে আলোচনা করা গুরুত্বপূর্ণ। উদাহরণস্বরূপ, একাধিক **Web Servers** সহ একটি **Load Balancer** যোগ করা দ্বারা কী issues address করা হয়? **CDN**? **Master-Slave Replicas**? প্রতিটির জন্য alternatives এবং **Trade-Offs** কী?

আমরা design complete করতে এবং scalability issues address করতে কিছু components পরিচয় করাব। Internal load balancers clutter কমাতে দেখানো হয়নি।

*আলোচনা repeat করা এড়াতে*, main talking points, tradeoffs, এবং alternatives এর জন্য নিম্নলিখিত [system design topics](../../bangla.md#index-of-system-design-topics) দেখুন:

* [DNS](../../bangla.md#domain-name-system)
* [CDN](../../bangla.md#content-delivery-network)
* [Load balancer](../../bangla.md#load-balancer)
* [Horizontal scaling](../../bangla.md#horizontal-scaling)
* [Web server (reverse proxy)](../../bangla.md#reverse-proxy-web-server)
* [API server (application layer)](../../bangla.md#application-layer)
* [Cache](../../bangla.md#cache)
* [Relational database management system (RDBMS)](../../bangla.md#relational-database-management-system-rdbms)
* [SQL write master-slave failover](../../bangla.md#fail-over)
* [Master-slave replication](../../bangla.md#master-slave-replication)
* [Consistency patterns](../../bangla.md#consistency-patterns)
* [Availability patterns](../../bangla.md#availability-patterns)

**Analytics Database** Amazon Redshift বা Google BigQuery এর মতো একটি data warehousing solution ব্যবহার করতে পারে।

Amazon S3 এর মতো একটি **Object Store** প্রতি মাসে 12.7 GB নতুন content এর constraint comfortably handle করতে পারে।

40 *average* read requests per second (peak এ higher) address করতে, popular content এর traffic **Memory Cache** দ্বারা handle করা উচিত database এর পরিবর্তে। **Memory Cache** unevenly distributed traffic এবং traffic spikes handle করতেও উপযোগী। **SQL Read Replicas** cache misses handle করতে সক্ষম হওয়া উচিত, যতক্ষণ replicas writes replicate করতে bogged down না হয়।

4 *average* paste writes per second (peak এ higher) একটি single **SQL Write Master-Slave** এর জন্য do-able হওয়া উচিত। অন্যথায়, আমাদের additional SQL scaling patterns employ করতে হবে:

* [Federation](../../bangla.md#federation)
* [Sharding](../../bangla.md#sharding)
* [Denormalization](../../bangla.md#denormalization)
* [SQL Tuning](../../bangla.md#sql-tuning)

আমাদের কিছু ডেটা একটি **NoSQL Database** এ move করারও বিবেচনা করা উচিত।

## Additional talking points

> সমস্যা scope এবং remaining time এর উপর নির্ভর করে dive into করার জন্য additional topics।

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

