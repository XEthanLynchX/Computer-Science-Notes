# JavaScript — Everything You Need to Know (with Arrays, Maps/Hashing, and Increment Patterns)

---

## Table of Contents

1. [Language Basics](#language-basics)
   - [Values & Types](#values--types)
   - [Variables: `let`, `const`, `var`](#variables-let-const-var)
   - [Operators (including `++`)](#operators-including-)
   - [Control Flow](#control-flow)
2. [Functions & Scope](#functions--scope)
   - [Function Declarations vs Expressions](#function-declarations-vs-expressions)
   - [Arrow Functions](#arrow-functions)
   - [`this`, Bindings & Closures](#this-bindings--closures)
3. [Modules & Classes](#modules--classes)
4. [Arrays — The Complete Guide](#arrays--the-complete-guide)
   - [Creation & Basics](#creation--basics)
   - [Reading & Writing](#reading--writing)
   - [Core Methods (mutating vs non-mutating)](#core-methods-mutating-vs-non-mutating)
   - [Transform, Query & Aggregate](#transform-query--aggregate)
   - [Sorting & Comparing](#sorting--comparing)
   - [Iteration & Performance Tips](#iteration--performance-tips)
5. [Hashing & Mapping — Objects, `Map`, `Set`, `WeakMap`, `WeakSet`](#hashing--mapping--objects-map-set-weakmap-weakset)
   - [When to Use `Object` vs `Map`](#when-to-use-object-vs-map)
   - [`Map` API: `set`, `get`, `has`, `delete`](#map-api-set-get-has-delete)
   - [Counting/Frequency Patterns](#countingfrequency-patterns)
   - [Group By / Dedup / Two-Sum](#group-by--dedup--two-sum)
   - [Key Equality & Edge Cases](#key-equality--edge-cases)
6. [Incrementing: When You Can and Can’t](#incrementing-when-you-can-and-cant)
   - [Prefix vs Postfix](#prefix-vs-postfix)
   - [With `const`, `BigInt`, `strings`, and `undefined` properties](#with-const-bigint-strings-and-undefined-properties)
   - [Safe Increment Patterns for Maps/Objects](#safe-increment-patterns-for-mapsobjects)
7. [Async JavaScript](#async-javascript)
   - [Callbacks, Promises, `async`/`await`](#callbacks-promises-asyncawait)
   - [Common Async Patterns & Pitfalls](#common-async-patterns--pitfalls)
8. [Error Handling](#error-handling)
9. [Node.js vs Browser Notes](#nodejs-vs-browser-notes)
10. [Quick Reference Snippets](#quick-reference-snippets)

---

## Language Basics

### Values & Types
- **Primitive types**: `number`, `bigint`, `string`, `boolean`, `symbol`, `undefined`, `null`.
- **Objects**: everything else (arrays, functions, dates, custom objects).
- `typeof` quick checks:

```js
typeof 42;            // 'number'
typeof 42n;           // 'bigint'
typeof 'hi';          // 'string'
typeof true;          // 'boolean'
typeof Symbol('x');   // 'symbol'
typeof undefined;     // 'undefined'
typeof null;          // 'object' (quirk)
typeof [];            // 'object'
typeof (() => {});    // 'function'
```

### Variables: `let`, `const`, `var`
- **`let`**: block-scoped, reassignable.
- **`const`**: block-scoped, **not reassignable** (but the referenced object **can** be mutated).
- **`var`**: function-scoped, hoisted; avoid in modern code.

```js
const config = { debug: true };
config.debug = false;    // ✅ allowed (mutating object)
// config = {}            // ❌ TypeError (reassignment not allowed)
```

### Operators (including `++`)
- Arithmetic: `+ - * / % **`
- Comparison: `=== !== > >= < <=` (prefer `===`/`!==` to avoid coercion quirks).
- Logical: `&& || !` plus **nullish coalescing** `??` and **optional chaining** `?.`
- **Increment/Decrement**: `++x` (prefix), `x++` (postfix return value differs). See [Incrementing](#incrementing-when-you-can-and-cant).

### Control Flow
- `if` / `else`, `switch`, `for` / `for...of`, `while`, `do...while`, `try` / `catch` / `finally`.

```js
for (let x of [1,2,3]) {
  // x is each element
}

for (let i = 0; i < 3; i++) { /* ... */ }
```

---

## Functions & Scope

### Function Declarations vs Expressions
```js
function add(a, b) { return a + b; }            // declaration (hoisted)
const multiply = function(a, b) { return a*b; } // expression
```

### Arrow Functions
- Compact syntax, lexical `this`, no `arguments`, not constructible.
```js
const square = x => x * x;
const sum = (a, b) => { return a + b; };
```

### `this`, Bindings & Closures
- `this` depends on how a function is called.
- Arrow functions **capture** `this` from the surrounding scope.
- Closures remember variables from their defining scope.

```js
const counter = () => {
  let n = 0;
  return () => ++n; // inner function closes over n
};
const inc = counter();
inc(); // 1
inc(); // 2
```

---

## Modules & Classes

### ES Modules
```js
// utils.js
export function id(x) { return x; }
export default class Logger { log(x) { console.log(x); } }

// main.js
import Logger, { id } from './utils.js';
```

### Classes & Inheritance
```js
class Animal {
  constructor(name) {
   this.name = name; 
   }
  speak(){ 
  console.log(`${this.name} makes a noise.`);
   }
}

class Dog extends Animal {
  speak() { console.log(`${this.name} barks.`); }
}

new Dog('Rex').speak(); // 'Rex barks.'
```

---

## Arrays — The Complete Guide

### Creation & Basics
```js
const a = [1, 2, 3];
const b = Array.of(1, 2, 3);
const c = Array(3).fill(0);  // [0,0,0]
const d = []; d.length = 3;   // [ <3 empty items> ] sparse
```

### Reading & Writing
```js
const arr = [10, 20, 30];
arr[0];        // 10
arr[1] = 25;   // update
arr.length;    // 3
arr.push(40);  // [10,20,25,30,40]
arr.pop();     // removes last
```

### Core Methods (mutating vs non-mutating)
- **Mutating**: `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `fill`.
- **Non-mutating**: `slice`, `concat`, `map`, `filter`, `reduce`, `flat`, `flatMap`, `includes`, `indexOf`, `find`, `findIndex`, `toSorted`, `toReversed`, `toSpliced` (if supported), `with`.

```js
const xs = [3,1,2];
xs.sort((a,b) => a - b); // mutates xs -> [1,2,3]
const sorted = xs.toSorted((a,b) => a - b); // non-mutating (new array)
```

### Transform, Query & Aggregate
```js
const nums = [1,2,3,4];
nums.map(x => x + 1);         // [2,3,4,5]
nums.filter(x => x % 2 === 0); // [2,4]
nums.reduce((sum,x) => sum + x, 0); // 10
nums.find(x => x > 2);        // 3
nums.some(x => x < 0);        // false
nums.every(x => x > 0);       // true
nums.includes(3);             // true
```

### Sorting & Comparing
- Default `Array.prototype.sort()` sorts **as strings**; always pass a comparator for numbers.
```js
[10, 2, 5].sort();           // ['10','2','5'] -> becomes [10,2,5] but lexicographic
[10, 2, 5].sort((a,b)=>a-b); // [2,5,10]
```

### Iteration & Performance Tips
- Prefer `for...of` when you need simple iteration.
- Avoid mutating in-place unless necessary; consider functional methods (`map`, `filter`, `reduce`).
- For large arrays, `for` loops can be faster; measure if performance matters.
- Beware of sparse arrays; methods may skip "holes".

---

## Hashing & Mapping — Objects, `Map`, `Set`, `WeakMap`, `WeakSet`

### When to Use `Object` vs `Map`
- **`Object`**: keys are **strings or symbols**; good for lightweight records. Prototype chain exists (use `Object.create(null)` for a true dictionary with no inherited keys).
- **`Map`**: keys can be **any value** (objects, arrays, functions, primitives). Preserves **insertion order**. Provides clean APIs for presence checks and iteration.

```js
// Object dictionary (string keys only)
const countsObj = Object.create(null);
countsObj['apple'] = 1;

// Map dictionary (any keys)
const countsMap = new Map();
countsMap.set({id:1}, 1);    // object key by reference
```

### `Map` API: `set`, `get`, `has`, `delete`
```js
const m = new Map();
m.set('a', 10);
m.get('a');     // 10
m.has('a');     // true
m.delete('a');  // true
m.size;         // 0 after delete

// iterate
for (const [key, value] of m) {
  console.log(key, value);
}

// convert between Map and Object
Object.fromEntries(m);             // Map -> plain object
new Map(Object.entries({a:1,b:2}));// object -> Map
```

### Counting/Frequency Patterns
```js
// Count frequency using Object
const freqObj = Object.create(null);
for (const x of ['a','b','a']) {
  freqObj[x] = (freqObj[x] ?? 0) + 1;
}
// freqObj = { a:2, b:1 }

// Count frequency using Map
const freqMap = new Map();
for (const x of ['a','b','a']) {
  freqMap.set(x, (freqMap.get(x) ?? 0) + 1);
}
// freqMap.get('a') === 2
```

### Group By / Dedup / Two-Sum
```js
// Group by length
const words = ['no','yes','cat','at'];
const groups = new Map();
for (const w of words) {
  const key = w.length;
  const list = groups.get(key) ?? [];
  list.push(w);
  groups.set(key, list);
}
// groups: 2 -> ['no','at'], 3 -> ['yes','cat']

// Deduplicate using Set
const deduped = [...new Set([1,2,2,3,3,3])]; // [1,2,3]

// Two-sum with Map (indices)
function twoSum(nums, target) {
  const seen = new Map();
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (seen.has(need)) return [seen.get(need), i];
    seen.set(nums[i], i);
  }
  return null;
}
```

### Key Equality & Edge Cases
- `Map`/`Set` use **SameValueZero** equality: `NaN` equals `NaN`; `+0` and `-0` are treated as equal.
- Object keys are strings/symbols; numeric keys become strings: `obj[1]` is the same as `obj['1']`.
- Object keys for references (like `{}`) are coerced to "[object Object]" if used incorrectly—use `Map` for object keys.

---

## Incrementing: When You Can and Can’t

### Prefix vs Postfix
```js
let x = 0;
++x;   // x becomes 1; expression value is 1 (prefix)
x++;   // x becomes 2; expression value is 1 (postfix returns old value)
```

### With `const`, `BigInt`, `strings`, and `undefined` properties
- **`const` variables** cannot be reassigned — `x++` on a `const` will throw.
- **`BigInt`** supports `++` (on a `let`/`var` BigInt), but you **cannot mix** `BigInt` with `number` in arithmetic.
- **Strings**: `++'2'` coerces to number (3); `++'x'` becomes `NaN`. Avoid relying on coercion.
- **Undefined properties**: `obj.count++` fails if `obj.count` is `undefined` (becomes `NaN`). Use safe default patterns.

```js
// Safe default (Object)
obj.count = (obj.count ?? 0) + 1; // ✅ works even if obj.count is undefined

// Safe default (Map)
map.set(key, (map.get(key) ?? 0) + 1);
```

### Safe Increment Patterns for Maps/Objects
- Prefer explicit addition with a default rather than `++` when values may be missing.
```js
const counts = new Map();
function inc(key) {
  counts.set(key, (counts.get(key) ?? 0) + 1);
}
inc('a'); inc('a'); // counts.get('a') === 2
```

---

## Async JavaScript

### Callbacks, Promises, `async`/`await`
```js
// Promise
function fetchUser(id) {
  return fetch(`/api/users/${id}`).then(res => res.json());
}

// async/await
async function getUserName(id) {
  try {
    const user = await fetchUser(id);
    return user.name;
  } catch (err) {
    console.error('Failed:', err);
    return null;
  }
}
```

### Common Async Patterns & Pitfalls
- Use `Promise.all([...])` to run in parallel; if one rejects, all reject.
- Use `Promise.allSettled([...])` when you need **all results** regardless of failures.
- Always `await` asynchronous functions you depend on; otherwise you’ll have pending promises.
- In Node.js, top-level `await` works in ES modules.

```js
const urls = ['a.json','b.json','c.json'];
const results = await Promise.all(urls.map(u => fetch(u).then(r => r.json())));
```

---

## Error Handling
```js
try {
  risky();
} catch (e) {
  console.error('Error:', e);
} finally {
  cleanUp();
}

// Custom errors
class AppError extends Error {
  constructor(message, code) { super(message); this.code = code; }
}
```

---

## Node.js vs Browser Notes
- **Global objects** differ: `window` (browser) vs `global` (Node), `globalThis` works in both.
- **Modules**: Node supports ES Modules (`.mjs` or `type: "module"`) and CommonJS (`require`). Prefer ESM for modern code.
- **APIs**: `fetch` is available in modern Node versions; otherwise use libraries.

```js
// Detect environment
const isNode = typeof process !== 'undefined' && process.versions?.node;
const isBrowser = typeof window !== 'undefined';
```

---

## Quick Reference Snippets

### Optional Chaining & Nullish Coalescing
```js
const city = user.address?.city ?? 'Unknown';
```

### Immutable Updates
```js
const next = { ...prev, count: prev.count + 1 };
const nextArr = arr.map(x => x.id === id ? { ...x, done: true } : x);
```

### Deep Clone (simple cases)
```js
const clone = structuredClone(obj); // browser/Node 17+
// Fallback (not for complex cases): JSON
const clone2 = JSON.parse(JSON.stringify(obj));
```

### Remove Duplicates & Count
```js
const data = ['a','b','a','c'];
const unique = [...new Set(data)]; // ['a','b','c']
const counts = data.reduce((m, x) => (m[x] = (m[x] ?? 0) + 1, m), {});
```

### Map vs Object: Choose Wisely
- Use **`Map`** when keys are not strings, or you need frequent presence checks and ordered iteration.
- Use **`Object`** for simple records/structs and JSON compatibility.

---

## Common Gotchas
- `==` vs `===`: prefer `===` to avoid coercion surprises.
- `sort()` without comparator sorts as strings.
- `for...in` iterates **keys** (including inherited) on objects; use `Object.keys()` or `hasOwnProperty` checks.
- Incrementing missing values produces `NaN`; use defaults (`?? 0`).
- Don’t mutate `const` bindings; update fields instead.

---

## Practice Tasks (Try These)
1. Implement frequency count of words using both `Object` and `Map`.
2. Write a `groupBy(array, keyFn)` utility using `Map`.
3. Implement `twoSum` using a single pass and a `Map`.
4. Sort an array of objects by multiple fields (e.g., age, then name).
5. Convert between `Map` and `Object` and back.

---

### License
This cheatsheet is MIT-licensed. Copy, modify, and share freely.
