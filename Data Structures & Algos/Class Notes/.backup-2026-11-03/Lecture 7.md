# Queues 
- Queue is a line of items, waiting to be serverd (by some method/function)
### Ways to implement queues
- Static Array
- Circular Array
- Linked List
### Operations of a queue 
- enqueue() - add element to queue
- dequeue() - Remove element and serve first element
- front() - return a copy of the first element 
- isFull() - check if the queue is full
- isEmpty() - check if the queue is empty

### Static array 
- Every time an item is dequeued we have to shift each element forward
- The shifting of each element is the time consuming operation

### Circular array
- The advantage of circular array implementation is that the elements dont shift rather think of it as the whatever is serving the top of the array moves to them 
- The mod operator is used to overwrite elements at the start of the list

### Linked List 

### Radit / digit sort / bucket sort
- Sorts value on the individual digits that make up the value.
- The number of iteration depends on the number of digits(k) in the largest value in the list 
- Time complexity is **linear** (k * n)
- Create 10 queues 1 for each possible digit
- Looks at each digit starting from the last digit of each number 
- enqueues it in the part 

### Priority Queue
- Based on something other than arrival time 
- For example people arriving to a hospital are treated based on injury severity rather than arrival time. 

### Double ended Queue
- Two queues joined together, but share one source that is serving them 
