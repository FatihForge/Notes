# ⚛️ 01. React ফান্ডামেন্টালস: JSX, Props, Events ও useState Hook (React Fundamentals)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 React ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. শিক্ষার উদ্দেশ্য (Learning Objectives)

এই পাঠটি সম্পন্ন করার পর আপনি:
- **React-এর মৌলিক দর্শন** এবং এটি কেন আধুনিক ওয়েব ফ্রন্টএন্ডের জন্য সবচেয়ে জনপ্রিয় লাইব্রেরি তা ব্যাখ্যা করতে পারবেন।
- **JSX (JavaScript XML)** কী এবং এটি সাধারণ HTML ও প্লেইন JavaScript-এর থেকে কীভাবে আলাদা তা অনুধাবন করতে পারবেন।
- কম্পোনেন্টের মধ্যে একমুখী ডেটা প্রবাহের (Unidirectional Data Flow) মাধ্যম হিসেবে **Props** ব্যবহার করতে পারবেন।
- React-এ **Event Handling** (যেমন `onClick`, `onChange`) কার্যকরভাবে পরিচালনা করতে পারবেন।
- React-এর সবচেয়ে গুরুত্বপূর্ণ হুক **`useState()`** ব্যবহার করে পরিবর্তনশীল ডেটা ম্যানেজ করতে এবং কম্পোনেন্ট **Re-render** মেকানিজম বুঝতে পারবেন।

---

## 📖 ২. ভূমিকা (Introduction)

> [!NOTE]
> **React কী?**
> **React** হলো Meta (Facebook) দ্বারা তৈরি একটি ওপেন-সোর্স জাভাস্ক্রিপ্ট লাইব্রেরি, যা মূলত ডিক্লেয়ারেটিভ (Declarative) এবং কম্পোনেন্ট-বেসড (Component-Based) ইউজার ইন্টারফেস (UI) তৈরির জন্য ব্যবহৃত হয়।

সাধারণ জাভাস্ক্রিপ্টে (Vanilla JS) যখন কোনো ওয়েবসাইটের ডেটা পরিবর্তন হতো, তখন সম্পূর্ণ ওয়েবপেজ রিলোড করতে হতো অথবা জটিল DOM ম্যানিপুলেশন (`document.getElementById`, `innerHTML`) করতে হতো। React এই সমস্যার সমাধান এনেছে **Virtual DOM** এবং **State-Driven Rendering**-এর মাধ্যমে।

React অ্যাপ্লিকেশনের পুরো পরিকাঠামো দাঁড়িয়ে আছে ৪টি মূল স্তম্ভের ওপর:
1. **JSX** (UI লেখার ভাষা)
2. **Props** (ডেটা স্থানান্তরের মাধ্যম)
3. **Events** (ব্যবহারকারীর ইন্টারঅ্যাকশন হ্যান্ডলার)
4. **useState** (পরিবর্তনশীল ডেটা ও রি-রেন্ডারিং নিয়ন্ত্রণ)

---

## 💡 ৩. মূল ধারণাসমূহ ও বিস্তারিত ব্যাখ্যা (Core Concepts & Detailed Explanation)

---

### 🎨 ১. JSX (JavaScript XML)

**JSX-এর পূর্ণরূপ:** JavaScript XML।

JSX হলো JavaScript-এর একটি Syntax Extension, যা React-এ HTML-এর মতো Syntax ব্যবহার করে UI (User Interface) তৈরি করতে সাহায্য করে।

#### সিনট্যাক্স ও উদাহরণ:
```jsx
const element = <h1>Hello World!</h1>;
```

> [!TIP]
> **কীভাবে কাজ করে?**  
> এখানে `<h1>` দেখতে হুবহু HTML-এর মতো মনে হলেও ব্রাউজার সরাসরি এই কোড পড়তে পারে না। Babel কম্পাইলার এই JSX-কে ব্যাকগ্রাউন্ডে খাঁটি জাভাস্ক্রিপ্ট অবজেক্টে রূপান্তর করে:
> ```javascript
> const element = React.createElement('h1', null, 'Hello World!');
> ```

---

