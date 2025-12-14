# Design Amazon's sales rank by category feature

*নোট: এই document [system design topics](https://github.com/Shakil-khan59/system-design-primer-bangla#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use case handle করার জন্য সমস্যাটি scope করব

* **Service** past week এর most popular products category দ্বারা calculate করে
* **User** past week এর most popular products category দ্বারা views করে
* **Service** উচ্চ প্রাপ্যতা রয়েছে

#### Out of scope

* General e-commerce site
    * শুধুমাত্র sales rank calculate করার জন্য components design করুন

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
* Items multiple categories এ থাকতে পারে
* Items categories পরিবর্তন করতে পারে না
* কোনো subcategories নেই অর্থাৎ `foo/bar/baz`
* Results hourly আপডেট করা আবশ্যক
    * আরও popular products আরও frequently আপডেট করা প্রয়োজন হতে পারে
* 10 million products
* 1000 categories
* প্রতি মাসে 1 billion transactions
* প্রতি মাসে 100 billion read requests
* 100:1 read to write ratio

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* প্রতি transaction size:
    * `created_at` - 5 bytes
    * `product_id` - 8 bytes
    * `category_id` - 4 bytes
    * `seller_id` - 8 bytes
    * `buyer_id` - 8 bytes
    * `quantity` - 4 bytes
    * `total_price` - 5 bytes
    * Total: ~40 bytes
* প্রতি মাসে 40 GB নতুন transaction content
    * প্রতি transaction 40 bytes * প্রতি মাসে 1 billion transactions
    * 3 বছরে 1.44 TB নতুন transaction content
    * ধরে নিন বেশিরভাগ existing ones এ updates এর পরিবর্তে নতুন transactions
* গড়ে প্রতি সেকেন্ডে 400 transactions
* গড়ে প্রতি সেকেন্ডে 40,000 read requests

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/vwMa1Qu.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: Service calculates the past week's most popular products by category

আমরা raw **Sales API** server log files একটি managed **Object Store** যেমন Amazon S3 এ store করতে পারি, আমাদের নিজের distributed file system manage করার পরিবর্তে।

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

আমরা ধরে নেব এটি একটি sample log entry, tab delimited:

```
timestamp   product_id  category_id    qty     total_price   seller_id    buyer_id
t1          product1    category1      2       20.00         1            1
t2          product1    category2      2       20.00         2            2
t2          product1    category2      1       10.00         2            3
t3          product2    category1      3        7.00         3            4
t4          product3    category2      7        2.00         4            5
t5          product4    category1      1        5.00         5            6
...
```

**Sales Rank Service** **MapReduce** ব্যবহার করতে পারে, **Sales API** server log files input হিসাবে ব্যবহার করে এবং results একটি **SQL Database** এ aggregate table `sales_rank` এ writing করে। আমাদের [SQL বা NoSQL বেছে নেওয়ার use cases এবং tradeoffs](https://github.com/Shakil-khan59/system-design-primer-bangla#sql-or-nosql) নিয়ে আলোচনা করা উচিত।

আমরা একটি multi-step **MapReduce** ব্যবহার করব:

* **Step 1** - ডেটা `(category, product_id), sum(quantity)` এ transform করুন
* **Step 2** - একটি distributed sort সম্পাদন করুন

```python
class SalesRanker(MRJob):

    def within_past_week(self, timestamp):
        """Return True if timestamp is within past week, False otherwise."""
        ...

    def mapper(self, _ line):
        """Parse each log line, extract and transform relevant lines.

        Emit key value pairs of the form:

        (category1, product1), 2
        (category2, product1), 2
        (category2, product1), 1
        (category1, product2), 3
        (category2, product3), 7
        (category1, product4), 1
        """
        timestamp, product_id, category_id, quantity, total_price, seller_id, \
            buyer_id = line.split('\t')
        if self.within_past_week(timestamp):
            yield (category_id, product_id), quantity

    def reducer(self, key, value):
        """Sum values for each key.

        (category1, product1), 2
        (category2, product1), 3
        (category1, product2), 3
        (category2, product3), 7
        (category1, product4), 1
        """
        yield key, sum(values)

    def mapper_sort(self, key, value):
        """Construct key to ensure proper sorting.

        Transform key and value to the form:

        (category1, 2), product1
        (category2, 3), product1
        (category1, 3), product2
        (category2, 7), product3
        (category1, 1), product4

        The shuffle/sort step of MapReduce will then do a
        distributed sort on the keys, resulting in:

        (category1, 1), product4
        (category1, 2), product1
        (category1, 3), product2
        (category2, 3), product1
        (category2, 7), product3
        """
        category_id, product_id = key
        quantity = value
        yield (category_id, quantity), product_id

    def reducer_identity(self, key, value):
        yield key, value

    def steps(self):
        """Run the map and reduce steps."""
        return [
            self.mr(mapper=self.mapper,
                    reducer=self.reducer),
            self.mr(mapper=self.mapper_sort,
                    reducer=self.reducer_identity),
        ]
```

Result হবে নিম্নলিখিত sorted list, যা আমরা `sales_rank` table এ insert করতে পারি:

```
(category1, 1), product4
(category1, 2), product1
(category1, 3), product2
(category2, 3), product1
(category2, 7), product3
```

`sales_rank` table নিম্নলিখিত structure থাকতে পারে:

```
id int NOT NULL AUTO_INCREMENT
category_id int NOT NULL
total_sold int NOT NULL
product_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(category_id) REFERENCES Categories(id)
FOREIGN KEY(product_id) REFERENCES Products(id)
```

আমরা lookups দ্রুত করতে (entire table scan করার পরিবর্তে log-time) এবং ডেটা memory এ রাখতে `id `, `category_id`, এবং `product_id` এ একটি [index](https://github.com/Shakil-khan59/system-design-primer-bangla#use-good-indices) তৈরি করব। Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=https://github.com/Shakil-khan59/system-design-primer-bangla#latency-numbers-every-programmer-should-know>1</a></sup>

### Use case: User views the past week's most popular products by category

* **Client** **Web Server** এ একটি request পাঠায়, একটি [reverse proxy](https://github.com/Shakil-khan59/system-design-primer-bangla#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Read API** server এ forward করে
* **Read API** server **SQL Database** `sales_rank` table থেকে পড়ে

আমরা একটি public [**REST API**](https://github.com/Shakil-khan59/system-design-primer-bangla#representational-state-transfer-rest) ব্যবহার করব:

```
$ curl https://amazon.com/api/v1/popular?category_id=1234
```

Response:

```
{
    "id": "100",
    "category_id": "1234",
    "total_sold": "100000",
    "product_id": "50",
},
{
    "id": "53",
    "category_id": "1234",
    "total_sold": "90000",
    "product_id": "200",
},
{
    "id": "75",
    "category_id": "1234",
    "total_sold": "80000",
    "product_id": "3",
},
```

Internal communications এর জন্য, আমরা [Remote Procedure Calls](https://github.com/Shakil-khan59/system-design-primer-bangla#remote-procedure-call-rpc) ব্যবহার করতে পারি।

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/MzExP06.png)

**গুরুত্বপূর্ণ: শুধু initial design থেকে final design এ সরাসরি jump করবেন না!**

State করুন আপনি 1) **Benchmark/Load Test**, 2) **Profile** bottlenecks এর জন্য 3) alternatives এবং trade-offs evaluate করার সময় bottlenecks address করবেন এবং 4) repeat করবেন। [Design a system that scales to millions of users on AWS](../scaling_aws/bangla.md) দেখুন initial design iteratively scale করার একটি sample হিসাবে।

Initial design এর সাথে আপনি যে bottlenecks এর মুখোমুখি হতে পারেন এবং আপনি কীভাবে প্রতিটি address করতে পারেন তা নিয়ে আলোচনা করা গুরুত্বপূর্ণ। উদাহরণস্বরূপ, একাধিক **Web Servers** সহ একটি **Load Balancer** যোগ করা দ্বারা কী issues address করা হয়? **CDN**? **Master-Slave Replicas**? প্রতিটির জন্য alternatives এবং **Trade-Offs** কী?

আমরা design complete করতে এবং scalability issues address করতে কিছু components পরিচয় করাব। Internal load balancers clutter কমাতে দেখানো হয়নি।

*আলোচনা repeat করা এড়াতে*, main talking points, tradeoffs, এবং alternatives এর জন্য নিম্নলিখিত [system design topics](https://github.com/Shakil-khan59/system-design-primer-bangla#index-of-system-design-topics) দেখুন:

* [DNS](https://github.com/Shakil-khan59/system-design-primer-bangla#domain-name-system)
* [CDN](https://github.com/Shakil-khan59/system-design-primer-bangla#content-delivery-network)
* [Load balancer](https://github.com/Shakil-khan59/system-design-primer-bangla#load-balancer)
* [Horizontal scaling](https://github.com/Shakil-khan59/system-design-primer-bangla#horizontal-scaling)
* [Web server (reverse proxy)](https://github.com/Shakil-khan59/system-design-primer-bangla#reverse-proxy-web-server)
* [API server (application layer)](https://github.com/Shakil-khan59/system-design-primer-bangla#application-layer)
* [Cache](https://github.com/Shakil-khan59/system-design-primer-bangla#cache)
* [Relational database management system (RDBMS)](https://github.com/Shakil-khan59/system-design-primer-bangla#relational-database-management-system-rdbms)
* [SQL write master-slave failover](https://github.com/Shakil-khan59/system-design-primer-bangla#fail-over)
* [Master-slave replication](https://github.com/Shakil-khan59/system-design-primer-bangla#master-slave-replication)
* [Consistency patterns](https://github.com/Shakil-khan59/system-design-primer-bangla#consistency-patterns)
* [Availability patterns](https://github.com/Shakil-khan59/system-design-primer-bangla#availability-patterns)

**Analytics Database** Amazon Redshift বা Google BigQuery এর মতো একটি data warehousing solution ব্যবহার করতে পারে।

আমরা সম্ভবত database এ শুধুমাত্র একটি limited time period ডেটা store করতে চাই, যখন বাকি একটি data warehouse বা একটি **Object Store** এ store করি। Amazon S3 এর মতো একটি **Object Store** প্রতি মাসে 40 GB নতুন content এর constraint comfortably handle করতে পারে।

40,000 *average* read requests per second (peak এ higher) address করতে, popular content (এবং তাদের sales rank) এর traffic **Memory Cache** দ্বারা handle করা উচিত database এর পরিবর্তে। **Memory Cache** unevenly distributed traffic এবং traffic spikes handle করতেও উপযোগী। Reads এর large volume সহ, **SQL Read Replicas** cache misses handle করতে সক্ষম নাও হতে পারে। আমাদের সম্ভবত additional SQL scaling patterns employ করতে হবে।

400 *average* writes per second (peak এ higher) একটি single **SQL Write Master-Slave** এর জন্য tough হতে পারে, additional scaling techniques এর প্রয়োজনও নির্দেশ করে।

SQL scaling patterns এর মধ্যে রয়েছে:

* [Federation](https://github.com/Shakil-khan59/system-design-primer-bangla#federation)
* [Sharding](https://github.com/Shakil-khan59/system-design-primer-bangla#sharding)
* [Denormalization](https://github.com/Shakil-khan59/system-design-primer-bangla#denormalization)
* [SQL Tuning](https://github.com/Shakil-khan59/system-design-primer-bangla#sql-tuning)

আমাদের কিছু ডেটা একটি **NoSQL Database** এ move করারও বিবেচনা করা উচিত।

## Additional talking points

> সমস্যা scope এবং remaining time এর উপর নির্ভর করে dive into করার জন্য additional topics।

#### NoSQL

* [Key-value store](https://github.com/Shakil-khan59/system-design-primer-bangla#key-value-store)
* [Document store](https://github.com/Shakil-khan59/system-design-primer-bangla#document-store)
* [Wide column store](https://github.com/Shakil-khan59/system-design-primer-bangla#wide-column-store)
* [Graph database](https://github.com/Shakil-khan59/system-design-primer-bangla#graph-database)
* [SQL vs NoSQL](https://github.com/Shakil-khan59/system-design-primer-bangla#sql-or-nosql)

### Caching

* কোথায় cache করতে হবে
    * [Client caching](https://github.com/Shakil-khan59/system-design-primer-bangla#client-caching)
    * [CDN caching](https://github.com/Shakil-khan59/system-design-primer-bangla#cdn-caching)
    * [Web server caching](https://github.com/Shakil-khan59/system-design-primer-bangla#web-server-caching)
    * [Database caching](https://github.com/Shakil-khan59/system-design-primer-bangla#database-caching)
    * [Application caching](https://github.com/Shakil-khan59/system-design-primer-bangla#application-caching)
* কী cache করতে হবে
    * [Caching at the database query level](https://github.com/Shakil-khan59/system-design-primer-bangla#caching-at-the-database-query-level)
    * [Caching at the object level](https://github.com/Shakil-khan59/system-design-primer-bangla#caching-at-the-object-level)
* কখন cache update করতে হবে
    * [Cache-aside](https://github.com/Shakil-khan59/system-design-primer-bangla#cache-aside)
    * [Write-through](https://github.com/Shakil-khan59/system-design-primer-bangla#write-through)
    * [Write-behind (write-back)](https://github.com/Shakil-khan59/system-design-primer-bangla#write-behind-write-back)
    * [Refresh ahead](https://github.com/Shakil-khan59/system-design-primer-bangla#refresh-ahead)

### Asynchronism and microservices

* [Message queues](https://github.com/Shakil-khan59/system-design-primer-bangla#message-queues)
* [Task queues](https://github.com/Shakil-khan59/system-design-primer-bangla#task-queues)
* [Back pressure](https://github.com/Shakil-khan59/system-design-primer-bangla#back-pressure)
* [Microservices](https://github.com/Shakil-khan59/system-design-primer-bangla#microservices)

### Communications

* Tradeoffs নিয়ে আলোচনা করুন:
    * Clients এর সাথে external communication - [HTTP APIs following REST](https://github.com/Shakil-khan59/system-design-primer-bangla#representational-state-transfer-rest)
    * Internal communications - [RPC](https://github.com/Shakil-khan59/system-design-primer-bangla#remote-procedure-call-rpc)
* [Service discovery](https://github.com/Shakil-khan59/system-design-primer-bangla#service-discovery)

### Security

[security section](https://github.com/Shakil-khan59/system-design-primer-bangla#security) দেখুন।

### Latency numbers

[Latency numbers every programmer should know](https://github.com/Shakil-khan59/system-design-primer-bangla#latency-numbers-every-programmer-should-know) দেখুন।

### Ongoing

* Bottlenecks আসার সাথে সাথে address করতে আপনার system benchmark এবং monitor করা চালিয়ে যান
* Scaling একটি iterative process
