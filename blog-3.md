# কীভাবে Generics reusable এবং strictly typed code তৈরি করতে সাহায্য করে?

TypeScript এর সবচেয়ে powerful feature গুলোর মধ্যে একটি হলো **Generics**।
Generics ব্যবহার করে এমন function, component, বা class তৈরি করা যায় যা বিভিন্ন ধরনের data নিয়ে কাজ করতে পারে, কিন্তু তারপরও type safety বজায় রাখে।

অর্থাৎ code reusable হয় এবং একই সাথে strictly typed থাকে।

---

# Generics কী?

Generics হলো এমন একটি system যেখানে আমরা type কে parameter হিসেবে পাঠাতে পারি।

সহজভাবে বললে,
যেমন function এ value parameter পাঠাই, ঠিক তেমনি Generics এ type parameter পাঠানো যায়।

---

# Generics ছাড়া সমস্যা কোথায়?

ধরুন আমরা এমন একটি function বানাতে চাই যেটা যেকোনো value return করবে।

```ts id="w2tk8q"
function identity(value: any) {
  return value;
}
```

এখানে function reusable হলেও সমস্যা হলো `any` ব্যবহার করা হয়েছে।
ফলে TypeScript type safety হারিয়ে ফেলে।

উদাহরণ:

```ts id="m5vp1x"
let result = identity("Hello");

result.toUpperCase();
```

এখানে কাজ করবে।
কিন্তু `any` থাকার কারণে TypeScript নিশ্চিত না `result` আসলে string কিনা।

---

# Generics ব্যবহার করলে

```ts id="j7ra3n"
function identity<T>(value: T): T {
  return value;
}
```

এখানে `T` একটি generic type parameter।

এখন:

```ts id="h9qd6m"
let result = identity("Hello");
```

TypeScript automatically বুঝবে `T` হলো `string`।

তাই:

```ts id="p4zk7f"
result.toUpperCase();
```

এখন safely কাজ করবে।

---

# কেন Generics গুরুত্বপূর্ণ?

কারণ এটি:

* reusable code তৈরি করে
* type safety বজায় রাখে
* runtime error কমায়
* autocomplete এবং IntelliSense improve করে

---

# Reusable Function Example

```ts id="n3yv8k"
function getFirstElement<T>(arr: T[]): T {
  return arr[0];
}
```

এখন function টি different type এর array এর সাথে কাজ করতে পারবে।

```ts id="c8pl2w"
const number = getFirstElement([1, 2, 3]);

const text = getFirstElement(["a", "b", "c"]);
```

প্রথম ক্ষেত্রে return type হবে `number`।
দ্বিতীয় ক্ষেত্রে return type হবে `string`।

একই function different data structure এর জন্য reusable হয়ে গেল।

---

# Generic Interface Example

```ts id="q6nm1r"
interface ApiResponse<T> {
  success: boolean;
  data: T;
}
```

এখন different ধরনের data এর জন্য same interface ব্যবহার করা যাবে।

```ts id="d2vk5s"
const userResponse: ApiResponse<string> = {
  success: true,
  data: "Tanjina",
};
```

আবার:

```ts id="u8gx4p"
const productResponse: ApiResponse<number> = {
  success: true,
  data: 100,
};
```

এখানে structure একই থাকলেও data type different।

---

# React এ Generics এর ব্যবহার

React component এও Generics অনেক ব্যবহার হয়।

উদাহরণ:

```ts id="f1mt9z"
type Props<T> = {
  items: T[];
};

function List<T>({ items }: Props<T>) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{String(item)}</li>
      ))}
    </ul>
  );
}
```

এখন একই component different ধরনের data handle করতে পারবে।

---

# Strict Typing কীভাবে বজায় থাকে?

Generics এর সবচেয়ে বড় সুবিধা হলো TypeScript actual type track করতে পারে।

উদাহরণ:

```ts id="l4qy7b"
function wrapInArray<T>(value: T): T[] {
  return [value];
}
```

যদি:

```ts id="x9vn3e"
wrapInArray(10);
```

তাহলে return type হবে `number[]`

আর যদি:

```ts id="z5kw8m"
wrapInArray("hello");
```

তাহলে return type হবে `string[]`

অর্থাৎ TypeScript dynamically type ধরে রাখছে।

---

# Generics বনাম `any`

| Feature              | Generics | any     |
| -------------------- | -------- | ------- |
| Type Safety          | থাকে     | থাকে না |
| Reusable             | হ্যাঁ    | হ্যাঁ   |
| IntelliSense Support | ভালো     | দুর্বল  |
| Runtime Error Chance | কম       | বেশি    |

---

# উপসংহার

Generics TypeScript এর একটি অত্যন্ত গুরুত্বপূর্ণ feature।
এটি reusable code তৈরি করতে সাহায্য করে এবং একই সাথে strict typing বজায় রাখে।

ফলে:

* একই function বা component বিভিন্ন data structure এর সাথে কাজ করতে পারে
* code duplication কমে
* type safety বজায় থাকে
* বড় project maintain করা সহজ হয়

তাই scalable এবং professional TypeScript application তৈরির জন্য Generics শেখা খুবই গুরুত্বপূর্ণ।
