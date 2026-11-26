# ⚛️ 04. React ইভেন্ট হ্যান্ডলিং ও ইন্টারঅ্যাকশন (Event Handling)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 React ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- React-এর **SyntheticEvent** আর্কিটেকচার এবং নেটিভ ব্রাউজার ইভেন্টের সাথে এর পার্থক্য বোঝা।
- JSX-এ ইভেন্ট হ্যান্ডলার লেখার নিয়মাবলী (`camelCase`, ফাংশন রেফারেন্স পাসিং) আয়ত্ত করা।
- `e.preventDefault()` ব্যবহার করে ব্রাউজারের ডিফল্ট আচরণ বন্ধ করার সঠিক উপায় জানা।
- ইভেন্ট হ্যান্ডলারে কাস্টম আর্গুমেন্ট পাস করার টেকনিক শেখা।
- Class Component-এ `this` বাইন্ডিং সমস্যা এবং অ্যারো ফাংশন দিয়ে সমাধান।
- ব্যবহারকারীর ইন্টারঅ্যাকশনের মাধ্যমে স্টেট পরিবর্তন করে লাইভ UI আপডেট করা।

---

## 📖 ২. React Synthetic Event System

> [!NOTE]
> **আমার সহজ উপলব্ধি:**  
> বিভিন্ন ব্রাউজারে (Chrome, Firefox, Safari) নেটিভ DOM ইভেন্টের বিহেভিয়ার বা প্রপার্টিতে কিছু সূক্ষ্ম পার্থক্য থাকে। রিয়্যাক্ট এই অসঙ্গতি দূর করতে ব্রাউজারের নেটিভ ইভেন্টকে একটি ক্রস-ব্রাউজার র‍্যাপার (Wrapper) দিয়ে মুড়ে দেয়, যার নাম **SyntheticEvent**। এর ফলে সমস্ত ব্রাউজারে ইভেন্ট অবজেক্ট একই নিয়মে কাজ করে।

```text
[ Browser Native Event ] ───▶ [ React SyntheticEvent Wrapper ] ───▶ [ Your Event Handler ]
```

---

## 💡 ৩. JSX-এ ইভেন্ট হ্যান্ডলার লেখার সুনির্দিষ্ট নিয়ম

সাধারণ HTML এবং React JSX-এ ইভেন্ট হ্যান্ডলিংয়ের দুটি প্রধান পার্থক্য রয়েছে:

```text
  ┌─────────────────────────────────────────────────────────────┐
  │ ১. নেমিং কনভেনশন:                                          │
  │    HTML:  <button onclick="handleClick()">Click</button>    │
  │    React: <button onClick={handleClick}>Click</button>      │
  │    (HTML-এ lowercase, কিন্তু React JSX-এ সর্বদা camelCase)  │
  ├─────────────────────────────────────────────────────────────┤
  │ ২. ফাংশন পাসিং:                                             │
  │    HTML-এ স্ট্রিং হিসেবে পাস হয়: "handleClick()"            │
  │    React-এ সরাসরি ফাংশনের রেফারেন্স পাস হয়: {handleClick}   │
  └─────────────────────────────────────────────────────────────┘
```

> [!CAUTION]
> **কমন মারাত্মক ভুল:**  
> JSX-এ কখনোই ইভেন্ট হ্যান্ডলারে সরাসরি প্যারেন্থেসিস `()` কল করবেন না:
> ```jsx
> // ❌ মারাত্মক ভুল! পেজ লোড হওয়ামাত্রই ফাংশন রান হয়ে যাবে!
> <button onClick={handleClick()}>ক্লিক করুন</button>
> 
> //  সঠিক নিয়ম: শুধুমাত্র ফাংশনের রেফারেন্স দিতে হবে
> <button onClick={handleClick}>ক্লিক করুন</button>
> ```

---

## 🛑 ৪. ডিফল্ট অ্যাকশন বন্ধ করা (`e.preventDefault()`)

