# ⚡ 02. Operators & Conditional Statements (অপারেটর ও শর্তসাপেক্ষ স্টেটমেন্ট)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

## ১. কী এটা? (What is it?)

প্রোগ্রামিংয়ের জগতে **লজিক বা যুক্তি হলো যেকোনো কোডের প্রাণ**। বাস্তব জীবনে আমরা বিভিন্ন তথ্যের ভিত্তিতে সিদ্ধান্ত নিই—যেমন বয়স ১৮ বছর বা তার বেশি হলে ভোটার আইডি কার্ড করা যাবে, কিংবা পরীক্ষায় পাসিং মার্কস পেলে পরবর্তী ক্লাসে উত্তীর্ণ হওয়া যাবে।

ঠিক তেমনি আমাদের কোডকেও বুদ্ধিমত্তার সাথে সিদ্ধান্ত নিতে হয়। JavaScript-এ এই সিদ্ধান্ত নেওয়ার ক্ষমতা প্রদান করে **Conditional Statements** (যা অ্যাপ্লিকেশনের মস্তিষ্কের মতো কাজ করে)।

আর এই সিদ্ধান্ত নেওয়ার জন্য ডেটা নিয়ে যে টুলসগুলো কাজ করে বা ডেটা পরিবর্তন করে, তাদের বলা হয় **Operators**। সহজ কথায়:

### 🔧 Operators (অপারেটর)

যেসব চিহ্ন বা টুলস দিয়ে আমরা ডেটা যোগ, বিয়োগ, তুলনা বা মূল্যায়ন করি (যেমন: `+`, `===`, `&&`)।

### 🚦 Conditionals (কন্ডিশনাল)

যেসব নিয়ম বা শাখা কোডের পরবর্তী এক্সিকিউশন পথ নির্ধারণ করে (যেমন: `if`, `else if`, `switch`)।

## ২. কেন এটি গুরুত্বপূর্ণ? (Why is it important?)

একটি ওয়েবসাইটের ইন্টারঅ্যাক্টিভিটির মূল মেরুদণ্ড হলো এই লজিক্যাল কনসেপ্টগুলো:

* ডেটা ক্যালকুলেশন: ইউজারের শপিং কার্ট টোটাল, ডিসকাউন্ট, গেম স্কোর বা পেমেন্টের হিসাব করার জন্য অপারেটর অপরিহার্য।

* ইউজার ইনপুট হ্যান্ডলিং: ইউজারের প্রদত্ত বয়স, ইমেইল বা পাসওয়ার্ডের ওপর ভিত্তি করে সঠিক সিদ্ধান্ত নেওয়া ও যাচাই করা।

* লজিক রিইউজেবিলিটি: একবার একটি নিখুঁত লজিক তৈরি করে দিলে সেটি বারবার হাজারো ভিন্ন ভিন্ন তথ্যের ওপর স্বয়ংক্রিয়ভাবে প্রয়োগ করা যায়।

## ৩. মূল ধারণাসমূহ (Core Concepts)

### 3.1
              Comments (কোড কমেন্ট)

কমেন্ট হলো কোডের এমন অংশ যা জাভাস্ক্রিপ্ট ইঞ্জিন রান করার সময় সম্পূর্ণ এড়িয়ে যায়। কোড কেন লেখা হয়েছে এবং কীভাবে কাজ করছে তা ডেভেলপারদের বোঝার সুবিধার জন্য কমেন্ট ব্যবহার করা হয়।

                comments-example.js
                Copy

```javascript
// Single-line Comment: এক লাইনের নোটের জন্য
let score = 95;

/* Multi-line Comment:
   একাধিক লাইনের ব্যাখ্যা বা
   কোনো ব্লক সাময়িকভাবে বন্ধ রাখার জন্য */
console.log("Player score:", score);
```

### 3.2
              Arithmetic Operators (গাণিতিক অপারেটর)

গাণিতিক হিসাবের অপারেটর: `+`, `-`, `*`, `/`, `%` (মডুলাস/ভাগশেষ) এবং `**` (এক্সপোনেনশিয়েশন/ঘাত)।

              💡 **মডুলাস (%) টিপ:** ভাগশেষ ০ হলে সংখ্যাটি নিঃশেষে বিভাজ্য। তাই কোনো সংখ্যা জোড় (Even) নাকি বিজোড় (Odd) তা বের করতে `num % 2 === 0` বহুল ব্যবহৃত হয়।

                arithmetic.js
                Copy

