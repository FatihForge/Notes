# ⚡ 06. DOM — Document Object Model & Manipulation (ডোম ম্যানিপুলেশন)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

## ১. DOM আসলে কী? (What is it?)

**DOM বা Document Object Model** হলো আপনার স্ট্যাটিক HTML এবং ডায়নামিক JavaScript-এর মধ্যকার মূল সেতুবন্ধন। যখন একটি ওয়েবপেজ ব্রাউজারে লোড হয়, ব্রাউজার স্বয়ংক্রিয়ভাবে সম্পূর্ণ HTML স্ট্রাকচারকে একটি অবজেক্টে রূপান্তর করে। এই অবজেক্টটি ব্রাউজারের গ্লোবাল `window` অবজেক্টের ভেতরে থাকে এবং একেই আমরা `document` বলে ডাকি।

সহজ ভাষায়, **DOM হলো আপনার ওয়েবসাইটের একটি "ট্রি মডেল" বা "গাছ" (Tree Structure)**। এখানে প্রতিটি HTML এলিমেন্ট (যেমন: ``, ``, বা ``) এক একটি অবজেক্ট হয়ে যায়, যাদের সাথে JavaScript সরাসরি কথা বলতে বা নিয়ন্ত্রণ করতে পারে।

### 🎯 কেন এটি প্রয়োজন?

স্ট্যাটিক HTML কোড নিজে থেকে কখনো রান-টাইমে কোনো পরিবর্তন করতে পারে না। JavaScript-কে ওয়েবপেজের উপাদানগুলো চিনিয়ে দেওয়া এবং নতুন নির্দেশ পাঠানোর একমাত্র মাধ্যমই হলো **DOM**।

## ২. কেন এটি গুরুত্বপূর্ণ? (Why is it important?)

DOM ছাড়া একটি ওয়েবসাইট কেবল একটি **"স্থির পোস্টার (Static Poster)"**-এর মতো। এটি দেখতে সুন্দর হতে পারে, কিন্তু ইউজারের সাথে কোনো ইন্টারঅ্যাকশন করতে পারে না।

* রান-টাইম পরিবর্তন (Runtime Manipulation): পেজ পুনরায় রিলোড না করেই লাইভ কন্টেন্ট, ডিজাইন ও লেআউট আপডেট করা যায়।

* অ্যানালিটিক্যাল লজিক ওয়্যারিং: বাটনে ক্লিক করে ডার্ক মোড অন করা, সার্চ বক্সে টাইপ করার সাথে সাথে রেজাল্ট দেখানো বা ফর্ম সাবমিট হলে লাইভ ভ্যালিডেশন চেক করা—সবকিছুই DOM-এর মাধ্যমে নিয়ন্ত্রিত হয়।

### 🏛️ ওয়েব ডেভেলপমেন্টের তিন মাস্কেটিয়ার

                1. HTML (কাঠামো)
                বাড়ির ইটের দেয়াল বা মৌলিক স্ট্রাকচার।

                2. CSS (সাজসজ্জা)
                দেয়ালের রঙ ও ইন্টেরিয়র ডেকোরেশন।

                3. JavaScript (ওয়্যারিং)
                ইলেকট্রিক সুইচবোর্ড—সুইচে চাপ দিলে লাইট জ্বলবে কিনা সেই লজিক।

## ৩. DOM সিলেক্টরস ও নোডস

            **🔥 Pro Tip (console.log বনাম console.dir):**

সাধারণত `console.log(element)` দিলে ব্রাউজার HTML ট্যাগের রূপ দেখায়। কিন্তু `console.dir(element)` ব্যবহার করলে ব্রাউজার সেই এলিমেন্টকে একটি **সম্পূর্ণ JavaScript Object** হিসেবে প্রদর্শন করে, যার ফলে তার সব Properties (যেমন: URL, children, baseURI) এবং Methods ডিটেইলসে দেখা যায়।

### আধুনিক DOM Selectors:

| Selector | কাজ / রিটার্ন মান | উদাহরণ কোড |
| --- | --- | --- |
| getElementById() | ইউনিক ID দিয়ে একক এলিমেন্ট ধরে। | document.getElementById("navbar") |
| getElementsByClassName() | নির্দিষ্ট ক্লাসের সমস্ত এলিমেন্ট (HTMLCollection)। | document.getElementsByClassName("card") |
| querySelector() | CSS সিলেক্টর দিয়ে ম্যাচ করা প্রথম এলিমেন্টটি রিটার্ন করে (সবচেয়ে আধুনিক)। | document.querySelector(".cta-btn") |
| querySelectorAll() | ম্যাচ করা সব এলিমেন্টের একটি NodeList রিটার্ন করে। | document.querySelectorAll("p.desc") |

### HTMLCollection বনাম NodeList:

| HTML Collection | NodeList |
| --- | --- |
| getElementsByClassName দিয়ে পাওয়া যায়। | querySelectorAll দিয়ে পাওয়া যায়। |
| এটি শুধুমাত্র Element Nodes ধারণ করতে পারে। | এটি Element, Text এমনকি Comment Node-ও ধারণ করতে পারে। |

## ৪. DOM Properties: টেক্সট দেখা ও পরিবর্তন

