# 04. Template Engines, EJS ও ডায়নামিক HTML রেন্ডারিং (Template Engines, EJS & Dynamic UI)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Express.js ট্র্যাক](../../Express.js/README.md)

---

## Introduction

আপনি কি কখনো ভেবে দেখেছেন—আপনি যখন Gmail বা কোনো স্টক মার্কেট ওয়েবসাইট খোলেন, তখন কীভাবে একই ওয়েবপেজের লেআউটে লাখ লাখ মানুষের জন্য আলাদা আলাদা ডেটা এবং রিয়েল-টাইম আপডেট ভেসে ওঠে? কীভাবে একটিমাত্র ওয়েবপেজ প্রতিটি ইউজারের জন্য পুরোপুরি ব্যক্তিগত বা 'Dynamic' হয়ে যায়?

By the end of this lesson, you will be able to:

* Static UI এবং Dynamic UI-এর মূল পার্থক্য ব্যাখ্যা করতে পারবেন এবং Dynamic UI-এর প্রয়োজনীয়তা বুঝতে পারবেন।
* Node.js ও Express অ্যাপ্লিকেশনে EJS (Embedded JavaScript) টেমপ্লেট ইঞ্জিন সেটআপ ও কনফিগার করতে পারবেন।
* `res.render()` পদ্ধতির মাধ্যমে ব্যাকএন্ড সার্ভার থেকে ইজেএস ভিউতে ডাইনামিক ডেটা সফলভাবে পাস ও রেন্ডার করতে পারবেন।
* EJS Partials ব্যবহার করে পুনরাবৃত্তিমূলক ইউআই কোড (যেমন Header, Navigation Bar, Head) কোড ডুপ্লিকেশন ছাড়া রিইউজ করতে পারবেন।
* রাউটের মাধ্যমে অ্যাক্টিভ নেভিগেশন স্টেট (Active Navigation Tab) ডাইনামিকালি হ্যান্ডেল করতে পারবেন।

## এই ক্লাসে কী কী শেখানো হয়েছে?

এই ক্লাসে প্রশান্ত স্যার আমাদের শিখিয়েছেন কীভাবে একটি সাধারণ স্ট্যাটিক ওয়েবসাইটকে **EJS (Embedded JavaScript)** ব্যবহার করে একটি ডাইনামিক ওয়েব অ্যাপ্লিকেশনে রূপান্তর করা যায়। এতদিন আমরা কেবল স্ট্যাটিক HTML ফাইল সার্ভার থেকে ব্রাউজারে পাঠাচ্ছিলাম (`res.sendFile()`), যার ফলে ইউজার যাই দেখুক না কেন, কনটেন্ট সবসময় স্থির বা অপরিবর্তনীয় থাকত।

**এই ক্লাসের প্রধান বিষয়বস্তুসমূহ:**

1. **Dynamic UI-এর ধারণা ও প্রয়োজনীয়তা:** কেন স্ট্যাটিক HTML বাস্তব জীবনের অ্যাপ্লিকেশনের জন্য যথেষ্ট নয় (যেমন: রিয়েল-টাইম স্টক আপডেট, পার্সোনালাইজড ড্যাশবোর্ড, লোকালাইজেশন)।
2. **Express রাউটে ডেটা শেয়ারিংয়ের অস্থায়ী হ্যাক:** রাউটারের মাধ্যমে ইউজারের ইনপুট করা ডেটা ব্যাকএন্ড ভ্যারিয়েবলে সেভ করা এবং তার সীমাবদ্ধতা।
3. **EJS (Embedded JavaScript) পরিচয়:** টেমপ্লেট ইঞ্জিন কী এবং ইজেএস কীভাবে কাজ করে।
4. **EJS ইন্সটলেশন ও কনফিগারেশন:** `npm install ejs` এবং Express অ্যাপ্লিকেশনে `app.set('view engine', 'ejs')` সেটআপ।
5. **EJS টেমপ্লেটে ডাইনামিক ডেটা রেন্ডারিং:** `&lt;% %&gt;`, `&lt;%= %&gt;`, এবং `&lt;%- %&gt;` ট্যাগ ব্যবহার করে HTML-এর ভেতর JavaScript কোড ও লুপ চালানো।
6. **EJS Partials (ইউআই পুনঃব্যবহারযোগ্যতা):** Header, Navbar এবং Head ট্যাগকে আলাদা পারশিয়াল ফাইলে ভাগ করে কোড রিইউজ করা।
7. **হ্যান্ডস-অন প্র্যাকটিস অ্যাসাইনমেন্ট:** Airbnb ক্লোন প্রজেক্টে নতুন ফর্ম ফিল্ড (Price, Location, Rating, Photo URL) যুক্ত করা, ডাইনামিক কার্ড লেআউট তৈরি করা এবং অ্যাক্টিভ ট্যাবের ব্যাকগ্রাউন্ড হাইলাইট করা।

