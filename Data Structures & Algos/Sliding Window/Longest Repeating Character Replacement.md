---
date: 2025-10-16
Difficulty: Medium
Type: Hash Table / String / Sliding Window
---

## **Problem: 

![[Longest Repeating Character Replacement Problem.png]]

---
## **Note: 
- For this we use a hash table to keep track of the most occurring character (optimal)
-  Then while the window size - max value in our hashtable is < k (Numbers of time we can replace) we decrement the current place L is at in our hash table and slide L right (optimal)
- To keep track of the window size its r - l + 1 

---

## **Time and Space Complexity: 
- Time is O(n) - We have to go through every character at least once 
- Space is O(1) - We are storing at most 26 characters which is still linear 

--- 

## **Brute Force: 

---
## **My Solution: 

![[Longest Repeating Character Replacement.png]]

---
## **Optimal Solution: 

![[Longest Repeating Character Replacement-1.png]]
