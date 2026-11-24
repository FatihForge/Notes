# ⚡ 02. রেন্ডারিং স্ট্র্যাটেজি: CSR vs SSR vs RSC (Rendering Strategies)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- **Client-Side Rendering (CSR)** এবং **Server-Side Rendering (SSR)**-এর নিখুঁত ডেটা ফ্লো বোঝা।
- **Hydration (হাইড্রেশন)** কী এবং এটি ব্রাউজারে কীভাবে কাজ করে তা জানা।
- **React Server Components (RSC)** কেন রিয়্যাক্ট ইকোসিস্টেমের সবচেয়ে বড় বিপ্লব তা ব্যাখ্যা করতে পারা।
- **FCP (First Contentful Paint)** এবং **TTI (Time to Interactive)** পারফরম্যান্স মেট্রিক্সের ওপর রেন্ডারিংয়ের প্রভাব বিশ্লেষণ করা।

---

## 📖 ২. তিনটি রেন্ডারিং পদ্ধতির সহজ মানসিক মডেল (Mental Model)

### ক. Client-Side Rendering (CSR)
> **আমার রূপক:** রেস্তোরাঁয় গিয়ে অর্ডার করার পর ওয়েটার যদি আপনাকে কাঁচা শাকসবজি, চাল আর চুলা টেবিলে এনে বলে—"ভাই, নিজেই রান্না করে খেয়ে নিন!"  
> CSR ঠিক তাই করে। সার্ভার শুধু একটি ফাঁকা HTML ফাইল এবং একটি বিশাল জাভাস্ক্রিপ্ট বান্ডেল পাঠায়। ব্যবহারকারীর ব্রাউজার সেই জাভাস্ক্রিপ্ট এক্সিকিউট করে স্ক্রিনে UI তৈরি করে।

```text
[Browser Request] ──> [Server Returns: Empty HTML Shell + Bundle.js]
                             │
                             ▼ (স্ক্রিন সম্পূর্ণ সাদা / ফাঁকা)
                     [Download Large JS Bundle]
                             │
                             ▼
                     [Parse & Execute JS in Browser]
                             │
                             ▼
                     [Fetch Data via Client API]
                             │
                             ▼
                     [Inject HTML into DOM ──> UI Visible]
```

### খ. Server-Side Rendering (SSR)
> **আমার রূপক:** রেস্তোরাঁ থেকে রান্না করা খাবার হটবক্সে ভরে আপনার টেবিলে দিয়ে গেল, কিন্তু চামচ দেয়নি। আপনি খাবার দেখতে পাচ্ছেন কিন্তু খেতে হলে ওয়েটার চামচ নিয়ে আসার জন্য সামান্য অপেক্ষা করতে হচ্ছে।  
> SSR-এ সার্ভার নিজেই কোড এক্সিকিউট করে সম্পূর্ণ প্রস্তুতকৃত HTML ব্রাউজারে পাঠায়। ব্যবহারকারী সাথে সাথেই কন্টেন্ট দেখতে পায় (Fast FCP), এরপর ব্যাকগ্রাউন্ডে ছোট জাভাস্ক্রিপ্ট এসে ইভেন্ট লিসেনারগুলো যুক্ত করে পেজটিকে ইন্টারঅ্যাক্টিভ করে (Hydration)।

```text
[Browser Request] ──> [Server Executes Code & Pre-renders HTML]
                             │
                             ▼
                    [Returns Hydrated HTML + CSS (Instant Content Display)]
                             │
                             ▼
                    [Downloads Small Client JS Bundle]
                             │
                             ▼
                    [Hydration: Attaching Event Listeners ──> Fully Interactive]
```

---

## 🚀 ৩. রিয়্যাক্ট সার্ভার কম্পোনেন্ট (React Server Components - RSC)

