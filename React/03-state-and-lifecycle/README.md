# ⚛️ 03. স্টেট ম্যানেজমেন্ট ও লাইফসাইকেল মেথডস (State & Lifecycle)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 React ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- **Local State**-এর প্রয়োজনীয়তা এবং এটি কেন কম্পোনেন্টের নিজস্ব প্রাইভেট সম্পদ তা জানা।
- **Props বনাম State**-এর গভীর তাত্ত্বিক পার্থক্য স্পষ্ট করা।
- Class Component-এ `super(props)`-এর ইন্টারনাল আর্কিটেকচার এবং এটি না দিলে কী বাগ হতে পারে তা বোঝা।
- কেন সরাসরি স্টেট মিউটেট করা নিষিদ্ধ এবং `setState()` কীভাবে কাজ করে তা জানা।
- `setState()`-এর **Shallow Merge** মেকানিজম আয়ত্ত করা।
- **Event Loop Batching** এবং **Asynchronous State Updates** বোঝা ও **Updater Callback Function** ব্যবহারের নিয়ম জানা।
- **Lifecycle Methods** (`componentDidMount`, `componentWillUnmount`) এবং **Memory Leak** প্রতিরোধের টেকনিক শেখা।
- **Top-Down / Unidirectional Data Flow** (একমুখী ডেটা প্রবাহ) আত্মস্থ করা।

---

## 📖 ২. Local State কী এবং কেন প্রয়োজন?

> [!NOTE]
> **আমার সহজ উপলব্ধি:**  
> পূর্বে আমরা দেখেছি যে UI আপডেট করার জন্য বাইরে থেকে নতুন এলিমেন্ট তৈরি করে রেন্ডার করতে হতো। কিন্তু আধুনিক কম্পোনেন্ট আর্কিটেকচারের দাবি হলো—প্রতিটি কম্পোনেন্ট স্বাবলম্বী হবে এবং তার নিজস্ব অভ্যন্তরীণ ডেটা নিজে পরিচালনা করবে।  
> **Local State** হলো কোনো কম্পোনেন্টের সম্পূর্ণ নিজস্ব এবং প্রাইভেট ডেটা স্টোরেজ, যা কম্পোনেন্টটির বাইরে থেকে সরাসরি পরিবর্তনযোগ্য নয়।

### 📊 Props বনাম State-এর তুলনামূলক ছক:

| বৈশিষ্ট্য | Props (প্রপস) | State (স্টেট) |
|---|---|---|
| **উৎস (Origin)** | প্যারেন্ট কম্পোনেন্ট থেকে পাস হয়ে আসে | কম্পোনেন্টের ভেতরে সংজ্ঞায়িত হয় |
| **মালিকানা (Ownership)** | প্যারেন্ট কম্পোনেন্টের অধীনে থাকে | নিজস্ব কম্পোনেন্টের পূর্ণ নিয়ন্ত্রণে থাকে |
| **পরিবর্তনযোগ্যতা (Mutability)** | **Immutable / Read-Only** | **Mutable** (কেবলমাত্র `setState()`-এর মাধ্যমে) |
| **প্রধান উদ্দেশ্য** | চাইল্ডে ডেটা ও কনফিগারেশন পাস করা | স্থানীয় ইন্টারঅ্যাকশন ও ডাইনামিক UI নিয়ন্ত্রণ করা |

---

## 💡 ৩. State ইনিশিয়ালাইজেশন ও `super(props)`-এর ইন্টারনালস

Class Component-এর `constructor`-এ `this.state` ইনিশিয়ালাইজ করা হয়:

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props); // ES6 Inheritance ও React নিয়ম
    this.state = {
      date: new Date()
    };
  }
  
  render() {
    return <h2>সময়: {this.state.date.toLocaleTimeString()}</h2>;
  }
}
```

### 🧠 `super(props)` কেন আবশ্যক?

```text
  ┌─────────────────────────────────────────────────────────────┐
  │ ১. ES6 Class Inheritance Rule:                              │
  │    যখন একটি সাব-ক্লাস কোনো প্যারেন্ট ক্লাসকে এক্সটেন্ড করে,   │
  │    তখন super() কল করার পূর্বে this কিওয়ার্ড ব্যবহার নিষিদ্ধ!│
  │    (অন্যথায় Uncaught ReferenceError ঘটবে)।                   │
  ├─────────────────────────────────────────────────────────────┤
  │ ২. this.props ইনিশিয়ালাইজেশন:                               │
  │    super(props)-এ props পাস করার ফলেই React.Component তার    │
  │    বেস কনস্ট্রাক্টরে this.props সেট করতে পারে।               │
  │    শুধু super() দিলে constructor-এ this.props = undefined!  │
  └─────────────────────────────────────────────────────────────┘
