# ⚡ 09. Callbacks, Promises & Async/Await (অ্যাসিনক্রোনাস জাভাস্ক্রিপ্ট)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

## ১. অ্যাসিনক্রোনাস প্রোগ্রামিং কী? (What is it?)

আধুনিক ওয়েব অ্যাপ্লিকেশন তৈরির মূল ভিত্তি হলো **Asynchronous Programming**। জাভাস্ক্রিপ্টে এটি একটি অত্যন্ত শক্তিশালী ধারণা যা আমাদের অ্যাপ্লিকেশনকে **"Non-blocking"** হতে সাহায্য করে।

অর্থাৎ, ডাটাবেস বা সার্ভার থেকে ডেটা ফেচ করার মতো সময়সাপেক্ষ কাজগুলো যখন ব্যাকগ্রাউন্ডে চলতে থাকে, তখন জাভাস্ক্রিপ্ট অন্যান্য কাজগুলো থামিয়ে না রেখে কোড এক্সিকিউশন চালিয়ে যেতে পারে। এটি অ্যাপ্লিকেশনের পারফরম্যান্স বজায় রাখে এবং ব্রাউজারের ইউজার ইন্টারফেসকে সচল রাখে।

## ২. কেন এটি গুরুত্বপূর্ণ? (UI Blocking প্রতিরোধ)

বাস্তব জীবনের অ্যাপ্লিকেশন যেমন—ওয়েদার অ্যাপ, লাইভ স্টক ট্র্যাকার বা সোশ্যাল মিডিয়া ফিড তৈরি করতে গেলে আমাদের সার্ভার API থেকে ডেটা সংগ্রহ করতে হয়।

ধরা যাক, একটি সার্ভার থেকে ডেটা আসতে **৫ সেকেন্ড** সময় লাগছে। যদি আমরা Synchronous (লাইন-বাই-লাইন ব্লকিং) পদ্ধতিতে কোড লিখি, তবে এই ৫ সেকেন্ড পুরো ওয়েব অ্যাপ্লিকেশনটি সম্পূর্ণরূপে আটকে থাকবে (**UI Blocking**)। ফলস্বরূপ ইউজার কোনো বাটনে ক্লিক করতে পারবে না, পেজ স্ক্রল করতে পারবে না—ব্রাউজার 'Not Responding' দেখাবে।

ইউজারের ফ্রন্ট-এন্ড ইন্টারফেসে কোনো ব্যাঘাত না ঘটিয়ে ব্যাকগ্রাউন্ডে ডেটা হ্যান্ডেল করার জন্যই জাভাস্ক্রিপ্টে **Callback, Promise এবং Async-Await** ব্যবহৃত হয়।

## ৩. সিনক্রোনাস বনাম অ্যাসিনক্রোনাস (Execution Flow)

              Synchronous (ব্লকিং)

প্রতিটি ইন্সট্রাকশন আগের ইন্সট্রাকশন শেষ না হওয়া পর্যন্ত অপেক্ষা করে। একটি কাজ শেষ হলেই কেবল পরবর্তী লাইনে যায়।

              Asynchronous (নন-ব্লকিং)

কোনো কাজ বেশি সময় নিলে সেটি ব্যাকগ্রাউন্ডে চলতে থাকে এবং তার পরের লাইনগুলো সাথে সাথেই এক্সিকিউট হয়।

              sync-async-demo.js
              Copy

```javascript
console.log("১");
console.log("২");

// ব্যাকগ্রাউন্ড টাইমার (২ সেকেন্ড পর কল হবে):
setTimeout(() => {
  console.log("হ্যালো! (২ সেকেন্ড পর আসলাম)");
}, 2000);

console.log("৩");
console.log("৪");

/* আউটপুট ক্রম:
   ১
   ২
   ৩
   ৪
   হ্যালো! (২ সেকেন্ড পর)
*/
```

লক্ষ্য করুন, `setTimeout` ২ সেকেন্ড সময় নিলেও সে ৩ এবং ৪ নম্বর লাইনের আউটপুট আটকে রাখেনি।

## ৪. কলব্যাক এবং কলব্যাক হেল (Pyramid of Doom)

যখন একটি ফাংশনকে অন্য একটি ফাংশনের ভেতর আর্গুমেন্ট হিসেবে পাঠানো হয় এবং অ্যাসিনক্রোনাস কাজ সম্পন্ন হওয়ার পর সেটি কল করা হয়, তাকে **Callback** বলা হয়।

কিন্তু যখন একাধিক অ্যাসিনক্রোনাস কাজ ক্রমান্বয়ে একের পর এক চালাতে হয়, তখন একটি কলব্যাকের ভেতর আরেকটি, তার ভেতর আরেকটি—এভাবে কোডের গভীর নেস্টিং তৈরি হয়। একে প্রোগ্রামিংয়ে **Callback Hell বা "Pyramid of Doom"** বলা হয়।

              callback-hell.js
              Copy

```javascript
// Callback Hell (ভীষণ জটিল ও অবিন্যস্ত পিরামিড স্ট্রাকচার):
getData(1, () => {
  console.log("Data 1 fetched");
  getData(2, () => {
    console.log("Data 2 fetched");
    getData(3, () => {
      console.log("Data 3 fetched");
      // এভাবে ১০টি থাকলে কোড নিয়ন্ত্রণহীন হয়ে পড়ে
    });
  });
});
```

## ৫. প্রমিজ (Promises): একটি আধুনিক সমাধান

**Promise হলো জাভাস্ক্রিপ্টের একটি বিশেষ অবজেক্ট যা ভবিষ্যতে কোনো কাজের "Eventual Completion" বা ফলাফল নিশ্চিত করে।**

