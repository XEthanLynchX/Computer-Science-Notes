# Trees: A **directed** graph with a root node (a structure of nodes and edges) 
## Tree terminology
- **Node:** A basic structural unit that stores data and connects to other nodes.
    
- **Child Node:** A node directly connected to and descending from a parent node.
    
- **Edge:** The logical link connecting two specific nodes.
    
- **Parent Node:** A node that maintains direct connections to one or more descending child nodes.
    
- **Path:** A sequence of nodes and connecting edges leading from a starting node to a destination node.
    
- **Leaf Node (External):** A node that has exactly zero child nodes. You reach it at the absolute end of a branch.
    
- **Non-Leaf Node (Internal):** A node that has at least one child node.

## Properties
### Nodes Propterties
1. Node degree - number of children nodes 
2. Node depth - How far it is down relative to the root node (count edges down from root)
3. Node height - How far up it is from the lowest reachable leaf node (count edges up from lowest level node)
4. Ancestors - A set of nodes on the path from the node to the root
5. Descendents - A set of nodes on the path from current node to lowest level reachable (it's children)
### **Tree Properties**
Tree degree - The number of children from the root = max {All Node degrees}
Tree depth - Path length from the root to the lowest reachable leaf node (same value as height)
Tree height - Path length from the root to the lowest reachable leaf node (same value as depth)
### Tree types
**General Tree** - A tree with a degree >= 0 
**Binary Tree** - A tree with a degree <= 2
### Binary Tree Types
1. **Full Tree** (degree of non-leaf node = 2) - Every node that has nodes connected has to have at least 2 non-leaf nodes
2. **Complete Tree** - A tree that is filled top down; left to right
	- Has Properties of MaxCapacity and MinCapacity and you use these and the math to see how big of a tree you need to create to store a certain amount of values
	- MaxCapacity = 2^(d+1) - 1 
	- MinCapacity = 2^d
3. **Height-Balanced Tree** - A tree that height is balanced on the left and right side and has a maximum difference than 1
	 - For each Node : ( | LeftHeight - RightHeight | <= 1 )  
4. **Binary Search Tree** - Ordered Tree (This makes it so you can split the search by half )
5. **AVC (Self-balancing search tree)** - A binary tree and height balancing tree combined (binary search tree that keeps itself balanced)
6. **Heap Tree** - Complete Tree + The parent should always be >= child nodes (largest value is the root always)

## Tree capacity

## Tree Implementation 

## Tree transversal algorithmns 

## Binary Search Tree (BST)

## AVL Tree ()

## Application of binary X BST 

## Heap Tree 

