# 07. এডিট ফর্ম প্রি-ফিলিং, ডাটা আপডেট ও ডিলিট অপারেশন (Edit Form Pre-filling, Update & Delete CRUD Operations)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Express.js ট্র্যাক](../../Express.js/README.md)

---

## Introduction

আপনি কি কখনো ভেবে দেখেছেন, একটি ওয়েবসাইটে 'Add Home' এবং 'Edit Home' পেজ দুটি দেখতে প্রায় হুবহু এক হলেও ব্যাকএন্ডে কীভাবে ডাটা প্রি-ফিল (Pre-fill) হয় এবং ব্যাকএন্ডে আপডেট কিংবা ডিলিট অপারেশন পরিচালিত হয়?

By the end of this lesson, you will be able to:

* Add এবং Edit Form-এর জন্য একই UI টেমপ্লেট পুনর্ব্যবহার (Reuse) করার কৌশল ব্যাখ্যা করতে পারবেন।
* Query Parameters (`req.query`) এবং Dynamic Path Parameters (`req.params`) এর কাজের ক্ষেত্র ও পার্থক্য চিহ্নিত করতে পারবেন।
* Hidden Input Field ব্যবহার করে POST রিকোয়েস্টে নিরাপদে আইডি (ID) প্রেরণের প্রক্রিয়া সম্পাদন করতে পারবেন।
* ফাইল সিস্টেমে `Array.prototype.map()` এবং `Array.prototype.filter()` ব্যবহার করে Data Update এবং Delete অপারেশন বাস্তবায়ন করতে পারবেন।
* ক্যাসকেডিং ডিলিট (Cascading Delete) প্রয়োগ করে মূল ডাটা মুছে যাওয়ার পর আনুষঙ্গিক ফেভারিট লিস্ট থেকেও ডাটা ডিলিট করতে পারবেন।

## এই ক্লাসে কী কী শেখানো হয়েছে?

এই ক্লাসে মূলত একটি সম্পূর্ণ ফুল-স্ট্যাক Node.js অ্যাপ্লিকেশনে **CRUD (Create, Read, Update, Delete)** অপারেশনের **Update** এবং **Delete** অংশ দুটি বিস্তারিতভাবে দেখানো হয়েছে।

### প্রধান আলোচিত বিষয়সমূহ:

1. **Form Reusability (ফর্ম পুনর্ব্যবহার):** আলাদাভাবে নতুন পেজ না বানিয়ে বিদ্যমান `add-home.ejs` পেজকে নাম পরিবর্তন করে `edit-home.ejs` করা এবং একই টেমপ্লেট দিয়ে নতুন ডাটা যুক্ত করা ও পুরাতন ডাটা এডিট করা।
2. **Query Parameters এর ব্যবহার:** URL-এর শেষে `?editing=true` যুক্ত করে মোড নির্ধারণ করা এবং ব্যাকএন্ডে `req.query.editing` দিয়ে তা রিড করা।
3. **Data Pre-filling (পূর্বে সংরক্ষিত তথ্য দেখানো):** `findById` মেথডের মাধ্যমে ফাইল থেকে নির্দিষ্ট বাড়ির তথ্য এনে এডিট ফর্মের ইনপুট ফিল্ডগুলোতে প্রদর্শন করা।
4. **Hidden Input Field এর মাধ্যমে ID পাঠানো:** ইউজার যেন আইডি পরিবর্তন করতে না পারে, কিন্তু সাবমিট করলে ব্যাকএন্ডে আইডি পৌঁছায় সে জন্য `` ব্যবহার করা।
5. **Data Update (ডাটা আপডেট):** `Array.prototype.map()` মেথড ব্যবহার করে ফাইল ডাটাবেসে নির্দিষ্ট আইডির ডাটা নতুন তথ্য দিয়ে রিপ্লেস করা।
6. **Delete Feature &amp; Security (ডিলিট ফিচার):** কোনো তথ্য মুছে ফেলার জন্য GET রিকোয়েস্টের পরিবর্তে HTML Form এর মাধ্যমে POST রিকোয়েস্ট পাঠানো এবং `Array.prototype.filter()` দিয়ে ফাইল থেকে নির্দিষ্ট ডাটা রিমুভ করা।
7. **Cascading Delete (ক্যাসকেডিং ডিলিট):** মূল হোস্ট তালিকা থেকে একটি বাড়ি ডিলিট করা হলে ব্যবহারকারীর ফেভারিট তালিকা (`favorite.json`) থেকেও যেন সেই আইডিটি স্বয়ংক্রিয়ভাবে মুছে যায় তা নিশ্চিত করা।

### 🧠 Lecture Summary Mind Map
> [!TIP]
> **মূল কনসেপ্ট সারসংক্ষেপ (Core Concept Breakdown):**
> এই মাইন্ড ম্যাপটি ক্লাসে শেখানো Edit, Delete, Query Parameters এবং Cascading Delete কনসেপ্টগুলোর সংযোগ তুলে ধরে।

## Query Parameters বনাম Dynamic Path Parameters