```javascript
let a = 7, b = 3;

console.log(a + b);  // যোগ: 10
console.log(a % b);  // ভাগশেষ: 1 (৭ কে ৩ দিয়ে ভাগ করলে ১ অবশিষ্ট থাকে)
console.log(a ** b); // ৭ এর পাওয়ার ৩ (7^3): 343
```

### 3.3
              Unary Operators: Pre vs Post Increment

একটিমাত্র অপারেন্ডের মান ১ বাড়াতে (`++`) বা ১ কমাতে (`--`) ইউনারি অপারেটর ব্যবহৃত হয়।

                Pre-increment (++a)
                আগে ভেরিয়েবলের মান ১ বৃদ্ধি পায়, তারপর ওই লাইনে বর্ধিত মান ব্যবহৃত হয়।

                Post-increment (a++)
                বর্তমান লাইনে পুরনো মানটিই ব্যবহৃত হয়, কাজ শেষ হওয়ার পর অভ্যন্তরীণভাবে মান ১ বৃদ্ধি পায়।

                unary-comparison.js
                Copy

```javascript
let x = 5;
console.log(++x); // Output: 6 (আগে মান বেড়েছে)

let y = 5;
console.log(y++); // Output: 5 (এই লাইনে পুরনো মান প্রদর্শন)
console.log(y);   // Output: 6 (পরবর্তী লাইনে বর্ধিত মান)
```

### 3.4
              Assignment Operators (অ্যাসাইনমেন্ট অপারেটর)

ভেরিয়েবলে মান জমা রাখতে ব্যবহৃত হয়। মনে রাখবেন, অ্যাসাইনমেন্ট অপারেশনে ডেটা প্রবাহ সবসময় **ডান থেকে বামে (Right to Left)** হয়।

                assignment.js
                Copy

```javascript
let count = 10;
count += 5;  // count = count + 5 (15)
count *= 2;  // count = count * 2 (30)
count -= 10; // count = count - 10 (20)
console.log(count); // 20
```

### 3.5
              Comparison Operators (কম্প্যারিজন অপারেটর)

দুটি মানের তুলনা করে এবং ফলাফল হিসেবে বুলিয়ান (`true` অথবা `false`) প্রদান করে।

| Operator | বর্ণনা | উদাহরণ | ফলাফল |
| --- | --- | --- | --- |
| == | Loose Equality (শুধু মান তুলনা করে, টাইপ কনভার্ট করে নেয়) | 5 == "5" | true |
| === | Strict Equality (মান এবং ডেটা টাইপ—উভয়ই চেক করে) | 5 === "5" | false |
| !== | Strict Not Equal | 5 !== "5" | true |
| >, =, = 5 | true |

### 3.6
              Logical Operators (লজিক্যাল অপারেটর)

একাধিক শর্ত বা এক্সপ্রেশন একসাথে যাচাই করার জন্য লজিক্যাল অপারেটর ব্যবহৃত হয়:

                && (AND)
                সবগুলো শর্তই সত্য (True) হলে তবেই সামগ্রিক ফলাফল True হবে।

                || (OR)
                যেকোনো একটি শর্ত সত্য (True) হলেই ফলাফল True হবে।

                ! (NOT)
                বুলিয়ান মান উল্টে দেয় (True কে False এবং False কে True বানায়)।

### 3.7
              Conditional Statements: if, else if, else

কোডে 'যদি-তবে' লজিক প্রয়োগের মূল হাতিয়ার। প্রথম শর্তটি `if` চেক করে, পরবর্তী বিকল্প শর্তসমূহ `else if` দেখে এবং কোনো শর্তই না মিললে ফলব্যাক হিসেবে `else` এক্সিকিউট হয়।

                Execution Flow Diagram

                  score = 75

                →

                  if (score >= 90) [False]

                →

                  else if (score >= 70) [True] → Grade 'B'

যেকোনো একটি শর্ত সত্য (True) হয়ে গেলে জাভাস্ক্রিপ্ট ইঞ্জিন সেই ব্লকটি সম্পন্ন করে বাকি সব লজিক স্কিপ করে বের হয়ে যায়।

                grading-system.js
                Copy

```javascript
let score = 75;

if (score >= 90) {
  console.log("Grade: A+");
} else if (score >= 70) {
  console.log("Grade: B"); // এটি এক্সিকিউট হবে
} else {
  console.log("Needs Improvement");
}
```

### 3.8
              Ternary Operator (টারনারি অপারেটর)

এটি `if-else`-এর একটি স্মার্ট এবং সংক্ষিপ্ত এক লাইনের রূপান্তর। এতে ৩টি অপারেন্ড থাকে: `condition ? trueResult : falseResult`।

                ternary.js
                Copy

