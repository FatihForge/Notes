# ⚡ 10. মেটাডেটা ও এসইও (Static & Dynamic Metadata)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- Next.js 14-এর **Metadata API** ব্যবহার করে `<title>`, `<meta>` এবং OpenGraph ট্যাগস পরিচালনা করা।
- স্ট্যাটিক পেজের জন্য `export const metadata` অবজেক্ট ডিফাইন করা।
- ডায়নামিক পেজের জন্য `generateMetadata()` ফাংশন দিয়ে ডেটাবেস/API থেকে মেটাডেটা তৈরি করা।
- মেটাডেটা ও পেজ কম্পোনেন্টের মধ্যে রিকোয়েস্ট ডিডুপ্লিকেশন বোঝা।

---

## 🏷️ ২. স্ট্যাটিক মেটাডেটা (Static Metadata)

যে পেজের তথ্য স্থির, সেখানে সরাসরি `metadata` অবজেক্ট এক্সপোর্ট করা যায়:

```jsx
// app/about/page.js
export const metadata = {
  title: 'আমাদের সম্পর্কে | FatihForge Notes',
  description: 'আমাদের প্রতিষ্ঠান এবং সফটওয়্যার আর্কিটেকচার ভিশন সম্পর্কে জানুন।',
  openGraph: {
    title: 'আমাদের সম্পর্কে | FatihForge',
    description: 'লার্নিং ও রিভিশন গাইডলাইন।',
    images: ['/images/og-about.jpg'],
  },
};

export default function AboutPage() {
  return <h1 className="text-3xl font-bold">About Us</h1>;
}
```

---

## 🔄 ৩. ডায়নামিক মেটাডেটা (Dynamic Metadata with `generateMetadata`)

ব্লগ পোস্ট বা প্রডাক্ট পেজের ক্ষেত্রে টাইটেল ডেটাবেজ বা এপিআই থেকে আসতে হয়:

```jsx
// app/posts/[id]/page.js

// ১. ডায়নামিক মেটাডেটা জেনারেটর
export async function generateMetadata({ params }) {
  const { id } = params;
  const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
  const post = await res.json();

  return {
    title: `${post.title} | FatihForge Blog`,
    description: post.body.slice(0, 100),
    openGraph: {
      title: post.title,
      description: post.body.slice(0, 120),
    },
  };
}

// ২. মূল পেজ কম্পোনেন্ট
export default async function SinglePost({ params }) {
  return <article className="p-6">...</article>;
}
```

---

## 💡 ৪. মেটাডেটা ডিডুপ্লিকেশন (Request Deduplication)

> [!NOTE]
> **আমার মনের প্রশ্ন:** `generateMetadata`-এর ভেতর একবার fetch করলাম, আবার নিচে পেজ কম্পোনেন্টের ভেতরেও একই fetch করলাম—এতে কি সার্ভারে ডাবল রিকোয়েস্ট যাবে?  
> **উত্তর:** না! Next.js স্বয়ংক্রিয়ভাবে একই URL-এর রিকোয়েস্টকে মেমোরিতে ক্যাশ করে রাখে (Automatic Request Memoization)। ফলে সার্ভারে মাত্র **একবারই** কল হয়!

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. ক্লায়েন্ট কম্পোনেন্টে (`'use client'`) কি সরাসরি `export const metadata` ব্যবহার করা যায়?  
*(উত্তর: না, মেটাডেটা শুধুমাত্র Server Components-এ এক্সপোর্ট করা যায়।)*  
২. OpenGraph মেটাডেটার মূল প্রয়োজনীয়তা কী? (সোশ্যাল মিডিয়ায় লিংক শেয়ার করলে প্রিভিউ ইমেজ ও কার্ড দেখাতে)।

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Metadata API Reference](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)
