[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 12: Linear Search vs. Binary Search

## Linear search

Check every element **from first to last** until you find the target or run out of elements.

- Works on **any** list, sorted or not.
- Works on arrays and linked lists.

```java
public static int linearSearch(int[] A, int target) {
    for (int i = 0; i < A.length; i++) {
        if (A[i] == target) {
            return i;           // found: return its index
        }
    }
    return -1;                  // not found
}
```

| Case | Time | When |
| --- | --- | --- |
| Best | O(1) | Target is the first element |
| Worst | O(n) | Target is last, or not in the list |
| Average | O(n) | About n/2 checks |

## Binary search

Search a **sorted** list by checking the **middle** element and throwing away the half that can't contain the target.

1. Set `low = 0`, `high = n − 1`.
2. While `low <= high`:
    - `mid = (low + high) / 2`
    - If `A[mid] == target` → **found**
    - If `A[mid] < target` → target is to the right: `low = mid + 1`
    - If `A[mid] > target` → target is to the left: `high = mid − 1`
3. If `low > high`, the target is **not in the list**.

```java
public static int binarySearch(int[] A, int target) {
    int low = 0, high = A.length - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (A[mid] == target) {
            return mid;
        } else if (A[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}
```

### Example: `[3, 8, 12, 17, 23, 31, 42, 56]`

**Search for 31** (found in 2 checks):

| Step | low | high | mid | A[mid] | Action |
| --- | --- | --- | --- | --- | --- |
| 1 | 0 | 7 | 3 | 17 | 17 < 31 → low = 4 |
| 2 | 4 | 7 | 5 | 31 | **found** at index 5 |

**Search for 10** (not found):

| Step | low | high | mid | A[mid] | Action |
| --- | --- | --- | --- | --- | --- |
| 1 | 0 | 7 | 3 | 17 | 17 > 10 → high = 2 |
| 2 | 0 | 2 | 1 | 8 | 8 < 10 → low = 2 |
| 3 | 2 | 2 | 2 | 12 | 12 > 10 → high = 1 |
| 4 | 2 | 1 | – | – | low > high → **not found** |

| Case | Time | Why |
| --- | --- | --- |
| Best | O(1) | Target is the first middle |
| Worst / Average | **O(log₂ n)** | Each check halves the list |

For 1,000,000 elements, binary search needs at most about **20** checks; linear search may need 1,000,000.

## Comparison

| | Linear search | Binary search |
| --- | --- | --- |
| Needs sorted data? | No | **Yes** |
| Worst case | O(n) | O(log₂ n) |
| Works on linked lists? | Yes | Not efficiently (no O(1) access to the middle) |

## Why sort first?

Sorting is an **upfront cost** (O(n log n) with a good sort), but it's often worth it: once the data is sorted, **every** later search is O(log n) instead of O(n). If you search many times, the sort pays for itself.

The same halving idea powers **binary search trees** → [[Data Structures & Algos/Class Notes/Lecture 14|Lecture 14]]

## Self-Check

1. Trace binary search for 42 in the array above.
2. Why does binary search fail on an unsorted list?
3. About how many checks does binary search need for 1,024 elements?
