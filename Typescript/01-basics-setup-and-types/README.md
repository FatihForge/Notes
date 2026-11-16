# 🔷 01. TypeScript বেসিকস: পরিচিতি, এনভায়রনমেন্ট সেটআপ, কম্পাইলেশন ও টাইপ সিস্টেম

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 TypeScript ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. শিক্ষার উদ্দেশ্য (Learning Objectives)

এই পাঠটি সম্পন্ন করার পর আপনি:
- **TypeScript কী** এবং কেন এটি JavaScript-এর একটি **সুপারসেট (Superset)** তা স্পষ্টভাবে ব্যাখ্যা করতে পারবেন।
- JavaScript-এর ডায়নামিক প্রকৃতির সীমাবদ্ধতা এবং TypeScript কীভাবে কোড লেখার সময়ই (Compile-time) বাগ প্রতিরোধ করে তা অনুধাবন করতে পারবেন।
- পিসিতে TypeScript ইনস্টল করা এবং প্রজেক্টের জন্য `tsconfig.json` কনফিগার করতে পারবেন।
- `rootDir` এবং `outDir` ব্যবহারের মাধ্যমে প্রফেশনাল ফাইল স্ট্রাকচার তৈরি করতে পারবেন।
- TypeScript কোডকে JavaScript-এ কম্পাইল (`tsc`) এবং লাইভ ডেভেলপমেন্টের জন্য `--watch` মোড চালু রাখতে পারবেন।
- **Type Inference**, **Explicit Types**, **Union Types (`|`)** এবং **`any`** টাইপের সঠিক ব্যবহার ও পার্থক্য ব্যাখ্যা করতে পারবেন।

---

## 📖 ২. ভূমিকা (Introduction)

> [!NOTE]
> **JavaScript-এর সমস্যা ও TypeScript-এর আবির্ভাব:**  
> JavaScript মূলত তৈরি হয়েছিল ছোটখাটো ইন্টারঅ্যাকশনের জন্য একটি **Dynamically Typed** স্ক্রিপ্টিং ভাষা হিসেবে। সেখানে কোনো ভ্যারিয়েবলের টাইপ আগে থেকে নির্ধারণ করতে হয় না। কিন্তু প্রজেক্ট যখন বড় ও জটিল হয় (Enterprise Scale), তখন এই নমনীয়তাই কাল হয়ে দাঁড়ায়। সামান্য টাইপের অমিল বা স্পেলিং ভুলের কারণে কোড লেখার সময় কোনো এরর আসে না, কিন্তু ব্যবহারকারীর হাতে লাইভ সার্ভারে গিয়ে রানটাইমে অ্যাপ্লিকেশন ক্র্যাশ করে!

এই সমস্যার সমাধানে Microsoft উদ্ভাবন করে **TypeScript**।

TypeScript কোনো সম্পূর্ণ ভিন্ন বা নতুন ভাষা নয়—এটি হলো **JavaScript-এর একটি সুপারসেট (Superset)**। অর্থাৎ, বৈধ JavaScript কোডের সাথে একটি শক্তিশালী **স্ট্যাটিক টাইপ চেকিং সিস্টেম (Static Type Checker)** যুক্ত করলেই তা TypeScript হয়ে যায়।

```text
  ┌─────────────────────────────────────────────────────────────┐
  │                        TypeScript                           │
  │   ┌─────────────────────────────────────────────────────┐   │
  │   │                   JavaScript                        │   │
  │   │   (ECMAScript: Variables, Functions, ES6+, Objects) │   │
  │   └─────────────────────────────────────────────────────┘   │
  │   + Static Types, Interfaces, Generics, Enums, Tuples       │
  │   + Compile-Time Error Checking & Rich IntelliSense         │
  └─────────────────────────────────────────────────────────────┘
```

---

## 💡 ৩. মূল ধারণাসমূহ (Core Concepts)

### ক. Statically Typed বনাম Dynamically Typed
- **Dynamically Typed (JavaScript):** ভ্যারিয়েবলের মান যখন রানটাইমে এক্সিকিউট হয় তখন তার টাইপ নির্ধারিত হয়।
- **Statically Typed (TypeScript):** কোড লেখার সময়ই প্রতিটি ভ্যারিয়েবল, প্যারামিটার এবং ফাংশন রিটার্নের টাইপ কঠোরভাবে যাচাই করা হয়।

### খ. Transpilation (ট্রান্সপাইলেশন মেকানিজম)
> [!IMPORTANT]
> **ব্রাউজার বা Node.js সরাসরি TypeScript চালাতে পারে না!**  
> ব্রাউজার কেবল সাধারণ JavaScript বোঝে। তাই TypeScript কম্পাইলার (`tsc`) আমাদের লেখা `.ts` ফাইলগুলোকে টাইপ ভ্যালিডেশনের পর বিশুদ্ধ `.js` ফাইলে রূপান্তর (Transpile) করে দেয়।

