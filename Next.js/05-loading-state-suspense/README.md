# ⚡ 05. লোডিং স্টেট ও ইউআই (Loading State with loading.js)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- `loading.js` ফাইল কনভেনশনের মেকানিজম জানা।
- React-এর আন্ডারলাইং **Suspense** প্রযুক্তির সাথে এর সম্পর্ক বোঝা।
- কোনো ক্লায়েন্ট-সাইড `isLoading` স্টেট ছাড়াই কীভাবে স্বয়ংক্রিয় লোডিং ইউআই তৈরি করা যায় তা শেখা।
- ব্যবহারকারীর জন্য মসৃণ ট্রানজিশন নিশ্চিত করা।

---

## 📖 ২. সহজ পরিচয়: `loading.js` কী?

> [!NOTE]
> প্রথাগত React-এ ডেটা ফেচিংয়ের সময় আমাদের ম্যানুয়ালি `const [loading, setLoading] = useState(true)` লিখতে হতো।  
> কিন্তু Next.js 14-এ যে ফোল্ডারে পেজের ডেটা ফেচ হতে সময় লাগবে, সেই ফোল্ডারে শুধু একটি `loading.js` ফাইল রেখে দিলেই Next.js ব্যাকগ্রাউন্ডে পুরো পেজটিকে একটি **React Suspense Boundary**-র ভেতরে মুড়িয়ে দেয়!

```text
  ┌─────────────────────────────────────────────────────────────┐
  │                 <Suspense fallback={<Loading />}>           │
  │                     <PageContent />                         │
  │                 </Suspense>                                 │
  └─────────────────────────────────────────────────────────────┘
```

---

## 💻 ৩. প্র্যাকটিক্যাল কোড উদাহরণ

### `app/blogs/loading.js`
```jsx
// app/blogs/loading.js
export default function Loading() {
  return (
    <div className="flex items-center justify-center min-h-[50vh] p-12">
      <div className="animate-spin rounded-full h-10 w-10 border-b-2 border-blue-600"></div>
      <span className="ml-4 font-semibold text-gray-700">কন্টেন্ট লোড হচ্ছে, অনুগ্রহ করে অপেক্ষা করুন...</span>
    </div>
  );
}
```

### স্কেলেটন লোডারের উদাহরণ (Skeleton UI):
```jsx
// app/posts/loading.js
export default function PostsLoading() {
  return (
    <div className="space-y-4 p-6 animate-pulse">
      <div className="h-8 bg-gray-200 rounded w-1/3"></div>
      <div className="h-4 bg-gray-200 rounded w-full"></div>
      <div className="h-4 bg-gray-200 rounded w-5/6"></div>
      <div className="h-4 bg-gray-200 rounded w-4/6"></div>
    </div>
  );
}
```

---

## 💡 ৪. আর্কিটেকচারাল সুবিধা
1. **Instant Loading States:** সার্ভার থেকে ডেটা আসার আগেই ব্যবহারকারী দেখতে পায় যে রিকোয়েস্ট কাজ করছে।
2. **Interruptible Navigation:** লোডিং চলার সময়েও ইউজার চাইলে অন্য লিংকে ক্লিক করে চলে যেতে পারে, ব্রাউজার আটকে থাকে না।

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. `loading.js` কি ক্লায়েন্ট কম্পোনেন্ট নাকি সার্ভার কম্পোনেন্ট?  
২. নেস্টেড রাউটে চাইল্ড সেগমেন্টের নিজস্ব `loading.js` থাকলে প্যারেন্ট লেআউট কি লোডিং অবস্থায় চলে যায়?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Loading UI and Streaming](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)