### 📦 ২. Props (Properties)

React-এ এক Component থেকে অন্য Component-এ Data Pass করার জন্য Props ব্যবহার করা হয়।

#### উদাহরণ:
```jsx
// ডেটা গ্রহণকারী চাইল্ড কম্পোনেন্ট
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// ডেটা প্রেরণকারী প্যারেন্ট কম্পোনেন্ট
function App() {
  return <Welcome name="Ashik" />;
}

export default App;
```

#### উপাদান বিশ্লেষণ:
- `name="Ashik"` → প্যারেন্ট থেকে চাইল্ডে পাঠানো Data (Property)।
- `props` → চাইল্ড কম্পোনেন্টে আসা সেই Data গ্রহণের একটি JavaScript Object।
- `{props.name}` → JSX-এর মধ্যে কার্লি ব্র্যাকেট `{}` ব্যবহার করে JavaScript এক্সপ্রেশন হিসেবে ডেটা Access করা হয়েছে।

> [!IMPORTANT]
> **Props হলো Read-Only (ইমিউটেবল):**  
> কোনো চাইল্ড কম্পোনেন্ট কখনোই নিজের ভেতর থেকে `props.name = "Rahim"` লিখে প্রপস পরিবর্তন করতে পারে না। ডেটা সবসময় প্যারেন্ট থেকে চাইল্ডের দিকে একমুখী (Unidirectional) প্রবাহিত হয়।

---

### ⚡ ৩. Events (Event Handling)

React-এ User-এর বিভিন্ন Interaction—যেমন Button Click, Input Change, Form Submit ইত্যাদি Handle করার জন্য Event Handling ব্যবহার করা হয়।

#### উদাহরণ:
```jsx
function App() {
  function handleClick() {
    alert("Button Clicked!");
  }

  return (
    <button onClick={handleClick}>
      Click Me
    </button>
  );
}

export default App;
```

#### উপাদান বিশ্লেষণ:
- `onClick` → React-এর সিনথেটিক ইভেন্ট হ্যান্ডলার (HTML-এর `onclick`-এর বদলে React-এ camelCase ব্যবহৃত হয়)।
- `handleClick` → Event ঘটলে যে ফাংশনটি Execute হবে তার রেফারেন্স।
- Button-এ ক্লিক করার সাথে সাথে `handleClick` ফাংশনটি রান হবে।

> [!WARNING]
> **কমন সিনট্যাক্স ভুল:**  
> কোডে `onClick={handleClick}` লিখতে হবে (ফাংশনের রেফারেন্স দিতে হবে)। কখনোই `onClick={handleClick()}` লিখবেন না; ব্র্যাকেট দিলে পেজ লোড হওয়ার সাথে সাথেই ফাংশনটি কল হয়ে যাবে, ক্লিকের জন্য অপেক্ষা করবে না!

---

### 🔄 ৪. React Hooks – `useState()`

`useState` হলো React-এর একটি কোর Hook, যা Functional Component-এর মধ্যে State (পরিবর্তনশীল অভ্যন্তরীণ ডেটা) সংরক্ষণ ও পরিবর্তন করতে ব্যবহার করা হয়।

#### Syntax (সিনট্যাক্স বিশ্লেষণ):
```jsx
const [count, setCount] = useState(0);
```

| উপাদান | ভূমিকা ও বিবরণ |
|---|---|
| `useState` | React লাইব্রেরি থেকে ইমপোর্ট করা বিল্ট-ইন হুক |
| `count` | বর্তমান State Value (বর্তমান মান ধারণ করে) |
| `setCount` | State পরিবর্তনের জন্য নির্ধারিত স্পেশাল Function |
| `0` | Initial Value বা প্রাথমিক প্রারম্ভিক মান |

---

## ⚙️ ৪. কীভাবে কাজ করে? (How It Works: Counter Example)

ধরা যাক, একটি রিয়েল-লাইফ **Counter** তৈরি করা হবে। Button Click করলে প্রতিবার সংখ্যা বাড়বে।

