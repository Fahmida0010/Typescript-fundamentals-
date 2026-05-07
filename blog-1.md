
### Why `any` is Called a "Type Safety Hole" and Why `unknown` is the Safer Choice in TypeScript

TypeScript-এ ডেটা টাইপ নিয়ে কাজ করার সময় `any` এবং `unknown` দুটি খুবই পরিচিত টাইপ। কিন্তু এদের মধ্যে পার্থক্য বোঝা অত্যন্ত গুরুত্বপূর্ণ, বিশেষ করে যখন আপনি unpredictable বা external data (যেমন API response, user input, JSON.parse() ইত্যাদি) নিয়ে কাজ করেন।

#### `any` কেন "Type Safety Hole"?

`any` হলো TypeScript-এর সবচেয়ে flexible টাইপ। এটি ব্যবহার করলে কম্পাইলার কোনো টাইপ চেকিং করে না। অর্থাৎ:

```ts
let data: any = JSON.parse(someString);

data.name.toUpperCase();        // কোনো এরর দেখাবে না
data.callSomeRandomMethod();    // এটাও চলবে
data * 100;                     // সবকিছুই allowed
```

**সমস্যাগুলো:**

- TypeScript-এর মূল উদ্দেশ্য ছিল **type safety** প্রদান করা — বাগ ধরা কম্পাইল টাইমে।
- `any` ব্যবহার করলে সেই সুরক্ষা পুরোপুরি চলে যায়। রানটাইমে এরর আসতে পারে যা কম্পাইলার ধরতে পারবে না।
- একবার `any` ব্যবহার করলে এটি প্রজেক্টের অন্যান্য অংশে ছড়িয়ে পড়তে পারে (type pollution)।
- অনেক ডেভেলপার এটাকে "escape hatch" হিসেবে ব্যবহার করেন, কিন্তু অতিরিক্ত ব্যবহারে TypeScript-এর সুবিধাই নষ্ট হয়ে যায়।

এজন্যই `any` কে **type safety hole** বলা হয়। এটি টাইপ সিস্টেমকে ফাঁকি দেয়।

#### `unknown` কেন নিরাপদ?

`unknown` হলো `any`-এর safer ভার্সন। এটি বলে: “আমি জানি না এই ভ্যালুর টাইপ কী, তাই আমাকে কিছু করার আগে প্রমাণ করো যে তুমি এটাকে সেফলি ব্যবহার করতে পারবে।”

**মূল পার্থক্য:**

- `unknown` এর উপর সরাসরি কোনো অপারেশন করা যায় না।
- আপনাকে অবশ্যই টাইপ চেক করে নিতে হবে।

```ts
let data: unknown = JSON.parse(someString);

// data.toUpperCase();     // ❌ Error: Object is of type 'unknown'

if (typeof data === "string") {
    console.log(data.toUpperCase());  // এখন সেফ
}
```

#### Type Narrowing কী?

**Type Narrowing** হলো TypeScript-এর একটি শক্তিশালী ফিচার যার মাধ্যমে আপনি একটি ব্রড টাইপকে (যেমন `unknown`) আরও নির্দিষ্ট টাইপে (string, number, object ইত্যাদি) রূপান্তর করেন।

**সাধারণ Narrowing টেকনিকস:**

1. **typeof চেক**
   ```ts
   if (typeof data === "string") { ... }
   if (typeof data === "number") { ... }
   ```

2. **instanceof চেক**
   ```ts
   if (data instanceof Date) { ... }
   ```

3. **Type Guard ফাংশন**
   ```ts
   function isUser(obj: unknown): obj is { name: string; age: number } {
       return typeof obj === "object" 
           && obj !== null 
           && "name" in obj 
           && "age" in obj;
   }

   if (isUser(data)) {
       console.log(data.name);  // এখন টাইপসেফ
   }
   ```

4. **Type Assertion** (শেষ অপশন হিসেবে)
   ```ts
   const user = data as { name: string };
   ```

#### কখন কোনটা ব্যবহার করবেন?

- **খুবই কম ক্ষেত্রে** `any` ব্যবহার করুন — শুধুমাত্র যখন আপনি পুরোপুরি নিশ্চিত যে টাইপ চেকিং বন্ধ করতেই হবে (third-party লাইব্রেরির খারাপ টাইপ ডেফিনিশন ইত্যাদি)।
- বেশিরভাগ ক্ষেত্রে **`unknown`** ব্যবহার করুন, বিশেষ করে external data, API রেসপন্স, বা কনফিগারেশন ফাইল নিয়ে কাজ করার সময়।
- `unknown` + proper type narrowing = সত্যিকারের robust ও maintainable কোড।

#### উপসংহার

`any` আপনাকে দ্রুত কোড লিখতে সাহায্য করে, কিন্তু দীর্ঘমেয়াদে এটি আপনার প্রজেক্টকে দুর্বল করে। `unknown` একটু বেশি কাজ দেয় (narrowing করতে হয়), কিন্তু এটি আপনাকে TypeScript-এর আসল শক্তি — **compile-time safety** — উপভোগ করতে দেয়।

**স্লোগান মনে রাখুন:**  
*"Use `unknown` by default. Use `any` only when you know better."*

