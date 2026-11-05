[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 16: Heap Trees & Priority Queues

## What a heap is

A heap is a **complete binary tree** that satisfies the **heap property**. Both rules must hold at all times.

- **Complete tree:** every level is full except possibly the last, and the last level fills **left to right** with no gaps.
- **Max heap:** every parent is **greater than or equal to** its children, so the **largest value is always at the root**.
- **Min heap (the mirror):** every parent is less than or equal to its children, so the smallest value is at the root.
- **Not a BST:** there is no left < right rule. Siblings can be in any order.
- **Two operations:** insert (the new value goes into the next open leaf) and delete (**only ever the root**).
- **Used for:** heapsort and priority queues.

## Storing a heap in an array

Because the tree is complete, it fits in an array with no gaps: read the tree level by level, left to right. With the root at index 0 (full details in [[Data Structures & Algos/Class Notes/Lecture 15|Lecture 15]]):

|To find|Formula|
|---|---|
|Parent of node i|(i − 1) / 2, rounded down|
|Left child of node i|2i + 1|
|Right child of node i|2i + 2|
|Next open leaf|index n (the current size)|
|Last node|index n − 1|

If your class puts the root at index 1 instead, use parent = i / 2, left = 2i, right = 2i + 1.

## Insertion (upward reheapification)

1. Place the new value in the **next open leaf** (end of the array). The tree stays complete.
2. **Reheapify upward** ("bubble up" / "sift up"): compare the new node with its parent.
3. If the new node is **larger** than its parent, swap them.
4. Repeat from the new position until the node is ≤ its parent or it reaches the root.

At most one swap per level, so one insert costs **O(log₂ n)**.

### Exam example: build a max heap from 10, 3, 50, 35, 10, 100, 40, 200

Insert one value at a time, left to right. The array after each insert:

|Insert|Swaps (bubble up)|Array after|
|---|---|---|
|10|none (root)|[10]|
|3|3 < 10, stop|[10, 3]|
|50|50 > 10, swap|[50, 3, 10]|
|35|35 > 3, swap; 35 < 50, stop|[50, 35, 10, 3]|
|10|10 < 35, stop|[50, 35, 10, 3, 10]|
|100|100 > 10, swap; 100 > 50, swap|[100, 35, 50, 3, 10, 10]|
|40|40 < 50, stop|[100, 35, 50, 3, 10, 10, 40]|
|200|200 > 3, swap; 200 > 35, swap; 200 > 100, swap|[200, 100, 50, 35, 10, 10, 40, 3]|

**Final heap:** [200, 100, 50, 35, 10, 10, 40, 3]

```
            200
          /     \
       100       50
      /   \     /  \
    35    10   10   40
   /
  3
```

On the exam, draw the tree after **every** insert and mark each swap. Duplicates (the two 10s) are allowed and never swap with each other, since equal satisfies ≥.

## Deletion (downward reheapification)

You can only delete the **root** (the max).

1. **Process the root:** save or output its value.
2. **Move the last node** (rightmost leaf on the bottom level) into the root, and remove that leaf. The tree stays complete.
3. **Reheapify downward** ("sift down"): compare the node with its children.
4. If it is smaller than either child, swap it with the **larger** child.
5. Repeat until it is ≥ both children or it becomes a leaf.

Swapping with the larger child matters: swap with the smaller one and the larger child ends up below a smaller parent, breaking the heap. One delete costs **O(log₂ n)**.

### Example: delete from the heap built above

Start: [200, 100, 50, 35, 10, 10, 40, 3]

|Step|Action|Array after|
|---|---|---|
|1|Remove 200; move last node (3) to the root|[3, 100, 50, 35, 10, 10, 40]|
|2|Children 100 and 50; larger is 100; 3 < 100, swap|[100, 3, 50, 35, 10, 10, 40]|
|3|Children 35 and 10; larger is 35; 3 < 35, swap|[100, 35, 50, 3, 10, 10, 40]|
|4|3 is now a leaf, stop|[100, 35, 50, 3, 10, 10, 40]|

```
            100
          /     \
        35       50
      /   \     /  \
     3    10   10   40
```

## Performance

A complete tree with n nodes has height about log₂ n, so any single bubble-up or sift-down is O(log₂ n).

|Operation|Cost|Why|
|---|---|---|
|Insert one node|O(log₂ n)|At most one swap per level|
|Delete the root|O(log₂ n)|At most one swap per level|
|Peek at the max|O(1)|It is always the root|
|Build a heap by n inserts|O(n log₂ n)|n inserts × log₂ n each|
|Empty the heap (n deletes)|O(n log₂ n)|n deletes × log₂ n each|
|Heapsort|O(n log₂ n)|Build + empty|

**Watch out:** inserting **one** node is O(log₂ n), not O(n · log₂ n). The O(n log₂ n) figure is for inserting all n nodes.

Aside: a heap can also be built bottom-up (sift down from the last parent to the root) in O(n). If your professor only taught build-by-insertion, O(n log₂ n) is the expected answer.

## Priority queues and heapsort

**Priority queue:** a queue where the highest-priority item leaves first, not the oldest. A max heap implements it directly, with the root as the highest priority.

- Enqueue = heap insert, O(log₂ n)
- Dequeue = delete root, O(log₂ n)
- Peek = read root, O(1)

**Heapsort:** insert every value into a heap, then delete the root n times. Each delete hands back the next-largest value, so the values come out in descending order. Total cost O(n log₂ n).

## Exam tips

- Always fill the bottom level **left to right**. A gap means it is not a complete tree.
- Insert = bubble **up**. Delete = sift **down**. Don't mix them up.
- On sift down, swap with the **larger** child.
- Delete uses the **last** node (bottom level, rightmost), not the smallest node.
- Equal values don't swap; ≥ already holds.
- Check your answer: every parent ≥ its children, and the max sits at the root.
- Practice: build a heap from 5, 20, 15, 8, 30, 25, then delete the root twice.

### Lecture Notes
![[354E0780-97AE-4E07-84DC-436A1075D27D_1_105_c.jpeg]]
![[BBEBC899-1A07-438D-BC9F-7117F05E2601_1_105_c.jpeg]]
![[25B4E8A7-DC59-450F-8B5D-B373B6F461FD_1_105_c.jpeg]]