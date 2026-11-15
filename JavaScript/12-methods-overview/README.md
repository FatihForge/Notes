# ⚡ 12. JavaScript Methods Overview Reference Sheet (মেথডস কুইক রেফারেন্স)

---

## 🧭 দ্রুত নেভিগেশন (Quick Navigation)
- [⬅️ মূল রিপোজিটরি সূচিপত্র](../../README.md)
- [📊 লার্নিং ট্র্যাকার](../../LEARNING-TRACKER.md)
- [📁 JavaScript ট্র্যাক সূচিপত্র](../README.md)

---

তুমি যদি JavaScript শিখে থাকো, তাহলে সব method একসাথে
                মুখস্থ করার দরকার নেই। একজন JavaScript Developer হিসেবে
                প্রতিদিন যেসব methods বেশি কাজে আসে, সেগুলো আগে ভালোভাবে
                বোঝাই বেশি গুরুত্বপূর্ণ।

এই reference-এ String, Array, Object, DOM, Math, Number,
                Date, JSON, Fetch এবং Local Storage-এর commonly used
                methods সাজানো হয়েছে।

01 / STRING

## String Methods

String-এর text নিয়ে কাজ করার সময় এই methodsগুলো সবচেয়ে বেশি
                কাজে আসে।

| Method | কাজ | সবচেয়ে বেশি কোথায় |
| --- | --- | --- |
| includes() | কোনো শব্দ আছে কিনা check করে | Search Box |
| toLowerCase() | সব character ছোট হাতের করে | Login, Search |
| toUpperCase() | সব character বড় হাতের করে | Formatting |
| trim() | সামনে ও পিছনের extra space কেটে দেয় | Form Validation |
| slice() | String-এর নির্দিষ্ট অংশ কেটে নেয় | Text |
| replace() | একটি শব্দ বা অংশ পরিবর্তন করে | Editing |
| split() | String কে Array-তে পরিণত করে | CSV, Tags |
| startsWith() | কোনো text দিয়ে শুরু হয়েছে কিনা check করে | URL, Validation |
| endsWith() | কোনো text দিয়ে শেষ হয়েছে কিনা check করে | File Extension |

                  `
                    includes()
                  `

কোনো নির্দিষ্ট শব্দ আছে কিনা check করতে ব্যবহার হয়।

                  Search / Validation

                    string-example.js

Copy

```javascript
let text = "I Love JavaScript";

console.log(text.includes("Love"));
```

                Output:
                `true`

              `
                trim()
              `

String-এর শুরু ও শেষে থাকা extra spaces সরাতে ব্যবহৃত হয়।
                Form-এর ক্ষেত্রে এটি useful।

                    form-validation.js

Copy

```javascript
let name = "   Ashik   ";

console.log(name.trim());
```

                Output:
                `Ashik`

              `
                toLowerCase()
              `

সব character ছোট হাতের অক্ষরে নিয়ে আসে।

                    normalization.js

Copy

```javascript
let email = "ASHIK@gmail.com";

console.log(email.toLowerCase());
```

                Output:
                `
                  ashik@gmail.com
                `

02 / ARRAY

## Array Methods

Array methods JavaScript-এর সবচেয়ে গুরুত্বপূর্ণ অংশগুলোর
                একটি। বিশেষ করে data filtering, rendering এবং list processing-এ
                এগুলো নিয়মিত ব্যবহার হয়।

| Method | কাজ | সবচেয়ে বেশি কোথায় |
| --- | --- | --- |
| push() | Array-এর শেষে item যোগ করে | Todo App |
| pop() | শেষ item বাদ দেয় | Stack |
| shift() | প্রথম item বাদ দেয় | Queue |
| unshift() | Array-এর শুরুতে item যোগ করে | Notifications |
| map() | নতুন Array তৈরি করে | React |
| filter() | শর্ত অনুযায়ী item বেছে নেয় | Search, Product Filter |
| find() | একটি item খুঁজে বের করে | Products |
| includes() | কোনো item আছে কিনা check করে | Favorites |
| forEach() | Array-এর item নিয়ে loop করে | Data Display |
| sort() | Data সাজায় | Product List |
| reverse() | Array উল্টো করে | Chat |
| join() | Array থেকে String তৈরি করে | Tags |

                  `
                    map()
                  `

