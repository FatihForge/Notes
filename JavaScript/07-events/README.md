# ⚡ 07. Events in JavaScript (ইভেন্ট হ্যান্ডলিং ও লিসেনারস)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

## ১. ইভেন্ট কী? (What is it?)

JavaScript-এ **Events হলো ইউজার এবং ওয়েবসাইটের মধ্যে যোগাযোগের প্রধান সেতু**। একটি সাধারণ স্ট্যাটিক ওয়েবপেজকে ইন্টারঅ্যাক্টিভ বা প্রাণবন্ত করতে ইভেন্টের ভূমিকা অনস্বীকার্য।

টেকনিক্যাল সংজ্ঞানুযায়ী, **কোনো অবজেক্টের অবস্থার পরিবর্তন হওয়াকেই (Change in the state of an object) ইভেন্ট বলা হয়**। যেমন—ইউজার যখন কোনো বাটনে ক্লিক করে, মাউস নাড়ায় কিংবা ইনপুট ফিল্ডে কিছু টাইপ করে, ব্রাউজার তাৎক্ষণিকভাবে সেই অ্যাকশন শনাক্ত করে এবং আমাদের জাভাস্ক্রিপ্ট কোডকে একটি সংকেত বা নোটিফিকেশন পাঠায়।

ইভেন্ট হ্যান্ডলিং নিখুঁতভাবে আয়ত্ত করাই হলো **Reactive Web Application** তৈরির প্রথম ও প্রধান সোপান।

## ২. কেন এটি গুরুত্বপূর্ণ? (Why is it important?)

একজন ফুল-স্ট্যাক ডেভেলপার হিসেবে মনে রাখবেন: **ইভেন্ট হ্যান্ডলিং একটি সাধারণ "UI" (User Interface)-কে বাস্তব জগতের উপযোগী "UX" (User Experience)-এ রূপান্তর করে**।

* ইন্টারঅ্যাক্টিভিটি: ইউজারের যেকেনো ক্লিকের পর লাইভ পেজ কন্টেন্ট আপডেট করা।

* ফর্ম সাবমিশন: লগইন বা সাইন-আপ বাটনে ক্লিক করলে ডেটা প্রসেস ও সার্ভারে পাঠানো।

* রিয়েল-টাইম ফিডব্যাক: মাউস হোভার করলে ভিজ্যুয়াল অ্যানিমেশন ও স্টাইল পরিবর্তন।

* স্টেট ম্যানেজমেন্ট: ডার্ক মোড অন/অফ করা বা ড্রপডাউন মেনু টগল করা।

## ৩. ইভেন্টের প্রকারভেদ ও হ্যান্ডলিং

              Mouse Events

click, dblclick, mouseover, mouseout, mousedown

              Keyboard Events

keydown (কী চাপ দেওয়া), keyup (কী ছেড়ে দেওয়া)

              Form Events

submit (ফর্ম জমা), input, change, focus, blur

              Window Events

load, resize, scroll, pageshow

### addEventListener (The Professional Standard)

HTML ট্যাগে ইনলাইন হ্যান্ডলার (`onclick="..."`) লেখা খারাপ অনুশীলন, কারণ এটি মার্কআপ ও লজিক মিশিয়ে ফেলে। আবার JS নোড প্রপার্টি (`node.onclick = fn`) দিলে কেবল একটিমাত্র ফাংশন রান করানো যায়—পরেরটি আগেরটিকে ওভাররাইট করে দেয়।
            আধুনিক স্ট্যান্ডার্ড হলো `addEventListener`।

              event-listeners.js
              Copy

```javascript
const btn = document.querySelector("#submit-btn");

// ১ নম্বর লিসনার:
btn.addEventListener("click", () => {
  console.log("১ম কাজ: বাটন ক্লিক হয়েছে!");
});

// ২ নম্বর লিসনার (একই ইভেন্টে নিরাপদে যুক্ত করা যায়):
btn.addEventListener("click", () => {
  console.log("২য় কাজ: অ্যানালিটিক্স ডেটা ট্র্যাক হচ্ছে!");
});
```

## ৪. ইভেন্ট অবজেক্ট (The Event Object - e)

