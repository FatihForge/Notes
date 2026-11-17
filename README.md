# 📚 FatihForge Personal Notes — MERN Stack & TypeScript

স্বাগতম! এটি আমার **ব্যক্তিগত স্টাডি নোটস এবং রিভিশন আর্কাইভ (Personal Learning & Revision Repository)**।

> [!IMPORTANT]
> **💡 এই রিপোজিটরির মূল উদ্দেশ্য:**  
> এটি কোনো সাধারণ জেনেরিক টিউটোরিয়াল বা গতানুগতিক স্টাডি গাইড নয়। পুরো সফটওয়্যার ডেভেলপমেন্ট যাত্রায় **আমি নিজে যা যা শিখেছি, আমার সেই ব্যক্তিগত শেখার উপকরণ, লেকচার নোটস ও নিজস্ব ব্যাখ্যাগুলোকে আমি এখানে গুছিয়ে সংরক্ষণ করে রাখছি**—যাতে:
> 1. **ভবিষ্যতে দ্রুত রিভিশন (Quick Revision)** দেওয়া যায়।
> 2. কোনো কনসেপ্ট পরবর্তীতে কখনো ভুলে গেলে এখান থেকে সহজেই আবার পড়ে ঝালাই (Refresh) করে নেওয়া যায়।
> 3. ইন্টারভিউ প্রস্তুতি কিংবা বাস্তব কাজের সময় একটি নির্ভরযোগ্য ব্যক্তিগত টেকনিক্যাল রেফারেন্স হিসেবে ব্যবহার করা যায়।

আমার মূল শেখার মেটেরিয়াল ও ব্যাখ্যাগুলোকে অক্ষুণ্ণ রেখে প্রতিটি চ্যাপ্টারে ইন্টারনাল আর্কিটেকচার ডায়াগ্রাম, হ্যান্ডস-অন কোড এক্সাম্পল, সম্ভাব্য কমন ভুল এবং কুইক রিভিশন সামারি সংযুক্ত করে রাখা হয়েছে।

---

## 🗺️ রিপোজিটরি নেভিগেশন (Personal Tracks Navigation)

| প্রযুক্তি ট্র্যাক | বিবরণ (Description) | নোটস ডিরেক্টরি | প্রস্তুতকৃত লেসন সংখ্যা | লার্নিং স্ট্যাটাস |
|---|---|:---:|:---:|:---:|
| **JavaScript** | ভাষা ফান্ডামেন্টালস, Functions, DOM, Classes, Async JS ও Closures | [`/JavaScript`](./JavaScript/) | ১২টি লেসন | 🟢 Completed |
| **TypeScript** | স্ট্যাটিক টাইপ চেকার, Superset, `tsconfig`, Type System ও Compilation | [`/TypeScript`](./Typescript/) | ১টি লেসন (প্রস্তুত) | 🟡 In Progress |
| **Node.js** | সার্ভার-সাইড রানটাইম, V8 ইঞ্জিন, Streams, Event Loop এবং কোর মডিউল | [`/Node.js`](./Node.js/) | ৮টি লেসন | 🟢 Completed |
| **Express.js** | ফ্লেক্সিবল ওয়েব ফ্রেমওয়ার্ক, মিডলওয়্যার, MVC, Routing, Auth ও Uploads | [`/Express.js`](./Express.js/) | ১১টি লেসন | 🟢 Completed |
| **MongoDB** | NoSQL ডেটাবেস, BSON, Compass, Atlas কানেকশন ও Mongoose ODM | [`/MongoDB`](./MongoDB/) | ৩টি লেসন | 🟢 Completed |
| **React** | ডিক্লেয়ারেটিভ UI, JSX, Props, Events ও useState Hook আর্কিটেকচার | [`/React`](./React/) | ১টি লেসন (প্রস্তুত) | 🟡 In Progress |

> 📊 **আমার সম্পূর্ণ শেখার অগ্রগতি ও রিভিশন ট্র্যাকার দেখতে দেখুন: [LEARNING-TRACKER.md](./LEARNING-TRACKER.md)**

---

## 📁 ডিরেক্টরি স্ট্রাকচার (Repository Folder Structure)

