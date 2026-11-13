# 05. MVC আর্কিটেকচার ও প্রজেক্ট স্ট্রাকচারিং (MVC Architecture Pattern & Clean Code Separation)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Express.js ট্র্যাক](../../Express.js/README.md)

---

## Introduction

আপনি কি কখনো ভেবে দেখেছেন কেন বড় বড় ওয়েব অ্যাপ্লিকেশনের কোডবেস হাজার হাজার লাইন হওয়া সত্ত্বেও এত নিখুঁত এবং সহজে মেইনটেইন করা যায়? আপনি যদি আপনার Express অ্যাপ্লিকেশনের সমস্ত Route, Database query এবং HTML Rendering একটি মাত্র ফাইলে গাদাগাদি করে রাখেন, তবে কোডটি দ্রুতই এক নিয়ন্ত্রণহীন "স্প্যাগেটি কোডে" (Spaghetti Code) পরিণত হবে। এই সমস্যার সমাধানই হলো **MVC Architecture**。

এই পাঠ শেষে আপনি যা যা শিখতে পারবেন:

* **Analyze:** MVC (Model-View-Controller) প্যাটার্ন এবং Separation of Concerns-এর আসল উদ্দেশ্য বিশ্লেষণ করতে পারবেন।
* **Implement:** Express অ্যাপ্লিকেশনে Route এবং Controller আলাদা করে ক্লিন কোড আর্কিটেকচার তৈরি করতে পারবেন।
* **Design:** ES6 Class ব্যবহার করে Model তৈরি করতে পারবেন এবং Asynchronous File System (`fs`) দিয়ে ডাটা স্থায়ীভাবে সংরক্ষণ (Persistence) করতে পারবেন।
* **Configure:** `nodemon.json` ফাইল কনফিগার করে ডাটা ফাইলের পরিবর্তনের কারণে অনাকাঙ্ক্ষিত সার্ভার রিস্টার্ট প্রতিরোধ করতে পারবেন।

## ক্লাসের সারসংক্ষেপ ও প্রধান বিষয়বস্তু

এই ক্লাসে প্রশান্ত স্যার কোড রিফ্যাক্টরিং এবং সফটওয়্যার আর্কিটেকচারের একটি অত্যন্ত গুরুত্বপূর্ণ ধাপ নিয়ে আলোচনা করেছেন। যখন একটি অ্যাপ্লিকেশনের সাইজ বড় হতে থাকে, তখন প্রতিটি কোডের দায়িত্ব আলাদাভাবে নির্ধারণ করা অত্যাবশ্যক হয়ে পড়ে。

**মূল যে বিষয়গুলো এই ক্লাসে কভার করা হয়েছে:**

1. **MVC Architecture Intro:** কোডে দায়িত্ব ভাগ করে দেওয়া বা *Separation of Concerns* এর ধারণা。
2. **Controllers Creation:** Routes ফাইল থেকে রিকোয়েস্ট হ্যান্ডলিং লজিক বের করে আলাদা `controllers/` ফোল্ডারে নিয়ে যাওয়া。
3. **Models Implementation:** `models/` ফোল্ডারে Class তৈরি করে ডাটা স্ট্রাকচার এবং বিজনেস লজিক হ্যান্ডেল করা。
4. **Asynchronous File Persistence:** ডাটা শুধুমাত্র মেমোরিতে (Array) না রেখে File System (`fs.readFile` ও `fs.writeFile`) এর মাধ্যমে স্থায়ীভাবে রাখা。
5. **Nodemon Configuration:** `nodemon.json` ফাইল তৈরি করে ডাটা ফাইল পরিবর্তনের সময় সার্ভার রিস্টার্ট হওয়া বন্ধ করা。
6. **Refactoring Views:** Views ফোল্ডারকে `host/` এবং `store/` ভাগে সুন্দরভাবে গুছিয়ে নেওয়া।

### 🧠 MVC Class Overview Mind Map
> [!TIP]
> **মূল কনসেপ্ট সারসংক্ষেপ (Core Concept Breakdown):**
> An overarching visual branching diagram that connects the major topics covered in this lesson including MVC concepts, Controllers, Models, Persistence, and Nodemon setup.

## MVC Architecture এবং Separation of Concerns

### What is it? (এটি কী?)

**MVC** বা **Model-View-Controller** হলো সফটওয়্যার ডিজাইনের একটি অত্যন্ত জনপ্রিয় **Architectural Pattern**। এটি সম্পূর্ণ অ্যাপ্লিকেশনকে তিনটি প্রধান ভাগে ভাগ করে দেয়: **Model**, **View**, এবং **Controller**。

