# ⚡ 06. এরর হ্যান্ডলিং ও রিকভারি (Error Handling with error.js)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- Next.js 14-এর স্পেশাল ফাইল `error.js`-এর কাজ ও মেকানিজম শেখা।
- কেন `error.js`-কে অবশ্যই একটি **Client Component (`'use client'`)** হতে হয় তা ব্যাখ্যা করা।
- `reset()` ফাংশন ব্যবহার করে পেজ রিলোড ছাড়াই কম্পোনেন্ট রি-রেন্ডার করে এরর রিকভারি করা।
- প্রোডাকশনে **Error Digest** দিয়ে সার্ভার এরর ট্র্যাক করার উপায় জানা।

---

## ⚠️ ২. এরর বাউন্ডারি আর্কিটেকচার

Next.js স্বয়ংক্রিয়ভাবে প্রতিটি রাউট সেগমেন্ট এবং তার চাইল্ড কম্পোনেন্টগুলোকে একটি **React Error Boundary** দিয়ে ঘিরে রাখে। কোনো পেজে বা ডেটা ফেচে এক্সেপশন থ্রো হলে পুরো ওয়েবসাইট ক্র্যাশ না করে শুধু সেই অংশটিতে `error.js` ফুটে ওঠে।

```text
  ┌─────────────────────────────────────────────────────────────┐
  │                 <ErrorBoundary fallback={<Error />}>        │
  │                     <PageContent />                         │
  │                 </ErrorBoundary>                            │
  └─────────────────────────────────────────────────────────────┘
```

---

## 💻 ৩. প্র্যাকটিক্যাল কোড উদাহরণ

> [!IMPORTANT]
> **বাধ্যতামূলক নিয়ম:** `error.js` ফাইলে অবশ্যই সবার উপরে `'use client'` ডিরেক্টিভ দিতে হবে, কারণ ক্লায়েন্টে ইউজার ইন্টারঅ্যাকশন (`reset()` ক্লিক) এবং রি-রেন্ডারিং হ্যান্ডেল করতে হয়।

```jsx
// app/blogs/error.js
'use client'; // ক্লায়েন্ট কম্পোনেন্ট হওয়া বাধ্যতামূলক

import { useEffect } from 'react';

export default function Error({ error, reset }) {
  useEffect(() => {
    // প্রোডাকশন এরর ট্র্যাকিং সার্ভিসে লগ পাঠানো (যেমন: Sentry)
    console.error('Captured Error:', error);
    if (error.digest) {
      console.log('Error Digest ID:', error.digest);
    }
  }, [error]);

  return (
    <div className="p-8 my-6 bg-red-50 border border-red-200 rounded-xl text-center space-y-4">
      <h2 className="text-2xl font-bold text-red-700">অনাকাঙ্ক্ষিত কোনো সমস্যা হয়েছে!</h2>
      <p className="text-gray-600 text-sm max-w-md mx-auto">
        {error.message || 'সার্ভার থেকে ডেটা লোড করার সময় একটি ত্রুটি দেখা দিয়েছে।'}
      </p>

      {error.digest && (
        <p className="text-xs font-mono text-gray-400">Error Digest: {error.digest}</p>
      )}

      <div>
        <button
          onClick={() => reset()} // পুরো পেজ রিলোড না করে কম্পোনেন্টটি পুনরায় রেন্ডারের চেষ্টা করবে
          className="px-5 py-2.5 bg-red-600 hover:bg-red-700 text-white font-medium rounded-lg shadow-sm transition-all"
        >
          Try Again (পুনরায় চেষ্টা করুন)
        </button>
      </div>
    </div>
  );
}
```

---

## 🛡️ ৪. Root Layout-এর এরর কীভাবে হ্যান্ডেল করব?

> [!WARNING]
> রুট ডিরেক্টরির `app/error.js` কিন্তু `app/layout.js`-এর ভেতরের এরর ক্যাচ করতে পারে না!  
> Root Layout-এর কোনো এরর হ্যান্ডেল করতে হলে `app/global-error.js` ব্যবহার করতে হয় (যাতে নিজস্ব `<html>` ও `<body>` ট্যাগ থাকতে হয়)।

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. `error.js`-এ `reset()` ফাংশনটি কল করলে ব্যাকগ্রাউন্ডে কী ঘটে?  
২. Error Digest কী এবং এটি ক্লায়েন্টকে সার্ভারের সিক্রেট মেসেজ দেখা থেকে কীভাবে রক্ষা করে?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Error Handling Documentation](https://nextjs.org/docs/app/building-your-application/routing/error-handling)
