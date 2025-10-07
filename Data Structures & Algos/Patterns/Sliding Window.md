## **Overview**

The **Sliding Window** pattern is a technique used to reduce the time complexity of problems involving arrays or strings. It involves creating a window (a subset of the data structure) that slides over the input to examine a portion of it at a time. This is especially useful for problems involving **contiguous sequences**, **maximum/minimum values**, or **sums**.

---

## **What Does it Look Like?**

- A fixed-size or variable-size window moving across an array or string.
- Often involves two pointers (`start`, `end`) to define the window boundaries.
- Common operations: expanding the window, shrinking it, updating a result based on the window contents.

**Visual Example (Variable Window):**

```
Input: [1, 3, 2, 5, 4], target = 7  
Window: [1, 3, 2] → sum = 6  
Slide: [3, 2, 5] → sum = 10  
```

![[Sliding Window Example.png]]

---

## **How to Recognize This Pattern**

- The problem asks for **maximum/minimum/average/sum** of a **subarray or substring**.
- You need to find **contiguous elements** that satisfy a condition.
- Brute-force solution involves nested loops or recalculating values repeatedly.
- Keywords: “subarray”, “substring”, “contiguous”, “window”, “at most k”, “exactly k”.

---

## **Example Implementation**

**Problem:** Find the maximum sum of a subarray of size `k`.

```Python
def max_sum_subarray(arr, k):
    max_sum = 0
    window_sum = sum(arr[:k])
    max_sum = window_sum

    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)

    return max_sum
```

  

Show more lines

**Key Idea:**

- Initialize the window with the first `k` elements.
- Slide the window by removing the element going out and adding the new one coming in.

---

## **Applied Problems**

| Problem Name                                   | Link          | Difficulty | Notes                               |
| ---------------------------------------------- | ------------- | ---------- | ----------------------------------- |
| Maximum Sum Subarray of Size K                 | LeetCode #643 | Easy       | Classic fixed-size window           |
| Longest Substring Without Repeating Characters | LeetCode #3   | Medium     | Variable-size window with hash set  |
| Minimum Size Subarray Sum                      | LeetCode #209 | Medium     | Shrinking window based on condition |
| Permutation in String                          | LeetCode #567 | Medium     | Sliding window with frequency map   |
| Sliding Window Maximum                         | LeetCode #239 | Hard       | Requires deque for optimization     |