---
date: 2025-10-06
Difficulty:
Type:
---

## **Problem: 

![[Remove Duplicates from Sorted Array.png]]

---
## **Note: 
- For this problem you need return the count of unique elements **and** modify nums itself but not return it
- Instead of removing from nums you can rearrange the array because you need to have the unique numbers positioned first 


---

## **Time and Space Complexity: 
- Time is O(n) - We have to loop through the size of nums each time 
- Space is O(1) - The only thing we are storing is a constant variable which we modify so space is constantly only storing one integer 

--- 

## **Brute Force: 

```python 
class Solution:

    def removeDuplicates(self, nums: List[int]) -> int:
        seen = {}
        count = 0

        for i in range(len(nums)):
            seen[nums[i]] = seen.get(nums[i], 0 ) + 1

        for key, value in seen.items():
            count += 1
            if value > 1:
                for i in range(value - 1):
                    nums.remove(key)

        return count
```

---
## **My Solution: 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        slow = 0
        
        for fast in range(1, len(nums)):
            if nums[slow] != nums[fast]:
                nums[slow + 1] = nums[fast]
                slow += 1

        return slow + 1
```



---
## **Optimal Solution: 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        slow = 0
        
        for fast in range(1, len(nums)):
            if nums[slow] != nums[fast]:
                nums[slow + 1] = nums[fast]
                slow += 1

        return slow + 1
```
