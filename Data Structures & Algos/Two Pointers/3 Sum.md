---
date: 2025-10-09
Difficulty: Medium
Type: Array / Two Pointer / Sorting
---

## **Problem: 

---
## **Note: 
- You have to sort the numbers before looping
- For the brute force you have to use a set to avoid duplicates AND you have to add to result using tuple (because you add a list to a set) THEN return each result in res using list(i) for loop in res
- For optimal solution we have to check for duplicates (Hint: use the previous index rather than storing it)
- In the optimal Solution we set a fix value and the solve for two sum afterwards using pointers
- In the optimal Solution when increasing pointers you can just move the left in case of duplicates as cases you write before will evaluate for the right pointer (Hint: make sure to check that l < r too!)


---

## **Time and Space Complexity: 
- Time is O(n^2) - We have a nest while loop and have to do this at least once to check all values
- Space is O(n) - We are storing our result worst case its the whole array

--- 

## **Brute Force: 

![[3 Sum.png]]

---
## **My Solution: 

![[3 Sum-1.png]]

---
## **Optimal Solution: 

![[3 Sum Solution.png]]
