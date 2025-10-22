First check to see in loadloan data if the email should been sent if diff was detected 

# Datebase to send email 
ALWAYS bump up the time 
and at least 1 other change to another field 

```sql
UPDATE Config SET tValue = '2025-10-17T12:14:46.000Z' WHERE tVariableName = 'MostRecentLockDate';

UPDATE LoanField 
SET t2592 = '10/17/2025 12:14:46 PM', 
t356 = '700000' 
WHERE tLoanGuid = 'D4DA6533-D03F-4DFE-B77A-717BF7F40DF6';
```

