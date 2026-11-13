# 11. ক্লায়েন্ট-সাইড Asynchronous Requests ও JSON REST API (Client-side Async Requests, Fetch API & JSON APIs)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Express.js ট্র্যাক](../../Express.js/README.md)

---

## ভূমিকা

আপনার কি কখনো মনে হয়েছে, ফেসবুক বা ইউটিউবে নতুন পোস্ট বা ভিডিও দেখার সময় কেন পুরো ওয়েব পেজটি নতুন করে লোড বা রিফ্রেশ হয় না, অথচ নতুন তথ্য স্ক্রিনে চলে আসে?

এই অলৌকিক বলে মনে হওয়া প্রযুক্তিটির পেছনে রয়েছে **অ্যাসিঙ্ক্রোনাস রিকোয়েস্ট (Asynchronous Requests)** এবং **REST API**। পূর্বে আমরা ব্যাকএন্ডের ভেতরেই EJS টেমপ্লেট বা ভিউ ব্যবহার করে ইউজার ইন্টারফেস (UI) তৈরি করতাম। কিন্তু আধুনিক ওয়েব ডেভেলপমেন্টে ব্যাকএন্ড এবং ফ্রন্টএন্ডকে সম্পূর্ণ আলাদা বা ডি-কাপল্ড (Decoupled) করে তৈরি করা হয়। এই পাঠে আমরা শিখব কীভাবে ব্যাকএন্ডকে শুধু ডেটা আদান-প্রদানের 'পিয়োর ব্যাকএন্ড' হিসেবে গড়ে তুলতে হয় এবং React ফ্রন্টএন্ডের সাথে Node.js ব্যাকএন্ড যুক্ত করে কীভাবে একটি সম্পূর্ণ ফুল-স্ট্যাক অ্যাপ্লিকেশন তৈরি করা যায়।

এই পাঠ শেষে আপনি যা করতে পারবেন:

* অ্যাসিঙ্ক্রোনাস 네트워크 রিকোয়েস্ট এবং সিঙ্গেল পেজ অ্যাপ্লিকেশন (SPA)-এর কার্যপ্রণালী ব্যাখ্যা করতে পারবেন।
* মনোলিথিক (Monolithic) এবং ডি-কাপল্ড (Decoupled) আর্কিটেকচারের সুবিধা ও পার্থক্য বিশ্লেষণ করতে পারবেন।
* REST API-এর ৬টি কোর কনসেপ্ট এবং HTTP মেথডগুলোর সঠিক ব্যবহার বুঝতে পারবেন।
* Express.js, Mongoose এবং CORS প্যাকেজ ব্যবহার করে ক্রুড (CRUD) RESTful API তৈরি করতে পারবেন।
* React ফ্রন্টএন্ডের সার্ভিস লেয়ারে ডেটা ম্যাপিং করে ব্যাকএন্ডের সাথে নিরবচ্ছিন্ন সংযোগ স্থাপন করতে পারবেন।

## ১. এই ক্লাসে কী শেখানো হয়েছে?

এই দীর্ঘ ব্যবহারিক ক্লাসে Prashant Sir ব্যাকএন্ড ডেভেলপমেন্টের একটি অত্যন্ত গুরুত্বপূর্ণ মাইলফলক অতিক্রম করেছেন। নিচে মূল টপিকগুলোর তালিকা দেওয়া হলো যা আমরা বিস্তারিত আলোচনা করব:

