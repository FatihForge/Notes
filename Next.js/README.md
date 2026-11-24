# ⚡ Next.js 14 Full-Stack Architecture & App Router Notes

Next.js হলো আধুনিক ওয়েব ডেভেলপমেন্ট এবং React ইকোসিস্টেমের সবচেয়ে শক্তিশালী **Full-Stack Production Framework**। সাধারণ ক্লায়েন্ট-সাইড React SPA-এর সীমাবদ্ধতা অতিক্রম করে সার্ভার-সাইড রেন্ডারিং (SSR), রিঅ্যাক্ট সার্ভার কম্পোনেন্টস (RSC), বিল্ট-ইন রাউটিং এবং পারফরম্যান্স অপটিমাইজেশনের সমন্বয়ে এটি প্রোডাকশন-রেডি ওয়েব অ্যাপ তৈরির স্ট্যান্ডার্ড সমাধান।

এই ডিরেক্টরিতে আমার দেখা Next.js 14 ক্র্যাশ কোর্সের মূল কনসেপ্টগুলোকে বিশ্লেষণ করে মোট ১৪টি অধ্যায়ে আমার নিজস্ব স্টাডি নোট হিসেবে গুছিয়ে রাখা হয়েছে—যাতে ভবিষ্যতে যেকোনো সময় রিভিশন দেওয়া যায় এবং ইন্টারভিউ বা প্রজেক্ট তৈরির সময় দ্রুত রেফারেন্স হিসেবে কাজে লাগে।

---

## 🗺️ লেসন রোডম্যাপ ও সূচিপত্র (Lessons Navigation)

| লেসন ডিরেক্টরি | বিষয়বস্তু | মূল শিক্ষণীয় বিষয় | স্ট্যাটাস |
|---|---|---|:---:|
| **[01-intro-and-why-nextjs](./01-intro-and-why-nextjs/)** | **Next.js পরিচিতি ও কেন ব্যবহার করব** | SPA-এর সীমাবদ্ধতা, লাইব্রেরি vs ফ্রেমওয়ার্ক, SEO ও পারফরম্যান্স সুবিধা | 📘 Notes Prepared |
| **[02-rendering-strategies-csr-ssr-rsc](./02-rendering-strategies-csr-ssr-rsc/)** | **রেন্ডারিং স্ট্র্যাটেজি (CSR vs SSR vs RSC)** | রেন্ডারিং মেকানিজম, Hydration, বেনিফিট ও আধুনিক আর্কিটেকচার | 📘 Notes Prepared |
| **[03-installation-and-folder-structure](./03-installation-and-folder-structure/)** | **ইনস্টলেশন ও ফোল্ডার স্ট্রাকচার** | `create-next-app` সেটআপ, App Router ডিরেক্টরি আর্কিটেকচার | 📘 Notes Prepared |
| **[04-pages-layouts-and-routing](./04-pages-layouts-and-routing/)** | **পেজ, লেআউট ও ফাইল-বেসড রাউটিং** | Nested Routing, Root Layout vs Child Layout, `<Link>` কম্পোনেন্ট ও নেভিগেশন | 📘 Notes Prepared |
| **[05-loading-state-suspense](./05-loading-state-suspense/)** | **লোডিং স্টেট ও রিঅ্যাক্ট সাসপেন্স** | `loading.tsx` স্পেশাল ফাইল, React Suspense বাউন্ডারি, স্কেলিটন লোডার ইউআই | 📘 Notes Prepared |
| **[06-error-handling](./06-error-handling/)** | **এরর হ্যান্ডলিং ও এরর বাউন্ডারি** | `error.tsx` স্পেশাল ফাইল, Client Component রিকোয়ারমেন্ট, `reset()` ফলব্যাক মেথড | 📘 Notes Prepared |
| **[07-dynamic-routing-and-not-found](./07-dynamic-routing-and-not-found/)** | **ডাইনামিক রাউটিং ও 404 পেজ** | `[id]` ফোল্ডার কনভেনশন, `params` প্রপস অবজেক্ট, `not-found.tsx` ও `notFound()` ফাংশন | 📘 Notes Prepared |
| **[08-server-vs-client-components](./08-server-vs-client-components/)** | **সার্ভার বনাম ক্লায়েন্ট কম্পোনেন্টস** | `"use client"` নির্দেশিকা, কম্পোনেন্ট নির্বাচনের সোনালী নিয়ম, হাইব্রিড আর্কিটেকচার | 📘 Notes Prepared |
| **[09-image-and-font-optimization](./09-image-and-font-optimization/)** | **ইমেজ ও ফন্ট অপটিমাইজেশন** | `next/image` বনাম `<img>`, CLS প্রতিরোধ, `next/font/google` ও জিরো লেআউট শিফট | 📘 Notes Prepared |
| **[10-static-and-dynamic-metadata](./10-static-and-dynamic-metadata/)** | **স্ট্যাটিক ও ডাইনামিক মেটাডেটা (SEO)** | Static `metadata` অবজেক্ট, `generateMetadata` ফাংশন, ওপেন গ্রাফ ও সোশ্যাল শেয়ার কার্ড | 📘 Notes Prepared |
| **[11-data-fetching-and-deduplication](./11-data-fetching-and-deduplication/)** | **ডেটা ফেচিং ও অটোমেটিক ডিডুপ্লিকেশন** | সার্ভার কম্পোনেন্টে সরাসরি `async/await`, স্বয়ংক্রিয় Request Deduplication ক্যাশ | 📘 Notes Prepared |
| **[12-progressive-rendering-streaming](./12-progressive-rendering-streaming/)** | **প্রোগ্রেসিভ রেন্ডারিং ও স্ট্রিমিং** | Instant UI Delivery, ফাস্ট ও স্লো কম্পোনেন্টের স্ট্রিম বিভাজন | 📘 Notes Prepared |
| **[13-ssr-ssg-isr-caching](./13-ssr-ssg-isr-caching/)** | **SSR, SSG, ISR ও ক্যাশিং কৌশল** | `force-cache`, `no-store`, `next: { revalidate: X }`, এক্সটেন্ডেড ফেচ ক্যাশ লাইফসাইকেল | 📘 Notes Prepared |
| **[14-summary-and-best-practices](./14-summary-and-best-practices/)** | **রিসোর্স সামারি, চিটশিট ও বেস্ট প্র্যাকটিস** | সর্বাঙ্গীন ফাইল কনভেনশন চার্ট, মানসিক মডেল ও প্রোডাকশন রেডি চেকলিস্ট | 📘 Notes Prepared |

---

## 🧭 দ্রুত নেভিগেশন
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../README.md)
- [📊 লার্নিং ট্র্যাকার](../LEARNING-TRACKER.md)
- [⚛️ পূর্ববর্তী ফ্রন্টএন্ড ট্র্যাক: React](../React/README.md)
- [🟢 ব্যাকএন্ড ট্র্যাক: Node.js](../Node.js/README.md)
