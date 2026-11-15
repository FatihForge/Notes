# ⚡ 08. Classes & Objects in JavaScript (অবজেক্ট, ক্লাস ও ইনহেরিট্যান্স)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

## ১. ক্লাস এবং অবজেক্ট কী? (What is it?)

জাভাস্ক্রিপ্ট শেখার যাত্রায় **Classes এবং Objects** অত্যন্ত গুরুত্বপূর্ণ একটি অধ্যায়। আপনি যখন বড় মাপের প্রজেক্ট বা স্কেলেবল (Scalable) ওয়েব অ্যাপ্লিকেশন তৈরি করবেন, তখন কোডকে সুশৃঙ্খল ও মডুলারভাবে সাজানোর জন্য এই কনসেপ্টগুলো অপরিহার্য। এছাড়া যেকোনো ফ্রন্ট-এন্ড বা ফুল-স্ট্যাক ইন্টারভিউতে 'প্রোটোটাইপ' এবং 'ইনহেরিটেন্স' থেকে প্রশ্ন আসা নিশ্চিত।

**অবজেক্টের সংজ্ঞা:** জাভাস্ক্রিপ্টে অবজেক্ট হলো এমন একটি সত্ত্বা (Entity) যার নিজস্ব **State** (Properties বা ভেরিয়েবল) এবং **Behavior** (Methods বা ফাংশন) থাকে।

সহজ কথায়, অবজেক্ট হলো ভেরিয়েবল এবং ফাংশনের একটি স্মার্ট পাত্র (Container)। এটি বাস্তব জগতের যেকোনো বস্তুর মতো কাজ করে যার নির্দিষ্ট তথ্য ও কাজ করার ক্ষমতা রয়েছে।

## ২. এটি কেন গুরুত্বপূর্ণ? (Why is it important?)

আমরা লিনিয়ার কোড (Linear Code) না লিখে অবজেক্ট ও ক্লাসের আর্কিটেকচার কেন ব্যবহার করব? এর মূল কারণ হলো **কার্যকারিতা (Efficiency) ও কোড রিইউজেবিলিটি**।

ধরা যাক, একটি কোম্পানির ৫০ জন কর্মীর প্রোফাইল এবং তাদের ট্যাক্স ক্যালকুলেট করার কোড লিখতে হবে। আপনি কি ৫০ বার আলাদা আলাদা ভেরিয়েবল এবং ক্যালকুলেশন ফাংশন লিখবেন? সেটি হবে চরম সময়সাপেক্ষ এবং কোড ডুপ্লিকেশনের কারণে মারাত্মক বাগের সম্ভাবনা তৈরি হবে।

**Classes এবং Objects আমাদের একটি Blueprint বা নকশা তৈরি করতে দেয়**। একবার নকশা তৈরি হয়ে গেলে আপনি চোখের পলকে একই ধরনের শত শত অবজেক্ট তৈরি করে ফেলতে পারবেন।

## ৩. মূল ধারণাসমূহ (Core Concepts)

### 3.1
              JavaScript Objects & The 'this' Keyword

অবজেক্টের ভেতর যখন আমরা কোনো মেথড লিখি, তখন সেই অবজেক্টের নিজস্ব প্রোপার্টিকে অ্যাক্সেস করার জন্য `this` কীওয়ার্ড ব্যবহার করতে হয়। এখানে `this.marks` মানে হলো সরাসরি `student.marks`-কে নির্দেশ করা।

                object-this.js
                Copy

```javascript
const student = {
  fullName: "Alvi Hasan",
  marks: 94.5,
  printMarks: function () {
    console.log("Student marks =", this.marks); // 'this' বর্তমান অবজেক্টকে রেফার করে
  },
};

student.printMarks(); // আউটপুট: Student marks = 94.5
```

### 3.2
              Prototypes (প্রোটোটাইপ ইনহেরিটেন্স)

জাভাস্ক্রিপ্টে প্রতিটি অবজেক্টের ভেতরে একটি ডিফল্ট হিডেন প্রোপার্টি থাকে যার নাম `__proto__`। এটি আসলে অন্য একটি প্যারেন্ট অবজেক্টের রেফারেন্স বা লিংক।

              💡 **Strategic Impact:** আপনি যখন কোনো সাধারণ অ্যারে তৈরি করেন, আপনি নিজে `push()` বা `pop()` কোড না লিখলেও সেগুলো কাজ করে—কারণ জাভাস্ক্রিপ্ট তার ইন্টারনাল Prototype চেইনের মাধ্যমে এই মেথডগুলো ডিফল্টভাবে ইনহেরিট করে দেয়।

                prototype-chain.js
                Copy

```javascript
const employee = {
  calcTax() {
    console.log("Standard tax rate is 10%");
  }
};

const karanArjun = {
  salary: 50000
};

// প্রোটোটাইপ সেট করা:
karanArjun.__proto__ = employee;

// karanArjun এখন employee-এর মেথড ব্যবহার করতে পারে:
karanArjun.calcTax(); // "Standard tax rate is 10%"
```

### 3.3
              Classes ও Constructor মেথড

ক্লাস হলো অবজেক্ট তৈরি করার আধুনিক টেমপ্লেট। আর **Constructor** হলো ক্লাসের ভেতরের একটি বিশেষ মেথড যা `new` কীওয়ার্ড দিয়ে অবজেক্ট তৈরির সাথে সাথেই স্বয়ংক্রিয়ভাবে এক্সিকিউট হয়।

                class-constructor.js
                Copy

