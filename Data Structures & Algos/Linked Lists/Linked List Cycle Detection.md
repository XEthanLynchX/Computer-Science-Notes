---
date: 2026-05-27
Difficulty: Easy
Type: Linked List / Recursion
---

## **Problem: 
![[Linked List Cycle Detection.png]]

---
## **Note: 
- For circular detection you can move one pointer two steps and one pointer 1 step and check for equivalence and they will eventually meet 
- 


---

## **Time and Space Complexity: 
- Time is O(n) - Takes up to as long as the list length
- Space is O(1) - Only Storing pointers which dont ever change in size

--- 

## **Brute Force: 
```python 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        seen = set()
        cur = head
        while cur:
            if cur in seen:
                return True
            seen.add(cur)
            cur = cur.next
        return False
```

---
## **My Solution: 
```python 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if head: 
            l, r = head, head.next 
        else: 
            return False
        while r: 
            if r.next == None: 
                return False
            if l == r: 
                return True
            r = r.next.next
            l = l.next
        return False
```

---
## **Optimal Solution: (Same as mine but cleaner)
```python 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```