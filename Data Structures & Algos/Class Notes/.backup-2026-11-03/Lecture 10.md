# Simple Sorting Algorithms

Bubble Sort · Selection Sort · Insertion Sort

All three are **comparison-based**, **in-place** (O(1) extra space), and **O(n²)** in the average/worst case. They are mainly useful for learning, for small inputs, and (in insertion sort's case) for nearly-sorted data.

---

## 1. Bubble Sort

### Concept

Repeatedly walk through the array and compare **adjacent pairs**. If a pair is out of order, **swap** them. After each full pass, the largest unsorted element has "bubbled up" to its final spot at the end.

- After pass 1, the largest element is in its final position.
- After pass k, the last k elements are sorted and never need checking again.
- **Early exit optimization:** if a full pass makes **zero swaps**, the array is already sorted, so stop.

### Walkthrough — `[5, 1, 4, 2, 8]`

**Pass 1** (compare indices 0–1, 1–2, 2–3, 3–4)

|Compare|Action|Array|
|---|---|---|
|5, 1|swap|[1, 5, 4, 2, 8]|
|5, 4|swap|[1, 4, 5, 2, 8]|
|5, 2|swap|[1, 4, 2, 5, 8]|
|5, 8|no swap|[1, 4, 2, 5, **8**]|

8 is now locked in place.

**Pass 2**

|Compare|Action|Array|
|---|---|---|
|1, 4|no swap|[1, 4, 2, 5, 8]|
|4, 2|swap|[1, 2, 4, 5, 8]|
|4, 5|no swap|[1, 2, 4, **5**, **8**]|

**Pass 3**

|Compare|Action|Array|
|---|---|---|
|1, 2|no swap|[1, 2, 4, 5, 8]|
|2, 4|no swap|[1, 2, 4, 5, 8]|

No swaps → **stop early**. Sorted: `[1, 2, 4, 5, 8]`

### Pseudocode

```
BUBBLE-SORT(A, n)
    for i = 0 to n - 2
        swapped = false
        for j = 0 to n - 2 - i          // last i elements already sorted
            if A[j] > A[j + 1]
                swap A[j] and A[j + 1]
                swapped = true
        if swapped == false
            break                        // already sorted
```

### Big O

|Case|Time|When|
|---|---|---|
|Best|**O(n)**|Already sorted (one pass, no swaps, early exit)|
|Average|**O(n²)**|Random order|
|Worst|**O(n²)**|Reverse sorted|

- **Space:** O(1)
- **Stable:** Yes (only swaps when strictly `>`)
- **Comparisons (worst):** n(n−1)/2
- **Swaps (worst):** n(n−1)/2 — the most swaps of the three

**Why O(n²)?** Pass 1 does n−1 comparisons, pass 2 does n−2, …, down to 1. Sum = (n−1) + (n−2) + … + 1 = n(n−1)/2 → O(n²).

### Implementation Variations

|Direction|Order|How it works|
|---|---|---|
|Left → right|Ascending|Swap if left > right; the **largest** bubbles to the **end** each pass|
|Left → right|Descending|Swap if left < right; the **smallest** bubbles to the **end** each pass|
|Right → left|Ascending|Swap if left > right; the **smallest** sinks to the **front** each pass|
|Right → left|Descending|Swap if left < right; the **largest** sinks to the **front** each pass|

---

## 2. Selection Sort

### Concept

Split the array into a **sorted left part** and an **unsorted right part**. On each pass, **find the minimum** of the unsorted part and **swap it** into the first unsorted position. The sorted part grows by one each pass.

- Key idea: _select_ the smallest remaining element.
- Does very few swaps (at most n − 1), but always scans the whole unsorted part, so it never gets faster on sorted input.

### Walkthrough — `[64, 25, 12, 22, 11]`

|Pass (i)|Unsorted part|Min found|Swap|Array after|
|---|---|---|---|---|
|0|[64, 25, 12, 22, 11]|11 (idx 4)|64 ↔ 11|[**11**, 25, 12, 22, 64]|
|1|[25, 12, 22, 64]|12 (idx 2)|25 ↔ 12|[**11, 12**, 25, 22, 64]|
|2|[25, 22, 64]|22 (idx 3)|25 ↔ 22|[**11, 12, 22**, 25, 64]|
|3|[25, 64]|25 (idx 3)|none|[**11, 12, 22, 25**, 64]|

Last element is automatically in place. Sorted: `[11, 12, 22, 25, 64]`

### Pseudocode

```
SELECTION-SORT(A, n)
    for i = 0 to n - 2
        minIndex = i
        for j = i + 1 to n - 1
            if A[j] < A[minIndex]
                minIndex = j
        if minIndex != i
            swap A[i] and A[minIndex]
```

### Big O

|Case|Time|When|
|---|---|---|
|Best|**O(n²)**|Even if sorted, it still scans for the min every pass|
|Average|**O(n²)**||
|Worst|**O(n²)**||

- **Space:** O(1)
- **Stable:** No (the long-distance swap can jump an element past an equal one). Example: `[4a, 4b, 1]` → swap 4a with 1 → `[1, 4b, 4a]`
- **Comparisons (always):** n(n−1)/2
- **Swaps:** at most **n − 1** → O(n). Useful when writes/swaps are expensive.

**Why O(n²)?** Same sum as bubble: (n−1) + (n−2) + … + 1 = n(n−1)/2, and it's the same regardless of input order.

### Implementation Variations

|Direction|Order|How it works|
|---|---|---|
|Left → right|Ascending|Find the **min**, swap it to the **front** of the unsorted part|
|Left → right|Descending|Find the **max**, swap it to the **front** of the unsorted part|
|Right → left|Ascending|Find the **max**, swap it to the **end** of the unsorted part|
|Right → left|Descending|Find the **min**, swap it to the **end** of the unsorted part|

---

## 3. Insertion Sort

### Concept

Like sorting a hand of playing cards. Keep a **sorted left part**. Take the next element (the **key**), and **shift** larger sorted elements one spot right until you find where the key belongs, then **insert** it there.

- Starts at index 1 (a single element at index 0 is trivially sorted).
- Uses **shifts**, not swaps — each shift is one write.
- **Adaptive:** the closer the input is to sorted, the less work it does.
- **Online:** can sort data as it arrives.

### Walkthrough — `[5, 2, 4, 6, 1, 3]`

|i|Key|What happens|Array after|
|---|---|---|---|
|1|2|5 > 2 → shift 5 right; insert 2 at idx 0|[**2, 5**, 4, 6, 1, 3]|
|2|4|5 > 4 → shift; 2 < 4 → stop; insert at idx 1|[**2, 4, 5**, 6, 1, 3]|
|3|6|5 < 6 → stop immediately; no change|[**2, 4, 5, 6**, 1, 3]|
|4|1|shift 6, 5, 4, 2; insert 1 at idx 0|[**1, 2, 4, 5, 6**, 3]|
|5|3|shift 6, 5, 4; 2 < 3 → stop; insert at idx 2|[**1, 2, 3, 4, 5, 6**]|

Sorted: `[1, 2, 3, 4, 5, 6]`

### Pseudocode

```
INSERTION-SORT(A, n)
    for i = 1 to n - 1
        key = A[i]
        j = i - 1
        while j >= 0 and A[j] > key
            A[j + 1] = A[j]             // shift right
            j = j - 1
        A[j + 1] = key                  // insert into the gap
```

### Big O

|Case|Time|When|
|---|---|---|
|Best|**O(n)**|Already sorted (inner loop never runs; n−1 comparisons)|
|Average|**O(n²)**|Random order|
|Worst|**O(n²)**|Reverse sorted (every key shifts all the way to the front)|

- **Space:** O(1)
- **Stable:** Yes (uses strictly `>`, so equal elements keep their order)
- **Comparisons (worst):** n(n−1)/2
- **Shifts (worst):** n(n−1)/2
- More precisely, runtime is **O(n + d)** where d = number of inversions (out-of-order pairs). Nearly sorted → few inversions → fast.

**Why O(n²) worst case?** Key at index i may need i shifts: 1 + 2 + … + (n−1) = n(n−1)/2.

### Implementation Variations

|Direction|Order|How it works|
|---|---|---|
|Left → right|Ascending|Sorted part grows on the **left**; shift elements **> key** right, insert key|
|Left → right|Descending|Sorted part grows on the **left**; shift elements **< key** right, insert key|
|Right → left|Ascending|Sorted part grows on the **right** (start at n−2); shift elements **< key** left, insert key|
|Right → left|Descending|Sorted part grows on the **right** (start at n−2); shift elements **> key** left, insert key|

---

## Side-by-Side Comparison

||Bubble|Selection|Insertion|
|---|---|---|---|
|Best|O(n)*|O(n²)|O(n)|
|Average|O(n²)|O(n²)|O(n²)|
|Worst|O(n²)|O(n²)|O(n²)|
|Space|O(1)|O(1)|O(1)|
|Stable|Yes|No|Yes|
|Adaptive|Yes*|No|Yes|
|Swaps/writes|O(n²)|**O(n)**|O(n²) shifts|
|Core move|Swap neighbors|Find min, swap once|Shift + insert|

* Only with the early-exit `swapped` flag.

### Invariants (what's true after pass k)

- **Bubble:** the last k elements are the k largest, in final position.
- **Selection:** the first k elements are the k smallest, in final position.
- **Insertion:** the first k+1 elements are sorted _relative to each other_ (not necessarily in final position).

### When to use which

- **Insertion:** small arrays or nearly sorted data. Used inside hybrid sorts (e.g., Timsort, introsort) for small subarrays.
- **Selection:** when writes/swaps are costly and you want the fewest.
- **Bubble:** mostly teaching; rarely the best choice in practice.

---

## Self-Check Questions

1. Run each algorithm by hand on `[3, 1, 2]`. How many comparisons and swaps/shifts does each make?
2. Why does selection sort stay O(n²) on a sorted array while insertion sort drops to O(n)?
3. What breaks stability if you change `>` to `>=` in bubble or insertion sort?
4. After 2 passes on `[7, 3, 9, 1, 5]`, what does the array look like for each algorithm?
5. Which algorithm's invariant guarantees elements are in their _final_ positions, and which doesn't?