### 🧠 Dynamic UI Architecture with EJS
> [!TIP]
> **মূল কনসেপ্ট সারসংক্ষেপ (Core Concept Breakdown):**
> A visual mind map showing the core components of building Dynamic UI in Node.js using EJS template engine.

## Static UI বনাম Dynamic UI-এর প্রয়োজনীয়তা

### What is Dynamic UI?

**Dynamic UI (ডাইনামিক ইউজার ইন্টারফেস)** হলো এমন এক ধরনের ওয়েবসাইট বা ইউআই লেআউট, যার কনটেন্ট বা ভেতরের উপাদান রান-টাইমে (Runtime) ব্যাকএন্ড ডেটা বা ইউজারের আচরণের ওপর ভিত্তি করে পরিবর্তিত হতে পারে।

### Static UI বনাম Dynamic UI

* **Static UI:** পূর্বে তৈরি করা স্থির HTML ফাইল। আমরা `res.sendFile()` ব্যবহার করে সার্ভার থেকে ফাইল ব্রাউজারে পাঠাই। ইউজার যেই হোক বা সময় যাই হোক, পেজের কনটেন্ট সবসময় একই থাকে (যেমন: একটি স্থির 404 পেজ বা নির্দেশিকা পেজ)।
* **Dynamic UI:** একই HTML লেআউট বা কাঠামো ব্যবহার করে ব্যাকএন্ড সার্ভারের ডেটা অনুসারে ভিন্ন ভিন্ন ভিজ্যুয়াল রেন্ডার করে (যেমন: ফেসবুক ফিড, স্টক প্রাইস, জিমেইল ইনবক্স)।

### Dynamic UI কেন প্রয়োজন?

1. **Personalized Content (ব্যক্তিগত কনটেন্ট):** উদাহরণস্বরূপ, জিমেইল খুললে আপনি আপনার ইমেইল দেখতে পান, অন্য কেউ তার ইমেইল দেখে। কিন্তু উভয়ের ব্যাকগ্রাউন্ড পেজ লেআউট কিন্তু একই!
2. **Dynamic Data Delivery (রিয়েল-টাইম ডেটা সরবরাহ):** স্টক মার্কেট বা লাইভ ক্রিকেট স্কোরের মতো পেজগুলোতে প্রতি মিনিটে সংখ্যা বা ডেটা পরিবর্তিত হয়।
3. **Security and Access Control:** একজন ইউজার লগইন অবস্থায় থাকলে একরকম ইউআই দেখাবে, আবার লগআউট করলে অন্যরকম পেজ দেখাবে।
4. **Localization &amp; Internationalization:** ইউজারের ভাষা বা ভৌগোলিক অবস্থানের ওপর ভিত্তি করে একই পেজের টেক্সট বাংলা, ইংরেজি বা অন্য ভাষায় পরিবর্তিত হওয়া।
5. **API &amp; Screen Size Adaptability:** ইউজারের ডিভাইস (মোবাইল বা বড় মনিটর) অনুযায়ী ইমেজের সাইজ বা ইউআই অ্যাডাপ্ট করা।

### ব্যাকএন্ডে লোকাল ভ্যারিয়েবল শেয়ারিং হ্যাক ও এর সীমাবদ্ধতা

প্রশান্ত স্যার প্রথমে একটি পরীক্ষামূলক পদ্ধতি দেখান: ফর্ম জমা হলে `req.body`\-এর ডেটা ব্যাকএন্ডের একটি সাধারণ অ্যারে (`registeredHomes.push()`) এ পুশ করা হয় এবং সেই অ্যারে সার্ভারের অন্য রাউটে এক্সপোর্ট করা হয়।

**কেন এটি আসল অ্যাপ্লিকেশনের জন্য সঠিক পদ্ধতি নয়?**

* **ডেটা হারানোর ঝুঁকি:** সার্ভার রিস্টার্ট বা কিল হলেই মেমরির সমস্ত ডেটা মুছে যায় (যেহেতু কোনো ডেটাবেস ব্যবহার করা হয়নি)।
* **গ্লোবাল এক্সেস সমস্যা:** কোনো একজন ইউজার ব্যাকএন্ডের মেমরি অ্যারেতে ডেটা যোগ করলে সকল ইউজার সেই একই ডেটা দেখতে পায়, যা সিকিউরিটির বরখেলাপ।