এলিমেন্টের ভেতরের কন্টেন্ট রিড বা রাইট করার জন্য ৩টি প্রপার্টির সূক্ষ্ম পার্থক্য জানা অত্যন্ত জরুরি:

              `innerText`

শুধুমাত্র পেজে দৃশ্যমান (Rendered visible) টেক্সট রিটার্ন করে। সিএসএস স্টাইলিং মেনে চলে।

              `textContent`

হাইডেন (`display: none`) উপাদানসহ পুরো নোডের আসল টেক্সট কন্টেন্ট বের করে আনে।

              `innerHTML`

ভেতরের টেক্সটের সাথে সাথে সমস্ত HTML ট্যাগ পড়ে এবং নতুন ডায়নামিক ট্যাগ ইনসার্ট করতে পারে।

## ৫. DOM Manipulation (এলিমেন্ট পরিবর্তন ও তৈরি)

### 5.1 Attributes & Dynamic Styling

জাভাস্ক্রিপ্ট দিয়ে স্টাইল পরিবর্তন করার সময় CSS-এর kebab-case প্রপার্টিগুলো জাভাস্ক্রিপ্টে **camelCase**-এ রূপান্তরিত হয় (যেমন: `background-color` হয়ে যায় `backgroundColor`)।

                style-manipulation.js
                Copy

```javascript
const box = document.querySelector("#box");

// 1. Attribute ম্যানিপুলেশন:
box.setAttribute("title", "Interactive Box");
let attrVal = box.getAttribute("id"); // "box"

// 2. ডায়নামিক স্টাইল প্রয়োগ (Inline CSS হিসেবে সেট হয়):
box.style.backgroundColor = "purple";
box.style.borderRadius = "12px";
```

### 5.2 নতুন এলিমেন্ট তৈরি: The Two-Step Process

              📌 **গোল্ডেন রুল:** নতুন এলিমেন্ট পেজে যুক্ত করার প্রক্রিয়াটি সর্বদা **২টি ধাপে** সম্পন্ন হয়:

* ধাপ ১ (Create): প্রথমে মেমোরিতে এলিমেন্ট তৈরি করা (document.createElement)।

* ধাপ ২ (Append/Add): তৈরি করা এলিমেন্টকে DOM Tree-তে যুক্ত করা (append / prepend / before / after)।

                create-element.js
                Copy

```javascript
// ধাপ ১: মেমোরিতে বাটন তৈরি
const newBtn = document.createElement("button");
newBtn.innerText = "Click Me!";
newBtn.classList.add("btn-primary");

// ধাপ ২: নির্দিষ্ট প্যারেন্ট এলিমেন্টের ভেতরে যুক্ত করা
const container = document.querySelector(".card-body");
container.append(newBtn); // কন্টেইনারের একদম শেষে যুক্ত করবে

// এলিমেন্ট ডিলিট করা:
// newBtn.remove();
```

### 5.3 classList API (The Professional Way)

ক্লাস পরিবর্তনের জন্য `setAttribute("class", ...)` ব্যবহার বিপজ্জনক, কারণ এটি পূর্বের সমস্ত ক্লাস মুছে ফেলে। পেশাদার ডেভেলপাররা সর্বদা `classList` ব্যবহার করেন।

| পদ্ধতি | setAttribute("class", "new") | classList.add("new") |
| --- | --- | --- |
| আচরণ | আগের সব ক্লাস মুছে ফেলে শুধু নতুনটি রাখে (Wipes the slate clean)। | বিদ্যমান সব ক্লাস অক্ষুণ্ণ রেখে কেবল নতুনটি নিরাপদে যোগ করে। |
| ঝুঁকি | বিদ্যমান CSS ডিজাইন ভেঙে পড়ার তীব্র ঝুঁকি থাকে। | ডিজাইনে কোনো পার্শ্বপ্রতিক্রিয়া পড়ে না। ১০০% নিরাপদ। |

## ৬. সাধারণ ভুলসমূহ (Common Mistakes)

১. script ট্যাগ ভুল জায়গায় প্লেস করা (Returns null)

যদি আপনি `` ট্যাগকে ``-এ রাখেন এবং `defer` ব্যবহার না করেন, তবে HTML বডি লোড হওয়ার আগেই স্ক্রিপ্ট রান হয়ে যাবে এবং যেকোনো এলিমেন্ট খুঁজলে `null` রিটার্ন করবে।

                ✅ সর্বদা <script> ট্যাগকে </body> শেষ হওয়ার ঠিক আগে রাখুন।

## ৭. দ্রুত রিভিশন (Quick Revision)

* ✓
                DOM: JavaScript-এর মাধ্যমে HTML নিয়ন্ত্রণ করার ডিজিটাল রিমোট।

* ✓
                হায়ারার্কি: Window > Document > HTML > Head/Body।

* ✓
                সিলেকশন: আধুনিক প্রজেক্টে querySelector ও querySelectorAll ব্যবহার করাই স্ট্যান্ডার্ড।

* ✓
                ইনসারশন: টু-স্টেপ প্রসেস—প্রথমে createElement দিয়ে মেমোরিতে তৈরি করুন, তারপর append দিয়ে পেজে যুক্ত করুন।

            ← Ch 05: Functions & Methods

            Next: Ch 07 — Events in JavaScript
            →

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/dom-document-object-model)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
