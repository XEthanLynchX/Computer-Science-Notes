# Abstract Data Type (ADT) 
- Idea, logical view, conceptual view 
- Description of set of data elements and methods to manipulate the data 
- Methods are described in terms of pre condition and post condition 
- **Precondition** the status of the elements before method is executed  
- **Postcondition** the status of the elements after the method is executed and how did it change

# Data Structure 
* A data structure is a way of organizing and storing data in a computer so that it can be accessed and used efficiently. It refers to the logical or mathematical representation of data, as well as the implementation in a computer program.
- Implementation 
- Physical view
- Implementation (Data Structure)

# Memory allocation 
- **Declaring** space in memory is creating a space in memory WITHOUT giving it a value 
- **Initializing** in memory means declaring space in memory then giving it a value
```java

//Example 
//This where we declare space in memory
int [] A; 
//Here is where we allocate space in memory 
A = new Int[5]

```
### Single space (int x; int y = 10)

### Continguous space (int[] A = new Int [5])
- When allocating space in memory first the compiler will assign an address to the array itself then it will create an **access formula** to access the different elements of the array
- The access formula is as given F(i)= Base Address + (i * length of an element)  
## Linked space ( Linked list)
- Each node is an object and has a value associated along with a an pointer to the next node 
```java 
Node temp = new Node(20)
// n1 becomes an access point to the head of the linked list
n1.next = temp 
// The temp link to the linked list is removed when set to null
temp = null
//n2 becomes an allias to access n1 
n2 = n1 
// Inaccessible Space (there is no longer a way to access the linked list) 
n1 = temp 
n2 temp 

```
- A linked list is a recursive structure 
- To transverse through a linked list use While loops (using a tmp variable | Use While (tmp != null))
- ```java
  Node L = New Node (4);
  Node temp;
  temp = L 
  While(temp != null){ 
	  System.out.print(temp.data)
	  temp.next
  }
  ```
## Method to find x in linked list 
```java
Public static boolean Find(Linked List L, String target){ 
	if(L == null){ 
	return False
	} else if(L.next == null){ 
		if (L == target){ 
		return True 
		} else {
		return False
		}
	}else{ 
	temp = L 
	while (temp != null){ 
		if (temp = target ){ 
		return true 
		}
		temp.next() 
	}
	return False
	}

}
```