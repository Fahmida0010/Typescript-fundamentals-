# কীভাবে `Pick` এবং `Omit` Utility Types কোড Duplication কমায় এবং কোডকে DRY রাখে?

TypeScript এ বড় project এ কাজ করার সময় আমরা অনেক interface ব্যবহার করি।
কিন্তু অনেক সময় একই interface এর কিছু specific property নিয়ে নতুন type তৈরি করতে হয়।

যদি বারবার manually একই property লিখতে হয়, তাহলে code duplication হয়।
এই সমস্যা সমাধানের জন্য TypeScript এ রয়েছে `Pick` এবং `Omit` utility types।

এগুলো master interface থেকে প্রয়োজনীয় অংশ নিয়ে নতুন type তৈরি করতে সাহায্য করে। ফলে কোড cleaner এবং DRY থাকে।

---

# DRY কী?

DRY এর পূর্ণরূপ হলো:

**Don’t Repeat Yourself**

অর্থাৎ একই code বা information বারবার না লিখে reusable way তে ব্যবহার করা।

Software development এ DRY principle খুব গুরুত্বপূর্ণ কারণ:

* কোড ছোট হয়
* maintain করা সহজ হয়
* bug কম হয়
* update করা সহজ হয়

---

# `Pick` কী?

`Pick` ব্যবহার করা হয় কোনো interface থেকে নির্দিষ্ট কিছু property নিয়ে নতুন type তৈরি করার জন্য।

Syntax:

```ts id="d91xk2"
Pick<Type, Keys>
```

---

# উদাহরণ

```ts id="a7m4pl"
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}
```

ধরুন frontend এ শুধু `id`, `name`, এবং `email` দরকার।
এখন নতুন interface manually লিখলে duplication হবে।

খারাপ approach:

```ts id="y4vq3c"
interface UserProfile {
  id: number;
  name: string;
  email: string;
}
```

এখানে একই property আবার লিখতে হচ্ছে।

এখন `Pick` ব্যবহার করলে:

```ts id="m2f8tz"
type UserProfile = Pick<User, "id" | "name" | "email">;
```

এখন `User` interface থেকে শুধুমাত্র প্রয়োজনীয় property নেওয়া হয়েছে।

---

# `Omit` কী?

`Omit` হলো `Pick` এর opposite।
এটি কিছু property বাদ দিয়ে নতুন type তৈরি করে।

Syntax:

```ts id="u7kp2s"
Omit<Type, Keys>
```

---

# উদাহরণ

```ts id="e4jw9q"
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}
```

ধরুন API response এ password পাঠাতে চাই না।

তাহলে:

```ts id="f9nc1m"
type SafeUser = Omit<User, "password">;
```

এখন `SafeUser` type এ সব property থাকবে শুধু `password` ছাড়া।

---

# কীভাবে Code Duplication কমায়?

ধরুন বড় project এ ২০টি interface আছে।
যদি প্রতিবার নতুন type manually লিখতে হয়, তাহলে:

* একই property বারবার লিখতে হবে
* typo হওয়ার chance বাড়বে
* update করা কঠিন হবে

কিন্তু `Pick` এবং `Omit` ব্যবহার করলে master interface একবার লিখলেই হয়।

সব specialized type সেখান থেকেই তৈরি করা যায়।

---

# Real-Life Example

```ts id="g8tr5v"
interface Product {
  id: number;
  title: string;
  price: number;
  description: string;
  stock: number;
}
```

Cart এ শুধু কিছু data দরকার:

```ts id="b2mh8x"
type CartProduct = Pick<Product, "id" | "title" | "price">;
```

Admin panel এ stock ছাড়া সব দরকার:

```ts id="k6pw3n"
type PublicProduct = Omit<Product, "stock">;
```

এভাবে একই interface থেকে different “slice” তৈরি করা যায়।

---

# কেন এটি DRY Principle Follow করে?

কারণ:

* একই structure বারবার লিখতে হয় না
* master interface update করলে related type গুলোও automatically update হয়
* maintenance সহজ হয়
* code reusable হয়

উদাহরণ:

```ts id="r5yv0d"
interface User {
  id: number;
  name: string;
  email: string;
}
```

যদি পরে `phone` add করা হয়:

```ts id="s8ln4a"
interface User {
  id: number;
  name: string;
  email: string;
  phone: string;
}
```

তাহলে `Pick` বা `Omit` দিয়ে তৈরি type গুলো automatically updated হবে।
ম্যানুয়ালি সব জায়গায় change করতে হবে না।

---

# উপসংহার

`Pick` এবং `Omit` TypeScript এর খুব useful utility types।
এগুলো master interface থেকে প্রয়োজন অনুযায়ী ছোট ছোট specialized type তৈরি করতে সাহায্য করে।

ফলে:

* code duplication কমে
* maintenance সহজ হয়
* reusable structure তৈরি হয়
* DRY principle বজায় থাকে

তাই বড় TypeScript project এ clean এবং scalable code লেখার জন্য `Pick` এবং `Omit` ব্যবহার করা খুবই গুরুত্বপূর্ণ।