Array-এর প্রতিটি item নিয়ে কাজ করে একটি নতুন Array তৈরি করে।

                  React / Data Transformation

                    array-map.js

Copy

```javascript
let numbers = [1, 2, 3];

let result = numbers.map(num => num * 2);

console.log(result);
```

                Output:
                `
                  [2, 4, 6]
                `

              `
                filter()
              `

নির্দিষ্ট condition অনুযায়ী Array থেকে item বেছে নেয়।

                    array-filter.js

Copy

```javascript
let numbers = [1, 2, 3, 4, 5];

let even = numbers.filter(num => num % 2 === 0);

console.log(even);
```

                Output:
                `
                  [2, 4]
                `

Search, Product Filter এবং Category Filter-এর মতো জায়গায়
                  `filter()` খুব কাজে আসে।

              `
                find()
              `

Array-এর মধ্যে একটি নির্দিষ্ট item খুঁজতে ব্যবহৃত হয়।

                    array-find.js

Copy

```javascript
let users = [
  { id: 1 },
  { id: 2 },
  { id: 3 }
];

let user = users.find(item => item.id === 2);

console.log(user);
```

              03 / OBJECT

## Object Methods

                Object-এর key এবং value নিয়ে কাজ করতে এই methodsগুলো গুরুত্বপূর্ণ।

| Method | কাজ | সবচেয়ে বেশি কোথায় |
| --- | --- | --- |
| Object.keys() | Object-এর key বের করে | Loop |
| Object.values() | Object-এর value বের করে | Dashboard |
| Object.entries() | Key-value pair বের করে | API |
| hasOwnProperty() | কোনো property আছে কিনা check করে | Validation |

                  object-keys.js

Copy

```javascript
let person = {
  name: "Ashik",
  age: 20
};

console.log(Object.keys(person));
```

              Output:
              `
                ["name", "age"]
              `

04 / DOM

## DOM Methods

Browser-এর page element select, create, modify এবং event handle
                করার সময় এগুলো নিয়মিত ব্যবহার হয়।

                `
                  getElementById()
                `

id দিয়ে একটি element খুঁজে আনে।

                `
                  querySelector()
                `

CSS selector ব্যবহার করে element select করে।

                `
                  querySelectorAll()
                `

একাধিক element select করে।

                `
                  addEventListener()
                `

Event handle করতে ব্যবহার হয়।

                `
                  createElement()
                `

নতুন element তৈরি করে।

                `
                  appendChild()
                `

element-এর সাথে child যোগ করে।

                `
                  remove()
                `

element মুছে দেয়।

                `
                  classList.add()
                `

element-এ class যোগ করে।

                `
                  classList.remove()
                `

class বাদ দেয়।

                `
                  classList.toggle()
                `

class add/remove toggle করে।

                    event-listener.js

Copy

```javascript
button.addEventListener("click", function(){

});
```

                `addEventListener()`
                DOM-এর সাথে JavaScript interaction করার সময় খুব বেশি ব্যবহৃত হয়।

05 / MATH

## Math Methods

Number generate, rounding এবং minimum/maximum value বের করার
                মতো কাজে Math methods ব্যবহার হয়।

| Method | কাজ |
| --- | --- |
| Math.random() | Random Number তৈরি করে |
| Math.floor() | নিচের পূর্ণসংখ্যায় নামিয়ে আনে |
| Math.ceil() | উপরের পূর্ণসংখ্যায় তোলে |
| Math.round() | Round করে |
| Math.max() | বড় সংখ্যা বের করে |
| Math.min() | ছোট সংখ্যা বের করে |

                  random-number.js

Copy

```javascript
let random = Math.floor(Math.random() * 10) + 1;
```

                Number Guessing Game-এর মতো project-এ এই pattern ব্যবহার করা হয়।

06 / NUMBER

## Number Methods

| Method | কাজ |
| --- | --- |
| parseInt() | Integer বানায় |
| parseFloat() | Decimal Number বানায় |
| toFixed() | Decimal-এর limit নির্ধারণ করে |

                  decimal-format.js

Copy

```javascript
let price = 19.456;

console.log(price.toFixed(2));
```

              Output:
              `19.46`

07 / DATE

## Date Methods

                `
                  new Date()
                `

বর্তমান সময়ের Date তৈরি করে।

                `
                  getFullYear()
                `

Year বের করে।

                `
                  getMonth()
                `

