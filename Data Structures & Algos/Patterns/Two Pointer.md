## **Overview**

The **Two Pointer** technique involves using two indices (or pointers) to traverse data structures like arrays or strings. It’s especially useful for problems involving **sorted arrays**, **searching pairs**, or **partitioning** data efficiently.

Depending on the problem, the pointers may:

- Start at opposite ends and move toward each other.
- Start at the same point and move forward at different speeds.

---

## **What Does it Look Like?**

Example: Given a **sorted array** `arr = [1, 2, 3, 4, 6]` and a target sum `6`, find if any two numbers sum to the target.

Using two pointers:

- Start with `left = 0` and `right = len(arr) - 1`
- Move inward based on the sum

Result: `2 + 4 = 6` → Found!

---

## **How to Recognize This Pattern**

You’re likely dealing with a two-pointer problem if:

- The input is a **sorted array or string**.
- You need to **find pairs**, **remove duplicates**, or **reverse** elements.
- The problem involves **minimizing/maximizing** something with a linear scan.

Common keywords: _pair sum_, _sorted_, _window_, _reverse_, _remove duplicates_, _merge_, _palindrome_, _intersection_.

---

## **Example Implementation**

Here’s a Python example to find if a pair in a sorted array sums to a target:


```Python
def has_pair_with_sum(arr, target):
    left, right = 0, len(arr) - 1

    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return True
        elif current_sum < target:
            left += 1
        else:
            right -= 1

    return False
```

**Example usage:**
```Python
arr = [1, 2, 3, 4, 6]
target = 6
print(has_pair_with_sum(arr, target))  # Output: True
```

---

## **Applied Problems**

Here are some classic problems that use the two-pointer technique:

- **Two Sum II - Input Array Is Sorted (Leetcode 167)**
- **Remove Duplicates from Sorted Array (Leetcode 26)**
- **Container With Most Water (Leetcode 11)**
- **Valid Palindrome (Leetcode 125)**