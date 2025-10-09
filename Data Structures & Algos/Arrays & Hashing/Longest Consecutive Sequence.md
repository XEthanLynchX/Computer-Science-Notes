---
date: 2025-10-08
Difficulty: Medium
Type: Array / Hashing
---

## **Problem: 

![[Longest Consecutive Sequence Problem.png]]

---
## **Note: 
- Sets are must here to reduce doing duplicate lookups (which will also cause issues with checks)
- For brute force you can check each number in nums then increment the current number (check if that's there) then return the longest sequence. This is inefficient because we checking every number even if there not the start of a sequence
- For the optimal solution we can check to if a number with a value 1 less than current value is already in our set if so we don't check the sequence this avoids redundant checks and takes time from O(n^ 2) -> O(n) 

---

## **Time and Space Complexity: 
- Time is O(n) - Worst case we have to loop through every number in our list for the longest sequence
- Space is O(n) - In the worst case every number in our list is unique and we have to store each number in our set

--- 

## **Brute Force: 

![[Longest Consecutive Sequence Brute.png]]

---
## **My Solution: 

![[Longest Consecutive Sequence Solution.png]]


---
## **Optimal Solution: 

**![[Longest Consecutive Sequence Solution.png]]**