Month বের করে।

                `
                  getDate()
                `

Date বের করে।

                `
                  getHours()
                `

Hour বের করে।

08 / JSON

## JSON Methods

API এবং data handling-এর সময় এই দুইটি method খুব গুরুত্বপূর্ণ।

                `
                  JSON.stringify()
                `

                    Object → JSON String

                `
                  JSON.parse()
                `

                    JSON String → Object

09 / FETCH

## Fetch / Promise Methods

Fetch ব্যবহার করার সময় Promise handle এবং response data নিয়ে
                কাজ করতে এই methodsগুলো ব্যবহার হয়।

| Method | কাজ |
| --- | --- |
| then() | Promise handle করে |
| catch() | Error handle করে |
| finally() | শেষে run করে |
| response.json() | JSON data নিয়ে কাজ করতে ব্যবহার হয় |

10 / LOCAL STORAGE

## Local Storage Methods

Todo App, Theme এবং Login State-এর মতো data browser-এ
                সংরক্ষণ করার সময় এই methodsগুলো ব্যবহৃত হয়।

                `
                  setItem()
                `

Data save করে।

                `
                  getItem()
                `

Data read করে।

                `
                  removeItem()
                `

নির্দিষ্ট Data delete করে।

                `
                  clear()
                `

সব Data delete করে।

QUICK REFERENCE

## একজন Junior Developer-এর Top 20

সব method একসাথে মুখস্থ না করে এই methodsগুলো আগে ভালোভাবে
                বুঝে practice করা একটি practical starting point।

                01
                `includes()`

                02
                `trim()`

                03
                `toLowerCase()`

                04
                `slice()`

                05
                `replace()`

                06
                `split()`

                07
                `push()`

                08
                `pop()`

                09
                `map()`

                10
                `filter()`

                11
                `find()`

                12
                `forEach()`

                13
                `sort()`

                14
                `Object.keys()`

                15
                `querySelector()`

                16
                `addEventListener()`

                17
                `createElement()`

                18
                `Math.random()`

                19
                `parseInt()`

                20
                `JSON.parse()`
                /
                `JSON.stringify()`

LEARNING PATH

## কোন order-এ শিখবে?

JavaScript logic practice করার জন্য methodsগুলো এই sequence-এ
                শেখা একটি practical approach।

                  01

### String Methods

Text নিয়ে কাজ করার foundation তৈরি করবে।

                  02

### Array Methods

Data process এবং list logic-এর জন্য সবচেয়ে গুরুত্বপূর্ণ অংশ।

                  03

### Object Methods

Object-এর key/value নিয়ে কাজ করতে শিখবে।

                  04

### Math Methods

Random number এবং basic number operations-এর জন্য।

                  05

### DOM Methods

Browser interaction এবং project building-এর জন্য।

                  06

### Date & JSON

Data এবং time-related কাজের foundation তৈরি করবে।

                  07

### Local Storage

Browser-এ data save এবং manage করার জন্য।

PRACTICE

## Methods শেখার সবচেয়ে ভালো জায়গা: Project

Methods শুধু মুখস্থ না করে Counter, Todo App, Calculator,
              Number Guessing Game-এর মতো ছোট project-এ ব্যবহার করলে
              JavaScript logic build করা সহজ হয়।

SUMMARY

## Final Takeaway

সব JavaScript method একসাথে শেখার চেষ্টা না করে আগে
                **String → Array → Object → DOM → Math → Date → JSON → Local Storage**
                এই sequence-এ practice করো।

বিশেষ করে `map()`, `filter()`,
                `find()`, `forEach()`,
                `querySelector()`,
                `addEventListener()` এবং
                `JSON.parse()` /
                `JSON.stringify()`-এর মতো methods ভালোভাবে
                বুঝে ফেললে অনেক ছোট JavaScript project সহজ হয়ে যাবে।

              ←
              Back to posts

---

## 🔗 তথ্যসূত্র ও গুরুত্বপূর্ণ লিংক (References)
- [MDN Web Docs - JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info — The Modern JavaScript Tutorial](https://javascript.info/)
- [FatihForge Blog - Original Article](https://fatihforge.pages.dev/blog/javascript-method-overview)

---
**পরবর্তী লেসন পড়ার জন্য আপনার ট্র্যাক সূচিপত্রে ফিরে যান:** [⬅️ Back to JavaScript Index](../README.md)
