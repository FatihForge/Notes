# ⚡ 11. সার্ভার কম্পোনেন্টে ডেটা ফেচিং (Data Fetching in RSC)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- Server Components-এ সরাসরি `async/await` দিয়ে ডেটা ফেচ করার আধুনিক নিয়ম শেখা।
- **Waterfall Requests** বনাম **Parallel Data Fetching (`Promise.all`)**-এর পার্থক্য জানা।
- Next.js-এর এক্সটেন্ডেড `fetch` মেকানিজম এবং **Automatic Request Deduplication** বোঝা।
- কেন ক্লায়েন্ট সাইড Axios বা `useEffect`-এর চেয়ে এটি হাজার গুণ ভালো তা জানা।

---

## 🚀 ২. সরাসরি `async/await` দিয়ে ফেচিং (No useEffect Needed!)

```jsx
// app/posts/page.js
async function getPosts() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5');
  if (!res.ok) throw new Error('ডেটা লোড করতে ব্যর্থ হয়েছে');
  return res.json();
}

export default async function PostsPage() {
  const posts = await getPosts(); // সরাসরি সার্ভারে await!

  return (
    <div className="p-6 space-y-4">
      <h1 className="text-2xl font-bold">পোস্ট তালিকা</h1>
      <ul className="space-y-2">
        {posts.map((post) => (
          <li key={post.id} className="p-3 bg-white border rounded shadow-sm">
            {post.title}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## ⚡ ৩. প্যারালাল ফেচিং (Parallel Fetching with `Promise.all`)

> [!WARNING]
> **ওয়াটারফল সমস্যা (Waterfall):**  
> যদি আপনি লেখেন `const user = await getUser();` এবং তার নিচে `const posts = await getPosts();`—তবে ইউজার ফেচ না হওয়া পর্যন্ত পোস্ট ফেচিং শুরুই হবে না। এতে পেজের মোট লোডিং সময় দ্বিগুণ হয়ে যায়!

### সমাধান: সমান্তরালে প্রমিজ চালানো:
```jsx
// app/dashboard/page.js
export default async function Dashboard() {
  // সমান্তরালে ফেচিং শুরু করা হলো
  const userPromise = getUser();
  const postsPromise = getPosts();

  // উভয় রিকোয়েস্ট একই সময়ে সার্ভার প্রসেস করবে
  const [user, posts] = await Promise.all([userPromise, postsPromise]);

  return (
    <main>
      <h1>স্বাগতম, {user.name}</h1>
      {/* পোস্ট তালিকা */}
    </main>
  );
}
```

---

## 💡 ৪. Request Deduplication মেকানিজম

```text
  Component A (generateMetadata) ──> fetch('/api/user/1') ──> [Actual Network Call]
                                                                     │
                                                       (Cached in Server Memory)
                                                                     │
  Component B (ProfilePage)      ──> fetch('/api/user/1') ──> [Instant Memoized Return]
```

Next.js একই রেন্ডার সাইকেলে অভিন্ন `fetch` রিকোয়েস্ট থাকলে দ্বিতীয়বার নেটওয়ার্কে কল পাঠায় না; মেমোরি থেকে তাৎক্ষণিক ডেটা রিটার্ন করে।

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. ওয়াটারফল ফেচিং কখন উপকারী হতে পারে? (যখন দ্বিতীয় রিকোয়েস্টের প্যারামিটার প্রথম রিকোয়েস্টের ডেটার ওপর নির্ভরশীল)।  
২. `fetch` মেথডে এরর হলে সেটি কোন ফাইলে ক্যাচ হবে? (`error.js`)

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Data Fetching and Caching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating)
