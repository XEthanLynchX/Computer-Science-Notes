---
date: 2025-10-03
Difficulty: Easy
Type: Two Pointers
---

## **Problem: 

![[Valid Palindrome Problem.png]]

---
## **Note: 
- Make sure to remove all non-alphanum characters ( Used Regex)
- First I reversed looped and stored the string and then compared which was O(n) Space 
- Using Two Pointers you can reduce to O(1) Space 

---

## **Time and Space Complexity: 
- Time is O(n) - Function to loop through is based on the size of the array
- Space is O(1) - We only reformat the string which is constant no extra storage

--- 

## **Brute Force:

![[Valid Palindrome Brute Force.png]]



---
## **My Solution: 

![[Valid Palindrome Solution.png]]

---
## **Optimal Solution: 
![[Valid Palindrome Solution.png]]