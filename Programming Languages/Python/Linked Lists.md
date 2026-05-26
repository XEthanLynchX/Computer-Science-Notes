### Singly Linked List
--- 
**Singly Linked Lists are contained of nodes and transversal is one directional , these nodes have two key properties. 

### Doubly Linked Lists 
---
**A Doubly Linked List are contained of nodes as well however these nodes have a value, previous pointer, and a next pointer 

### Circular Linked Lists
--- 



## Node(s) 
--- 
**A Node is defined with data and a next pointer which points to the next node in the linked list
```python 
class SinglyLinkedList:
	def __init__(self, val = 0, next = None): 
		self.val = val 
		self.next = next
```

```python
class DoublyLinkedList: 
	def __init__(self, val = 0, prev = None, next = None): 
		self.val = val
		self.prev = prev
		self.next = next
```

Note: For a circular linked list these can work with either a singly or doubly linked list, the last node just has to point to the head. 

## Why Linked Lists 
- **Linked Lists can have O(1) insert and removal (if you already have a ref to the node) allowing for quick access time 
- **Linked Lists can grow and shrink very easily 


## When Not to use Linked Lists 
- **They have no random access (Must transverse through the whole linked list)
- **Extra memory for storing pointers
- **Slower access compared to arrays 

## Time Complexity 
|Operation| Complexity |
|---|---|
|Access| O(n)       |
|Search| O(n)       |
|Insertion (head)| O(1)       |
|Deletion (head)| O(1)       
