## Flow of 2.0.0 
1. SaveLoanBlob is the entry point to queue a message -> SaveLoanBlobQueue
2. SaveLoanBlobQueue dequeues the message for it to processed asyncly 
3. Save the message in Azure blob storage as .json to then be processed -> SaveLoanToSql 
4. SaveLoanToSql is triggered when blob arrives 