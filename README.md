*[English](README.md) ∙ [日本語](README-ja.md) ∙ [简体中文](README-zh-Hans.md) ∙ [繁體中文](README-zh-TW.md) | [العَرَبِيَّة‎](https://github.com/donnemartin/system-design-primer/issues/170) ∙ [বাংলা](https://github.com/donnemartin/system-design-primer/issues/220) ∙ [Português do Brasil](https://github.com/donnemartin/system-design-primer/issues/40) ∙ [Deutsch](https://github.com/donnemartin/system-design-primer/issues/186) ∙ [ελληνικά](https://github.com/donnemartin/system-design-primer/issues/130) ∙ [עברית](https://github.com/donnemartin/system-design-primer/issues/272) ∙ [Italiano](https://github.com/donnemartin/system-design-primer/issues/104) ∙ [한국어](https://github.com/donnemartin/system-design-primer/issues/102) ∙ [فارسی](https://github.com/donnemartin/system-design-primer/issues/110) ∙ [Polski](https://github.com/donnemartin/system-design-primer/issues/68) ∙ [русский язык](https://github.com/donnemartin/system-design-primer/issues/87) ∙ [Español](https://github.com/donnemartin/system-design-primer/issues/136) ∙ [ภาษาไทย](https://github.com/donnemartin/system-design-primer/issues/187) ∙ [Türkçe](https://github.com/donnemartin/system-design-primer/issues/39) ∙ [tiếng Việt](https://github.com/donnemartin/system-design-primer/issues/127) ∙ [Français](https://github.com/donnemartin/system-design-primer/issues/250) | [Add Translation](https://github.com/donnemartin/system-design-primer/issues/28)*

**এই গাইডটি [অনুবাদ করতে](TRANSLATIONS.md) সাহায্য করুন!**

# The System Design Primer

<p align="center">
  <img src="images/jj3A5N8.png">
  <br/>
</p>

## Motivation

> বড় আকারের সিস্টেম ডিজাইন করার পদ্ধতি শিখুন।
>
> সিস্টেম ডিজাইন ইন্টারভিউয়ের জন্য প্রস্তুতি নিন।

### বড় আকারের সিস্টেম ডিজাইন করার পদ্ধতি শিখুন

স্কেলযোগ্য সিস্টেম ডিজাইন করার পদ্ধতি শিখলে আপনি একজন ভালো ইঞ্জিনিয়ার হতে পারবেন।

সিস্টেম ডিজাইন একটি বিস্তৃত বিষয়। সিস্টেম ডিজাইনের নীতিমালা সম্পর্কে ওয়েবে **প্রচুর পরিমাণে সম্পদ ছড়িয়ে ছিটিয়ে রয়েছে**।

এই রিপোজিটরি হল একটি **সুসংগঠিত সংগ্রহ** যা আপনাকে স্কেলযোগ্য সিস্টেম তৈরি করার পদ্ধতি শিখতে সাহায্য করবে।

### ওপেন সোর্স কমিউনিটি থেকে শিখুন

এটি একটি ক্রমাগত আপডেট হওয়া, ওপেন সোর্স প্রকল্প।

[অবদান](#contributing) স্বাগত!

### সিস্টেম ডিজাইন ইন্টারভিউয়ের জন্য প্রস্তুতি নিন

কোডিং ইন্টারভিউ ছাড়াও, সিস্টেম ডিজাইন অনেক টেক কোম্পানির **প্রযুক্তিগত ইন্টারভিউ প্রক্রিয়ার** একটি **অপরিহার্য উপাদান**।

**সাধারণ সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্ন অনুশীলন করুন** এবং **নমুনা সমাধানের** সাথে আপনার ফলাফল **তুলনা করুন**: আলোচনা, কোড, এবং ডায়াগ্রাম।

ইন্টারভিউ প্রস্তুতির জন্য অতিরিক্ত বিষয়:

* [স্টাডি গাইড](#study-guide)
* [সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্নের কাছে যাওয়ার পদ্ধতি](#how-to-approach-a-system-design-interview-question)
* [সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্ন, **সমাধান সহ**](#system-design-interview-questions-with-solutions)
* [অবজেক্ট-ওরিয়েন্টেড ডিজাইন ইন্টারভিউ প্রশ্ন, **সমাধান সহ**](#object-oriented-design-interview-questions-with-solutions)
* [অতিরিক্ত সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্ন](#additional-system-design-interview-questions)

## Anki flashcards

<p align="center">
  <img src="images/zdCAkB3.png">
  <br/>
</p>

প্রদত্ত [Anki flashcard decks](https://apps.ankiweb.net/) spaced repetition ব্যবহার করে আপনাকে মূল সিস্টেম ডিজাইন ধারণাগুলো মনে রাখতে সাহায্য করে।

* [System design deck](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/System%20Design.apkg)
* [System design exercises deck](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/System%20Design%20Exercises.apkg)
* [Object oriented design exercises deck](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/OO%20Design.apkg)

ভ্রমণের সময় ব্যবহারের জন্য চমৎকার।

### Coding Resource: Interactive Coding Challenges

[**Coding Interview**](https://github.com/donnemartin/interactive-coding-challenges) এর জন্য প্রস্তুতিতে সাহায্য করার জন্য সম্পদ খুঁজছেন?

<p align="center">
  <img src="images/b4YtAEN.png">
  <br/>
</p>

সিস্টার রিপো [**Interactive Coding Challenges**](https://github.com/donnemartin/interactive-coding-challenges) দেখুন, যেখানে একটি অতিরিক্ত Anki deck রয়েছে:

* [Coding deck](https://github.com/donnemartin/interactive-coding-challenges/tree/master/anki_cards/Coding.apkg)

## Contributing

> কমিউনিটি থেকে শিখুন।

সাহায্য করার জন্য pull request জমা দিতে নির্দ্বিধায় করুন:

* ত্রুটি ঠিক করা
* সেকশন উন্নত করা
* নতুন সেকশন যোগ করা
* [অনুবাদ](https://github.com/donnemartin/system-design-primer/issues/28)

যে বিষয়বস্তুর কিছুটা পলিশিং প্রয়োজন তা [under development](#under-development) এ রাখা হয়েছে।

[Contributing Guidelines](CONTRIBUTING.md) পর্যালোচনা করুন।

## Index of system design topics

> বিভিন্ন সিস্টেম ডিজাইন বিষয়ের সারসংক্ষেপ, সুবিধা এবং অসুবিধা সহ। **সবকিছুই একটি trade-off**।
>
> প্রতিটি সেকশনে আরও গভীর সম্পদের লিঙ্ক রয়েছে।

<p align="center">
  <img src="images/jrUBAF7.png">
  <br/>
</p>

* [System design topics: start here](#system-design-topics-start-here)
    * [Step 1: Review the scalability video lecture](#step-1-review-the-scalability-video-lecture)
    * [Step 2: Review the scalability article](#step-2-review-the-scalability-article)
    * [Next steps](#next-steps)
* [Performance vs scalability](#performance-vs-scalability)
* [Latency vs throughput](#latency-vs-throughput)
* [Availability vs consistency](#availability-vs-consistency)
    * [CAP theorem](#cap-theorem)
        * [CP - consistency and partition tolerance](#cp---consistency-and-partition-tolerance)
        * [AP - availability and partition tolerance](#ap---availability-and-partition-tolerance)
* [Consistency patterns](#consistency-patterns)
    * [Weak consistency](#weak-consistency)
    * [Eventual consistency](#eventual-consistency)
    * [Strong consistency](#strong-consistency)
* [Availability patterns](#availability-patterns)
    * [Fail-over](#fail-over)
    * [Replication](#replication)
    * [Availability in numbers](#availability-in-numbers)
* [Domain name system](#domain-name-system)
* [Content delivery network](#content-delivery-network)
    * [Push CDNs](#push-cdns)
    * [Pull CDNs](#pull-cdns)
* [Load balancer](#load-balancer)
    * [Active-passive](#active-passive)
    * [Active-active](#active-active)
    * [Layer 4 load balancing](#layer-4-load-balancing)
    * [Layer 7 load balancing](#layer-7-load-balancing)
    * [Horizontal scaling](#horizontal-scaling)
* [Reverse proxy (web server)](#reverse-proxy-web-server)
    * [Load balancer vs reverse proxy](#load-balancer-vs-reverse-proxy)
* [Application layer](#application-layer)
    * [Microservices](#microservices)
    * [Service discovery](#service-discovery)
* [Database](#database)
    * [Relational database management system (RDBMS)](#relational-database-management-system-rdbms)
        * [Master-slave replication](#master-slave-replication)
        * [Master-master replication](#master-master-replication)
        * [Federation](#federation)
        * [Sharding](#sharding)
        * [Denormalization](#denormalization)
        * [SQL tuning](#sql-tuning)
    * [NoSQL](#nosql)
        * [Key-value store](#key-value-store)
        * [Document store](#document-store)
        * [Wide column store](#wide-column-store)
        * [Graph Database](#graph-database)
    * [SQL or NoSQL](#sql-or-nosql)
* [Cache](#cache)
    * [Client caching](#client-caching)
    * [CDN caching](#cdn-caching)
    * [Web server caching](#web-server-caching)
    * [Database caching](#database-caching)
    * [Application caching](#application-caching)
    * [Caching at the database query level](#caching-at-the-database-query-level)
    * [Caching at the object level](#caching-at-the-object-level)
    * [When to update the cache](#when-to-update-the-cache)
        * [Cache-aside](#cache-aside)
        * [Write-through](#write-through)
        * [Write-behind (write-back)](#write-behind-write-back)
        * [Refresh-ahead](#refresh-ahead)
* [Asynchronism](#asynchronism)
    * [Message queues](#message-queues)
    * [Task queues](#task-queues)
    * [Back pressure](#back-pressure)
* [Communication](#communication)
    * [Transmission control protocol (TCP)](#transmission-control-protocol-tcp)
    * [User datagram protocol (UDP)](#user-datagram-protocol-udp)
    * [Remote procedure call (RPC)](#remote-procedure-call-rpc)
    * [Representational state transfer (REST)](#representational-state-transfer-rest)
* [Security](#security)
* [Appendix](#appendix)
    * [Powers of two table](#powers-of-two-table)
    * [Latency numbers every programmer should know](#latency-numbers-every-programmer-should-know)
    * [Additional system design interview questions](#additional-system-design-interview-questions)
    * [Real world architectures](#real-world-architectures)
    * [Company architectures](#company-architectures)
    * [Company engineering blogs](#company-engineering-blogs)
* [Under development](#under-development)
* [Credits](#credits)
* [Contact info](#contact-info)
* [License](#license)

## Study guide

> আপনার ইন্টারভিউ টাইমলাইনের উপর ভিত্তি করে পর্যালোচনার জন্য প্রস্তাবিত বিষয় (ছোট, মাঝারি, দীর্ঘ)।

![Imgur](images/OfVllex.png)

**প্র: ইন্টারভিউয়ের জন্য, এখানকার সবকিছু জানা কি আমার প্রয়োজন?**

**উ: না, ইন্টারভিউয়ের প্রস্তুতির জন্য এখানকার সবকিছু জানার প্রয়োজন নেই**।

ইন্টারভিউতে আপনাকে কী জিজ্ঞাসা করা হবে তা বিভিন্ন চলকের উপর নির্ভর করে, যেমন:

* আপনার কতটা অভিজ্ঞতা আছে
* আপনার প্রযুক্তিগত পটভূমি কী
* আপনি কোন পদগুলোর জন্য ইন্টারভিউ দিচ্ছেন
* আপনি কোন কোম্পানিগুলোর সাথে ইন্টারভিউ দিচ্ছেন
* ভাগ্য

আরও অভিজ্ঞ প্রার্থীদের সাধারণত সিস্টেম ডিজাইন সম্পর্কে আরও জানার আশা করা হয়। আর্কিটেক্ট বা টিম লিডদের সাধারণত individual contributors এর চেয়ে আরও জানার আশা করা হয়। শীর্ষ টেক কোম্পানিগুলোতে সম্ভবত এক বা একাধিক ডিজাইন ইন্টারভিউ রাউন্ড থাকবে।

প্রশস্তভাবে শুরু করুন এবং কয়েকটি ক্ষেত্রে গভীরে যান। বিভিন্ন মূল সিস্টেম ডিজাইন বিষয় সম্পর্কে একটু জানা সাহায্য করে। আপনার টাইমলাইন, অভিজ্ঞতা, আপনি কোন পদগুলোর জন্য ইন্টারভিউ দিচ্ছেন এবং কোন কোম্পানিগুলোর সাথে ইন্টারভিউ দিচ্ছেন তার উপর ভিত্তি করে নিম্নলিখিত গাইডটি সামঞ্জস্য করুন।

* **ছোট টাইমলাইন** - সিস্টেম ডিজাইন বিষয়গুলোর জন্য **প্রশস্ততা** লক্ষ্য করুন। **কিছু** ইন্টারভিউ প্রশ্ন সমাধান করে অনুশীলন করুন।
* **মাঝারি টাইমলাইন** - সিস্টেম ডিজাইন বিষয়গুলোর জন্য **প্রশস্ততা** এবং **কিছু গভীরতা** লক্ষ্য করুন। **অনেক** ইন্টারভিউ প্রশ্ন সমাধান করে অনুশীলন করুন।
* **দীর্ঘ টাইমলাইন** - সিস্টেম ডিজাইন বিষয়গুলোর জন্য **প্রশস্ততা** এবং **আরও গভীরতা** লক্ষ্য করুন। **অধিকাংশ** ইন্টারভিউ প্রশ্ন সমাধান করে অনুশীলন করুন।

|| | Short | Medium | Long |
|---|---|---|---|
|| [System design topics](#index-of-system-design-topics) পড়ে সিস্টেম কীভাবে কাজ করে তার একটি বিস্তৃত বোঝাপড়া পান | :+1: | :+1: | :+1: |
|| আপনি যে কোম্পানিগুলোর সাথে ইন্টারভিউ দিচ্ছেন তাদের [Company engineering blogs](#company-engineering-blogs) এ কয়েকটি নিবন্ধ পড়ুন | :+1: | :+1: | :+1: |
|| কয়েকটি [Real world architectures](#real-world-architectures) পড়ুন | :+1: | :+1: | :+1: |
|| [How to approach a system design interview question](#how-to-approach-a-system-design-interview-question) পর্যালোচনা করুন | :+1: | :+1: | :+1: |
|| [System design interview questions with solutions](#system-design-interview-questions-with-solutions) নিয়ে কাজ করুন | Some | Many | Most |
|| [Object-oriented design interview questions with solutions](#object-oriented-design-interview-questions-with-solutions) নিয়ে কাজ করুন | Some | Many | Most |
|| [Additional system design interview questions](#additional-system-design-interview-questions) পর্যালোচনা করুন | Some | Many | Most |

## How to approach a system design interview question

> সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্নের কাছে যাওয়ার পদ্ধতি।

সিস্টেম ডিজাইন ইন্টারভিউ হল একটি **খোলা-মুখী কথোপকথন**। আপনাকে এটি নেতৃত্ব দেওয়ার আশা করা হয়।

আলোচনা পরিচালনা করতে আপনি নিম্নলিখিত ধাপগুলো ব্যবহার করতে পারেন। এই প্রক্রিয়াটি দৃঢ় করতে সাহায্য করার জন্য, নিম্নলিখিত ধাপগুলো ব্যবহার করে [System design interview questions with solutions](#system-design-interview-questions-with-solutions) সেকশন নিয়ে কাজ করুন।

### Step 1: Outline use cases, constraints, and assumptions

আবশ্যকতা সংগ্রহ করুন এবং সমস্যাটি স্কোপ করুন। use cases এবং constraints স্পষ্ট করতে প্রশ্ন করুন। assumptions নিয়ে আলোচনা করুন।

* কে এটি ব্যবহার করবে?
* তারা কীভাবে এটি ব্যবহার করবে?
* কতজন ব্যবহারকারী আছে?
* সিস্টেমটি কী করে?
* সিস্টেমের ইনপুট এবং আউটপুট কী?
* আমরা কতটা ডেটা হ্যান্ডল করার আশা করি?
* আমরা প্রতি সেকেন্ডে কতটি রিকোয়েস্ট আশা করি?
* প্রত্যাশিত read to write ratio কী?

### Step 2: Create a high level design

সব গুরুত্বপূর্ণ কম্পোনেন্ট সহ একটি high level design রূপরেখা তৈরি করুন।

* মূল কম্পোনেন্ট এবং সংযোগগুলো স্কেচ করুন
* আপনার ধারণাগুলো ন্যায়সঙ্গত করুন

### Step 3: Design core components

প্রতিটি মূল কম্পোনেন্টের জন্য বিস্তারিতভাবে যান। উদাহরণস্বরূপ, যদি আপনাকে [design a url shortening service](solutions/system_design/pastebin/bangla.md) করতে বলা হয়, আলোচনা করুন:

* সম্পূর্ণ url এর hash তৈরি এবং সংরক্ষণ করা
    * [MD5](solutions/system_design/pastebin/bangla.md) এবং [Base62](solutions/system_design/pastebin/bangla.md)
    * Hash collisions
    * SQL বা NoSQL
    * Database schema
* একটি hashed url কে সম্পূর্ণ url এ রূপান্তর করা
    * Database lookup
* API এবং object-oriented design

### Step 4: Scale the design

Constraints দেওয়া থাকলে bottlenecks চিহ্নিত করুন এবং সমাধান করুন। উদাহরণস্বরূপ, scalability সমস্যা সমাধানের জন্য আপনার কি নিম্নলিখিতগুলোর প্রয়োজন?

* Load balancer
* Horizontal scaling
* Caching
* Database sharding

সম্ভাব্য সমাধান এবং trade-offs নিয়ে আলোচনা করুন। সবকিছুই একটি trade-off। [principles of scalable system design](#index-of-system-design-topics) ব্যবহার করে bottlenecks সমাধান করুন।

### Back-of-the-envelope calculations

আপনাকে কিছু অনুমান হাতে করতে বলা হতে পারে। নিম্নলিখিত সম্পদের জন্য [Appendix](#appendix) দেখুন:

* [Use back of the envelope calculations](http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html)
* [Powers of two table](#powers-of-two-table)
* [Latency numbers every programmer should know](#latency-numbers-every-programmer-should-know)

### Source(s) and further reading

কী আশা করা যায় তার একটি ভালো ধারণা পেতে নিম্নলিখিত লিঙ্কগুলো দেখুন:

* [How to ace a systems design interview](https://web.archive.org/web/20210505130322/https://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/)
* [The system design interview](http://www.hiredintech.com/system-design)
* [Intro to Architecture and Systems Design Interviews](https://www.youtube.com/watch?v=ZgdS0EUmn70)
* [System design template](https://leetcode.com/discuss/career/229177/My-System-Design-Template)

## System design interview questions with solutions

> নমুনা আলোচনা, কোড, এবং ডায়াগ্রাম সহ সাধারণ সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্ন।
>
> সমাধানগুলো `solutions/` ফোল্ডারে বিষয়বস্তুর সাথে লিঙ্ক করা।

| Question | |
|---|---|
| Design Pastebin.com (or Bit.ly) | [Solution](solutions/system_design/pastebin/bangla.md) |
| Design the Twitter timeline and search (or Facebook feed and search) | [Solution](solutions/system_design/twitter/bangla.md) |
| Design a web crawler | [Solution](solutions/system_design/web_crawler/bangla.md) |
| Design Mint.com | [Solution](solutions/system_design/mint/bangla.md) |
| Design the data structures for a social network | [Solution](solutions/system_design/social_graph/bangla.md) |
| Design a key-value store for a search engine | [Solution](solutions/system_design/query_cache/bangla.md) |
| Design Amazon's sales ranking by category feature | [Solution](solutions/system_design/sales_rank/bangla.md) |
| Design a system that scales to millions of users on AWS | [Solution](solutions/system_design/scaling_aws/bangla.md) |
| Add a system design question | [Contribute](#contributing) |

### Design Pastebin.com (or Bit.ly)

[View exercise and solution](solutions/system_design/pastebin/bangla.md)

![Imgur](images/4edXG0T.png)

### Design the Twitter timeline and search (or Facebook feed and search)

[View exercise and solution](solutions/system_design/twitter/bangla.md)

![Imgur](images/jrUBAF7.png)

### Design a web crawler

[View exercise and solution](solutions/system_design/web_crawler/bangla.md)

![Imgur](images/bWxPtQA.png)

### Design Mint.com

[View exercise and solution](solutions/system_design/mint/bangla.md)

![Imgur](images/V5q57vU.png)

### Design the data structures for a social network

[View exercise and solution](solutions/system_design/social_graph/bangla.md)

![Imgur](images/cdCv5g7.png)

### Design a key-value store for a search engine

[View exercise and solution](solutions/system_design/query_cache/bangla.md)

![Imgur](images/4j99mhe.png)

### Design Amazon's sales ranking by category feature

[View exercise and solution](solutions/system_design/sales_rank/bangla.md)

![Imgur](images/MzExP06.png)

### Design a system that scales to millions of users on AWS

[View exercise and solution](solutions/system_design/scaling_aws/bangla.md)

![Imgur](images/jj3A5N8.png)

## Object-oriented design interview questions with solutions

> নমুনা আলোচনা, কোড, এবং ডায়াগ্রাম সহ সাধারণ object-oriented design ইন্টারভিউ প্রশ্ন।
>
> সমাধানগুলো `solutions/` ফোল্ডারে বিষয়বস্তুর সাথে লিঙ্ক করা।

>**Note: This section is under development**

| Question | |
|---|---|
| Design a hash map | [Solution](solutions/object_oriented_design/hash_table/hash_map.ipynb)  |
| Design a least recently used cache | [Solution](solutions/object_oriented_design/lru_cache/lru_cache.ipynb)  |
| Design a call center | [Solution](solutions/object_oriented_design/call_center/call_center.ipynb)  |
| Design a deck of cards | [Solution](solutions/object_oriented_design/deck_of_cards/deck_of_cards.ipynb)  |
| Design a parking lot | [Solution](solutions/object_oriented_design/parking_lot/parking_lot.ipynb)  |
| Design a chat server | [Solution](solutions/object_oriented_design/online_chat/online_chat.ipynb)  |
| Design a circular array | [Contribute](#contributing)  |
| Add an object-oriented design question | [Contribute](#contributing) |

## System design topics: start here

সিস্টেম ডিজাইনে নতুন?

প্রথমে, আপনার সাধারণ নীতিগুলোর একটি মৌলিক বোঝাপড়ার প্রয়োজন হবে, সেগুলো কী, কীভাবে ব্যবহার করা হয়, এবং তাদের সুবিধা এবং অসুবিধা সম্পর্কে শেখা।

### Step 1: Review the scalability video lecture

[Scalability Lecture at Harvard](https://www.youtube.com/watch?v=-W9F__D3oY4)

* Topics covered:
    * Vertical scaling
    * Horizontal scaling
    * Caching
    * Load balancing
    * Database replication
    * Database partitioning

### Step 2: Review the scalability article

[Scalability](https://web.archive.org/web/20221030091841/http://www.lecloud.net/tagged/scalability/chrono)

* Topics covered:
    * [Clones](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
    * [Databases](https://web.archive.org/web/20220602114024/https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database)
    * [Caches](https://web.archive.org/web/20230126233752/https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache)
    * [Asynchronism](https://web.archive.org/web/20220926171507/https://www.lecloud.net/post/9699762917/scalability-for-dummies-part-4-asynchronism)

### Next steps

এরপর, আমরা high-level trade-offs দেখব:

* **Performance** vs **scalability**
* **Latency** vs **throughput**
* **Availability** vs **consistency**

মনে রাখবেন যে **সবকিছুই একটি trade-off**।

তারপর আমরা DNS, CDNs, এবং load balancers এর মতো আরও নির্দিষ্ট বিষয়গুলোর মধ্যে যাব।

## Performance vs scalability

একটি সেবা **scalable** হয় যদি এটি যোগ করা সম্পদের অনুপাতে **performance** বৃদ্ধি করে। সাধারণভাবে, performance বৃদ্ধি মানে আরও বেশি কাজের ইউনিট পরিবেশন করা, তবে এটি আরও বড় কাজের ইউনিট হ্যান্ডল করতেও হতে পারে, যেমন যখন datasets বৃদ্ধি পায়।<sup><a href=http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html>1</a></sup>

Performance vs scalability দেখার আরেকটি উপায়:

* যদি আপনার একটি **performance** সমস্যা থাকে, আপনার সিস্টেম একটি একক ব্যবহারকারীর জন্য ধীর।
* যদি আপনার একটি **scalability** সমস্যা থাকে, আপনার সিস্টেম একটি একক ব্যবহারকারীর জন্য দ্রুত কিন্তু ভারী লোডের অধীনে ধীর।

### Source(s) and further reading

* [A word on scalability](http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)
* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)

## Latency vs throughput

**Latency** হল কিছু কাজ সম্পাদন করতে বা কিছু ফলাফল তৈরি করতে সময়।

**Throughput** হল প্রতি ইউনিট সময়ে এমন কাজ বা ফলাফলের সংখ্যা।

সাধারণভাবে, আপনার **গ্রহণযোগ্য latency** সহ **সর্বোচ্চ throughput** লক্ষ্য করা উচিত।

### Source(s) and further reading

* [Understanding latency vs throughput](https://community.cadence.com/cadence_blogs_8/b/fv/posts/understanding-latency-vs-throughput)

## Availability vs consistency

### CAP theorem

<p align="center">
  <img src="images/bgLMI2u.png">
  <br/>
  <i><a href="https://robertgreiner.com/cap-theorem-revisited">Source: CAP theorem revisited</a></i>
</p>

একটি distributed computer system এ, আপনি নিম্নলিখিত গ্যারান্টিগুলোর মধ্যে শুধুমাত্র দুটি সমর্থন করতে পারেন:

* **Consistency** - প্রতিটি read সবচেয়ে সাম্প্রতিক write পায় বা একটি error
* **Availability** - প্রতিটি request একটি response পায়, নিশ্চিততা ছাড়াই যে এতে তথ্যের সবচেয়ে সাম্প্রতিক সংস্করণ রয়েছে
* **Partition Tolerance** - নেটওয়ার্ক ব্যর্থতার কারণে arbitrary partitioning সত্ত্বেও সিস্টেম কাজ চালিয়ে যায়

*নেটওয়ার্কগুলো নির্ভরযোগ্য নয়, তাই আপনাকে partition tolerance সমর্থন করতে হবে। আপনাকে consistency এবং availability এর মধ্যে একটি সফটওয়্যার tradeoff করতে হবে।*

#### CP - consistency and partition tolerance

Partitioned node থেকে response এর জন্য অপেক্ষা করা timeout error এর ফলাফল হতে পারে। CP হল একটি ভালো পছন্দ যদি আপনার ব্যবসায়িক প্রয়োজন atomic reads এবং writes প্রয়োজন।

#### AP - availability and partition tolerance

Responses যে কোনো node এ উপলব্ধ ডেটার সবচেয়ে সহজলভ্য সংস্করণ ফেরত দেয়, যা সর্বশেষ নাও হতে পারে। Partition সমাধান হলে writes কিছু সময় নিতে পারে propagate হতে।

AP হল একটি ভালো পছন্দ যদি ব্যবসায়িক প্রয়োজন [eventual consistency](#eventual-consistency) অনুমোদন করে বা সিস্টেমকে বাহ্যিক ত্রুটিগুলো সত্ত্বেও কাজ চালিয়ে যেতে হয়।

### Source(s) and further reading

* [CAP theorem revisited](https://robertgreiner.com/cap-theorem-revisited/))
* [A plain english introduction to CAP theorem](http://ksat.me/a-plain-english-introduction-to-cap-theorem)
* [CAP FAQ](https://github.com/henryr/cap-faq)
* [The CAP theorem](https://www.youtube.com/watch?v=k-Yaq8AHlFA)

## Consistency patterns

একই ডেটার একাধিক কপি সহ, আমরা কীভাবে সেগুলো synchronize করব যাতে ক্লায়েন্টদের ডেটার একটি সামঞ্জস্যপূর্ণ দৃশ্য থাকে সেই বিষয়ে আমরা বিকল্পগুলোর মুখোমুখি। [CAP theorem](#cap-theorem) থেকে consistency এর সংজ্ঞা মনে করুন - প্রতিটি read সবচেয়ে সাম্প্রতিক write পায় বা একটি error।

### Weak consistency

একটি write এর পরে, reads এটি দেখতে পারে বা নাও পারে। একটি best effort approach নেওয়া হয়।

এই approach memcached এর মতো সিস্টেমে দেখা যায়। Weak consistency VoIP, video chat, এবং realtime multiplayer games এর মতো real time use cases এ ভালো কাজ করে। উদাহরণস্বরূপ, যদি আপনি একটি ফোন কল করছেন এবং কয়েক সেকেন্ডের জন্য reception হারিয়ে থাকেন, যখন আপনি আবার connection পান তখন আপনি connection loss এর সময় যা বলা হয়েছিল তা শুনতে পাবেন না।

### Eventual consistency

একটি write এর পরে, reads অবশেষে এটি দেখবে (সাধারণত milliseconds এর মধ্যে)। ডেটা asynchronously replicate হয়।

এই approach DNS এবং email এর মতো সিস্টেমে দেখা যায়। Eventual consistency highly available systems এ ভালো কাজ করে।

### Strong consistency

একটি write এর পরে, reads এটি দেখবে। ডেটা synchronously replicate হয়।

এই approach file systems এবং RDBMSes এ দেখা যায়। Strong consistency transactions প্রয়োজন এমন সিস্টেমে ভালো কাজ করে।

### Source(s) and further reading

* [Transactions across data centers](http://snarfed.org/transactions_across_datacenters_io.html)

## Availability patterns

উচ্চ প্রাপ্যতা সমর্থন করার জন্য দুটি complementary patterns রয়েছে: **fail-over** এবং **replication**।

### Fail-over

#### Active-passive

Active-passive fail-over এর সাথে, active এবং passive server এর মধ্যে standby অবস্থায় heartbeats পাঠানো হয়। যদি heartbeat বাধাগ্রস্ত হয়, passive server active এর IP address গ্রহণ করে এবং service পুনরায় শুরু করে।

Downtime এর দৈর্ঘ্য নির্ধারণ করা হয় passive server ইতিমধ্যে 'hot' standby এ চলছে কিনা বা 'cold' standby থেকে শুরু করতে হবে কিনা তা দিয়ে। শুধুমাত্র active server ট্র্যাফিক হ্যান্ডল করে।

Active-passive failover কে master-slave failover হিসাবেও উল্লেখ করা যেতে পারে।

#### Active-active

Active-active এ, উভয় server ট্র্যাফিক পরিচালনা করে, তাদের মধ্যে load ছড়িয়ে দেয়।

যদি servers public-facing হয়, DNS উভয় server এর public IPs সম্পর্কে জানতে হবে। যদি servers internal-facing হয়, application logic উভয় server সম্পর্কে জানতে হবে।

Active-active failover কে master-master failover হিসাবেও উল্লেখ করা যেতে পারে।

### Disadvantage(s): failover

* Fail-over আরও hardware এবং অতিরিক্ত complexity যোগ করে।
* Active system অন্য nodes এ replicate হওয়ার আগে ব্যর্থ হলে নতুনভাবে লেখা ডেটা হারানোর সম্ভাবনা রয়েছে।

### Replication

#### Master-slave and master-master

এই বিষয়টি [Database](#database) সেকশনে আরও আলোচনা করা হয়েছে:

* [Master-slave replication](#master-slave-replication)
* [Master-master replication](#master-master-replication)

### Availability in numbers

Availability প্রায়শই uptime (বা downtime) হিসাবে পরিমাপ করা হয় সেবা উপলব্ধ থাকার সময়ের শতাংশ হিসাবে। Availability সাধারণত 9s এর সংখ্যায় পরিমাপ করা হয় - 99.99% availability সহ একটি সেবাকে চারটি 9s বলা হয়।

#### 99.9% availability - three 9s

| Duration            | Acceptable downtime|
|---------------------|--------------------|
| Downtime per year   | 8h 45min 57s       |
| Downtime per month  | 43m 49.7s          |
| Downtime per week   | 10m 4.8s           |
| Downtime per day    | 1m 26.4s           |

#### 99.99% availability - four 9s

| Duration            | Acceptable downtime|
|---------------------|--------------------|
| Downtime per year   | 52min 35.7s        |
| Downtime per month  | 4m 23s             |
| Downtime per week   | 1m 5s              |
| Downtime per day    | 8.6s               |

#### Availability in parallel vs in sequence

যদি একটি সেবা ব্যর্থতার প্রবণতা সহ একাধিক কম্পোনেন্ট নিয়ে গঠিত হয়, সেবার সামগ্রিক availability নির্ভর করে কম্পোনেন্টগুলো sequence এ আছে নাকি parallel এ আছে তার উপর।

###### In sequence

দুটি কম্পোনেন্ট < 100% availability সহ sequence এ থাকলে সামগ্রিক availability হ্রাস পায়:

```
Availability (Total) = Availability (Foo) * Availability (Bar)
```

যদি `Foo` এবং `Bar` উভয়ের 99.9% availability থাকে, sequence এ তাদের মোট availability হবে 99.8%।

###### In parallel

দুটি কম্পোনেন্ট < 100% availability সহ parallel এ থাকলে সামগ্রিক availability বৃদ্ধি পায়:

```
Availability (Total) = 1 - (1 - Availability (Foo)) * (1 - Availability (Bar))
```

যদি `Foo` এবং `Bar` উভয়ের 99.9% availability থাকে, parallel এ তাদের মোট availability হবে 99.9999%।

## Domain name system

<p align="center">
  <img src="images/IOyLj4i.jpg">
  <br/>
  <i><a href=http://www.slideshare.net/srikrupa5/dns-security-presentation-issa>Source: DNS security presentation</a></i>
</p>

একটি Domain Name System (DNS) www.example.com এর মতো একটি domain name কে একটি IP address এ রূপান্তর করে।

DNS hierarchical, top level এ কয়েকটি authoritative servers সহ। আপনার router বা ISP lookup করার সময় কোন DNS server(s) এর সাথে যোগাযোগ করতে হবে সেই তথ্য প্রদান করে। Lower level DNS servers mappings cache করে, যা DNS propagation delays এর কারণে stale হতে পারে। DNS results আপনার browser বা OS দ্বারা একটি নির্দিষ্ট সময়ের জন্য cache করা যেতে পারে, [time to live (TTL)](https://en.wikipedia.org/wiki/Time_to_live) দ্বারা নির্ধারিত।

* **NS record (name server)** - আপনার domain/subdomain এর জন্য DNS servers নির্দিষ্ট করে।
* **MX record (mail exchange)** - মেসেজ গ্রহণের জন্য mail servers নির্দিষ্ট করে।
* **A record (address)** - একটি name কে একটি IP address এ নির্দেশ করে।
* **CNAME (canonical)** - একটি name কে অন্য একটি name বা `CNAME` (example.com থেকে www.example.com) বা একটি `A` record এ নির্দেশ করে।

[CloudFlare](https://www.cloudflare.com/dns/) এবং [Route 53](https://aws.amazon.com/route53/) এর মতো সেবাগুলো managed DNS services প্রদান করে। কিছু DNS services বিভিন্ন পদ্ধতিতে ট্র্যাফিক route করতে পারে:

* [Weighted round robin](https://www.jscape.com/blog/load-balancing-algorithms)
    * রক্ষণাবেক্ষণের অধীনে servers এ ট্র্যাফিক যাওয়া প্রতিরোধ করা
    * বিভিন্ন cluster sizes এর মধ্যে ভারসাম্য
    * A/B testing
* [Latency-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-latency.html)
* [Geolocation-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-geo.html)

### Disadvantage(s): DNS

* একটি DNS server অ্যাক্সেস করা একটি সামান্য delay প্রবর্তন করে, যদিও উপরে বর্ণিত caching দ্বারা প্রশমিত।
* DNS server management জটিল হতে পারে এবং সাধারণত [governments, ISPs, and large companies](http://superuser.com/questions/472695/who-controls-the-dns-servers/472729) দ্বারা পরিচালিত হয়।
* DNS services সম্প্রতি [DDoS attack](http://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/) এর অধীনে এসেছে, ব্যবহারকারীদের Twitter এর IP address(es) না জেনে Twitter এর মতো ওয়েবসাইটে অ্যাক্সেস করা থেকে বিরত রাখছে।

### Source(s) and further reading

* [DNS architecture](https://technet.microsoft.com/en-us/library/dd197427(v=ws.10).aspx)
* [Wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)
* [DNS articles](https://support.dnsimple.com/categories/dns/)

## Content delivery network

<p align="center">
  <img src="images/h9TAuGI.jpg">
  <br/>
  <i><a href=https://www.creative-artworks.eu/why-use-a-content-delivery-network-cdn/>Source: Why use a CDN</a></i>
</p>

একটি content delivery network (CDN) হল proxy servers এর একটি globally distributed network, ব্যবহারকারীর কাছাকাছি অবস্থান থেকে content পরিবেশন করে। সাধারণভাবে, HTML/CSS/JS, photos, এবং videos এর মতো static files CDN থেকে পরিবেশন করা হয়, যদিও Amazon এর CloudFront এর মতো কিছু CDNs dynamic content সমর্থন করে। সাইটের DNS resolution ক্লায়েন্টদের জানাবে কোন server এর সাথে যোগাযোগ করতে হবে।

CDNs থেকে content পরিবেশন performance উল্লেখযোগ্যভাবে উন্নত করতে পারে দুটি উপায়ে:

* ব্যবহারকারীরা তাদের কাছাকাছি data centers থেকে content পায়
* আপনার servers CDN fulfill করে এমন requests পরিবেশন করতে হবে না

### Push CDNs

Push CDNs আপনার server এ পরিবর্তন ঘটলে নতুন content গ্রহণ করে। আপনি content প্রদানের সম্পূর্ণ দায়িত্ব নেন, সরাসরি CDN এ upload করেন এবং URLs rewrite করেন CDN নির্দেশ করতে। আপনি কখন content expire হবে এবং কখন আপডেট হবে তা configure করতে পারেন। Content শুধুমাত্র যখন নতুন বা পরিবর্তিত হয় তখন upload করা হয়, traffic কে minimize করে, কিন্তু storage কে maximize করে।

ছোট পরিমাণে ট্র্যাফিক সহ সাইট বা প্রায়শই আপডেট হয় না এমন content সহ সাইট push CDNs এর সাথে ভালো কাজ করে। Content CDNs এ একবার স্থাপন করা হয়, নিয়মিত intervals এ re-pull করার পরিবর্তে।

### Pull CDNs

Pull CDNs প্রথম ব্যবহারকারী content request করলে আপনার server থেকে নতুন content grab করে। আপনি আপনার server এ content রেখে যান এবং URLs rewrite করেন CDN নির্দেশ করতে। এটি CDN এ cache হওয়া পর্যন্ত একটি ধীর request এর ফলাফল দেয়।

একটি [time-to-live (TTL)](https://en.wikipedia.org/wiki/Time_to_live) নির্ধারণ করে content কতক্ষণ cache করা হবে। Pull CDNs CDN এ storage space minimize করে, কিন্তু files expire হলে এবং সেগুলো আসলে পরিবর্তিত হওয়ার আগে pull করা হলে redundant traffic তৈরি করতে পারে।

ভারী ট্র্যাফিক সহ সাইট pull CDNs এর সাথে ভালো কাজ করে, কারণ ট্র্যাফিক আরও সমানভাবে ছড়িয়ে পড়ে শুধুমাত্র সম্প্রতি request করা content CDN এ থাকার সাথে।

### Disadvantage(s): CDN

* CDN costs ট্র্যাফিকের উপর নির্ভর করে উল্লেখযোগ্য হতে পারে, যদিও এটি CDN ব্যবহার না করার ফলে আপনি যে অতিরিক্ত খরচ করবেন তার সাথে ওজন করা উচিত।
* TTL expire হওয়ার আগে content আপডেট করা হলে content stale হতে পারে।
* CDNs static content এর জন্য URLs পরিবর্তন করতে প্রয়োজন CDN নির্দেশ করতে।

### Source(s) and further reading

* [Globally distributed content delivery](https://figshare.com/articles/Globally_distributed_content_delivery/6605972)
* [The differences between push and pull CDNs](http://www.travelblogadvice.com/technical/the-differences-between-push-and-pull-cdns/)
* [Wikipedia](https://en.wikipedia.org/wiki/Content_delivery_network)

## Load balancer

<p align="center">
  <img src="images/h81n9iK.png">
  <br/>
  <i><a href=http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html>Source: Scalable system design patterns</a></i>
</p>

Load balancers incoming client requests application servers এবং databases এর মতো computing resources এ বিতরণ করে। প্রতিটি ক্ষেত্রে, load balancer computing resource থেকে response appropriate client এ ফেরত দেয়। Load balancers এ কার্যকর:

* Unhealthy servers এ requests যাওয়া প্রতিরোধ করা
* Resources overload করা প্রতিরোধ করা
* একটি single point of failure দূর করতে সাহায্য করা

Load balancers hardware (ব্যয়বহুল) দিয়ে বা HAProxy এর মতো software দিয়ে implement করা যেতে পারে।

অতিরিক্ত সুবিধাগুলোর মধ্যে রয়েছে:

* **SSL termination** - Incoming requests decrypt করা এবং server responses encrypt করা যাতে backend servers এই potentially expensive operations সম্পাদন করতে না হয়
    * প্রতিটি server এ [X.509 certificates](https://en.wikipedia.org/wiki/X.509) ইনস্টল করার প্রয়োজন দূর করে
* **Session persistence** - Cookies issue করা এবং web apps sessions ট্র্যাক না রাখলে একটি নির্দিষ্ট client এর requests একই instance এ route করা

ব্যর্থতার বিরুদ্ধে সুরক্ষার জন্য, [active-passive](#active-passive) বা [active-active](#active-active) mode এ একাধিক load balancers সেট আপ করা সাধারণ।

Load balancers বিভিন্ন metrics এর উপর ভিত্তি করে ট্র্যাফিক route করতে পারে, যার মধ্যে রয়েছে:

* Random
* Least loaded
* Session/cookies
* [Round robin or weighted round robin](https://www.g33kinfo.com/info/round-robin-vs-weighted-round-robin-lb)
* [Layer 4](#layer-4-load-balancing)
* [Layer 7](#layer-7-load-balancing)

### Layer 4 load balancing

Layer 4 load balancers [transport layer](#communication) এ info দেখে requests কীভাবে বিতরণ করবে তা সিদ্ধান্ত নেয়। সাধারণভাবে, এটি header এ source, destination IP addresses, এবং ports জড়িত, কিন্তু packet এর contents নয়। Layer 4 load balancers upstream server থেকে এবং upstream server এ network packets forward করে, [Network Address Translation (NAT)](https://www.nginx.com/resources/glossary/layer-4-load-balancing/) সম্পাদন করে।

### Layer 7 load balancing

Layer 7 load balancers [application layer](#communication) দেখে requests কীভাবে বিতরণ করবে তা সিদ্ধান্ত নেয়। এটি header, message, এবং cookies এর contents জড়িত করতে পারে। Layer 7 load balancers network traffic terminate করে, message পড়ে, একটি load-balancing decision নেয়, তারপর selected server এ একটি connection খোলে। উদাহরণস্বরূপ, একটি layer 7 load balancer videos host করে এমন servers এ video traffic direct করতে পারে যখন আরও সংবেদনশীল user billing traffic security-hardened servers এ direct করতে পারে।

Flexibility এর খরচে, layer 4 load balancing Layer 7 এর চেয়ে কম সময় এবং computing resources প্রয়োজন, যদিও modern commodity hardware এ performance impact minimal হতে পারে।

### Horizontal scaling

Load balancers horizontal scaling এও সাহায্য করতে পারে, performance এবং availability উন্নত করে। Commodity machines ব্যবহার করে scaling out একটি single server কে আরও ব্যয়বহুল hardware এ scale up করার চেয়ে বেশি cost efficient এবং higher availability এর ফলাফল দেয়, যাকে **Vertical Scaling** বলা হয়। এটি specialized enterprise systems এর জন্য talent hire করার চেয়ে commodity hardware এ কাজ করা talent hire করাও সহজ।

#### Disadvantage(s): horizontal scaling

* Horizontal scaling complexity প্রবর্তন করে এবং servers clone করা জড়িত
    * Servers stateless হওয়া উচিত: তাদের sessions বা profile pictures এর মতো user-related data থাকা উচিত নয়
    * Sessions একটি [database](#database) (SQL, NoSQL) বা একটি persistent [cache](#cache) (Redis, Memcached) এর মতো centralized data store এ সংরক্ষণ করা যেতে পারে
* Downstream servers যেমন caches এবং databases upstream servers scale out হওয়ার সাথে আরও simultaneous connections handle করতে হবে

### Disadvantage(s): load balancer

* Load balancer একটি performance bottleneck হতে পারে যদি এটির পর্যাপ্ত resources না থাকে বা এটি properly configured না হয়।
* একটি single point of failure দূর করতে সাহায্য করার জন্য একটি load balancer প্রবর্তন করা increased complexity এর ফলাফল দেয়।
* একটি single load balancer একটি single point of failure, একাধিক load balancers configure করা আরও complexity বৃদ্ধি করে।

### Source(s) and further reading

* [NGINX architecture](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
* [HAProxy architecture guide](http://www.haproxy.org/download/1.2/doc/architecture.txt)
* [Scalability](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
* [Wikipedia](https://en.wikipedia.org/wiki/Load_balancing_(computing))
* [Layer 4 load balancing](https://www.nginx.com/resources/glossary/layer-4-load-balancing/)
* [Layer 7 load balancing](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)
* [ELB listener config](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html)

## Reverse proxy (web server)

<p align="center">
  <img src="images/n41Azff.png">
  <br/>
  <i><a href=https://upload.wikimedia.org/wikipedia/commons/6/67/Reverse_proxy_h2g2bob.svg>Source: Wikipedia</a></i>
  <br/>
</p>

একটি reverse proxy হল একটি web server যা internal services centralize করে এবং public এর জন্য unified interfaces প্রদান করে। ক্লায়েন্টদের requests একটি server এ forward করা হয় যা এটি fulfill করতে পারে reverse proxy server এর response ক্লায়েন্টে ফেরত দেওয়ার আগে।

অতিরিক্ত সুবিধাগুলোর মধ্যে রয়েছে:

* **Increased security** - Backend servers সম্পর্কে তথ্য লুকানো, IPs blacklist করা, client প্রতি connections সংখ্যা সীমাবদ্ধ করা
* **Increased scalability and flexibility** - ক্লায়েন্টরা শুধুমাত্র reverse proxy এর IP দেখে, আপনাকে servers scale করতে বা তাদের configuration পরিবর্তন করতে দেয়
* **SSL termination** - Incoming requests decrypt করা এবং server responses encrypt করা যাতে backend servers এই potentially expensive operations সম্পাদন করতে না হয়
    * প্রতিটি server এ [X.509 certificates](https://en.wikipedia.org/wiki/X.509) ইনস্টল করার প্রয়োজন দূর করে
* **Compression** - Server responses compress করা
* **Caching** - Cached requests এর জন্য response ফেরত দেওয়া
* **Static content** - Static content সরাসরি পরিবেশন করা
    * HTML/CSS/JS
    * Photos
    * Videos
    * Etc

### Load balancer vs reverse proxy

* একটি load balancer deploy করা উপযোগী যখন আপনার একাধিক servers থাকে। প্রায়শই, load balancers একই function পরিবেশন করে এমন servers এর একটি সেটে ট্র্যাফিক route করে।
* Reverse proxies শুধুমাত্র একটি web server বা application server সহেও উপযোগী হতে পারে, previous section এ বর্ণিত সুবিধাগুলো খুলে দেয়।
* NGINX এবং HAProxy এর মতো solutions layer 7 reverse proxying এবং load balancing উভয়ই সমর্থন করতে পারে।

### Disadvantage(s): reverse proxy

* একটি reverse proxy প্রবর্তন করা increased complexity এর ফলাফল দেয়।
* একটি single reverse proxy একটি single point of failure, একাধিক reverse proxies (অর্থাৎ একটি [failover](https://en.wikipedia.org/wiki/Failover)) configure করা আরও complexity বৃদ্ধি করে।

### Source(s) and further reading

* [Reverse proxy vs load balancer](https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/)
* [NGINX architecture](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
* [HAProxy architecture guide](http://www.haproxy.org/download/1.2/doc/architecture.txt)
* [Wikipedia](https://en.wikipedia.org/wiki/Reverse_proxy)

## Application layer

<p align="center">
  <img src="images/yB5SYwm.png">
  <br/>
  <i><a href=http://lethain.com/introduction-to-architecting-systems-for-scale/#platform_layer>Source: Intro to architecting systems for scale</a></i>
</p>

Web layer কে application layer (platform layer নামেও পরিচিত) থেকে আলাদা করা আপনাকে উভয় layer independently scale এবং configure করতে দেয়। একটি নতুন API যোগ করার ফলে necessarily additional web servers যোগ না করে application servers যোগ করা হয়। **Single responsibility principle** ছোট এবং স্বায়ত্তশাসিত services এর পক্ষে যা একসাথে কাজ করে। ছোট teams ছোট services সহ আরও aggressively plan করতে পারে দ্রুত বৃদ্ধির জন্য।

Application layer এ workers [asynchronism](#asynchronism) সক্ষম করতেও সাহায্য করে।

### Microservices

এই আলোচনার সাথে সম্পর্কিত [microservices](https://en.wikipedia.org/wiki/Microservices), যা independently deployable, ছোট, modular services এর একটি suite হিসাবে বর্ণনা করা যেতে পারে। প্রতিটি service একটি unique process চালায় এবং একটি business goal পরিবেশন করার জন্য একটি well-defined, lightweight mechanism এর মাধ্যমে যোগাযোগ করে। <sup><a href=https://smartbear.com/learn/api-design/what-are-microservices>1</a></sup>

Pinterest, উদাহরণস্বরূপ, নিম্নলিখিত microservices থাকতে পারে: user profile, follower, feed, search, photo upload, ইত্যাদি।

### Service Discovery

[Consul](https://www.consul.io/docs/index.html), [Etcd](https://coreos.com/etcd/docs/latest), এবং [Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper) এর মতো systems registered names, addresses, এবং ports ট্র্যাক করে services একে অপরকে খুঁজে পেতে সাহায্য করতে পারে। [Health checks](https://www.consul.io/intro/getting-started/checks.html) service integrity যাচাই করতে সাহায্য করে এবং প্রায়শই একটি [HTTP](#hypertext-transfer-protocol-http) endpoint ব্যবহার করে করা হয়। Consul এবং Etcd উভয়েরই একটি built-in [key-value store](#key-value-store) রয়েছে যা config values এবং অন্যান্য shared data সংরক্ষণের জন্য উপযোগী হতে পারে।

### Disadvantage(s): application layer

* Loosely coupled services সহ একটি application layer যোগ করা architectural, operations, এবং process দৃষ্টিকোণ থেকে একটি monolithic system থেকে একটি ভিন্ন approach প্রয়োজন।
* Microservices deployments এবং operations এর ক্ষেত্রে complexity যোগ করতে পারে।

### Source(s) and further reading

* [Intro to architecting systems for scale](http://lethain.com/introduction-to-architecting-systems-for-scale)
* [Crack the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)
* [Service oriented architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture)
* [Introduction to Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper)
* [Here's what you need to know about building microservices](https://cloudncode.wordpress.com/2016/07/22/msa-getting-started/)

## Database

<p align="center">
  <img src="images/Xkm5CXz.png">
  <br/>
  <i><a href=https://www.youtube.com/watch?v=kKjm4ehYiMs>Source: Scaling up to your first 10 million users</a></i>
</p>

### Relational database management system (RDBMS)

SQL এর মতো একটি relational database হল tables এ সংগঠিত data items এর একটি collection।

**ACID** হল relational database [transactions](https://en.wikipedia.org/wiki/Database_transaction) এর বৈশিষ্ট্যগুলোর একটি সেট।

* **Atomicity** - প্রতিটি transaction সব বা কিছুই নয়
* **Consistency** - যেকোনো transaction ডাটাবেসকে একটি valid state থেকে অন্য একটি valid state এ নিয়ে যাবে
* **Isolation** - Transactions concurrently execute করা serial এ execute করার মতো একই ফলাফল দেয়
* **Durability** - একবার একটি transaction commit করা হলে, এটি তাই থাকবে

একটি relational database scale করার অনেক কৌশল রয়েছে: **master-slave replication**, **master-master replication**, **federation**, **sharding**, **denormalization**, এবং **SQL tuning**।

#### Master-slave replication

Master reads এবং writes পরিবেশন করে, এক বা একাধিক slaves এ writes replicate করে, যা শুধুমাত্র reads পরিবেশন করে। Slaves একটি tree-like fashion এ additional slaves এ replicate করতে পারে। যদি master offline হয়ে যায়, সিস্টেম একটি slave কে master এ promote করা বা একটি নতুন master provision করা পর্যন্ত read-only mode এ কাজ চালিয়ে যেতে পারে।

<p align="center">
  <img src="images/C9ioGtn.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source: Scalability, availability, stability, patterns</a></i>
</p>

##### Disadvantage(s): master-slave replication

* একটি slave কে master এ promote করার জন্য অতিরিক্ত logic প্রয়োজন।
* **master-slave এবং master-master উভয়ের** সাথে সম্পর্কিত পয়েন্টগুলোর জন্য [Disadvantage(s): replication](#disadvantages-replication) দেখুন।

#### Master-master replication

উভয় master reads এবং writes পরিবেশন করে এবং writes এ একে অপরের সাথে coordinate করে। যদি যেকোনো master down হয়ে যায়, সিস্টেম reads এবং writes উভয়ের সাথে কাজ চালিয়ে যেতে পারে।

<p align="center">
  <img src="images/krAHLGg.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source: Scalability, availability, stability, patterns</a></i>
</p>

##### Disadvantage(s): master-master replication

* আপনার একটি load balancer প্রয়োজন হবে বা আপনার application logic পরিবর্তন করতে হবে কোথায় write করতে হবে তা নির্ধারণ করতে।
* বেশিরভাগ master-master systems হয় loosely consistent (ACID লঙ্ঘন করে) বা synchronization এর কারণে increased write latency আছে।
* আরও write nodes যোগ করা হলে এবং latency বৃদ্ধি পেলে conflict resolution আরও বেশি খেলায় আসে।
* **master-slave এবং master-master উভয়ের** সাথে সম্পর্কিত পয়েন্টগুলোর জন্য [Disadvantage(s): replication](#disadvantages-replication) দেখুন।

##### Disadvantage(s): replication

* Master অন্য nodes এ replicate হওয়ার আগে ব্যর্থ হলে নতুনভাবে লেখা ডেটা হারানোর সম্ভাবনা রয়েছে।
* Writes read replicas এ replay করা হয়। যদি অনেক writes থাকে, read replicas writes replay করতে bogged down হতে পারে এবং অনেক reads করতে পারে না।
* যত বেশি read slaves, তত বেশি replicate করতে হবে, যা greater replication lag এর দিকে নিয়ে যায়।
* কিছু systems এ, master এ লেখা parallel এ write করতে multiple threads spawn করতে পারে, যেখানে read replicas শুধুমাত্র একটি single thread দিয়ে sequentially write সমর্থন করে।
* Replication আরও hardware এবং অতিরিক্ত complexity যোগ করে।

##### Source(s) and further reading: replication

* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)
* [Multi-master replication](https://en.wikipedia.org/wiki/Multi-master_replication)

#### Federation

<p align="center">
  <img src="images/U3qV33e.png">
  <br/>
  <i><a href=https://www.youtube.com/watch?v=kKjm4ehYiMs>Source: Scaling up to your first 10 million users</a></i>
</p>

Federation (বা functional partitioning) function দ্বারা databases ভাগ করে। উদাহরণস্বরূপ, একটি single, monolithic database এর পরিবর্তে, আপনার তিনটি databases থাকতে পারে: **forums**, **users**, এবং **products**, ফলে প্রতিটি database এ কম read এবং write traffic এবং তাই কম replication lag। ছোট databases আরও ডেটা memory এ fit করতে পারে, যা turn করে আরও cache hits এর ফলাফল দেয় improved cache locality এর কারণে। কোনো single central master serializing writes ছাড়াই আপনি parallel এ write করতে পারেন, throughput বৃদ্ধি করে।

##### Disadvantage(s): federation

* Federation কার্যকর নয় যদি আপনার schema বিশাল functions বা tables প্রয়োজন।
* আপনার application logic আপডেট করতে হবে কোন database read এবং write করতে হবে তা নির্ধারণ করতে।
* [server link](http://stackoverflow.com/questions/5145637/querying-data-by-joining-two-tables-in-two-database-on-different-servers) সহ দুটি database থেকে ডেটা join করা আরও জটিল।
* Federation আরও hardware এবং অতিরিক্ত complexity যোগ করে।

##### Source(s) and further reading: federation

* [Scaling up to your first 10 million users](https://www.youtube.com/watch?v=kKjm4ehYiMs)

#### Sharding

<p align="center">
  <img src="images/wU8x5Id.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source: Scalability, availability, stability, patterns</a></i>
</p>

Sharding বিভিন্ন databases জুড়ে ডেটা বিতরণ করে যাতে প্রতিটি database শুধুমাত্র ডেটার একটি subset manage করতে পারে। একটি users database উদাহরণ হিসাবে নেওয়া, users সংখ্যা বৃদ্ধি পেলে, cluster এ আরও shards যোগ করা হয়।

[Federation](#federation) এর সুবিধাগুলোর মতো, sharding কম read এবং write traffic, কম replication, এবং আরও cache hits এর ফলাফল দেয়। Index sizeও হ্রাস পায়, যা সাধারণভাবে faster queries সহ performance উন্নত করে। যদি একটি shard down হয়ে যায়, অন্য shards এখনও operational, যদিও আপনি data loss এড়াতে কিছু form of replication যোগ করতে চাইবেন। Federation এর মতো, কোনো single central master serializing writes নেই, আপনাকে increased throughput সহ parallel এ write করতে দেয়।

Users এর একটি table shard করার সাধারণ উপায় হল user এর last name initial বা user এর geographic location এর মাধ্যমে।

##### Disadvantage(s): sharding

* আপনার application logic shards এর সাথে কাজ করার জন্য আপডেট করতে হবে, যা complex SQL queries এর ফলাফল দিতে পারে।
* একটি shard এ data distribution lopsided হতে পারে। উদাহরণস্বরূপ, একটি shard এ power users এর একটি set অন্য shards এর তুলনায় সেই shard এ increased load এর ফলাফল দিতে পারে。
    * Rebalancing অতিরিক্ত complexity যোগ করে। [consistent hashing](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html) এর উপর ভিত্তি করে একটি sharding function transferred data এর পরিমাণ কমাতে পারে।
* একাধিক shards থেকে ডেটা join করা আরও জটিল।
* Sharding আরও hardware এবং অতিরিক্ত complexity যোগ করে।

##### Source(s) and further reading: sharding

* [The coming of the shard](http://highscalability.com/blog/2009/8/6/an-unorthodox-approach-to-database-design-the-coming-of-the.html)
* [Shard database architecture](https://en.wikipedia.org/wiki/Shard_(database_architecture))
* [Consistent hashing](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html)

#### Denormalization

Denormalization কিছু write performance এর ব্যয়ে read performance উন্নত করার চেষ্টা করে। ডেটার redundant copies multiple tables এ লেখা হয় expensive joins এড়াতে। [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL) এবং Oracle এর মতো কিছু RDBMS [materialized views](https://en.wikipedia.org/wiki/Materialized_view) সমর্থন করে যা redundant information সংরক্ষণ এবং redundant copies consistent রাখার কাজ handle করে।

একবার ডেটা [federation](#federation) এবং [sharding](#sharding) এর মতো কৌশল দিয়ে distributed হয়ে গেলে, data centers জুড়ে joins manage করা আরও complexity বৃদ্ধি করে। Denormalization এমন complex joins এর প্রয়োজন circumventing করতে পারে।

অধিকাংশ systems এ, reads writes এর চেয়ে 100:1 বা এমনকি 1000:1 বেশি হতে পারে। একটি complex database join এর ফলে read খুব expensive হতে পারে, disk operations এ উল্লেখযোগ্য পরিমাণ সময় ব্যয় করে।

##### Disadvantage(s): denormalization

* ডেটা duplicate হয়।
* Constraints redundant copies of information sync থাকতে সাহায্য করতে পারে, যা database design এর complexity বৃদ্ধি করে।
* একটি denormalized database heavy write load এর অধীনে তার normalized counterpart এর চেয়ে খারাপ perform করতে পারে।

###### Source(s) and further reading: denormalization

* [Denormalization](https://en.wikipedia.org/wiki/Denormalization)

#### SQL tuning

SQL tuning একটি বিস্তৃত বিষয় এবং অনেক [books](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=sql+tuning) reference হিসাবে লেখা হয়েছে।

**Benchmark** এবং **profile** করা গুরুত্বপূর্ণ simulate এবং bottlenecks uncover করতে।

* **Benchmark** - [ab](http://httpd.apache.org/docs/2.2/programs/ab.html) এর মতো tools দিয়ে high-load situations simulate করা।
* **Profile** - [slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) এর মতো tools enable করা performance issues track করতে সাহায্য করে।

Benchmarking এবং profiling আপনাকে নিম্নলিখিত optimizations এর দিকে নির্দেশ করতে পারে।

##### Tighten up the schema

* MySQL fast access এর জন্য contiguous blocks এ disk এ dumps করে।
* Fixed-length fields এর জন্য `VARCHAR` এর পরিবর্তে `CHAR` ব্যবহার করুন।
    * `CHAR` effectively fast, random access অনুমোদন করে, যেখানে `VARCHAR` এর সাথে, আপনাকে পরবর্তীতে যাওয়ার আগে একটি string এর শেষ খুঁজে বের করতে হবে।
* Blog posts এর মতো large blocks of text এর জন্য `TEXT` ব্যবহার করুন। `TEXT` boolean searches অনুমোদন করে। একটি `TEXT` field ব্যবহার করা disk এ একটি pointer সংরক্ষণের ফলাফল দেয় যা text block locate করতে ব্যবহৃত হয়।
* 2^32 বা 4 billion পর্যন্ত larger numbers এর জন্য `INT` ব্যবহার করুন।
* Currency এর জন্য `DECIMAL` ব্যবহার করুন floating point representation errors এড়াতে।
* বড় `BLOBS` সংরক্ষণ করা এড়িয়ে চলুন, object পাওয়ার স্থান সংরক্ষণ করুন।
* `VARCHAR(255)` হল একটি 8 bit number এ গণনা করা যায় এমন সবচেয়ে বড় সংখ্যা, প্রায়শই কিছু RDBMS এ একটি byte এর ব্যবহার maximize করে।
* যেখানে প্রযোজ্য `NOT NULL` constraint সেট করুন [search performance](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search) উন্নত করতে।

##### Use good indices

* আপনি যে columns query করছেন (`SELECT`, `GROUP BY`, `ORDER BY`, `JOIN`) indices সহ দ্রুত হতে পারে।
* Indices সাধারণত self-balancing [B-tree](https://en.wikipedia.org/wiki/B-tree) হিসাবে উপস্থাপিত হয় যা ডেটা sorted রাখে এবং logarithmic time এ searches, sequential access, insertions, এবং deletions অনুমোদন করে।
* একটি index স্থাপন করা ডেটা memory এ রাখতে পারে, আরও space প্রয়োজন।
* Writes ধীর হতে পারে যেহেতু index কেও আপডেট করতে হবে।
* বড় পরিমাণে ডেটা load করার সময়, indices disable করা, ডেটা load করা, তারপর indices rebuild করা দ্রুত হতে পারে।

##### Avoid expensive joins

* Performance demands করলে [Denormalize](#denormalization) করুন।

##### Partition tables

* Hot spots একটি separate table এ রেখে একটি table ভেঙে ফেলুন memory এ রাখতে সাহায্য করতে।

##### Tune the query cache

* কিছু ক্ষেত্রে, [query cache](https://dev.mysql.com/doc/refman/5.7/en/query-cache.html) [performance issues](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/) এর দিকে নিয়ে যেতে পারে।

##### Source(s) and further reading: SQL tuning

* [Tips for optimizing MySQL queries](http://aiddroid.com/10-tips-optimizing-mysql-queries-dont-suck/)
* [Is there a good reason i see VARCHAR(255) used so often?](http://stackoverflow.com/questions/1217466/is-there-a-good-reason-i-see-varchar255-used-so-often-as-opposed-to-another-l)
* [How do null values affect performance?](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)
* [Slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)

### NoSQL

NoSQL হল **key-value store**, **document store**, **wide column store**, বা একটি **graph database** এ উপস্থাপিত data items এর একটি collection। ডেটা denormalized হয়, এবং joins সাধারণত application code এ করা হয়। বেশিরভাগ NoSQL stores সত্য ACID transactions এর অভাব রয়েছে এবং [eventual consistency](#eventual-consistency) পছন্দ করে।

**BASE** প্রায়শই NoSQL databases এর বৈশিষ্ট্যগুলো বর্ণনা করতে ব্যবহৃত হয়। [CAP Theorem](#cap-theorem) এর সাথে তুলনায়, BASE consistency এর উপর availability বেছে নেয়।

* **Basically available** - সিস্টেম availability গ্যারান্টি দেয়।
* **Soft state** - সিস্টেমের state সময়ের সাথে পরিবর্তন হতে পারে, এমনকি input ছাড়াই।
* **Eventual consistency** - সিস্টেম সময়ের একটি period এর মধ্যে consistent হয়ে উঠবে, প্রদত্ত যে সিস্টেম সেই period এর সময় input গ্রহণ করে না।

[SQL or NoSQL](#sql-or-nosql) এর মধ্যে বেছে নেওয়ার পাশাপাশি, কোন ধরনের NoSQL database আপনার use case(s) এর জন্য সবচেয়ে উপযুক্ত তা বোঝা সহায়ক। আমরা পরবর্তী সেকশনে **key-value stores**, **document stores**, **wide column stores**, এবং **graph databases** পর্যালোচনা করব।

#### Key-value store

> Abstraction: hash table

একটি key-value store সাধারণত O(1) reads এবং writes অনুমোদন করে এবং প্রায়শই memory বা SSD দ্বারা backed হয়। Data stores [lexicographic order](https://en.wikipedia.org/wiki/Lexicographical_order) এ keys maintain করতে পারে, key ranges এর efficient retrieval অনুমোদন করে। Key-value stores একটি value এর সাথে metadata সংরক্ষণ অনুমোদন করতে পারে।

Key-value stores উচ্চ performance প্রদান করে এবং প্রায়শই simple data models বা rapidly-changing data এর জন্য ব্যবহৃত হয়, যেমন একটি in-memory cache layer। যেহেতু তারা শুধুমাত্র operations এর একটি limited set অফার করে, complexity application layer এ shift হয় যদি additional operations প্রয়োজন হয়।

একটি key-value store আরও complex systems যেমন একটি document store এর ভিত্তি, এবং কিছু ক্ষেত্রে, একটি graph database।

##### Source(s) and further reading: key-value store

* [Key-value database](https://en.wikipedia.org/wiki/Key-value_database)
* [Disadvantages of key-value stores](http://stackoverflow.com/questions/4056093/what-are-the-disadvantages-of-using-a-key-value-table-over-nullable-columns-or)
* [Redis architecture](http://qnimate.com/overview-of-redis-architecture/)
* [Memcached architecture](https://adayinthelifeof.nl/2011/02/06/memcache-internals/)

#### Document store

> Abstraction: key-value store with documents stored as values

একটি document store documents (XML, JSON, binary, ইত্যাদি) এর চারপাশে কেন্দ্রীভূত, যেখানে একটি document একটি প্রদত্ত object এর জন্য সমস্ত তথ্য সংরক্ষণ করে। Document stores APIs বা একটি query language প্রদান করে document এর internal structure এর উপর ভিত্তি করে query করতে। *নোট, অনেক key-value stores একটি value এর metadata এর সাথে কাজ করার জন্য features অন্তর্ভুক্ত করে, এই দুটি storage types এর মধ্যে রেখাগুলো blur করে।*

অন্তর্নিহিত implementation এর উপর ভিত্তি করে, documents collections, tags, metadata, বা directories দ্বারা সংগঠিত হয়। যদিও documents সংগঠিত বা একসাথে grouped করা যেতে পারে, documents এর fields থাকতে পারে যা একে অপরের থেকে সম্পূর্ণভাবে আলাদা।

[MongoDB](https://www.mongodb.com/mongodb-architecture) এবং [CouchDB](https://blog.couchdb.org/2016/08/01/couchdb-2-0-architecture/) এর মতো কিছু document stores complex queries সম্পাদন করার জন্য একটি SQL-like language প্রদান করে। [DynamoDB](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf) key-values এবং documents উভয়ই সমর্থন করে।

Document stores উচ্চ flexibility প্রদান করে এবং প্রায়শই occasionally changing data এর সাথে কাজ করার জন্য ব্যবহৃত হয়।

##### Source(s) and further reading: document store

* [Document-oriented database](https://en.wikipedia.org/wiki/Document-oriented_database)
* [MongoDB architecture](https://www.mongodb.com/mongodb-architecture)
* [CouchDB architecture](https://blog.couchdb.org/2016/08/01/couchdb-2-0-architecture/)
* [Elasticsearch architecture](https://www.elastic.co/blog/found-elasticsearch-from-the-bottom-up)

#### Wide column store

<p align="center">
  <img src="images/n16iOGk.png">
  <br/>
  <i><a href=http://blog.grio.com/2015/11/sql-nosql-a-brief-history.html>Source: SQL & NoSQL, a brief history</a></i>
</p>

> Abstraction: nested map `ColumnFamily<RowKey, Columns<ColKey, Value, Timestamp>>`

একটি wide column store এর basic unit of data হল একটি column (name/value pair)। একটি column column families (একটি SQL table এর অনুরূপ) এ grouped হতে পারে। Super column families আরও column families group করে। আপনি একটি row key দিয়ে প্রতিটি column independently access করতে পারেন, এবং একই row key সহ columns একটি row গঠন করে। প্রতিটি value versioning এবং conflict resolution এর জন্য একটি timestamp ধারণ করে।

Google [Bigtable](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf) কে প্রথম wide column store হিসাবে চালু করেছিল, যা Hadoop ecosystem এ প্রায়শই ব্যবহৃত open-source [HBase](https://www.edureka.co/blog/hbase-architecture/) এবং Facebook এর [Cassandra](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html) কে প্রভাবিত করেছিল। BigTable, HBase, এবং Cassandra এর মতো stores lexicographic order এ keys maintain করে, selective key ranges এর efficient retrieval অনুমোদন করে।

Wide column stores উচ্চ availability এবং উচ্চ scalability অফার করে। তারা প্রায়শই খুব বড় data sets এর জন্য ব্যবহৃত হয়।

##### Source(s) and further reading: wide column store

* [SQL & NoSQL, a brief history](http://blog.grio.com/2015/11/sql-nosql-a-brief-history.html)
* [Bigtable architecture](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf)
* [HBase architecture](https://www.edureka.co/blog/hbase-architecture/)
* [Cassandra architecture](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html)

#### Graph database

<p align="center">
  <img src="images/fNcl65g.png">
  <br/>
  <i><a href=https://en.wikipedia.org/wiki/File:GraphDatabase_PropertyGraph.png>Source: Graph database</a></i>
</p>

> Abstraction: graph

একটি graph database এ, প্রতিটি node একটি record এবং প্রতিটি arc দুটি node এর মধ্যে একটি relationship। Graph databases অনেক foreign keys বা many-to-many relationships সহ complex relationships উপস্থাপন করার জন্য optimized।

Graphs databases complex relationships সহ data models এর জন্য উচ্চ performance অফার করে, যেমন একটি social network। তারা তুলনামূলকভাবে নতুন এবং এখনও ব্যাপকভাবে ব্যবহৃত হয় না; development tools এবং resources খুঁজে পাওয়া আরও কঠিন হতে পারে। অনেক graphs শুধুমাত্র [REST APIs](#representational-state-transfer-rest) দিয়ে access করা যেতে পারে।

##### Source(s) and further reading: graph

* [Graph database](https://en.wikipedia.org/wiki/Graph_database)
* [Neo4j](https://neo4j.com/)
* [FlockDB](https://blog.twitter.com/2010/introducing-flockdb)

#### Source(s) and further reading: NoSQL

* [Explanation of base terminology](http://stackoverflow.com/questions/3342497/explanation-of-base-terminology)
* [NoSQL databases a survey and decision guidance](https://medium.com/baqend-blog/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.wskogqenq)
* [Scalability](https://web.archive.org/web/20220602114024/https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database)
* [Introduction to NoSQL](https://www.youtube.com/watch?v=qI_g07C_Q5I)
* [NoSQL patterns](http://horicky.blogspot.com/2009/11/nosql-patterns.html)

### SQL or NoSQL

<p align="center">
  <img src="images/wXGqG5f.png">
  <br/>
  <i><a href=https://www.infoq.com/articles/Transition-RDBMS-NoSQL/>Source: Transitioning from RDBMS to NoSQL</a></i>
</p>

**SQL** এর জন্য কারণ:

* Structured data
* Strict schema
* Relational data
* Complex joins এর প্রয়োজন
* Transactions
* Scaling এর জন্য clear patterns
* আরও প্রতিষ্ঠিত: developers, community, code, tools, ইত্যাদি
* Index দ্বারা lookups খুব দ্রুত

**NoSQL** এর জন্য কারণ:

* Semi-structured data
* Dynamic বা flexible schema
* Non-relational data
* Complex joins এর প্রয়োজন নেই
* অনেক TB (বা PB) ডেটা সংরক্ষণ করা
* খুব data intensive workload
* IOPS এর জন্য খুব উচ্চ throughput

NoSQL এর জন্য ভালোভাবে উপযুক্ত sample data:

* Clickstream এবং log data এর rapid ingest
* Leaderboard বা scoring data
* Temporary data, যেমন একটি shopping cart
* Frequently accessed ('hot') tables
* Metadata/lookup tables

##### Source(s) and further reading: SQL or NoSQL

* [Scaling up to your first 10 million users](https://www.youtube.com/watch?v=kKjm4ehYiMs)
* [SQL vs NoSQL differences](https://www.sitepoint.com/sql-vs-nosql-differences/)

## Cache

<p align="center">
  <img src="images/Q6z24La.png">
  <br/>
  <i><a href=http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html>Source: Scalable system design patterns</a></i>
</p>

Caching page load times উন্নত করে এবং আপনার servers এবং databases এর load কমাতে পারে। এই model এ, dispatcher প্রথমে lookup করবে request আগে করা হয়েছে কিনা এবং previous result খুঁজে ফেরত দেওয়ার চেষ্টা করবে, actual execution সংরক্ষণ করার জন্য।

Databases প্রায়শই তার partitions জুড়ে reads এবং writes এর uniform distribution থেকে উপকৃত হয়। Popular items distribution skew করতে পারে, bottlenecks সৃষ্টি করে। একটি database এর সামনে একটি cache রাখা uneven loads এবং traffic spikes absorb করতে সাহায্য করতে পারে।

### Client caching

Caches client side (OS বা browser), [server side](#reverse-proxy-web-server), বা একটি distinct cache layer এ অবস্থিত হতে পারে।

### CDN caching

[CDNs](#content-delivery-network) cache এর একটি ধরন হিসাবে বিবেচিত হয়।

### Web server caching

[Reverse proxies](#reverse-proxy-web-server) এবং [Varnish](https://www.varnish-cache.org/) এর মতো caches static এবং dynamic content সরাসরি পরিবেশন করতে পারে। Web servers requests cache করতে পারে, application servers এর সাথে যোগাযোগ না করে responses ফেরত দেয়।

### Database caching

আপনার database সাধারণত default configuration এ কিছু level of caching অন্তর্ভুক্ত করে, একটি generic use case এর জন্য optimized। Specific usage patterns এর জন্য এই settings tweaking performance আরও boost করতে পারে।

### Application caching

Memcached এবং Redis এর মতো In-memory caches আপনার application এবং আপনার data storage এর মধ্যে key-value stores। যেহেতু ডেটা RAM এ রাখা হয়, এটি typical databases এর চেয়ে অনেক দ্রুত যেখানে ডেটা disk এ সংরক্ষিত হয়। RAM disk এর চেয়ে আরও সীমিত, তাই [cache invalidation](https://en.wikipedia.org/wiki/Cache_algorithms) algorithms যেমন [least recently used (LRU)](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU)) 'cold' entries invalidate করতে এবং 'hot' data RAM এ রাখতে সাহায্য করতে পারে।

Redis এর নিম্নলিখিত অতিরিক্ত features রয়েছে:

* Persistence option
* Built-in data structures যেমন sorted sets এবং lists

আপনি cache করতে পারেন এমন multiple levels রয়েছে যা দুটি সাধারণ categories এ পড়ে: **database queries** এবং **objects**:

* Row level
* Query-level
* Fully-formed serializable objects
* Fully-rendered HTML

সাধারণভাবে, আপনার file-based caching এড়ানো উচিত, কারণ এটি cloning এবং auto-scaling আরও কঠিন করে তোলে।

### Caching at the database query level

যখনই আপনি database query করেন, query কে একটি key হিসাবে hash করুন এবং result cache এ সংরক্ষণ করুন। এই approach expiration issues থেকে ভোগে:

* Complex queries সহ একটি cached result delete করা কঠিন
* যদি একটি ডেটা পরিবর্তিত হয় যেমন একটি table cell, আপনাকে সমস্ত cached queries delete করতে হবে যা পরিবর্তিত cell অন্তর্ভুক্ত করতে পারে

### Caching at the object level

আপনার ডেটাকে একটি object হিসাবে দেখুন, আপনার application code এর সাথে যা করেন তার অনুরূপ। আপনার application database থেকে dataset assemble করুন একটি class instance বা একটি data structure(s) এ:

* যদি এর underlying data পরিবর্তিত হয় cache থেকে object remove করুন
* Asynchronous processing অনুমোদন করে: workers latest cached object consume করে objects assemble করে

কী cache করতে হবে তার পরামর্শ:

* User sessions
* Fully rendered web pages
* Activity streams
* User graph data

### When to update the cache

যেহেতু আপনি cache এ একটি সীমিত পরিমাণ ডেটা সংরক্ষণ করতে পারেন, আপনাকে নির্ধারণ করতে হবে কোন cache update strategy আপনার use case এর জন্য সবচেয়ে ভালো কাজ করে।

#### Cache-aside

<p align="center">
  <img src="images/ONjORqk.png">
  <br/>
  <i><a href=http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast>Source: From cache to in-memory data grid</a></i>
</p>

Application storage থেকে পড়া এবং লেখার জন্য দায়ী। Cache সরাসরি storage এর সাথে interact করে না। Application নিম্নলিখিত কাজগুলো করে:

* Cache এ entry খুঁজুন, একটি cache miss এর ফলাফল
* Database থেকে entry load করুন
* Cache এ entry যোগ করুন
* Entry ফেরত দিন

```python
def get_user(self, user_id):
    user = cache.get("user.{0}", user_id)
    if user is None:
        user = db.query("SELECT * FROM users WHERE user_id = {0}", user_id)
        if user is not None:
            key = "user.{0}".format(user_id)
            cache.set(key, json.dumps(user))
    return user
```

[Memcached](https://memcached.org/) সাধারণত এইভাবে ব্যবহৃত হয়।

Cache এ যোগ করা ডেটার পরবর্তী reads দ্রুত। Cache-aside কে lazy loading হিসাবেও উল্লেখ করা হয়। শুধুমাত্র requested data cache করা হয়, যা requested নয় এমন ডেটা দিয়ে cache fill করা এড়ায়।

##### Disadvantage(s): cache-aside

* প্রতিটি cache miss তিনটি trips এর ফলাফল দেয়, যা একটি লক্ষণীয় delay সৃষ্টি করতে পারে।
* Database এ আপডেট করা হলে ডেটা stale হতে পারে। এই সমস্যাটি একটি time-to-live (TTL) সেট করে প্রশমিত করা হয় যা cache entry এর একটি update বাধ্য করে, বা write-through ব্যবহার করে।
* যখন একটি node ব্যর্থ হয়, এটি একটি নতুন, empty node দ্বারা প্রতিস্থাপিত হয়, latency বৃদ্ধি করে।

#### Write-through

<p align="center">
  <img src="images/0vBc0hN.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source: Scalability, availability, stability, patterns</a></i>
</p>

Application cache কে main data store হিসাবে ব্যবহার করে, এতে ডেটা পড়ে এবং লেখে, যখন cache database থেকে পড়া এবং লেখার জন্য দায়ী:

* Application cache এ entry যোগ/আপডেট করে
* Cache synchronously data store এ entry লেখে
* ফেরত দিন

Application code:

```python
set_user(12345, {"foo":"bar"})
```

Cache code:

```python
def set_user(user_id, values):
    user = db.query("UPDATE Users WHERE id = {0}", user_id, values)
    cache.set(user_id, user)
```

Write-through write operation এর কারণে একটি ধীর overall operation, কিন্তু just written data এর পরবর্তী reads দ্রুত। Users সাধারণত data পড়ার চেয়ে data আপডেট করার সময় latency এর জন্য আরও সহনশীল। Cache এ ডেটা stale নয়।

##### Disadvantage(s): write through

* ব্যর্থতা বা scaling এর কারণে একটি নতুন node তৈরি হলে, database এ entry আপডেট না হওয়া পর্যন্ত নতুন node entries cache করবে না। Write through এর সাথে conjunction এ cache-aside এই সমস্যাটি প্রশমিত করতে পারে।
* লেখা বেশিরভাগ ডেটা কখনই পড়া নাও হতে পারে, যা একটি TTL দিয়ে minimize করা যেতে পারে।

#### Write-behind (write-back)

<p align="center">
  <img src="images/rgSrvjG.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source: Scalability, availability, stability, patterns</a></i>
</p>

Write-behind এ, application নিম্নলিখিত কাজগুলো করে:

* Cache এ entry যোগ/আপডেট করুন
* Asynchronously data store এ entry লিখুন, write performance উন্নত করে

##### Disadvantage(s): write-behind

* Cache data store এ hit করার আগে down হয়ে গেলে data loss হতে পারে।
* Cache-aside বা write-through implement করার চেয়ে write-behind implement করা আরও complex।

#### Refresh-ahead

<p align="center">
  <img src="images/kxtjqgE.png">
  <br/>
  <i><a href=http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast>Source: From cache to in-memory data grid</a></i>
</p>

আপনি cache configure করতে পারেন যাতে এটি expiration এর আগে স্বয়ংক্রিয়ভাবে কোনো recently accessed cache entry refresh করে।

Refresh-ahead read-through এর তুলনায় reduced latency এর ফলাফল দিতে পারে যদি cache ভবিষ্যতে কোন items প্রয়োজন হতে পারে তা accurately predict করতে পারে।

##### Disadvantage(s): refresh-ahead

* ভবিষ্যতে কোন items প্রয়োজন হতে পারে তা accurately predict না করা refresh-ahead ছাড়া reduced performance এর ফলাফল দিতে পারে।

### Disadvantage(s): cache

* Database এর মতো source of truth এবং caches এর মধ্যে consistency maintain করার প্রয়োজন [cache invalidation](https://en.wikipedia.org/wiki/Cache_algorithms) এর মাধ্যমে।
* Cache invalidation একটি কঠিন সমস্যা, cache কখন আপডেট করতে হবে তার সাথে সম্পর্কিত অতিরিক্ত complexity রয়েছে।
* Redis বা memcached যোগ করার মতো application changes করার প্রয়োজন।

### Source(s) and further reading

* [From cache to in-memory data grid](http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast)
* [Scalable system design patterns](http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html)
* [Introduction to architecting systems for scale](http://lethain.com/introduction-to-architecting-systems-for-scale/)
* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)
* [Scalability](https://web.archive.org/web/20230126233752/https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache)
* [AWS ElastiCache strategies](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Strategies.html)
* [Wikipedia](https://en.wikipedia.org/wiki/Cache_(computing))

## Asynchronism

<p align="center">
  <img src="images/54GYsSx.png">
  <br/>
  <i><a href=http://lethain.com/introduction-to-architecting-systems-for-scale/#platform_layer>Source: Intro to architecting systems for scale</a></i>
</p>

Asynchronous workflows expensive operations এর জন্য request times কমাতে সাহায্য করে যা অন্যথায় in-line সম্পাদিত হবে। তারা advance এ time-consuming work করে সাহায্য করতে পারে, যেমন ডেটার periodic aggregation।

### Message queues

Message queues messages receive, hold, এবং deliver করে। যদি একটি operation inline সম্পাদন করার জন্য খুব ধীর হয়, আপনি নিম্নলিখিত workflow সহ একটি message queue ব্যবহার করতে পারেন:

* একটি application queue এ একটি job publish করে, তারপর user কে job status জানায়
* একটি worker queue থেকে job নেয়, এটি process করে, তারপর job complete signal দেয়

User blocked হয় না এবং job background এ process হয়। এই সময়ের মধ্যে, client optionally একটি ছোট পরিমাণ processing করতে পারে যাতে মনে হয় task complete হয়েছে। উদাহরণস্বরূপ, যদি একটি tweet posting করা হয়, tweet আপনার timeline এ instantly posted হতে পারে, কিন্তু আপনার tweet আসলে আপনার সমস্ত followers এ deliver হতে কিছু সময় লাগতে পারে।

**[Redis](https://redis.io/)** একটি simple message broker হিসাবে উপযোগী কিন্তু messages হারিয়ে যেতে পারে।

**[RabbitMQ](https://www.rabbitmq.com/)** জনপ্রিয় কিন্তু আপনাকে 'AMQP' protocol এ adapt করতে হবে এবং আপনার নিজের nodes manage করতে হবে।

**[Amazon SQS](https://aws.amazon.com/sqs/)** hosted কিন্তু high latency থাকতে পারে এবং messages দুবার deliver হওয়ার সম্ভাবনা রয়েছে।

### Task queues

Tasks queues tasks এবং তাদের related data receive করে, সেগুলো run করে, তারপর তাদের results deliver করে। তারা scheduling সমর্থন করতে পারে এবং background এ computationally-intensive jobs run করতে ব্যবহৃত হতে পারে।

**[Celery](https://docs.celeryproject.org/en/stable/)** scheduling এর জন্য support রয়েছে এবং প্রাথমিকভাবে python support রয়েছে।

### Back pressure

যদি queues উল্লেখযোগ্যভাবে বৃদ্ধি পেতে শুরু করে, queue size memory এর চেয়ে বড় হতে পারে, cache misses, disk reads, এবং এমনকি ধীর performance এর ফলাফল দেয়। [Back pressure](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html) queue size সীমাবদ্ধ করে সাহায্য করতে পারে, thereby queue এ already থাকা jobs এর জন্য একটি high throughput rate এবং good response times maintain করে। একবার queue fill হয়ে গেলে, clients একটি server busy বা HTTP 503 status code পায় পরে আবার চেষ্টা করতে। Clients পরে একটি সময়ে request retry করতে পারে, সম্ভবত [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) সহ।

### Disadvantage(s): asynchronism

* Inexpensive calculations এবং realtime workflows এর মতো use cases synchronous operations এর জন্য আরও উপযুক্ত হতে পারে, কারণ queues প্রবর্তন delays এবং complexity যোগ করতে পারে।

### Source(s) and further reading

* [It's all a numbers game](https://www.youtube.com/watch?v=1KRYH75wgy4)
* [Applying back pressure when overloaded](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html)
* [Little's law](https://en.wikipedia.org/wiki/Little%27s_law)
* [What is the difference between a message queue and a task queue?](https://www.quora.com/What-is-the-difference-between-a-message-queue-and-a-task-queue-Why-would-a-task-queue-require-a-message-broker-like-RabbitMQ-Redis-Celery-or-IronMQ-to-function)

## Communication

<p align="center">
  <img src="images/5KeocQs.jpg">
  <br/>
  <i><a href=http://www.escotal.com/osilayer.html>Source: OSI 7 layer model</a></i>
</p>

### Hypertext transfer protocol (HTTP)

HTTP হল client এবং server এর মধ্যে ডেটা encoding এবং transporting করার একটি method। এটি একটি request/response protocol: clients requests issue করে এবং servers request সম্পর্কে relevant content এবং completion status info সহ responses issue করে। HTTP self-contained, requests এবং responses অনেক intermediate routers এবং servers এর মাধ্যমে flow করতে অনুমোদন করে যা load balancing, caching, encryption, এবং compression সম্পাদন করে।

একটি basic HTTP request একটি verb (method) এবং একটি resource (endpoint) নিয়ে গঠিত। নিচে common HTTP verbs:

| Verb | Description | Idempotent* | Safe | Cacheable |
|---|---|---|---|---|
| GET | Reads a resource | Yes | Yes | Yes |
| POST | Creates a resource or trigger a process that handles data | No | No | Yes if response contains freshness info |
| PUT | Creates or replace a resource | Yes | No | No |
| PATCH | Partially updates a resource | No | No | Yes if response contains freshness info |
| DELETE | Deletes a resource | Yes | No | No |

*Can be called many times without different outcomes.

HTTP হল একটি application layer protocol যা **TCP** এবং **UDP** এর মতো lower-level protocols এর উপর নির্ভর করে।

#### Source(s) and further reading: HTTP

* [What is HTTP?](https://www.nginx.com/resources/glossary/http/)
* [Difference between HTTP and TCP](https://www.quora.com/What-is-the-difference-between-HTTP-protocol-and-TCP-protocol)
* [Difference between PUT and PATCH](https://laracasts.com/discuss/channels/general-discussion/whats-the-differences-between-put-and-patch?page=1)

### Transmission control protocol (TCP)

<p align="center">
  <img src="images/JdAsdvG.jpg">
  <br/>
  <i><a href=http://www.wildbunny.co.uk/blog/2012/10/09/how-to-make-a-multi-player-game-part-1/>Source: How to make a multiplayer game</a></i>
</p>

TCP হল একটি [IP network](https://en.wikipedia.org/wiki/Internet_Protocol) এর উপর একটি connection-oriented protocol। Connection একটি [handshake](https://en.wikipedia.org/wiki/Handshaking) ব্যবহার করে established এবং terminated হয়। পাঠানো সমস্ত packets original order এ এবং corruption ছাড়াই destination এ পৌঁছানোর গ্যারান্টি দেওয়া হয়:

* প্রতিটি packet এর জন্য sequence numbers এবং [checksum fields](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Checksum_computation)
* [Acknowledgement](https://en.wikipedia.org/wiki/Acknowledgement_(data_networks)) packets এবং automatic retransmission

যদি sender একটি correct response গ্রহণ না করে, এটি packets resend করবে। যদি multiple timeouts থাকে, connection dropped হয়। TCP [flow control](https://en.wikipedia.org/wiki/Flow_control_(data)) এবং [congestion control](https://en.wikipedia.org/wiki/Network_congestion#Congestion_control) implement করে। এই guarantees delays সৃষ্টি করে এবং সাধারণত UDP এর তুলনায় less efficient transmission এর ফলাফল দেয়।

High throughput নিশ্চিত করতে, web servers অনেক TCP connections open রাখতে পারে, high memory usage এর ফলাফল দেয়। Web server threads এবং একটি [memcached](https://memcached.org/) server এর মধ্যে অনেক open connections থাকা ব্যয়বহুল হতে পারে। [Connection pooling](https://en.wikipedia.org/wiki/Connection_pool) applicable যেখানে UDP এ switching এর পাশাপাশি সাহায্য করতে পারে।

TCP উচ্চ reliability প্রয়োজন এমন applications এর জন্য উপযোগী কিন্তু কম time critical। কিছু উদাহরণের মধ্যে রয়েছে web servers, database info, SMTP, FTP, এবং SSH।

UDP এর উপর TCP ব্যবহার করুন যখন:

* আপনার সমস্ত ডেটা intact arrive করার প্রয়োজন
* আপনি automatically network throughput এর একটি best estimate use করতে চান

### User datagram protocol (UDP)

<p align="center">
  <img src="images/yzDrJtA.jpg">
  <br/>
  <i><a href=http://www.wildbunny.co.uk/blog/2012/10/09/how-to-make-a-multi-player-game-part-1/>Source: How to make a multiplayer game</a></i>
</p>

UDP হল connectionless। Datagrams (packets এর অনুরূপ) শুধুমাত্র datagram level এ guaranteed। Datagrams তাদের destination এ out of order বা মোটেও পৌঁছাতে পারে না। UDP congestion control সমর্থন করে না। TCP support যে guarantees ছাড়া, UDP সাধারণত আরও efficient।

UDP broadcast করতে পারে, subnet এর সমস্ত devices এ datagrams পাঠায়। এটি [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) এর সাথে উপযোগী কারণ client এখনও একটি IP address গ্রহণ করেনি, thus IP address ছাড়া TCP stream করার একটি way প্রতিরোধ করে।

UDP কম reliable কিন্তু VoIP, video chat, streaming, এবং realtime multiplayer games এর মতো real time use cases এ ভালো কাজ করে।

TCP এর উপর UDP ব্যবহার করুন যখন:

* আপনার lowest latency প্রয়োজন
* Late data data loss এর চেয়ে খারাপ
* আপনি আপনার নিজের error correction implement করতে চান

#### Source(s) and further reading: TCP and UDP

* [Networking for game programming](http://gafferongames.com/networking-for-game-programmers/udp-vs-tcp/)
* [Key differences between TCP and UDP protocols](http://www.cyberciti.biz/faq/key-differences-between-tcp-and-udp-protocols/)
* [Difference between TCP and UDP](http://stackoverflow.com/questions/5970383/difference-between-tcp-and-udp)
* [Transmission control protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)
* [User datagram protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)
* [Scaling memcache at Facebook](http://www.cs.bu.edu/~jappavoo/jappavoo.github.com/451/papers/memcache-fb.pdf)

### Remote procedure call (RPC)

<p align="center">
  <img src="images/iF4Mkb5.png">
  <br/>
  <i><a href=http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview>Source: Crack the system design interview</a></i>
</p>

একটি RPC এ, একটি client একটি different address space এ, সাধারণত একটি remote server এ একটি procedure execute করতে বাধ্য করে। Procedure একটি local procedure call এর মতো coded হয়, client program থেকে server এর সাথে কীভাবে যোগাযোগ করতে হবে তার details abstracting away করে। Remote calls সাধারণত local calls এর চেয়ে ধীর এবং কম reliable তাই RPC calls local calls থেকে আলাদা করা সহায়ক। জনপ্রিয় RPC frameworks এর মধ্যে রয়েছে [Protobuf](https://developers.google.com/protocol-buffers/), [Thrift](https://thrift.apache.org/), এবং [Avro](https://avro.apache.org/docs/current/)।

RPC হল একটি request-response protocol:

* **Client program** - Client stub procedure call করে। Parameters একটি local procedure call এর মতো stack এ push করা হয়।
* **Client stub procedure** - Procedure id এবং arguments একটি request message এ marshals (packs) করে।
* **Client communication module** - OS client থেকে server এ message পাঠায়।
* **Server communication module** - OS incoming packets server stub procedure এ pass করে।
* **Server stub procedure** - Results unmarshalls করে, procedure id match করে server procedure call করে এবং প্রদত্ত arguments pass করে।
* Server response reverse order এ উপরের steps পুনরাবৃত্তি করে।

Sample RPC calls:

```
GET /someoperation?data=anId

POST /anotheroperation
{
  "data":"anId";
  "anotherdata": "another value"
}
```

RPC behaviors expose করার উপর ফোকাস করে। RPCs প্রায়শই internal communications এর সাথে performance reasons এর জন্য ব্যবহৃত হয়, কারণ আপনি আপনার use cases এর সাথে ভালো fit করার জন্য native calls hand-craft করতে পারেন।

একটি native library (aka SDK) বেছে নিন যখন:

* আপনি আপনার target platform জানেন।
* আপনি কীভাবে আপনার "logic" access করা হয় তা control করতে চান।
* আপনি কীভাবে error control আপনার library থেকে ঘটে তা control করতে চান।
* Performance এবং end user experience আপনার primary concern।

**REST** অনুসরণ করে HTTP APIs public APIs এর জন্য আরও প্রায়শই ব্যবহৃত হয়।

#### Disadvantage(s): RPC

* RPC clients service implementation এর সাথে tightly coupled হয়ে যায়।
* প্রতিটি নতুন operation বা use case এর জন্য একটি নতুন API সংজ্ঞায়িত করতে হবে।
* RPC debug করা কঠিন হতে পারে।
* আপনি existing technologies out of the box leverage করতে পারবেন না। উদাহরণস্বরূপ, [Squid](http://www.squid-cache.org/) এর মতো caching servers এ [RPC calls are properly cached](https://web.archive.org/web/20170608193645/http://etherealbits.com/2012/12/debunking-the-myths-of-rpc-rest/) নিশ্চিত করতে অতিরিক্ত প্রচেষ্টা প্রয়োজন হতে পারে।

### Representational state transfer (REST)

REST হল একটি architectural style যা একটি client/server model enforce করে যেখানে client server দ্বারা পরিচালিত resources এর একটি set এ কাজ করে। Server resources এবং actions এর একটি representation প্রদান করে যা resources manipulate করতে পারে বা resources এর একটি নতুন representation পেতে পারে। সমস্ত communication stateless এবং cacheable হতে হবে।

একটি RESTful interface এর চারটি গুণ রয়েছে:

* **Identify resources (URI in HTTP)** - যেকোনো operation নির্বিশেষে একই URI ব্যবহার করুন।
* **Change with representations (Verbs in HTTP)** - verbs, headers, এবং body ব্যবহার করুন।
* **Self-descriptive error message (status response in HTTP)** - Status codes ব্যবহার করুন, wheel reinvent করবেন না।
* **[HATEOAS](http://restcookbook.com/Basics/hateoas/) (HTML interface for HTTP)** - আপনার web service একটি browser এ fully accessible হওয়া উচিত।

Sample REST calls:

```
GET /someresources/anId

PUT /someresources/anId
{"anotherdata": "another value"}
```

REST data expose করার উপর ফোকাস করে। এটি client/server এর মধ্যে coupling minimize করে এবং প্রায়শই public HTTP APIs এর জন্য ব্যবহৃত হয়। REST URIs এর মাধ্যমে resources expose করার একটি আরও generic এবং uniform method ব্যবহার করে, [representation through headers](https://github.com/for-GET/know-your-http-well/blob/master/headers.md), এবং GET, POST, PUT, DELETE, এবং PATCH এর মতো verbs এর মাধ্যমে actions। Stateless হওয়ার কারণে, REST horizontal scaling এবং partitioning এর জন্য দুর্দান্ত।

#### Disadvantage(s): REST

* REST data expose করার উপর ফোকাস করার সাথে, resources যদি naturally organized না হয় বা একটি simple hierarchy এ access না হয় তবে এটি একটি ভাল fit নাও হতে পারে। উদাহরণস্বরূপ, past hour থেকে একটি particular set of events match করে সমস্ত updated records ফেরত দেওয়া একটি path হিসাবে সহজে express করা যায় না। REST এর সাথে, এটি URI path, query parameters, এবং সম্ভবত request body এর combination দিয়ে implement করা সম্ভব।
* REST সাধারণত কয়েকটি verbs (GET, POST, PUT, DELETE, এবং PATCH) এর উপর নির্ভর করে যা কখনও কখনও আপনার use case এর সাথে fit করে না। উদাহরণস্বরূপ, expired documents archive folder এ move করা এই verbs এর মধ্যে cleanly fit নাও হতে পারে।
* Nested hierarchies সহ complicated resources fetch করা single views render করার জন্য client এবং server এর মধ্যে multiple round trips প্রয়োজন, উদাহরণস্বরূপ, একটি blog entry এর content এবং সেই entry এর comments fetch করা। Variable network conditions এ কাজ করা mobile applications এর জন্য, এই multiple roundtrips highly undesirable।
* সময়ের সাথে, আরও fields একটি API response এ যোগ করা হতে পারে এবং older clients সমস্ত নতুন data fields receive করবে, এমনকি যেগুলো তাদের প্রয়োজন নেই, ফলস্বরূপ, এটি payload size bloat করে এবং larger latencies এর দিকে নিয়ে যায়।

### RPC and REST calls comparison

| Operation | RPC | REST |
|---|---|---|
| Signup    | **POST** /signup | **POST** /persons |
| Resign    | **POST** /resign<br/>{<br/>"personid": "1234"<br/>} | **DELETE** /persons/1234 |
| Read a person | **GET** /readPerson?personid=1234 | **GET** /persons/1234 |
| Read a person's items list | **GET** /readUsersItemsList?personid=1234 | **GET** /persons/1234/items |
| Add an item to a person's items | **POST** /addItemToUsersItemsList<br/>{<br/>"personid": "1234";<br/>"itemid": "456"<br/>} | **POST** /persons/1234/items<br/>{<br/>"itemid": "456"<br/>} |
| Update an item    | **POST** /modifyItem<br/>{<br/>"itemid": "456";<br/>"key": "value"<br/>} | **PUT** /items/456<br/>{<br/>"key": "value"<br/>} |
| Delete an item | **POST** /removeItem<br/>{<br/>"itemid": "456"<br/>} | **DELETE** /items/456 |

<p align="center">
  <i><a href=https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/>Source: Do you really know why you prefer REST over RPC</a></i>
</p>

#### Source(s) and further reading: REST and RPC

* [Do you really know why you prefer REST over RPC](https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/)
* [When are RPC-ish approaches more appropriate than REST?](http://programmers.stackexchange.com/a/181186)
* [REST vs JSON-RPC](http://stackoverflow.com/questions/15056878/rest-vs-json-rpc)
* [Debunking the myths of RPC and REST](https://web.archive.org/web/20170608193645/http://etherealbits.com/2012/12/debunking-the-myths-of-rpc-rest/)
* [What are the drawbacks of using REST](https://www.quora.com/What-are-the-drawbacks-of-using-RESTful-APIs)
* [Crack the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)
* [Thrift](https://code.facebook.com/posts/1468950976659943/)
* [Why REST for internal use and not RPC](http://arstechnica.com/civis/viewtopic.php?t=1190508)

## Security

এই সেকশন কিছু updates ব্যবহার করতে পারে। [contributing](#contributing) বিবেচনা করুন!

Security একটি বিস্তৃত বিষয়। যদি আপনার যথেষ্ট experience, একটি security background না থাকে, বা আপনি এমন একটি position এর জন্য apply না করছেন যার security এর knowledge প্রয়োজন, আপনি সম্ভবত basics এর চেয়ে বেশি জানার প্রয়োজন নেই:

* Transit এবং rest এ encrypt করুন।
* [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting) এবং [SQL injection](https://en.wikipedia.org/wiki/SQL_injection) প্রতিরোধ করতে user inputs বা user এর কাছে exposed যেকোনো input parameters sanitize করুন।
* SQL injection প্রতিরোধ করতে parameterized queries ব্যবহার করুন।
* [least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) এর principle ব্যবহার করুন।

### Source(s) and further reading

* [API security checklist](https://github.com/shieldfy/API-Security-Checklist)
* [Security guide for developers](https://github.com/FallibleInc/security-guide-for-developers)
* [OWASP top ten](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet)

## Appendix

আপনাকে কখনও কখনও 'back-of-the-envelope' estimates করতে বলা হবে। উদাহরণস্বরূপ, আপনার disk থেকে 100 image thumbnails generate করতে কত সময় লাগবে বা একটি data structure কত memory নেবে তা নির্ধারণ করতে হতে পারে। **Powers of two table** এবং **Latency numbers every programmer should know** handy references।

### Powers of two table

```
Power           Exact Value         Approx Value        Bytes
---------------------------------------------------------------
7                             128
8                             256
10                           1024   1 thousand           1 KB
16                         65,536                       64 KB
20                      1,048,576   1 million            1 MB
30                  1,073,741,824   1 billion            1 GB
32                  4,294,967,296                        4 GB
40              1,099,511,627,776   1 trillion           1 TB
```

#### Source(s) and further reading

* [Powers of two](https://en.wikipedia.org/wiki/Power_of_two)

### Latency numbers every programmer should know

```
Latency Comparison Numbers
--------------------------
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                      14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                      20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy            10,000   ns       10 us
Send 1 KB bytes over 1 Gbps network     10,000   ns       10 us
Read 4 KB randomly from SSD*           150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
HDD seek                            10,000,000   ns   10,000 us   10 ms  20x datacenter roundtrip
Read 1 MB sequentially from 1 Gbps  10,000,000   ns   10,000 us   10 ms  40x memory, 10X SSD
Read 1 MB sequentially from HDD     30,000,000   ns   30,000 us   30 ms 120x memory, 30X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms

Notes
-----
1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns
```

উপরের numbers এর উপর ভিত্তি করে handy metrics:

* HDD থেকে sequentially পড়ুন 30 MB/s এ
* 1 Gbps Ethernet থেকে sequentially পড়ুন 100 MB/s এ
* SSD থেকে sequentially পড়ুন 1 GB/s এ
* Main memory থেকে sequentially পড়ুন 4 GB/s এ
* প্রতি সেকেন্ডে 6-7 world-wide round trips
* একটি data center এর মধ্যে প্রতি সেকেন্ডে 2,000 round trips

#### Latency numbers visualized

![](https://camo.githubusercontent.com/77f72259e1eb58596b564d1ad823af1853bc60a3/687474703a2f2f692e696d6775722e636f6d2f6b307431652e706e67)

#### Source(s) and further reading

* [Latency numbers every programmer should know - 1](https://gist.github.com/jboner/2841832)
* [Latency numbers every programmer should know - 2](https://gist.github.com/hellerbarde/2843375)
* [Designs, lessons, and advice from building large distributed systems](http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf)
* [Software Engineering Advice from Building Large-Scale Distributed Systems](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/stanford-295-talk.pdf)

### Additional system design interview questions

> সাধারণ সিস্টেম ডিজাইন ইন্টারভিউ প্রশ্ন, প্রতিটি কীভাবে সমাধান করতে হবে তার resources এর সাথে links সহ।

| Question | Reference(s) |
|---|---|
| Design a file sync service like Dropbox | [youtube.com](https://www.youtube.com/watch?v=PE4gwstWhmc) |
| Design a search engine like Google | [queue.acm.org](http://queue.acm.org/detail.cfm?id=988407)<br/>[stackexchange.com](http://programmers.stackexchange.com/questions/38324/interview-question-how-would-you-implement-google-search)<br/>[ardendertat.com](http://www.ardendertat.com/2012/01/11/implementing-search-engines/)<br/>[stanford.edu](http://infolab.stanford.edu/~backrub/google.html) |
| Design a scalable web crawler like Google | [quora.com](https://www.quora.com/How-can-I-build-a-web-crawler-from-scratch) |
| Design Google docs | [code.google.com](https://code.google.com/p/google-mobwrite/)<br/>[neil.fraser.name](https://neil.fraser.name/writing/sync/) |
| Design a key-value store like Redis | [slideshare.net](http://www.slideshare.net/dvirsky/introduction-to-redis) |
| Design a cache system like Memcached | [slideshare.net](http://www.slideshare.net/oemebamo/introduction-to-memcached) |
| Design a recommendation system like Amazon's | [hulu.com](https://web.archive.org/web/20170406065247/http://tech.hulu.com/blog/2011/09/19/recommendation-system.html)<br/>[ijcai13.org](http://ijcai13.org/files/tutorial_slides/td3.pdf) |
| Design a tinyurl system like Bitly | [n00tc0d3r.blogspot.com](http://n00tc0d3r.blogspot.com/) |
| Design a chat app like WhatsApp | [highscalability.com](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)
| Design a picture sharing system like Instagram | [highscalability.com](http://highscalability.com/flickr-architecture)<br/>[highscalability.com](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html) |
| Design the Facebook news feed function | [quora.com](http://www.quora.com/What-are-best-practices-for-building-something-like-a-News-Feed)<br/>[quora.com](http://www.quora.com/Activity-Streams/What-are-the-scaling-issues-to-keep-in-mind-while-developing-a-social-network-feed)<br/>[slideshare.net](http://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture) |
| Design the Facebook timeline function | [facebook.com](https://www.facebook.com/note.php?note_id=10150468255628920)<br/>[highscalability.com](http://highscalability.com/blog/2012/1/23/facebook-timeline-brought-to-you-by-the-power-of-denormaliza.html) |
| Design the Facebook chat function | [erlang-factory.com](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf)<br/>[facebook.com](https://www.facebook.com/note.php?note_id=14218138919&id=9445547199&index=0) |
| Design a graph search function like Facebook's | [facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-building-out-the-infrastructure-for-graph-search/10151347573598920)<br/>[facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-indexing-and-ranking-in-graph-search/10151361720763920)<br/>[facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-the-natural-language-interface-of-graph-search/10151432733048920) |
| Design a content delivery network like CloudFlare | [figshare.com](https://figshare.com/articles/Globally_distributed_content_delivery/6605972) |
| Design a trending topic system like Twitter's | [michael-noll.com](http://www.michael-noll.com/blog/2013/01/18/implementing-real-time-trending-topics-in-storm/)<br/>[snikolov .wordpress.com](http://snikolov.wordpress.com/2012/11/14/early-detection-of-twitter-trends/) |
| Design a random ID generation system | [blog.twitter.com](https://blog.twitter.com/2010/announcing-snowflake)<br/>[github.com](https://github.com/twitter/snowflake/) |
| Return the top k requests during a time interval | [cs.ucsb.edu](https://www.cs.ucsb.edu/sites/default/files/documents/2005-23.pdf)<br/>[wpi.edu](http://davis.wpi.edu/xmdv/docs/EDBT11-diyang.pdf) |
| Design a system that serves data from multiple data centers | [highscalability.com](http://highscalability.com/blog/2009/8/24/how-google-serves-data-from-multiple-datacenters.html) |
| Design an online multiplayer card game | [indieflashblog.com](https://web.archive.org/web/20180929181117/http://www.indieflashblog.com/how-to-create-an-asynchronous-multiplayer-game.html)<br/>[buildnewgames.com](http://buildnewgames.com/real-time-multiplayer/) |
| Design a garbage collection system | [stuffwithstuff.com](http://journal.stuffwithstuff.com/2013/12/08/babys-first-garbage-collector/)<br/>[washington.edu](http://courses.cs.washington.edu/courses/csep521/07wi/prj/rick.pdf) |
| Design an API rate limiter | [https://stripe.com/blog/](https://stripe.com/blog/rate-limiters) |
| Design a Stock Exchange (like NASDAQ or Binance) | [Jane Street](https://youtu.be/b1e4t2k2KJY)<br/>[Golang Implementation](https://around25.com/blog/building-a-trading-engine-for-a-crypto-exchange/)<br/>[Go Implementation](http://bhomnick.net/building-a-simple-limit-order-in-go/) |
| Add a system design question | [Contribute](#contributing) |

### Real world architectures

> কীভাবে real world systems ডিজাইন করা হয় তার উপর articles।

<p align="center">
  <img src="images/TcUo2fw.png">
  <br/>
  <i><a href=https://www.infoq.com/presentations/Twitter-Timeline-Scalability>Source: Twitter timelines at scale</a></i>
</p>

**নিম্নলিখিত articles এর জন্য nitty gritty details এর উপর ফোকাস করবেন না, পরিবর্তে:**

* এই articles এর মধ্যে shared principles, common technologies, এবং patterns identify করুন
* প্রতিটি component দ্বারা কী সমস্যা সমাধান করা হয়, এটি কোথায় কাজ করে, কোথায় কাজ করে না তা study করুন
* Lessons learned পর্যালোচনা করুন

|Type | System | Reference(s) |
|---|---|---|
| Data processing | **MapReduce** - Distributed data processing from Google | [research.google.com](http://static.googleusercontent.com/media/research.google.com/zh-CN/us/archive/mapreduce-osdi04.pdf) |
| Data processing | **Spark** - Distributed data processing from Databricks | [slideshare.net](http://www.slideshare.net/AGrishchenko/apache-spark-architecture) |
| Data processing | **Storm** - Distributed data processing from Twitter | [slideshare.net](http://www.slideshare.net/previa/storm-16094009) |
| | | |
| Data store | **Bigtable** - Distributed column-oriented database from Google | [harvard.edu](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf) |
| Data store | **HBase** - Open source implementation of Bigtable | [slideshare.net](http://www.slideshare.net/alexbaranau/intro-to-hbase) |
| Data store | **Cassandra** - Distributed column-oriented database from Facebook | [slideshare.net](http://www.slideshare.net/planetcassandra/cassandra-introduction-features-30103666)
| Data store | **DynamoDB** - Document-oriented database from Amazon | [harvard.edu](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf) |
| Data store | **MongoDB** - Document-oriented database | [slideshare.net](http://www.slideshare.net/mdirolf/introduction-to-mongodb) |
| Data store | **Spanner** - Globally-distributed database from Google | [research.google.com](http://research.google.com/archive/spanner-osdi2012.pdf) |
| Data store | **Memcached** - Distributed memory caching system | [slideshare.net](http://www.slideshare.net/oemebamo/introduction-to-memcached) |
| Data store | **Redis** - Distributed memory caching system with persistence and value types | [slideshare.net](http://www.slideshare.net/dvirsky/introduction-to-redis) |
| | | |
| File system | **Google File System (GFS)** - Distributed file system | [research.google.com](http://static.googleusercontent.com/media/research.google.com/zh-CN/us/archive/gfs-sosp2003.pdf) |
| File system | **Hadoop File System (HDFS)** - Open source implementation of GFS | [apache.org](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) |
| | | |
| Misc | **Chubby** - Lock service for loosely-coupled distributed systems from Google | [research.google.com](http://static.googleusercontent.com/external_content/untrusted_dlcp/research.google.com/en/us/archive/chubby-osdi06.pdf) |
| Misc | **Dapper** - Distributed systems tracing infrastructure | [research.google.com](http://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36356.pdf)
| Misc | **Kafka** - Pub/sub message queue from LinkedIn | [slideshare.net](http://www.slideshare.net/mumrah/kafka-talk-tri-hug) |
| Misc | **Zookeeper** - Centralized infrastructure and services enabling synchronization | [slideshare.net](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper) |
| | Add an architecture | [Contribute](#contributing) |

### Company architectures

| Company | Reference(s) |
|---|---|
| Amazon | [Amazon architecture](http://highscalability.com/amazon-architecture) |
| Cinchcast | [Producing 1,500 hours of audio every day](http://highscalability.com/blog/2012/7/16/cinchcast-architecture-producing-1500-hours-of-audio-every-d.html) |
| DataSift | [Realtime datamining At 120,000 tweets per second](http://highscalability.com/blog/2011/11/29/datasift-architecture-realtime-datamining-at-120000-tweets-p.html) |
| Dropbox | [How we've scaled Dropbox](https://www.youtube.com/watch?v=PE4gwstWhmc) |
| ESPN | [Operating At 100,000 duh nuh nuhs per second](http://highscalability.com/blog/2013/11/4/espns-architecture-at-scale-operating-at-100000-duh-nuh-nuhs.html) |
| Google | [Google architecture](http://highscalability.com/google-architecture) |
| Instagram | [14 million users, terabytes of photos](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html)<br/>[What powers Instagram](http://instagram-engineering.tumblr.com/post/13649370142/what-powers-instagram-hundreds-of-instances) |
| Justin.tv | [Justin.Tv's live video broadcasting architecture](http://highscalability.com/blog/2010/3/16/justintvs-live-video-broadcasting-architecture.html) |
| Facebook | [Scaling memcached at Facebook](https://cs.uwaterloo.ca/~brecht/courses/854-Emerging-2014/readings/key-value/fb-memcached-nsdi-2013.pdf)<br/>[TAO: Facebook's distributed data store for the social graph](https://cs.uwaterloo.ca/~brecht/courses/854-Emerging-2014/readings/data-store/tao-facebook-distributed-datastore-atc-2013.pdf)<br/>[Facebook's photo storage](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf)<br/>[How Facebook Live Streams To 800,000 Simultaneous Viewers](http://highscalability.com/blog/2016/6/27/how-facebook-live-streams-to-800000-simultaneous-viewers.html) |
| Flickr | [Flickr architecture](http://highscalability.com/flickr-architecture) |
| Mailbox | [From 0 to one million users in 6 weeks](http://highscalability.com/blog/2013/6/18/scaling-mailbox-from-0-to-one-million-users-in-6-weeks-and-1.html) |
| Netflix | [A 360 Degree View Of The Entire Netflix Stack](http://highscalability.com/blog/2015/11/9/a-360-degree-view-of-the-entire-netflix-stack.html)<br/>[Netflix: What Happens When You Press Play?](http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html) |
| Pinterest | [From 0 To 10s of billions of page views a month](http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)<br/>[18 million visitors, 10x growth, 12 employees](http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html) |
| Playfish | [50 million monthly users and growing](http://highscalability.com/blog/2010/9/21/playfishs-social-gaming-architecture-50-million-monthly-user.html) |
| PlentyOfFish | [PlentyOfFish architecture](http://highscalability.com/plentyoffish-architecture) |
| Salesforce | [How they handle 1.3 billion transactions a day](http://highscalability.com/blog/2013/9/23/salesforce-architecture-how-they-handle-13-billion-transacti.html) |
| Stack Overflow | [Stack Overflow architecture](http://highscalability.com/blog/2009/8/5/stack-overflow-architecture.html) |
| TripAdvisor | [40M visitors, 200M dynamic page views, 30TB data](http://highscalability.com/blog/2011/6/27/tripadvisor-architecture-40m-visitors-200m-dynamic-page-view.html) |
| Tumblr | [15 billion page views a month](http://highscalability.com/blog/2012/2/13/tumblr-architecture-15-billion-page-views-a-month-and-harder.html) |
| Twitter | [Making Twitter 10000 percent faster](http://highscalability.com/scaling-twitter-making-twitter-10000-percent-faster)<br/>[Storing 250 million tweets a day using MySQL](http://highscalability.com/blog/2011/12/19/how-twitter-stores-250-million-tweets-a-day-using-mysql.html)<br/>[150M active users, 300K QPS, a 22 MB/S firehose](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html)<br/>[Timelines at scale](https://www.infoq.com/presentations/Twitter-Timeline-Scalability)<br/>[Big and small data at Twitter](https://www.youtube.com/watch?v=5cKTP36HVgI)<br/>[Operations at Twitter: scaling beyond 100 million users](https://www.youtube.com/watch?v=z8LU0Cj6BOU)<br/>[How Twitter Handles 3,000 Images Per Second](http://highscalability.com/blog/2016/4/20/how-twitter-handles-3000-images-per-second.html) |
| Uber | [How Uber scales their real-time market platform](http://highscalability.com/blog/2015/9/14/how-uber-scales-their-real-time-market-platform.html)<br/>[Lessons Learned From Scaling Uber To 2000 Engineers, 1000 Services, And 8000 Git Repositories](http://highscalability.com/blog/2016/10/12/lessons-learned-from-scaling-uber-to-2000-engineers-1000-ser.html) |
| WhatsApp | [The WhatsApp architecture Facebook bought for $19 billion](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html) |
| YouTube | [YouTube scalability](https://www.youtube.com/watch?v=w5WVu624fY8)<br/>[YouTube architecture](http://highscalability.com/youtube-architecture) |

### Company engineering blogs

> আপনি যে কোম্পানিগুলোর সাথে ইন্টারভিউ দিচ্ছেন তাদের জন্য architectures।

> আপনি যে questions এর মুখোমুখি হতে পারেন সেগুলো একই domain থেকে হতে পারে।

* [Airbnb Engineering](http://nerds.airbnb.com/)
* [Atlassian Developers](https://developer.atlassian.com/blog/)
* [AWS Blog](https://aws.amazon.com/blogs/aws/)
* [Bitly Engineering Blog](http://word.bitly.com/)
* [Box Blogs](https://blog.box.com/blog/category/engineering)
* [Cloudera Developer Blog](http://blog.cloudera.com/)
* [Dropbox Tech Blog](https://tech.dropbox.com/)
* [Engineering at Quora](https://www.quora.com/q/quoraengineering)
* [Ebay Tech Blog](http://www.ebaytechblog.com/)
* [Evernote Tech Blog](https://blog.evernote.com/tech/)
* [Etsy Code as Craft](http://codeascraft.com/)
* [Facebook Engineering](https://www.facebook.com/Engineering)
* [Flickr Code](http://code.flickr.net/)
* [Foursquare Engineering Blog](http://engineering.foursquare.com/)
* [GitHub Engineering Blog](https://github.blog/category/engineering)
* [Google Research Blog](http://googleresearch.blogspot.com/)
* [Groupon Engineering Blog](https://engineering.groupon.com/)
* [Heroku Engineering Blog](https://engineering.heroku.com/)
* [Hubspot Engineering Blog](http://product.hubspot.com/blog/topic/engineering)
* [High Scalability](http://highscalability.com/)
* [Instagram Engineering](http://instagram-engineering.tumblr.com/)
* [Intel Software Blog](https://software.intel.com/en-us/blogs/)
* [Jane Street Tech Blog](https://blogs.janestreet.com/category/ocaml/)
* [LinkedIn Engineering](http://engineering.linkedin.com/blog)
* [Microsoft Engineering](https://engineering.microsoft.com/)
* [Microsoft Python Engineering](https://blogs.msdn.microsoft.com/pythonengineering/)
* [Netflix Tech Blog](http://techblog.netflix.com/)
* [Paypal Developer Blog](https://medium.com/paypal-engineering)
* [Pinterest Engineering Blog](https://medium.com/@Pinterest_Engineering)
* [Reddit Blog](http://www.redditblog.com/)
* [Salesforce Engineering Blog](https://developer.salesforce.com/blogs/engineering/)
* [Slack Engineering Blog](https://slack.engineering/)
* [Spotify Labs](https://labs.spotify.com/)
* [Stripe Engineering Blog](https://stripe.com/blog/engineering)
* [Twilio Engineering Blog](http://www.twilio.com/engineering)
* [Twitter Engineering](https://blog.twitter.com/engineering/)
* [Uber Engineering Blog](http://eng.uber.com/)
* [Yahoo Engineering Blog](http://yahooeng.tumblr.com/)
* [Yelp Engineering Blog](http://engineeringblog.yelp.com/)
* [Zynga Engineering Blog](https://www.zynga.com/blogs/engineering)

#### Source(s) and further reading

একটি blog যোগ করতে চান? কাজ duplicate করা এড়াতে, নিম্নলিখিত repo এ আপনার company blog যোগ করার বিবেচনা করুন:

* [kilimchoi/engineering-blogs](https://github.com/kilimchoi/engineering-blogs)

## Under development

একটি section যোগ করতে বা একটি in-progress সম্পূর্ণ করতে আগ্রহী? [Contribute](#contributing)!

* Distributed computing with MapReduce
* Consistent hashing
* Scatter gather
* [Contribute](#contributing)

## Credits

Credits এবং sources এই repo জুড়ে প্রদান করা হয়।

Special thanks to:

* [Hired in tech](http://www.hiredintech.com/system-design/the-system-design-process/)
* [Cracking the coding interview](https://www.amazon.com/dp/0984782850/)
* [High scalability](http://highscalability.com/)
* [checkcheckzz/system-design-interview](https://github.com/checkcheckzz/system-design-interview)
* [shashank88/system_design](https://github.com/shashank88/system_design)
* [mmcgrana/services-engineering](https://github.com/mmcgrana/services-engineering)
* [System design cheat sheet](https://gist.github.com/vasanthk/485d1c25737e8e72759f)
* [A distributed systems reading list](http://dancres.github.io/Pages/)
* [Cracking the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)

## Contact info

যেকোনো issues, questions, বা comments নিয়ে আলোচনা করতে নির্দ্বিধায় contact করুন।

আমার contact info আমার [GitHub page](https://github.com/donnemartin) এ পাওয়া যাবে।

## License

*আমি এই repository এ একটি open source license এর অধীনে আপনাকে code এবং resources প্রদান করছি। কারণ এটি আমার personal repository, আপনি আমার code এবং resources এর জন্য যে license receive করেন তা আমার কাছ থেকে এবং আমার employer (Facebook) থেকে নয়।*

    Copyright 2017 Donne Martin

    Creative Commons Attribution 4.0 International License (CC BY 4.0)

    http://creativecommons.org/licenses/by/4.0/ 
