[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 13: Tree Terminology & Types

A **tree** is a structure of **nodes** connected by **edges**, with one **root** at the top. Every node except the root has exactly **one parent**, and there are **no cycles**. (Formally: a directed, connected, acyclic graph with a root.)

## Example tree

```
            A          ← root (depth 0)
          /   \
         B     C       ← depth 1
        / \     \
       D   E     F     ← depth 2
                /
               G       ← depth 3
```

## Terminology

| Term | Meaning | In the example |
| --- | --- | --- |
| **Node** | Unit that stores data and links to other nodes | A through G |
| **Edge** | Link between a parent and a child | A–B, C–F, … |
| **Root** | The top node; has no parent | A |
| **Parent** | Node with a direct link down to a child | B is the parent of D and E |
| **Child** | Node directly below a parent | F is a child of C |
| **Siblings** | Nodes with the same parent | D and E |
| **Path** | Sequence of nodes and edges from one node to another | A → C → F → G |
| **Leaf (external) node** | Node with **zero** children | D, E, G |
| **Non-leaf (internal) node** | Node with **at least one** child | A, B, C, F |
| **Subtree** | A node plus all of its descendants | C, F, G |

## Node properties

| Property | Definition | Example |
| --- | --- | --- |
| **Degree** | Number of children | deg(B) = 2, deg(C) = 1, deg(G) = 0 |
| **Depth** | Edges on the path **from the root down** to the node | depth(E) = 2 |
| **Height** | Edges on the **longest** path **from the node down** to a leaf | height(C) = 2 (C→F→G), height(G) = 0 |
| **Ancestors** | All nodes on the path from the node **up to the root** | ancestors(G) = F, C, A |
| **Descendants** | All nodes **below** it (its whole subtree) | descendants(C) = F, G |

Depth counts **down from the root**; height counts **down to the deepest leaf**.

## Tree properties

| Property | Definition | Example |
| --- | --- | --- |
| **Tree degree** | The **maximum** node degree in the tree | 2 |
| **Tree height** | Height of the root = longest root-to-leaf path | 3 |
| **Tree depth** | Depth of the deepest leaf (**same value as height**) | 3 |

## Tree types

| Type | Rule |
| --- | --- |
| **General tree** | Any node can have any number of children (degree ≥ 0) |
| **Binary tree** | Every node has **at most 2** children (degree ≤ 2): a left and a right |

## Binary tree types

### 1. Full tree

Every node has **either 0 or 2** children. No node has exactly one child.

```
      A
     / \
    B   C
       / \
      D   E
```

### 2. Complete tree

Every level is **completely filled** except possibly the last, and the last level is filled **left to right** with no gaps.

```
Complete:          Not complete (gap on the left):
      A                  A
     / \                / \
    B   C              B   C
   / \                  \
  D   E                  E
```

**Capacity of a binary tree of depth d** (root at depth 0):

| | Formula | d = 3 |
| --- | --- | --- |
| Max nodes (every level full) | $2^{d+1} - 1$ | 15 |
| Min nodes in a **complete** tree | $2^d$ | 8 |
| Max nodes on level k | $2^k$ | level 3 holds up to 8 |

Use these to find how deep a tree must be to hold n values. Example: 10 values → depth 3 (since 8 ≤ 10 ≤ 15).

### 3. Height-balanced tree

For **every** node, the heights of its left and right subtrees differ by **at most 1**:

**| leftHeight − rightHeight | ≤ 1**

The example tree at the top is **not** balanced: at node C, the left subtree is empty (height −1) and the right has height 1, a difference of 2.

### 4. Binary Search Tree (BST)

An **ordered** binary tree: everything in a node's **left** subtree is **smaller**, everything in its **right** subtree is **larger**. Lets you discard half the tree at each step when searching. → [[Data Structures & Algos/Class Notes/Lecture 14|Lecture 14]]

### 5. AVL tree (self-balancing BST)

A **BST that is also height-balanced**. After each insert or delete, it **rotates** nodes to restore balance, so operations stay **O(log n)**.

### 6. Heap tree

A **complete tree** where every **parent ≥ its children** (max heap), so the largest value is always the root. → [[Data Structures & Algos/Class Notes/Lecture 16|Lecture 16]]

## Coming up

| Topic | Notes |
| --- | --- |
| Binary Search Trees: insert, search, delete | [[Data Structures & Algos/Class Notes/Lecture 14\|Lecture 14]] |
| Array representation & traversals (pre/in/post/level-order) | [[Data Structures & Algos/Class Notes/Lecture 15\|Lecture 15]] |
| Heap trees & priority queues | [[Data Structures & Algos/Class Notes/Lecture 16\|Lecture 16]] |

## Self-Check

1. In the example tree, what are the depth and height of B?
2. Draw a tree that is **complete** but **not full**.
3. What is the maximum number of nodes in a binary tree of depth 4?
