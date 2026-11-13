# 05. Streams, Chunks, Buffers ও Request Body Parsing (Data Streaming & Request Parsing)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Node.js ট্র্যাক](../../Node.js/README.md)

---

## Introduction

আপনি কি জানেন, ইন্টারনেট দিয়ে ১০ গিগাবাইটের একটি ফাইল যখন আপনার সার্ভারে আসে, তখন তা একবারে পার্সেল হিসেবে আসে না—বরং নদীর স্রোতের মতো ছোট ছোট ডেটার ফোঁটায় প্রবাহিত হয়? Node.js কীভাবে এই ধারাবাহিক ডেটা গ্রহণ করে, সাজায় এবং ব্যবহার উপযোগী অবজেক্টে রূপান্তর করে, তা দেখাই এই পাঠের মূল উদ্দেশ্য।

By the end of this lesson, you will be able to:

* Analyze the architectural role of Streams, Chunks, and Buffers in Node.js.
* Implement event listeners (`req.on('data')` and `req.on('end')`) to read incoming HTTP request data.
* Parse URL-encoded form data into meaningful JavaScript objects using `Buffer.concat()` and `URLSearchParams`.
* Design modular Node.js applications by exporting and importing custom modules with `module.exports` and `require()`.

## এই ক্লাসে কী কী শেখানো হয়েছে?

এই ক্লাসে মূলত Node.js-এ ক্লায়েন্ট (Browser) থেকে পাঠানো ফর্ম ডেটা বা যেকোনো রিকোয়েস্টের ডেটা কীভাবে সার্ভারে রিসিভ করতে হয়, রিড করতে হয় এবং পার্স করতে হয় তা বিস্তারিত শেখানো হয়েছে।

### প্রধান শেখানো বিষয়সমূহ:

1. **Streams, Chunks, এবং Buffers-এর ধারণাসমূহ:** নেটওয়ার্কে ডেটা কীভাবে ছোট ছোট অংশে যাতায়াত করে।
2. **Event-Driven Request Data Reading:** Node.js-এর `req.on('data')` এবং `req.on('end')` ইভেন্ট লিসনার ব্যবহার করে আগত ডেটা রিসিভ করা।
3. **Buffer Combination &amp; Decoding:** মেমোরিতে থাকা এলোমেলো চাঙ্কগুলোকে একত্রে জোড়া দেওয়া এবং `URLSearchParams` ব্যবহার করে ফর্মের ইনপুটগুলোকে পার্স করা।
4. **Custom Module System:** একটি বড় ফাইলের কোডকে ছোট ছোট মডিউলে ভাগ করা (`module.exports` এবং `require`) এবং প্রজেক্ট ফাইল স্ট্রাকচার তৈরি করা।
5. **Practical Exercise (Calculator Application):** রাউটিং, রিকোয়েস্ট পার্সিং, মডিউল ব্যাকএন্ড এবং রিডাইরেকশনের সমন্বয়ে একটি ক্যালকুলেটর অ্যাপ্লিকেশন তৈরি।

### 📊 Request Parsing Architecture Overview (তথ্য প্রবাহের চিত্র)
```text
  [Client Browser] ──(HTTP POST Form)──> [Network Stream (Chunks of Bytes)]
                                                      │
                                                      ▼
                                       req.on('data', (chunk) => {
                                           body.push(chunk); // মেমোরি অ্যারেতে জমা
                                       });
                                                      │
                                                      ▼
                                       req.on('end', () => {
                                           const parsedBody = Buffer.concat(body).toString();
                                           const params = new URLSearchParams(parsedBody);
                                           // রেজাল্ট: { username: "shanto" }
                                       });
```

## Streams, Chunks, এবং Buffers

### What is it? (ইহা কী?)

* **Stream (স্ট্রিম):** ডেটার একটি ধারাবাহিক প্রবাহ (Continuous Flow)। যখন ডেটা উৎস থেকে গন্তব্যে একবারে না গিয়ে ক্রমাগত আসতে থাকে, তাকে Stream বলে।
* **Chunk (চাঙ্ক):** কোনো বড় ডেটার সবচেয়ে ছোট অংশ (Smallest Piece) যা একবারে নেটওয়ার্ক দিয়ে ভ্রমণ করে।
* **Buffer (বাফার):** একটি অস্থায়ী মেমোরি পার্কিং এরিয়া (Temporary Holding Area), যেখানে রানিং চাঙ্কগুলো এসে জমা হয়, যাতে সেগুলোকে ক্রমানুসারে সাজিয়ে প্রসেস করা যায়।

