# 02. মিডলওয়্যার আর্কিটেকচার, Body Parser ও Express Router (Middleware Architecture & Express Router)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Express.js ট্র্যাক](../../Express.js/README.md)

---

## Introduction

আপনি কি জানেন যে র উইথআউট-ফ্রেমওয়ার্ক (pure) Node.js-এ ক্লায়েন্টের পাঠানো Form Data প্রসেস করতে Buffer এবং Event Listener দিয়ে দীর্ঘ জটিল কোড লিখতে হয়, কিন্তু Express.js ঠিক এই কাজটি মাত্র ১ লাইন কোডে সম্পাদন করতে পারে?

By the end of this lesson, you will be able to:

* **Analyze** how incoming request bodies are parsed using `express.urlencoded()` and mapped to `req.body`.
* **Structure** modular route handlers using `express.Router()` and path prefixing.
* **Serve** static HTML files seamlessly using Node.js's native `path` module and `rootDir` helpers.
* **Implement** custom 404 error pages with correct HTTP status codes and middleware fallthrough sequencing.

## এক নজরে আজকের ক্লাসের বিষয়বস্তু

আজকের ক্লাসে আমরা Express.js-এর গুরুত্বপূর্ণ এবং অ্যাডভান্সড মেকানিজম নিয়ে গভীরভাবে আলোচনা করব। পূর্বের পাঠে আমরা Node.js দিয়ে ম্যানুয়ালি যেভাবে রিকোয়েস্ট হ্যান্ডেল করেছিলাম, এক্সপ্রেস কীভাবে সেই পুরো ডেভেলপমেন্ট এক্সপেরিয়েন্সকে অনেক বেশি সহজ, পরিষ্কার এবং মডিউলার করে তোলে তা দেখব।

### মূল আলোচ্য বিষয়সমূহ:

1. **Request Body Parsing**: `body-parser` মডিউল এবং এক্সপ্রেসের বিল্ট-ইন `express.urlencoded()` মিডিয়ালওয়্যারের ব্যবহার।
2. **Express Router (** **express.Router()** **)**: অ্যাপ্লিকেশনের সমস্ত রাউট এক ফাইলে না রেখে `userRouter` ও `hostRouter`\-এর মতো পৃথক মডিউলে ভাগ করা।
3. **Path Prefixing**: বিভিন্ন রাউটের পূর্বে একই ধরণের কমন পাথ (যেমন: `/host`) যুক্ত করা।
4. **Static HTML File Serving**: JavaScript স্ট্রিংয়ের বদলে আলাদা `.html` ফাইল তৈরি করে `res.sendFile()` দিয়ে সার্ভিস দেওয়া।
5. **Path Utility (** **rootDir** **Helper)**: অপারেটিং সিস্টেমের পাথ জটিলতা এড়াতে Node.js Core `path` মডিউল এবং হেলপার তৈরি।
6. **Custom 404 Page**: ফলব্যাক মিডলওয়্যার এবং সঠিক HTTP স্ট্যাটাস কোড সহ কাস্টম ৪০৪ পেজ হ্যান্ডলিং।

### 🧠 Lecture Topics Mind Map
> [!TIP]
> **মূল কনসেপ্ট সারসংক্ষেপ (Core Concept Breakdown):**
> A visual breakdown of all core sub-topics covered in Express.js Deep Dive including Parsing, Routing, File Serving, and Error Handling.

## Request Body Parsing এবং express.urlencoded()

### What is it? (রিকোয়েস্ট পার্সিং কী?)

রিকোয়েস্ট পার্সিং (Request Parsing) হলো ক্লায়েন্ট বা ব্রাউজার থেকে POST রিকোয়েস্টের মাধ্যমে পাঠানো ফর্মের ডেটাকে সার্ভারে পড়ার উপযোগী জাভাস্ক্রিপ্ট অবজেক্টে (JavaScript Object) রূপান্তর করার প্রক্রিয়া।

### Why is it used? (কেন এটি ব্যবহৃত হয়?)

