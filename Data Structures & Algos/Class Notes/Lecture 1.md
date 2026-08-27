# Recursion 
### What is recursion? 
- Recursion is a complex problem that is broken down into smaller chunks until the problem is solved. 
- Recursion MUST have a **Base Case and Recursive Case** 
- An example of where recursion would be useful is in a problem such as 
Find the sum of n where n = 1 + 2 + 3 + 4 + 5 + 6 + ... + n 
```java
// return 1 if n = 1 
// return (n + sum(n-1))
```

Using Recursion with factorials 
```java 
// Fact (n) = {return 1 if n = 1 and return (n * Fact(n-1)) when n > 1}
```

### Activation Record 
- is the block of memory allocated for a single invocation of a function or procedure. It holds everything that call needs to run and to return properly.
![[63FE6493-EEEB-4D37-96C6-9B5ECBF71641_1_102_o.jpeg|429]]
### Runtime Stack 
- The runtime stack executes different Activation Records. Each Activation record that is currently being executed has **execution control** then once it finishes it passes it to the next record. 
- The activation record being executed is always at the top of the stack and then when it's no longer being used it is popped from the stack 
![[1C79FD5E-A354-469F-B669-D6DB3580EF06.jpeg|346]]

### When is Recursion used
- Trees are used when a problem has recursive characteristics ( e.g. Trees ) 
- This is an example problem of recursion being where the base case is there is no more commas after the number. 
- ![[Recursion Example.png]]