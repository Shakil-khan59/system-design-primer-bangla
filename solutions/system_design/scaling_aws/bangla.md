# Design a system that scales to millions of users on AWS

*নোট: এই document [system design topics](../../bangla.md#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

এই সমস্যা সমাধান করা একটি iterative approach নেয়: 1) **Benchmark/Load Test**, 2) **Profile** bottlenecks এর জন্য 3) alternatives এবং trade-offs evaluate করার সময় bottlenecks address করুন, এবং 4) repeat করুন, যা basic designs থেকে scalable designs এ evolving করার জন্য একটি ভাল pattern।

যদি না আপনার AWS এ background থাকে বা আপনি এমন একটি position এর জন্য apply করছেন যার AWS knowledge প্রয়োজন, AWS-specific details একটি requirement নয়। তবে, **এই exercise এ আলোচিত principles এর অনেক কিছু AWS ecosystem এর বাইরেও আরও সাধারণভাবে apply করতে পারে।**

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **User** একটি read বা write request করে
    * **Service** processing করে, user data store করে, তারপর results return করে
* **Service** একটি small amount of users serve করা থেকে millions of users এ evolve করতে হবে
    * একটি large number of users এবং requests handle করার জন্য একটি architecture evolve করার সাথে general scaling patterns নিয়ে আলোচনা করুন
* **Service** উচ্চ প্রাপ্যতা রয়েছে

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
* Relational data এর প্রয়োজন
* 1 user থেকে tens of millions of users এ scale করুন
    * Users এর increase নির্দেশ করুন:
        * Users+
        * Users++
        * Users+++
        * ...
    * 10 million users
    * প্রতি মাসে 1 billion writes
    * প্রতি মাসে 100 billion reads
    * 100:1 read to write ratio
    * Write প্রতি 1 KB content

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* প্রতি মাসে 1 TB নতুন content
    * Write প্রতি 1 KB * প্রতি মাসে 1 billion writes
    * 3 বছরে 36 TB নতুন content
    * ধরে নিন বেশিরভাগ writes existing ones এ updates এর পরিবর্তে নতুন content থেকে
* গড়ে প্রতি সেকেন্ডে 400 writes
* গড়ে প্রতি সেকেন্ডে 40,000 reads

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/B8LDKD7.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: User makes a read or write request

#### Goals

* শুধুমাত্র 1-2 users সহ, আপনার শুধুমাত্র একটি basic setup প্রয়োজন
    * Simplicity এর জন্য single box
    * প্রয়োজন হলে Vertical scaling
    * Bottlenecks নির্ধারণ করতে monitor করুন

#### Start with a single box