ওয়েব অ্যাপ্লিকেশনে ক্লায়েন্ট (Browser) থেকে সার্ভারে ডাটা জানানোর জন্য দুই ধরনের ইউআরএল কৌশল ব্যবহৃত হয়: **Path Parameters** এবং **Query Parameters**।

### Dynamic Path Parameters (`req.params`)

* **What is it?** ইউআরএল পাথের (URL Path) ভেতরে নির্ধারিত পরিবর্তনশীল অংশ। এক্সপ্রেস রাউটে এটি কোলন (`:`) দিয়ে লেখা হয়।
* **Syntax Example:** রাউট: `/host/edit-home/:homeId` \-&gt; আসল ইউআরএল: `/host/edit-home/102`
* **Node.js Access:** `req.params.homeId`
* **Why &amp; When used:** যখন কোনো নির্দিষ্ট রিসোর্সকে চিহ্নিত করতে হয় (যেমন: একটি নির্দিষ্ট প্রোডাক্ট বা পোস্টের অনন্য আইডি)। এটি ইউআরএল এর মূল স্ট্রাকচারের অংশ।

### Query Parameters (`req.query`)

* **What is it?** ইউআরএল-এর শেষে প্রশ্নবোধক চিহ্নের (`?`) পর `key=value` জোড়ায় প্রেরিত ডাটা। একাধিক ডাটা পাঠাতে `&amp;` চিহ্ন ব্যবহার করা হয়।
* **Syntax Example:** ইউআরএল: `/host/edit-home/102?editing=true&amp;theme=dark`
* **Node.js Access:** `req.query.editing` (যার আউটপুট হবে String `"true"`)
* **Why &amp; When used:** অতিরিক্ত তথ্য, ফিল্টারিং, সার্চ কোয়েরি, প্যাগিনেশন বা কোনো অন-অফ ফ্ল্যাগ (Flag) পাঠানোর জন্য ব্যবহৃত হয়। এটি ইউআরএল এর কাঠামোগত বাধ্যতামূলক অংশ নয়।

### Mental Model

চিন্তা করুন, **Dynamic Path Parameter** হলো একটি ভবনের নির্দিষ্ট **রুম নম্বর** (যেমন: `/building/room-302`), যা সরাসরি স্থান চিহ্নিত করে। অন্যদিকে, **Query Parameter** হলো ঐ রুমে প্রবেশ করার মোড বা কনফিগারেশন নির্দেশক সুইচ (যেমন: `?ac=true&amp;light=dim`), যা ঘরের অবস্থান পরিবর্তন করে না, কিন্তু ব্যবহারের মোড বদলে দেয়।

### 📊 Path vs Query Parameters Comparison
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> Path Parameters এবং Query Parameters-এর গঠন, ব্যাকএন্ডে এক্সেস করার নিয়ম এবং ব্যবহারের পার্থক্যের তুলনামূলক ইনফোগ্রাফিক।

## Form Reusability ও Data Pre-filling (Edit Home Feature)