> [!IMPORTANT]
> **RSC কী এবং কেন এটি যুগান্তকারী?**  
> Next.js 14-এর App Router-এ প্রতিটি কম্পোনেন্ট **বাই-ডিফল্ট একটি Server Component**।  
> এটি সার্ভারে রান হয় এবং কখনোই ব্রাউজারে জাভাস্ক্রিপ্ট বান্ডেল হিসেবে যায় না! ব্রাউজার শুধুমাত্র পিওর HTML এবং JSON স্ট্রিম পায়।

### RSC-এর জাদুকরী দুটি সুবিধা:
1. **Zero Client Bundle Size:** আপনি যদি সার্ভার কম্পোনেন্টে `lodash`, `moment.js` বা `prisma` ব্যবহার করেন, সেগুলোর এক বাইট কোডও ব্যবহারকারীর ব্রাউজারে ডাউনলোড হবে না!
2. **সরাসরি ব্যাকএন্ড অ্যাক্সেস:** কোনো ক্লায়েন্ট এপিআই (useEffect/Axios) ছাড়াই সরাসরি ডেটাবেজ (Prisma/Mongoose) বা ফাইল সিস্টেমে কোয়েরি চালানো যায়।

```jsx
// app/users/page.js - বাই-ডিফল্ট একটি React Server Component (RSC)
import db from '@/lib/db'; // সরাসরি সার্ভার-সাইড ডেটাবেস

export default async function UsersPage() {
  // কোনো useEffect বা লোডিং স্টেট ছাড়াই সরাসরি ডেটাবেস কোয়েরি
  const users = await db.user.findMany();

  return (
    <main className="p-6">
      <h1 className="text-2xl font-bold">ইউজার তালিকা (Server Rendered)</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </main>
  );
}
```

---

## 📊 ৪. CSR বনাম SSR বনাম RSC তুলনামূলক চার্ট

| বৈশিষ্ট্য | CSR (Client-Side) | SSR (Server-Side) | RSC (Server Components) |
|---|---|---|---|
| **কোড কোথায় রান হয়?** | সম্পূর্ণ ব্রাউজারে | সার্ভারে, প্রতি রিকোয়েস্টে | শুধুমাত্র সার্ভারে |
| **Client Bundle Size** | বিশাল (সব কোড ক্লায়েন্টে যায়) | মাঝারি | **Zero Bundle Size** |
| **SEO ফ্রেন্ডলিনেস** | অত্যন্ত দুর্বল | চমৎকার | অসাধারণ |
| **ইনিশিয়াল লোডিং স্পিড** | ধীরগতির (Blank Screen) | তাৎক্ষণিক দৃশ্যমান | অত্যন্ত দ্রুতগতির স্ট্রিমড HTML |
| **ব্রাউজার ইন্টারঅ্যাক্টিভিটি** | হ্যাঁ (`useState`, `onClick`) | হ্যাঁ (Hydration-এর পর) | সরাসরি নয় (Client Component প্রয়োজন) |

---

## 💧 ৫. হাইড্রেশন (Hydration) কী?

> [!NOTE]
> সার্ভার থেকে পাঠানো রেডিমেড স্ট্যাটিক HTML-এর ওপর ক্লায়েন্ট-সাইড জাভাস্ক্রিপ্টের ইভেন্ট হ্যান্ডলারগুলো (`onClick`, `onChange`) জোড়া লাগানো বা সক্রিয় করার প্রক্রিয়াকেই বলা হয় **Hydration**।

---

## ❓ ৬. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. CSR অ্যাপ্লিকেশনে First Contentful Paint (FCP) কেন ধীরগতির হয়?  
২. "Hydration mismatch error" বলতে কী বোঝায় এবং এটি কেন ঘটে?  
৩. RSC কীভাবে ক্লায়েন্ট বান্ডেল সাইজ শূন্যে নামিয়ে আনে?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [React Essentials - Server Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components)
- [Next.js Rendering Overview](https://nextjs.org/docs/app/building-your-application/rendering)