1. **অ্যাসিঙ্ক্রোনাস নেটওয়ার্ক রিকোয়েস্ট (Async Requests &amp; SPA):** পেজ রিফ্রেশ না করে কীভাবে ব্যাকএন্ড থেকে ডেটা এসে UI আপডেট করে।
2. **ডি-কাপলিং (Decoupled Architecture):** ফ্রন্টএন্ড এবং ব্যাকএন্ড আলাদা প্রজেক্ট ও আলাদা সার্ভারে রাখার কারণ এবং সুবিধা।
3. **REST API-এর কোর কনসেপ্ট:** Statelessness, Uniform Interface, Cacheability সহ ইন্টারেক্টিভ ইন্টারভিউ লেভেলের ব্যাখ্যা।
4. **HTTP মেথডস ও রাউটিং:** GET, POST, PUT, DELETE, PATCH এবং ডায়নামিক পাথ বা রাউট প্যারামিটার (`:id`)।
5. **Mongoose Schema &amp; Model গঠন:** ToDo অ্যাপের জন্য টাইপ ডেফিনেশন, ডেফল্ট ভ্যালু এবং `timestamps: true` এর সঠিক ব্যবহার।
6. **CORS ও express.json Middleware:** পোর্ট ৩০০০/৫১৭৩ এর মধ্যকার সিকিউরিটি ব্লক (CORS) সমাধান এবং JSON রিকোয়েস্ট বডি পার্স করা।
7. **React-এর সাথে Node.js API সংযোগ:** `fetch()` API, সার্ভিস লেয়ার ডেটা ম্যাপিং (`serverItem` থেকে `localItem`) এবং স্টেট আপডেট।

### 🧠 লেকচার সংক্ষেপণ মাইন্ড ম্যাপ
> [!TIP]
> **মূল কনসেপ্ট সারসংক্ষেপ (Core Concept Breakdown):**
> REST API, ডি-কাপল্ড আর্কিটেকচার এবং ফুল-স্ট্যাক টু-ডু অ্যাপ তৈরির প্রধান টপিকগুলোর একটি ভিজ্যুয়াল ওভারভিউ।

## ২. Topic অনুযায়ী বিস্তারিত পাঠ (পর্ব ১): Async Request, Decoupling ও REST API

### ক. অ্যাসিঙ্ক্রোনাস নেটওয়ার্ক রিকোয়েস্ট (Asynchronous Network Requests) কী?

**সহজ পরিচয়:** সাধারণ ওয়েবসাইটে কোনো বোতামে ক্লিক করলে ব্রাউজার পুরো পেজ রিফ্রেশ করে নতুন HTML লোড করে। কিন্তু অ্যাসিঙ্ক্রোনাস রিকোয়েস্টের মাধ্যমে ব্রাউজার ব্যাকগ্রাউন্ডে সার্ভারের সাথে কথা বলে এবং কেবল প্রয়োজনীয় ডেটা নিয়ে আসে। পেজ রিফ্রেশ হওয়ার কোনো প্রয়োজন পড়ে না।

**কেন প্রয়োজন:** Single Page Application (SPA)—যেমন React বা Next.js-এ তৈরি অ্যাপগুলোতে শুরুতে ব্যাকএন্ড থেকে পুরো UI টেমপ্লেট বা স্ক্রিপ্ট ব্রাউজারে চলে আসে। এরপর যখনই নতুন ডেটা (যেমন: র্যাংক প্রেডিক্টর সংখ্যা, কোর্সের তালিকা) দরকার হয়, ব্রাউজার ব্যাকগ্রাউন্ডে একটি রিকোয়েস্ট পাঠায়। এটি ইউজার এক্সপেরিয়েন্সকে অনেক দ্রুত ও স্মুথ করে তোলে।

**কীভাবে কাজ করে:**

1. ব্রাউজার/ক্লায়েন্ট ব্যাকএন্ড সার্ভারে একটি JSON রিকোয়েস্ট পাঠায়।
2. সার্ভার কেবল র ডেটা (Raw Data) JSON আকারে ফেরত পাঠায়।
3. ক্লায়েন্ট সেই ডেটা গ্রহণ করে UI-এর নির্দিষ্ট অংশ ডাইনামিকভাবে পরিবর্তন করে।

---

### খ. ফ্রন্টএন্ড এবং ব্যাকএন্ড ডি-কাপলিং (Decoupled Architecture)

**সহজ পরিচয়:** পূর্বে আমরা একই প্রজেক্টের ভেতর `views` ফোল্ডারে EJS ফাইল রাখতাম, যা Server-Side Rendering (SSR) করত। ডি-কাপলিং মানে হলো ব্যাকএন্ড অ্যাপকে ফ্রন্টএন্ড থেকে সম্পূর্ণ আলাদা করে ফেলা। ব্যাকএন্ডের কাজ শুধুই ডেটা ও লজিক হ্যান্ডেল করা (Pure Backend), ভিউ বা UI তৈরি করা নয়।

