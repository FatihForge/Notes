# ⚛️ 02. React কম্পোনেন্ট আর্কিটেকচার ও প্রপস (Components & Props)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 React ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- **React Element** এবং **React Component**-এর মধ্যে মৌলিক ও তাত্ত্বিক পার্থক্য স্পষ্ট করা।
- **Functional Component** এবং **Class Component**-এর ইন্টারনাল এক্সিকিউশন পার্থক্য জানা।
- **Props (Properties)** কীভাবে কাজ করে এবং রিয়্যাক্টের সুকঠোর নীতি **"Props are Read-Only"** অনুধাবন করা।
- **Pure Function বনাম Impure Function**-এর ধারণা এবং প্রপস মিউটেশন কেন নিষিদ্ধ তা জানা।
- **Component Composition** এবং ES6 Module সিস্টেমে কম্পোনেন্ট ইনক্যাপসুলেশন প্র্যাকটিস করা।

---

## 📖 ২. React Element বনাম React Component

> [!NOTE]
> **আমার সহজ উপলব্ধি:**  
> - **React Element:** এটি একটি প্লেইন জাভাস্ক্রিপ্ট অবজেক্ট, যা UI-এর কোনো একটি নির্দিষ্ট মুহূর্তের স্থির স্ন্যাপশট প্রকাশ করে।  
> - **React Component:** এটি একটি ব্লুপ্রিন্ট বা ফাংশন/ক্লাস, যা ইনপুট হিসেবে প্রপস (Props) গ্রহণ করে এবং আউটপুট হিসেবে রিয়্যাক্ট এলিমেন্ট (JSX) রিটার্ন করে।

```text
[ Data / Props ] ───▶ [ React Component (Blueprint) ] ───▶ [ React Element (JSX Tree) ]
```

---

## 💡 ৩. Functional Component বনাম Class Component

রিয়্যাক্টে কম্পোনেন্ট দুইভাবে লেখা যায়:

### ১. Functional Component:
Functional Component হলো রিয়্যাক্টের সবচেয়ে সরল ও আধুনিক রূপ। এটি মূলত একটি সাধারণ জাভাস্ক্রিপ্ট ফাংশন:

```jsx
function Greeting(props) {
  return <h1>হ্যালো, {props.name}!</h1>;
}
```

**অভ্যন্তরীণ এক্সিকিউশন (Internal Execution):**  
রিয়্যাক্ট যখন কোনো Functional Component (যেমন `<Greeting name="Ashik" />`) রেন্ডার করে, তখন কোনো অবজেক্ট ইনস্ট্যান্স তৈরি হয় না; রিয়্যাক্ট কেবল ফাংশনটিকে সরাসরি কল করে: `Greeting(props)`।

### ২. Class Component:
ES6-এর Class আর্কিটেকচার চালুর পর রিয়্যাক্ট অবজেক্ট-ওরিয়েন্টেড ধাঁচে Class Component চালুর সুযোগ দেয়:

```jsx
import React from 'react';

class Greeting extends React.Component {
  render() {
    return <h1>হ্যালো, {this.props.name}!</h1>;
  }
}
```

### 🧠 এক্সিকিউশন মেকানিজমের পার্থক্য:

```text
  ┌─────────────────────────────────────────────────────────────┐
  │ 1. Functional Component Execution:                          │
  │    রিয়্যাক্ট সরাসরি ফাংশন কল করে: Greeting(props)            │
  ├─────────────────────────────────────────────────────────────┤
  │ 2. Class Component Instantiation:                           │
  │    রিয়্যাক্ট new কিওয়ার্ড দিয়ে ইনস্ট্যান্স তৈরি করে:          │
  │    const instance = new Greeting(props);                    │
  │    এরপর render মেথড কল করে: instance.render();              │
  └─────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> Class Component-এর অপরিহার্য শর্ত হলো এতে অবশ্যই একটি `render()` মেথড থাকতে হবে যা JSX (বা রিয়্যাক্ট এলিমেন্ট) রিটার্ন করবে।

---

## 📦 ৪. Props (Properties)-এর কার্যপ্রণালী ও Read-Only নীতি

**Props** (Properties-এর সংক্ষিপ্ত রূপ) হলো প্যারেন্ট কম্পোনেন্ট থেকে চাইল্ড কম্পোনেন্টে ডেটা পাঠানোর জন্য একটি একমুখী ট্রান্সপোর্টার (One-way transporter)।

### Props কীভাবে তৈরি হয়?
যখন কোনো কম্পোনেন্ট ট্যাগে অ্যাট্রিবিউট সংজ্ঞায়িত করা হয়:

```jsx
<Clock locale="bn-BD" title="ঢাকা সময়" />
```

রিয়্যাক্ট সমস্ত অ্যাট্রিবিউট একত্র করে একটি একক হিমায়িত (Frozen) অবজেক্ট তৈরি করে:
`{ locale: "bn-BD", title: "ঢাকা সময়" }`
- **Functional Component-এ:** ফাংশনের প্রথম আর্গুমেন্ট `props` হিসেবে পাওয়া যায়।
- **Class Component-এ:** ক্লাসের ইনস্ট্যান্স প্রপার্টি `this.props` হিসেবে পাওয়া যায়।

### ⚠️ সুকঠোর অলঙ্ঘনীয় নিয়ম: "Props are Read-Only"

> [!CAUTION]
> **React Core Rule:**  
> *"All React components must act like pure functions with respect to their props."*  
> অর্থাৎ, প্রতিটি রিয়্যাক্ট কম্পোনেন্টকে তার প্রপসের ক্ষেত্রে অবশ্যই Pure Function-এর মতো আচরণ করতে হবে।

### Pure Function বনাম Impure Function:

```javascript
// Pure Function (ইনপুট a ও b অবিকৃত থাকে, কোনো পার্শ্বপ্রতিক্রিয়া নেই)
function sum(a, b) {
  return a + b;
}

