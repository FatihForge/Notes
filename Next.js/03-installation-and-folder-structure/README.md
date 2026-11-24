# ⚡ 03. ইনস্টলেশন ও ফোল্ডার আর্কিটেকচার (Installation & Folder Structure)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- `create-next-app` দিয়ে একটি আধুনিক Next.js 14 প্রজেক্ট সফলভাবে সেটআপ করা।
- **App Router (`app/`)** আর্কিটেকচার এবং ফাইল কনভেনশন বোঝা।
- `@/*` ইমপোর্ট পাথ অ্যালিয়াস কনফিগারেশন আয়ত্ত করা।
- প্রজেক্টের প্রতিটি ফাইল ও ফোল্ডারের সুনির্দিষ্ট দায়িত্ব অনুধাবন করা।

---

## 🛠️ ২. ইনস্টলেশন কমান্ড ও টার্মিনাল সেটআপ

একটি নতুন Next.js প্রজেক্ট শুরু করতে টার্মিনালে নিচের কমান্ডটি রান করি:

```bash
npx create-next-app@latest
```

### কনফিগারেশন অপশনসমূহ (Recommended Choices):
```text
✔ What is your project named? … my-next-app
✔ Would you like to use TypeScript? … Yes / No
✔ Would you like to use ESLint? … Yes
✔ Would you like to use Tailwind CSS? … Yes
✔ Would you like to use `src/` directory? … No (বা আপনার সুবিধা অনুযায়ী)
✔ Would you like to use App Router? (recommended) … Yes (বাধ্যতামূলক!)
✔ Would you like to customize the default import alias (@/*)? … Yes (ডিফল্ট @/* রাখা সেরা)
```

---

## 📁 ৩. Next.js App Router ফোল্ডার স্ট্রাকচার

```text
my-next-app/
│
├── app/                          # App Router-এর মূল প্রাণকেন্দ্র
│   ├── favicon.ico               # ব্রাউজার ট্যাব আইকন
│   ├── globals.css               # গ্লোবাল টেইলউইন্ড ও সিএসএস স্টাইল
│   ├── layout.js                 # রুট লেআউট (HTML ও Body ট্যাগ)
│   ├── page.js                   # হোমপেজ রুট (/)
│   ├── loading.js                # স্বয়ংক্রিয় লোডিং স্পিনার
│   ├── error.js                  # এরর বাউন্ডারি কম্পোনেন্ট
│   └── not-found.js              # কাস্টম ৪০৪ পেজ
│
├── public/                       # স্ট্যাটিক ফাইলস (Images, SVGs, Fonts)
│   ├── next.svg
│   └── vercel.svg
│
├── next.config.js                # Next.js ইঞ্জিন কনফিগারেশন (ইমেজ ডোমেইন ইত্যাদি)
├── tailwind.config.js            # Tailwind CSS কনফিগারেশন
├── package.json                  # স্ক্রিপ্টস ও ডিপেনডেন্সিজ
└── jsconfig.json (বা tsconfig)   # @/* পাথ অ্যালিয়াস ম্যাপিং
```

---

## 💡 ৪. App Router-এর স্পেশাল ফাইল কনভেনশনসমূহ

Next.js 14-এ ফোল্ডারের ভেতরের ফাইলের নামগুলো বিশেষ অর্থ বহন করে:

| ফাইলের নাম | কাজের ভূমিকা |
|---|---|
| `page.js` | যে ফোল্ডারে থাকবে, সেই ইউআরএলের মূল দৃশ্যমান UI বা রুট তৈরি করে। |
| `layout.js` | একাধিক পেজের শেয়ার্ড কাঠামো (Header, Footer, Nav) ধরে রাখে; স্টেট বজায় রাখে। |
| `loading.js` | পেজের ডেটা ফেচ চলাকালীন স্বয়ংক্রিয়ভাবে React Suspense লোডার দেখায়। |
| `error.js` | কোনো কম্পোনেন্টে এরর হলে ক্যাচ করে এবং সুন্দর ফলব্যাক ইউআই প্রদর্শন করে। |
| `not-found.js` | রিসোর্স বা রুট না পাওয়া গেলে কাস্টম ৪০৪ ভিউ দেখায়। |
| `route.js` | ব্যাকএন্ড API এন্ডপয়েন্ট তৈরি করতে ব্যবহৃত হয় (GET, POST ইত্যাদি)। |

---

## 🔗 ৫. `@/*` পাথ অ্যালিয়াসের সুবিধা

> [!TIP]
> কোডে বারবার `../../components/Button` লেখার অস্বস্তি দূর করে `@/` ব্যবহার করা যায়।  
> `@` সরাসরি প্রজেক্টের রুট বা `src` ফোল্ডারকে নির্দেশ করে:
> ```javascript
> // অস্বস্তিকর রিলেটিভ পাথ:
> import Button from '../../../components/Button';
> 
> // পরিচ্ছন্ন ও আধুনিক অ্যালিয়াস পাথ:
> import Button from '@/components/Button';
> ```

---

## ❓ ৬. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. App Router-এ কোনো ফোল্ডারের ভেতরে `page.js` না থাকলে কি সেই ফোল্ডারটি রুট হিসেবে কাজ করবে?  
২. `layout.js` এবং `page.js`-এর মধ্যে প্রধান আর্কিটেকচারাল পার্থক্য কী?  
৩. `next.config.js` ফাইলের মূল কাজ কী?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Project Structure Documentation](https://nextjs.org/docs/app/getting-started/project-structure)
