[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 3: Linked List Insertion & Deletion

## The Node class

```java
public class Node<T> {
    T data;          // generic, so the list can hold any type
    Node<T> next;    // starts as null

    public Node(T data) {
        this.data = data;
        this.next = null;
    }
}
```

- Make `data` **generic** (`<T>`) so you can store any data type.
- `next` should always **start as `null`**.

> **Always check if the list is empty first** (`head == null`) before walking it, or `temp.next` throws a `NullPointerException`.

## Inserting a node in the middle

Insert 25 after 20:

```
Before:  [10] -> [20] -> [30] -> [40]
                     \
                     [25]
After:   [10] -> [20] -> [25] -> [30] -> [40]
```

**Order matters:**

1. Point the **new node** at the node after the spot: `newNode.next = temp.next` (25 → 30)
2. Point the **previous node** at the new node: `temp.next = newNode` (20 → 25)

If you do step 2 first, 20 stops pointing at 30 and you **lose the rest of the list**.

```java
Node newNode = new Node(25);
Node temp = head;

// walk to the node we insert AFTER
while (temp != null && !temp.data.equals(20)) {
    temp = temp.next;
}

if (temp != null) {               // only insert if 20 was found
    newNode.next = temp.next;     // 1. new node -> 30
    temp.next = newNode;          // 2. 20 -> new node
}

temp = null;      // class convention: clear temp pointers when done
newNode = null;
```

## Inserting at the head

The head has no node before it, so it's a special case:

```java
Node newNode = new Node(5);
newNode.next = head;   // 5 -> old head
head = newNode;        // head is now 5
```

## Removing a node from the middle

Delete 25 (the node after 20):

```
Before:  [10] -> [20] -> [25] -> [30]
After:   [10] -> [20] -> [30]          [25] (disconnected)
```

1. Walk to the node **before** the one you're deleting.
2. Point it **past** the deleted node.
3. Disconnect the deleted node.

```java
Node temp = head;

// walk to the node BEFORE the one we delete
while (temp.next != null && !temp.next.data.equals(25)) {
    temp = temp.next;
}

if (temp.next != null) {           // only delete if 25 was found
    Node temp2 = temp.next;        // temp2 = node to delete (25)
    temp.next = temp2.next;        // 20 -> 30 (skip over 25)
    temp2.next = null;             // disconnect 25 from the chain
    temp2 = null;
}
temp = null;
```

## Removing the head

```java
if (head != null) {
    Node old = head;
    head = head.next;    // second node becomes the head
    old.next = null;
}
```

## Cost

| Operation | Time | Why |
| --- | --- | --- |
| Insert/delete at head | **O(1)** | No walking needed |
| Insert/delete after a given node | **O(1)** | Just re-point two references |
| Find the spot first, then insert/delete | **O(n)** | Walking the list dominates |

## Edge-case checklist

- Empty list (`head == null`)
- Inserting or deleting at the **head**
- Deleting the **last** node (its `next` is already `null`, which works with the code above)
- Target value **not in the list**

Next: stacks → [[Data Structures & Algos/Class Notes/Lecture 5|Lecture 5]]
