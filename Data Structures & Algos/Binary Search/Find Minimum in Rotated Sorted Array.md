---
date: 2025-10-20
Difficulty: Medium
Type: Array / Binary Search
---

## **Problem: 

![[Find Minimum in Rotated Sorted Array Problem.png]]

---
## **Note: 
- Rather than returning the middle return the left then if when you move the left to middle, don't make it middle rather make it mid + 1 
- Make sure the loop is less than NOT <= because in that case it will eval an extra time and give the wrong answer 


---

## **Time and Space Complexity: 
- Time is O( log n) - Because we are splitting our search time in half 
- Space is O(1) - the only thing we are storing are our pointers which constantly consume the same memory 

--- 

## **Brute Force: 

![[Find Minimum in Rotated Sorted Array Brute.png]]
---
## **My Solution: 

![[Find Minimum in Rotated Sorted Array optimal.png]]

---
## **Optimal Solution: 

![[Find Minimum in Rotated Sorted Array.png]]