### পূর্ণাঙ্গ কোড:
```jsx
import { useState } from "react";

function Counter() {
  // স্টেট ডিক্লেয়ারেশন
  const [count, setCount] = useState(0);

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>বর্তমান গণনা: {count}</h1>

      <button 
        style={{ padding: "10px 20px", fontSize: "16px", cursor: "pointer" }}
        onClick={() => setCount(count + 1)}
      >
        Increase (+1)
      </button>
    </div>
  );
}

export default Counter;
```

---

### 🔄 রি-রেন্ডারিং কার্যপ্রণালী (Step-by-Step Working Process)

নিচের আর্কিটেকচার ফ্লোটি লক্ষ্য করুন:

```text
  [1. Initial Render]
         │
         ▼
  count = 0 ──> UI Display: "বর্তমান গণনা: 0"
         │
  [2. User Interaction]
         │
         ▼
  User Clicks Button ──> onClick Triggered
         │
  [3. State Updater Execution]
         │
         ▼
  setCount(0 + 1) ──> React Schedules State Update (New count = 1)
         │
  [4. Component Re-execution]
         │
         ▼
  Counter() Function Runs Again (Re-render)
         │
  [5. Virtual DOM Diffing]
         │
         ▼
  React detects ONLY the <h1> text changed: 0 ──> 1
         │
  [6. Targeted Real DOM Update]
         │
         ▼
  UI Updates smoothly WITHOUT Full Page Reload! 🚀
```

#### ধাপসমূহ:
1. **Initial Mount:** প্রথমবার Component Render হলে `count`-এর Initial Value থাকে `0`।
2. **User Click:** ইউজার "Increase" বাটনে ক্লিক করে।
3. **State Setter:** `setCount(count + 1)` ফাংশনটি Execute হয়।
4. **State Update:** React ইন্টারনালি মেমোরিতে স্টেট আপডেট করে।
5. **Re-render:** React কম্পোনেন্ট ফাংশনটিকে আবার নতুন মান নিয়ে এক্সিকিউট করে (Re-render)।
6. **Targeted UI Update:** নতুন স্টেট অনুযায়ী ব্রাউজারের UI-তে শুধুমাত্র পরিবর্তিত সংখ্যাটি রিফ্রেশ হয়।

> [!NOTE]
> **🌟 সবচেয়ে গুরুত্বপূর্ণ মূলনীতি:**  
> `useState` নিজে সরাসরি ব্রাউজার উইন্ডো রিফ্রেশ করে না। State পরিবর্তন হলে React কম্পোনেন্ট ফাংশনটিকে রি-রান করে এবং **Virtual DOM**-এর সাহায্যে তুলনা করে শুধুমাত্র যে লেখাটুকু বদলানো দরকার তা আপডেট করে। এর ফলে পুরো ওয়েবসাইট রিলোড না হয়েও তাৎক্ষণিক ও মসৃণ ইউজার এক্সপেরিয়েন্স পাওয়া যায়!

---

## 🖥️ ৫. প্রত্যাশিত আউটপুট (Expected Output)

### স্ক্রিন লোডের সময়:
```text
বর্তমান গণনা: 0
[ Increase (+1) ]
```

### বাটনটিতে ৩ বার ক্লিক করার পর:
```text
বর্তমান গণনা: 3
[ Increase (+1) ]
```

*(কোনো পেজ রিফ্রেশ স্পিনার ঘুরবে না, তাৎক্ষণিক স্ক্রিনে সংখ্যা ৩ হয়ে যাবে)*

---

## ⚠️ ৬. সাধারণ ভুলসমূহ ও সমাধান (Common Mistakes)

> [!CAUTION]
> **১. স্টেট সরাসরি পরিবর্তন করার চেষ্টা (Direct State Mutation):**
> ```jsx
> // ❌ মারাত্মক ভুল:
> count = count + 1;
>
> // ✅ সঠিক নিয়ম:
> setCount(count + 1);
> ```
> স্টেট সরাসরি পরিবর্তন করলে React কখনো জানতেই পারে না যে ডেটা বদলেছে, ফলে কম্পোনেন্ট Re-render হয় না এবং UI আপডেট হয় না।