যখনই কোনো ইভেন্ট ঘটে, ব্রাউজার স্বয়ংক্রিয়ভাবে একটি **"বিশেষ রিপোর্ট অবজেক্ট"** তৈরি করে কলব্যাক ফাংশনে আর্গুমেন্ট (সাধারণত `e` বা `event` নামে) হিসেবে পাঠায়।

              e.type
              ইভেন্টের নাম (যেমন: "click")

              e.target
              যে নির্দিষ্ট এলিমেন্টে ইভেন্টটি ঘটেছে

              e.clientX / clientY
              স্ক্রিনের ঠিক কোন কোঅর্ডিনেটে মাউস ক্লিক হয়েছে

              event-object-example.js
              Copy

```javascript
const box = document.querySelector("#canvas-box");

box.addEventListener("click", (e) => {
  console.log("Event Type:", e.type);        // "click"
  console.log("Target Element:", e.target);  //

  console.log(`X: ${e.clientX}, Y: ${e.clientY}`); // ব্রাউজার গেমসে অত্যন্ত দরকারী
});
```

## ৫. ইভেন্ট লিসনার রিমুভ করা (removeEventListener)

### ⚡ The Golden Rule (Senior Engineering Depth):

সরাসরি অ্যানোনিমাস ফাংশন ব্যবহার করলে `removeEventListener` **কখনোই কাজ করবে না!** কারণ জাভাস্ক্রিপ্ট ইঞ্জিনে প্রতিটি নতুন অ্যানোনিমাস ফাংশন মেমোরিতে সম্পূর্ণ ভিন্ন লোকেশন (Different Memory Address) দখল করে। ব্রাউজার তখন পূর্বের ফাংশনটিকে মেলাতে পারে না।

              remove-listener.js
              Copy

```javascript
// ❌ ভুল পদ্ধতি (কখনোই কাজ করবে না):
// btn.removeEventListener("click", () => { ... });

// ✅ সঠিক পদ্ধতি: ফাংশনের রেফারেন্স নির্দিষ্ট করা
const handleClick = () => {
  console.log("Handled safely!");
};

// লিসনার যুক্ত করা:
btn.addEventListener("click", handleClick);

// কাজ শেষে লিসনার সঠিকভাবে মুছে ফেলা:
btn.removeEventListener("click", handleClick);
```

## ৬. JS Node Property বনাম Event Listeners

| বৈশিষ্ট্য | node.onclick | addEventListener |
| --- | --- | --- |
| একাধিক হ্যান্ডলার | সম্ভব নয় (পরেরটি আগেরটিকে Overwrite করে দেয়) | সম্পূর্ণ সম্ভব (একাধিক ফাংশন স্বাধীনভাবে চলে) |
| রিমুভ সুবিধা | সরাসরি কোনো মেথড নেই (btn.onclick = null) | removeEventListener দিয়ে চমৎকারভাবে নিয়ন্ত্রিত |
| প্রফেশনাল স্ট্যান্ডার্ড | ছোট খসড়া বা পরীক্ষামূলক কাজের জন্য চলে | উৎপাদনমুখী ও মডুলার কোডের বেস্ট প্র্যাকটিস |

## ৭. সাধারণ ভুলসমূহ (Common Mistakes)

১. onclick ওভাররাইট হওয়া খেয়াল না করা

                  btn.onclick = () => { log("A"); };
                  btn.onclick = () => { log("B"); };
                  // ❌ কেবল "B" প্রিন্ট হবে! "A" মুছে গেছে

                  btn.addEventListener("click", () => log("A"));
                  btn.addEventListener("click", () => log("B"));
                  // ✅ "A" এবং "B" উভয়ই রান করবে

## ৮. দ্রুত রিভিশন (Quick Revision)

* ✓
                মানসিক মডেল: ইভেন্ট হলো একটি "Notification" এবং ইভেন্ট হ্যান্ডলার হলো সেই নোটিফিকেশন পাওয়ার পর সিস্টেমের গৃহীত "Action"।

* ✓
                স্ট্যান্ডার্ড: পেশাদার কোডিংয়ে সর্বদা addEventListener ব্যবহার করুন।

* ✓
                রিমুভাল শর্ত: লিসনার সরানোর সময় হ্যান্ডলার ফাংশনের মেমোরি রেফারেন্স হুবহু একই হতে হবে।

            ← Ch 06: DOM & Manipulation

            Next: Ch 08 — Classes & Objects
            →

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/events-in-javascript)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
