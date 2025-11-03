---
date: 2025-10-27
Difficulty: Easy
Type: Array / Two Pointers/ Sorting
---

## **Problem: 

![[Merge Sorted Array Problem.png]]

---
## **Note: 
- This problem was kind of hard at first I only got brute force, which is O((m + n) log(m + n))
- **Fill `nums1` from the back:** Use a "write pointer" starting at the very end of `nums1` (`m + n - 1`).
- **Compare the largest elements:** Use two other pointers (`p1` and `p2`) to look at the last real elements of `nums1` (at `m-1`) and `nums2` (at `n-1`). Place the **larger** of the two at the write pointer's position.
- **Move pointers down:** After copying a number, move both the write pointer and the pointer for the array you took the number from (either `p1` or `p2`) one step to the left.
- **Copy `nums2` leftovers:** If you run out of elements in `nums1` (p1 < 0) before `nums2`, copy any remaining elements from `nums2` into the front of `nums1`. (If `nums2` finishes first, the remaining `nums1` elements are already in their correct spot).


---

## **Time and Space Complexity: 
- Time is O(m + n) - We have to loop through each array at least once linear for each array
- Space is O(1) - Everything we're storing is constant time 

--- 

## **Brute Force: 
![[Merge Sorted Arrays Brute.png]]


---
## **My Solution: 

![[Merge Sorted Arrays Brute.png]]


---
## **Optimal Solution: 

![[Merge Sorted Arrays Optimal.png]]