Pure Node.js-এ ক্লায়েন্টের ডেটা ছোট ছোট টুকরো বা Chunk আকারে আসতো। সেই স্ট্রিমিং ডেটাকে `req.on('data')` এবং `req.on('end')` ইভেন্ট লিসনার দিয়ে ম্যানুয়ালি বাফার জোড়া লাগিয়ে অবজেক্টে কনভার্ট করতে হতো। এই দীর্ঘ ও ঝামেলার কাজটিকে সহজ করতেই আমরা **Body Parser** বা **express.urlencoded()** ব্যবহার করি।

### How does it work? (কীভাবে কাজ করে?)

এটি একটি মিডলওয়্যার (Middleware) হিসেবে কাজ করে। ইনকামিং POST রিকোয়েস্টের হেডার ও পে-লোড পর্যালোচনা করে এটি অটোমেটিকালি ডেটা পার্স করে এবং তৈরি হওয়া অবজেক্টটি `req.body`\-এর মধ্যে জমা করে দেয়। ফলে পরবর্তীতে যেকোনো রাউট হ্যান্ডলারে `req.body.fieldName` লিখেই ক্লায়েন্টের পাঠানো তথ্য পাওয়া যায়।

### Important Method: express.urlencoded({ extended: false })

* **Purpose**: ফর্মের এনকোডেড ডেটা (`application/x-www-form-urlencoded`) পার্স করা।
* **Input**: Options Object (যেমন `{ extended: false }`)।
* **Output**: একটি মিডলওয়্যার ফাংশন যা `req.body` সেট করে।

```
// app.js-এ মিডলওয়্যার যুক্ত করা
app.use(express.urlencoded({ extended: false }));

// POST রাউট হ্যান্ডলার
app.post('/contact-us', (req, res) =&gt; {
  console.log(req.body); // Output: { name: 'Prashant', email: 'test@gmail.com' }
  res.send('Form Submitted Successfully!');
});

```

### Common Mistakes

* POST রাউট ডিফাইন করার **পরে** পার্সিং মিডলওয়্যার সেট করা। পার্সিং মিডলওয়্যার সবসময় রাউটের **ওপরে** লাগাতে হয়, তা না হলে `req.body` রিটার্ন করবে `undefined`!

### Mental Model

চিন্তা করুন একজন অনুবাাদক বা দোভাষীর কথা। ক্লায়েন্ট কাঁচা ডেটা ফর্মে পাঠায়, আর `express.urlencoded()` মিডলওয়্যার সেই ডেটা পড়ে সাজিয়ে আপনার টেবিলের ওপর (`req.body`) সুবিন্যস্তভাবে রেখে দেয়।

### 📊 Request Body Parsing Flow
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> Visual diagram contrasting the complex chunk-based parsing in Raw Node.js versus single-line parsing with express.urlencoded().

## Express Router (express.Router()) ও Route Modularization

### What is it? (এক্সপ্রেস রাউটার কী?)

`express.Router()` হলো এক্সপ্রেসের একটি অন্যতম সেরা ফিচার। এটি একটি স্বয়ংসম্পূর্ণ "মিনি এক্সপ্রেস অ্যাপ্লিকেশন" (Mini-App) যা বিভিন্ন রাউট (Routes) এবং মিডলওয়্যারকে পৃথক ফাইলে গুছিয়ে রাখার সুবিধা দেয়।

### Why is it used? (কেন এটি ব্যবহৃত হয়?)

বাস্তব জীবনের একটি বড় প্রজেক্টে (যেমন AirBnB) ১০০টিরও বেশি রাউট থাকতে পারে। সব রাউট যদি কেবল `app.js` ফাইলে লেখা হয়, তবে ফাইলটি বিশাল এবং মেইনটেইন করা অসম্ভব হয়ে পড়ে। `express.Router()` ব্যবহার করে আমরা ইউজার সম্পর্কিত রাউট (`userRouter.js`) এবং হোস্ট বা অ্যাডমিন সম্পর্কিত রাউট (`hostRouter.js`) সম্পূর্ণ আলাদা ফোল্ডারে ও ফাইলে ভাগ করে ফেলতে পারি।

### How to implement Express Router?