### Why is it used? (কেন ব্যবহার করা হয়?)

আপনি যখন মেগাবাইট বা গিগাবাইটের একটি ফাইল ক্লায়েন্ট থেকে রিসিভ করেন, তখন সার্ভার পুরো ফাইল মেমোরিতে লোড হওয়া পর্যন্ত অপেক্ষা করলে সার্ভার ক্র্যাশ করতে পারে বা মেমোরি ফুল হয়ে যেতে পারে। Streams ব্যবহার করলে ডেটা ছোট ছোট חלק (Chunks) হিসেবে প্রসেস হওয়া শুরু করে, ফলে পারফরম্যান্স অনেক ফাস্ট হয়।

### How does it work? (কীভাবে কাজ করে?)

1. ক্লায়েন্ট যখন ফাইল বা ফর্ম সাবমিট করে, তখন ডেটা ছোট ছোট Chunks এ ভেঙে যায়।
2. এই Chunks গুলো একটি Stream তৈরি করে সার্ভারের দিকে প্রবাহিত হয়।
3. সার্ভার গ্রহণ করার সময় Chunks গুলোকে সাথে সাথে প্রসেস না করে মেমোরির একটি Buffer-এ পার্ক করে বা জমা করে।
4. সমস্ত Chunks জমা হলে Buffer সেগুলো জোড়া দিয়ে একটি অর্থপূর্ণ বড় ডেটা স্ট্রাকচার বানিয়ে অ্যাপ্লিকেশনে সরবরাহ করে।

### Important Methods / Properties

* `Buffer.concat(arrayOfBuffers)`: মেমোরিতে থাকা একাধিক বাফার বা চাঙ্কের অ্যারেকে যুক্ত করে একটি একক বাফারে রূপান্তর করে।
* `buffer.toString()`: বাফার অবজেক্টের বাইনারি ডেটাকে মানুষের পঠনযোগ্য String এ রূপান্তর করে।

### Mental Model (বাস্তব জীবনের উদাহরণ)

* **Stream:** একটি নদীর স্রোত বা ক্যানাল।
* **Chunk:** ধরুন, আপনাকে ১০০০ টি ইট এক জায়গা থেকে অন্য জায়গায় নিতে হবে। আপনি একবারে ৩-৪টি করে ইট হাতে তুলে নিয়ে যাচ্ছেন। এই ৩-৪টি ইট হলো একেকটি **Chunk**।
* **Buffer:** যে জায়গায় ইটগুলো নিয়ে গিয়ে এক এক করে জমা করছেন এবং লাইন সাজাচ্ছেন, সেটি হলো **Buffer**।

> [!TIP]
> **💡 Streams, Chunks এবং Buffers-এর বাস্তব রূপক (Real-World Analogy):**
> * **Stream (নদীর স্রোত):** ক্রমাগত প্রবাহিত পানির ধারা। ইন্টারনেটে ডেটা একবারে আসে না, খণ্ডে খণ্ডে প্রবাহিত হয়।
> * **Chunk (পানির ফোঁটা / বালতি):** নদী থেকে এক এক বালতি করে পানি তোলার মতো—প্রতিটি ছোট ছোট বাইনারি ডেটা প্যাকেট।
> * **Buffer (পানির রিজার্ভার / চৌবাচ্চা):** যেখানে প্রতিটি বালতির পানি সাময়িকভাবে ঢেলে পূর্ণাঙ্গ পানির আধার তৈরি করা হয়। সম্পূর্ণ ডেটা না আসা পর্যন্ত Buffer একে মেমোরিতে ধরে রাখে।

## Event-Driven Request Reading (req.on('data') ও req.on('end'))

### What is it?

Node.js একটি **Event-Driven Architecture** অনুসরণ করে। যখন সার্ভারে কোনো নতুন রিকোয়েস্ট আসে বা ডেটার নতুন চাঙ্ক পৌঁছায়, তখন Node.js ইভেন্ট ফায়ার করে। ডেভেলপার হিসেবে আমাদের সেই ইভেন্টগুলোকে "Listen" বা শোনার জন্য লিসনার রেজিস্টার করতে হয়।

