# Big (O)
### Given the equation f(n) = $2n^3$ - $3n^2$ + 5n + 10 
- $2n^3$ is the **dominating term** which is how we find our growth rate
- $3n^2$ + 5n + 10 are **non-dominating terms** which don't matter to find our growth rate
- Our **Growth rate** is O($n^3$) = cubic growth
### What's excluded from the Big (O) analysis
- The two factors in the Big(O) equation we don't take into account is the **Hardware** and **input**

### Order of Magnitude - Is the average of the best case scenario and worst case of the algorithm

```java
function ReadArray(n){ 
	int[] A = new int[n]
	for(i=0; to n-1){ 
		Read x
		if(x > 0){ 
			A[i] = x
		}	
	}
}
```

| Iteration   | int[] | for | Read x | If  | A[i]   |
| ----------- | ----- | --- | ------ | --- | ------ |
| Before loop | 1     | 0   | –      | –   | –      |
| i = 0 (1st) | –     | 1   | 1      | 1   | 0 or 1 |
| i = 1 (2nd) | –     | 1   | 1      | 1   | 0 or 1 |
| i = 2 (3rd) | –     | 1   | 1      | 1   | 0 or 1 |
Best case scenario = 1+n+n+0 = (1 + 2n) = O(n)
Worst case scenario = 1 + n + n +n = (1 +3n) = O(n)
Avg Case = O(n) 

### Bubble Sort (with early exit)

java

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

#### Per-pass table

|Pass|Outer for|swapped=false|Inner comparisons (L4)|Swaps, worst (L5)|Swaps, best (L5)|
|---|---|---|---|---|---|
|i = 0 (1st)|1|1|n − 1|n − 1|0, then break|
|i = 1 (2nd)|1|1|n − 2|n − 2|–|
|i = 2 (3rd)|1|1|n − 3|n − 3|–|
|⋮|⋮|⋮|⋮|⋮|⋮|
|i = n − 2 (last)|1|1|1|1|–|
|**Total (worst)**|**n − 1**|**n − 1**|**(n² − n)/2**|**(n² − n)/2**||

#### BCS, WCS, ACS

|Case|Input|Comparisons|Swaps|Big O|
|---|---|---|---|---|
|**Best**|Already sorted|n − 1 (one pass, then break)|0|**O(n)**|
|**Worst**|Reverse sorted|(n² − n)/2|(n² − n)/2|**O(n²)**|
|**Average**|Random order|≈ (n² − n)/2|≈ (n² − n)/4 (about half of comparisons swap)|**O(n²)**|

If your class uses bubble sort without the `swapped` flag, the best case also becomes O(n²), because it always runs every pass.

### How to use the (n² + n)/2 summation formula

**The formula:**  
1 + 2 + 3 + … + n = n(n + 1)/2 = **(n² + n)/2**

**Why it works (Gauss's trick):** Write the sum forwards and backwards, then add the two lines together:

```
  1   +   2   + ... + (n-1) +  n
  n   + (n-1) + ... +   2   +  1
-------------------------------------
(n+1) + (n+1) + ... + (n+1) + (n+1)   → n copies of (n+1)
```

That's n(n+1), but you counted the sum twice, so divide by 2.

**How to apply it, step by step:**

1. Write out what the inner loop does on each pass: n−1, n−2, …, 2, 1.
2. Flip it into standard form: 1 + 2 + … + (n−1).
3. Look at the **last term**. The formula assumes the sum ends at n. Here it ends at **n − 1**, so replace every n with (n − 1):  
    (n − 1)((n − 1) + 1)/2 = (n − 1)(n)/2 = **(n² − n)/2**
4. Drop the constants and lower terms: (n² − n)/2 → **O(n²)**


