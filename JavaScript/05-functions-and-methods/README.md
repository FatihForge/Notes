# ⚡ 05. Functions & Methods in JavaScript (ফাংশনস, অ্যারো ফাংশন ও HOF)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

## ১. ফাংশন কী? (What is it?)

প্রোগ্রামিং শেখার যাত্রায় **Functions এবং Methods হলো সবচেয়ে গুরুত্বপূর্ণ এবং শক্তিশালী বিল্ডিং ব্লক**। কৌশলগত দিক থেকে একটি ফাংশনকে আপনি একটি **"Black Box"** হিসেবে কল্পনা করতে পারেন। এর কাজের মূল ধারা হলো:

              Input (Parameters) ➔ Logic Processing ➔ Output (Return Value)

সহজ কথায়, **ফাংশন হলো একগুচ্ছ কোডের ব্লক (Block of Code) যা কোনো নির্দিষ্ট উদ্দেশ্যমূলক কাজ সম্পন্ন করার জন্য ডিজাইন করা হয়**।

মজার বিষয় হলো, আপনি ইতোমধ্যেই অজান্তেই অনেক ফাংশন ব্যবহার করেছেন! যেমন: কনসোলে কিছু দেখানোর জন্য `console.log()`, স্ট্রিং বড় হাতের করার জন্য `toUpperCase()`, কিংবা অ্যারেতে আইটেম যোগ করতে `push()`। এগুলো সবই প্রি-ডিফাইন্ড (Built-in) ফাংশন।

## ২. এটি কেন গুরুত্বপূর্ণ? (Why is it important?)

ফাংশন ব্যবহারের প্রধান উদ্দেশ্য হলো কোড থেকে **Redundancy বা পুনরাবৃত্তি দূর করা** এবং কোডবেসকে মডুলার রাখা।

### 👨‍🍳 বাবুর্চি ও রেসিপির অ্যানালজি

মনে করুন আপনার ব্যক্তিগত টিমে একজন শেফ বা বাবুর্চি আছে:

* Function Definition: বাবুর্চিকে কাজে নিয়োগ দিয়ে বিরিয়ানি রান্নার রেসিপি ও নিয়ম শিখিয়ে রাখা (লজিক সেট করা)।

* Function Call (Invocation): বাবুর্চি কিন্তু সারাদিন অকারণে রান্না করবে না। যখনই আপনার ক্ষুধা লাগবে, আপনি তাকে ডাকবেন (cookBiryani()) এবং সে রান্না শেষ করে সুস্বাদু খাবার আপনার টেবিলে পরিবেশন (Return) করবে।

## ৩. মূল ধারণাসমূহ (Core Concepts)

### 3.1
              Function Definition & Function Call

ফাংশন ডিফাইন করা হলো কাজের বর্ণনা তৈরি করা, আর কল করা হলো বাস্তবে সেই কাজটিকে কার্যকর করা।

                function-basics.js
                Copy

```javascript
// 1. Function Definition:
function greetUser() {
  console.log("Welcome to JavaScript Study Notes!");
}

// 2. Function Call (Invoke):
greetUser();
```

### 3.2
              Parameters বনাম Arguments

                Parameters (প্যারামিটার)

ফাংশন সংজ্ঞায়িত করার সময় প্রথম বন্ধনীর ভেতরে লেখা প্লেসহোল্ডার ভেরিয়েবলসমূহ (যেমন: `x, y`)।

                Arguments (আর্গুমেন্ট)

ফাংশন কল করার সময় প্যারামিটারের বিপরীতে পাঠানো বাস্তব মান বা Values (যেমন: `5, 10`)।

### 3.3
              Return কীওয়ার্ড এবং Unreachable Code সতর্কতা

ফাংশন কাজ শেষে যে ফলাফলটি ফেরত দেয়, তাকে **Return Value** বলে। মনে রাখবেন, `return` স্টেটমেন্ট এক্সিকিউট হওয়ার সাথে সাথে ফাংশন তাৎক্ষণিকভাবে বন্ধ হয়ে যায়। এর নিচে কোনো কোড লেখা থাকলে তা কখনো চলবে না (একে **Unreachable Code** বলা হয়)।

                return-value.js
                Copy

```javascript
function calculateTotal(price, tax) {
  let finalAmount = price + tax;
  return finalAmount; // মান ফেরত পাঠাচ্ছে

  console.log("This will NEVER run!"); // Unreachable code warning
}

let bill = calculateTotal(500, 75);
console.log(bill); // 575
```

### 3.4
              Arrow Functions (ES6+)

আধুনিক জাভাস্ক্রিপ্টে ছোট ও সংক্ষিপ্ত ফাংশন লেখার স্ট্যান্ডার্ড সিনট্যাক্স হলো Arrow Function:

                arrow-functions.js
                Copy