### Why is it used?

HTTP Post Request-এ ক্লায়েন্ট কত বড় ডেটা পাঠাচ্ছে তা আগে থেকে জানা থাকে না। তাই ডেটা আসার সাথে সাথে গ্রহণ করার জন্য এবং ডেটা পাঠানো শেষ হয়েছে কিনা তা নিশ্চিত হওয়ার জন্য Event Handling প্রয়োজন।

### Key Events &amp; Usage

1. **req.on('data', callback)**:
  * **Purpose:** যখনই নতুন একটি Chunk ডেটা আসবে, এই কলব্যাক ফাংশনটি স্বয়ংক্রিয়ভাবে কল হবে।
  * **Input:** 콜백 ফাংশনে আর্গুমেন্ট হিসেবে `chunk` পাওয়া যায়।
2. **req.on('end', callback)**:
  * **Purpose:** ক্লায়েন্ট যখন সমস্ত Chunk পাঠানো শেষ করে, তখন এই ইভেন্ট ট্রিগার হয়। অর্থাৎ ডেটা আসার প্রক্রিয়া শেষ।

### Code Logic &amp; Syntax Example

```
const body = [];

// ১. ডেটার প্রতিটি চাঙ্ক রিসিভ করে অ্যারেকে পুশ করা
req.on('data', (chunk) =&gt; {
  body.push(chunk);
});

// ২. সম্পূর্ণ ডেটা চলে আসার পর প্রসেস করা
req.on('end', () =&gt; {
  const parsedBody = Buffer.concat(body).toString();
  console.log(parsedBody); // Output: username=Prashant&amp;gender=Male
});

```

### Common Beginner Mistakes

* **Mistake:** `req.on('end')` এর বাইরে প্রসেস করা ডেটা ব্যবহার করার চেষ্টা করা।
* **Why it happens:** কারণ `req.on()` আসিনক্রোনাস (Asynchronous)। রিকোয়েস্টের ডেটা আসার আগেই যদি বাইরের কোড এক্সিকিউট হয়, তবে খালি ডেটা পাওয়া যাবে।

### 🔄 Asynchronous Data Stream Sequence (ধাপে ধাপে কার্যপ্রণালী)
```text
  ১. Stream Start ──────> ক্লায়েন্ট ফর্ম সাবমিট করে ডেটা পাঠানো শুরু করে
  ২. 'data' Event ──────> প্রতিবার নতুন Chunk আসার সাথে সাথে Listener ট্রিগার হয় এবং Array-তে পুশ করে
  ৩. Stream End   ──────> সম্পূর্ণ ডেটা পাঠানো শেষ হলে 'end' Event ফায়ার হয়
  ৪. Buffer Concat ─────> Buffer.concat(body).toString() দিয়ে পূর্ণাঙ্গ টেক্সটে রূপান্তর
  ৫. Parse Data   ──────> URLSearchParams দিয়ে মূল অবজেক্ট ডেটা বের করে নেওয়া হয়
```

## Data Parsing এবং URLSearchParams

### What is it?

ফর্ম সাবমিট করার পর সার্ভারে যে স্ট্রিম ডেটা পাওয়া যায়, তা এনকোডেড স্ট্রিং হিসেবে আসে (যেমন: `username=Prashant+Jain&amp;gender=Male`)। এই অসংগঠিত স্ট্রিং ডেটাকে JavaScript Object এ রূপান্তর করার প্রক্রিয়াই হলো **Parsing**।

### Why is it used?

সার্ভার বা ডেটাবেজে সরাসরি `username=Prashant+Jain` সংরক্ষণ করা কঠিন। ডেভেলপারদের সুবিধার জন্য এটিকে `{ username: 'Prashant Jain', gender: 'Male' }` অবজেক্টে রূপান্তর করা প্রয়োজন।

### How to Parse using `URLSearchParams`?

Node.js-এ বিল্ট-ইন `URLSearchParams` ক্লাস রয়েছে যা এই এনকোডেড স্ট্রিং পার্স করতে ব্যবহৃত হয়।