```text
Notes/
│
├── README.md                                # ব্যক্তিগত রিপোজিটরি পরিচিতি ও নেভিগেশন হাব
├── LEARNING-TRACKER.md                      # ব্যক্তিগত লার্নিং ও রিভিশন ট্র্যাকার
│
├── JavaScript/                              # JavaScript Foundation & Core Engine (১২টি লেসন)
│   ├── README.md
│   ├── 01-basics-variables-data-types/
│   ├── 02-operators-conditional-statements/
│   ├── 03-loops-and-strings/
│   ├── 04-arrays/
│   ├── 05-functions-and-methods/
│   ├── 06-dom-manipulation/
│   ├── 07-events/
│   ├── 08-classes-and-objects/
│   ├── 09-callbacks-promises-async-await/
│   ├── 10-fetch-api-http-requests/
│   ├── 11-closures-execution-context/
│   └── 12-methods-overview/
│
├── TypeScript/                              # TypeScript Static Typing & Compilation
│   ├── README.md
│   ├── src/                                 # মূল সোর্স নোট
│   └── 01-basics-setup-and-types/           # লেকচার ০১: ইনস্টলেশন ও টাইপ সিস্টেম
│
├── Node.js/                                 # Node.js Core Architecture (৮টি লেসন)
│   ├── README.md
│   ├── 01-introduction/
│   ├── 02-installation-and-repl/
│   ├── 03-first-server-and-core-modules/
│   ├── 04-request-response-routing/
│   ├── 05-streams-buffers-parsing/
│   ├── 06-event-loop-architecture/
│   ├── 07-npm-and-tools/
│   └── 08-debugging-guide/
│
├── Express.js/                              # Express.js Web Framework & APIs (১১টি লেসন)
│   ├── README.md
│   ├── 01-introduction/
│   ├── 02-middleware-and-routing/
│   ├── 03-static-files-and-tailwind/
│   ├── 04-template-engines-ejs/
│   ├── 05-mvc-architecture/
│   ├── 06-dynamic-routes-params/
│   ├── 07-crud-update-delete/
│   ├── 08-sessions-and-cookies/
│   ├── 09-authentication-and-security/
│   ├── 10-file-upload-multer/
│   └── 11-async-client-requests-json/
│
├── MongoDB/                                 # MongoDB & Mongoose Database (৩টি লেসন)
│   ├── README.md
│   ├── 01-databases-sql-vs-nosql/
│   ├── 02-mongodb-fundamentals-crud/
│   └── 03-mongoose-odm/
│
└── React/                                   # React Frontend Engineering
    ├── README.md
    └── 01-react-fundamentals/               # JSX, Props, Events ও useState
```

---

## 🎯 ব্যক্তিগত নোটের বৈশিষ্ট্য ও মানদণ্ড (Documentation Standards)

1. **ব্যক্তিগত লার্নিং ফোকাস:** আমার নিজস্ব নোটস ও ধারণাই এখানে প্রধান উৎস; কোনো জেনেরিক এআই টিউটোরিয়াল হিসেবে নয়, বরং নিজের শেখার স্মৃতিকে স্থায়ী রূপ দেওয়া হয়েছে।
2. **সহজ ও প্রাঞ্জল বাংলা:** জটিল কনসেপ্টগুলো সহজে রিভিশন দেওয়ার স্বার্থে বাংলায় ব্যাখ্যা, কিন্তু সমস্ত টেকনিক্যাল টার্ম (যেমন: Runtime, Event Loop, Closures, Virtual DOM, Superset) আন্তর্জাতিক মান বজায় রেখে ইংরেজিতে রাখা হয়েছে।
3. **ভবিষ্যতের রিভিশন উপযোগিতা:** প্রতিটি লেসনের শেষে কুইক রিভিশন চার্ট এবং ইন্টারভিউ উপযোগী প্রশ্নাবলী রয়েছে, যাতে দীর্ঘ বিরতির পর মাত্র ৫ মিনিটে পুরো টপিক রিভিশন দেওয়া সম্ভব হয়।
4. **ভিজ্যুয়াল আর্কিটেকচার:** ডেটা ফ্লো ও ইন্টারনাল এক্সিকিউশন মনে রাখার সুবিধার্থে ASCII ডায়াগ্রাম ও চার্ট যুক্ত।
5. **বাস্তব কোড ও সতর্কতা:** নিজে রান করা ভেরিফাইড কোড এক্সাম্পল এবং সাধারণত যেসব এরর বা ভুল হতে পারে সেগুলোর আগাম সতর্কতা।
