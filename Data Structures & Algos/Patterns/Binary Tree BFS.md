## **Overview**

Breadth-First Search (BFS) on a binary tree is a traversal technique that visits nodes **level by level**, starting from the root and moving left to right across each level. It uses a **queue** to keep track of nodes to visit next.

---

## **What Does it Look Like?**

Given this binary tree:

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

A BFS traversal would return:  
**[1, 2, 3, 4, 5, 6]**

Each level is visited before moving to the next.

---

## **How to Recognize This Pattern**

You’re likely dealing with a BFS traversal if:

- The problem mentions **level order traversal**.
- You need to **process nodes by depth** or **group nodes by level**.
- You’re asked to find the **shortest path** in an unweighted tree or graph.

Common keywords: _level order_, _breadth-first_, _queue_, _layers_, _minimum steps_.

---

## **Example Implementation**

Here’s a Python implementation of BFS on a binary tree:


```Python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def bfs_traversal(root):
    if not root:
        return []

    queue = deque([root])
    result = []

    while queue:
        node = queue.popleft()
        result.append(node.val)

        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)

    return result
```

**Example usage:**


```Python
# Constructing the tree:
#         1
#        / \
#       2   3
#      / \   \
#     4   5   6

root = TreeNode(1)
root.left = TreeNode(2, TreeNode(4), TreeNode(5))
root.right = TreeNode(3, None, TreeNode(6))

print(bfs_traversal(root))  # Output: [1, 2, 3, 4, 5, 6]
```

---

## **Applied Problems**

Here are some problems where BFS on binary trees is useful:

- **Binary Tree Level Order Traversal (Leetcode 102)**
- **Minimum Depth of Binary Tree (Leetcode 111)**
- **Populating Next Right Pointers in Each Node (Leetcode 116)**