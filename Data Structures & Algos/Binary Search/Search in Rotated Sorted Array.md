---
date: 2025-10-22
Difficulty: Medium
Type: Array / Binary Search
---

## **Problem: 

![[Search in Rotated Sorted Array Problem.png]]

---
## **Note: 
- For the brute force we are checking if it's in array other wise return -1 
- For the optimal solution we have to check which side of the array the mid pointer is in 
- Depending on the which case the mid point is in evaluate target using the m and l/r pointer to see whether to move the l / r pointer to the right or left 

---

## **Time and Space Complexity: 
- Time is O( log n ) - We cut the search time in half in a one pass solution 
- Space is O( 1 ) - We are only storing constants to check 

--- 

## **Brute Force: 

![[Search in Rotated Sorted Array Brute.png]]

---
## **My Solution: 

![[Search in Rotated Sorted Array Solution.png]]



---
## **Optimal Solution: 

![[Search in Rotated Sorted Array.png]]