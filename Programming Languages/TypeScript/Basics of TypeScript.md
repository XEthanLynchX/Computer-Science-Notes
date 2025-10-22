## What is TypeScript?

- Superset of **JavaScript** that adds **static typing**.
    
- Compiles down to **plain JavaScript**.
    
- Helps catch errors early and improves developer experience.
    

---

## Setup

`tsconfig.json` (basic example):
``` shell
# Install TypeScript globally
npm install -g typescript

# Initialize in a project
npx tsc --init

# Compile TS → JS
npx tsc

# Watch mode
npx tsc --watch

```

#### ```tsconfig.json ```

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist",
    "rootDir": "src"
  }
}

```

---

## Basic Types

```ts 
let isDone: boolean = false;
let age: number = 25;
let userName: string = "Ethan";
let ids: number[] = [1, 2, 3];
let tuple: [string, number] = ["Age", 25];
let anything: any = "can be anything"; 
let unknownValue: unknown = 10; 
let nothing: void = undefined;
let neverValue: never; // something that never occurs

```


---

## Type Inference

```ts
`let count = 10;  // inferred as number`
```


---

## Type vs Interface

### Interface: 

```ts
interface User {
  id: number;
  name: string;
  isAdmin?: boolean; // optional
}

```


### Type Alias

```ts 
type User = {
  id: number;
  name: string;
  isAdmin?: boolean;
};

```

### Notes
- Use **interface**s for when you to classes that perform the same function but have different logic or return values
- Use **type** for unions, primitives, utility type

---

## Functions

```ts 
function add(a: number, b: number): number {
  return a + b;
}

const log = (msg: string): void => console.log(msg);

// Optional & default params
function greet(name: string, greeting: string = "Hello") {
  return `${greeting}, ${name}`;
}

```

---

## Generics

```ts
function identity<T>(value: T): T {
  return value;
}

let num = identity<number>(10);
let str = identity("hello"); // inferred

```

---

## Classes

```ts 
class Person {
  constructor(
    public name: string,
    private age: number
  ) {}

  greet(): string {
    return `Hello, my name is ${this.name}`;
  }
}

```

---

## Enums

```ts 
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}
```

---

## Union & Intersection Types

```ts 
type Status = "success" | "error" | "loading";

type User = { id: number } & { name: string };
```

---

## Utility Types

```ts
interface User {
  id: number;
  name: string;
  age?: number;
}

type PartialUser = Partial<User>;     // all optional
type RequiredUser = Required<User>;   // all required
type ReadonlyUser = Readonly<User>;   // readonly fields
type UserId = Pick<User, "id">;       // only id
type UserName = Omit<User, "id">;     // everything except id
type UserKeys = keyof User;           // "id" | "name" | "age"

```

---

## Advanced Types

```ts
// Conditional types
type IsString<T> = T extends string ? true : false;
type Test = IsString<"hello">; // true

// Mapped types
type Options<T> = {
  [K in keyof T]?: T[K];
};

```

---

## Modules

```ts
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

// app.ts
import { add } from "./math";
```

---

## Type Narrowing

```ts
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}

```

---

## Best Practices

- Always enable **`strict`** mode.
    
- Prefer **type inference** when possible.
    
- Organize with **interfaces** for objects.
    
- Prefer **union types** for fixed values instead of enums.
    
- Leverage **utility types**.
    

---

## Tooling

- **ts-node** → Run TypeScript directly
    
- **eslint + @typescript-eslint** → Linting
    
- **prettier** → Formatting
    
- **Jest / Vitest** → Testing
    
- **Next.js / NestJS** → Frameworks with TS built-in