```
// ১. ফুল বডি পাওয়া গেলো স্ট্রিং হিসেবে
const fullBody = Buffer.concat(body).toString(); // "username=Prashant+Jain&amp;gender=Male"

// ২. URLSearchParams এ দিয়ে প্যারামিটার তৈরি
const params = new URLSearchParams(fullBody);

// ৩. প্যারামিটার থেকে অবজেক্টে কনভার্ট (Short Cut Method)
const bodyObject = Object.fromEntries(params);

console.log(bodyObject); 
// Output: { username: 'Prashant Jain', gender: 'Male' }

```

### Important Functions &amp; Utility Methods

* **new URLSearchParams(string)** **:** এনকোডেড URL string গ্রহণ করে ডিকোড করে।
* **Object.fromEntries(params)** **:** Key-Value পেয়ারের লিষ্ট বা পারামস অবজেক্ট থেকে সরাসরি স্ট্যান্ডার্ড জাভাস্ক্রিপ্ট অবজেক্ট গঠন করে।

### Common Beginner Mistakes

* `URLSearchParams` তৈরি করার সময় `new` কিওয়ার্ড দিতে ভুলে যাওয়া (যেমন: `URLSearchParams(fullBody)`), যা রানটাইম টাইপ এরর ঘটায়।

### 📋 Parsing মেথডসমূহের সারসংক্ষেপ (Methods Summary)
| মেথড / কনস্ট্রাক্টর | ভূমিকা ও ব্যবহার |
|---|---|
| `Buffer.concat(chunkArray)` | মেমোরিতে থাকা সমস্ত বাইনারি বাফার চাঙ্ককে একসাথে জোড়া দিয়ে একটি বড় বাফার তৈরি করে। |
| `buffer.toString()` | বাইনারি বাফার ডেটাকে মানুষের পাঠযোগ্য UTF-8 স্ট্রিংয়ে রূপান্তর করে। |
| `new URLSearchParams(bodyString)` | `username=kg&gender=male` জাতীয় URL Encoded টেক্সটকে সহজে কুয়েরি করার অবজেক্টে রূপান্তর করে। |
| `Object.fromEntries(params)` | `URLSearchParams` ইটারেটরকে সরাসরি সাধারণ প্লেইন জাভাস্ক্রিপ্ট অবজেক্টে কনভার্ট করে। |

## Node.js Custom Modules (module.exports ও require)

### What is it?

মডিউল (Module) হলো পৃথক ফাইল যেখানে সুনির্দিষ্ট কিছু কোড বা লজিক লেখা থাকে। নোডজেএস-এর মডিউল সিস্টেম ব্যবহার করে এক ফাইলের কোড অন্য ফাইলে ইমপোর্ট বা এক্সপোর্ট করা যায়।

### Why is it used?

একটি ফাইলে (যেমন `app.js`) সমস্ত কোড, রাউটিং, ফাইল সিস্টেম লজিক লিখলে কোড অনেক বড় ও জটিল হয়ে যায়। ক্লিন কোড এবং রিউজেবিলিটির জন্য কোডকে ছোট ছোট মডিউলে ভাগ করা হয়।

### How to Export and Import Custom Modules?

#### 1\. Exporting Code (তৈরিকৃত ফাইলে):

```
// handler.js
const requestHandler = (req, res) =&gt; {
  // Routing and parsing logic
};

// Default Export
module.exports = requestHandler;

// Named Exports (Multiple Export Shortcut)
exports.requestHandler = requestHandler;

```

#### 2\. Importing Code (মূল ফাইলে):

```
// app.js
// কাস্টম লোকাল মডিউল আনার সময় আপেক্ষিক পাথ (Relative Path) ./ বা ../ ব্যবহার করতে হয়
const requestHandler = require('./handler'); 

const server = http.createServer(requestHandler);

```

### Core vs Local Modules

* **Core Module:** Node.js-এর বিল্ট-ইন মডিউল (যেমন: `http`, `fs`, `path`)। ইমপোর্ট করতে পাথ লাগে না: `require('http')`
* **Local Module:** প্রজেক্টের নিজেদের তৈরি ফাইল। ইমপোর্ট করতে অবশ্যই আপেক্ষিক পাথ লাগে: `require('./user')`

### Mental Model

