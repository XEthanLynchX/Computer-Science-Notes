---
date: 2025-10-20
Difficulty: Easy
Type: String / Stack
---

## **Problem: 

![[Valid Parentheses Problem.png]]
---
## **Note: 
- For the optimal solution we use a stack and then evaluate the top of the stack vs the current element 
- Create a valid hash table using the closing parentheses as the keys and the opening as the value 
- Edge case is that the stack won't have any values due to only character being a closing so you wouldn't append it ( check for this in eval condition)
- How do we know answer is True or False (think about the values in the stack)

---

## **Time and Space Complexity: 
- Time is O(n) - We have to go through the whole list at least once
- Space is O(n) - Worst case we have to store every element in the array 

--- 

## **Brute Force: 

---
## **My Solution: 

![[Valid Parentheses Optimal.png]]

---
## **Optimal Solution: 

![[Valid Parentheses.png]]