**Mental Model:** স্ট্যাটিক ইউআই হলো একটি মুদ্রিত বইয়ের পেজ—একবার ছাপা হলে আর বদলানো যায় না। আর ডাইনামিক ইউআই হলো একটি ডিজিটাল নোটিশ বোর্ড—কন্ট্রোল রুম থেকে ডেটা পাঠালেই স্ক্রিনের টেক্সট মুহূর্তে বদলে যায়!

### 📊 Static UI vs Dynamic UI Comparison
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> Visual chart comparing Static UI (res.sendFile) and Dynamic UI (res.render with EJS) concepts and features.

## EJS (Embedded JavaScript) টেমপ্লেট ইঞ্জিন এবং এক্সপ্রেস সেটআপ

### What is EJS?

**EJS (Embedded JavaScript)** হলো একটি জনপ্রিয় টেমপ্লেট ইঞ্জিন (Template Engine)। টেমপ্লেট ইঞ্জিন এমন একটি মেকানিজম বা টুল, যা ব্যাকএন্ডের ডেটা (Variables/Objects) এবং প্লেইন HTML স্ট্রাকচারকে একত্রিত করে রান-টাইমে একটি নতুন ফাইনাল HTML ফাইল তৈরি করে ব্রাউজারে পাঠায়।

**সহজ বাংলায়:** EJS আপনাকে HTML ফাইলের ভেতরে সরাসরি JavaScript কোড (যেমন: ভ্যারিয়েবল প্রিন্ট করা, লুপ চালানো, ইফ-এলস কন্ডিশন লেখা) লেখার সুযোগ দেয়।

### NPM দিয়ে EJS ইন্সটলেশন

EJS কোনো Node.js বিল্ট-ইন মডিউল নয়; এটি একটি থার্ড-পার্টি **npm package**। তাই এটি টার্মিনালে ইন্সটল করতে হয়:

```
npm install ejs --save

```

*নোট:* *\--save* *ব্যবহার করা হয় যাতে ডিপেন্ডেন্সিটি* *package.json* *ফাইলের ভেতরে যুক্ত হয়, কারণ প্রোডাকশন সার্ভারেও আমাদের EJS প্রয়োজন হবে।*

### Express অ্যাপ্লিকেশনে EJS কনফিগারেশন

Express অ্যাপ্লিকেশনে EJS ব্যবহার করার জন্য `app.js` বা মূল সার্ভার ফাইলে দুটি কনফিগারেশন সেট করতে হয়:

```
// app.js
const express = require('express');
const app = express();

// ১. Express-কে জানিয়ে দেওয়া হচ্ছে যে আমরা EJS ভিউ ইঞ্জিন ব্যবহার করব
app.set('view engine', 'ejs');

// ২. Express-কে বলা হচ্ছে যে আমাদের টেমপ্লেট বা ভিউ ফাইলগুলো 'views' ফোল্ডারে আছে
app.set('views', 'views');

```

### Important Method: `app.set(name, value)`

* **What is it?** Express অ্যাপ্লিকেশন অবজেক্টের একটি কনফিগারেশন সেট করার মেথড।
* **Purpose:** অ্যাপ্লিকেশনের বিভিন্ন সেটিংস বা এনভায়রনমেন্ট ভ্যারিয়েবল নির্ধারণ করা।
* **Parameters:**
  1. `name` (String): কনফিগারেশনের নাম (যেমন: `'view engine'`, `'views'`)।
  2. `value` (String/Object): সেই সেটিংসের মান (যেমন: `'ejs'`)।
* **Return:** Express অ্যাপ্লিকেশন ইনস্ট্যান্স ফেরত দেয় (মেথড চেইনিংয়ের জন্য)।

**Mental Model:** ইজেএস হলো এক ধরনের ছবি আঁকার ফ্রেম (Template)। আপনার ব্যাকএন্ড থেকে আসা ডেটা হলো রঙ। ইজেএস ইঞ্জিন এই রঙ ফ্রেমের ভেতরে সঠিক জায়গায় বসিয়ে একটি সম্পূর্ণ সুন্দর ছবি (Final HTML) তৈরি করে সার্ভার থেকে ব্রাউজারে পাঠায়।

