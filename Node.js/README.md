# 🟢 Node.js Study Path & Documentation

Node.js হলো একটি ওপেন-সোর্স, ক্রস-প্ল্যাটফর্ম **JavaScript Runtime Environment**, যা ব্রাউজারের বাইরে সার্ভার-সাইডে জাভাস্ক্রিপ্ট কোড এক্সিকিউট করতে সাহায্য করে। MERN স্ট্যাকের ব্যাকএন্ড আর্কিটেকচারের মেরুদণ্ড হলো এই Node.js।

---

## 🗺️ মডিউল রোডম্যাপ ও লেসন তালিকা (Syllabus & Navigation)

| লেসন ডিরেক্টরি | বিষয়বস্তু | মূল শিক্ষণীয় বিষয় | স্ট্যাটাস |
|---|---|---|:---:|
| **[01-introduction](./01-introduction/)** | **Node.js পরিচিতি ও কার্যপ্রণালী** | Node.js কী, কেন উদ্ভাবন, V8 ইঞ্জিন, Non-blocking I/O এবং বেসিক আর্কিটেকচার | 📘 Notes Prepared |
| **[02-installation-and-repl](./02-installation-and-repl/)** | **ইনস্টলেশন, IDE ধারণা ও REPL** | Node & NPM ইনস্টল, Node REPL ব্যবহার, VS Code সেটআপ ও ফার্স্ট `.js` ফাইল রান | ⚪ Not Started |
| **[03-runtime-architecture](./03-runtime-architecture/)** | **রানটাইম আর্কিটেকচার ও অভ্যন্তরীণ গঠন** | V8 ইঞ্জিন, libuv লাইব্রেরি, C++ বাইন্ডিং এবং ইন্টারনাল ওয়ার্কফ্লো | ⚪ Not Started |
| **[04-event-loop](./04-event-loop/)** | **ইভেন্ট লুপ (Event Loop)** | Single Threaded প্রকৃতি, ফেজেস অফ ইভেন্ট লুপ, মাইক্রোটাস্ক ও ম্যাক্রোটাস্ক কিউ | ⚪ Not Started |
| **[05-module-system](./05-module-system/)** | **মডিউল সিস্টেম ও প্যাকেজ ম্যানেজার** | CommonJS (`require`) বনাম ES Modules (`import`), `package.json` এবং NPM | ⚪ Not Started |
| **[06-file-system-streams](./06-file-system-streams/)** | **ফাইল সিস্টেম ও স্ট্রিমস** | `fs` মডিউল (Sync vs Async), Buffer, Streams ও ডেটা পাইপিং | ⚪ Not Started |
| **[07-http-server](./07-http-server/)** | **কোর HTTP সার্ভার** | কোনো এক্সটার্নাল লাইব্রেরি ছাড়া `http` মডিউল দিয়ে নেটিভ ওয়েব সার্ভার তৈরি | ⚪ Not Started |

---

## 🎯 পূর্বশর্ত (Prerequisites)

Node.js ভালোভাবে আয়ত্ত করতে নিচের বিষয়গুলোর বেসিক ধারণা থাকা আবশ্যক:
1. **JavaScript Fundamentals:** ভ্যারিয়েবল, ডেটা টাইপস, ফাংশনস, অবজেক্ট এবং অ্যারে।
2. **Asynchronous JS:** Callbacks, Promises, এবং `async/await` কীভাবে কাজ করে।
3. **Basic Terminal/CLI:** কমান্ড লাইনের সাধারণ কমান্ড যেমন `cd`, `ls` / `dir`, `mkdir` ইত্যাদি চালানো।

---

## 🚀 শুরু করার নির্দেশিকা
প্রথমে **[01-introduction/README.md](./01-introduction/README.md)** থেকে পড়াশোনা শুরু করুন। প্রতিটি লেসনের কোড নিজ হাতে চালিয়ে টার্মিনাল আউটপুট পর্যবেক্ষণ করার পরামর্শ দেওয়া হচ্ছে।
