অবদান
============

অবদান স্বাগত!

**কোড review প্রক্রিয়া যতটা সম্ভব smooth করতে এবং আপনার অবদান merge হওয়ার সম্ভাবনা maximize করতে অনুগ্রহ করে এই page টি carefully পড়ুন।**

## Bug Reports

Bug reports বা requests এর জন্য [একটি issue submit করুন](https://github.com/donnemartin/system-design-primer/issues)।

## Pull Requests

অবদান করার preferred way হল GitHub এ [main repository](https://github.com/donnemartin/system-design-primer) fork করা।

1. [main repository](https://github.com/donnemartin/system-design-primer) fork করুন। Page এর top এর কাছে 'Fork' button এ click করুন। এটি GitHub server এ আপনার account এর অধীনে code এর একটি copy তৈরি করে।

2. এই copy টি আপনার local disk এ clone করুন:

        $ git clone git@github.com:YourLogin/system-design-primer.git
        $ cd system-design-primer

3. আপনার changes hold করার জন্য একটি branch তৈরি করুন এবং changes করা শুরু করুন। `master` branch এ কাজ করবেন না!

        $ git checkout -b my-feature

4. Version control করতে Git ব্যবহার করে আপনার computer এ এই copy এ কাজ করুন। আপনি editing শেষ হলে, Git এ আপনার changes record করতে নিম্নলিখিত run করুন:

        $ git add modified_files
        $ git commit

5. GitHub এ আপনার changes push করুন:

        $ git push -u origin my-feature

6. অবশেষে, `system-design-primer` repo এর আপনার fork এর web page এ যান এবং review এর জন্য আপনার changes পাঠাতে 'Pull Request' click করুন।

### GitHub Pull Requests Docs

যদি আপনি pull requests এর সাথে familiar না হন, [pull request docs](https://help.github.com/articles/using-pull-requests/) review করুন।

## Translations

আমরা চাই guide অনেক languages এ available থাকুক। Translations maintain করার প্রক্রিয়া এখানে:

* Guide এর এই original version এবং content English এ maintained হয়।
* Translations original এর content follow করে। Contributors এর অন্তত কিছু English বলতে হবে, যাতে translations diverge না করে।
* প্রতিটি translation এর একটি maintainer থাকে original evolve হওয়ার সাথে translation update করতে এবং অন্যরা changes review করতে। এটি অনেক সময় প্রয়োজন করে না, কিন্তু maintainer দ্বারা একটি review quality maintain করার জন্য গুরুত্বপূর্ণ।

[Translations](TRANSLATIONS.md) দেখুন।

### Changes to translations

* Content এ changes প্রথমে English version এ করা উচিত, এবং তারপর প্রতিটি other language এ translate করা উচিত।
* Translations improve করে এমন changes সরাসরি সেই language এর file এ করা উচিত। Pull requests শুধুমাত্র একবারে একটি language modify করা উচিত।
* সেই language এর file এ changes সহ একটি pull request submit করুন। প্রতিটি language এর একটি maintainer থাকে, যিনি সেই language এ changes review করেন। তারপর primary maintainer [@donnemartin](https://github.com/donnemartin) এটি merge করে।
* Pull requests এবং issues language codes দিয়ে prefix করুন যদি তারা শুধুমাত্র সেই translation এর জন্য হয়, উদাহরণস্বরূপ "es: Improve grammar", যাতে maintainers সহজে খুঁজে পেতে পারে।
* Code review এর জন্য translation maintainer tag করুন, [translation maintainers](TRANSLATIONS.md) এর list দেখুন।
    * আপনার pull request merge হওয়ার আগে আপনার একটি native speaker (preferably language maintainer) থেকে একটি review পাওয়া প্রয়োজন হবে।

### Adding translations to new languages

নতুন languages এ translations সবসময় স্বাগত! মনে রাখবেন একটি translation maintain করা আবশ্যক।

* আপনার একটি নতুন language এর maintainer হওয়ার সময় আছে? অনুগ্রহ করে [translations](TRANSLATIONS.md) এর list দেখুন এবং আমাদের জানান যাতে আমরা জানতে পারি আমরা future এ আপনার উপর count করতে পারি।
* [translations](TRANSLATIONS.md), issues, এবং pull requests check করুন একটি translation progress এ আছে বা stalled আছে কিনা দেখতে। যদি এটি progress এ থাকে, সাহায্য করার offer করুন। যদি এটি stalled থাকে, আপনি commit করতে পারলে maintainer হওয়ার বিবেচনা করুন।
* যদি একটি translation এখনও শুরু না হয়ে থাকে, আপনার language এর জন্য একটি issue file করুন যাতে people জানতে পারে আপনি এটি নিয়ে কাজ করছেন এবং আমরা coordinate করব। নিশ্চিত করুন আপনি language এ native level এবং translation maintain করতে ইচ্ছুক, যাতে এটি orphaned না হয়।
* শুরু করতে, repo fork করুন, তারপর main repo এ একটি pull request submit করুন single file README-xx.md যোগ করা সহ, যেখানে xx হল language code। Standard [IETF language tags](https://www.w3.org/International/articles/language-tags/) ব্যবহার করুন, অর্থাৎ Wikipedia দ্বারা ব্যবহৃত একই, *না* একটি single country এর code। এগুলি সাধারণত শুধুমাত্র two-letter lowercase code, উদাহরণস্বরূপ, French এর জন্য `fr` এবং Ukrainian এর জন্য `uk` (না `ua`, যা country এর জন্য)। Variations আছে এমন languages এর জন্য, shortest tag ব্যবহার করুন, যেমন `zh-Hant`।
* আপনার friends কে invite করতে feel free করুন আপনার original translation সাহায্য করতে তাদের repo fork করার মাধ্যমে, তারপর তাদের pull requests আপনার forked repo এ merge করার মাধ্যমে। Translations difficult এবং সাধারণত errors থাকে যা others খুঁজে বের করতে হবে।
* প্রতিটি README-XX.md file এর top এ আপনার translation এর links যোগ করুন। Consistency এর জন্য, link ISO code দ্বারা alphabetical order এ যোগ করা উচিত, এবং anchor text native language এ হওয়া উচিত।
* যখন আপনি English README.md fully translate করেছেন, main repo এ pull request এ comment করুন এটি merge করার জন্য ready।
    * আপনার translation `master` branch এ merge হওয়ার আগে আপনার English README.md এর একটি complete এবং reviewed translation থাকা প্রয়োজন হবে।
    * একবার accepted হলে, আপনার pull request `master` branch এ একটি single commit এ squashed হবে।

### Translation template credits

Translation template এর জন্য [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line) কে ধন্যবাদ।
