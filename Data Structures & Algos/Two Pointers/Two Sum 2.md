---
date: 2025-10-08
Difficulty: Medium
Type: Array / Two Pointer / Binary Search
---

## **Problem: 

![[Two Sum 2 Problem.png]]

---
## **Note: 

- My Brute force failed due to Time limit Exceeded but it does work just too inefficient 
- The pointers on each side simply compare if the sum > target we move right pointer if sum < target we move the left pointer until we get our answer

---

## **Time and Space Complexity: 
- Time is O(n) - We have to loop through the whole list of nums in the worst case 
- Space is O(1) - We only use pointers which is constant 

--- 

## **Brute Force: 

![[Two Sum 2.png]]

---
## **My Solution: 

![[Two Sum 2 Solution.png]]

---
## **Optimal Solution: 

![[Two Sum 2 Solution.png]]