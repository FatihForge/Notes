# ⚡ 07. ডায়নামিক রাউটিং ও কাস্টম ৪০৪ (Dynamic Routing & Not Found)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- স্কয়ার ব্র্যাকেট (`[param]`) সিনট্যাক্স দিয়ে ডায়নামিক ইউআরএল হ্যান্ডেল করা।
- পেজ কম্পোনেন্টে `params` প্রপ থেকে ডায়নামিক ভ্যালু রিসিভ করা।
- কাস্টম ৪০৪ পেজের জন্য `not-found.js` ব্যবহার করা।
- শর্তসাপেক্ষে কোডের ভেতর থেকে `notFound()` ফাংশন ট্রিগার করা।

---

## 🛣️ ২. ডায়নামিক রুট সিনট্যাক্স: `app/blogs/[id]/page.js`

যখন ব্লগের আইডি, প্রডাক্ট স্ল্যাগ বা ইউজারনেম অনুযায়ী রুট তৈরি করতে হয়:

```text
  app/blogs/[id]/page.js
  ├── /blogs/1      ──> params.id = "1"
  ├── /blogs/react  ──> params.id = "react"
  └── /blogs/100    ──> params.id = "100"
```

### প্র্যাকটিক্যাল কোড:
```jsx
// app/blogs/[id]/page.js
import { notFound } from 'next/navigation';

export default async function BlogDetails({ params }) {
  const { id } = params;

  // সিমুলেশন: যদি আইডি ৩ হয়, তবে ডেটাবেসে এই ব্লগটি নেই
  if (id === '3') {
    notFound(); // এটি সরাসরি নিকটের not-found.js ফাইলটি এক্সিকিউট করবে
  }

  return (
    <main className="p-6 space-y-3">
      <h1 className="text-3xl font-bold">ব্লগ বিস্তারিত</h1>
      <p className="text-gray-700">
        বর্তমান ব্লগ আইডি: <span className="font-mono bg-blue-100 text-blue-800 px-2 py-1 rounded">{id}</span>
      </p>
    </main>
  );
}
```

---

## 🚫 ৩. কাস্টম Not Found পেজ (`app/not-found.js`)

ব্যবহারকারী যদি এমন কোনো লিংকে যায় যা অস্তিত্বহীন, তখন ডিফল্ট অপরিচ্ছন্ন ৪০৪ পেজের পরিবর্তে সুন্দর কাস্টম পেজ প্রদর্শন করা:

```jsx
// app/not-found.js
import Link from 'next/link';

export default function NotFound() {
  return (
    <div className="flex flex-col items-center justify-center min-h-[60vh] text-center p-6 space-y-4">
      <h1 className="text-6xl font-extrabold text-blue-600">404</h1>
      <h2 className="text-2xl font-bold text-gray-800">কাঙ্ক্ষিত পেজটি পাওয়া যায়নি!</h2>
      <p className="text-gray-500 max-w-sm">
        আপনি যে রিসোর্সটি খুঁজছেন তা মুছে ফেলা হয়েছে অথবা ইউআরএলটি ভুল লেখা হয়েছে।
      </p>
      <Link 
        href="/"
        className="px-6 py-2.5 bg-blue-600 hover:bg-blue-700 text-white rounded-lg font-medium shadow transition-all"
      >
        হোম পেজে ফিরে যান
      </Link>
    </div>
  );
}
```

---

## 💡 ৪. `notFound()` ফাংশনের সুবিধা
- ডেটাবেস কোয়েরি করে যদি কাঙ্ক্ষিত ডেটা `null` আসে, তবে বিভ্রান্তিকর খালি পেজ না দেখিয়ে তাৎক্ষণিক `notFound()` কল করে ৪০৪ ইউআই রেন্ডার করা যায়।

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. `[...slug]` (Catch-all segments) এবং `[[...slug]]` (Optional catch-all)-এর মধ্যে পার্থক্য কী?  
২. `notFound()` কল করলে HTTP Response Status Code কত পাঠানো হয়? (উত্তর: 404)

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Dynamic Routes](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)
- [not-found.js API Reference](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
