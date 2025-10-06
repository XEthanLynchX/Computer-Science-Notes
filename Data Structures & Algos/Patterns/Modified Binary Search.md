## **Overview**

**Modified Binary Search** is a variation of the classic binary search algorithm, adapted to solve problems where the input is not strictly sorted or has additional constraints. It’s commonly used in problems involving **rotated arrays**, **searching in unknown-sized arrays**, or **finding boundaries** (like first/last occurrence).

This pattern maintains the binary search principle of dividing the search space in half, but adds logic to handle irregularities or conditions.

---

## **What Does it Look Like?**

- Still uses `low`, `high`, and `mid` pointers.
- Instead of checking for equality only, it may check for:
    - Sorted halves
    - Rotation points
    - Boundary conditions
    - Custom comparison logic

**Visual Example (Rotated Array):**

```
Input: [4,5,6,7,0,1,2], target = 0  
Mid = 3 → nums[mid] = 7  
Right half is sorted → search in [4,5,6,7] vs [0,1,2]
```

---

## **How to Recognize This Pattern**

- The array is **not fully sorted**, or has been **rotated**.
- You’re asked to find **first/last occurrence**, **peak**, or **boundary**.
- The problem involves **logarithmic time complexity** (hinting at binary search).
- Keywords: “rotated”, “peak”, “first/last”, “boundary”, “log(n)”, “sorted but…”

---

## **Example Implementation**

**Problem:** Search in Rotated Sorted Array

```python
def search(nums, target):
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        # Left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1

    return -1
```

---

## **Applied Problems**

|Problem Name|Link|Difficulty|Notes|
|---|---|---|---|
|Search in Rotated Sorted Array|LeetCode #33|Medium|Classic modified binary search|
|Find Minimum in Rotated Sorted Array|LeetCode #153|Medium|Focus on finding pivot|
|Peak Index in a Mountain Array|LeetCode #852|Easy|Binary search for peak|
|First Bad Version|LeetCode #278|Easy|Binary search for boundary|
|Find First and Last Position of Element|LeetCode #34|Medium|Binary search for both boundaries|