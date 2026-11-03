[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 8: Big O & Algorithm Analysis

## What Big O measures

**Big O** describes how an algorithm's running time **grows** as the input size **n** grows.

It deliberately **ignores**:

- **Hardware:** machine speed, compiler, language
- **The specific input data:** we count steps as a function of n, not of particular values

## Finding the growth rate

Given f(n) = $2n^3 - 3n^2 + 5n + 10$:

1. Find the **dominating term**, the one that grows fastest: $2n^3$.
2. Drop the **non-dominating terms** ($-3n^2 + 5n + 10$). For large n they barely matter.
3. Drop the **constant coefficient** (the 2).

Growth rate: **O($n^3$)**, cubic.

## Common growth rates (fastest to slowest)

| Big O | Name | Example |
| --- | --- | --- |
| O(1) | Constant | Array access by index, stack push/pop |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Linear search, traversing a list |
| O(n log n) | Linearithmic | Merge sort, heapsort |
| O(n²) | Quadratic | Bubble, selection, insertion sort |
| O(n³) | Cubic | Three nested loops over n |
| O(2ⁿ) | Exponential | Trying every subset |

## Best, worst, and average case

- **Best case (BCS):** the input that makes the algorithm do the **least** work
- **Worst case (WCS):** the input that makes it do the **most** work
- **Average case (ACS) / order of magnitude:** the expected work on typical input; in class, the average of the best and worst cases

## Counting steps: ReadArray

```java
function ReadArray(n) {
    int[] A = new int[n];          // 1 step
    for (i = 0; to n - 1) {        // runs n times
        Read x;                    // 1 step per iteration
        if (x > 0) {               // 1 step per iteration
            A[i] = x;              // 0 or 1 step per iteration
        }
    }
}
```

| Iteration | `int[]` | `for` | `Read x` | `if` | `A[i] = x` |
| --- | --- | --- | --- | --- | --- |
| Before loop | 1 | – | – | – | – |
| i = 0 | – | 1 | 1 | 1 | 0 or 1 |
| i = 1 | – | 1 | 1 | 1 | 0 or 1 |
| i = 2 | – | 1 | 1 | 1 | 0 or 1 |
| **Total** | **1** | **n** | **n** | **n** | **0 to n** |

- **Best case** (every x ≤ 0, no assignments): 1 + n + n + n + 0 = **1 + 3n → O(n)**
- **Worst case** (every x > 0): 1 + n + n + n + n = **1 + 4n → O(n)**
- **Average case:** **O(n)**

The constants change (3n vs. 4n), but the growth rate is the same.

---

## Bubble Sort (with early exit)

```java
function BubbleSort(A, n){
    for(i = 0; to n-2){                 // L1  outer loop (passes)
        swapped = false                 // L2
        for(j = 0; to n-2-i){           // L3  inner loop (shrinks each pass)
            if(A[j] > A[j+1]){          // L4  comparison
                swap A[j] and A[j+1]    // L5
                swapped = true          // L6
            }
        }
        if(swapped == false){           // L7
            break                       // L8  already sorted, stop early
        }
    }
}
```

Each pass pushes the largest remaining value to the end, so the inner loop gets **one shorter every pass**. That shrinking is where the summation formula comes in.

### Per-pass table

| Pass | Outer for | swapped=false | Inner comparisons (L4) | Swaps, worst (L5) | Swaps, best (L5) |
| --- | --- | --- | --- | --- | --- |
| i = 0 (1st) | 1 | 1 | n − 1 | n − 1 | 0, then break |
| i = 1 (2nd) | 1 | 1 | n − 2 | n − 2 | – |
| i = 2 (3rd) | 1 | 1 | n − 3 | n − 3 | – |
| ⋮ | ⋮ | ⋮ | ⋮ | ⋮ | ⋮ |
| i = n − 2 (last) | 1 | 1 | 1 | 1 | – |
| **Total (worst)** | **n − 1** | **n − 1** | **(n² − n)/2** | **(n² − n)/2** | |

### BCS, WCS, ACS

| Case | Input | Comparisons | Swaps | Big O |
| --- | --- | --- | --- | --- |
| **Best** | Already sorted | n − 1 (one pass, then break) | 0 | **O(n)** |
| **Worst** | Reverse sorted | (n² − n)/2 | (n² − n)/2 | **O(n²)** |
| **Average** | Random order | ≈ (n² − n)/2 | ≈ (n² − n)/4 (about half of comparisons swap) | **O(n²)** |

If your class uses bubble sort without the `swapped` flag, the best case also becomes O(n²), because it always runs every pass.

## How to use the (n² + n)/2 summation formula

**The formula:**
1 + 2 + 3 + … + n = n(n + 1)/2 = **(n² + n)/2**

**Why it works (Gauss's trick):** write the sum forwards and backwards, then add the two lines:

```
  1   +   2   + ... + (n-1) +  n
  n   + (n-1) + ... +   2   +  1
-------------------------------------
(n+1) + (n+1) + ... + (n+1) + (n+1)   → n copies of (n+1)
```

That's n(n + 1), but you counted the sum twice, so divide by 2.

**How to apply it, step by step:**

1. Write out what the inner loop does on each pass: n−1, n−2, …, 2, 1.
2. Flip it into standard form: 1 + 2 + … + (n−1).
3. Look at the **last term**. The formula assumes the sum ends at n. Here it ends at **n − 1**, so replace every n with (n − 1):
    (n − 1)((n − 1) + 1)/2 = (n − 1)(n)/2 = **(n² − n)/2**
4. Drop the constants and lower terms: (n² − n)/2 → **O(n²)**

## Self-Check

1. What is the Big O of f(n) = 5n log n + 3n + 100?
2. A loop runs `for i = 0 to n−1` with another `for j = 0 to n−1` inside. How many times does the inner body run?
3. Use the summation formula to find 1 + 2 + … + 100.

Full walkthroughs of bubble, selection, and insertion sort → [[Data Structures & Algos/Class Notes/Lecture 10|Lecture 10]]
