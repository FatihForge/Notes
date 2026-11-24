# ⚡ 13. ক্যাশিং ও প্রি-রেন্ডারিং: SSR, SSG, ISR (Caching & SSG)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- Next.js 14-এর **Caching Options** (`force-cache`, `no-store`, `revalidate`) সম্পূর্ণ আয়ত্ত করা।
- **SSG (Static Site Generation)** এবং **ISR (Incremental Static Regeneration)**-এর পার্থক্য জানা।
- ডায়নামিক রুটে বিল্ড-টাইমে স্ট্যাটিক পেজ তৈরি করতে `generateStaticParams()` ব্যবহার করা।
- `npm run build` টার্মিনাল রিপোর্টের সিম্বলগুলো (○, ●, ƒ) সঠিকভাবে রিড করা।

---

## 📦 ২. Caching Options (`fetch` অপশনসমূহ)

Next.js 14-এ নেটিভ `fetch` মেথডে ক্যাশিং নিয়ন্ত্রণ করা অত্যন্ত সহজ:

```javascript
// ১. SSG (Default): বিল্ড টাইমে ডেটা ক্যাশ হবে এবং স্ট্যাটিকালি সার্ভ হবে
fetch('https://api.com/data', { cache: 'force-cache' });

// ২. SSR: প্রতি রিকোয়েস্টে লাইভ সার্ভার থেকে ডেটা ফেচ হবে (কখনো ক্যাশ হবে না)
fetch('https://api.com/data', { cache: 'no-store' });

// ৩. ISR: নির্দিষ্ট সময় পর পর ব্যাকগ্রাউন্ডে ক্যাশ রি-ভ্যালিডেট হবে (যেমন: প্রতি ৬০ সেকেন্ডে)
fetch('https://api.com/data', { next: { revalidate: 60 } });
```

---

## 🚀 ৩. ডায়নামিক পেজে SSG: `generateStaticParams`

ডায়নামিক রুটগুলোকে (যেমন: `/blogs/[id]`) প্রথাগতভাবে প্রতিবার অন-ডিমান্ড সার্ভারে কম্পিউট করতে হতো। কিন্তু `generateStaticParams` ব্যবহার করে বিল্ড টাইমেই নির্দিষ্ট আইডিগুলোর জন্য স্ট্যাটিক HTML ফাইল বানিয়ে রাখা যায়!

```jsx
// app/blogs/[id]/page.js

// বিল্ড টাইমে শীর্ষ ৫টি ব্লগের পেজ স্ট্যাটিক জেনারেট হয়ে যাবে
export async function generateStaticParams() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5');
  const posts = await res.json();

  return posts.map((post) => ({
    id: post.id.toString(), // রিটার্ন অবজেক্টের মান অবশ্যই স্ট্রিং হতে হবে
  }));
}

export default async function BlogDetails({ params }) {
  const { id } = params;
  const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
  const post = await res.json();

  return (
    <article className="p-6">
      <h1 className="text-2xl font-bold">{post.title}</h1>
      <p className="mt-3 text-gray-700">{post.body}</p>
    </article>
  );
}
```

---

## 📊 ৪. Build Output চিহ্নের অর্থ (`npm run build`)

টার্মিনালে বিল্ড দিলে Next.js নিচের চিহ্নগুলো দেখায়:

```text
Route (app)                              Size     First Load JS
┌ ○ /                                    142 B          84.2 kB
├ ○ /about                               138 B          84.1 kB
├ Dynamic /blogs/[id]
│   ├ ● /blogs/1                         150 B          84.3 kB
│   ├ ● /blogs/2                         150 B          84.3 kB
└ ƒ /dashboard                           160 B          84.4 kB
```

| সিম্বল | ধরন | অর্থ ও পারফরম্যান্স প্রভাব |
|:---:|---|---|
| **○** | **Static** | কোনো ডাইনামিক ডেটা নেই; আল্ট্রা-ফাস্ট CDN থেকে সরাসরি সার্ভ হয়। |
| **●** | **SSG / StaticParams** | `generateStaticParams` দিয়ে বিল্ড টাইমে তৈরি প্রিপেয়ার্ড স্ট্যাটিক HTML ফাইল। |
| **ƒ** | **Dynamic (SSR)** | `no-store`, `cookies()` বা `headers()` থাকার কারণে প্রতি রিকোয়েস্টে সার্ভারে রেন্ডার হয়। |

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. ISR কেন SSR এবং SSG-এর মধ্যে সেরা ভারসাম্যপূর্ণ সমাধান?  
২. `generateStaticParams` ব্যবহার করলে কী পরিমাণ সার্ভার মেমোরি বাঁচে?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [generateStaticParams API Reference](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)
- [Next.js Caching Deep Dive](https://nextjs.org/docs/app/building-your-application/caching)
