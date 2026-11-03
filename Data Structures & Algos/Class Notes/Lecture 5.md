[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 5: Stacks & Polish Notation

## Stack ADT

A **stack** is a container where you can only reach the **top** element. It is **LIFO**: **L**ast **I**n, **F**irst **O**ut (like a stack of plates).

### Stack methods

| Method | What it does | Cost |
| --- | --- | --- |
| `push(e)` | Add `e` to the top | O(1) |
| `pop()` | Remove and return the top element | O(1) |
| `top()` / `peek()` | Return the top element **without** removing it | O(1) |
| `isEmpty()` | `true` if there are no elements | O(1) |
| `isFull()` | `true` if the stack is at capacity (array version) | O(1) |
| `size()` | Number of elements | O(1) |

Errors to guard against (see [[Data Structures & Algos/Class Notes/Lecture 6|Lecture 6]]):

- **Stack overflow:** `push` on a full stack
- **Stack empty (underflow):** `pop` or `peek` on an empty stack

## Implementation 1: Array

- Elements fill from index **0** upward: 0, 1, 2, …
- `top` holds the index of the top element. **`top = -1` means empty.**
- The top element is always at the **highest used index**.

```java
public class ArrayStack {
    private int[] data;
    private int top = -1;                 // -1 means empty

    public ArrayStack(int capacity) {
        data = new int[capacity];
    }

    public boolean isEmpty() { return top == -1; }
    public boolean isFull()  { return top == data.length - 1; }
    public int size()        { return top + 1; }

    public void push(int e) {
        if (isFull()) throw new RuntimeException("Stack overflow");
        top++;
        data[top] = e;
    }

    public int pop() {
        if (isEmpty()) throw new RuntimeException("Stack empty");
        int value = data[top];
        top--;
        return value;
    }

    public int peek() {
        if (isEmpty()) throw new RuntimeException("Stack empty");
        return data[top];
    }
}
```

Example: push 0, 1, 2, 3, 4, 5 into a stack of capacity 6.

```
index:  0  1  2  3  4  5
data:  [0, 1, 2, 3, 4, 5]     top = 5 (stack is full)
```

Popping everything returns **5, 4, 3, 2, 1, 0**: reverse order of insertion.

## Implementation 2: Linked List

- The **head** of the list is the **top** of the stack.
- `push` = insert a new node at the head. `pop` = remove the head.
- Both are O(1), and the stack never gets "full" (it grows as needed).

```java
public void push(int e) {
    Node n = new Node(e);
    n.next = head;
    head = n;
}

public int pop() {
    if (head == null) throw new RuntimeException("Stack empty");
    int value = head.data;
    head = head.next;
    return value;
}
```

![[BCCA247F-BC0A-4B51-93AA-7C0049D7FE14.jpeg]]

| | Array stack | Linked list stack |
| --- | --- | --- |
| push / pop | O(1) | O(1) |
| Size limit | Fixed capacity (can be full) | No limit |
| Extra memory | None | One `next` reference per node |

In class, the **array** was called the more efficient choice: no extra references and better memory locality.

---

## Stack Application: Polish Notation

### Goals for this unit

- Convert infix expressions to **prefix** and **postfix** by hand
- **Evaluate** infix, prefix, and postfix expressions using stacks
- Convert infix to prefix/postfix **algorithmically** ([[Data Structures & Algos/Class Notes/Lecture 6|Lecture 6]])

### Infix, prefix, postfix

They differ only in **where the operator goes**:

| Notation | Operator position | `a + b` |
| --- | --- | --- |
| **Infix** | Between operands | `a + b` |
| **Prefix** (Polish) | Before operands | `+ a b` |
| **Postfix** (Reverse Polish) | After operands | `a b +` |

Prefix and postfix **never need parentheses**; the order of the symbols alone fixes the order of operations.

### Manual conversion method

1. **Fully parenthesize** the infix expression using order of operations (`*` `/` before `+` `-`, then left to right).
2. **Prefix:** move each operator to replace its **left** parenthesis. **Postfix:** move each operator to replace its **right** parenthesis.
3. Remove the remaining parentheses.

Example: `a + b * c` → `(a + (b * c))`

- Prefix: `+ a * b c`
- Postfix: `a b c * +`

### Worked examples

| Infix | Fully parenthesized | Prefix | Postfix |
| --- | --- | --- | --- |
| `a + b * c` | `(a + (b * c))` | `+a*bc` | `abc*+` |
| `a + b + c - d` | `(((a + b) + c) - d)` | `-++abcd` | `ab+c+d-` |
| `M - (x - y) * t / k` | `(M - (((x - y) * t) / k))` | `-M/*-xytk` | `Mxy-t*k/-` |
| `((v - w) * M) + (L * K) - A` | `((((v - w) * M) + (L * K)) - A)` | `-+*-vwM*LKA` | `vw-M*LK*+A-` |

Check: with a = 1, b = 2, c = 3, `a + b * c` = 1 + 6 = **7** in every notation.

---

## Evaluating infix with two stacks

Example: `30 - 10 * 2 + 50` = **60**

1. **Fully parenthesize:** `((30 - (10 * 2)) + 50)`
2. Use **two stacks**: one for **operands** (numbers), one for **operators**.
3. Scan left to right:
    - **Number** → push onto the operand stack
    - **Operator** → push onto the operator stack
    - **`(`** → ignore
    - **`)`** → pop one operator, pop two operands, compute **(second popped) op (first popped)**, push the result

| Token | Action | Operand stack | Operator stack |
| --- | --- | --- | --- |
| `(` `(` | ignore | [] | [] |
| 30 | push | [30] | [] |
| − | push | [30] | [−] |
| `(` | ignore | [30] | [−] |
| 10 | push | [30, 10] | [−] |
| * | push | [30, 10] | [−, *] |
| 2 | push | [30, 10, 2] | [−, *] |
| `)` | 10 * 2 = 20 | [30, 20] | [−] |
| `)` | 30 − 20 = 10 | [10] | [] |
| + | push | [10] | [+] |
| 50 | push | [10, 50] | [+] |
| `)` | 10 + 50 = 60 | [60] | [] |

The last value on the operand stack is the answer: **60**.

Watch the operand order: the **first** number popped is the **right** operand. That matters for − and /.

Next: evaluating prefix/postfix and the conversion algorithms → [[Data Structures & Algos/Class Notes/Lecture 6|Lecture 6]]
