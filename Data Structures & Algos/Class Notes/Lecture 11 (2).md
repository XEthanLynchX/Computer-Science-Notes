# Advanced Sorting Algorithms

---

## Part 1: The Big Picture

### Why "advanced"?

The simple sorts (bubble, selection, insertion) are **O(n²)**. Advanced sorts get to **O(n log n)**, or even **O(n)** in special cases.

### The speed limit for comparison sorts

Any sort that works by **comparing** elements (`a < b?`) can't beat **O(n log n)** in the worst case.

Sorts that **don't compare** (counting, radix, bucket) can beat this, because they use the values themselves (like digits) to place elements. The catch is that they only work on certain kinds of data.

---

## Part 2: Approach Types (Paradigms)

|Approach|Idea|Algorithms|
|---|---|---|
|**Divide and conquer**|Split the problem into smaller pieces, solve each, combine|Merge sort, Quick sort|
|**Transform and conquer**|Rearrange the data into a helpful structure first, then sort|Heap sort (builds a heap)|
|**Diminishing increment**|Insertion sort on elements far apart, then closer and closer|Shell sort|
|**Distribution (non-comparison)**|Use the values or digits to drop elements into slots/buckets|Counting, Radix, Bucket sort|
|**Hybrid**|Combine algorithms, using each where it's strongest|Timsort, Introsort|

### Divide and conquer, 3 steps

1. **Divide:** break the array into smaller parts.
2. **Conquer:** sort each part (usually recursively).
3. **Combine:** put the sorted parts together.

**Merge vs. quick, the key difference:**

||Divide step|Combine step|
|---|---|---|
|**Merge sort**|Easy (just cut in half)|**Hard** (merging does the real work)|
|**Quick sort**|**Hard** (partitioning does the real work)|Easy (nothing to do)|

---

## Part 3: Overview Table

|Algorithm|Approach|Implemented|Best|Average|Worst|Space|Stable|In-Place|
|---|---|---|---|---|---|---|---|---|
|**Merge**|Divide & conquer|Recursive (or iterative bottom-up)|O(n log n)|O(n log n)|O(n log n)|O(n)|✅|❌|
|**Quick**|Divide & conquer|Recursive|O(n log n)|O(n log n)|**O(n²)**|O(log n)|❌|✅|
|**Heap**|Transform & conquer|Iterative (heapify can be recursive)|O(n log n)|O(n log n)|O(n log n)|O(1)|❌|✅|
|**Shell**|Diminishing increment|Iterative|O(n log n)|~O(n^1.3–1.5)|O(n²)*|O(1)|❌|✅|
|**Counting**|Distribution|Iterative|O(n + k)|O(n + k)|O(n + k)|O(n + k)|✅|❌|
|**Radix**|Distribution|Iterative (LSD) / Recursive (MSD)|O(d(n + k))|O(d(n + k))|O(d(n + k))|O(n + k)|✅|❌|
|**Bucket**|Distribution|Iterative|O(n + k)|O(n + k)|O(n²)|O(n + k)|✅**|❌|
|**Timsort**|Hybrid (merge + insertion)|Iterative|O(n)|O(n log n)|O(n log n)|O(n)|✅|❌|
|**Introsort**|Hybrid (quick + heap + insertion)|Recursive|O(n log n)|O(n log n)|O(n log n)|O(log n)|❌|✅|

- **k** = range of values (counting) or base/number of buckets (radix, bucket)
- **d** = number of digits
- * depends on the gap sequence
- ** if the sort used inside each bucket is stable

### One-liners

