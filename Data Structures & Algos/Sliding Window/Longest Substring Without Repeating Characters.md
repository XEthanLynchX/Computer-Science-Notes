---
date: 2025-10-14
Difficulty: Medium
Type: Hash Table / String / Sliding Window
---

## **Problem: 

![[Longest Substring Without Repeating Characters Problem.png]]
---
## **Note: 
- Brute Force Checks every substring and checks the seen array to see if there a duplicate if so it clear the seen array and moves to the next substring 
- For the optimal solution go through the whole array and use a `set()` if the current letter is in the set remove and slide the left pointer forward otherwise add it to the set and obviously keep track of the max value
- You have to use while when checking because the duplicate might not be left pointer but rather a character that came after it so we have to remove every character until its removed 
-  ![[Longest Substring Without Repeating Characters note.png]]

---

## **Time and Space Complexity: 
- Time is O(n) - We always have to loop through the whole thing 
- Space is O(n) - Worst case every character is unique so we have to store every character

--- 

## **Brute Force: 

![[Longest Substring Without Repeating Characters.png]]
---
## **My Solution: 

![[Longest Substring Without Repeating Characters Solution.png]]


---
## **Optimal Solution: 

![[Longest Substring Without Repeating Characters-1.png]]
