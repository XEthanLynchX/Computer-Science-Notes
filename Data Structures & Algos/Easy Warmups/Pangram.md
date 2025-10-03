---
date: 2025-10-01
Difficulty: Easy
Type: Hash table / String
---
## **Problem: 
![[Pangram 1.png]]

--- 

## **Notes: 
- Create a string that is set too alphabet to check against sentence
- Clear any whitespace from string and lowercase to match alpha string
- for each letter in alpha check if each letter is not in the sentence(pay attention to this wording)

---

## **Time and Space Complexity
- Time is O(n) - The time we take to loop through is based on the size of the sentence
- Space is O(1) - Were storing 2 things which is constant O(1)

---

## **Solution: 

![[Pangram.png]]
