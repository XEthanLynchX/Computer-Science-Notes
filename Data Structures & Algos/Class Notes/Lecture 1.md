[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 1: Recursion

## What is recursion?

A **recursive** function solves a problem by **calling itself** on a smaller version of the same problem, until the problem is small enough to answer directly.

Every recursive function needs two parts:

| Part | What it does | If it's missing |
| --- | --- | --- |
| **Base case** | Answers the smallest version directly, with no recursive call | Infinite recursion → `StackOverflowError` |
| **Recursive case** | Calls itself on a smaller input and builds the answer from the result | The function never makes progress |

Each recursive call must move **closer to the base case** (for example, n → n − 1).

## Example 1: Sum of 1 to n

sum(n) = 1 + 2 + 3 + … + n

- **Base case:** sum(1) = 1
- **Recursive case:** sum(n) = n + sum(n − 1)

```java
public static int sum(int n) {
    if (n == 1) {          // base case
        return 1;
    }
    return n + sum(n - 1); // recursive case
}
```

Trace of `sum(4)`:

```
sum(4) = 4 + sum(3)
       = 4 + (3 + sum(2))
       = 4 + (3 + (2 + sum(1)))
       = 4 + (3 + (2 + 1))
       = 10
```

## Example 2: Factorial

n! = n × (n − 1) × … × 1

- **Base case:** fact(1) = 1 (fact(0) = 1 also works)
- **Recursive case:** fact(n) = n × fact(n − 1)

```java
public static int fact(int n) {
    if (n <= 1) {           // base case
        return 1;
    }
    return n * fact(n - 1); // recursive case
}
```

`fact(5)` = 5 × 4 × 3 × 2 × 1 = **120**

## Activation Record

An **activation record** (also called a **stack frame**) is the block of memory created for **one call** of a function. It holds everything that call needs to run and return:

- The **parameters** passed in
- The function's **local variables**
- The **return address** (where to continue in the caller when this call finishes)
- Space for the **return value**

Every recursive call gets its **own** activation record, so each call has its own copy of `n`.

![[63FE6493-EEEB-4D37-96C6-9B5ECBF71641_1_102_o.jpeg|429]]

## Runtime Stack

The **runtime stack** (call stack) holds the activation records of all calls that haven't finished yet.

- Calling a function **pushes** its activation record on top.
- The record on **top** has **execution control**; it's the call running right now.
- When a call returns, its record is **popped**, and control goes back to the record underneath.

Runtime stack for `fact(3)`:

```
Calls going down (push)            Returns coming back (pop)

| fact(1) |  ← top, base case       fact(1) returns 1
| fact(2) |                         fact(2) returns 2 × 1 = 2
| fact(3) |                         fact(3) returns 3 × 2 = 6
|  main   |                         main receives 6
```

If recursion never hits the base case, records keep getting pushed until memory runs out: a **stack overflow**.

![[1C79FD5E-A354-469F-B669-D6DB3580EF06.jpeg|346]]

## When is recursion used?

Recursion fits problems that are **made of smaller copies of themselves**:

- **Trees** (each subtree is a smaller tree), covered in [[Data Structures & Algos/Class Notes/Lecture 13|Lecture 13]]
- **Linked lists** (a list is a node followed by a smaller list)
- **Divide and conquer** sorts like merge sort and quick sort ([[Data Structures & Algos/Class Notes/Lecture 11 (2)|Lecture 11]])
- Parsing nested structures, like expressions with parentheses

Example from class: processing a number list one item at a time, where the **base case** is reaching a number with **no more commas after it** (the last item).

![[Recursion Example.png]]

## Recursion vs. iteration

| | Recursion | Iteration (loops) |
| --- | --- | --- |
| Memory | One activation record per call, O(n) stack space for depth n | Usually O(1) extra space |
| Readability | Clean for naturally recursive problems (trees) | Clean for simple counting |
| Risk | Stack overflow if too deep or no base case | Infinite loop if the condition never fails |

Any recursive function can be rewritten with a loop (sometimes with an explicit stack).

## Self-Check

1. Write a recursive `power(b, e)` that returns bᵉ. What is the base case?
2. Draw the runtime stack for `sum(3)` at its deepest point.
3. What happens if you call `fact(-1)` with the base case `n == 1`?