```javascript
class ToyotaCar {
  constructor(brand, mileage) {
    this.brand = brand;
    this.mileage = mileage;
    console.log(`Creating new instance for: ${brand}`);
  }

  startEngine() {
    console.log(`${this.brand} engine started.`);
  }
}

// new কীওয়ার্ড মেমোরিতে নতুন অবজেক্ট তৈরি করে এবং কনস্ট্রাক্টর রান করায়:
let fortuner = new ToyotaCar("Fortuner", 12);
fortuner.startEngine();
```

### 3.4
              Inheritance এবং 'super' কীওয়ার্ড

প্যারেন্ট ক্লাস থেকে বৈশিষ্ট্য চাইল্ড ক্লাসে উত্তরাধিকার সূত্রে নিয়ে আসাই হলো ইনহেরিটেন্স। এর জন্য `extends` ব্যবহার করা হয়।
              আর চাইল্ড ক্লাসের কনস্ট্রাক্টর থেকে প্যারেন্ট ক্লাসের কনস্ট্রাক্টরকে কল করতে `super()` ব্যবহার করা হয়।

                inheritance.js
                Copy

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  eat() {
    console.log(`${this.name} is eating.`);
  }
}

class Engineer extends Person {
  constructor(name, department) {
    // ⚠️ চাইল্ড ক্লাসে 'this' ব্যবহারের পূর্বে অবশ্যই super() কল করতে হবে:
    super(name);
    this.department = department;
  }

  work() {
    console.log(`${this.name} builds web architectures.`);
  }
}

const eng = new Engineer("Naim", "Software Engineering");
eng.eat();
eng.work();
```

### 3.5
              Error Handling (try...catch): অ্যাপ ক্র্যাশ রোধ করা

কোড চলার সময় কোনো ত্রুটি হওয়ার আশঙ্কা থাকলে পুরো অ্যাপ্লিকেশন ক্র্যাশ হওয়া থেকে বাঁচাতে `try...catch` ব্লক ব্যবহার করা হয়।

                Error Boundary Flow

                  try { সন্দেহজনক কোড }

                ➔ এরর ঘটলে ➔

                  catch (err) { নিরাপদ হ্যান্ডলিং }

                ➔ এরপরও ➔

                  পরবর্তী কোড স্বাভাবিক চলবে ✓

                try-catch.js
                Copy

```javascript
let a = 10;

try {
  console.log(a + b); // 'b' ডিফাইন করা নেই! এরর ঘটবে
} catch (err) {
  console.warn("Caught an error gracefully:", err.message);
}

// এই লাইনটি স্বাভাবিকভাবে এক্সিকিউট হবে, অ্যাপ ক্র্যাশ করবে না:
console.log("Application continues running smoothly!");
```

## ৪. Object বনাম Class

| Object (অবজেক্ট) | Class (ক্লাস) |
| --- | --- |
| সরাসরি মেমোরিতে ডেটা এবং মেথড ধারণ করে এমন একটি একক ইনস্ট্যান্স। | একই ধরনের বহু অবজেক্ট তৈরি করার ছাঁচ বা ব্লুপ্রিন্ট (নকশা)। |
| ছোট প্রজেক্ট বা একক কাজের জন্য অবজেক্ট লিটারেল দ্রুত ও সুবিধাজনক। | যখন একই বৈশিষ্ট্যের শত শত অবজেক্ট বানাতে হয়, তখন ক্লাস ব্যবহার অপরিহার্য। |

## ৫. সাধারণ ভুলসমূহ (Common Mistakes)

১. new কীওয়ার্ড দিতে ভুলে যাওয়া

ক্লাস থেকে অবজেক্ট তৈরির সময় `new` না লিখলে জাভাস্ক্রিপ্ট বুঝবে না যে আপনি একটি নতুন ইনস্ট্যান্স তৈরি করছেন।

                ✅ const car = new ToyotaCar("Camry", 15);

২. চাইল্ড ক্লাসে super() কল করতে ভুলে যাওয়া

চাইল্ড ক্লাসের কনস্ট্রাক্টরে `super()` কল না করে `this` অ্যাক্সেস করতে গেলে জাভাস্ক্রিপ্ট `ReferenceError: Must call super constructor in derived class before accessing 'this'` নিক্ষেপ করবে।

## ৬. দ্রুত রিভিশন (Quick Revision)

* ✓
                Objects: প্রোপার্টি (State) এবং মেথড (Behavior)-এর সমন্বিত কাঠামো।

* ✓
                Classes: অবজেক্ট তৈরির ব্লুপ্রিন্ট বা টেমপ্লেট।

* ✓
                Inheritance: extends দিয়ে প্যারেন্ট ক্লাসের কার্যক্ষমতা চাইল্ড ক্লাসে আনা হয়।

* ✓
                Error Safety: রান-টাইমে কোড ক্র্যাশ হওয়া রোধ করতে সম্ভাব্য ঝুঁকিপূর্ণ অংশগুলো সর্বদা try...catch দিয়ে ঘিরে রাখুন।

            ← Ch 07: Events in JavaScript

            Next: Ch 09 — Async/Await & Promises
            →

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/classes-and-objects)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