**কেন ডি-কাপলিং করব? (সুবিধাসমূহ):**

* **স্বাধীন স্কেলিং (Independent Scaling):** ব্যাকএন্ড সার্ভারে প্রসেসিং বেশি হলে সেখানে ১০-১৫টি সার্ভার চালানো যায়, যেখানে ফ্রন্টএন্ডের জন্য মাত্র ২-৩টি সার্ভারই যথেষ্ট।
* **পুনর্ব্যবহারযোগ্যতা (Reusability):** একটি পিয়োর REST API ব্যাকএন্ড তৈরি করলে সেটি দিয়ে আপনি Web App, Mobile App (React Native/Flutter), এবং Desktop App—সবগুলোতেই একই ব্যাকএন্ড ব্যবহার করতে পারবেন।
* **টিমের বিভাজন (Team Separation):** ফ্রন্টএন্ড টিম এবং ব্যাকএন্ড টিম একে অপরের কাজে বাধা না দিয়ে স্বাধীনভাবে কাজ করতে পারে।

---

### গ. REST API এবং এর কোর কনসেপ্ট

**REST (Representational State Transfer) API** হলো এমন একটি আর্কিটেকচারাল স্টাইল বা নিয়মনীতি, যা এইচটিটিপি (HTTP) প্রোটোকল ব্যবহার করে ক্লায়েন্ট ও সার্ভারের মধ্যে ডেটা আদান-প্রদান নিশ্চিত করে।

**REST API-এর কোর কনসেপ্টসমূহ:**

1. **Statelessness (স্ট্যাটলেসনেস):** সার্ভার ক্লায়েন্টের কোনো আগের সেশন বা স্টেট মনে রাখে না। প্রতিটি রিকোয়েস্টে ক্লায়েন্টকে তার পরিচয় এবং প্রয়োজনীয় সমস্ত ডেটা নতুন করে পাঠাতে হয়।
2. **Uniform Interface:** প্রতিটি রিসোর্স (যেমন: সার্ভে, কোর্স, ইউজার) পাওয়ার জন্য স্ট্যান্ডার্ড HTTP মেথড (GET, POST, PUT, DELETE) এবং সুনির্দিষ্ট URL ব্যবহার করা হয়।
3. **Client-Server Separation:** ক্লায়েন্ট (UI) এবং সার্ভার (Data &amp; Logic) সম্পূর্ণ আলাদা থাকে।
4. **Cacheability (ক্যাশেবিলিটি):** সার্ভারের রেসপন্সে উল্লেখ থাকে যে উক্ত ডেটা ব্রাউজার ক্যাশ করে রাখতে পারবে কিনা, যাতে বারবার সার্ভারে রিকোয়েস্ট পাঠাতে না হয়।
5. **Layered System:** ক্লায়েন্ট ও মূল সার্ভারের মাঝে মিডলওয়্যার, লোড ব্যালেন্সার বা সিকিউরিটি লেয়ার থাকতে পারে, যা ক্লায়েন্ট সরাসরি জানতে পারে না।

### 📊 মনোলিথিক বনাম ডি-কাপল্ড REST API আর্কিটেকচার
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> সার্ভার-সাইড রেন্ডারিং এবং পিয়োর ডি-কাপল্ড REST API-এর কাজের প্রক্রিয়া ও স্কেলিং ব্যবস্থার তুলনামূলক চিত্র।

## ২. Topic অনুযায়ী বিস্তারিত পাঠ (পর্ব ২): ToDo Backend API এবং Frontend Integration

### ক. ব্যাকএন্ডে Mongoose Schema ও Model তৈরি

একটি ToDo অ্যাপ তৈরির জন্য প্রথমে MongoDB-তে ডেটা কীভাবে সংরক্ষিত হবে, তার জন্য Schema তৈরি করতে হয়।