### Why is it used? (কেন ব্যবহার করা হয়?)

সফটওয়্যার ডেভেলপমেন্টের প্রধান নিয়ম হলো **Separation of Concerns (SoC)**—অর্থাৎ প্রতিটি অংশের নির্দিষ্ট আলাদা দায়িত্ব থাকবে। রিকোয়েস্ট হ্যান্ডলিং, ডাটাবেস প্রসেসিং এবং UI রেন্ডারিং সব এক জায়গায় লিখে ফেললে কোডে বাগ বের করা ও নতুন ফিচার যোগ করা অসম্ভব হয়ে পড়ে। MVC ব্যবহার করলে কোড ক্লিন, রিডঅ্যাবল এবং সহজে মেইনটেইনেবল হয়。

### How does it work? (কীভাবে কাজ করে?)

* **Model (ডাটা ও লজিক):** ডাটা স্ট্রাকচার কেমন হবে, ডাটাবেস বা ফাইল থেকে ডাটা আনা ও সেভ করার মূল বিজনেস লজিক পরিচালনা করে。
* **View (ইউজার ইন্টারফেস):** স্ক্রিনে ডাটা কীভাবে প্রেজেন্ট বা প্রদর্শন করা হবে তা নিয়ন্ত্রণ করে (যেমন: EJS বা HTML ফাইল)।
* **Controller (মধ্যস্থতাকারী):** ক্লায়েন্ট বা ইউজারের কাছ থেকে রিকোয়েস্ট গ্রহণ করে, Model-এর সাথে কথা বলে ডাটা প্রসেস করে এবং প্রয়োজনীয় View রেন্ডার করে উত্তর ফেরত পাঠায়。

### Mental Model (মানসিকভাবে অনুধাবনের উপায়)

একটি রেস্টুরেন্টের কথা চিন্তা করুন:

* **View:** ডাইনিং এরিয়া এবং খাবারের সুন্দর মেনু কার্ড (যা ইউজার দেখতে পায়)।
* **Controller:** ওয়েটার (যে ইউজারের অর্ডার নেয়, কিচেনে পাঠায় এবং খাবার প্রস্তুত হলে টেবিলে এনে পরিবেশন করে)।
* **Model:** শেফ ও কিচেনের ইনভেন্টরি (যেখানে মূল খাবার তৈরি হয় এবং কাঁচামাল সংরক্ষিত থাকে)।

এখানে ওয়েটার রান্না করে না, শেফ কাস্টমারের অর্ডার সরাসরি টেবিলে গিয়ে নেয় না—সবাই নিজ নিজ কাজ নিখুঁতভাবে করে।

### 📊 MVC Architecture Data Flow
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> An infographic clearly illustrating the request-response lifecycle and communication pathways between User, Controller, Model, View, and Storage.

## Controllers deeply: Routes থেকে Logic আলাদা করা

### What is Controller? (কন্ট্রোলার কী?)

Controller হলো অ্যাপ্লিকেশনের "মস্তিষ্ক" বা ট্রাফিক পুলিশ। এর কাজ রিকোয়েস্টের ইউআরএল দেখে সঠিক বিজনেস লজিক কল করা এবং উপযুক্ত Response বা View ক্লায়েন্টকে ফেরত পাঠানো。

### Why &amp; How to Refactor?

পূর্বে আমাদের Route ফাইলে রিকোয়েস্ট হ্যান্ডলিং লজিক সরাসরি লেখা ছিল: `router.get('/add-home', (req, res, next) =&gt; { res.render(...) })`

আমরা এখন Route এর কাজ শুধুমাত্র **Routing** (কোন Path এ কোন ফাংশন চলবে তা ঠিক করা)-এ সীমাবদ্ধ রাখব এবং রিকোয়েস্ট প্রসেসিং এরিয়াকে `controllers/` ফোল্ডারে স্থানান্তর করব。

### Controller Functions Implementation

আমরা `controllers/hostController.js` এবং `controllers/storeController.js` ফাইল তৈরি করি。

```
// controllers/hostController.js
const Home = require('../models/home');

exports.getAddHome = (req, res, next) =&gt; {
  res.render('host/add-home', {
    pageTitle: 'Add Home',
    currentPage: 'add-home'
  });
};

exports.postAddHome = (req, res, next) =&gt; {
  const { houseName, price, location, rating, photoUrl } = req.body;
  const home = new Home(houseName, price, location, rating, photoUrl);
  home.save();
  res.render('host/home-added', {
    pageTitle: 'Home Added Successfully',
    currentPage: 'home-added'
  });
};

```

### Routes Clean Structure