একটি মানসম্মত অ্যাপ্লিকেশনে বারবার একই কোড না লিখে DRY (Don't Repeat Yourself) নীতি মেনে চলা হয়।

### UI Reusability (ইউআই পুনর্ব্যবহার)

Add Home এবং Edit Home-এর ইনপুট ফিল্ডগুলো (Name, Price, Location, Image URL) একই। তাই আমরা দুটি আলাদা EJS টেমপ্লেট তৈরি না করে একটি টেমপ্লেটেই ফ্ল্যাগ (Flag) পাঠাই:

* **Add Mode:** `editing: false`
* **Edit Mode:** `editing: true`

### Boolean Conversion Mistake

`req.query.editing` ব্যাকএন্ডে সর্বদা একটি **String** হিসেবে আসে (যেমন: `"true"`)। তাই সরাসরি `if (req.query.editing)` চেক না করে বুলিয়ানে রূপান্তর করতে হয়:

```
const isEditing = req.query.editing === 'true';

```

### Data Pre-filling Mechanism

ইউজার এডিট বাটনে চাপ দিলে Controller মেথডটি URL থেকে ID সংগ্রহ করে Model এর `findById` ফাংশনের মাধ্যমে নির্দিষ্ট অবজেক্টটি উদ্ধার করে। এরপর EJS টেমপ্লেটে মান বসানো হয়:

```

```

### Hidden Input Field (লুকানো ইনপুট ফিল্ড)

**কী ও কেন?** ব্যাকএন্ডে আপডেট রিকোয়েস্ট পাঠাতে জানা আবশ্যক কোন আইডির ডাটা আপডেট হচ্ছে। কিন্তু ইউজারকে সেই আইডি পরিবর্তন করতে দেওয়া যাবে না। তাই `` ব্যবহার করা হয়।

```
&lt;% if (editing) { %&gt;
  
&lt;% } %&gt;

```

এটি ফর্মে দৃশ্যমান হয় না, কিন্তু সাবমিট করলে `req.body.id` হিসেবে সার্ভারে পৌঁছে যায়।

### 📊 Edit Form Workflow &amp; Pre-filling Process
> [!NOTE]
> **আর্কিটেকচার ও কাজের প্রবাহচিত্র (Visual Diagram & Flow):**
> ইউজার যখন এডিট বাটনে ক্লিক করে তখন থেকে ব্যাকএন্ড ডাটা ফেচ করে ফর্মে দেখানোর সম্পূর্ণ ধাপভিত্তিক উপস্থাপনা।

## Data Update, Delete এবং Cascading Delete

ফাইল-ভিত্তিক ডাটাবেসে ডাটা পরিবর্তন এবং মুছে ফেলার জন্য JavaScript Array-এর মেথডসমূহ অত্যন্ত কার্যকরীভাবে ব্যবহৃত হয়।

### ১. Data Update (মেথড: `Array.prototype.map`)

সবগুলো ঘর লোড করে `map()` চালানো হয়। যেটির ID আপডেটকৃত অবজেক্টের ID-এর সাথে মেলে, সেটিকে নতুন অবজেক্ট (`this`) দিয়ে রিপ্লেস করা হয়, অন্যগুলো অপরিবর্তিত থাকে:

```
static save() {
  Home.fetchAll((registeredHomes) =&gt; {
    if (this.id) { // Edit Case
      const updatedHomes = registeredHomes.map((home) =&gt;
        home.id === this.id ? this : home
      );
      // এরপর updatedHomes ফাইলে সেভ করা হয়
    } else { // Add Case
      // নতুন আইডি জেনারেট করে অ্যারোতে पुश করা হয়
    }
  });
}

```

### ২. Delete Operation (মেথড: `Array.prototype.filter`)

তথ্য মুছে ফেলার জন্য `filter()` মেথড ব্যবহার করে কাঙ্ক্ষিত ID টি বাদ দিয়ে নতুন অ্যারো তৈরি করা হয়:

```
static deleteById(homeId, callback) {
  Home.fetchAll((homes) =&gt; {
    const filteredHomes = homes.filter((home) =&gt; home.id !== homeId);
    fs.writeFile(filePath, JSON.stringify(filteredHomes), callback);
  });
}

```

### ৩. HTTP Security Rule: Delete-এ কেন Form &amp; POST প্রয়োজন?

নরমাল এঙ্কর ট্যাগ (`<a href="/delete/1">`) দ্বারা রিকোয়েস্ট পাঠালে তা **GET** রিকোয়েস্ট হয়। HTTP কনভেনশন অনুযায়ী, GET রিকোয়েস্টে ব্যাকএন্ডের ডাটা বা স্টেটের কোনো পরিবর্তন হওয়া উচিত নয়। ডাটা পরিবর্তনের জন্য অবশ্যই **POST** (বা DELETE) রিকোয়েস্ট ব্যবহার করা উচিত। তাই ডিলিট বাটনটিকে একটি `</a>`
4. **Cascading Delete ভুলে যাওয়া:**
  * *ভুল:* মূল আইটেম মুছে ফেলা হলেও ফেভারিট বা সাব-লিস্টে আইডি থেকে যাওয়া, যার ফলে ফেভারিট পেজ লোড করার সময় অ্যাপ ক্র্যাশ করে।

### 📌 Node.js Architecture &amp; CRUD Operations Overview
> Dynamic Path, Form Reusability, Update &amp; Delete Flow এবং Cascading Delete-এর একটি সহজ বাংলা অডিও ব্যাখ্যা।

## Summary

এই ক্লাসে আমরা শিখেছি কীভাবে ইউআই কোড রি-ইউজ করে Add এবং Edit ফর্ম তৈরি করতে হয়, Query Parameters ও Hidden Fields ব্যবহার করে আপডেট অপারেশন পরিচালনা করতে হয়, এবং HTTP POST রিকোয়েস্ট ও Array Methods (Filter/Map) ব্যবহার করে ডাটা মুছে ফেলা ও ক্যাসকেডিং ডিলিট নিশ্চিত করতে হয়।

### Key Takeaways

* UI Reusability নিশ্চিত করতে একই ইজেএস (EJS) টেমপ্লেটে `editing` ফ্ল্যাগ পাঠিয়ে কন্ট্রোল করা হয়।
* `req.params` ব্যবহার করা হয় বাধ্যতামূলক ইউআরএল পাথের জন্য এবং `req.query` ব্যবহার করা হয় ঐচ্ছিক ফ্ল্যাগ বা কোয়েরি স্ট্রিং রিড করতে।
* ব্যাকএন্ডে সুরক্ষিতভাবে আইডি পাঠাতে ফর্মে `` ফিল্ড ব্যবহার করা হয়।
* সিস্টেমের স্টেট বা ডাটা পরিবর্তনকারী যেকোনো রিকোয়েস্ট (Update/Delete) অবশ্যই POST মেথডে করা উচিত।
* কোনো তথ্য মূল ডাটাবেস থেকে মুছে ফেলার পর তার সাথে সম্পর্কিত আনুষঙ্গিক ডাটা (যেমন: ফেভারিট লিস্ট) মোছাকে Cascading Delete বলে।

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Guide](https://expressjs.com/)
- [MongoDB Manual](https://www.mongodb.com/docs/manual/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to Express.js Index](../../Express.js/README.md)
