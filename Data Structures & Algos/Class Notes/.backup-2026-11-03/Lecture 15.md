# Array Representation of a Binary Tree

Works for **complete binary trees** (every level full except possibly the last, which fills left to right), which is why it's the standard way to store **heaps**. Nodes are stored in **level order**: top to bottom, left to right.

## Index formulas (0-indexed)

For a node at index `i` in an array of size `n`:

|Relationship|Formula|Valid when|
|---|---|---|
|Left child|`2i + 1`|`2i + 1 < n`|
|Right child|`2i + 2`|`2i + 2 < n`|
|Parent|`⌊(i − 1) / 2⌋`|`i > 0` (the root has no parent)|

These formulas go both ways. Children formulas move **down** the tree and the parent formula moves **up**. You use both when converting in either direction, and during heap operations (sift down uses the child formulas, sift up uses the parent formula).

## Example

```
Array:  [A, B, C, D, E, F]
Index:   0  1  2  3  4  5

        A(0)
       /    \
     B(1)   C(2)
    /   \    /
  D(3) E(4) F(5)
```

- Children of B (i=1): 2(1)+1 = **3 (D)**, 2(1)+2 = **4 (E)**
- Children of C (i=2): **5 (F)**; index 6 ≥ n, so there's no right child
- Parent of E (i=4): ⌊(4−1)/2⌋ = **1 (B)**

## Other useful facts

- **Leaves** are at indices `⌊n/2⌋` to `n − 1`.
- **Last non-leaf node** is at `⌊n/2⌋ − 1`. Build-heap starts here and works backward to 0.
- **Height** is `⌊log₂ n⌋`.
- **1-indexed variant** (root at index 1): left = `2i`, right = `2i + 1`, parent = `⌊i/2⌋`. Some textbooks use this, so check which convention your class uses.
- **Non-complete trees** leave gaps in the array, which need null placeholders. That wastes space, so linked nodes are usually better for those.

Quick heads-up: VLR, LVR, and LRV are **binary tree traversals** (preorder, inorder, postorder), not binary search. They connect to binary _search trees_ through inorder, though, so these notes use a BST as the example.

# Binary Tree Traversals (Depth-First)

**V** = visit the node, **L** = go left, **R** = go right. Left always comes before right; the only difference is **where V goes**.

|Name|Order|Memory trick|
|---|---|---|
|**Preorder**|V L R|Visit _before_ children|
|**Inorder**|L V R|Visit _in between_ children|
|**Postorder**|L R V|Visit _after_ children|

## Example tree (a BST)

```
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13
```

|Traversal|Result|
|---|---|
|Preorder (VLR)|8, 3, 1, 6, 4, 7, 10, 14, 13|
|Inorder (LVR)|1, 3, 4, 6, 7, 8, 10, 13, 14|
|Postorder (LRV)|1, 4, 7, 6, 3, 13, 14, 10, 8|

**Patterns to notice:**

- Preorder always **starts with the root** (8).
- Postorder always **ends with the root** (8).
- Inorder on a BST comes out **sorted**. That's the key link to binary search trees.

## Code (recursive)

```java
void preorder(Node n) {
    if (n == null) return;
    visit(n);              // V
    preorder(n.left);      // L
    preorder(n.right);     // R
}

void inorder(Node n) {
    if (n == null) return;
    inorder(n.left);       // L
    visit(n);              // V
    inorder(n.right);      // R
}

void postorder(Node n) {
    if (n == null) return;
    postorder(n.left);     // L
    postorder(n.right);    // R
    visit(n);              // V
}
```

All three are **O(n) time**, and **O(h) space** for the recursion stack, where h is the tree height.

## Second example: expression tree

The expression `(3 + 4) * 2` as a tree:

```
      *
     / \
    +   2
   / \
  3   4
```

|Traversal|Result|Notation|
|---|---|---|
|Preorder|`* + 3 4 2`|Prefix (Polish)|
|Inorder|`3 + 4 * 2`|Infix (needs parentheses to keep its meaning)|
|Postorder|`3 4 + 2 *`|Postfix (Reverse Polish)|