---

## ⚙️ ৪. আর্কিটেকচার ও কীভাবে কাজ করে? (How It Works)

### কম্পাইলেশন পাইপলাইন (Compilation Pipeline)
```text
  +-----------------------+
  |   Developer writes    |
  |      app.ts code      |
  +-----------┬-----------+
              │
              ▼
  +-----------------------+
  |    TypeScript (tsc)   | ──(Type Error found?)──> ❌ Compilation Fails & Highlights Error in Editor
  |   Static Type Checker |
  +-----------┬-----------+
              │ (No Errors)
              ▼
  +-----------------------+
  |    Clean, Compatible  |
  |       app.js file     |
  +-----------┬-----------+
              │
              ▼
  +-----------------------+
  | Runs on Browser / Node| ──> 🚀 100% Type-Safe Execution!
  +-----------------------+
```

---

## 🛠️ ৫. ইনস্টলেশন ও প্রজেক্ট কনফিগারেশন গাইড

### ১. TypeScript ইনস্টলেশন
TypeScript গ্লোবালি অথবা প্রজেক্ট লেভেলে devDependency হিসেবে ইনস্টল করা যায়:

```bash
# গ্লোবাল ইনস্টলেশন (যেকোনো ফোল্ডার থেকে tsc কমান্ড চালাতে)
npm install -g typescript

# ইনস্টলেশন ও ভার্সন যাচাই
tsc -v
```

> [!TIP]
> **প্রজেক্টের ভেতর লোকাল ইনস্টলেশন (Recommended for Team Projects):**
> ```bash
> npm install -D typescript
> npx tsc -v
> ```

---

### ২. `tsconfig.json` কনফিগারেশন

একটি প্রফেশনাল প্রজেক্টের রুট ডিরেক্টরিতে কনফিগারেশন ফাইল তৈরি করার কমান্ড:
```bash
tsc --init
```

এটি স্বয়ংক্রিয়ভাবে একটি বিস্তারিত `tsconfig.json` তৈরি করবে। এর মধ্যে সবচেয়ে গুরুত্বপূর্ণ দুটি ডিরেক্টরি পাথ সেট করা হয়:

```json
{
  "compilerOptions": {
    "target": "ES2022",           // কম্পাইল হওয়া জাভাস্ক্রিপ্টের ভার্সন
    "module": "commonjs",         // মডিউল সিস্টেম
    "rootDir": "./src",           // আমাদের সমস্ত TypeScript সোর্স কোড থাকবে এখানে
    "outDir": "./dist",          // কম্পাইল হওয়া JavaScript ফাইল জমা হবে এখানে
    "strict": true,               // কঠোর টাইপ চেকিং সক্রিয় রাখবে
    "esModuleInterop": true
  },
  "include": ["src/**/*"]
}
```

### প্রজেক্ট ফোল্ডার স্ট্রাকচার:
```text
my-ts-project/
│
├── node_modules/
├── src/                      # Source Code (TypeScript)
│   └── index.ts
│
├── dist/                     # Compiled Production Code (JavaScript)
│   └── index.js
│
├── package.json
└── tsconfig.json             # TypeScript Compiler Rules
```

---

### ৩. অটোমেটিক কম্পাইলেশন (Watch Mode)
ফাইল সেভ করার সাথে সাথে যেন স্বয়ংক্রিয়ভাবে কম্পাইল হয়ে যায়:
```bash
tsc -w
# অথবা
tsc --watch
```

---

## 🧩 ৬. TypeScript Type System (টাইপ সিস্টেমের বিস্তারিত)

---

### ক. Implicit Type Inference (স্বয়ংক্রিয় টাইপ অনুমান)
ভ্যারিয়েবল ডিক্লেয়ার করার সময় টাইপ না বলে দিলেও, তার প্রাথমিক মান দেখে TypeScript নিজে থেকেই টাইপ নির্ধারণ করে নেয়। একে **Type Inference** বলে।

```typescript
let playerName = "Mashrafe"; // TypeScript স্বয়ংক্রিয়ভাবে ধরে নিয়েছে এটি string

// playerName = 34; 
// ❌ ERROR: Type 'number' is not assignable to type 'string'.
```

---

### খ. Explicit Types (স্পষ্টভাবে টাইপ নির্ধারণ)
ভ্যারিয়েবল তৈরির সময় কোলন (`:`) ব্যবহার করে নির্দিষ্ট টাইপ ডিক্লেয়ার করাকে **Explicit Type** বলে।

