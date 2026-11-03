[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 2: ADTs, Data Structures & Memory Allocation

## Abstract Data Type (ADT)

An **ADT** is the **logical / conceptual view** of data: *what* it stores and *what operations* you can do, with no detail about *how* it's built.

- Describes a **set of data elements** and the **methods** that manipulate them.
- Each method is described with a **precondition** and a **postcondition**:

| Term | Meaning | Example: `pop()` on a stack |
| --- | --- | --- |
| **Precondition** | What must be true **before** the method runs | The stack is not empty |
| **Postcondition** | What is true **after** it runs, and how the data changed | The top element is removed and returned; size went down by 1 |

## Data Structure

A **data structure** is the **implementation** (physical view) of an ADT: the actual way data is organized in memory so it can be accessed and used efficiently.

| | ADT | Data Structure |
| --- | --- | --- |
| View | Logical / conceptual | Physical / implementation |
| Answers | *What* can it do? | *How* is it done? |
| Example | Stack (push, pop, peek) | Stack built with an array, or with a linked list |

One ADT can have many data structures behind it.

## Memory Allocation

| Term | Meaning | Java |
| --- | --- | --- |
| **Declare** | Create a name (reference) without a value or space for the data | `int[] A;` |
| **Allocate** | Reserve space in memory for the data | `A = new int[5];` |
| **Initialize** | Give the space its first value | `int y = 10;` or `A[0] = 7;` |

```java
int[] A;           // declare: A exists but points to nothing yet
A = new int[5];    // allocate: space for 5 ints (all start at 0 in Java)
A[0] = 7;          // initialize an element
```

There are three kinds of memory space:

### 1. Single space

One variable, one value: `int x;` or `int y = 10;`

### 2. Contiguous space (arrays)

`int[] A = new int[5];`

- The elements sit **side by side** in one block of memory.
- The array gets one **base address** (where element 0 starts).
- Any element is found with the **access formula**:

**Address of A[i] = Base Address + (i × size of one element)**

Example: base address = 1000, `int` = 4 bytes → A[3] is at 1000 + (3 × 4) = **1012**.

This is why array access by index is **O(1)**: it's one calculation, no searching.

### 3. Linked space (linked lists)

- Each **node** is a separate object holding a **value** and a **reference (pointer) to the next node**.
- Nodes can be anywhere in memory; the `next` references chain them together.
- The last node's `next` is `null`.
- A linked list is a **recursive structure**: a node followed by a (smaller) linked list.

```
head
 |
[10 | •]--> [20 | •]--> [30 | null]
```

## Linked list references in code

```java
Node n1 = new Node(10);   // n1 is the head (access point to the list)
Node temp = new Node(20);
n1.next = temp;           // link: 10 -> 20
temp = null;              // temp no longer points to 20, but n1.next still does

Node n2 = n1;             // n2 is an ALIAS: a second reference to the same node
n1 = null;                // list still reachable through n2
n2 = null;                // now NOTHING points to the list → inaccessible
```

Once no reference points to a node, it's **inaccessible** (garbage); Java's garbage collector reclaims it. Losing your only reference to the head loses the whole list.

## Traversing a linked list

Use a temporary pointer and a `while` loop that stops at `null`:

```java
Node temp = head;
while (temp != null) {
    System.out.print(temp.data + " ");
    temp = temp.next;       // move forward (don't forget this, or it loops forever)
}
```

Traversal is **O(n)**: unlike an array, you can't jump to element i.

## Searching a linked list for a target

```java
public static boolean find(Node head, String target) {
    Node temp = head;
    while (temp != null) {                 // also handles an empty list (head == null)
        if (temp.data.equals(target)) {    // compare the DATA, and use .equals for Strings
            return true;
        }
        temp = temp.next;
    }
    return false;
}
```

Common mistakes fixed here:

- Compare `temp.data` to the target, not the node itself.
- Use `.equals()` for Strings, not `==` (and never `=`, which assigns).
- Advance with `temp = temp.next;` (just `temp.next` does nothing).
- No special case is needed for a one-node list; the loop handles it.

## Array vs. Linked List

| | Array (contiguous) | Linked list (linked) |
| --- | --- | --- |
| Access element i | **O(1)** via access formula | O(n), must walk from head |
| Insert/delete in middle | O(n), shift elements | **O(1)** once you're at the spot |
| Size | Fixed when allocated | Grows and shrinks as needed |
| Extra memory | None | One `next` reference per node |

Next: inserting and removing nodes → [[Data Structures & Algos/Class Notes/Lecture 3|Lecture 3]]