```javascript
// রেগুলার ফাংশন:
const multiplyRegular = function (a, b) {
  return a * b;
};

// আধুনিক Arrow Function:
const multiply = (a, b) => a * b;

console.log(multiply(4, 5)); // 20
```

## ৪. Callbacks এবং Higher-Order Functions (HOF)

JavaScript-এ ফাংশন হলো **First-Class Citizens**। অর্থাৎ ভেরিয়েবলের মতো ফাংশনকেও এক জায়গা থেকে অন্য জায়গায় আর্গুমেন্ট হিসেবে পাঠানো যায়।

* Callback: যখন কোনো একটি ফাংশনকে অপর একটি ফাংশনের ভেতর Argument হিসেবে পাস করা হয়।

* Higher Order Function (HOF): যে ফাংশন অন্য কোনো ফাংশনকে প্যারামিটার হিসেবে গ্রহণ করে অথবা কোনো ফাংশনকে আউটপুট হিসেবে রিটার্ন করে।

### অ্যারের ৪টি অত্যাবশ্যকীয় Built-in HOFs:

#### A. forEach

অ্যারের প্রতিটি উপাদানের জন্য একটি নির্দিষ্ট লুপ চালায়। এটি কোনো নতুন অ্যারে রিটার্ন করে না।

```javascript
let nums = [1, 2, 3];
nums.forEach((val, index) => {
  console.log(`Value: ${val}, Index: ${index}`);
});
```

#### B. map (Transformation)

প্রতিটি উপাদানের ওপর অপারেশন চালিয়ে **একটি সম্পূর্ণ নতুন পরিবর্তিত অ্যারে রিটার্ন করে**। মূল অ্যারে অক্ষত থাকে।

```javascript
let numbers = [2, 4, 6];
let squared = numbers.map(val => val * val);
console.log(squared); // [4, 16, 36]
```

#### C. filter (চায়ের ছাঁকনি)

একটি নির্দিষ্ট শর্তের ওপর ভিত্তি করে উপাদান ছাঁকন করে। শর্ত সত্য (True) হলে উপাদানটি নতুন অ্যারেতে থাকে, অন্যথায় বাদ পড়ে যায়।

```javascript
let scores = [45, 82, 91, 33, 76];
let passingScores = scores.filter(score => score >= 50);
console.log(passingScores); // [82, 91, 76]
```

#### D. reduce (Accumulator)

যখন পুরো অ্যারে প্রসেস করে **একটি মাত্র একক আউটপুট মান** (যেমন: মোট যোগফল বা সর্বোচ্চ সংখ্যা) প্রয়োজন হয়।

```javascript
let cartPrices = [120, 450, 300];
let grandTotal = cartPrices.reduce((accumulator, current) => accumulator + current, 0);
console.log(grandTotal); // 870
```

## ৫. forEach বনাম map

| forEach | map |
| --- | --- |
| সাধারণত অ্যারের প্রতিটি আইটেমে কোনো পার্শ্ব-প্রতিক্রিয়া (যেমন console.log বা DOM আপডেট) চালানোর জন্য ব্যবহৃত হয়। | মূলত ডেটা রূপান্তর করে একটি নতুন মডিফাইড অ্যারে তৈরি করার জন্য ব্যবহৃত হয়। |
| এটি কোনো কিছু রিটার্ন করে না (রিটার্ন মান undefined)। | এটি সর্বদা রূপান্তরিত উপাদানের একটি নতুন অ্যারে রিটার্ন করে। |

## ৬. সাধারণ ভুলসমূহ (Common Mistakes)

১. return স্টেটমেন্টের নিচে কোড লেখা (Unreachable Code)

রিটার্ন করার সাথে সাথে ফাংশনের লাইফসাইকেল শেষ হয়ে যায়, এর নিচের যেকোনো স্টেটমেন্ট ইঞ্জিন উপেক্ষা করবে।

২. map() ব্যবহার করে ফলাফল সেভ না করা

                  nums.map(x => x * 2); // ❌ নতুন অ্যারে বাতাসে হারিয়ে গেল!

                  const doubled = nums.map(x => x * 2); // ✅ সংরক্ষিত

## ৭. কুইক রিভিশন (Quick Revision)

* ✓
                মানসিক মডেল: ফাংশন হলো একটি রিইউজেবল মেশিন; মেথড হলো এমন ফাংশন যা কোনো অবজেক্টের সাথে যুক্ত থাকে।

* ✓
                Parameters vs Arguments: প্যারামিটার থাকে ফাংশন সংজ্ঞায়, আর্গুমেন্ট থাকে ফাংশন কলে।

* ✓
                HOF সামারি: forEach লুপ চালানোর জন্য, map নতুন অ্যারে বানাতে, filter উপাদান ছাঁকতে, আর reduce একক মান বের করতে।

            ← Ch 04: Arrays in JavaScript

            Next: Ch 06 — DOM & Manipulation
            →

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/functions-and-methods)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