## When to use each

- **Preorder:** copying or serializing a tree, since the parent is created before its children. Also prefix expressions.
- **Inorder:** getting a BST's values in sorted order, or checking whether a tree is a valid BST.
- **Postorder:** deleting or freeing a tree, since children go before the parent. Also evaluating expression trees and computing height or size, because a node needs its children's results first.

## Tracing trick

Walk around the outside of the tree counterclockwise, starting on the root's left side, hugging each node.

- **Preorder:** record a node when you pass its **left** side.
- **Inorder:** record it when you pass **underneath**.
- **Postorder:** record it when you pass its **right** side.

## Extra facts

- Rebuilding a unique tree requires **inorder plus either preorder or postorder**. Preorder and postorder together aren't enough in general.
- **Level-order** (breadth-first) is the fourth common traversal. It uses a queue instead of recursion and matches the array layout from your last notes.

**Try it:** write all three traversals for this tree, and I'll check your answers.

```
      5
     / \
    2   9
     \  /
     3 7
```

## Breadth-First Traversal (Level-Order)

This visits nodes **level by level**, top to bottom and left to right within each level. Instead of recursion, it uses a **queue** (first in, first out), so nodes come out in the same order they were discovered.

**Example tree:**

```
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13
```

**Level-order:** 8, 3, 10, 1, 6, 14, 4, 7, 13

To trace it by hand, read the tree row by row like a book:

|Level|Nodes|
|---|---|
|0|8|
|1|3, 10|
|2|1, 6, 14|
|3|4, 7, 13|

**Expression tree** `(3 + 4) * 2`: `*, +, 2, 3, 4`

### How the queue works

1. Put the root in the queue.
2. While the queue isn't empty, remove the front node, visit it, and add its left and then right child (if they exist) to the back.

Trace on the example:

|Step|Remove & visit|Add|Queue after|
|---|---|---|---|
|1|8|3, 10|[3, 10]|
|2|3|1, 6|[10, 1, 6]|
|3|10|14|[1, 6, 14]|
|4|1|none|[6, 14]|
|5|6|4, 7|[14, 4, 7]|
|6|14|13|[4, 7, 13]|
|7–9|4, 7, 13|none|[]|

### Code

```java
void levelOrder(Node root) {
    if (root == null) return;
    Queue<Node> queue = new LinkedList<>();
    queue.add(root);

    while (!queue.isEmpty()) {
        Node n = queue.remove();
        visit(n);
        if (n.left != null)  queue.add(n.left);
        if (n.right != null) queue.add(n.right);
    }
}
```

**Level-by-level variant:** to know where each level ends (for example, to print each level on its own line), record `queue.size()` at the start of each loop and process exactly that many nodes before moving on.

```java
while (!queue.isEmpty()) {
    int levelSize = queue.size();
    for (int i = 0; i < levelSize; i++) {
        Node n = queue.remove();
        visit(n);
        if (n.left != null)  queue.add(n.left);
        if (n.right != null) queue.add(n.right);
    }
    // one full level done here
}
```

**Complexity:** O(n) time. O(w) space, where w is the tree's maximum width; the bottom level of a complete tree holds about n/2 nodes.

### When to use it

- Processing or printing a tree **level by level**.
- Finding the **minimum depth**, or the shortest path in an unweighted tree or graph. BFS reaches closer nodes first, so it can stop early.
- Finding the leftmost or rightmost node of each level (the "right side view" problem).
- It matches the **array layout** from your heap notes: level-order of a complete tree gives you exactly the array.

### DFS vs. BFS at a glance

||DFS (pre/in/post)|BFS (level-order)|
|---|---|---|
|Data structure|Stack (or recursion)|Queue|
|Explores|Deep first, then backtracks|Wide first, level by level|
|Extra space|O(h), the height|O(w), the width|

For the practice tree from before, you can add level-order to the three you're writing out.