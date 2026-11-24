# ⚡ 09. ইমেজ ও ফন্ট অপটিমাইজেশন (Image & Font Optimization)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 Next.js ট্র্যাক সূচিপত্র](../README.md)

---

## 🎯 ১. এই পাঠ থেকে আমার লক্ষ্য (Learning Objectives)

- সাধারণ `<img>` ট্যাগের বদলে `next/image` ব্যবহারের সুনির্দিষ্ট সুবিধাগুলো জানা।
- **Cumulative Layout Shift (CLS)** কীভাবে রোধ করা যায় তা শেখা।
- রিমোট ইমেজের জন্য `next.config.js`-এ `remotePatterns` কনফিগার করা।
- `next/font/google` দিয়ে জিরো-লেআউট শিফটে ফন্ট অপটিমাইজেশন নিশ্চিত করা।

---

## 🖼️ ২. Image Optimization: `next/image`

সাধারণ HTML `<img>` ট্যাগ ব্যবহার করলে বড় সাইজের ইমেজ সম্পূর্ণ ব্যান্ডউইথ নষ্ট করে এবং পেজ লোড হওয়ার সময় ছবি নেমে এসে স্ক্রিনের লেখাগুলো হঠাৎ লাফালাফি করে (Layout Shift)।

### `next/image`-এর তিনটি মূল শক্তি:
1. **স্বয়ংক্রিয় WebP কনভার্শন:** আধুনিক ব্রাউজারে স্বয়ংক্রিয়ভাবে লাইটওয়েট WebP/AVIF ফরম্যাট সার্ভ করে।
2. **Prevent CLS:** ইমেজের অনুপাত আগে থেকেই সংরক্ষিত থাকায় পেজে কোনো লাফালাফি হয় না।
3. **Lazy Loading:** স্ক্রল করে ছবির কাছে না যাওয়া পর্যন্ত ব্যাকগ্রাউন্ডে ইমেজ ডাউনলোড স্থগিত থাকে।

---

## 💻 ৩. লোকাল বনাম রিমোট ইমেজ ব্যবহার

### ক. লোকাল ইমেজ (Local Image):
```jsx
import Image from 'next/image';
import heroBanner from '@/public/hero.jpg';

export default function Banner() {
  return (
    <Image
      src={heroBanner}
      alt="Hero Banner"
      placeholder="blur" // Next.js স্বয়ংক্রিয়ভাবে সুন্দর ব্লার লোডিং ইফেক্ট তৈরি করবে
      className="rounded-xl shadow"
    />
  );
}
```

### খ. রিমোট ইমেজ ও `next.config.js` কনফিগারেশন:
এক্সটার্নাল কোনো ডোমেইন থেকে ছবি লোড করতে হলে সুরক্ষার স্বার্থে `next.config.js`-এ ডোমেইন হোয়াইটলিস্ট করা বাধ্যতামূলক:

```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'images.unsplash.com',
      },
    ],
  },
};

module.exports = nextConfig;
```

```jsx
// রিমোট ইমেজে width এবং height দেওয়া বাধ্যতামূলক
<Image
  src="https://images.unsplash.com/photo-1506744038136-46273834b3fb"
  alt="Scenic View"
  width={800}
  height={500}
  className="rounded-lg"
/>
```

---

## 🔤 ৪. ফন্ট অপটিমাইজেশন: `next/font/google`

> [!TIP]
> `next/font` বিল্ড টাইমে স্বয়ংক্রিয়ভাবে গুগল ফন্ট নামিয়ে লোকাল অ্যাসেট হিসেবে প্রজেক্টে হোস্ট করে। এর ফলে ব্রাউজারকে গুগলের সার্ভারে আলাদা রিকোয়েস্ট পাঠাতে হয় না, প্রাইভেসি বজায় থাকে এবং ফন্ট লোডের সময় টেক্সট ফ্লিকার করে না!

```jsx
// app/layout.js
import { Poppins } from 'next/font/google';

const poppins = Poppins({
  subsets: ['latin'],
  weight: ['400', '600', '700'],
  variable: '--font-poppins',
});

export default function RootLayout({ children }) {
  return (
    <html lang="bn" className={poppins.variable}>
      <body className={poppins.className}>
        {children}
      </body>
    </html>
  );
}
```

---

## ❓ ৫. স্ব-মূল্যায়ন প্রশ্নাবলী (Self-Assessment)

১. রিমোট ইমেজের ক্ষেত্রে `width` এবং `height` নির্ধারণ করা কেন বাধ্যতামূলক?  
২. `placeholder="blur"` এবং `blurDataURL` কখন প্রয়োজন হয়?

---

## 🔗 তথ্যসূত্র ও প্রয়োজনীয় লিংক (References)
- [Next.js Image Component API](https://nextjs.org/docs/app/api-reference/components/image)
- [Font Optimization Guide](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)