সাধারণ HTML-এ কোনো লিংকের ডিফল্ট নেভিগেশন বন্ধ করতে `return false;` লিখলে চলত। কিন্তু React-এ এটি কাজ করে না; স্পষ্টভাবে `e.preventDefault()` কল করতে হয়:

```jsx
function Form() {
  function handleSubmit(e) {
    e.preventDefault(); // ব্রাউজার যেন পেজ রিলোড না করে!
    console.log('ফর্ম সফলভাবে সাবমিট হয়েছে!');
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" placeholder="নাম লিখুন" />
      <button type="submit">জমা দিন</button>
    </form>
  );
}
```

---

## 🎯 ৫. ইভেন্ট হ্যান্ডলারে প্যারামিটার বা আর্গুমেন্ট পাস করা

কখনো কখনো আমাদের বাটনে ক্লিকের সাথে সাথে কোনো নির্দিষ্ট আইডি বা ডেটা পাস করার প্রয়োজন হয়:

```jsx
function ItemList() {
  function deleteItem(id, e) {
    console.log(`আইটেম ${id} মুছে ফেলা হচ্ছে`);
  }

  return (
    <div>
      {/* Arrow Function দিয়ে প্যারামিটার পাস */}
      <button onClick={(e) => deleteItem(101, e)}>
        মুছে ফেলুন
      </button>
    </div>
  );
}
```

---

## 🔄 ৬. ইভেন্টের মাধ্যমে State আপডেট (কাউন্টার ও টগল উদাহরণ)

বাস্তব জীবনে ব্যবহারকারী যখন বাটনে ক্লিক করে, তখন আমরা স্টেট আপডেট করি এবং UI স্বয়ংক্রিয়ভাবে পরিবর্তিত হয়:

```jsx
import React from 'react';

class InteractiveCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  // Arrow function ব্যবহারের কারণে 'this' স্বয়ংক্রিয়ভাবে বাইন্ড থাকে
  handleIncrement = () => {
    this.setState((prevState) => ({
      count: prevState.count + 1
    }));
  };

  handleDecrement = () => {
    this.setState((prevState) => ({
      count: prevState.count - 1
    }));
  };

  render() {
    return (
      <div style={{ textAlign: 'center', marginTop: '20px' }}>
        <h2>বর্তমান কাউন্ট: {this.state.count}</h2>
        <button onClick={this.handleIncrement}>➕ বৃদ্ধি করুন</button>
        <button onClick={this.handleDecrement} style={{ marginLeft: '10px' }}>➖ হ্রাস করুন</button>
      </div>
    );
  }
}

export default InteractiveCounter;
```

---

## 📝 ৭. কুইক রিভিশন চিটশিট (Summary Cheatsheet)

| বিষয় | বিবরণ |
|---|---|
| **SyntheticEvent** | ব্রাউজার-নিরপেক্ষ ক্রস-প্ল্যাটফর্ম ইভেন্ট অবজেক্ট র‍্যাপার। |
| **camelCase** | `onclick` এর বদলে `onClick`, `onchange` এর বদলে `onChange`। |
| **Function Passing** | ফাংশন রেফারেন্স `{handleClick}` পাস করতে হয়, এক্সিকিউশন `{handleClick()}` নয়। |
| **`e.preventDefault()`** | ব্রাউজারের স্বাভাবিক আচরণ (যেমন পেজ রিলোড) প্রতিরোধ করার নিরাপদ মাধ্যম। |
| **Argument Passing** | অ্যারো ফাংশন `(e) => handleClick(id, e)` দিয়ে নিরাপদে আর্গুমেন্ট পাস করা যায়। |

---

## ⏭️ পরবর্তী রোডম্যাপ ও হুকস (Future Hooks Roadmap)
আমরা React-এর কোর ফাউন্ডেশন (JSX, Element Rendering, Components, Props, State, Lifecycle এবং Events) পুঙ্খানুপুঙ্খভাবে শিখেছি। পরবর্তীতে আমরা আধুনিক Functional Component-এর **React Hooks** (`useState`, `useEffect`, `useRef`, `useContext`) নিয়ে বিস্তারিত লেসন যুক্ত করব!