* EC2 এ **Web server**
    * User data এর জন্য Storage
    * [**MySQL Database**](../../bangla.md#relational-database-management-system-rdbms)

**Vertical Scaling** ব্যবহার করুন:

* কেবল একটি bigger box বেছে নিন
* কীভাবে scale up করতে হবে তা নির্ধারণ করতে metrics এর উপর নজর রাখুন
    * Bottlenecks নির্ধারণ করতে basic monitoring ব্যবহার করুন: CPU, memory, IO, network, ইত্যাদি
    * CloudWatch, top, nagios, statsd, graphite, ইত্যাদি
* Scaling vertically খুব expensive হতে পারে
* কোনো redundancy/failover নেই

*Trade-offs, alternatives, এবং additional details:*

* **Vertical Scaling** এর alternative হল [**Horizontal scaling**](../../bangla.md#horizontal-scaling)

#### Start with SQL, consider NoSQL

Constraints ধরে নেয় relational data এর প্রয়োজন রয়েছে। আমরা single box এ একটি **MySQL Database** ব্যবহার করে শুরু করতে পারি।

*Trade-offs, alternatives, এবং additional details:*

* [Relational database management system (RDBMS)](../../bangla.md#relational-database-management-system-rdbms) section দেখুন
* [SQL বা NoSQL](../../bangla.md#sql-or-nosql) ব্যবহার করার কারণ নিয়ে আলোচনা করুন

#### Assign a public static IP

* Elastic IPs একটি public endpoint প্রদান করে যার IP reboot এ পরিবর্তিত হয় না
* Failover এ সাহায্য করে, কেবল domain একটি নতুন IP নির্দেশ করুন

#### Use a DNS

Domain instance এর public IP এ map করতে Route 53 এর মতো একটি **DNS** যোগ করুন।

*Trade-offs, alternatives, এবং additional details:*

* [Domain name system](../../bangla.md#domain-name-system) section দেখুন

#### Secure the web server

* শুধুমাত্র necessary ports খুলুন
    * Web server কে incoming requests থেকে respond করতে অনুমোদন করুন:
        * HTTP এর জন্য 80
        * HTTPS এর জন্য 443
        * শুধুমাত্র whitelisted IPs এর জন্য SSH এর জন্য 22
    * Web server কে outbound connections initiate করা থেকে প্রতিরোধ করুন

*Trade-offs, alternatives, এবং additional details:*

* [Security](../../bangla.md#security) section দেখুন

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

### Users+

![Imgur](http://i.imgur.com/rrfjMXB.png)

#### Assumptions

আমাদের user count pick up করা শুরু করছে এবং আমাদের single box এ load বৃদ্ধি পাচ্ছে। আমাদের **Benchmarks/Load Tests** এবং **Profiling** নির্দেশ করছে **MySQL Database** আরও বেশি memory এবং CPU resources নিচ্ছে, যখন user content disk space fill up করছে।

আমরা এখন পর্যন্ত **Vertical Scaling** দিয়ে এই issues address করতে সক্ষম হয়েছি। দুর্ভাগ্যবশত, এটি quite expensive হয়ে উঠেছে এবং এটি **MySQL Database** এবং **Web Server** এর independent scaling অনুমোদন করে না।

#### Goals

* Single box এ load lighten করুন এবং independent scaling অনুমোদন করুন
    * Static content separately একটি **Object Store** এ store করুন
    * **MySQL Database** একটি separate box এ move করুন
* Disadvantages
    * এই changes complexity বৃদ্ধি করবে এবং **Object Store** এবং **MySQL Database** নির্দেশ করতে **Web Server** এ changes প্রয়োজন হবে
    * নতুন components secure করতে additional security measures নেওয়া আবশ্যক
    * AWS costs ও বৃদ্ধি পেতে পারে, তবে আপনার নিজের উপর similar systems manage করার costs এর সাথে weighed করা উচিত

#### Store static content separately

* Static content store করতে S3 এর মতো একটি managed **Object Store** ব্যবহার করার বিবেচনা করুন
    * Highly scalable এবং reliable
    * Server side encryption
* Static content S3 এ move করুন
    * User files
    * JS
    * CSS
    * Images
    * Videos

#### Move the MySQL database to a separate box

* **MySQL Database** manage করতে RDS এর মতো একটি service ব্যবহার করার বিবেচনা করুন
    * Administer, scale করা simple
    * Multiple availability zones
    * Encryption at rest

#### Secure the system

* Transit এবং rest এ ডেটা encrypt করুন
* একটি Virtual Private Cloud ব্যবহার করুন
    * Single **Web Server** এর জন্য একটি public subnet তৈরি করুন যাতে এটি internet থেকে traffic send এবং receive করতে পারে
    * অন্য সবকিছুর জন্য একটি private subnet তৈরি করুন, outside access প্রতিরোধ করে
    * প্রতিটি component এর জন্য শুধুমাত্র whitelisted IPs থেকে ports খুলুন
* Exercise এর remainder এ নতুন components এর জন্য এই same patterns implement করা উচিত

*Trade-offs, alternatives, এবং additional details:*

* [Security](../../bangla.md#security) section দেখুন

### Users++

![Imgur](http://i.imgur.com/raoFTXM.png)

#### Assumptions

আমাদের **Benchmarks/Load Tests** এবং **Profiling** দেখায় যে আমাদের single **Web Server** peak hours এ bottlenecks, slow responses এবং কিছু ক্ষেত্রে, downtime এর ফলাফল দেয়। Service mature হওয়ার সাথে, আমরা higher availability এবং redundancy এর দিকেও move করতে চাই।

#### Goals

* নিম্নলিখিত goals **Web Server** এর সাথে scaling issues address করার চেষ্টা করে
    * **Benchmarks/Load Tests** এবং **Profiling** এর উপর ভিত্তি করে, আপনার শুধুমাত্র এই techniques এর একটি বা দুটি implement করার প্রয়োজন হতে পারে
* Increasing loads handle করতে এবং single points of failure address করতে [**Horizontal Scaling**](../../bangla.md#horizontal-scaling) ব্যবহার করুন
    * Amazon এর ELB বা HAProxy এর মতো একটি [**Load Balancer**](../../bangla.md#load-balancer) যোগ করুন
        * ELB highly available
        * যদি আপনি আপনার নিজের **Load Balancer** configure করছেন, multiple availability zones এ [active-active](../../bangla.md#active-active) বা [active-passive](../../bangla.md#active-passive) এ multiple servers set up করা availability উন্নত করবে
        * Backend servers এ computational load কমাতে এবং certificate administration simplify করতে **Load Balancer** এ SSL terminate করুন
    * Multiple availability zones জুড়ে spread করা multiple **Web Servers** ব্যবহার করুন
    * Redundancy উন্নত করতে multiple availability zones জুড়ে [**Master-Slave Failover**](../../bangla.md#master-slave-replication) mode এ multiple **MySQL** instances ব্যবহার করুন
* [**Application Servers**](../../bangla.md#application-layer) থেকে **Web Servers** আলাদা করুন
    * উভয় layer independently scale এবং configure করুন
    * **Web Servers** একটি [**Reverse Proxy**](../../bangla.md#reverse-proxy-web-server) হিসাবে run করতে পারে
    * উদাহরণস্বরূপ, আপনি **Read APIs** handle করে এমন **Application Servers** যোগ করতে পারেন যখন অন্যরা **Write APIs** handle করে
* Load এবং latency কমাতে CloudFront এর মতো একটি [**Content Delivery Network (CDN)**](../../bangla.md#content-delivery-network) এ static (এবং কিছু dynamic) content move করুন

*Trade-offs, alternatives, এবং additional details:*

* Details এর জন্য উপরের linked content দেখুন

### Users+++

![Imgur](http://i.imgur.com/OZCxJr0.png)

**Note:** **Internal Load Balancers** clutter কমাতে দেখানো হয়নি

#### Assumptions

আমাদের **Benchmarks/Load Tests** এবং **Profiling** দেখায় যে আমরা read-heavy (writes এর সাথে 100:1) এবং আমাদের database high read requests থেকে poor performance ভোগ করছে।

#### Goals

* নিম্নলিখিত goals **MySQL Database** এর সাথে scaling issues address করার চেষ্টা করে
    * **Benchmarks/Load Tests** এবং **Profiling** এর উপর ভিত্তি করে, আপনার শুধুমাত্র এই techniques এর একটি বা দুটি implement করার প্রয়োজন হতে পারে
* Load এবং latency কমাতে Elasticache এর মতো একটি [**Memory Cache**](../../bangla.md#cache) এ নিম্নলিখিত ডেটা move করুন:
    * **MySQL** থেকে frequently accessed content
        * প্রথমে, একটি **Memory Cache** implement করার আগে **MySQL Database** cache configure করার চেষ্টা করুন যদি bottleneck relieve করার জন্য এটি sufficient হয়
    * **Web Servers** থেকে Session data
        * **Web Servers** stateless হয়ে যায়, **Autoscaling** অনুমোদন করে
    * Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=../../bangla.md#latency-numbers-every-programmer-should-know>1</a></sup>
* Write master এ load কমাতে [**MySQL Read Replicas**](../../bangla.md#master-slave-replication) যোগ করুন
* Responsiveness উন্নত করতে আরও **Web Servers** এবং **Application Servers** যোগ করুন

*Trade-offs, alternatives, এবং additional details:*

* Details এর জন্য উপরের linked content দেখুন

#### Add MySQL read replicas

* একটি **Memory Cache** যোগ করা এবং scaling ছাড়াও, **MySQL Read Replicas** **MySQL Write Master** এ load relieve করতে সাহায্য করতে পারে
* Writes এবং reads আলাদা করতে **Web Server** এ logic যোগ করুন
* **MySQL Read Replicas** এর সামনে **Load Balancers** যোগ করুন (clutter কমাতে pictured নয়)
* বেশিরভাগ services write-heavy এর তুলনায় read-heavy

*Trade-offs, alternatives, এবং additional details:*

* [Relational database management system (RDBMS)](../../bangla.md#relational-database-management-system-rdbms) section দেখুন

### Users++++

![Imgur](http://i.imgur.com/3X8nmdL.png)

#### Assumptions

আমাদের **Benchmarks/Load Tests** এবং **Profiling** দেখায় যে আমাদের traffic U.S. এ regular business hours এ spikes এবং users office ছেড়ে যাওয়ার সময় significantly drop করে। আমরা actual load এর উপর ভিত্তি করে automatically servers spin up এবং down করে costs cut করতে পারি বলে মনে করি। আমরা একটি small shop তাই **Autoscaling** এবং general operations এর জন্য DevOps এর যতটা সম্ভব automate করতে চাই।

#### Goals

* প্রয়োজন অনুযায়ী capacity provision করতে **Autoscaling** যোগ করুন
    * Traffic spikes এর সাথে keep up করুন
    * Unused instances power down করে costs কমাতে
* DevOps automate করুন
    * Chef, Puppet, Ansible, ইত্যাদি
* Bottlenecks address করতে metrics monitor করা চালিয়ে যান
    * **Host level** - একটি single EC2 instance পর্যালোচনা করুন
    * **Aggregate level** - Load balancer stats পর্যালোচনা করুন
    * **Log analysis** - CloudWatch, CloudTrail, Loggly, Splunk, Sumo
    * **External site performance** - Pingdom বা New Relic
    * **Handle notifications and incidents** - PagerDuty
    * **Error Reporting** - Sentry

#### Add autoscaling

* AWS **Autoscaling** এর মতো একটি managed service বিবেচনা করুন
    * প্রতিটি **Web Server** এবং প্রতিটি **Application Server** type এর জন্য একটি group তৈরি করুন, প্রতিটি group multiple availability zones এ রাখুন
    * Instances এর একটি min এবং max number set করুন
    * CloudWatch এর মাধ্যমে scale up এবং down trigger করুন
        * Predictable loads এর জন্য simple time of day metric বা
        * একটি time period এর উপর metrics:
            * CPU load
            * Latency
            * Network traffic
            * Custom metric
    * Disadvantages
        * Autoscaling complexity প্রবর্তন করতে পারে
        * Increased demand meet করতে system appropriately scale up হতে কিছু সময় লাগতে পারে, বা demand drop হলে scale down হতে পারে

### Users+++++

![Imgur](http://i.imgur.com/jj3A5N8.png)

**Note:** **Autoscaling** groups clutter কমাতে দেখানো হয়নি

#### Assumptions

Service constraints এ outlined figures এর দিকে continue growing হওয়ার সাথে, আমরা iteratively **Benchmarks/Load Tests** এবং **Profiling** run করি new bottlenecks uncover এবং address করতে।

#### Goals

আমরা problem এর constraints এর কারণে scaling issues address করা চালিয়ে যাব:

* যদি আমাদের **MySQL Database** খুব বড় হতে শুরু করে, আমরা সম্ভবত database এ শুধুমাত্র একটি limited time period ডেটা store করার বিবেচনা করতে পারি, যখন বাকি Redshift এর মতো একটি data warehouse এ store করি
    * Redshift এর মতো একটি data warehouse প্রতি মাসে 1 TB নতুন content এর constraint comfortably handle করতে পারে
* প্রতি সেকেন্ডে 40,000 average read requests সহ, popular content এর read traffic **Memory Cache** scaling করে address করা যেতে পারে, যা unevenly distributed traffic এবং traffic spikes handle করতেও উপযোগী
    * **SQL Read Replicas** cache misses handle করতে trouble হতে পারে, আমাদের সম্ভবত additional SQL scaling patterns employ করতে হবে
* প্রতি সেকেন্ডে 400 average writes (presumably significantly higher peaks সহ) একটি single **SQL Write Master-Slave** এর জন্য tough হতে পারে, additional scaling techniques এর প্রয়োজনও নির্দেশ করে

SQL scaling patterns এর মধ্যে রয়েছে:

* [Federation](../../bangla.md#federation)
* [Sharding](../../bangla.md#sharding)
* [Denormalization](../../bangla.md#denormalization)
* [SQL Tuning](../../bangla.md#sql-tuning)

High read এবং write requests আরও address করতে, আমাদের appropriate ডেটা DynamoDB এর মতো একটি [**NoSQL Database**](../../bangla.md#nosql) এ move করারও বিবেচনা করা উচিত।

আমরা independent scaling অনুমোদন করতে আমাদের [**Application Servers**](../../bangla.md#application-layer) আরও আলাদা করতে পারি। Batch processes বা computations যা real-time এ করা প্রয়োজন নেই তা **Queues** এবং **Workers** সহ [**Asynchronously**](../../bangla.md#asynchronism) করা যেতে পারে:

* উদাহরণস্বরূপ, একটি photo service এ, photo upload এবং thumbnail creation আলাদা করা যেতে পারে:
    * **Client** photo upload করে
    * **Application Server** SQS এর মতো একটি **Queue** এ একটি job রাখে
    * EC2 বা Lambda এ **Worker Service** **Queue** থেকে work pull করে তারপর:
        * একটি thumbnail তৈরি করে
        * একটি **Database** আপডেট করে
        * **Object Store** এ thumbnail store করে

*Trade-offs, alternatives, এবং additional details:*

* Details এর জন্য উপরের linked content দেখুন

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

