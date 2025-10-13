---
date: 2025-10-12
Difficulty: Easy
Type: Array / Dynamic Programming
---

## **Problem: 

![[Best time to buy and sell a stock Problem.png]]
---
## **Note: 
- Brute force is O(n^2) because we have a nested loop 
- For the optimal solution you can use a sliding window like in my solution or you can use a for loop and keep track of the minimum value and update the current highest - min and return best 


---

## **Time and Space Complexity: 
- Time is O(n) - Worst case we have to loop through the list at least once
- Space is O(1) - We are using constant space because we are only storing the best and pointers

--- 

## **Brute Force: 

![[Best time to buy and sell a stock Brute.png]]
---
## **My Solution: 

![[Best time to buy and sell a stock.png]]


---
## **Optimal Solution: 

![[Best time to buy and sell a stock-1.png]]