```
// models/ToDoItem.js
const mongoose = require('mongoose');

const toDoItemSchema = new mongoose.Schema({
  task: { 
    type: String, 
    required: true 
  },
  date: { 
    type: Date 
  },
  completed: { 
    type: Boolean, 
    default: false 
  }
}, { 
  timestamps: true // createdAt এবং updatedAt স্বয়ংক্রিয়ভাবে তৈরি হবে
});

module.exports = mongoose.model('ToDoItem', toDoItemSchema);

```

&gt; **বিশেষ সতর্কবার্তা:** `timestamps: true` প্রপার্টিটি কোনো ফিল্ডের ভেতরে লেখা যাবে না। এটি Schema অবজেক্টের দ্বিতীয় আর্গুমেন্ট (Options) হিসেবে পাস করতে হয়।

---

### খ. Express Controllers এবং Routers গঠন

আমরা ৪টি মৌলিক CRUD অপারেশনের জন্য ব্যাকএন্ডে চার ধরনের মেথড ও রাউট লিখব:

```
// controllers/toDoItemsController.js
const ToDoItem = require('../models/ToDoItem');

// ১. Create: নতুন আইটেম তৈরি
exports.createToDoItem = async (req, res) =&gt; {
  try {
    const { task, date } = req.body;
    const newItem = new ToDoItem({ task, date });
    await newItem.save();
    res.status(201).json(newItem); // JSON রেসপন্স পাঠানো হচ্ছে
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
};

// ২. Read: সব আইটেম ফেচ করা
exports.getToDoItems = async (req, res) =&gt; {
  const items = await ToDoItem.find();
  res.json(items);
};

// ৩. Delete: আইডি দিয়ে আইটেম ডিলিট করা
exports.deleteToDoItem = async (req, res) =&gt; {
  const { id } = req.params; // ডাইনামিক URL থেকে আইডি নেওয়া
  await ToDoItem.findByIdAndDelete(id);
  res.json({ _id: id });
};

// ৪. Update: কমপ্লিট হিসেবে মার্ক করা (PUT)
exports.markCompleted = async (req, res) =&gt; {
  const { id } = req.params;
  const item = await ToDoItem.findById(id);
  if (item) {
    item.completed = true;
    await item.save();
  }
  res.json(item);
};

```

---

### গ. CORS এবং express.json Middleware-এর গুরুত্ব

`app.js` ফাইলে দুটি অত্যন্ত গুরুত্বপূর্ণ মিডলওয়্যার যুক্ত করতে হয়:

```
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors()); // CORS অন করা হলো
app.use(express.json()); // Incoming JSON Body পার্স করার জন্য

```

1. **express.json()** **:** ক্লায়েন্ট যখন JSON রিকোয়েস্ট পাঠায়, এটি ব্যাকএন্ডে `req.body`\-এর ভেতরে অবজেক্ট আকারে অ্যাক্সেস নিশ্চিত করে।
2. **cors()** **:** ব্রাউজার নিরাপত্তার কারণে একটি পোর্ট (যেমন React `http://localhost:5173`) থেকে অন্য পোর্টে (Node.js `http://localhost:3001`) সরাসরি রিকোয়েস্ট পাঠানো ব্লক করে। `cors()` এটি সমাধান করে।

---

### ঘ. React-এ Service Layer এবং Data Mapping

ব্যাকএন্ডের ডেটা ফিল্ডের নাম (যেমন: `_id`, `task`, `date`) এবং ফ্রন্টএন্ডের স্টেট প্রপার্টির নাম (যেমন: `id`, `name`, `dueDate`) ভিন্ন হতে পারে। সার্ভিস লেয়ারের মাধ্যমে এই ডেটা ম্যাপিং করা হয়:

