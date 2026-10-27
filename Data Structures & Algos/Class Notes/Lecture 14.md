# Binary Search Tree (BST) Notes

**BST rule:** for every node, everything in its **left** subtree is **smaller**, and everything in its **right** subtree is **larger**.

---

## 1. Insert a Value ⭐ _(on the final)_

**How it works:**

1. Start at the root.
2. Compare the new value to the current node:
    - **Smaller** → go **left**
    - **Larger** → go **right**
3. Repeat until you hit an empty spot, then insert the value there.

New values are always inserted as a **leaf** (a node with no children).

**Example:** insert 50, 10, 60, 35, 70, 25, 11 in order

|Step|Insert|Path taken|Placed as|
|---|---|---|---|
|1|50|(empty tree)|root|
|2|10|10 < 50 → left|left child of 50|
|3|60|60 > 50 → right|right child of 50|
|4|35|< 50 → left, > 10 → right|right child of 10|
|5|70|> 50 → right, > 60 → right|right child of 60|
|6|25|< 50 → left, > 10 → right, < 35 → left|left child of 35|
|7|11|< 50, > 10, < 35, < 25 → left|left child of 25|

**After step 1:**

```
  50
```

**After step 2:**

```
    50
   /
 10
```

**After step 7 (final tree):**

```
        50
       /  \
     10    60
       \     \
       35     70
      /
    25
   /
  11
```

---

## 2. Search for a Value

**How it works:** use the same comparisons as insert.

1. Start at the root.
2. If the value **equals** the current node, it's **found**.
3. If it's **smaller**, go **left**. If it's **larger**, go **right**.
4. If you reach an empty spot (null), the value is **not in the tree**.

**Example 1: search for 25** (it's in the tree)

- 25 < 50 → go left to 10
- 25 > 10 → go right to 35
- 25 < 35 → go left to 25
- 25 = 25 → **found** (4 comparisons)

**Example 2: search for 40** (it's not in the tree)

- 40 < 50 → go left to 10
- 40 > 10 → go right to 35
- 40 > 35 → go right to null
- **Not found**

Each comparison discards one whole subtree, which is why searching is fast.

---

## 3. Delete a Value

First **search** for the node. Then handle one of 3 cases.

### Case 1: The node is a leaf (no children)

Remove it.

**Delete 11:** 11 has no children, so remove it.

```
        50
       /  \
     10    60
       \     \
       35     70
      /
    25
```

### Case 2: The node has one child

Replace the node with its child. The child's whole subtree moves up with it.

**Delete 35 (from the original tree):** 35 has one child (25), so 25 moves up into 35's spot.

```
        50
       /  \
     10    60
       \     \
       25     70
      /
    11
```

### Case 3: The node has two children

1. Find the **inorder successor**, which is the **smallest value in the right subtree**: go right once, then go left as far as you can.
2. Copy the successor's value into the node you're deleting.
3. Delete the successor from its old spot. It will always be Case 1 or Case 2.

_(Alternative: use the **inorder predecessor** instead, which is the **largest value in the left subtree**: go left once, then right as far as you can. Check which one your professor uses.)_

**Delete 50 (from the original tree):**

- Successor: go right to 60, then try left. 60 has no left child, so **60** is the successor.
- Copy 60 into the root.
- Delete the old 60. It has one child (70), so this is Case 2 and 70 moves up.

```
        60
       /  \
     10    70
       \
       35
      /
    25
   /
  11
```

_(Using the predecessor instead, 35 would replace 50.)_

---

## Time Complexity (all 3 operations)

All three operations follow a single path from the root downward, so each one is **O(h)**, where h is the height of the tree.

|Tree shape|Height|Time|
|---|---|---|
|Balanced|log n|**O(log n)**|
|Skewed (looks like a linked list)|n|**O(n)** worst case|

Inserting values in sorted order (for example 10, 20, 30, 40) produces the worst case: a tree with every node on one side.