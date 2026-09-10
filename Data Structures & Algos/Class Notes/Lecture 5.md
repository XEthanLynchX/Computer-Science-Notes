## Stack ADT 
- A container where you can manipulate only the top element 
### Stack Methods
- push(e) - Adds an element to a stack 
- pop() - removes the top element from the stack 
- top() / peek() - returns the top element of the stack (doesn't remove)
- isFull() - True if stack is full 
- isEmpty() - checks if there are any elements in the stack returns true if there are none
- size() - return the # of elements in the stack
## Implementation 
### Using Array
- when an item is inserted into an array (as a stack) it will be inserted initially at 0 and then 1 , 2, etc
- The top element is the element at the highest index
```java
public class ArrayStackExample {
    public static void main(String[] args) {
        int[] stack = new int[6];  // fixed-size array, capacity 6
        int top = -1;              // -1 means empty

        for (int i = 0; i <= 5; i++) {
            top++;
            stack[top] = i;
        }

        System.out.print("Stack contents (bottom to top): ");
        for (int i = 0; i <= top; i++) {
            System.out.print(stack[i] + " ");
        }
        System.out.println();
        // Output: 0 1 2 3 4 5
        // stack[0] = 0 (bottom), stack[5] = 5 (top)
        System.out.println("Top index is: " + top);
	    }
	}
}
```

```java
public class ArrayStackExample {
    public static void main(String[] args) {
        int[] stack = new int[6];
        int top = stack.length;

        for (int i = stack.length; i >= 0; i++) {
            top--;
            stack[top] = i;
        }

        System.out.println("Popping order:");
        while (top >= 0) {
            int popped = stack[top];
            top--;
            System.out.println(popped);
        }
        // Output: 5, 4, 3, 2, 1, 0
    }
}
```
### Using Linked List 
- Array is the better an more efficient way to implement a stack 

![[BCCA247F-BC0A-4B51-93AA-7C0049D7FE14.jpeg]]
## Stack Application (Polish Notation) : 
### What we'll use this for
- Convert manually infix expression to prefix and postfix notation 
- Evaluate the value of (infix, prefix, and postfix expressions ) - using stacks
- Algorithmically convert infix to pre/postfix notation
### What is Prefix, Postfix, and Infix
**These are all just different name for where the operator is in the expression**
- Prefix - x = +ab 
- Postfix - x = ab+
- Infix - x = a + b

### Manual Conversion 
```python 
# Examples

a,b,c = 1,2,3
x = a + b * c
# Output is 7 

# Prefix version 
x = +a*bc

# Postfix
x = abc*+

# Example 2 
x = a + b + c - d

# Prefix 
x = -++abcd

# Postfix 
x = ab+c+d-

# Example 3
x = M - (x-y) * t / k

# Prefix
 x = -m/*-xytk
 
# Postfix
x = Mxy-t*k/-

# Example 3 
x = ((v-w) * M) + (L*K) - A

#Prefix 
x = -+*-vwm*lka

# Postfix
x = vw-m*lk*+a-


```


## Evaluating Infix expressions using Stacks 
- x = 30 - 10 * 2 + 50 = (60)
- So first you parenthesis the expression (based on order of operations)
- You need to two stacks | One for the operators | One for the Operands(numbers)
- When you reach a right parenthesis you apply the top operator of the operator stack(pop() the top of operator stack) to the top 2 numbers of the operand stack (pop() the two numbers of operands stack into variables and push the new number you got )