এখন আমাদের Route ফাইলগুলো একদম ছোট ও পরিষ্কার হয়ে যায়:

```
// routes/hostRouter.js
const express = require('express');
const hostController = require('../controllers/hostController');
const hostRouter = express.Router();

hostRouter.get('/add-home', hostController.getAddHome);
hostRouter.post('/add-home', hostController.postAddHome);

module.exports = hostRouter;

```

### Common Mistakes (সাধারণ ভুল)

* Controller ফাংশন রেজিস্টার করার সময় ফাংশনটি কল করে ফেলা (যেমন: `hostController.getAddHome()`)—এখানে ফাংশনটি **কল না করে শুধুমাত্র তার Reference** পাঠাতে হবে।

### 📊 Refactoring Routes to Controllers
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> A structured step-by-step presentation detailing how raw Express routes are refactored into modular controller functions.

## Models &amp; Data Persistence (fs Module দ্বারা ডাটা সেভ করা)

### What is Model? (মডেল কী?)

Model হলো আপনার অ্যাপ্লিকেশনের ডাটা এবং ডাটা সম্পর্কিত মূল নিয়মনীতির (Business Logic) রূপরেখা। ডাটা কোথায় সেভ হবে (Array, JSON File, নাকি SQL/NoSQL Database) তা জানার দায়িত্ব একমাত্র Model-এর。

### Object-Oriented Blueprint (`class Home`)

আমরা ES6 Class ব্যবহার করে একটি Home Model ডিফাইন করি。

```
// models/home.js
const fs = require('fs');
const path = require('path');
const rootDir = require('../utils/pathUtil');

const homeDataPath = path.join(rootDir, 'data', 'homes.json');

module.exports = class Home {
  constructor(houseName, price, location, rating, photoUrl) {
    this.houseName = houseName;
    this.price = price;
    this.location = location;
    this.rating = rating;
    this.photoUrl = photoUrl;
  }

  // Instance Method: একক অবজেক্ট সেভ করা
  save() {
    Home.fetchAll((homes) =&gt; {
      homes.push(this);
      fs.writeFile(homeDataPath, JSON.stringify(homes), (error) =&gt; {
        if (error) console.log('File write error:', error);
      });
    });
  }

  // Static Method: সমস্ত ডাটা ফেচ করা
  static fetchAll(callback) {
    fs.readFile(homeDataPath, (err, fileContent) =&gt; {
      if (err) {
        callback([]);
      } else {
        callback(JSON.parse(fileContent));
      }
    });
  }
};

```

### Instance Method vs Static Method

1. **Instance Method (** **save()** **):** এই পদ্ধতিটি কাজ করার জন্য ক্লাসের একটি নির্দিষ্ট ইন্সট্যান্স বা অবজেক্ট দরকার হয় (যেমন: `const myHome = new Home(...); myHome.save();`)।
2. **Static Method (** **Home.fetchAll()** **):** কোনো অবজেক্ট তৈরি না করেই সরাসরি ক্লাসের নাম ধরে এই পদ্ধতি কল করা যায় (যেমন: `Home.fetchAll(callback)`)। এটি পুরো ডাটা লিস্ট রিট্রিভ করার জন্য আদর্শ。

### Asynchronous Callback-এর গুরুত্ব

Node.js এ `fs.readFile` একটি Asynchronous অপারেশন। ফাইল পড়তে সময় লাগে, তাই `fetchAll` সরাসরি `return` দিয়ে ডাটা পাঠাতে পারে না। যদি রিটার্ন করার চেষ্টা করা হয়, তবে ইউজার `undefined` পাবে। এই সমস্যা মেটাতে **Callback Function** ব্যবহার করতে হয়, যাতে ফাইল পড়া শেষ হলে কলব্যাক ফাংশনটি ডাটা সহ এক্সিকিউট হয়।

### 📌 Asynchronous Model Data Fetching
> A conversational audio walkthrough explaining why synchronous returns fail in asynchronous file operations and how callback architecture solves it.

## Important Modules, Tools &amp; nodemon.json Setup

### Key Core Modules

1. **fs** **(File System):** ফাইল তৈরি, পড়া, এডিট ও ডিলিট করার জন্য Node.js এর বিল্ট-ইন মডিউল。
  * `fs.readFile(path, callback)`: ফাইল রিড করার জন্য。
  * `fs.writeFile(path, data, callback)`: ফাইলে ডাটা রাইট করার জন্য。
2. **path** **:** বিভিন্ন অপারেটিং সিস্টেমে ফাইলের ডিরেক্টরি পাথ সঠিকভাবে যুক্ত করার মডিউল (`path.join()`)。

