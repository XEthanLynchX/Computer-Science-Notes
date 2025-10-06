## **Overview**

**Binary Tree DFS (Depth-First Search)** is a traversal technique used to explore nodes and branches of a binary tree deeply before backtracking. It’s commonly implemented using **recursion** or a **stack**, and comes in three main forms: **preorder**, **inorder**, and **postorder**.

This pattern is essential for solving problems involving **tree traversal**, **path finding**, **subtree analysis**, and **recursive tree operations**.

---

## **What Does it Look Like?**

- Recursive or iterative traversal of a binary tree.
- Visits nodes in a specific order:
    - **Preorder**: Root → Left → Right
    - **Inorder**: Left → Root → Right
    - **Postorder**: Left → Right → Root
- Often involves passing state (e.g., path, sum, depth) through recursive calls.

**Visual Example (Inorder):**

```
Tree:       1
           / \
          2   3
         / \
        4   5

Inorder: 4 → 2 → 5 → 1 → 3
```

---

## **How to Recognize This Pattern**

- The problem involves **binary trees** and requires visiting **all nodes**.
- You need to compute something based on **node values**, **paths**, or **structure**.
- Keywords: “traverse”, “path”, “sum”, “recursive”, “visit nodes”, “left/right subtree”.

---

## **Example Implementation**

**Problem:** Inorder Traversal of a Binary Tree
```Python
def inorderTraversal(root):  


def inorderTraversal(root):
    result = []

    def dfs(node):
        if not node:
            return
        dfs(node.left)
        result.append(node.val)
        dfs(node.right)

    dfs(root)
    return result
```

**Key Idea:**

- Use recursion to visit left subtree, then root, then right subtree.
- Can be adapted for preorder or postorder by changing the order of operations.

---

## **Applied Problems**

| Problem Name                            | Link                                                                         | Difficulty | Notes                        |
| --------------------------------------- | ---------------------------------------------------------------------------- | ---------- | ---------------------------- |
| Binary Tree Inorder Traversal           | [LeetCode #94](https://leetcode.com/problems/binary-tree-inorder-traversal/) | Easy       | Classic DFS traversal        |
| Path Sum                                | LeetCode #112                                                                | Easy       | DFS with path tracking       |
| Binary Tree Maximum Path Sum            | LeetCode #124                                                                | Hard       | DFS with global max tracking |
| Lowest Common Ancestor of a Binary Tree | LeetCode #236                                                                | Medium     | DFS with backtracking        |
| Diameter of Binary Tree                 | LeetCode #543                                                                | Easy       | DFS with depth calculation   |