```
// services/itemsService.js
const API_URL = 'http://localhost:3001/api/todo';

// সার্ভারের ডেটাকে ফ্রন্টএন্ড লোকাল ফরম্যাটে ম্যাপিং
const mapServerItemToLocalItem = (serverItem) =&gt; {
  return {
    id: serverItem._id,
    name: serverItem.task,
    dueDate: serverItem.date,
    completed: serverItem.completed
  };
};

export const getItemsFromServer = async () =&gt; {
  const response = await fetch(API_URL);
  const data = await response.json(); // প্রমিস হ্যান্ডেল করার জন্য await প্রয়োজন
  return data.map(mapServerItemToLocalItem);
};

```

### 📊 ফুল-স্ট্যাক ToDo অ্যাপের ডাটা ফ্লো ও আর্কিটেকচার
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> React UI থেকে সার্ভিস লেয়ার, Express Router, Controller এবং MongoDB পর্যন্ত ডেটার প্রবাহের প্রতিটি ধাপের স্লাইড উপস্থাপনা।

## ৩. Important Methods, Functions, Properties ও Packages

এই ক্লাসে ব্যবহৃত মূল মেথড, ফাংশন ও প্যাকেজগুলোর বিস্তারিত তালিকা ও কাজ নিচে দেওয়া হলো:

### ১. HTTP Methods (এইচটিটিপি মেথডসমূহ)

* **GET** **:** সার্ভার থেকে নির্দিষ্ট ডেটা পড়ার জন্য ব্যবহৃত হয়। কোনো ডেটা পরিবর্তন করে না।
* **POST** **:** নতুন কোনো রিসোর্স বা ডেটা সার্ভারে পাঠানোর জন্য ব্যবহৃত হয়।
* **PUT** **:** বিদ্যমান ডেটাকে সম্পূর্ণ আপডেট বা প্রতিস্থাপন করার জন্য ব্যবহৃত হয়।
* **DELETE** **:** নির্দিষ্ট রিসোর্স সার্ভার থেকে মুছে ফেলার জন্য ব্যবহৃত হয়।
* **PATCH** **:** কোনো ডেটার আংশিক (Partial Update) পরিবর্তনের জন্য ব্যবহৃত হয়।

---

### ২. Express &amp; Mongoose Functions

* **res.json(data)** **:** ব্রাউজারে সুন্দরভাবে JSON ফরম্যাটে রেসপন্স পাঠায় এবং উপযুক্ত `Content-Type: application/json` হেডার সেট করে।
* **res.status(code)** **:** HTTP স্ট্যাটাস কোড (যেমন: 200 OK, 201 Created, 404 Not Found, 500 Server Error) নির্ধারণ করে।
* **ToDoItem.find()** **:** মঙ্গোডিবি কালেকশনের সমস্ত ডকুমেন্ট এরে (Array) আকারে ফেরত দেয়।
* **ToDoItem.findByIdAndDelete(id)** **:** প্রাইমারি `_id` ম্যাচ করে সরাসরি ডাটাবেজ থেকে সেই আইটেমটি মুছে দেয়।

---

### ৩. NPM Packages

* **cors** **:** Cross-Origin Resource Sharing সেটআপের জন্য প্রয়োজনীয় এক্সপ্রেস মিডলওয়্যার।
* **express** **:** ওয়েব সার্ভার এবং RESTful API তৈরির জন্য নোড.জেএস ফ্রেমওয়ার্ক।

### 💡 REST API এবং Mongoose শব্দাবলী ফ্ল্যাশকার্ড
> [!IMPORTANT]
> **কুইক রিভিশন পয়েন্টস (Quick Revision Points):**
> HTTP মেথড, স্ট্যাটাস কোড এবং এক্সপ্রেস ফাংশনগুলোর দ্রুত রিভিশনের জন্য একটি ফ্ল্যাশকার্ড সেট।

## ৪. Important Syntax, Conceptual Differences ও Common Mistakes

### গুরুত্বপূর্ণ ধারণাগত পার্থক্য

1. **res.send()** **বনাম** **res.json()** **:**
  * `res.send()` যেকোনো টেক্সট বা HTML পাঠাতে পারে।
  * `res.json()` বিশেষত্ব হলো এটি জাভাস্ক্রিপ্ট অবজেক্টকে সরাসরি JSON স্ট্রিঙে রূপান্তর করে এবং হেডার হিসেবে `application/json` সেট করে।
