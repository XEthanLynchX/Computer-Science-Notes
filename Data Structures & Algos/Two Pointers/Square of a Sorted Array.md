---
date: 2025-10-06
Difficulty: Easy
Type: Array / Two Pointer / Sorting
---

## **Problem: 

![[Square of a Sorted Array.png]]

---
## **Note: 
- Solving this using sorting is O( n log n) and space is O(1) because of sorting 
- You need an array for memory to make this answer O(n)
- Use the absolute value and compare value at left and right 
---

## **Time and Space Complexity: 
- Time is O(n) - We have to go through every item in the array
- Space is O(n) - Our result array is the size of our input array

--- 

## **Brute Force: 

```python 
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            nums[i] = nums[i] * nums[i]

        nums.sort()
        return nums
```

---
## **My Solution: 

![[Square of a Sorted Array Solution-1.png]]
---
## **Optimal Solution: 

![[Square of a Sorted Array Solution.png]]
