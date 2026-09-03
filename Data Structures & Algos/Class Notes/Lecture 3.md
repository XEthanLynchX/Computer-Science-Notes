### Inserting a node into Linked List
|10| -> |20| -> |30| -> |40| 
		   ^
		 |25| 
- When inserting a new node into this list you need to update the new node to point to the node after (in this case 30) then update the previous node to point to the new node ; otherwise if you do it the opposite way you WON'T have access to the rest of the linked list (iterating with temp pointer)
```java 
//advance until we find node we're looking for
Node newNode = new Node (25)
Node temp = L 

while(temp.data != 20){ 
	temp = temp.next
}
//set newNode pointer to the next node
newNode.next = temp.next
//set current node to point at newNode
temp.next = newNode

//always set temp pointer(s) to null after so they can't affect the list after the operation is done
temp = null 
newNode = null
```


### Removing a Node from a Linked List 
```java
Node temp = L 
//iterate until we find the node before the one we want to delete
while(temp.data != 20){ 
	temp = temp.next
}
// temp2 is the node we want to delete
Node temp2 = temp.next
// assign the pointer to the next node AFTER the one we are trying to delete
temp.next = temp2.next
// set the node's pointer we are deleting to null so it's no longer connected to the chain
temp2.next = null

temp = null 
temp2 = null 
```

### About Node's 
When creating a node class the element should be a generic so you can pass any data type to it and the next value should always start out as null 

## Remember always check if the list is empty (Head node exists)