১. `routes/` ফোল্ডারে একটি পৃথক ফাইল তৈরি করুন (যেমন: `hostRouter.js`)। ২. `const router = express.Router();` দিয়ে রাউটার অবজেক্ট তৈরি করুন। ৩. `app.get()` বা `app.post()`\-এর বদলে `router.get()` বা `router.post()` ব্যবহার করুন। ৪. ফাইল শেষে `module.exports = router;` রফতানি করুন। ৫. মূল `app.js` ফাইলে রিকোয়ার (require) করে `app.use(router)` দিয়ে কানেক্ট করুন।

### Path Prefixing (কমন পাথ যুক্ত করা)

ধরা যাক, হোস্ট সম্পর্কিত প্রতিটি ইউআরএল `/host`\-দিয় শুরু হয় (যেমন: `/host/add-home`)। আমাদের বারবার রাউটার ফাইলের ভেতরে `/host/add-home` লেখার দরকার নেই। আমরা `app.js`\-এ ফিল্টার পাথ ব্যবহার করতে পারি:

```
// routes/hostRouter.js
const express = require('express');
const router = express.Router();

router.get('/add-home', (req, res) =&gt; {
  res.send('Add Home Page');
});

module.exports = router;

// app.js
const hostRouter = require('./routes/hostRouter');
app.use('/host', hostRouter); // Prefixing /host

```

এখন ব্রাউজারে `/host/add-home` হিট করলেই রাউটটি কাজ করবে!

### Mental Model

একটি বিশাল মাল্টি-স্টোরি শপিং মল ভাবুন। `app.js` হলো মূল রিসেপশন গেট, আর `userRouter` ও `hostRouter` হলো লেভেল-১ ও লেভেল-২ এর নির্দিষ্ট ব্র্যান্ড শোরুম।

### 📊 Modular Architecture with Express Router
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> Step-by-step slides showing how app.js delegates requests to modular router files with path prefixes.

## Static HTML File Serving এবং Path Helper (rootDir)

### What is it? (এইচটিএমএল ফাইল সার্ভিং কী?)

সার্ভার থেকে সরাসরি JavaScript ফাইলে এইচটিএমএল স্ট্রিং (`<h1>Hello</h1>`) না লিখে আলাদা `.html` ফাইল বানিয়ে `res.sendFile()` মেথডের মাধ্যমে ক্লায়েন্টে পাঠানো।

### Why is it used? (কেন এটি ব্যবহৃত হয়?)

জাভাস্ক্রিপ্ট ফাইলের ভেতরে HTML কোড লিখলে কোনো অটো-কমপ্লিট, কালার হাইলাইটিং বা সিনট্যাক্স চেকিং পাওয়া যায় না। কোডের সাইজও অনেক বড় হয়ে যায়। তাই UI কোডকে `views/` ফোল্ডারে আলাদা রাখাই স্ট্যান্ডার্ড প্র্যাকটিস।

### Core Problem with Relative Paths

