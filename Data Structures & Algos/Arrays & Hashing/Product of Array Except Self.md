---
date: 2025-10-07
Difficulty: Medium
Type: Array / Prefix Sum
---

## **Problem: 
![[Product of Array Except Self.png]]
---
## **Note: 
- There is a constraint that doesn't allow you too but if you could you could take the product of every number and divide by the current number then append to result list
- This is a hard problem lol
- We create a prefix array forward which just the number multiplied by 1 and then postfix which is same but backwards then we multiply to get our final answer (less optimal)
- Optimal way is to use res list as our prefix and postfix list and use variables for prefix and postfix and set to1 and multiply by the current value and set our position in result list to that value and then do the same for post fix (bad explanation read the code )


---

## **Time and Space Complexity: 
- Time is O(n) - We have to go through each array only based on the size of array 'n'
- Space is O(n) - Our output array is the size of input array

--- 

## **Brute Force: 
N/A

---
## **My Solution: 
![[Product of Array Except Self Solution.png]]

---
## **Optimal Solution: 

![[Product of Array Except Self optimal.png]]

