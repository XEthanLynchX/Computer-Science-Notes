## **Overview**

The **Subset** pattern is commonly used in problems that require generating all possible combinations or subsets of a given set of elements. It’s closely tied to **backtracking** and **recursion**, and is often used in problems involving **combinatorics**, **power sets**, and **decision trees**.

---

## **What Does it Look Like?**

- Recursive tree where each level represents a decision: include or exclude an element.
- Often involves building up a list (`path`) and exploring all branches.
- Can be visualized as a binary tree of choices.

**Visual Example:**

```
Input: [1, 2]  
Subsets:  
→ []  
→ [1]  
→ [2]  
→ [1, 2]
```

---

## **How to Recognize This Pattern**

- The problem asks for **all combinations**, **subsets**, or **permutations**.
- You need to explore **every possible grouping** of elements.
- Often includes constraints like “no duplicates”, “fixed length”, or “sum equals target”.
- Keywords: “generate all”, “combinations”, “subsets”, “backtracking”, “power set”.

---

## **Example Implementation**

**Problem:** Generate all subsets of a list of unique integers.


```Python
def subsets(nums):
    result = []

    def backtrack(start, path):
        result.append(path[:])
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()

    backtrack(0, [])
    return result
```

**Key Idea:**

- Use recursion to explore all inclusion/exclusion paths.
- Backtrack by removing the last element after recursive call.

---

## **Applied Problems**

|Problem Name|Link|Difficulty|Notes|
|---|---|---|---|
|Subsets|LeetCode #78|Medium|Classic subset generation|
|Subsets II (with duplicates)|LeetCode #90|Medium|Requires skipping duplicates|
|Combination Sum|LeetCode #39|Medium|Subset + target sum|
|Permutations|LeetCode #46|Medium|Similar recursive structure|
|Letter Combinations of a Phone Number|LeetCode #17|Medium|Subset-like branching with mapping|