> [!WARNING]
> **২. ইভেন্ট হ্যান্ডলারে তাৎক্ষণিক ফাংশন কল করা:**
> ```jsx
> // ❌ ভুল: রেন্ডারের সময়ই এক্সিকিউট হয়ে যাবে এবং Infinite Loop তৈরি হতে পারে
> <button onClick={setCount(count + 1)}>Increase</button>
>
> // ✅ সঠিক: অ্যানোনিমাস অ্যারো ফাংশন ব্যবহার করুন
> <button onClick={() => setCount(count + 1)}>Increase</button>
> ```

> [!TIP]
> **৩. পূর্ববর্তী স্টেটের ওপর নির্ভরশীল আপডেট (Functional Updates):**
> যখন নতুন স্টেট আগের স্টেটের ওপর সম্পূর্ণ নির্ভরশীল হয়, তখন বেস্ট প্র্যাকটিস হলো কলব্যাক সিনট্যাক্স ব্যবহার করা:
> ```jsx
> setCount((prevCount) => prevCount + 1);
> ```

---

## 📊 ৭. কুইক রিভিশন টেবিল (Quick Revision)

| বিষয় (Topic) | মূল ভূমিকা (Core Responsibility) | বাস্তব কোড উদাহরণ |
|---|---|---|
| 🎨 **JSX** | HTML-এর মতো ডিক্লেয়ারেটিভ সিনট্যাক্স দিয়ে UI ডিজাইন করা | `const el = <h1>Hello</h1>;` |
| 📦 **Props** | প্যারেন্ট কম্পোনেন্ট থেকে চাইল্ডে একমুখী ডেটা প্রেরণ করা | `<Welcome name="Ashik" />` |
| ⚡ **Events** | ব্যবহারকারীর ক্লিক, টাইপ ইত্যাদি ইন্টারঅ্যাকশন গ্রহণ করা | `<button onClick={handleClick}>` |
| 🔄 **useState** | কম্পোনেন্টের পরিবর্তনশীল ডেটা ধরে রাখা ও Re-render ঘটানো | `const [val, setVal] = useState(0);` |

---

## 📌 ৮. মূল সারসংক্ষেপ (Core Takeaways)

> **💡 মূল দর্শন:**  
> React অ্যাপ্লিকেশনে **JSX** দিয়ে UI তৈরি করা হয়, **Props** দিয়ে কম্পোনেন্টগুলোর মাঝে ডেটা আদান-প্রদান করা হয়, **Events** দিয়ে ব্যবহারকারীর প্রতিটি ইন্টারঅ্যাকশন হ্যান্ডেল করা হয়, এবং **useState** দিয়ে পরিবর্তনশীল ডেটা নিয়ন্ত্রণ করে ব্রাউজার রিলোড ছাড়াই চমৎকার রিয়েল-টাইম ইন্টারফেস তৈরি করা হয়।

---

## ❓ ৯. স্ব-মূল্যায়ন ও ইন্টারভিউ প্রশ্নাবলী (Self-Assessment)

১. JSX কী? ব্রাউজার কি সরাসরি JSX কোড বুঝতে পারে? যদি না পারে, তবে কীভাবে এটি কার্যকর হয়?  
২. Props এবং State-এর মধ্যকার মৌলিক ৩টি পার্থক্য কী কী?  
৩. `onClick={handleClick}` এবং `onClick={handleClick()}`-এর মধ্যে পার্থক্য কী এবং কোন ক্ষেত্রে কী ঘটে?  
৪. `useState`-এ `setCount` কল করলে React ব্যাকগ্রাউন্ডে কী কী পদক্ষেপ গ্রহণ করে?  
৫. State পরিবর্তন হলে কেন পুরো ওয়েবপেজ রিলোড হয় না?

---

## 🔗 ১০. তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)

- [React Official Documentation - Describing the UI](https://react.dev/learn/describing-the-ui)
- [React Official Documentation - Passing Props to a Component](https://react.dev/learn/passing-props-to-a-component)
- [React Official Documentation - Responding to Events](https://react.dev/learn/responding-to-events)
- [React Official Documentation - useState Reference](https://react.dev/reference/react/useState)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to React Index](../README.md)
