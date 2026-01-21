---
date: 2026-01-21
Difficulty: Medium
Type: Array / Hashing
---

## **Problem: 
![[Group Anagrams.png]]
---
## **Note: 


---

## **Time and Space Complexity: 
- Time is O() - 
- Space is O() -

--- 

## **Brute Force: 


---
## **My Solution: 


---
## **Optimal Solution: 
``` python 
class Solution:

    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # defaultdict creates a value if no value is present for that key
        # in this case it automatically creates a list []
        res = defaultdict(list)
		# for each word create a "bucket" for each letter (to keep count of letters present)
        for w in strs:
            count = [0] * 26
            for l in w:
            # increment count of each letter (only works lowercase)
                count[ord(l) - ord('a')] += 1
            # Use the count as the key and append the word to it 
            # If it doesn't exist defaultdict will create a count 
            res[tuple(count)].append(w)

        return list(res.values())
```


