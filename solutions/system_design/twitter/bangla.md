# Design the Twitter timeline and search

*নোট: এই document [system design topics](../../bangla.md#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

**Design the Facebook feed** এবং **Design Facebook search** অনুরূপ প্রশ্ন।

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **User** একটি tweet posts করে
    * **Service** followers এ tweets push করে, push notifications এবং emails পাঠায়
* **User** user timeline views করে (user থেকে activity)
* **User** home timeline views করে (user যাদের follow করছে তাদের থেকে activity)
* **User** keywords search করে
* **Service** উচ্চ প্রাপ্যতা রয়েছে

#### Out of scope

* **Service** Twitter Firehose এবং other streams এ tweets push করে
* **Service** users' visibility settings এর উপর ভিত্তি করে tweets strip out করে
    * User যাকে replied করা হচ্ছে তাকেও follow না করলে @reply hide করুন
    * 'hide retweets' setting respect করুন
* Analytics

### Constraints and assumptions

#### State assumptions

General

* Traffic evenly distributed নয়
* একটি tweet posting করা দ্রুত হওয়া উচিত
    * আপনার সমস্ত followers এ একটি tweet fanning out করা দ্রুত হওয়া উচিত, যদি না আপনার millions of followers থাকে
* 100 million active users
* প্রতি দিনে 500 million tweets বা প্রতি মাসে 15 billion tweets
    * প্রতিটি tweet গড়ে 10 deliveries এর fanout
    * Fanout এ প্রতি দিনে 5 billion total tweets delivered
    * Fanout এ প্রতি মাসে 150 billion tweets delivered
* প্রতি মাসে 250 billion read requests
* প্রতি মাসে 10 billion searches

Timeline

* Timeline viewing করা দ্রুত হওয়া উচিত
* Twitter write heavy এর চেয়ে read heavy
    * Tweets এর fast reads এর জন্য optimize করুন
* Tweets ingesting করা write heavy

Search

* Searching করা দ্রুত হওয়া উচিত
* Search read-heavy

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* প্রতি tweet size:
    * `tweet_id` - 8 bytes
    * `user_id` - 32 bytes
    * `text` - 140 bytes
    * `media` - 10 KB average
    * Total: ~10 KB
* প্রতি মাসে 150 TB নতুন tweet content
    * প্রতি tweet 10 KB * প্রতি দিনে 500 million tweets * প্রতি মাসে 30 days
    * 3 বছরে 5.4 PB নতুন tweet content
* প্রতি সেকেন্ডে 100 thousand read requests
    * প্রতি মাসে 250 billion read requests * (প্রতি সেকেন্ডে 400 requests / প্রতি মাসে 1 billion requests)
* প্রতি সেকেন্ডে 6,000 tweets
    * প্রতি মাসে 15 billion tweets * (প্রতি সেকেন্ডে 400 requests / প্রতি মাসে 1 billion requests)
* Fanout এ প্রতি সেকেন্ডে 60 thousand tweets delivered
    * Fanout এ প্রতি মাসে 150 billion tweets delivered * (প্রতি সেকেন্ডে 400 requests / প্রতি মাসে 1 billion requests)
* প্রতি সেকেন্ডে 4,000 search requests
    * প্রতি মাসে 10 billion searches * (প্রতি সেকেন্ডে 400 requests / প্রতি মাসে 1 billion requests)

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/48tEA2j.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: User posts a tweet

আমরা user এর নিজের tweets user timeline (user থেকে activity) populate করতে একটি [relational database](../../bangla.md#relational-database-management-system-rdbms) এ store করতে পারি। আমাদের [SQL বা NoSQL বেছে নেওয়ার use cases এবং tradeoffs](../../bangla.md#sql-or-nosql) নিয়ে আলোচনা করা উচিত।

Tweets deliver করা এবং home timeline (user যাদের follow করছে তাদের থেকে activity) build করা trickier। সমস্ত followers এ tweets fanning out করা (fanout এ প্রতি সেকেন্ডে 60 thousand tweets delivered) একটি traditional [relational database](../../bangla.md#relational-database-management-system-rdbms) overload করবে। আমরা সম্ভবত fast writes সহ একটি data store বেছে নিতে চাই যেমন একটি **NoSQL database** বা **Memory Cache**। Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=../../bangla.md#latency-numbers-every-programmer-should-know>1</a></sup>

আমরা photos বা videos এর মতো media একটি **Object Store** এ store করতে পারি।

* **Client** **Web Server** এ একটি tweet posts করে, একটি [reverse proxy](../../bangla.md#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Write API** server এ forward করে
* **Write API** একটি **SQL database** এ user এর timeline এ tweet store করে
* **Write API** **Fan Out Service** এর সাথে যোগাযোগ করে, যা নিম্নলিখিত কাজগুলো করে:
    * **User Graph Service** query করে **Memory Cache** এ stored user এর followers খুঁজে পেতে
    * একটি **Memory Cache** এ *user এর followers এর home timeline* এ tweet store করে
        * O(n) operation: 1,000 followers = 1,000 lookups এবং inserts
    * Fast searching enable করতে **Search Index Service** এ tweet store করে
    * **Object Store** এ media store করে
    * Followers এ push notifications পাঠাতে **Notification Service** ব্যবহার করে:
        * Notifications asynchronously পাঠাতে একটি **Queue** (pictured নয়) ব্যবহার করে

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

যদি আমাদের **Memory Cache** Redis হয়, আমরা নিম্নলিখিত structure সহ একটি native Redis list ব্যবহার করতে পারি:

```
           tweet n+2                   tweet n+1                   tweet n
|| 8 bytes   8 bytes  1 byte | 8 bytes   8 bytes  1 byte | 8 bytes   8 bytes  1 byte |
|| tweet_id  user_id  meta   | tweet_id  user_id  meta   | tweet_id  user_id  meta   |
```

নতুন tweet **Memory Cache** এ স্থাপন করা হবে, যা user এর home timeline (user যাদের follow করছে তাদের থেকে activity) populate করে।

আমরা একটি public [**REST API**](../../bangla.md#representational-state-transfer-rest) ব্যবহার করব:

```
$ curl -X POST --data '{ "user_id": "123", "auth_token": "ABC123", \
    "status": "hello world!", "media_ids": "ABC987" }' \
    https://twitter.com/api/v1/tweet
```

Response:

```
{
    "created_at": "Wed Sep 05 00:37:15 +0000 2012",
    "status": "hello world!",
    "tweet_id": "987",
    "user_id": "123",
    ...
}
```

Internal communications এর জন্য, আমরা [Remote Procedure Calls](../../bangla.md#remote-procedure-call-rpc) ব্যবহার করতে পারি।

### Use case: User views the home timeline

* **Client** **Web Server** এ একটি home timeline request posts করে
* **Web Server** request **Read API** server এ forward করে
* **Read API** server **Timeline Service** এর সাথে যোগাযোগ করে, যা নিম্নলিখিত কাজগুলো করে:
    * **Memory Cache** এ stored timeline data পায়, tweet ids এবং user ids ধারণ করে - O(1)
    * Tweet ids সম্পর্কে additional info পেতে [multiget](http://redis.io/commands/mget) সহ **Tweet Info Service** query করে - O(n)
    * User ids সম্পর্কে additional info পেতে multiget সহ **User Info Service** query করে - O(n)

REST API:

```
$ curl https://twitter.com/api/v1/home_timeline?user_id=123
```

Response:

```
{
    "user_id": "456",
    "tweet_id": "123",
    "status": "foo"
},
{
    "user_id": "789",
    "tweet_id": "456",
    "status": "bar"
},
{
    "user_id": "789",
    "tweet_id": "579",
    "status": "baz"
},
```

### Use case: User views the user timeline

* **Client** **Web Server** এ একটি user timeline request posts করে
* **Web Server** request **Read API** server এ forward করে
* **Read API** **SQL Database** থেকে user timeline retrieve করে

REST API home timeline এর অনুরূপ হবে, তবে সমস্ত tweets user থেকে আসবে user যাদের follow করছে তাদের থেকে নয়।

### Use case: User searches keywords

* **Client** **Web Server** এ একটি search request পাঠায়
* **Web Server** request **Search API** server এ forward করে
* **Search API** **Search Service** এর সাথে যোগাযোগ করে, যা নিম্নলিখিত কাজগুলো করে:
    * Input query parse/tokenize করে, কী search করতে হবে তা নির্ধারণ করে
        * Markup remove করে
        * Text terms এ ভেঙে দেয়
        * Typos fix করে
        * Capitalization normalize করে
        * Query boolean operations ব্যবহার করতে convert করে
    * Results এর জন্য **Search Cluster** (অর্থাৎ [Lucene](https://lucene.apache.org/)) query করে:
        * Query এর জন্য cluster এ প্রতিটি server এ [scatter gathers](../../bangla.md#under-development) কোনো results আছে কিনা তা নির্ধারণ করতে
        * Results merge, rank, sort, এবং return করে

REST API:

```
$ curl https://twitter.com/api/v1/search?query=hello+world
```

Response home timeline এর অনুরূপ হবে, তবে প্রদত্ত query match করে এমন tweets এর জন্য।

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/jrUBAF7.png)

**গুরুত্বপূর্ণ: শুধু initial design থেকে final design এ সরাসরি jump করবেন না!**

State করুন আপনি 1) **Benchmark/Load Test**, 2) **Profile** bottlenecks এর জন্য 3) alternatives এবং trade-offs evaluate করার সময় bottlenecks address করবেন এবং 4) repeat করবেন। [Design a system that scales to millions of users on AWS](../scaling_aws/bangla.md) দেখুন initial design iteratively scale করার একটি sample হিসাবে।

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

**Fanout Service** একটি potential bottleneck। Millions of followers সহ Twitter users তাদের tweets fanout process এর মধ্য দিয়ে যেতে কয়েক মিনিট সময় নিতে পারে। এটি tweet এ @replies এর সাথে race conditions এর দিকে নিয়ে যেতে পারে, যা আমরা serve time এ tweets re-ordering করে mitigate করতে পারি।

আমরা highly-followed users থেকে tweets fanning out এড়াতে পারি। পরিবর্তে, আমরা highly-followed users এর জন্য tweets খুঁজে পেতে search করতে পারি, search results user এর home timeline results এর সাথে merge করতে পারি, তারপর serve time এ tweets re-order করতে পারি।

Additional optimizations এর মধ্যে রয়েছে:

* **Memory Cache** এ প্রতিটি home timeline এর জন্য শুধুমাত্র several hundred tweets রাখুন
* **Memory Cache** এ শুধুমাত্র active users' home timeline info রাখুন
    * যদি একটি user past 30 days এ previously active না হয়, আমরা **SQL Database** থেকে timeline rebuild করতে পারি
        * User কে follow করছে তা নির্ধারণ করতে **User Graph Service** query করুন
        * **SQL Database** থেকে tweets get করুন এবং **Memory Cache** এ যোগ করুন
* **Tweet Info Service** এ শুধুমাত্র একটি মাসের tweets store করুন
* **User Info Service** এ শুধুমাত্র active users store করুন
* **Search Cluster** সম্ভবত latency কম রাখতে memory এ tweets রাখতে হবে

আমরা **SQL Database** এর সাথে bottleneck address করব।

যদিও **Memory Cache** database এর load কমাতে হবে, এটি unlikely **SQL Read Replicas** alone cache misses handle করার জন্য যথেষ্ট হবে। আমাদের সম্ভবত additional SQL scaling patterns employ করতে হবে।

Writes এর high volume একটি single **SQL Write Master-Slave** overwhelm করবে, additional scaling techniques এর প্রয়োজনও নির্দেশ করে।

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

