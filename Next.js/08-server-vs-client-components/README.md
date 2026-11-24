# ⚡ 08. সার্ভার বনাম ক্লায়েন্ট কম্পোনেন্টস (Server vs Client Components)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- Server Components এবং Client Components-এর সঠিক দায়িত্বের বিভাজন বোঝা।
- `'use client'` ডিরেক্টিভ কখন, কোথায় এবং কেন ব্যবহার করতে হবে তা স্পষ্ট করা।
- সার্ভার কম্পোনেন্টে ক্লায়েন্ট কম্পোনেন্ট ইমপোর্ট করার নিয়ম ও সিকিউরিটি বাউন্ডারি রক্ষা করা।
- আর্কিটেকচারাল ডিসিশন ম্যাট্রিক্স আয়ত্ত করা।

---

## ⚖️ ২. সার্ভার ও ক্লায়েন্ট কম্পোনেন্টের গভীর তুলনা

```text
  ┌─────────────────────────────────────────────────────────────┐
  │                 Server Component (Default)                  │
  ├─────────────────────────────────────────────────────────────┤
  │ 🌟 সরাসরি ডেটাবেজ অ্যাক্সেস ও সিক্রেট এপিআই কি রাখা যায়      │
  │ 🌟 ব্রাউজার বান্ডেলে ০ বাইট জাভাস্ক্রিপ্ট যোগ করে            │
  │ ❌ কোনো ব্রাউজার ইভেন্ট (onClick, onChange) চলে না          │
  │ ❌ কোনো রিয়্যাক্ট হুক (useState, useEffect) চলে না          │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
             (ইন্টারঅ্যাক্টিভিটি প্রয়োজন হলে সীমানা নির্ধারণ)
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │                 Client Component ('use client')             │
  ├─────────────────────────────────────────────────────────────┤
  │ 🌟 ব্রাউজার ইভেন্ট (onClick, onSubmit) সমর্থন করে            │
  │ 🌟 স্টেট ও হুক্স (useState, useEffect, custom hooks) চলে     │
  │ ❌ জাভাস্ক্রিপ্ট কোড ব্রাউজারে ডাউনলোড হয়                    │
  │ ❌ সরাসরি ডেটাবেজ কানেক্ট করা মারাত্মক নিরাপত্তা ঝুঁকি       │
  └─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ ৩. `'use client'` ডিরেক্টিভ কীভাবে ব্যবহার করব?

> [!TIP]
> **বেস্ট প্র্যাকটিস (Push Client Components to the Leaves):**  
> পুরো পেজকে কখনো `'use client'` বানাবেন না! পেজটি সার্ভার কম্পোনেন্ট হিসেবেই থাকবে; শুধু যেখানে বোতাম বা ফর্মের ক্লিক দরকার, সেই ছোট কম্পোনেন্টটুকুকে আলাদা ফাইলে নিয়ে `'use client'` লিখবেন।

```jsx
// app/components/LikeButton.js
'use client'; // এটি নির্দেশ করে যে এই ফাইলটি ক্লায়েন্ট বান্ডেলে যাবে

import { useState } from 'react';

export default function LikeButton() {
  const [likes, setLikes] = useState(0);

  return (
    <button 
      onClick={() => setLikes(likes + 1)}
      className="px-4 py-2 bg-pink-600 text-white rounded-lg hover:bg-pink-700"
    >
      ❤️ লাইক: {likes}
    </button>
  );
}
```

```jsx
// app/posts/page.js - এটি সার্ভার কম্পোনেন্ট হিসেবেই থাকবে
import db from '@/lib/db';
import LikeButton from '@/components/LikeButton'; // ক্লায়েন্ট কম্পোনেন্ট ইমপোর্ট

export default async function PostPage() {
  const post = await db.post.findFirst();

  return (
    <article className="p-6">
      <h1 className="text-3xl font-bold">{post.title}</h1>
      <p className="mt-2 text-gray-700">{post.content}</p>
      
      {/* শুধু ইন্টারেক্টিভ অংশটি ক্লায়েন্ট কম্পোনেন্ট */}
      <div className="mt-4">
        <LikeButton />
      </div>
    </article>
  );
}
```

---

## 📋 ৪. সিদ্ধান্ত গ্রহণের চেকলিস্ট (When to use what?)

| আপনার কাজের প্রয়োজনীয়তা | সার্ভার কম্পোনেন্ট | ক্লায়েন্ট কম্পোনেন্ট |
|---|:---:|:---:|
| ব্যাকএন্ড ডেটা সরাসরি ফেচ করা | ✅ | ❌ |
| এপিআই কি বা টোকেন নিরাপদে রাখা | ✅ | ❌ |
| বড় ডিপেনডেন্সি ব্যবহার করে সাইজ কমানো | ✅ | ❌ |
| `onClick`, `onChange` ইভেন্ট লিসেনার | ❌ | ✅ |
| `useState`, `useReducer`, `useEffect` | ❌ | ✅ |
| ব্রাউজার API (localStorage, geolocation) | ❌ | ✅ |

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. ক্লায়েন্ট কম্পোনেন্টের ভেতরে কি সরাসরি কোনো সার্ভার কম্পোনেন্টকে ইমপোর্ট করা যায়?  
*(উত্তর: সরাসরি ইমপোর্ট করা যায় না; তবে চিলড্রেন প্রপ `<ClientComp>{children}</ClientComp>` আকারে পাস করা যায়।)*  
২. সার্ভার কম্পোনেন্টে `window` বা `document` অ্যাক্সেস করলে কী ঘটবে?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Server and Client Composition Patterns](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns)