মানসিক রূপক হিসেবে আপনি একে কোনো **অনলাইন ই-কমার্স অর্ডারের** সাথে তুলনা করতে পারেন:

                1. Pending (অপেক্ষমাণ)
                অর্ডার পাঠানো হয়েছে, কিন্তু ডেলিভারি সম্পন্ন হয়নি।

                2. Fulfilled / Resolved
                পণ্যটি সফলভাবে গ্রাহকের কাছে পৌঁছেছে।

                3. Rejected
                পণ্যটি স্টকে নেই বা কোনো কারণে অর্ডার বাতিল হয়েছে।

              promise-handling.js
              Copy

```javascript
function fetchUserData() {
  return new Promise((resolve, reject) => {
    let success = true;
    setTimeout(() => {
      if (success) {
        resolve({ id: 101, name: "Naim", role: "Developer" });
      } else {
        reject("Error: Failed to connect to server.");
      }
    }, 1500);
  });
}

// প্রমিজ কনজিউম করা (.then এবং .catch):
fetchUserData()
  .then((data) => {
    console.log("User received:", data.name);
  })
  .catch((error) => {
    console.error("Fetch failed:", error);
  });
```

## ৬. অ্যাসিনক্রোনাস-অ্যাওয়েট (Async-Await)

বর্তমান সময়ে জাভাস্ক্রিপ্টে অ্যাসিনক্রোনাস কোড লেখার সবচেয়ে আধুনিক, পরিষ্কার এবং জনপ্রিয় পদ্ধতি হলো **Async-Await**। এটি অ্যাসিনক্রোনাস কোডকে দেখতে এবং কাজ করতে একদম সিনক্রোনাস সাধারণ কোডের মতো করে তোলে।

* async: যেকোনো সাধারণ ফাংশনের শুরুতে লিখলে ফাংশনটি বাধ্যতামূলকভাবে একটি প্রমিজ রিটার্ন করে।

* await: শুধুমাত্র async ফাংশনের ভেতরে ব্যবহার করা যায়। এটি প্রমিজের কাজ সম্পন্ন (Settle) না হওয়া পর্যন্ত ফাংশনের অভ্যন্তরীণ এক্সিকিউশন থামিয়ে রাখে।

              async-await-sequential.js
              Copy

```javascript
function getData(id) {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(`Data fetched for ID: ${id}`);
      resolve(200);
    }, 1000);
  });
}

// Async/Await এবং IIFE (Immediately Invoked Function Expression) সমাধান:
(async function getAllData() {
  console.log("Getting Data 1...");
  await getData(1);

  console.log("Getting Data 2...");
  await getData(2);

  console.log("Getting Data 3...");
  await getData(3);

  console.log("All data successfully fetched! ✓");
})();
```

## ৭. পদ্ধতির তুলনা ও বিবর্তন

| বৈশিষ্ট্য | Callback Hell | Promise Chaining | Async-Await |
| --- | --- | --- | --- |
| পঠনযোগ্যতা | খুবই জটিল ও পিরামিডের মতো বিশৃঙ্খল | কলব্যাক থেকে ভালো, তবে চেইনিং বড় হলে জটিল | সর্বোচ্চ পরিষ্কার ও সাধারণ কোডের মতো সহজবোধ্য |
| এরর হ্যান্ডলিং | প্রতিটি স্তরে আলাদা হ্যান্ডেল করা অত্যন্ত দুরূহ | সহজেই .catch() দিয়ে ধরা যায় | স্ট্যান্ডার্ড try...catch ব্লক দিয়ে পরিষ্কার হ্যান্ডলিং |
| ইন্ডাস্ট্রি স্ট্যান্ডার্ড | লেগ্যাসি / চরম পরিহার্য | মাঝারি গ্রহণযোগ্যতা | সর্বোচ্চ পছন্দের আধুনিক স্ট্যান্ডার্ড (Gold Standard) |

## ৮. সাধারণ ভুলসমূহ (Common Mistakes)

১. টাইমার ফাংশনে রেফারেন্সের বদলে ফাংশন কল দিয়ে ফেলা

                  setTimeout(sum(), 2000); // ❌ সাথে সাথেই রান করে যাবে!

                  setTimeout(sum, 2000); // ✅ ২ সেকেন্ড পর চলবে

২. async ফাংশনের বাইরে await ব্যবহার করা

সাধারণ স্ক্রিপ্ট ফাইলে সরাসরি `await getData()` লিখলে SyntaxError দেবে। এটিকে অবশ্যই কোনো `async function` অথবা async IIFE-এর ভেতরে রাখতে হবে।

৩. প্রমিজে resolve বা reject কল করতে ভুলে যাওয়া

কাজের শর্ত পূরণ হলে অবশ্যই `resolve(val)` বা ব্যর্থ হলে `reject(err)` কল করতে হবে, অন্যথায় প্রমিজটি চিরকালের জন্য *Pending* অবস্থায় আটকে থাকবে।

## ৯. দ্রুত রিভিশন (Quick Revision)

* ✓
                Asynchronous: মূল কোড ব্লক না করে সমান্তরালে ব্যাকগ্রাউন্ড কাজ চালানোর প্রক্রিয়া।

* ✓
                Promise States: pending ➔ fulfilled (resolved) অথবা rejected।

* ✓
                Async/Await: প্রমিজের ওপর তৈরি করা আধুনিক সিনট্যাক্টিক সুগার যা কোডকে সহজে পঠনযোগ্য করে।

* ✓
                IIFE: অবিলম্বে স্বয়ংক্রিয়ভাবে async ফাংশন রান করানোর জন্য (async () => { ... })() সিনট্যাক্স।

            ← Ch 08: Classes & Objects

            Next: Ch 10 — Fetch API & HTTP Requests
            →

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/callbacks-promises-async-await)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
