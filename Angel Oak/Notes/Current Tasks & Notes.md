## Task 
--- 
**Look into sending a batch of loans as a single job in PDD** 


## Notes 
--- 
- **LDD** processes loans in batches internally, but PDD currently calls LDD per loan (single-job mode).
- If you switch PDD to send a batch to LDD:
    - LDD throws an error on the first failed loan.
    - PDD interprets that as the entire batch failing → all loans marked as failed.
- You need **partial success handling** so successful loans aren’t marked failed.



## Solutions 
--- 
## Proposed solution 
- Have LDD return result object with the loans that have succeeded rather than throwing an error 

```json 
{
  "batchId": "12345",
  "results": [
    { "loanId": "L1", "status": "SUCCESS" },
    { "loanId": "L2", "status": "FAILED", "error": "Reason" }
  ]
}
```
- Then PDD can check this result as processed and store
- LDD stores the failed results in a poison blob on a per loan basis rather the whole entire job allowing us to gather the successful loans and fix the failed loans (partial success)
- Would have to tweak the LDD response slightly and PoisonJobHandler to handle loans payloads rather than just a JOB 
- 