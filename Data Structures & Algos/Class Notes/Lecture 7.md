[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 7: Queues & Radix Sort

## Queue ADT

A **queue** is a line of items waiting to be served. It is **FIFO**: **F**irst **I**n, **F**irst **O**ut. Items join at the **rear** and leave from the **front**.

### Queue operations

| Operation | What it does |
| --- | --- |
| `enqueue(e)` | Add `e` at the **rear** |
| `dequeue()` | Remove and return the **front** element (serve it) |
| `front()` | Return a copy of the front element without removing it |
| `isFull()` | `true` if the queue is at capacity (array versions) |
| `isEmpty()` | `true` if there are no elements |

Stack vs. queue: a stack adds and removes at the **same** end (LIFO); a queue adds at one end and removes at the **other** (FIFO).

## Three implementations

### 1. Static array

- Front is always index 0.
- On every `dequeue`, **shift every remaining element forward** one spot.
- That shifting makes `dequeue` **O(n)**, which is the slow part.

```
enqueue 10, 20, 30:   [10, 20, 30, _, _]
dequeue (returns 10): [20, 30, _, _, _]   ← everything shifted left
```

### 2. Circular array

- Elements **don't shift**. Instead, the `front` and `rear` **indexes move**.
- When an index reaches the end of the array, it **wraps around** to 0 using **mod**:

```java
rear  = (rear + 1)  % capacity;   // enqueue
front = (front + 1) % capacity;   // dequeue
```

- This reuses the empty slots at the start of the array, so both operations are **O(1)**.
- Keep a `count` (or leave one slot empty) to tell a full queue from an empty one, since `front == rear` could mean either.

```
capacity 5, after enqueue A,B,C,D,E and dequeue A,B:
index:  0  1  2  3  4
       [_, _, C, D, E]     front = 2, rear = 4
enqueue F → rear = (4 + 1) % 5 = 0
       [F, _, C, D, E]     F wrapped around to index 0
```

### 3. Linked list

- Keep two references: **`front`** (head) and **`rear`** (tail).
- `enqueue` = add a node after `rear`. `dequeue` = remove the `front` node.
- Both are **O(1)**, and the queue never fills up.

### Comparison

| | enqueue | dequeue | Size limit |
| --- | --- | --- | --- |
| Static array | O(1) | **O(n)** (shifting) | Fixed |
| Circular array | O(1) | O(1) | Fixed |
| Linked list | O(1) | O(1) | None |

---

## Radix sort (digit / bucket sort)

A queue application: sort numbers **one digit at a time**, without comparing numbers to each other.

1. Create **10 queues**, one for each digit 0–9.
2. Starting with the **last (ones) digit**, enqueue each number into the queue for that digit.
3. Dequeue all queues in order 0 → 9 back into the list.
4. Repeat for the tens digit, hundreds digit, and so on.
5. The number of passes **k** = number of digits in the **largest** value.

Because queues are FIFO, the order from earlier passes is kept for ties. That's what makes it work.

### Example: `[170, 45, 75, 90, 802, 24, 2, 66]` (largest has 3 digits → 3 passes)

| Pass | Digit | Queues used (digit: values) | List after |
| --- | --- | --- | --- |
| 1 | ones | 0: 170, 90 · 2: 802, 2 · 4: 24 · 5: 45, 75 · 6: 66 | [170, 90, 802, 2, 24, 45, 75, 66] |
| 2 | tens | 0: 802, 2 · 2: 24 · 4: 45 · 6: 66 · 7: 170, 75 · 9: 90 | [802, 2, 24, 45, 66, 170, 75, 90] |
| 3 | hundreds | 0: 2, 24, 45, 66, 75, 90 · 1: 170 · 8: 802 | [2, 24, 45, 66, 75, 90, 170, 802] |

**Time: O(k · n)**, which is **linear** in n when k is small. More detail in [[Data Structures & Algos/Class Notes/Lecture 11 (2)|Lecture 11]].

---

## Other queue types

| Type | How it works | Example |
| --- | --- | --- |
| **Priority queue** | Served by **priority**, not arrival time | ER patients treated by injury severity. Usually built with a heap ([[Data Structures & Algos/Class Notes/Lecture 16|Lecture 16]]) |
| **Double-ended queue (deque)** | Add and remove at **both** the front and the rear | Can act as a stack or a queue |

## Self-Check

1. In a circular array of capacity 6, `rear = 5`. Where does the next `enqueue` go?
2. Why is `dequeue` O(n) in a static array but O(1) in a circular array?
3. Run radix sort on `[31, 4, 15, 9, 26]`. How many passes does it need?
