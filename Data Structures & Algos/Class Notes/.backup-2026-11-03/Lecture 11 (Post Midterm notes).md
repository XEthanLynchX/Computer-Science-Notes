# Stable vs. Unstable & In-Place Sorting

---

## Part 1: Stable vs. Unstable

### What is stability?

A sort is **stable** if elements with **equal keys keep their original relative order**.

> If A came before B and they're "equal," A still comes before B after sorting.

### Quick example

Sort these by **number only**. Letters show the original order of equal values.

```
Input:     [3a, 1, 3b, 2]

Stable:    [1, 2, 3a, 3b]   ← 3a still before 3b ✅
Unstable:  [1, 2, 3b, 3a]   ← order flipped ❌ (possible, not guaranteed)
```

Both outputs are correctly sorted. Stability only matters for **ties**.

### Why does it matter?

**Sorting by multiple keys.** Example: a class list.

1. Sort students by **name**.
2. Then sort by **grade** using a **stable** sort.

Result: students are grouped by grade, and **within each grade they're still alphabetical**. An unstable sort could scramble the names inside each grade.

```
After step 1 (by name):   Alice-B, Bob-A, Cara-B, Dan-A
After stable sort (grade): Bob-A, Dan-A, Alice-B, Cara-B   ← names still A→Z within each grade
```

### What makes a sort unstable?

**Long-distance swaps.** If an element can jump over an equal element, order can flip.

Selection sort example:

```
[4a, 4b, 1]   → min is 1, swap with 4a
[1, 4b, 4a]   ← 4a jumped past 4b ❌
```

Stable sorts usually only move elements **one step at a time** (bubble, insertion) or **merge carefully** (merge sort takes from the left half on ties).

### Rule of thumb

- Compare with **strictly `>`** (not `>=`) when deciding to swap/shift → keeps equal elements in place.
- Any sort can be made stable by using the **original index as a tiebreaker** (costs extra space).

### The lists

|✅ Stable|❌ Unstable|
|---|---|
|Bubble sort|Selection sort|
|Insertion sort|Quick sort|
|Merge sort|Heap sort|
|Counting sort|Shell sort|
|Radix sort||
|Timsort (Python, Java objects)||

**Memory trick:** _"Bubble, Insert, Merge — they keep their word."_ The unstable ones (**S**election, **Q**uick, **H**eap, **Sh**ell) all make **big jumps**.

---

## Part 2: In-Place Sorting

### What is in-place?

A sort is **in-place** if it sorts **within the original array**, using only a **constant amount of extra memory**, O(1).

> No second array. Just a few variables (like `temp`, `i`, `j`).

**Note:** Quick sort is usually still called in-place even though its recursion uses **O(log n)** stack space. It doesn't copy the array, so it counts.

### Not in-place

Needs extra memory that **grows with n** (usually a helper array).

- **Merge sort:** needs a temporary array to merge into → O(n)
- **Counting sort:** needs a count array → O(k), plus an output array O(n)

### The lists

|✅ In-Place|❌ Not In-Place|
|---|---|
|Bubble sort — O(1)|Merge sort — O(n)|
|Selection sort — O(1)|Counting sort — O(n + k)|
|Insertion sort — O(1)|Radix sort — O(n + k)|
|Heap sort — O(1)|Timsort — O(n)|
|Shell sort — O(1)|Bucket sort — O(n + k)|
|Quick sort — O(log n) stack||

**Memory trick:** If the algorithm **merges** or **counts**, it needs a **second array** → not in-place.

---

## Part 3: Put It Together

|Algorithm|Stable?|In-Place?|Extra Space|Time (avg)|
|---|---|---|---|---|
|Bubble|✅|✅|O(1)|O(n²)|
|Insertion|✅|✅|O(1)|O(n²)|
|Selection|❌|✅|O(1)|O(n²)|
|Shell|❌|✅|O(1)|~O(n^1.3)–O(n^1.5)|
|Heap|❌|✅|O(1)|O(n log n)|
|Quick|❌|✅*|O(log n)|O(n log n)|
|Merge|✅|❌|O(n)|O(n log n)|
|Counting|✅|❌|O(n + k)|O(n + k)|
|Radix|✅|❌|O(n + k)|O(d·(n + k))|

* via recursion stack only

### The 4 quadrants

||**In-Place**|**Not In-Place**|
|---|---|---|
|**Stable**|Bubble, Insertion|Merge, Counting, Radix|
|**Unstable**|Selection, Quick, Heap, Shell|_(rare — no common ones)_|

### Big takeaway

- Want **stable + in-place + fast**? There's no common O(n log n) algorithm that gives all three. You usually trade:
    - **Merge sort** → stable + fast, but uses O(n) memory
    - **Heap sort** → in-place + fast, but unstable
    - **Insertion sort** → stable + in-place, but O(n²)

---

## Self-Check

1. Sort `[2a, 1, 2b, 1b]` with selection sort by hand. Does 2a stay before 2b?
2. Why does changing `>` to `>=` make insertion sort unstable?
3. Why is merge sort not in-place, but quick sort is?
4. You need to sort employees by department while keeping them sorted by salary within each department. Which property do you need?
5. Name one algorithm in each of the three filled quadrants.