```

---

## ⚠️ ৪. `setState()`-এর নিয়মাবলী ও Shallow Merge

### কড়া নিয়ম: সরাসরি State পরিবর্তন নিষিদ্ধ
```javascript
// ❌ ভুল এবং সম্পূর্ণ নিষিদ্ধ! (রিয়্যাক্ট রি-রেন্ডার ট্রিগার হবে না)
this.state.date = new Date();

//  সঠিক নিয়ম: সর্বদা setState() ব্যবহার করতে হবে
this.setState({
  date: new Date()
});
```

### `setState()`-এর Shallow Merge মেকানিজম:
`this.setState()` কল করলে রিয়্যাক্ট পুরো স্টেট অবজেক্টকে ধ্বংস বা প্রতিস্থাপন করে না; বরং এটি **Shallow Merge (অগভীর সংযোজন)** করে।

```jsx
import React from 'react';

class PostList extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: ['প্রথম কমেন্ট', 'দ্বিতীয় কমেন্ট']
    };
  }

  componentDidMount() {
    // API থেকে ডেটা আসার পর শুধুমাত্র posts আপডেট করা হচ্ছে
    setTimeout(() => {
      const fetchedPosts = [{ id: 1, title: 'রিয়্যাক্ট আর্কিটেকচার' }];
      
      this.setState({
        posts: fetchedPosts
        // comments প্রপার্টি এখানে না উল্লেখ করলেও তা অক্ষুণ্ণ থাকবে (Shallow Merge)
      });
    }, 1000);
  }

  render() {
    return (
      <div>
        <h2>পোস্ট সংখ্যা: {this.state.posts.length}</h2>
        <h2>কমেন্ট সংখ্যা: {this.state.comments.length}</h2>
      </div>
    );
  }
}

export default PostList;
```

---

## ⚡ ৫. Event Loop Batching এবং Asynchronous State Updates

রিয়্যাক্ট অ্যাপ্লিকেশনের পারফর্ম্যান্স অপটিমাইজ করার স্বার্থে ইভেন্ট হ্যান্ডলারের ভেতরের একাধিক `setState()` কলকে একসাথে **Batch (একত্রিত)** করে অ্যাসিনক্রোনাসলি এক্সিকিউট করে। এর ফলে প্রতিটি `setState()`-এর জন্য বারবার ব্রাউজারে রি-রেন্ডারিং না হয়ে একটি একক রেন্ডার পাসে সমস্ত আপডেট সম্পন্ন হয়।

### ⚠️ বিপদ: বর্তমান স্টেটের ওপর নির্ভর করে স্টেট আপডেট করা
যেহেতু `setState()` অ্যাসিনক্রোনাস, তাই নিচের কোডটি ঝুঁকিপূর্ণ:

```javascript
// ❌ ভুল পদ্ধতি (অ্যাসিনক্রোনাস ব্যাচিংয়ের কারণে ভুল মান গণনা হতে পারে)
this.setState({
  counter: this.state.counter + this.props.increment
});
```

###  সমাধান: Updater Callback Function পাস করা
পূর্ববর্তী স্টেট বা প্রপসের ওপর ভিত্তি করে নিখুঁত গণনা নিশ্চিত করতে `setState()`-এ অবজেক্টের বদলে একটি **Callback Function** পাস করতে হয়:

```javascript
//  সঠিক পদ্ধতি (Functional setState)
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

---

## 🔄 ৬. Component Lifecycle Methods ও Memory Leak প্রতিরোধ

একটি রিয়্যাক্ট কম্পোনেন্টের জন্ম (DOM-এ মাউন্ট হওয়া), জীবনকাল (স্টেট/প্রপস আপডেট) এবং মৃত্যু (DOM থেকে আনমাউন্ট হওয়া)—এই পুরো চক্রকে **Component Lifecycle** বলা হয়।

```text
  [ Component Mounts ] ───▶ componentDidMount() ───▶ (Timer/API/Subscription চালু)
          │
          ▼
   [ State Updates ]   ───▶ render() ───▶ (Virtual DOM Diffing & UI Update)
          │
          ▼
 [ Component Unmounts ] ──▶ componentWillUnmount() ─▶ (clearInterval / Cleanup)
```