### The Problem with Nodemon &amp; Data Files

ডিফল্টভাবে `nodemon` প্রজেক্টের যেকোনো ফাইলে পরিবর্তন দেখলেই পুরো সার্ভার রিস্টার্ট করে দেয়। যখন আমরা `homes.json` ফাইলে নতুন ডাটা সেভ করি, তখন Nodemon ফাইল চেঞ্জ শনাক্ত করে সার্ভার রিস্টার্ট করে দিত। ফলে ইন-মেমোরি স্টেট মুছে যেত এবং অনাকাঙ্ক্ষিত বাগ তৈরি হতো。

### The Solution: `nodemon.json` Configuration

প্রজেক্টের রুট ডিরেক্টরিতে `nodemon.json` কনফিগারেশন ফাইল তৈরি করে আমরা Nodemon-কে নির্দিষ্ট ফোল্ডার বা ফাইল ইগনোর (Ignore) করতে বলতে পারি。

```
{
  "watch": ["."],
  "ext": "js,json,ejs",
  "ignore": ["data/*", "node_modules/*"],
  "exec": "node app.js"
}

```

* **watch** **:** কোন ডিরেক্টরি নজরদারিতে রাখবে (এখানে পুরো প্রজেক্ট)।
* **ext** **:** কোন কোন এক্সটেনশনের ফাইল পরিবর্তিত হলে রিস্টার্ট হবে (`.js`, `.json`, `.ejs`)।
* **ignore** **:** `data/` ফোল্ডারের ভেতরের ফাইলের ডাটা পাল্টালে সার্ভার রিস্টার্ট হবে না।

### 💡 Nodemon and FS Terminology Flashcards
> [!IMPORTANT]
> **কুইক রিভিশন পয়েন্টস (Quick Revision Points):**
> Flashcards covering essential configuration syntax and node module methods such as fs.readFile, fs.writeFile, path.join, and nodemon.json properties.

## Important Syntax, Differences &amp; Common Mistakes

### Key Syntax &amp; Keywords

* **class** **:** ES6 এর মাধ্যমে অবজেক্টের ব্লুপ্রিন্ট তৈরির নিয়ম。
* **constructor** **:** অবজেক্ট তৈরির সময় প্রোপার্টিগুলো ইনিশিয়ালাইজ করার স্পেশাল মেথড。
* **this** **:** ক্লাসের বর্তমান ইন্সট্যান্সকে রেফার করে。
* **JSON.stringify()** **:** JavaScript অবজেক্ট বা অ্যারেকে JSON স্ট্রিং-এ রূপান্তর করে (ফাইলে লেখার উপযোগী করার জন্য)।
* **JSON.parse()** **:** JSON স্ট্রিংকে পুনরায় JavaScript অবজেক্ট বা এ্যারেতে রূপান্তর করে。

### Important Differences (পার্থক্য)

| বিষয়                  | Route (রাউট)              | Controller (কন্ট্রোলার)                | Model (মডেল)                            |
| --------------------- | ------------------------- | -------------------------------------- | --------------------------------------- |
| **মূল কাজ**           | Endpoints/URLs ডিফাইন করা | রিকোয়েস্ট ট্রাফিক পরিচালনা ও রেন্ডারিং | ডাটা প্রসেস ও ডাটাবেস/ফাইল হ্যান্ডলিং   |
| **কাদের সাথে সংযোগ?** | Controller-এর সাথে        | Route, Model ও View-এর সাথে            | শুধুমাত্র Controller বা Storage-এর সাথে |
| **উদাহরণ**            | `router.get('/homes')`    | `exports.getHomes = ...`               | `class Home { save() {...} }`           |

### Common Beginner Mistakes (শিক্ষানবিসদের সাধারণ ভুলসমূহ)

1. **Asynchronous Code-এ Sync ভাবা:** `fs.readFile` থেকে সরাসরি মান রিটার্ন করার চেষ্টা করা। কলব্যাক বা প্রমিস ব্যবহার না করলে ডাটা পাওয়া যাবে না。
2. **Fat Controllers:** কন্ট্রোলারের ভেতরে সরাসরি ফাইল সিস্টেমে ডাটা পুশ করা বা ডাটাবেস কুয়েরি লেখা। ডাটা সম্পর্কিত কাজ সব সময় Model-এ থাকা উচিত。
3. **Nodemon Loop:** ডাটা ফাইল সেভ করার পর নোডমন বারবার রিস্টার্ট হওয়া। `nodemon.json` ব্যবহার করে ইগনোর লিস্টে ডাটা ফোল্ডার যুক্ত না করা।

subscriptions