গাড়ির বিভিন্ন পার্টস যেমন আলাদা কারখানায় তৈরি হয়ে এসে অ্যাসেম্বলি লাইনে এক জায়গায় জোড়া লাগে, তেমনি বিভিন্ন মডিউলের ফাইল আলাদা লিখে `app.js` ফাইলে যুক্ত করা হয়।

### 📦 Node.js Module Export Patterns (মডিউল এক্সপোর্টের ৩টি স্ট্যান্ডার্ড নিয়ম)
```javascript
// প্যাটার্ন ১: একটি মাত্র ফাংশন এক্সপোর্ট করা
module.exports = requestHandler;

// প্যাটার্ন ২: একাধিক প্রপার্টি অবজেক্ট আকারে এক্সপোর্ট করা
module.exports = {
  handler: requestHandler,
  someText: "Some hardcoded text"
};

// প্যাটার্ন ৩: শর্টকাট exports সিনট্যাক্স ব্যবহার করা
exports.handler = requestHandler;
exports.someText = "Some hardcoded text";
```

## Important Differences

শিক্ষার্থীদের সুবিধার জন্য কিছু সম্পর্কিত বিষয়বস্তুর মধ্যে পার্থক্য নিচে দেওয়া হলো:

### 1\. Streams vs Buffers

* **Streams:** ডেটার ক্রমাগত প্রবাহ প্রসেস করে যখন ডেটা ট্রানজিটে থাকে।
* **Buffers:** মেমোরিতে নির্দিষ্ট পরিমাণ ডেটা সাময়িকভাবে ধরে রাখে প্রসেসিংয়ের পূর্বে।

### 2\. Core Modules vs Custom Local Modules

* **Core Modules:** Node.js ইনস্টলেশনের সাথেই থাকে (e.g., `http`, `fs`) এবং সরাসরি নাম দিয়ে ইমপোর্ট করা হয় (`require('fs')`)।
* **Local Modules:** আমরা নিজেরা ফাইল বানিয়ে তৈরি করি এবং ইমপোর্ট করার সময় `./` বা `../` ব্যবহার করতে হয় (`require('./handler')`)।

### 3\. `module.exports` vs `exports`

* **module.exports** **:** প্রকৃত অবজেক্ট যা Node.js রিটার্ন করে। এটি দিয়ে সিঙ্গেল ফাংশন, অবজেক্ট বা ক্লাস সরাসরি এক্সপোর্ট করা যায়।
* **exports** **:** এটি `module.exports` এর একটি শর্টকাট রেফারেন্স। শুধুমাত্র একাধিক প্রপার্টি যুক্ত করতে এটি ব্যবহার করা যায়। সরাসরি নতুন ফাংশন অ্যাসাইন করলে সম্পর্ক ভেঙে যায়।

### ⚖️ কোর কনসেপ্টের তুলনামূলক ম্যাট্রিক্স (Comparison Matrix)
| বিষয় ১ | বিষয় ২ | মৌলিক পার্থক্য |
|---|---|---|
| **Stream** | **Buffer** | Stream হলো ক্রমাগত ডেটা আসার পাইপলাইন; আর Buffer হলো সেই ডেটা সাময়িকভাবে ধরে রাখার মেমোরি স্টোরেজ। |
| **Core Module** | **Local Module** | Core Module হলো Node.js-এর সাথে ডিফল্টভাবে আসা (যেমন: `http`, `fs`), আর Local Module ডেভেলপার নিজে ফাইলে তৈরি করে (`./user.js`)। |
| **`module.exports`** | **`exports`** | `module.exports` হলো আসল অবজেক্ট রেফারেন্স; `exports` কেবল তার একটি সুবিধাজনক শর্টকাট পয়েন্টার। |

## Common Beginner Mistakes

১. **req.on('end')** **এর বাইরে ডেটা একসেস করা:**

* *সমস্যা:* আসিনক্রোনাস প্রকৃতির কারণে `end` ইভেন্ট শেষ হওয়ার আগেই বাইরের কোড রান হয়ে যায় এবং ডেটা ফাঁকা থেকে যায়।
* *সমাধান:* পার্স করা ডেটা নির্ভর সমস্ত লজিক (যেমন ফাইল রাইট বা রেসপন্স পাঠানো) `req.on('end')` এর কলব্যাক ফাংশনের ভেতরে রাখুন।

