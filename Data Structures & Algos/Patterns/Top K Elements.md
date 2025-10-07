## **Overview**

The **Top K Elements** pattern is used when you're asked to find the **K most frequent**, **K largest**, or **K smallest** elements in a dataset. It often involves using a **heap (priority queue)** or **bucket sort**, and sometimes **Quickselect** for optimization.

This pattern is common in problems involving **frequency counts**, **ranking**, or **streaming data**.

---

## **What Does it Look Like?**

- Use of a **min-heap** or **max-heap** to keep track of top `k` elements.
- Use of a **hash map** to count frequencies.
- Sometimes involves **sorting** or **bucket sort** for frequency-based problems.
- May use **Quickselect** for optimized selection.

**Visual Example (Top K Frequent Elements):**

```
Input: [1,1,1,2,2,3], k = 2  
Frequencies: {1:3, 2:2, 3:1}  
Top 2: [1,2]
```

![[Top K Elements example.png]]

---

## **How to Recognize This Pattern**

- The problem asks for **top K**, **most frequent**, **K largest/smallest**, or **K closest**.
- You need to maintain a subset of elements based on a ranking or score.
- Keywords: “top K”, “most frequent”, “closest”, “largest/smallest”, “stream”, “ranking”.

---

## **Example Implementation**

**Problem:** Top K Frequent Elements


```Python

import heapq
from collections import Counter

def topKFrequent(nums, k):
    freq_map = Counter(nums)
    return [item for item, _ in heapq.nlargest(k, freq_map.items(), key=lambda x: x[1])]
```

**Key Idea:**

- Count frequencies using `Counter`.
- Use `heapq.nlargest` to get top `k` based on frequency.

---

## **Applied Problems**

|Problem Name|Link|Difficulty|Notes|
|---|---|---|---|
|Top K Frequent Elements|LeetCode #347|Medium|Use heap or bucket sort|
|K Closest Points to Origin|LeetCode #973|Medium|Use max-heap or Quickselect|
|Top K Frequent Words|LeetCode #692|Medium|Lexical ordering with frequency|
|Kth Largest Element in an Array|LeetCode #215|Medium|Quickselect or min-heap|
|Kth Smallest Element in a Sorted Matrix|LeetCode #378|Medium|Min-heap or binary search|