### 📌 Common MVC and Asynchronous Coding Mistakes Overview
> A dynamic summary video explaining structural bad practices like Fat Controllers, synchronous returns on async file I/O, and Nodemon watch loops.

## Summary

এই ক্লাসের মূল সারমর্ম হলো একটি পেশাদার এবং সুসংগঠিত Express অ্যাপ্লিকেশন তৈরি করার জন্য কোডকে MVC 패턴 অনুযায়ী ভাগ করা জরুরি। রিকোয়েস্ট রাউটিং, রেসপন্স রেন্ডারিং এবং ডাটা প্রসেসিং আলাদা করার ফলে কোড স্কেলেবল হয়।

### Key Takeaways

* **MVC Architecture:** Separation of Concerns বজায় রাখতে সাহায্য করে。
* **Routes:** কেবল URL পথ নির্দিষ্ট করে এবং রিকোয়েস্টকে Controller-এ পাঠায়。
* **Controller:** রিকোয়েস্ট রিসিভ করে, Model থেকে ডাটা নেয় এবং View রেন্ডার করে。
* **Model:** ক্লাসের মাধ্যমে ডাটা অবজেক্ট তৈরি করে এবং `fs` মডিউল ব্যবহার করে ফাইল সিস্টেমে স্থায়িত্ব দান করে。
* **Asynchronous Callbacks:** ফাইল রিড বা ডাটাবেস থেকে পাওয়া ডাটা প্রসেস করতে Asynchronous Callbacks প্রয়োজন。
* **nodemon.json** **:** ডাটা ফোল্ডারের পরিবর্তনের ফলে সার্ভারের বারংবার রিস্টার্ট রোধ করে。

---

## Quick Revision

### Important Methods / Functions

* `fs.readFile(path, callback)`: ফাইল থেকে ডাটা অ্যাসিঙ্ক্রোনাসভাবে পড়ে。
* `fs.writeFile(path, data, callback)`: ফাইলে ডাটা অ্যাসিঙ্ক্রোনাসভাবে লেখে。
* `JSON.stringify(obj)`: অবজেক্টকে JSON স্ট্রিং বানায়。
* `JSON.parse(jsonString)`: JSON স্ট্রিংকে JavaScript অবজেক্ট বানায়。
* `path.join(...)`: একাধিক পাথ সেগমেন্টকে সঠিকভাবে যুক্ত করে。

### Important Modules / Packages

* `fs`: File handling-এর বিল্ট-ইন মডিউল。
* `path`: File path পরিচালনার বিল্ট-ইন মডিউল。
* `express.Router()`: মডুলার রাউট তৈরির এক্সপ্রেস মেথড。

### Important Terms

* **Separation of Concerns:** অ্যাপ্লিকেশনের প্রতিটি কোড ব্লককে তার নিজস্ব কাজটুকু করার দায়িত্ব দেওয়া。
* **Data Persistence:** মেমোরির ডাটা যেন সার্ভার রিস্টার্ট হলেও হারিয়ে না যায়, বরং ফাইল বা ডাটাবেসে সেভ থাকে。
* **Static Method:** ক্লাসের কোনো নতুন অবজেক্ট তৈরি না করে সরাসরি ক্লাস থেকে যে মেথড ব্যবহার করা যায়。

### What I Must Remember

* কখনো Route ফাইল বা Controller-এ সরাসরি ডাটাবেস কুয়েরি বা ফাইল রিড/রাইটের কাজ করবেন না。
* Asynchronous ফাংশন ব্যবহারের সময় রিটার্ন স্টেটমেন্টের বদলে Callbacks, Promises, বা Async/Await ব্যবহার নিশ্চিত করুন。

---

## Practice Questions

1. MVC Architecture ব্যবহারের প্রধান সুবিধা কী?
2. Controller এবং Route-এর মধ্যে মূল পার্থক্য ব্যাখ্যা করুন。
3. Model ক্লাসে static মেথড কেন ব্যবহার করা হয়? `fetchAll()` মেথডে কেন static ব্যবহার করা হলো?
4. `fs.readFile` থেকে সরাসরি ডাটা return করলে সমস্যা কী হয় এবং কীভাবে Callback এই সমস্যার সমাধান করে?
5. `nodemon.json` ফাইলের প্রয়োজনীয়তা কী? এটি কীভাবে অ্যাপ্লিকেশনের পারফরম্যান্স এবং ডেভেলপার এক্সপেরিয়েন্স উন্নত করে?

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Guide](https://expressjs.com/)
- [MongoDB Manual](https://www.mongodb.com/docs/manual/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to Express.js Index](../../Express.js/README.md)