### 📊 EJS Setup and Configuration Guide
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> Step-by-step slides illustrating the installation of EJS and configuration inside an Express application.

## ইজেএস-এ ডাইনামিক ডেটা রেন্ডারিং ও সিনট্যাক্স

### Dynamic Data Rendering using `res.render()`

পূর্বে আমরা HTML ফাইল পাঠানোর জন্য `res.sendFile()` ব্যবহার করতাম। কিন্তু EJS টেমপ্লেট ব্যবহার করার সময় আমাদের `res.render()` মেথড ব্যবহার করতে হয়।

```
// routes/userRouter.js
router.get('/', (req, res, next) =&gt; {
  res.render('home', { 
    registeredHomes: registeredHomes, 
    pageTitle: 'AirBnB Home',
    currentPage: 'Home' 
  });
});

```

#### Important Method: `res.render(view, [locals], [callback])`

* **What is it?** Express-এর রেন্সপন্স অবজেক্টের মেথড যা একটি ভিউ টেমপ্লেটকে ডেটা দিয়ে রেন্ডার করে HTML তৈরি করে পাঠায়।
* **Input Parameters:**
  1. `view` (String): ইজেএস ফাইলের নাম (এক্সটেনশন `.ejs` লেখার প্রয়োজন নেই, কারণ এক্সপ্রেস আগে থেকেই জানে এটি ejs ফাইল)।
  2. `locals` (Object): ভিউ ফাইলে পাঠানোর জন্য ভ্যারিয়েবলসমূহের অবজেক্ট।
* **Return:** তৈরি করা HTML রেসপন্স হিসেবে ক্লায়েন্ট বা ব্রাউজারে চলে যায়।

---

### EJS Tag Syntax (ইজেএস ট্যাগের ব্যবহার)

EJS ফাইলের ভেতরে (যেমন: `home.ejs`) JavaScript কোড লেখার জন্য বিশেষ ৩টি ট্যাগ সবচেয়ে বেশি ব্যবহৃত হয়:

| EJS Tag Syntax | কাজের বিবরণ (Purpose)                                                                                                                     | উদাহরণ                                    |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| `&lt;% ... %&gt;`    | **Scriptlet / Control Flow Tag:** শর্ত বা লুপ (for, forEach, if-else) চালানোর জন্য। এটি ইউআই-তে কোনো টেক্সট প্রিন্ট করে না।               | `&lt;% registeredHomes.forEach(home =&gt; { %&gt;` |
| `&lt;%= ... %&gt;`   | **Escaped Output Tag:** কোনো ভ্যারিয়েবলের মান বা এক্সপ্রেসনের ফলাফল সরাসর ইউআই-তে প্রিন্ট করার জন্য। এটি সিকিউরিটির জন্য স্যানিটাইজড করে। | `&lt;%= home.houseName %&gt;`                   |
| `&lt;%- ... %&gt;`   | **Unescaped Output / Include Tag:** র (Raw) HTML কোড রেন্ডার করতে বা পারশিয়ালস (Partials) ফাইল যুক্ত করতে।                               | `&lt;%- include('partials/head') %&gt;`         |

### EJS-এ Array Loop চালানোর পদ্ধতি

```

<main>
  <h2>Here are our Registered Homes:</h2>
  <ul>
    &lt;% registeredHomes.forEach(home =&gt; { %&gt;
      <li>
        <h3>&lt;%= home.houseName %&gt;</h3>
        <p>Price: ₹&lt;%= home.price %&gt;</p>
      </li>
    &lt;% }) %&gt;
  </ul>
</main>

```

*খেয়াল করুন:* `&lt;%` দিয়ে লুপের ওপেনিং এবং ক্লোজিং স্কোপিং করা হয়েছে, আর অবজেক্টের প্রোপার্টি আউটপুট করার জন্য `&lt;%= home.houseName %&gt;` ব্যবহার করা হয়েছে।

### 💡 EJS Syntax and Tag Cheat Sheet
> [!IMPORTANT]
> **কুইক রিভিশন পয়েন্টস (Quick Revision Points):**
> Flashcards covering essential EJS tags, res.render syntax, and data loop structures for active recall.

## ইজেএস পারশিয়ালস দিয়ে ইউআই পুনঃব্যবহার

### What are Partials in EJS?

**EJS Partials** হলো ছোট ছোট পুনঃব্যবহারযোগ্য (Reusable) ইজেএস কোড ব্লক বা ফাইল। যেমন: ওয়েবসাইটের Header, Navigation Bar, HTML Head ট্যাগ, বা Footer—যা প্রতিটি পেজে বারবার একই থাকে।

