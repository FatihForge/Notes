# ⚡ 04. পেজেস, লেআউটস ও নেভিগেশন (Pages, Layouts & Routing)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- Next.js 14-এর **File-System Based Routing** সম্পূর্ণ আয়ত্ত করা।
- **Root Layout** এবং **Nested Segment Layouts**-এর ব্যবহার ও পার্থক্য জানা।
- সাধারণ `<a>` ট্যাগের বদলে `next/link`-এর মাধ্যমে **Client-Side Soft Navigation** নিশ্চিত করা।
- পেজ পরিবর্তনের সময় স্টেট বজায় রাখার কৌশল শেখা।

---

## 🛣️ ২. ফাইল-সিস্টেম বেসড রাউটিং কীভাবে কাজ করে?

Next.js-এ কোনো রাউটিং কনফিগারেশন ফাইল লিখতে হয় না। `app` ফোল্ডারের ভেতরে আপনি যেভাবে ফোল্ডার সাজাবেন, ইউআরএল পাথ ঠিক সেভাবেই তৈরি হবে:

```text
  app/page.js              ──> / (হোম পেজ)
  app/about/page.js        ──> /about
  app/about/mission/page.js──> /about/mission
  app/contact/page.js      ──> /contact
```

```jsx
// app/about/mission/page.js
export default function MissionPage() {
  return (
    <section className="p-6">
      <h1 className="text-2xl font-bold">আমাদের মিশন</h1>
      <p className="mt-2 text-gray-600">গুণগত টেকনিক্যাল শিক্ষা সবার কাছে পৌঁছে দেওয়া আমাদের লক্ষ্য।</p>
    </section>
  );
}
```

---

## 🧱 ৩. Root Layout বনাম Segment Layouts

### ক. Root Layout (`app/layout.js`)
এটি সমগ্র অ্যাপ্লিকেশনের শীর্ষস্থানীয় কম্পোনেন্ট। এটি নিশ্চিত করে যে প্রতিটি পেজে `<html>` এবং `<body>` ট্যাগ থাকবে এবং গ্লোবাল Header/Footer সংরক্ষিত থাকবে।

```jsx
// app/layout.js
import './globals.css';
import Link from 'next/link';

export const metadata = {
  title: 'আমার Next.js অ্যাপ',
  description: 'ব্যক্তিগত লার্নিং নোটবুক',
};

export default function RootLayout({ children }) {
  return (
    <html lang="bn">
      <body className="antialiased bg-gray-50 text-gray-900">
        <header className="p-4 bg-slate-900 text-white flex justify-between">
          <h1 className="font-bold">My Next App</h1>
          <nav className="space-x-4">
            <Link href="/" className="hover:underline">Home</Link>
            <Link href="/about" className="hover:underline">About</Link>
          </nav>
        </header>

        {/* প্রতিটি পেজের কন্টেন্ট এখানে রেন্ডার হবে */}
        <main className="container mx-auto p-6">{children}</main>

        <footer className="p-4 text-center text-sm border-t">© 2026 My Notes</footer>
      </body>
    </html>
  );
}
```

### খ. Segment Layout (`app/about/layout.js`)
সাব-ফোল্ডারের ভেতরে `layout.js` তৈরি করলে তা কেবল সেই নির্দিষ্ট সেগমেন্টের পেজগুলোর জন্যই কাজ করে।

```jsx
// app/about/layout.js
import Link from 'next/link';

export default function AboutLayout({ children }) {
  return (
    <div className="space-y-6">
      <nav className="border-b pb-3 flex gap-4 text-sm font-semibold text-blue-600">
        <Link href="/about">About Home</Link>
        <Link href="/about/mission">Our Mission</Link>
      </nav>
      {/* About-এর চাইল্ড পেজগুলো এখানে মাউন্ট হবে */}
      <div>{children}</div>
    </div>
  );
}
```

---

## 🚀 ৪. ক্লায়েন্ট-সাইড নেভিগেশন: `next/link`

> [!CAUTION]
> **কখনোই সাধারণ `<a>` ট্যাগ ব্যবহার করবেন না!**  
> `<a>` ট্যাগ ব্যবহার করলে ব্রাউজার পুরো পেজ রিফ্রেশ করে (Hard Reload), যার ফলে ক্লায়েন্ট স্টেট নষ্ট হয় এবং পারফরম্যান্স কমে।

> [!TIP]
> **কেন `next/link` ব্যবহার করব?**
> 1. **Soft Navigation:** পেজ রিফ্রেশ না করে শুধুমাত্র প্রয়োজনীয় কম্পোনেন্টটুকু পরিবর্তন করে।
> 2. **Automatic Prefetching:** ব্যবহারকারী যে লিংকগুলো স্ক্রিনে দেখছে, Next.js ব্যাকগ্রাউন্ডে সেগুলোর কোড আগে থেকেই প্রি-ফেচ করে রাখে; ফলে ক্লিকে ক্লিক করা মাত্রই ইনস্ট্যান্ট পেজ খুলে যায়!

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. Nested Layout ব্যবহার করলে কি মূল Root Layout আনমাউন্ট হয়ে যায়?  
২. `next/link` কীভাবে ব্যাকগ্রাউন্ডে প্রি-ফেচিং সম্পন্ন করে?  
৩. একটি ফোল্ডারকে রাউটে যুক্ত না করে কেবল অর্গানাইজ করার জন্য কোন সিনট্যাক্স (Route Groups) ব্যবহৃত হয়?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Routing Fundamentals](https://nextjs.org/docs/app/building-your-application/routing)
- [Linking and Navigating](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating)
