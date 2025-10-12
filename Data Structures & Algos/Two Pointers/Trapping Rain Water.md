---
date: 2025-10-10
Difficulty: Hard
Type:
---

## **Problem: 

![[Trapping Rain Water Problem.png]]

---
## **Note: 
- This problem is challenging however not very complicated 
- The formula to find the amount of water is (leftMax or rightMax) - current postion 
- We basically subtract the Max we've seen on either side from the current place we are and once the left and right pointer meet we'll have our solution 

---

## **Time and Space Complexity: 
- Time is O(n) - We are looping through our list at once
- Space is O(1) - Everything we are storing is constant time 

--- 

## **Brute Force: 
(not mine)
![[Trapping Rain Water Brute.png]]


---
## **My Solution: 
![[Trapping Rain Water-1.png]]

---
## **Optimal Solution: 
![[Trapping Rain Water.png]]