### Why use Partials?

যদি আমাদের ৪টি পেজ থাকে (`home.ejs`, `add-home.ejs`, `404.ejs`, `home-added.ejs`), তবে হেডার বা নেভিগেশন বারের কোড ৪টি ফাইলে কপি-পেস্ট করতে হতো। পরবর্তীতে কোনো একটা মেনু পরিবর্তন করতে হলে ৪টি ফাইলেই আলাদা আলাদা এডিট করতে হতো। Partials ব্যবহার করলে কোড কেবল ১টি আলাদা ফাইলে রাখা হয় এবং সব জায়গায় রিইউজ করা যায় (DRY Principle: Don't Repeat Yourself)।

### কীভাবে Partials তৈরি ও ব্যবহার করবেন?

১. `views` ফোল্ডারের ভেতর একটি নতুন ফোল্ডার তৈরি করুন: `views/partials/` ২. এর ভেতর আলাদা ইজেএস ফাইল তৈরি করুন, যেমন: `head.ejs`, `nav.ejs` ৩. মূল ফাইলে এটি যুক্ত করতে `&lt;%- include() %&gt;` সিনট্যাক্স ব্যবহার করুন:

```

  
  &lt;%- include('partials/head') %&gt;

  
  &lt;%- include('partials/nav') %&gt;

  <main>
    <h1>Welcome to Airbnb</h1>
  </main>

```

### Dynamic Partials (পারশিয়ালসে ডাইনামিক ডেটা পাস করা)

পারশিয়াল ফাইলও মূল পেজে রেন্ডার করার সময় পাস করা ভ্যারিয়েবল এক্সেস করতে পারে। যেমন: `head.ejs`\-এ যদি আমরা টাইটেল ডাইনামিক রাখতে চাই:

```

<link rel="stylesheet" />

```

### Dynamic Active Navigation Highlight Task

নেভিগেশন বারে কোন ট্যাবটিতে ইউজার বর্তমানে অবস্থান করছে, সেটি হাইলাইট করার জন্য রাউট থেকে `currentPage` পাস করে কন্ডিশনাল ক্লাস বসানো হয়:

```

<nav>
  <a href="```" summary="" value="">`: পারশিয়াল ইজেএস ফাইল মূল ফাইলে ইনক্লুড করা।

---

### Key Differences: `res.sendFile()` vs `res.render()`

| বিষয়               | `res.sendFile()`                                        | `res.render()`                                                |
| ------------------ | ------------------------------------------------------- | ------------------------------------------------------------- |
| **কাজ**            | স্ট্যাটিক ফাইল (যেমন .html) সরাসরি ক্লায়েন্টে পাঠায়।  | ইজেএস ফাইল রেন্ডার করে ডাইনামিক HTML তৈরি করে পাঠায়।          |
| **ডেটা পাস**       | এতে সার্ভার থেকে ব্যাকএন্ড ভ্যারিয়েবল পাস করা যায় না।   | দ্বিতীয় আর্গুমেন্ট হিসেবে অবজেক্টের মাধ্যমে ডেটা পাস করা যায়। |
| **ফাইল এক্সটেনশন** | পূর্ণ ফাইলের পাথ ও নাম দিতে হয় (e.g., `path.join(...)`) | শুধু ফাইলের নাম দিতে হয়, এক্সটেনশন ছা়ড়াই (e.g., `'home'`)    |

---

### Hands-On Practice Exercise Breakdown

প্রশাত স্যার ক্লাসের শেষে একটি সম্পূর্ণ অনুশীলন সমাধান করে দেখিয়েছেন:

1. **Add Home Form ফিল্ড বৃদ্ধি:** হোম নেম ছাড়াও Price per night, Location, Rating, Photo URL ইনপুট ফিল্ড যুক্ত করা।
2. **Host Router-এ ডেটা ক্যাপচার:** `req.body`\-এর সকল প্রোপার্টি অ্যারেতে সংরক্ষণ করা।
3. **Home Page Dynamic Card Display:** ইজেএস লুপ ব্যবহার করে প্রতিটি বাড়ির জন্য টেলউইন্ড সিএসএস (Tailwind CSS) স্টাইলড কার্ড রেন্ডার করা।
4. **Active Tab Highlight:** `currentPage` ভ্যারিয়েবলের ওপর ভিত্তি করে সক্রিয় ট্যাবের ব্যাকগ্রাউন্ড কালার ডাইনামিকালি পরিবর্তন করা।

subscriptions

### 📌 Building Dynamic Airbnb UI Walkthrough
> A video presentation walking through the hands-on code exercise of creating dynamic Airbnb cards and navigation highlighting using EJS.

## সাধারণ ভুল, মেন্টাল মডেল ও রিভিশন

### Common Beginner Mistakes &amp; How to Avoid Them

1. **res.render()** **এ এক্সটেনশন লেখা:**
  * *ভুল:* `res.render('home.ejs')`
  * *সঠিক:* `res.render('home')` (কারণ `view engine` আগেই `'ejs'` সেট করা হয়েছে)।
2. **ভ্যারিয়েবল পাস করতে ভুলে যাওয়া (ReferenceError):**
  * *সমস্যা:* EJS ফাইলে `&lt;%= pageTitle %&gt;` লিখেছেন, কিন্তু `res.render()` করার সময় `{ pageTitle }` অবজেক্ট পাঠাননি। ব্রাউজারে `ReferenceError: pageTitle is not defined` ক্র্যাশ করবে।
  * *সমাধান:* যে সকল ভ্যারিয়েবল ইজেএস টেমপ্লেট বা পারশিয়ালসে ব্যবহৃত হয়েছে, তা প্রতিটি রাউটের `res.render()` এ পাস করতে হবে।
3. **EJS ট্যাগে গুলিয়ে ফেলা:**
  * লুপ বা ইফ-কন্ডিশনে `&lt;%=` ব্যবহার করে ফেলা (যার ফলে কোনো টেক্সট না পেয়ে সিনট্যাক্স এরর হতে পারে)। লুপ বা লজিকের জন্য কেবল `&lt;%` ব্যবহার করুন।
4. **include** **ট্যাগে ভুল সিনট্যাক্স:**
  * `&lt;%= include() %&gt;` লেখা। ইনক্লুড করার সময় অবশ্যই `&lt;%- include() %&gt;` (Raw HTML tag) ব্যবহার করতে হবে।

---

### Mental Models for Memory

* **EJS Template Engine = Stencil &amp; Paint:** HTML কাঠামোটি একটি স্টেনসিল (Stencil), আর সার্ভার ডেটা হলো রঙ (Paint)। প্রতিবার নতুন ডেটা আসলে স্টেনসিল দিয়ে নতুন নতুন রঙচিত্র তৈরি হয়।
* **Partials = Modular Blocks:** এটি হলো বাড়ির ইটের মতো—একবার তৈরি করে যেখানে খুশি সেখানে বসিয়ে দেওয়া যায়।

### 📊 EJS Rendering Pipeline Architecture
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> Diagram illustrating the end-to-end flow from client request to Express controller, EJS compilation, and final HTML delivery.

## Summary

আজকের ক্লাসে আমরা শিখেছি কীভাবে Express অ্যাপ্লিকেশনে EJS (Embedded JavaScript) টেমপ্লেট ইঞ্জিন ব্যবহার করে স্ট্যাটিক ওয়েবসাইটকে ডাইনামিক ওয়েব অ্যাপ্লিকেশনে রূপান্তর করা হয়।

### Key Takeaways:

* **Static vs Dynamic:** Dynamic UI দিয়ে রান-টাইমে ইউজারের চাহিদানুযায়ী পেজের কনটেন্ট পরিবর্তন করা যায়।
* **EJS Installation &amp; Setup:** `npm install ejs` এবং `app.set('view engine', 'ejs')` এর মাধ্যমে ভিউ ইঞ্জিন সক্রিয় করা হয়।
* **Data Rendering:** `res.render('viewName', { data })` ব্যবহার করে ভ্যারিয়েবল ইজেএস ফাইলে পাস করা হয়।
* **EJS Syntax:** `&lt;% %&gt;` ফর লজিক/লুপ, `&lt;%= %&gt;` ফর আউটপুট প্রিন্টিং, এবং `&lt;%- %&gt;` ফর ইনক্লুডিং পারশিয়ালস।
* **EJS Partials:** `&lt;%- include('partials/filename') %&gt;` ব্যবহার করে Header, Navbar-এর মতো কমন ইউআই ব্লক রিইউজ করে DRY প্রিন্সিপল বজায় রাখা হয়।</a></nav>

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Guide](https://expressjs.com/)
- [MongoDB Manual](https://www.mongodb.com/docs/manual/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to Express.js Index](../../Express.js/README.md)
