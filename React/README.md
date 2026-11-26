# ⚛️ React Frontend Engineering Study Notes

React হলো Meta (Facebook) দ্বারা তৈরি একটি আধুনিক, কম্পোনেন্ট-বেসড এবং ডিক্লেয়ারেটিভ জাভাস্ক্রিপ্ট লাইব্রেরি। এটি সিঙ্গেল পেজ অ্যাপ্লিকেশন (SPA) এবং ডায়নামিক, রেসপনসিভ ইউজার ইন্টারফেস (UI) তৈরির জন্য বিশ্বব্যাপী সর্বাধিক ব্যবহৃত ফ্রন্টএন্ড প্রযুক্তি।

এই ডিরেক্টরিতে আমার ব্যক্তিগত শেখার উপকরণ ([src.txt](./src.txt) ও [src2.txt](./src2.txt))-এর ওপর ভিত্তি করে React-এর ইন্টারনাল আর্কিটেকচার, এলিমেন্ট রেন্ডারিং, কম্পোনেন্ট, প্রপস, স্টেট ও লাইফসাইকেল নিয়ে মোট ৪টি স্বয়ংসম্পূর্ণ স্টাডি নোট প্রস্তুত রাখা হয়েছে—যাতে ভবিষ্যতে যেকোনো সময় দ্রুত রিভিশন দেওয়া যায়।

---

## 🗺️ লেসন রোডম্যাপ ও সূচিপত্র (Lessons Navigation)

| লেসন ডিরেক্টরি | বিষয়বস্তু | মূল শিক্ষণীয় বিষয় | স্ট্যাটাস |
|---|---|---|:---:|
| **[01-jsx-and-element-rendering](./01-jsx-and-element-rendering/)** | **JSX ও এলিমেন্ট রেন্ডারিং মেকানিজম** | DOM vs React Element, `React.createElement`, Babel ট্রান্সপাইলেশন, XSS প্রতিরোধ, Immutability, Virtual DOM ও Reconciliation | 🟢 Completed |
| **[02-components-and-props](./02-components-and-props/)** | **কম্পোনেন্ট আর্কিটেকচার ও প্রপস** | Functional vs Class Component, Instantiation, Read-Only Props, Pure Functions ও Component Composition | 🟢 Completed |
| **[03-state-and-lifecycle](./03-state-and-lifecycle/)** | **স্টেট ম্যানেজমেন্ট ও লাইফসাইকেল মেথডস** | Local State, `super(props)` ইন্টারনালস, `setState()` Shallow Merge, Async Batching, Lifecycle Methods ও Memory Leak প্রতিরোধ | 🟢 Completed |
| **[04-event-handling](./04-event-handling/)** | **ইভেন্ট হ্যান্ডলিং ও ইন্টারঅ্যাকশন** | SyntheticEvent সিস্টেম, `camelCase`, `e.preventDefault()`, আর্গুমেন্ট পাসিং ও ইভেন্টের মাধ্যমে State আপডেট | 🟢 Completed |
| **[05-react-hooks-overview](./README.md)** | **আধুনিক রিয়্যাক্ট হুকস (`useState`, `useEffect`)** | (ভবিষ্যতে বিস্তারিত নোট আসবে) Functional Component State, Side Effects ও Custom Hooks | ⚪ Roadmap |
| **[06-forms-controlled-components](./README.md)** | **ফর্ম ও ইনপুট হ্যান্ডলিং** | (রোডম্যাপ) Controlled Components, Form Validation ও সাবমিশন | ⚪ Roadmap |
| **[07-context-api-state-management](./README.md)** | **গ্লোবাল স্টেট ম্যানেজমেন্ট (Context API / Redux)** | (রোডম্যাপ) প্রপ ড্রিলিং সমাধান ও গ্লোবাল অ্যাপ স্টেট | ⚪ Roadmap |

---

## 🧭 দ্রুত নেভিগেশন
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../README.md)
- [📊 লার্নিং ট্র্যাকার](../LEARNING-TRACKER.md)
- [⚡ ফুল-স্ট্যাক পরবর্তী ট্র্যাক: Next.js](../Next.js/README.md)
- [🚀 ব্যাকএন্ড ট্র্যাক: Express.js](../Express.js/README.md)