২. **লোকাল মডিউল ইমপোর্টে** **./** **না দেওয়া:**

* *সমস্যা:* `require('user')` লিখলে Node.js মনে করে এটি কোনো npm প্যাকেজ বা Core Module এবং `MODULE_NOT_FOUND` এরর দেয়।
* *সমাধান:* সবসময় লোকাল ফাইলের ক্ষেত্রে `require('./user')` ব্যবহার করুন।

৩. **URLSearchParams** **ডিক্লেয়ার করার সময়** **new** **কিওয়ার্ড বাদ পড়া:**

* *সমস্যা:* `TypeError: Class constructor URLSearchParams cannot be invoked without 'new'` এরর হওয়া।
* *সমাধান:* কনস্ট্রাক্টর কল করার জন্য অবশই `new URLSearchParams(...)` লিখুন।

### ⚠️ কমন এরর এবং ডিবাগিং কৌশল (Common Errors & Fixes)
| এরর মেসেজ | মূল কারণ | তাৎক্ষণিক সমাধান |
|---|---|---|
| `ERR_HTTP_HEADERS_SENT` | `res.end()` ডাকার পরেও পরবর্তীতে আবার `res.write()` বা `res.setHeader()` ডাকার চেষ্টা করা। | রেসপন্স মেথড কল করার পর অবশ্যই `return res.end();` লিখুন। |
| `Cannot find module './routes'` | ফাইল পাথ ভুল দেওয়া বা ফাইলের স্পেলিং/এক্সটেনশন গরমিল। | ফাইলের আপেক্ষিক পাথ চেক করুন (`./` নিশ্চিত করুন) এবং স্পেলিং মিলিয়ে দেখুন। |
| রিকোয়েস্ট বডি ফাঁকা (`undefined`) | `req.on('end')`-এর বাইরে বা সিনক্রোনাসভাবে বডি রিড করার চেষ্টা করা। | অবশ্যই `req.on('end')` ইভেন্ট লিসেনারের ভেতরে পার্সিং লজিক লিখুন। |

## Quick Revision &amp; Syntax Guide

### Important Methods / Functions

* `req.on('data', cb)` — রিকোয়েস্ট থেকে আগত প্রতি চাঙ্ক ডেটা গ্রহণ করে।
* `req.on('end', cb)` — রিকোয়েস্টের সমস্ত চাঙ্ক গ্রহণ সম্পন্ন হলে ফায়ার হয়।
* `Buffer.concat(array)` — একাধিক বাফার চাঙ্ককে একত্রে যুক্ত করে।
* `Object.fromEntries(entries)` — কি-ভ্যালু পেয়ার থেকে অবজেক্ট গঠন করে।
* `require('./path')` — কাস্টম লোকাল মডিউল ইমপোর্ট করে।

### Important Modules / Packages

* **http** (Built-in Core Module): সার্ভার তৈরি ও এইচটিটিপি নেটওয়ার্কিং প্রসেস করার জন্য ব্যবহৃত হয়।
* **fs** (Built-in Core Module): ফাইল সিস্টেমে ফাইল রাইট ও রিড করার জন্য ব্যবহৃত হয়।

### Important Terms

* **Stream:** ডেটার ক্রমান্বয়ে বহমান প্রবাহ।
* **Chunk:** স্ট্রিমের ভেতরের ক্ষুদ্রতম ডেটা ইউনিট।
* **Buffer:** ডেটা জমা রাখার জন্য মেমোরির স্থান।
* **Event-driven:** কোনো ঘটনা ঘটলে তার সাপেক্ষে সাড়া দেওয়ার প্রোগ্রামিং নীতি।

### 🧠 লেকচার ৫-এর একনজরে রিভিশন চেকলিস্ট (Quick Revision)
- [x] **Streams & Chunks:** ক্লায়েন্টের পাঠানো বড় ডেটা ছোট ছোট চাঙ্ক আকারে পৌঁছায়।
- [x] **Buffer:** সমস্ত চাঙ্ককে মেমোরিতে একত্রিত করতে `Buffer.concat()` ব্যবহৃত হয়।
- [x] **Event-Driven Parsing:** `req.on('data')` এবং `req.on('end')` দিয়ে অ্যাসিনক্রোনাসলি ডেটা সংগ্রহ করা হয়।
- [x] **URLSearchParams:** এনকোডেড টেক্সটকে প্রপার্টিজে পার্স করে জাভাস্ক্রিপ্ট অবজেক্টে রূপান্তর করা।
- [x] **Modularity:** কোডবেস পরিষ্কার রাখতে রাউটিং ও লজিককে আলাদা ফাইলে (`routes.js`) নিয়ে `module.exports` করা।

## Practice Questions &amp; Conceptual Challenges

নিজের শেখা যাচাই করার জন্য নিচের প্রশ্নগুলোর উত্তর দেওয়ার চেষ্টা করুন:

1. **কনসেপ্ট:** ইন্টারনেট থেকে ফাইলের ডেটা কেন একবারে মেমোরিতে না এনে Stream আকারে রিসিভ করা হয়?
2. **ইভেন্ট হ্যান্ডলিং:** `req.on('data')` এবং `req.on('end')` ইভেন্ট দুটির মধ্যে মূল পার্থক্য এবং কাজের ধারাবাহিকতা কী?
3. **পার্সিং:** `Buffer.concat(body).toString()` লাইনটি ঠিক কী কাজ করে?
4. **মডিউল:** `require('http')` এবং `require('./user')`\-এর মধ্যে পাথ লেখার নিয়মে কী পার্থক্য রয়েছে এবং কেন?
5. **অবজেক্ট কনভার্সন:** `URLSearchParams` ব্যবহারের পর `Object.fromEntries()` কেন ব্যবহার করা হয়?

### ❓ স্ব-মূল্যায়ন ও প্র্যাকটিস প্রশ্নাবলী (Self-Assessment Questions)
1. ক্লায়েন্ট যখন ফর্মে ডেটা সাবমিট করে, তখন Node.js কেন তা সরাসরি একটি সাধারণ ভ্যারিয়েবলে না দিয়ে Stream আকারে গ্রহণ করে?
2. `req.on('data')` এবং `req.on('end')`-এর মধ্যে পার্থক্য কী এবং এদের কার্যপ্রণালী বুঝিয়ে বলুন।
3. `Buffer.concat(body).toString()` মেথডটি ঠিক কী কাজ সম্পন্ন করে?
4. `module.exports = requestHandler` এবং `exports.handler = requestHandler`-এর মধ্যে পার্থক্য কী?
5. `ERR_HTTP_HEADERS_SENT` এররটি কেন ঘটে এবং এটি রোধ করার নিয়ম কী?

## Summary

Node.js-এ Incoming HTTP Request প্রসেস করার জন্য Streams, Chunks এবং Buffers-এর ধারণা অত্যন্ত মৌলিক। Event-driven আর্কিটেকচারের সাহায্যে `data` ও `end` ইভেন্ট লিসেন করে ডেটা প্রসেস করা এবং কাস্টম মডিউলের মাধ্যমে কোডকে পরিচ্ছন্ন রাখাই এই ক্লাসের মূল প্রতিপাদ্য।

### Key Takeaways:

* **Data Handling via Streams:** নেটওয়ার্কের মাধ্যমে ডেটা ছোট ছোট Chunks হিসেবে ট্রাভেল করে এবং মেমোরিতে Buffer আকারে জমা হয়।
* **Event-Driven Assembly:** `req.on('data')` দিয়ে চাঙ্ক রিসিভ করতে হয় এবং `req.on('end')` এর ভেতরে সম্পূর্ণ বাফার জোড়া দিয়ে মূল ডেটা গঠন করতে হয়।
* **Data Parsing:** এনকোডেড ফর্ম স্ট্রিংকে `URLSearchParams` ও `Object.fromEntries()` দিয়ে ব্যবহারযোগ্য JavaScript Object-এ রূপান্তর করা যায়।
* **Modular Structure:** `module.exports` ও `require('./path')` ব্যবহার করে এপ্লিকেশন কোডকে ক্লিন ও মডিউলার রাখা যায়।

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Guide](https://expressjs.com/)
- [MongoDB Manual](https://www.mongodb.com/docs/manual/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to Node.js Index](../../Node.js/README.md)
