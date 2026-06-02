1. What Should happen if data lookup fails (copying messages temporarily)




2. What is the requirement for a message to accepted rather than dropped in the filtering process or all we all copying all messages? 




3. - What fields are **mandatory vs optional** for passing validation? Is it the same as what's in the plant-api repo? 




4. What are the **timeout + retry policies**?



5. What is the **MaxRetries Count**?



6. Should retries use **exponential backoff** or fixed intervals?



7. What is the **process for replaying DLQ messages** after someone has gone in manually to see what could be wrong?




