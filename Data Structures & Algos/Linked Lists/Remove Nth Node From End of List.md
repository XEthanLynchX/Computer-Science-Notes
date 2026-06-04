---
date: 2026-06-04
Difficulty: Medium
Type: Linked List / Two Pointers
---

## **Problem: 
![[Remove Nth Node From End of List problem.png]]

---
## **Note: 


---

## **Time and Space Complexity: 
- Time is O(n) - Brute and optimal are the same but Brute has more passes while optimal is a one pass
- Space is O(1) - Space is only take by the pointers 

--- 

## **Brute Force: 
```python 
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        prev = None 
        current = head

        while current: 
            temp = current.next
            current.next = prev
            prev = current 
            current = temp 
        
        reversed_head = prev
        
        if n == 1:
            reversed_head = reversed_head.next
        else:
            curr = reversed_head
            count = 1

            while count < n - 1:
                curr = curr.next
                count += 1

            if curr.next:
                curr.next = curr.next.next

        prev = None
        current = reversed_head

        while current: 
            temp = current.next
            current.next = prev
            prev = current 
            current = temp 

        return prev

```

---
## **My Solution: 
Same as brute 


---
## **Optimal Solution: 