#### ১. প্রিমিটিভ ভ্যারিয়েবলস (Primitives):
```typescript
let title: string = "Captain";
let age: number = 36;
let isCaptain: boolean = true;
```

#### ২. অ্যারেই (Array):
```typescript
// শুধুমাত্র string ডেটা থাকবে এমন অ্যারে
let players: string[] = [];
players.push("Sakib");
// players.push(75); // ❌ ERROR: Argument of type 'number' is not assignable to parameter of type 'string'.

// শুধুমাত্র number ডেটা থাকবে এমন অ্যারে
let scores: number[] = [100, 85, 92];
```

#### ৩. অবজেক্ট (Object):
```typescript
let person: {
  name: string;
  age: number;
  isAdult: boolean;
};

person = {
  name: "Tamim",
  age: 34,
  isAdult: true
};

// person.country = "Bangladesh"; 
// ❌ ERROR: Property 'country' does not exist on type '{ name: string; age: number; isAdult: boolean; }'
```

---

### গ. Union Types (একাধিক টাইপ অনুমোদন)
বাস্তব অ্যাপ্লিকেশনে একটি ভ্যারিয়েবল বা অ্যারেতে একাধিক টাইপের ডেটা রাখার প্রয়োজন হতে পারে। এর জন্য পাইপ অপারেটর (`|`) ব্যবহার করে **Union Type** তৈরি করা হয়।

```typescript
// ১. Variable-এ Union Type (string অথবা number হতে পারে)
let id: string | number;
id = "USER-101"; // ✅ Valid
id = 101;        // ✅ Valid
// id = true;    // ❌ ERROR: Type 'boolean' is not assignable to type 'string | number'.

// ২. Array-তে Union Type
let mixed: (string | number)[] = [];
mixed.push("Tamim"); // ✅ Valid
mixed.push(28);      // ✅ Valid
// mixed.push(true); // ❌ ERROR: Type 'boolean' is not assignable.
```

---

### ঘ. Dynamic Type (`any`)

> [!CAUTION]
> **`any` ব্যবহারের ঝুঁকি ও সতর্কতা:**  
> কোনো ভ্যারিয়েবলের টাইপ `any` দিলে TypeScript সেই ভ্যারিয়েবলের ক্ষেত্রে সব ধরনের টাইপ চেকিং সম্পূর্ণ বন্ধ করে দেয়। ফলে এটি সাধারণ অনিরাপদ JavaScript-এর মতো আচরণ করে। খুব নিরুপায় না হলে প্রফেশনাল কোডে `any` ব্যবহার করা পরিহার করা উচিত!

```typescript
let data: any;

data = "Sumit"; // string
data = 45;      // number
data = true;    // boolean
// TypeScript কোনো এরর দেবে না, যা টাইপস্ক্রিপ্টের মূল উদ্দেশ্যকে নষ্ট করে।
```

---

## 💻 ৭. কোড উদাহরণ: সোর্স কোড বনাম কম্পাইলড কোড

### TypeScript সোর্স ফাইল (`src/index.ts`):
```typescript
function calculateTotal(price: number, taxRate: number): number {
  return price + (price * taxRate);
}

const itemPrice: number = 500;
const tax: number = 0.15;

const finalAmount = calculateTotal(itemPrice, tax);
console.log(`মোট প্রদেয় বিল: ৳${finalAmount}`);
```

### `tsc` দিয়ে কম্পাইল হওয়ার পর তৈরি হওয়া জাভাস্ক্রিপ্ট (`dist/index.js`):
```javascript
"use strict";
function calculateTotal(price, taxRate) {
    return price + (price * taxRate);
}
var itemPrice = 500;
var tax = 0.15;
var finalAmount = calculateTotal(itemPrice, tax);
console.log("\u09AE\u09CB\u099F \u09AA\u09CD\u09B0\u09A6\u09C7\u09AF\u09BC \u09AC\u09BF\u09B2: \u09F3" + finalAmount);
```

> **লক্ষণীয়:** কম্পাইল করা JavaScript ফাইলে কোনো টাইপ অ্যানোটেশন (`: number`) থাকে না। টাইপস্ক্রিপ্টের সমস্ত টাইপ শুধুমাত্র ডেভেলপমেন্টকালীন সুরক্ষার জন্য, রানটাইমে কোনো অতিরিক্ত ওভারহেড তৈরি করে না!

---

## ⚠️ ৮. সাধারণ ভুলসমূহ ও সমাধান (Common Mistakes)

> [!WARNING]
> **১. HTML ফাইলে সরাসরি `.ts` ফাইল লিংক করা:**  
> ❌ **ভুল:**
> ```html
> <script src="src/index.ts"></script> <!-- ব্রাউজার রান করতে পারবে না -->
> ```
> ✅ **সঠিক:** সবসময় কম্পাইল হওয়া `.js` ফাইলটি HTML ফাইলে লিংক করবেন:
> ```html
> <script src="dist/index.js"></script>
> ```