```javascript
let age = 19;
let message = age >= 18 ? "Eligible to vote" : "Not eligible";

console.log(message); // "Eligible to vote"
```

### 3.9
              Switch Statement (সুইচ স্টেটমেন্ট)

যখন একটি নির্দিষ্ট ভ্যালুকে অনেকগুলো সম্ভাব্য মানের সাথে মেলাতে হয়, তখন একাধিক `else if`-এর বদলে `switch` ব্যবহার কোডকে অনেক বেশি পরিষ্কার ও রিডেবল রাখে।

                switch-case.js
                Copy

```javascript
let dayNumber = 3;

switch (dayNumber) {
  case 1:
    console.log("Monday");
    break;
  case 2:
    console.log("Tuesday");
    break;
  case 3:
    console.log("Wednesday"); // ম্যাচ করবে
    break;
  default:
    console.log("Other day");
}
```

## ৪. গুরুত্বপূর্ণ মেথড ও কীওয়ার্ড

| Name | কাজ / Purpose | Important Note |
| --- | --- | --- |
| alert() | ব্রাউজারে একটি পপ-আপ মেসেজ বক্স দেখায়। | ইউজারকে তাৎক্ষণিক সতর্কবার্তা দিতে ব্যবহৃত হয়। |
| prompt() | ইউজার থেকে ইনপুট নেওয়ার ডায়ালগ বক্স দেখায়। | সতর্কতা: এটি ইউজারের যেকোনো ইনপুটকে সর্বদা String হিসেবে রিটার্ন করে। |
| break | সুইচ বা লুপ থেকে তাৎক্ষণিকভাবে বের হওয়ার জন্য। | সুইচ কেসে ব্রেক না দিলে তার নিচের সব কেস অটোমেটিক এক্সিকিউট হয়ে যায় (Fall-through)। |

## ৫. গুরুত্বপূর্ণ পার্থক্য (Comparisons)

### == বনাম === (Loose vs Strict Equality)

| == (Equal to) | === (Strict Equal to) |
| --- | --- |
| শুধুমাত্র মান (Value) তুলনা করে। টাইপ আলাদা হলে ইন্টারনালি কনভার্ট করে। | মান (Value) এবং ডেটা টাইপ (Type) উভয়ই কঠোরভাবে চেক করে। |
| 5 == "5" → true | 5 === "5" → false |

### Pre-increment (++a) বনাম Post-increment (a++)

| Operator | Value in Current Line | Actual Variable Value |
| --- | --- | --- |
| ++a (Pre) | আগে বেড়ে সরাসরি ৬ হয় (Updated) | ৬ |
| a++ (Post) | পুরনো মান ৫-ই প্রদর্শিত হয় | ৬ (পরবর্তী লাইনের জন্য বৃদ্ধি পায়) |

## ৬. সাধারণ ভুলসমূহ (Common Mistakes)

১. Assignment vs Comparison ট্র্যাপ

                  ❌ if (a = 5) { ... } // মান অ্যাসাইন হয়ে যাবে!

                  ✅ if (a === 5) { ... } // সঠিক তুলনা

একটি সমান (=) মান বসানোর জন্য, আর তিনটি সমান (===) মানের সঠিক তুলনা করার জন্য।

২. Switch Statement-এ break ভুলে যাওয়া (Fall-through)

সুইচ স্টেটমেন্টে কোনো কেস মিলার পর যদি `break` না দেন, তবে জাভাস্ক্রিপ্ট পরবর্তী কেসগুলোও এক্সিকিউট করে ফেলে।

                ✅ সর্বদা প্রতিটি case ব্লকের শেষে break; ব্যবহার নিশ্চিত করুন।

## ৭. কুইক রিভিশন (Quick Revision)

* ✓
                অপারেটর: ডেটা পরিবর্তন ও মূল্যায়নের হাতিয়ার (যেমন: +, ===, &&, ? :)।

* ✓
                কন্ডিশনাল: সিদ্ধান্ত গ্রহণের স্ট্রাকচার (যেমন: if-else, switch)।

* ✓
                মানসিক মডেল (Mental Model): কন্ডিশনাল স্ট্রাকচারকে রাস্তার একটি মোড় বা 'Fork in the road' হিসেবে কল্পনা করুন—আপনার শর্ত ঠিক করে দেয় কোড কোন পথে ধাবিত হবে।

            ← Ch 01: Basics, Variables & Types

            Next: Ch 03 — Loops & Strings
            →

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/operators-conditional-statements)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