- **Merge:** split in half, sort each half, merge them back.
- **Quick:** pick a pivot, put smaller items left and bigger right, repeat on each side.
- **Heap:** build a max-heap, repeatedly pull the max to the end.
- **Shell:** insertion sort with shrinking gaps.
- **Counting:** count how many of each value, then rebuild the array.
- **Radix:** sort digit by digit (ones, tens, hundreds…) using a stable sort.
- **Bucket:** scatter into buckets by value range, sort each bucket, concatenate.
- **Timsort:** find already-sorted runs, insertion sort small ones, merge them. (Python's `sorted()`, Java for objects)
- **Introsort:** quick sort, but switches to heap sort if recursion gets too deep. (C++ `std::sort`)

---

## Part 4: Merge Sort (Deep Dive)

### Concept

1. **Divide:** split the array into two halves.
2. **Conquer:** recursively merge sort each half, down to single elements (a single element is already sorted).
3. **Combine:** **merge** the two sorted halves into one sorted array.

**Merging two sorted lists:** compare the front of each, take the smaller one, repeat. When one side runs out, copy the rest of the other.

### Walkthrough: `[38, 27, 43, 3, 9, 82, 10, 15]`

**Split down**

```
                [38, 27, 43, 3, 9, 82, 10, 15]
               /                              \
      [38, 27, 43, 3]                  [9, 82, 10, 15]
       /          \                     /          \
   [38, 27]    [43, 3]             [9, 82]     [10, 15]
   /    \      /    \              /    \      /    \
 [38]  [27]  [43]   [3]          [9]   [82]  [10]  [15]
```

**Merge up**

```
 [27, 38]     [3, 43]            [9, 82]     [10, 15]
      \        /                       \       /
   [3, 27, 38, 43]                 [9, 10, 15, 82]
              \                        /
         [3, 9, 10, 15, 27, 38, 43, 82]
```

**One merge step up close:** merging `[3, 27, 38, 43]` and `[9, 10, 15, 82]`

|Compare|Take|Result so far|
|---|---|---|
|3 vs 9|3|[3]|
|27 vs 9|9|[3, 9]|
|27 vs 10|10|[3, 9, 10]|
|27 vs 15|15|[3, 9, 10, 15]|
|27 vs 82|27|[3, 9, 10, 15, 27]|
|38 vs 82|38|[3, 9, 10, 15, 27, 38]|
|43 vs 82|43|[3, 9, 10, 15, 27, 38, 43]|
|left empty|82|[3, 9, 10, 15, 27, 38, 43, 82]|

### Pseudocode

```
MERGE-SORT(A, left, right)
    if left >= right
        return                              // 0 or 1 element: already sorted
    mid = (left + right) / 2
    MERGE-SORT(A, left, mid)
    MERGE-SORT(A, mid + 1, right)
    MERGE(A, left, mid, right)

MERGE(A, left, mid, right)
    L = copy of A[left..mid]
    R = copy of A[mid+1..right]
    i = 0, j = 0, k = left
    while i < length(L) and j < length(R)
        if L[i] <= R[j]                     // <= keeps it STABLE
            A[k] = L[i];  i = i + 1
        else
            A[k] = R[j];  j = j + 1
        k = k + 1
    copy any remaining L[i..] into A
    copy any remaining R[j..] into A
```

### Big O

|Case|Time|Why|
|---|---|---|
|Best|O(n log n)|Always splits and merges everything, even if sorted|
|Average|O(n log n)||
|Worst|O(n log n)|Guaranteed: the splits are always even|

**Why O(n log n)?**

- Halving n until you reach size 1 takes **log n levels**.
- Each level merges **n elements total**.
- **log n levels × n work = O(n log n)**

Recurrence: **T(n) = 2T(n/2) + O(n)** → O(n log n)

- **Space:** O(n) for the temporary arrays, plus O(log n) recursion stack
- **Stable:** ✅ Yes (take from the left half on ties: `<=`)
- **In-place:** ❌ No

### How it's implemented

|Style|How|
|---|---|
|**Top-down (recursive)**|The standard version: split recursively, merge on the way back up|
|**Bottom-up (iterative)**|No recursion: merge size-1 pieces into size 2, then 2 into 4, 4 into 8…|

### Strengths and weaknesses

- ✅ Guaranteed O(n log n), no bad worst case
- ✅ Stable
- ✅ Great for **linked lists** (no extra array needed) and **external sorting** (data too big for memory)
- ❌ Needs O(n) extra memory
- ❌ Slower than quick sort in practice on arrays (more copying)

---

## Part 5: Quick Sort (Deep Dive)

### Concept

1. **Pick a pivot** element.
2. **Partition:** rearrange so everything **≤ pivot** is on its left and everything **> pivot** is on its right. The pivot is now in its **final position**.
3. **Recursively** quick sort the left part and the right part.

No combine step is needed: once both sides are sorted, the whole array is sorted.

### Walkthrough: `[10, 80, 30, 90, 40, 50, 70]`

Using **Lomuto partition** with the **last element as pivot**.

`i` marks the end of the "≤ pivot" region (starts at −1). `j` scans left to right.

**Partition 1: pivot = 70**

|j|A[j]|≤ 70?|Action|Array|
|---|---|---|---|---|
|0|10|yes|i=0, swap A[0]↔A[0]|[10, 80, 30, 90, 40, 50, 70]|
|1|80|no|—|[10, 80, 30, 90, 40, 50, 70]|
|2|30|yes|i=1, swap A[1]↔A[2]|[10, 30, 80, 90, 40, 50, 70]|
|3|90|no|—|[10, 30, 80, 90, 40, 50, 70]|
|4|40|yes|i=2, swap A[2]↔A[4]|[10, 30, 40, 90, 80, 50, 70]|
|5|50|yes|i=3, swap A[3]↔A[5]|[10, 30, 40, 50, 80, 90, 70]|
|end|||swap pivot into A[i+1] = A[4]|[10, 30, 40, 50, **70**, 90, 80]|

70 is in its final spot. Left side `[10, 30, 40, 50]` is all ≤ 70; right side `[90, 80]` is all > 70.

**Recurse right:** `[90, 80]`, pivot = 80 → 90 > 80, so swap pivot into place → `[80, 90]`

**Recurse left:** `[10, 30, 40, 50]`, pivot = 50 → everything is ≤ 50, so the pivot stays at the end. Then `[10, 30, 40]` → pivot 40 stays, and so on.

⚠️ Notice the left side: it was **already sorted**, so every pivot was the max and each partition only removed one element. That's the **worst-case pattern** (see Big O below).

**Final:** `[10, 30, 40, 50, 70, 80, 90]`

### Pseudocode

```
QUICK-SORT(A, low, high)
    if low < high
        p = PARTITION(A, low, high)         // pivot's final index
        QUICK-SORT(A, low, p - 1)
        QUICK-SORT(A, p + 1, high)

PARTITION(A, low, high)                     // Lomuto scheme
    pivot = A[high]
    i = low - 1
    for j = low to high - 1
        if A[j] <= pivot
            i = i + 1
            swap A[i] and A[j]
    swap A[i + 1] and A[high]               // put pivot in place
    return i + 1
```

### Big O

|Case|Time|When|
|---|---|---|
|Best|O(n log n)|Pivot splits the array roughly in **half** every time|
|Average|O(n log n)|Random data, random pivots|
|Worst|**O(n²)**|Pivot is always the **min or max** (e.g., already sorted input + first/last pivot)|

**Why O(n log n) on average?** Good splits → **log n levels**, and partitioning each level is **O(n)**. Recurrence: **T(n) = 2T(n/2) + O(n)** → O(n log n)

**Why O(n²) worst case?** Bad split → one side has n−1 elements, the other has 0. That's **n levels × O(n)** work. Recurrence: **T(n) = T(n−1) + O(n)** → O(n²)

- **Space:** O(log n) recursion stack on average, O(n) in the worst case
- **Stable:** ❌ No (partition swaps jump elements over each other)
- **In-place:** ✅ Yes (no extra array)

### How it's implemented

**Pivot choice:**

|Strategy|Notes|
|---|---|
|First or last element|Simple, but O(n²) on sorted or reverse-sorted input|
|Random element|Makes the worst case very unlikely|
|Median-of-three|Median of first, middle, last; good in practice|

**Partition scheme:**

|Scheme|Notes|
|---|---|
|**Lomuto**|One pointer scans left → right; easier to learn (used above)|
|**Hoare**|Two pointers move toward each other from both ends; fewer swaps, faster|
|**3-way (Dutch flag)**|Splits into < pivot, = pivot, > pivot; great with many duplicates|

**Recursive vs. iterative:** Normally recursive. Can be made iterative with an explicit stack.

### Strengths and weaknesses

- ✅ Usually the **fastest in practice** on arrays (cache-friendly, little copying)
- ✅ In-place
- ❌ O(n²) worst case (fixed with good pivot choice, or with introsort)
- ❌ Not stable

---

## Part 6: Merge Sort vs. Quick Sort

||Merge Sort|Quick Sort|
|---|---|---|
|Approach|Divide & conquer|Divide & conquer|
|Where the work happens|**Combine** (merge)|**Divide** (partition)|
|How it splits|Always in half|Depends on the pivot|
|Best|O(n log n)|O(n log n)|
|Average|O(n log n)|O(n log n)|
|Worst|O(n log n)|**O(n²)**|
|Space|O(n)|O(log n)|
|Stable|✅|❌|
|In-place|❌|✅|
|Implemented|Recursive or iterative (bottom-up)|Recursive|
|Best for|Linked lists, huge data, when stability matters|Arrays in memory, speed|

**Memory trick:**

- **Merge:** "split dumb, merge smart"
- **Quick:** "split smart, merge nothing"

---

## Self-Check

1. Draw the merge sort split/merge tree for `[5, 2, 4, 7, 1, 3]`.
2. Run one Lomuto partition on `[3, 8, 2, 5, 1, 4]` with pivot = 4. Where does 4 end up?
3. Why does quick sort hit O(n²) on an already sorted array with a last-element pivot?
4. Why is merge sort's worst case still O(n log n)?
5. Which advanced sorts can beat O(n log n), and what's the catch?
6. You need a stable O(n log n) sort. Which one do you pick? What if memory is tight and stability doesn't matter?