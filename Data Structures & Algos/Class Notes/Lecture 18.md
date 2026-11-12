# AVL Tree: 
- AVL is a self-balancing BST 
- AVL is always blanced before node insertion / deletion 
- Each Node has a balance factor BF = LH - RH 
## What throws a tree out of balance 
1. Left-Left Imbalance
	- Solution: Single Right Rotation on X
2. Right-Right Imbalance
	- Solution: Single left rotation on X
3. Left-Right Imbalance 
	- Solution: Single left Rotation on y followed by single right rotation on X
4. Right-Left Imbalance
	- Solution: Single right rotation on y followed by single left rotation on X

Note: The Z pointer is 
## Terminology: 
```
{x: pointer to critical node }
{y: pointer to the child of x on the heavy side}
{z: pointer to the child of y on the side of x}
```

**Drawing out the balance steps of an AVL Tree is commonly a question on the final**
### Lecture Notes:

