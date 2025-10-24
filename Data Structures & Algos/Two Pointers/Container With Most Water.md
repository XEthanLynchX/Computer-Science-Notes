---
date: 2025-10-10
Difficulty: Medium
Type: Array / Two Pointer
---

## **Problem: 

![[Container With Most Water.png]]


---
## **Note: 

- First Medium I first tried with optimal space and runtime (LET'S GOO!)
- Implement two pointer and multiply the lowest of the two pointers by the len and keep track of largest one you have found
- Brute force we check every possible position of i and j which O(n^2) because we have to use a nest for loop  


---

## **Time and Space Complexity: 
- Time is O(n) - We have to loop through the whole list in case edge cases where the height is ridiculous 
- Space is O(1) - We only store primitive types to keep track of position and max height which is constant

--- 

## **Brute Force: 

![[Container With Most Water Brute.png]]

---
## **My Solution: 

![[Container With Most Water Solution.png]]

---
## **Optimal Solution: 

![[Container With Most Water-1.png]]