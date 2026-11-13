# 🟢 Node.js Core Architecture & Backend Engineering

Node.js হলো একটি ওপেন-সোর্স, ক্রস-প্ল্যাটফর্ম **JavaScript Runtime Environment**, যা ব্রাউজারের বাইরে সার্ভার-সাইডে জাভাস্ক্রিপ্ট কোড এক্সিকিউট করতে ব্যবহৃত হয়। এটি Google-এর V8 ইঞ্জিন এবং C-ভিত্তিক libuv লাইব্রেরির ওপর দাঁড়িয়ে অত্যন্ত দ্রুতগতির Non-blocking Asynchronous I/O পরিচালনা করে।

---

## 🗺️ মডিউল রোডম্যাপ ও লেসন তালিকা (Lessons Navigation)

| লেসন ডিরেক্টরি | বিষয়বস্তু | মূল শিক্ষণীয় বিষয় | স্ট্যাটাস |
|---|---|---|:---:|
| **[01-introduction](./01-introduction/)** | **Node.js পরিচিতি ও কোর আর্কিটেকচার** | V8 Engine, Non-blocking I/O, ব্রাউজার বনাম Node.js গ্লোবাল অবজেক্ট | 📘 Notes Prepared |
| **[02-installation-and-repl](./02-installation-and-repl/)** | **ইনস্টলেশন, IDE ধারণা, REPL ও প্রথম স্ক্রিপ্ট** | Multi-OS Setup (Mac/Win/Linux), NVM, VS Code টিউনিং, REPL ও `fs.writeFile` | 📘 Notes Prepared |
| **[03-first-server-and-core-modules](./03-first-server-and-core-modules/)** | **প্রথম নোড সার্ভার, DNS ও কোর মডিউল** | DNS রেজোলিউশন ফ্লো, TCP/HTTP প্রটোকল, `http.createServer`, Core Modules | 📘 Notes Prepared |
| **[04-request-response-routing](./04-request-response-routing/)** | **রিকোয়েস্ট-রেসপন্স সাইকেল, রাউটিং ও রিডাইরেকশন** | Event Queue & Thread Pool, `req.url`, `req.method`, Form POST ও 302 Redirect | 📘 Notes Prepared |
| **[05-streams-buffers-parsing](./05-streams-buffers-parsing/)** | **Streams, Chunks, Buffers ও Request Parsing** | Streams বনাম Buffers, `req.on('data')`, `req.on('end')`, URLSearchParams | 📘 Notes Prepared |
| **[06-event-loop-architecture](./06-event-loop-architecture/)** | **Event Loop ডিপ ডাইভ ও libuv আর্কিটেকচার** | Event Loop-এর ৬টি পর্যায়, Worker Pool, Microtask Queue (`process.nextTick`) | 📘 Notes Prepared |
| **[07-npm-and-tools](./07-npm-and-tools/)** | **NPM, Package Management ও Development Tools** | `npm init`, dependencies vs devDependencies, package.json scripts, Nodemon | 📘 Notes Prepared |
| **[08-debugging-guide](./08-debugging-guide/)** | **Node.js ডিবাগিং গাইড ও VS Code ডিবাগার** | Syntax/Runtime/Logical Errors, VS Code `launch.json`, Breakpoints ও Variable Inspection | 📘 Notes Prepared |

---

## 🧭 দ্রুত নেভিগেশন
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../README.md)
- [📊 লার্নিং ট্র্যাকার](../LEARNING-TRACKER.md)
- [🚀 পরবর্তী ট্র্যাক: Express.js](../Express.js/README.md)