> [!WARNING]
> **২. ফাংশন প্যারামিটারে টাইপ না দেওয়া (Implicit Any):**  
> ❌ **ভুল:**
> ```typescript
> function multiply(a, b) { 
>   return a * b; // এখানে a ও b-এর টাইপ 'any' হয়ে যায়
> }
> ```
> ✅ **সঠিক:**
> ```typescript
> function multiply(a: number, b: number): number {
>   return a * b;
> }
> ```

> [!TIP]
> **৩. অবজেক্টে অনির্ধারিত প্রপার্টি ব্যবহারের চেষ্টা:**  
> অবজেক্ট তৈরি করার পর তাতে নতুন কোনো অচেনা প্রপার্টি যোগ করা যাবে না। যদি কোনো প্রপার্টি অপশনাল বা ঐচ্ছিক রাখতে চান, তবে প্রশ্নবোধক চিহ্ন (`?`) ব্যবহার করুন:
> ```typescript
> let user: { name: string; age?: number }; // age প্রপার্টিটি অপশনাল
> user = { name: "Naim" }; // ✅ কোনো এরর আসবে না
> ```

---

## 📊 ৯. কুইক রিভিশন তুলনা চার্ট (Quick Revision)

| বৈশিষ্ট্য | JavaScript | TypeScript |
|---|---|---|
| **Type Checking** | Dynamically Typed (রানটাইমে ভুল ধরা পড়ে) | Statically Typed (কোড লেখার সময়ই কম্পাইলারে ধরা পড়ে) |
| **ব্রাউজার সাপোর্ট** | ব্রাউজার ও Node.js সরাসরি রান করতে পারে | সরাসরি চলে না; `tsc` দিয়ে JS-এ কনভার্ট করতে হয় |
| **বাগ প্রতিরোধ** | লাইভ অ্যাপ্লিকেশনে ক্র্যাশ করার ঝুঁকি বেশি | ডেভেলপমেন্টের সময়ই ৯০% টাইপজনিত বাগ দূর করে |
| **IntelliSense** | সীমিত অটো-কমপ্লিট সুবিধা | কোড এডিটরে অসাধারণ অটোকমপ্লিট ও ডকুমেন্টেশন পাওয়া যায় |

---

## 📝 ১০. সারসংক্ষেপ ও মূল শিক্ষা (Key Takeaways)

- **সুপারসেট:** TypeScript হলো JavaScript + Static Types।
- **Transpilation:** `tsc` কমান্ড দিয়ে `.ts` ফাইলকে বিশুদ্ধ `.js` ফাইলে রূপান্তর করে চালাতে হয়।
- **প্রজেক্ট স্ট্রাকচার:** `tsconfig.json`-এ `rootDir` (সোর্স) এবং `outDir` (আউটপুট) কনফিগার করে পরিচ্ছন্ন প্রজেক্ট গঠন নিশ্চিত করা হয়।
- **Type Inference:** মান অ্যাসাইন করলে টাইপস্ক্রিপ্ট নিজে থেকেই টাইপ চিনে নেয়।
- **Union Types:** পাইপ অপারেটর (`|`) দিয়ে একাধিক সম্ভাব্য টাইপ অনুমোদন করা যায়।
- **`any` এড়িয়ে চলা:** `any` টাইপস্ক্রিপ্টের নিরাপত্তা নষ্ট করে; তাই এর ব্যবহার সীমিত রাখুন।

---

## ❓ ১১. স্ব-মূল্যায়ন ও প্র্যাকটিস প্রশ্নাবলী (Self-Assessment)

১. TypeScript-কে কেন JavaScript-এর সুপারসেট বলা হয়?  
২. `rootDir` এবং `outDir`-এর কাজ কী এবং কেন এগুলো কনফিগার করা জরুরি?  
৩. Type Inference এবং Explicit Type Annotation-এর মধ্যে পার্থক্য কী?  
৪. `let id: string | number;` এই সিনট্যাক্সটির নাম কী এবং এটি কখন প্রয়োজন হয়?  
৫. HTML ফাইলে `<script src="index.ts"></script>` লিখলে কেন ব্রাউজার এরর দেয় এবং এর সঠিক উপায় কী?

---

## 🔗 ১২. তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)

- [TypeScript Official Documentation](https://www.typescriptlang.org/)
- [TypeScript Handbook — The Basics](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)
- [TSConfig Reference (rootDir, outDir)](https://www.typescriptlang.org/tsconfig)
- [MDN Web Docs - JavaScript Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to TypeScript Index](../README.md)
