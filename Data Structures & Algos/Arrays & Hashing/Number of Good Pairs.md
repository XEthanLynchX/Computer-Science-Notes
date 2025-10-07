---
date: 2025-10-04
Difficulty: Easy
Type: Hash table / Array / Math
---

## **Problem: 
![[Number of Good Pairs Problem.png]]
---
## **Note: 
- For the Brute Force instead of using two pointers you can just use a nested for loop 
- The formula for finding pairs is count * (count - 1) // 2 

---

## **Time and Space Complexity: 
- Time is O(n) - Time is O(n) because the each loop is based on the size of our array
- Space is O(n) - Our dictionary is built on the size of given array

--- 

## **Brute Force: 
![[Number of Good Pairs Brute Force.png]]
---
## **My Solution: 
![[Number of Good Pairs Solution.png]]


---
## **Optimal Solution: 
![[Number of Good Pairs.png]]
