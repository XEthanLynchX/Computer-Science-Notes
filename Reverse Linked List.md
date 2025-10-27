---
date: 2025-10-24
Difficulty: Easy
Type: Linked List / Recursion
---

## **Problem: 

![[Reverse Linked List Problem.png]]

---
## **Note: 
- First Linked List Problem 
- For linked lists to transverse you have to set a current pointer as the head and use while current (this will do the logic inside until you get to end) which also means you have update current pointer to not get an infinite loop 
- For the Problem we need to keep track of the previous node we visited 
- Use a temp variable to store the next position so once we update the current position we can still travel to it
- Update the what current is pointing to (prev) 
- Then that temp varaible we used is how we'll update the current postion 
- to return the list you have to return the **last element** 
- return looks like this 
- 
``` shell 
`ListNode{val: 5, next: ListNode{val: 4, next: ListNode{val: 3, next: ListNode{val: 2, etc}
```
---

## **Time and Space Complexity: 
- Time is O(n) - 
- Space is O(1) -

--- 

## **Brute Force: 

![[Reverse Linked List Solution.png]]
---
## **My Solution: 




---
## **Optimal Solution: 