2. **req.params** **বনাম** **req.body** **:**
  * `req.params`: URL-এর ডাইনামিক অংশ (যেমন: `/api/todo/:id`) থেকে আইডি বা তথ্য সংগ্রহ করে।
  * `req.body`: রিকোয়েস্টের ভেতর পাস করা JSON পেলোড বা ফরম ডেটা থেকে তথ্য গ্রহণ করে (এর জন্য `express.json()` লাগে)।

---

### সাধারণ ভুল এবং তা এড়ানোর উপায় (Common Mistakes)

* **ভুল ১:** **res.json()** **রিড করার সময়** **await** **বাদ দেওয়া।**
  * *ভুল কোড:* `const data = response.json();` (এটি একটি পেন্ডিং প্রমিস ফেরত দেবে)।
  * *সঠিক কোড:* `const data = await response.json();`
* **ভুল ২: Mongoose-এ Schema Options সঠিক জায়গায় না দেওয়া।**
  * *ভুল কোড:* `{ task: String, timestamps: true }` (ফিল্ডের ভেতরে লেখা)।
  * *সঠিক কোড:* `new mongoose.Schema({ task: String }, { timestamps: true });` (Schema-এর দ্বিতীয় আর্গুমেন্ট হিসেবে পাস করা)।
* **ভুল ৩: CORS এরর।**
  * ব্যাকএন্ডে `app.use(cors())` না লিখলে ব্রাউজার থেকে রিকোয়েস্ট পাঠালে ব্লক হয়ে যাবে এবং কনসোলে 'Access-Control-Allow-Origin' এরর দেখাবে।
* **ভুল ৪:** **express.json()** **ব্যবহার না করা।**
  * ক্লায়েন্ট থেকে JSON ডাটা পাঠালে ব্যাকএন্ডে `req.body`\-এর মান `undefined` দেখাবে যদি অ্যাপে `app.use(express.json())` যোগ করা না থাকে।

### 📊 সাধারণ কোডিং ভুল এবং সমাধান নির্দেশিকা
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> CORS এরর, প্রমিস সমস্যা এবং Express Middleware-এর সাধারণ ভুলগুলোর ভিজ্যুয়াল চেকলিস্ট।

## ৫. সারসংক্ষেপ ও স্ব-মূল্যায়ন

আজকের এই পাঠে আমরা সফলভাবে বুঝেছি কীভাবে একটি পিয়োর ব্যাকএন্ড REST API তৈরি করতে হয় এবং ফ্রন্টএন্ডকে সম্পূর্ণ আলাদা রেখে অ্যাসিঙ্ক্রোনাস রিকোয়েস্টের মাধ্যমে ডেটা আদান-প্রদান করতে হয়।

**মূল শিক্ষণীয় দিকসমূহ (Key Takeaways):**

* **পিয়োর ব্যাকএন্ড:** আধুনিক ব্যাকএন্ড সার্ভারে কোনো HTML/Views ফোল্ডার থাকে না; এটি শুধুই JSON ডেটা লেনদেন করে।
* **ডি-কাপল্ড সুবিধা:** ফ্রন্টএন্ড ও ব্যাকএন্ড আলাদা থাকলে স্বতন্ত্র স্কেলিং এবং মাল্টি-প্ল্যাটফর্ম সুবিধা পাওয়া যায়।
* **REST নীতিনির্দেশনা:** ক্লায়েন্ট-সার্ভার সেপারেশন এবং স্ট্যাটলেসনেস মেনেই সকল API রাউট তৈরি করা আবশ্যক।
* **সার্ভিস লেয়ার:** ফ্রন্টএন্ডে সরাসরি ব্যাকএন্ডের আইডি ফিল্ড ব্যবহার না করে সার্ভিস লেয়ারের মাধ্যমে ডাটা ম্যাপিং করা একটি আদর্শ প্র্যাকটিস।

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Guide](https://expressjs.com/)
- [MongoDB Manual](https://www.mongodb.com/docs/manual/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to Express.js Index](../../Express.js/README.md)
