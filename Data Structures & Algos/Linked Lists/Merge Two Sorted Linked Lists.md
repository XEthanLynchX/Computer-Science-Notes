---
date: 2026-05-26
Difficulty: Easy
Type: Linked List / Recursion
---

## **Problem: 
![[Merge Two Sorted Linked Lists Problem.png]]

---
## **Note: 
- Using a dummy node (head of linked list with no value) makes this a lot easier 
- This problem is just comparison between list1 value and list 2 value 
- Make sure to update the nodes after setting them (including the dummy node)
- 

---

## **Time and Space Complexity: 
- Time is O(n + m)  - Have to iterate through two full linked lists  
- Space is O(1) - Only things stored in memory are the pointers we are using which are list1, list2, tail, and dummy node

--- 

## **Brute Force: 
```python 
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        if list1 and not list2: 
            return list1
        elif not list1 and list2: 
            return list2 
        elif not list1 and not list2: 
            return None

        if list1.val < list2.val: 
            head = list1
            tail = list1
            list1 = list1.next
        else: 
            head = list2
            tail = list2 
            list2 = list2.next

        while list1 and list2: 
            if list1.val < list2.val:
                tail.next = list1
                list1 = list1.next
                tail = tail.next
            else: 
                tail.next = list2 
                list2 = list2.next
                tail = tail.next

        if list1: 
            tail.next = list1 
        elif list2: 
            tail.next = list2 

        return head
        
```

---
## **My Solution:  
Same as brute


---
## **Optimal Solution: 
```python
class Solution:
	def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:

dummy = ListNode()
tail = dummy

while list1 and list2:
	if list1.val < list2.val:
		tail.next = list1
		list1 = list1.next

	else:
		tail.next = list2
		list2 = list2.next
	tail = tail.next
	
	if list1:
		tail.next = list1
	
	elif list2:
		tail.next = list2

return dummy.next
```

