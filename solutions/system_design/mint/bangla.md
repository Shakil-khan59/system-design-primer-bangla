# Design Mint.com

*নোট: এই document [system design topics](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) এ পাওয়া relevant areas এর সাথে সরাসরি link করে duplication এড়াতে। সাধারণ talking points, tradeoffs, এবং alternatives এর জন্য linked content দেখুন।*

## Step 1: Outline use cases and constraints

> আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি scope করুন।
> Use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন।
> Assumptions নিয়ে আলোচনা করুন।

Clarifying questions address করার জন্য interviewer ছাড়া, আমরা কিছু use cases এবং constraints সংজ্ঞায়িত করব।

### Use cases

#### আমরা শুধুমাত্র নিম্নলিখিত use cases handle করার জন্য সমস্যাটি scope করব

* **User** একটি financial account এর সাথে connect করে
* **Service** account থেকে transactions extract করে
    * Daily updates করে
    * Transactions categorize করে
        * User দ্বারা manual category override অনুমোদন করে
        * কোনো automatic re-categorization নেই
    * Category দ্বারা monthly spending analyze করে
* **Service** একটি budget recommends করে
    * Users manually একটি budget set করতে অনুমোদন করে
    * Budget এর কাছে আসার বা exceed করার সময় notifications পাঠায়
* **Service** উচ্চ প্রাপ্যতা রয়েছে

#### Out of scope

* **Service** additional logging এবং analytics সম্পাদন করে

### Constraints and assumptions

#### State assumptions

* Traffic evenly distributed নয়
* Accounts এর automatic daily update শুধুমাত্র past 30 days এ active users এর জন্য প্রযোজ্য
* Financial accounts যোগ করা বা remove করা relatively rare
* Budget notifications instant হওয়ার প্রয়োজন নেই
* 10 million users
    * User প্রতি 10 budget categories = 100 million budget items
    * উদাহরণ categories:
        * Housing = $1,000
        * Food = $200
        * Gas = $100
    * Sellers transaction category নির্ধারণ করতে ব্যবহৃত হয়
        * 50,000 sellers
* 30 million financial accounts
* প্রতি মাসে 5 billion transactions
* প্রতি মাসে 500 million read requests
* 10:1 write to read ratio
    * Write-heavy, users daily transactions করে, কিন্তু few daily site visit করে

#### Calculate usage

**আপনার interviewer এর সাথে clarify করুন যদি আপনার back-of-the-envelope usage calculations run করা উচিত।**

* প্রতি transaction size:
    * `user_id` - 8 bytes
    * `created_at` - 5 bytes
    * `seller` - 32 bytes
    * `amount` - 5 bytes
    * Total: ~50 bytes
* প্রতি মাসে 250 GB নতুন transaction content
    * প্রতি transaction 50 bytes * প্রতি মাসে 5 billion transactions
    * 3 বছরে 9 TB নতুন transaction content
    * ধরে নিন বেশিরভাগ existing ones এ updates এর পরিবর্তে নতুন transactions
* গড়ে প্রতি সেকেন্ডে 2,000 transactions
* গড়ে প্রতি সেকেন্ডে 200 read requests

Handy conversion guide:

* প্রতি মাসে 2.5 million seconds
* প্রতি সেকেন্ডে 1 request = প্রতি মাসে 2.5 million requests
* প্রতি সেকেন্ডে 40 requests = প্রতি মাসে 100 million requests
* প্রতি সেকেন্ডে 400 requests = প্রতি মাসে 1 billion requests

## Step 2: Create a high level design

> সব গুরুত্বপূর্ণ components সহ একটি high level design রূপরেখা তৈরি করুন।