ব্রাউজার বা ফাইল সিস্টেমে ফাইল খোঁজার সময় অপারেটিং সিস্টেম ভেদে পাথের ফরম্যাট ভিন্ন হয় (Windows-এ ব্যাকস্ল্যাশ `\` এবং Linux/Mac-এ ফরওয়ার্ডস্ল্যাশ `/`)। তাই ম্যানুয়ালি `'../views/index.html'` লিখলে সার্ভার ক্র্যাশ করতে পারে।

### Node.js Core 'path' Module and 'rootDir' Helper

এই সমস্যার চিরস্থায়ী সমাধানের জন্য আমরা Node.js-এর বিল্ট-ইন `path` মডিউল এবং একটি হেলপার ইউটিলিটি ফাইল তৈরি করি।

```
// utils/pathUtil.js (Helper File)
const path = require('path');
// require.main.filename আপনার প্রজেক্টের রুট ফাইল (app.js) এর লোকেশন নির্দেশ করে
module.exports = path.dirname(require.main.filename);

```

### HTML File পাঠানোর নিয়ম:

```
const path = require('path');
const rootDir = require('../utils/pathUtil');

router.get('/add-home', (req, res) =&gt; {
  // path.join যেকোনো ওএস (OS)-এর জন্য সঠিক ফাইল পাথ তৈরি করে
  res.sendFile(path.join(rootDir, 'views', 'add-home.html'));
});

```

### 💡 Path Management Flashcards
> [!IMPORTANT]
> **কুইক রিভিশন পয়েন্টস (Quick Revision Points):**
> Interactive flashcards testing path.join, require.main.filename, path.dirname, and res.sendFile usage.

## Custom 404 Error Handling ও Middleware Sequence

### What is it? (কাস্টম ৪০৪ পেজ কী?)

যখন কোনো ব্যবহারকারী এমন কোনো ইউআরএল (URL) অ্যাক্সেস করার চেষ্টা করে যা আমাদের সার্ভারে ডিফাইন করা নেই, তখন তাকে একটি সুন্দর কাস্টম "404 Page Not Found" পেজ দেখানো।

### How does Middleware Sequence work?

এক্সপ্রেস মিডলওয়্যারগুলো সম্পূর্ণ সিকোয়েন্স বা ক্রম অনুযায়ী ওপর থেকে নিচে কাজ করে। যদি একটি রিকোয়েস্ট প্রথম কোনো রাউটের সাথে ম্যাচ না করে, তবে এটি পরবর্তী মিডলওয়্যারে চলে যায়।

আমরা যদি সব বৈধ রাউটিংয়ের একদম **শেষে** একটি ফ্যালব্যাক (Fallback) মিডলওয়্যার বসিয়ে দিই, তবে অব্যাখ্যায়িত যেকোনো ইউআরএল সেখানে এসে আটকে যাবে!

```
// বৈধ রাউটসমূহ ওপর থাকবে
app.use('/', userRouter);
app.use('/host', hostRouter);

// একদম শেষে ৪০৪ হ্যান্ডলার (Fallback)
app.use((req, res, next) =&gt; {
  res.status(404).sendFile(path.join(rootDir, 'views', '404.html'));
});

```

### Why res.status(404) is crucial?

যদি আপনি শুধুমাত্র `res.sendFile()` পাঠাবেন কিন্তু `res.status(404)` মেথড না দেবেন, তবে এক্সপ্রেস বাই-ডিফল্ট HTTP Status `200 OK` পাঠাবে! এর ফলে সার্চ ইঞ্জিন (SEO) এবং বিভিন্ন ব্রাউজার মনে করবে পেজটি সঠিকভাবে পাওয়া গেছে, যা একটি বড় প্রযুক্তিগত ভুল।

### 📌 Middleware Order and Fallthrough Mechanics
> Audio explanation discussing how middleware fallthrough works in Express and why 404 catch-all positioning matters.

## গুরুত্বপূর্ণ মডিউল, সিনট্যাক্স, ভুলসমূহ এবং পার্থক্য

### Modules Breakdown:

* **express**: Third-party NPM Package। রিকোয়েস্ট হ্যান্ডলিং, রাউটিং ও মিডলওয়্যার ম্যানেজমেন্টে ব্যবহৃত।
* **path**: Node.js Core Module (অতিরিক্ত ইনস্টল করতে হয় না)। ফাইল পাথ জোড়া দেওয়া ও ম্যানেজ করা।
* **body-parser**: Third-party NPM Package (বর্তমানে এক্সপ্রেসেই `express.urlencoded()` নামে বিল্ট-ইন থাকে)।

### Important Syntax Comparison:

| মেথড / ফিচার                 | উদ্দেশ্য / ব্যবহার                                        |
| ---------------------------- | --------------------------------------------------------- |
| `express.Router()`           | একটি নির্দিষ্ট মডিউলের জন্য নতুন রাউটার অবজেক্ট তৈরি করা। |
| `app.use('/prefix', router)` | একটি নির্দিষ্ট পাথের ওপর রাউটার সেট করা।                  |
| `res.sendFile(path)`         | সার্ভার থেকে ক্লায়েন্টে একটি নির্দিষ্ট ফাইল পাঠানো।      |
| `res.status(code)`           | HTTP স্ট্যাটাস কোড (যেমন: 200, 404, 500) সেট করা।         |

### Common Beginner Mistakes:

1. **404 মিডলওয়্যার সবার ওপর লেখা**: এটি করলে কোনো সাধারণ রাউট আর কাজ করবে না, কারণ রিকোয়েস্ট শুরুতেই ৪০৪ পেজে আটকে যাবে!
2. **express.urlencoded()** **বাদ দিয়ে** **req.body** **অ্যাক্সেস করা**: এর ফলে `req.body` সবসময় `undefined` দেখাবে।
3. **res.sendFile()** **\-এ সঠিক এবসোলিউট (Absolute) পাথ না দেওয়া**: ফাইল পাথ ভুল হলে Node.js `ENOENT` ফাইল নট ফাউন্ড এরর দিয়ে ক্র্যাশ করবে।
4. **res.status(404)** **বাদ দেওয়া**: পেজে "Not Found" লেখা থাকলেও ব্রাউজার স্ট্যাটাস ২০০ দেখাবে।

subscriptions

### 📌 Debugging Express Deep Dive Errors
> Short video walking through common bugs like undefined req.body, 404 middleware ordering issues, and file path mismatches.

## Summary

আজকের ক্লাসে আমরা এক্সপ্রেসের রিয়েল-ওয়ার্ল্ড প্রজেক্ট স্ট্রাকচার শেখার অন্যতম গুরুত্বপূর্ণ ধাপ সম্পন্ন করেছি। কীভাবে `express.urlencoded()` ব্যবহার করে ফর্ম ডেটা পার্স করতে হয়, কীভাবে বড় প্রজেক্টকে `express.Router()` দিয়ে মডিউলার করতে হয়, কীভাবে `path` মডিউলের সাহায্যে সুন্দরভাবে HTML ফাইল রেসপন্স হিসেবে পাঠাতে হয় এবং ফ্যালব্যাক মিডলওয়্যার দিয়ে ৪০৪ হ্যান্ডেল করতে হয় তা আমরা বিস্তারিত শিখেছি।

### Key Takeaways:

* **Parsing**: `app.use(express.urlencoded({ extended: false }))` ব্যবহার না করলে `req.body` পাওয়া যায় না।
* **Modular Routes**: `express.Router()` ব্যবহার করে প্রজেক্টের কোড `routes/` ফোল্ডারে গুছিয়ে রাখা হয়।
* **Path Filtering**: `app.use('/prefix', router)` দিয়ে যেকোনো রাউটারের আগে প্রিফিক্স যুক্ত করা যায়।
* **Clean Views**: HTML ফাইল `views/` ফোল্ডারে রেখে `res.sendFile(path.join(rootDir, 'views', 'file.html'))` পদ্ধতিতে পাঠাতে হয়।
* **Error Handling**: রাউটিং টেক্সটের শেষে `app.use()` বসিয়ে কাস্টম ৪০৪ পেজ ও `res.status(404)` হ্যান্ডেল করতে হয়।

---

## Quick Revision

### Important Methods / Functions

* `express.urlencoded()`: Form Data পার্স করে `req.body`\-তে রূপান্তর করে।
* `express.Router()`: মডিউলার রাউট তৈরির জন্য মিনি রাউটার ইন্সট্যান্স তৈরি করে।
* `res.sendFile()`: ক্লায়েন্টকে নির্দিষ্ট ডিস্ক ফাইল রেসপন্স করে।
* `path.join()`: ওএস-নিরপেক্ষ সঠিক ফাইল পাথ গঠন করে।
* `res.status()`: HTTP রেসপন্স স্ট্যাটাস কোড সেট করে।

### Important Terms

* **Parsing**: কাঁচা রিকোয়েস্ট ডেটাকে ব্যবহারযোগ্য অবজেক্টে রূপান্তর।
* **Router**: নির্দিষ্ট পাথ বা ফিচারের সাথে সম্পর্কিত রাউটের সমাহার।
* **Prefixing**: বেশ কয়েকটি রাউটের শুরুতে কমন ইউআরএল ফিক্স করা।
* **rootDir**: প্রজেক্টের মূল ডিরেক্টরি বের করার হেলパー পাথ।

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Guide](https://expressjs.com/)
- [MongoDB Manual](https://www.mongodb.com/docs/manual/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to Express.js Index](../../Express.js/README.md)
