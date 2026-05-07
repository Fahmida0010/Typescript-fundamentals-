# Why `any` Is Called a “Type Safety Hole” and Why `unknown` Is Safer in TypeScript

When I started learning TypeScript, I saw two types that looked almost the same: `any` and `unknown`. At first, I thought both were identical because both can store any kind of value. But after practicing more, I understood there is a big difference between them.

In this blog, I will explain:

* Why `any` is called a “type safety hole”
* Why `unknown` is safer
* What type narrowing means in TypeScript

---

## What is `any`?

The `any` type means a variable can contain any kind of value. When we use `any`, TypeScript stops checking types.

Example:

```ts id="w0c5z2"
let value: any = "Hello";

console.log(value.toUpperCase());
console.log(value.toFixed(2));
```

Here TypeScript does not show any error, even though `toFixed()` is only for numbers.

This can create problems during runtime.

---

## Why is `any` Called a “Type Safety Hole”?

TypeScript is mainly used to make code safer by checking types before running the program. But when we use `any`, TypeScript cannot protect us anymore.

That is why developers call it a:

> “Type Safety Hole”

Because it creates a hole in TypeScript’s safety system.

Example:

```ts id="oq5m8x"
function printData(data: any) {
  console.log(data.toUpperCase());
}

printData(50);
```

This code will compile successfully, but during runtime it will crash because numbers do not have `toUpperCase()`.

So using `any` too much can make our code unsafe.

---

## What is `unknown`?

The `unknown` type is also able to store any value, but it is safer than `any`.

With `unknown`, TypeScript forces us to check the type before using the value.

Example:

```ts id="q2m7dv"
let value: unknown = "TypeScript";

console.log(value.toUpperCase());
```

TypeScript will show an error here because it does not know whether the value is actually a string.

---

## What is Type Narrowing?

Type narrowing means checking the type of a variable before using it.

After checking, TypeScript understands the specific type.

Example:

```ts id="w9r1tp"
let value: unknown = "TypeScript";

if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

Now TypeScript knows that `value` is a string inside the `if` block. So the code becomes safe.

This process is called type narrowing.

---

## Common Ways of Type Narrowing

### Using `typeof`

```ts id="g4z7nc"
let age: unknown = 20;

if (typeof age === "number") {
  console.log(age.toFixed(2));
}
```

---

### Using `instanceof`

```ts id="x8b3ke"
let dateValue: unknown = new Date();

if (dateValue instanceof Date) {
  console.log(dateValue.getFullYear());
}
```

---

### Using `in` Operator

```ts id="b6m2qj"
type User = {
  name: string;
};

type Admin = {
  role: string;
};

function printInfo(person: User | Admin) {
  if ("name" in person) {
    console.log(person.name);
  } else {
    console.log(person.role);
  }
}
```

---

## Difference Between `any` and `unknown`

| `any`                           | `unknown`                    |
| ------------------------------- | ---------------------------- |
| Disables type checking          | Keeps type safety            |
| Unsafe                          | Safer                        |
| Can cause runtime errors easily | Requires checking before use |
| Easy to misuse                  | Better for secure coding     |

---

## Conclusion

From my learning experience, I think `unknown` is much better than `any` in most cases. Although `any` is easier to use, it removes TypeScript’s safety features. On the other hand, `unknown` helps us write safer and cleaner code by forcing type checks.
