---
date: 2026-05-28
Difficulty: Medium
Type: Linked List / Recursion
---

## **Problem: 
![[Reorder Linked Lists problem.png]]
---
## **Note: 
- For the optimal solution solution you can use slow and fast pointer technique to get the half point of the linked list
- Then reverse the second half of the linked list 
-  Your prev pointer and the midpoint.next need to be set to None
- The midpoint.next(slow.next) connection must be severed otherwise  

---

## **Time and Space Complexity: 
- Time is O(n) - There are no nested loops making it O(n) 
- Space is O(1) - The only being stored are the pointers which never change in size

--- 

## **Brute Force: 
```python 
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        if not head:
            return

        nodes = []
        cur = head
        while cur:
            nodes.append(cur)
            cur = cur.next

        i, j = 0, len(nodes) - 1
        while i < j:
            nodes[i].next = nodes[j]
            i += 1
            if i >= j:
                break
            nodes[j].next = nodes[i]
            j -= 1

        nodes[i].next = None
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
    def reorderList(self, head: Optional[ListNode]) -> None:
        slow, fast = head, head.next

        while fast and fast.next: 
            slow = slow.next 
            fast = fast.next.next 
        
        half = slow.next
        prev = slow.next = None

        while half: 
            tmp = half.next
            half.next = prev
            prev = half 
            half = tmp 

        first, second = head, prev
        while second: 
            tmp1, tmp2 = first.next, second.next 
            first.next = second
            second.next = tmp1 
            first, second = tmp1, tmp2
        
# (I got this partially but not all the way)
```

---
## **Optimal Solution: 
( Same as My solution)
