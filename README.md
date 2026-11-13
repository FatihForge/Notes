# 📚 FatihForge Notes: Full-Stack MERN Technical Documentation & Study Guide

স্বাগতম! এটি আমার **MERN Stack (MongoDB, Express.js, React, Node.js)** এবং এর ভিত্তি **JavaScript**-এর একটি পূর্ণাঙ্গ, সুসংগঠিত, টেকনিক্যাল স্টাডি রিপোজিটরি।

প্রতিটি চ্যাপ্টারের মূল তাত্ত্বিক আলোচনা, আর্কিটেকচার ডায়াগ্রাম, প্র্যাকটিক্যাল কোড এক্সাম্পল, টার্মিনাল আউটপুট, কমন ভুল এবং সেলফ-অ্যাসেসমেন্ট প্রশ্নাবলী সংযুক্ত করে পেশাদার মানের ডকুমেন্টেশন তৈরি করা হয়েছে।

---

## 🗺️ রিপোজিটরি নেভিগেশন (Repository Navigation)

| প্রযুক্তি ট্র্যাক | বিবরণ (Description) | নোটস ডিরেক্টরি | প্রস্তুতকৃত লেসন সংখ্যা | লার্নিং স্ট্যাটাস |
|---|---|:---:|:---:|:---:|
| **Node.js** | সার্ভার-সাইড রানটাইম, V8 ইঞ্জিন, Streams, Event Loop এবং কোর মডিউল | [`/Node.js`](./Node.js/) | ৮টি লেসন | 🟢 Completed |
| **Express.js** | ফ্লেক্সিবল ওয়েব ফ্রেমওয়ার্ক, মিডলওয়্যার, MVC, Routing, Auth ও Uploads | [`/Express.js`](./Express.js/) | ১১টি লেসন | 🟢 Completed |
| **MongoDB** | NoSQL ডেটাবেস, BSON, Compass, Atlas কানেকশন ও Mongoose ODM | [`/MongoDB`](./MongoDB/) | ৩টি লেসন | 🟢 Completed |
| **JavaScript** | ভাষা ফান্ডামেন্টালস, Functions, Closures এবং Asynchronous JS | [`/JavaScript`](./JavaScript/) | ৩টি মডিউল | 🟢 Completed |
| **React** | ফ্রন্টএন্ড UI কম্পোনেন্টস, Hooks, State Management ও SPA | [`/React`](./React/) | ৩টি মডিউল | 🟡 In Progress |

> 📊 **সম্পূর্ণ শেখার অগ্রগতি ও বিস্তারিত বিবরণ দেখতে দেখুন: [LEARNING-TRACKER.md](./LEARNING-TRACKER.md)**

---

## 📁 ডিরেক্টরি স্ট্রাকচার (Repository Folder Structure)

```text
Notes/
│
├── README.md                                # রিপোজিটরি ওভারভিউ ও নেভিগেশন হাব
├── LEARNING-TRACKER.md                      # কেন্দ্রীয় লার্নিং ট্র্যাকার
│
├── Node.js/                                 # Node.js Core Architecture
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
├── Express.js/                              # Express.js Web Framework & APIs
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
├── MongoDB/                                 # MongoDB & Mongoose Database
│   ├── README.md
│   ├── 01-databases-sql-vs-nosql/
│   ├── 02-mongodb-fundamentals-crud/
│   └── 03-mongoose-odm/
│
├── JavaScript/                              # JavaScript Foundation
│   └── README.md
│
└── React/                                   # React Frontend
    └── README.md
```

---

## 🎯 নোটের বৈশিষ্ট্য ও লেখার মানদণ্ড (Documentation Standards)

1. **সহজ ও প্রাঞ্জল বাংলা:** কঠিন কনসেপ্টগুলোকে সহজ ভাষায় ব্যাখ্যা করা হয়েছে, তবে সমস্ত টেকনিক্যাল টার্ম (যেমন: Runtime, Event Loop, Non-blocking I/O, Streams, Middleware, ORM/ODM) ইংরেজিতে রাখা হয়েছে।
2. **আর্কিটেকচার ডায়াগ্রাম ও চার্ট:** প্রতিটি মডিউলে ডেটা প্রবাহ এবং ইন্টারনাল ওয়ার্কফ্লো চিত্রায়িত করতে ASCII এবং Mermaid ডায়াগ্রাম যুক্ত।
3. **প্র্যাকটিক্যাল কোড ও আউটপুট:** প্রতিটি লেসনের কোড সিনট্যাক্স-হাইলাইটেড এবং টার্মিনাল আউটপুটসহ সংকলিত।
4. **কমন ভুল ও প্রতিকার (Common Mistakes):** নতুনদের সম্ভাব্য ভুলগুলো চিহ্নিত করে আগাম সতর্কবার্তা যুক্ত।
5. **ভবিষ্যতের রিভিশন প্রস্তুতি:** প্রতিটি চ্যাপ্টারে কুইক সামারি এবং ইন্টারভিউ উপযোগী স্ব-মূল্যায়ন প্রশ্নাবলী সংযুক্ত।