1. **`componentDidMount()`:** কম্পোনেন্ট ব্রাউজার DOM-এ সফলভাবে প্রথমবার রেন্ডার হওয়ার সাথে সাথে চালু হয়। API কল, টাইমার চালু বা ইভেন্ট সাবস্ক্রিপশনের জন্য আদর্শ।
2. **`componentWillUnmount()`:** কম্পোনেন্ট DOM থেকে মুছে ফেলার ঠিক পূর্বমুহূর্তে রান হয়। **Memory Leak প্রতিরোধ** করার জন্য টাইমার বন্ধ (`clearInterval`) বা ইভেন্ট লিসেনার রিমুভ করা আবশ্যক।

### পূর্ণাঙ্গ আত্মনির্ভরশীল `Clock` কম্পোনেন্ট:

```jsx
import React from 'react';

class Clock extends React.Component {
  // Step 1: Initialize Local State & call super(props)
  constructor(props) {
    super(props);
    this.state = { 
      date: new Date() 
    };
  }

  // Step 2: Set up timer in componentDidMount after DOM mounting
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  // Step 3: Clean up timer in componentWillUnmount to prevent memory leak
  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  // Step 4: Update state periodically via setState
  tick() {
    this.setState({
      date: new Date()
    });
  }

  // Step 5: Render UI snapshot based on current state
  render() {
    return (
      <div>
        <h1>বর্তমান সময়:</h1>
        <h2>{this.state.date.toLocaleTimeString(this.props.locale)}</h2>
      </div>
    );
  }
}

export default Clock;
```

---

## 🌊 ৭. Top-Down / Unidirectional Data Flow

রিয়্যাক্ট আর্কিটেকচারের অনন্য বৈশিষ্ট্য হলো **একমুখী ডেটা প্রবাহ (Top-Down Data Flow)**:

```text
       [ Parent Component (Holds State) ]
                      │
                      ▼  (Passes state as Prop)
        [ Child Component (Receives Prop) ]
                      │
                      ▼  (Passes state as Prop)
       [ Sub-Child Component (Pure Display) ]
```

- ডেটা সর্বদা ওপরের প্যারেন্ট থেকে নিচের চাইল্ড কম্পোনেন্টে প্রবাহিত হয়।
- কোনো কম্পোনেন্টের State তার নিজস্ব প্রাইভেট সম্পদ। কিন্তু সে চাইলে তার State-কে নিচে থাকা চাইল্ডে Props হিসেবে পাস করতে পারে:
  ```jsx
  <FormattedDate date={this.state.date} />
  ```
- চাইল্ড কম্পোনেন্ট (`FormattedDate`) পুরোপুরি নিরপেক্ষ যে এই `date` ডেটাটি প্যারেন্টের State থেকে এলো, নাকি Props থেকে এলো, নাকি কোনো হার্ডকোডেড ভ্যালু।

---

## 📝 ৮. কুইক রিভিশন চিটশিট (Summary Cheatsheet)

| কনসেপ্ট | নিয়ম ও মেকানিজম |
|---|---|
| **Local State** | কম্পোনেন্টের প্রাইভেট পরিবর্তনশীল মেমোরি। |
| **`super(props)`** | `this` অ্যাক্সেস নিশ্চিত করতে এবং `this.props` ইনিশিয়ালাইজ করতে কনস্ট্রাক্টরে প্রথম স্টেটমেন্ট। |
| **Direct Mutation** | সম্পূর্ণ নিষিদ্ধ; কেবল `setState()` UI রি-রেন্ডার ট্রিগার করে। |
| **Shallow Merge** | `setState()` কেবল উল্লেখিত কী আপডেট করে, বাকি স্টেট অপরিবর্তিত থাকে। |
| **Async Batching** | একাধিক `setState` একত্র হয়ে রান হয়; পূর্ববর্তী স্টেটের ওপর নির্ভরশীল হলে Callback ফাংশন ব্যবহার করতে হয়। |
| **Cleanup** | `componentWillUnmount()`-এ টাইমার বা সাবস্ক্রিপশন ক্লিয়ার না করলে মেমোরি লিক ঘটে। |

---

## ⏭️ পরবর্তী লেসনের যোগসূত্র
কম্পোনেন্টে ব্যবহারকারীর বিভিন্ন ইন্টারঅ্যাকশন (যেমন বাটনে ক্লিক, ইনপুট টাইপ করা) হ্যান্ডেল করে কীভাবে স্টেট পরিবর্তন করতে হয়, তা আমরা পরবর্তী [০৪. React ইভেন্ট হ্যান্ডলিং ও ইন্টারঅ্যাকশন](../04-event-handling/README.md) লেসনে শিখব।