// Impure Function (ইনপুট অবজেক্টকে সরাসরি পরিবর্তন/মিউটেট করে!)
function withdraw(account, amount) {
  account.total = account.total - amount; // ❌ ইনপুট মিউটেট হয়েছে!
}
```

রিয়্যাক্ট চাইল্ড কম্পোনেন্টকে কখনো তার নিজের প্রপস মিউটেট করতে দেয় না:

```jsx
function Clock(props) {
  // ❌ অত্যন্ত মারাত্মক ভুল ও সম্পূর্ণ নিষিদ্ধ!
  props.locale = 'en-US'; 
  
  return <h1>{props.locale}</h1>;
}
```

প্রপস হলো ওপর থেকে নিচে প্রবাহিত Read-Only ডেটা। চাইল্ড কম্পোনেন্ট কেবল এই ডেটা পড়তে ও রেন্ডার করতে পারে, কখনো সংশোধন করতে পারে না।

---

## 🧩 ৫. Component Composition ও Encapsulation

### Component Composition (কম্পোনেন্ট সংযোজন):
একটি পূর্ণাঙ্গ ওয়েব পেজের UI তৈরি করতে ছোট ছোট কাস্টম কম্পোনেন্টকে সমন্বিত করার পদ্ধতিই হলো Component Composition:

```jsx
function App() {
  return (
    <div className="container">
      <Greeting name="রহিম" />
      <Greeting name="করিম" />
      <Clock locale="bn-BD" />
    </div>
  );
}
```

### Encapsulation ও ES6 Modules:
প্রতিটি কম্পোনেন্টকে আলাদা ফাইলে ইনক্যাপসুলেট (Encapsulate) করে `export` এবং `import` করা হয়:

```jsx
// components/Clock.jsx
import React from 'react';

export default class Clock extends React.Component {
  render() {
    return <h1>{this.props.date.toLocaleTimeString()}</h1>;
  }
}

// App.jsx
import Clock from './components/Clock'; // মডিউল ইমপোর্ট
```

---

## 📝 ৬. কুইক রিভিশন চিটশিট (Summary Cheatsheet)

| বিষয় | Functional Component | Class Component |
|---|---|---|
| **সিনট্যাক্স** | প্লেইন জাভাস্ক্রিপ্ট ফাংশন | `React.Component` এক্সটেন্ড করা ES6 ক্লাস |
| **ইনস্ট্যান্স** | কোনো অবজেক্ট ইনস্ট্যান্স তৈরি হয় না | `new` দিয়ে ইনস্ট্যান্স অবজেক্ট তৈরি হয় |
| **Props অ্যাক্সেস** | প্রথম প্যারামিটার `props` | `this.props` ইনস্ট্যান্স প্রপার্টি |
| **Render মেথড** | সরাসরি JSX রিটার্ন করে | আবশ্যক `render()` মেথডের মাধ্যমে রিটার্ন করে |
| **Props নীতি** | সম্পূর্ণ **Read-Only / Immutable** | সম্পূর্ণ **Read-Only / Immutable** |

---

## ⏭️ পরবর্তী লেসনের যোগসূত্র
যেহেতু প্রপস সম্পূর্ণ Read-Only এবং চাইল্ড কখনো তা পরিবর্তন করতে পারে না, তাই কোনো বাটনে ক্লিক করলে বা সময় পার হলে ডেটা আপডেট করার জন্য আমাদের প্রয়োজন **State** এবং **Lifecycle**—যা আমরা পরবর্তী [০৩. স্টেট ম্যানেজমেন্ট ও লাইফসাইকেল মেথডস](../03-state-and-lifecycle/README.md) লেসনে শিখব।
