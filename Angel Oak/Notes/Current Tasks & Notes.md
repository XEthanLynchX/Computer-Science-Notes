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




## LDD Service
## 1. **Orchestrator Logic ([DownloadLoanOrchestrator.ts](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html))**

- **Error Handling:**
    
    - Instead of throwing errors for a failed loan, the orchestrator now catches errors and sets [success: false](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) and [errorMessage](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) in the response object for that loan.
    - The orchestrator calls the `PoisonMessageHandler` activity with the [loanId](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) and error details, so failed loans are tracked individually.
    - The orchestrator continues processing other loans, allowing partial success.
- **Response Structure:**
    
    - Each loan’s result includes:
        - [success: true](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) or `false`
        - [errorMessage](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) if failed
        - [attachments](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) array (with per-attachment success/error info)

---

## 2. **Top-Level Orchestrator ([LoanDocumentDownloader.ts](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html))**

- **Batch Handling:**
    - The main orchestrator runs all loans in parallel and collects their results.
    - It only throws and calls `PoisonMessageHandler` for infrastructure/authentication errors (not for individual loan failures).
    - The final response to the client includes all loan results, showing which succeeded and which failed.

---

## 3. **Poison Message Handler (`PoisonJobHandler.ts`)**

- **Loan-Level Poison Tracking:**
    - The handler now accepts an optional [loanId](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) and logs/stores poison messages for individual failed loans.
    - Blob naming convention:
        - Loan failures: [poison-loan-{loanId}-{timestamp}-{invocationId}.json](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
        - Job failures: [poison-job-{timestamp}-{invocationId}.json](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

---

## 4. **Types ([DownloadLoansOrchestratorResponse.ts](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html), `PoisonMessage.ts`)**

- **Response Types Updated:**
    - Added [success: boolean](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) and [errorMessage?: string](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) to loan response types.
    - `PoisonMessage` type now includes optional [loanId](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) for per-loan tracking.

---

## 5. **Tests ([DownloadLoanOrchestrator.test.ts](vscode-file://vscode-app/c:/Users/ELynch/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html))**

- **Test Logic Updated:**
    - Tests now expect the orchestrator to return a failed loan response (not throw).
    - The generator test flow was updated to handle the yield for `PoisonMessageHandler` before completion.

---

## 6. **Documentation**

- **README and Copilot Instructions:**
    - Updated to explain partial success, per-loan error handling, and poison message conventions.