![Imgur](http://i.imgur.com/E8klrBh.png)

## Step 3: Design core components

> প্রতিটি core component এর জন্য বিস্তারিতভাবে যান।

### Use case: User connects to a financial account

আমরা 10 million users এর info একটি [relational database](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms) এ store করতে পারি। আমাদের [SQL বা NoSQL বেছে নেওয়ার use cases এবং tradeoffs](https://github.com/donnemartin/system-design-primer#sql-or-nosql) নিয়ে আলোচনা করা উচিত।

* **Client** **Web Server** এ একটি request পাঠায়, একটি [reverse proxy](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server) হিসাবে চলছে
* **Web Server** request **Accounts API** server এ forward করে
* **Accounts API** server newly entered account info সহ **SQL Database** `accounts` table আপডেট করে

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

`accounts` table নিম্নলিখিত structure থাকতে পারে:

```
id int NOT NULL AUTO_INCREMENT
created_at datetime NOT NULL
last_update datetime NOT NULL
account_url varchar(255) NOT NULL
account_login varchar(32) NOT NULL
account_password_hash char(64) NOT NULL
user_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

আমরা lookups দ্রুত করতে (entire table scan করার পরিবর্তে log-time) এবং ডেটা memory এ রাখতে `id`, `user_id `, এবং `created_at` এ একটি [index](https://github.com/donnemartin/system-design-primer#use-good-indices) তৈরি করব। Memory থেকে sequentially 1 MB পড়তে প্রায় 250 microseconds লাগে, যখন SSD থেকে পড়তে 4x এবং disk থেকে পড়তে 80x বেশি সময় লাগে।<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

আমরা একটি public [**REST API**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) ব্যবহার করব:

```
$ curl -X POST --data '{ "user_id": "foo", "account_url": "bar", \
    "account_login": "baz", "account_password": "qux" }' \
    https://mint.com/api/v1/account
```

Internal communications এর জন্য, আমরা [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc) ব্যবহার করতে পারি।

এরপর, service account থেকে transactions extract করে।

### Use case: Service extracts transactions from the account

আমরা এই cases এ একটি account থেকে information extract করতে চাই:

* User প্রথমবার account link করে
* User manually account refresh করে
* Past 30 days এ active users এর জন্য automatically প্রতিদিন

Data flow:

* **Client** **Web Server** এ একটি request পাঠায়
* **Web Server** request **Accounts API** server এ forward করে
* **Accounts API** server [Amazon SQS](https://aws.amazon.com/sqs/) বা [RabbitMQ](https://www.rabbitmq.com/) এর মতো একটি **Queue** এ একটি job রাখে
    * Transactions extract করা কিছু সময় নিতে পারে, আমরা সম্ভবত এটি [asynchronously with a queue](https://github.com/donnemartin/system-design-primer#asynchronism) করতে চাই, যদিও এটি additional complexity প্রবর্তন করে
* **Transaction Extraction Service** নিম্নলিখিত কাজগুলো করে:
    * **Queue** থেকে pull করে এবং financial institution থেকে প্রদত্ত account এর জন্য transactions extract করে, results raw log files হিসাবে **Object Store** এ storing করে
    * প্রতিটি transaction categorize করতে **Category Service** ব্যবহার করে
    * Category দ্বারা aggregate monthly spending calculate করতে **Budget Service** ব্যবহার করে
        * **Budget Service** users জানাতে **Notification Service** ব্যবহার করে যদি তারা budget এর কাছে আসছে বা exceed করেছে
    * Categorized transactions সহ **SQL Database** `transactions` table আপডেট করে
    * Category দ্বারা aggregate monthly spending সহ **SQL Database** `monthly_spending` table আপডেট করে
    * **Notification Service** এর মাধ্যমে user কে জানায় transactions complete হয়েছে:
        * Notifications asynchronously পাঠাতে একটি **Queue** (pictured নয়) ব্যবহার করে

`transactions` table নিম্নলিখিত structure থাকতে পারে:

```
id int NOT NULL AUTO_INCREMENT
created_at datetime NOT NULL
seller varchar(32) NOT NULL
amount decimal NOT NULL
user_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

আমরা `id`, `user_id `, এবং `created_at` এ একটি [index](https://github.com/donnemartin/system-design-primer#use-good-indices) তৈরি করব।

`monthly_spending` table নিম্নলিখিত structure থাকতে পারে:

```
id int NOT NULL AUTO_INCREMENT
month_year date NOT NULL
category varchar(32)
amount decimal NOT NULL
user_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

আমরা `id` এবং `user_id ` এ একটি [index](https://github.com/donnemartin/system-design-primer#use-good-indices) তৈরি করব।

#### Category service

**Category Service** এর জন্য, আমরা most popular sellers সহ একটি seller-to-category dictionary seed করতে পারি। যদি আমরা 50,000 sellers estimate করি এবং estimate করি প্রতিটি entry 255 bytes এর কম নেয়, dictionary শুধুমাত্র প্রায় 12 MB memory নেবে।

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

```python
class DefaultCategories(Enum):

    HOUSING = 0
    FOOD = 1
    GAS = 2
    SHOPPING = 3
    ...

seller_category_map = {}
seller_category_map['Exxon'] = DefaultCategories.GAS
seller_category_map['Target'] = DefaultCategories.SHOPPING
...
```

Map এ initially seeded না হওয়া sellers এর জন্য, আমরা আমাদের users প্রদত্ত manual category overrides evaluate করে একটি crowdsourcing effort ব্যবহার করতে পারি। আমরা O(1) time এ quickly lookup করতে একটি heap ব্যবহার করতে পারি seller প্রতি top manual override।

```python
class Categorizer(object):

    def __init__(self, seller_category_map, seller_category_crowd_overrides_map):
        self.seller_category_map = seller_category_map
        self.seller_category_crowd_overrides_map = \
            seller_category_crowd_overrides_map

    def categorize(self, transaction):
        if transaction.seller in self.seller_category_map:
            return self.seller_category_map[transaction.seller]
        elif transaction.seller in self.seller_category_crowd_overrides_map:
            self.seller_category_map[transaction.seller] = \
                self.seller_category_crowd_overrides_map[transaction.seller].peek_min()
            return self.seller_category_map[transaction.seller]
        return None
```

Transaction implementation:

```python
class Transaction(object):

    def __init__(self, created_at, seller, amount):
        self.created_at = created_at
        self.seller = seller
        self.amount = amount
```

### Use case: Service recommends a budget

শুরু করতে, আমরা একটি generic budget template ব্যবহার করতে পারি যা income tiers এর উপর ভিত্তি করে category amounts allocate করে। এই approach ব্যবহার করে, আমাদের constraints এ identified 100 million budget items store করার প্রয়োজন হবে না, শুধুমাত্র user overrides করে এমন items। যদি একটি user একটি budget category override করে, যা আমরা `TABLE budget_overrides` এ override store করতে পারি।

```python
class Budget(object):

    def __init__(self, income):
        self.income = income
        self.categories_to_budget_map = self.create_budget_template()

    def create_budget_template(self):
        return {
            DefaultCategories.HOUSING: self.income * .4,
            DefaultCategories.FOOD: self.income * .2,
            DefaultCategories.GAS: self.income * .1,
            DefaultCategories.SHOPPING: self.income * .2,
            ...
        }

    def override_category_budget(self, category, amount):
        self.categories_to_budget_map[category] = amount
```

**Budget Service** এর জন্য, আমরা `transactions` table এ SQL queries run করতে পারি `monthly_spending` aggregate table generate করতে। `monthly_spending` table সম্ভবত total 5 billion transactions এর চেয়ে অনেক কম rows থাকবে, কারণ users সাধারণত প্রতি মাসে অনেক transactions থাকে।

একটি alternative হিসাবে, আমরা raw transaction files এ **MapReduce** jobs run করতে পারি:

* প্রতিটি transaction categorize করতে
* Category দ্বারা aggregate monthly spending generate করতে

Transaction files এ analyses run করা database এর load significantly কমাতে পারে।

যদি user একটি category আপডেট করে, আমরা analysis re-run করতে **Budget Service** call করতে পারি।

**আপনার interviewer এর সাথে clarify করুন আপনি কতটা code লেখার আশা করা হয়**।

Sample log file format, tab delimited:

```
user_id   timestamp   seller  amount
```

**MapReduce** implementation:

```python
class SpendingByCategory(MRJob):

    def __init__(self, categorizer):
        self.categorizer = categorizer
        self.current_year_month = calc_current_year_month()
        ...

    def calc_current_year_month(self):
        """Return the current year and month."""
        ...

    def extract_year_month(self, timestamp):
        """Return the year and month portions of the timestamp."""
        ...

    def handle_budget_notifications(self, key, total):
        """Call notification API if nearing or exceeded budget."""
        ...

    def mapper(self, _, line):
        """Parse each log line, extract and transform relevant lines.

        Argument line will be of the form:

        user_id   timestamp   seller  amount

        Using the categorizer to convert seller to category,
        emit key value pairs of the form:

        (user_id, 2016-01, shopping), 25
        (user_id, 2016-01, shopping), 100
        (user_id, 2016-01, gas), 50
        """
        user_id, timestamp, seller, amount = line.split('\t')
        category = self.categorizer.categorize(seller)
        period = self.extract_year_month(timestamp)
        if period == self.current_year_month:
            yield (user_id, period, category), amount

    def reducer(self, key, value):
        """Sum values for each key.

        (user_id, 2016-01, shopping), 125
        (user_id, 2016-01, gas), 50
        """
        total = sum(values)
        yield key, sum(values)
```

## Step 4: Scale the design

> Constraints দেওয়া থাকলে bottlenecks identify করুন এবং address করুন।

![Imgur](http://i.imgur.com/V5q57vU.png)

**গুরুত্বপূর্ণ: শুধু initial design থেকে final design এ সরাসরি jump করবেন না!**

State করুন আপনি 1) **Benchmark/Load Test**, 2) **Profile** bottlenecks এর জন্য 3) alternatives এবং trade-offs evaluate করার সময় bottlenecks address করবেন এবং 4) repeat করবেন। [Design a system that scales to millions of users on AWS](../scaling_aws/README.md) দেখুন initial design iteratively scale করার একটি sample হিসাবে।

Initial design এর সাথে আপনি যে bottlenecks এর মুখোমুখি হতে পারেন এবং আপনি কীভাবে প্রতিটি address করতে পারেন তা নিয়ে আলোচনা করা গুরুত্বপূর্ণ। উদাহরণস্বরূপ, একাধিক **Web Servers** সহ একটি **Load Balancer** যোগ করা দ্বারা কী issues address করা হয়? **CDN**? **Master-Slave Replicas**? প্রতিটির জন্য alternatives এবং **Trade-Offs** কী?

আমরা design complete করতে এবং scalability issues address করতে কিছু components পরিচয় করাব। Internal load balancers clutter কমাতে দেখানো হয়নি।

*আলোচনা repeat করা এড়াতে*, main talking points, tradeoffs, এবং alternatives এর জন্য নিম্নলিখিত [system design topics](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) দেখুন:

* [DNS](https://github.com/donnemartin/system-design-primer#domain-name-system)
* [CDN](https://github.com/donnemartin/system-design-primer#content-delivery-network)
* [Load balancer](https://github.com/donnemartin/system-design-primer#load-balancer)
* [Horizontal scaling](https://github.com/donnemartin/system-design-primer#horizontal-scaling)
* [Web server (reverse proxy)](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* [API server (application layer)](https://github.com/donnemartin/system-design-primer#application-layer)
* [Cache](https://github.com/donnemartin/system-design-primer#cache)
* [Relational database management system (RDBMS)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)
* [SQL write master-slave failover](https://github.com/donnemartin/system-design-primer#fail-over)
* [Master-slave replication](https://github.com/donnemartin/system-design-primer#master-slave-replication)
* [Asynchronism](https://github.com/donnemartin/system-design-primer#asynchronism)
* [Consistency patterns](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [Availability patterns](https://github.com/donnemartin/system-design-primer#availability-patterns)

আমরা একটি additional use case যোগ করব: **User** summaries এবং transactions access করে।

User sessions, category দ্বারা aggregate stats, এবং recent transactions Redis বা Memcached এর মতো একটি **Memory Cache** এ রাখা যেতে পারে।

* **Client** **Web Server** এ একটি read request পাঠায়
* **Web Server** request **Read API** server এ forward করে
    * Static content S3 এর মতো **Object Store** থেকে serve করা যেতে পারে, যা **CDN** এ cached
* **Read API** server নিম্নলিখিত কাজগুলো করে:
    * Content এর জন্য **Memory Cache** check করে
        * যদি url **Memory Cache** এ থাকে, cached contents return করে
        * অন্যথায়
            * যদি url **SQL Database** এ থাকে, contents fetch করে
                * Contents সহ **Memory Cache** আপডেট করে

Tradeoffs এবং alternatives এর জন্য [When to update the cache](https://github.com/donnemartin/system-design-primer#when-to-update-the-cache) দেখুন। উপরের approach [cache-aside](https://github.com/donnemartin/system-design-primer#cache-aside) বর্ণনা করে।

**SQL Database** এ `monthly_spending` aggregate table রাখার পরিবর্তে, আমরা Amazon Redshift বা Google BigQuery এর মতো একটি data warehousing solution ব্যবহার করে একটি separate **Analytics Database** তৈরি করতে পারি।

আমরা সম্ভবত database এ শুধুমাত্র একটি মাসের `transactions` data store করতে চাই, যখন বাকি একটি data warehouse বা একটি **Object Store** এ store করি। Amazon S3 এর মতো একটি **Object Store** প্রতি মাসে 250 GB নতুন content এর constraint comfortably handle করতে পারে।

200 *average* read requests per second (peak এ higher) address করতে, popular content এর traffic **Memory Cache** দ্বারা handle করা উচিত database এর পরিবর্তে। **Memory Cache** unevenly distributed traffic এবং traffic spikes handle করতেও উপযোগী। **SQL Read Replicas** cache misses handle করতে সক্ষম হওয়া উচিত, যতক্ষণ replicas writes replicate করতে bogged down না হয়।

2,000 *average* transaction writes per second (peak এ higher) একটি single **SQL Write Master-Slave** এর জন্য tough হতে পারে। আমাদের সম্ভবত additional SQL scaling patterns employ করতে হবে:

* [Federation](https://github.com/donnemartin/system-design-primer#federation)
* [Sharding](https://github.com/donnemartin/system-design-primer#sharding)
* [Denormalization](https://github.com/donnemartin/system-design-primer#denormalization)
* [SQL Tuning](https://github.com/donnemartin/system-design-primer#sql-tuning)

আমাদের কিছু ডেটা একটি **NoSQL Database** এ move করারও বিবেচনা করা উচিত।

## Additional talking points

> সমস্যা scope এবং remaining time এর উপর নির্ভর করে dive into করার জন্য additional topics।

#### NoSQL

* [Key-value store](https://github.com/donnemartin/system-design-primer#key-value-store)
* [Document store](https://github.com/donnemartin/system-design-primer#document-store)
* [Wide column store](https://github.com/donnemartin/system-design-primer#wide-column-store)
* [Graph database](https://github.com/donnemartin/system-design-primer#graph-database)
* [SQL vs NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql)

### Caching

* কোথায় cache করতে হবে
    * [Client caching](https://github.com/donnemartin/system-design-primer#client-caching)
    * [CDN caching](https://github.com/donnemartin/system-design-primer#cdn-caching)
    * [Web server caching](https://github.com/donnemartin/system-design-primer#web-server-caching)
    * [Database caching](https://github.com/donnemartin/system-design-primer#database-caching)
    * [Application caching](https://github.com/donnemartin/system-design-primer#application-caching)
* কী cache করতে হবে
    * [Caching at the database query level](https://github.com/donnemartin/system-design-primer#caching-at-the-database-query-level)
    * [Caching at the object level](https://github.com/donnemartin/system-design-primer#caching-at-the-object-level)
* কখন cache update করতে হবে
    * [Cache-aside](https://github.com/donnemartin/system-design-primer#cache-aside)
    * [Write-through](https://github.com/donnemartin/system-design-primer#write-through)
    * [Write-behind (write-back)](https://github.com/donnemartin/system-design-primer#write-behind-write-back)
    * [Refresh ahead](https://github.com/donnemartin/system-design-primer#refresh-ahead)

### Asynchronism and microservices

* [Message queues](https://github.com/donnemartin/system-design-primer#message-queues)
* [Task queues](https://github.com/donnemartin/system-design-primer#task-queues)
* [Back pressure](https://github.com/donnemartin/system-design-primer#back-pressure)
* [Microservices](https://github.com/donnemartin/system-design-primer#microservices)

### Communications

* Tradeoffs নিয়ে আলোচনা করুন:
    * Clients এর সাথে external communication - [HTTP APIs following REST](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)
    * Internal communications - [RPC](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)
* [Service discovery](https://github.com/donnemartin/system-design-primer#service-discovery)

### Security

[security section](https://github.com/donnemartin/system-design-primer#security) দেখুন।

### Latency numbers

[Latency numbers every programmer should know](https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know) দেখুন।

### Ongoing

* Bottlenecks আসার সাথে সাথে address করতে আপনার system benchmark এবং monitor করা চালিয়ে যান
* Scaling একটি iterative process

