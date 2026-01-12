Past Notes needed for pdd ldd client/service
- Encompass was updated to fix isRemoved bug issue 
- Tested that isRemoved is set to false by default in call so in LDD made it pass to the call itself so that is acutally can be set to true if needed 
- The loans are including a few more docs then old PDD, however EVERYTHING expected to be filtered is being filtered correctly and contains the all old pdd data files (Could be a filter I couldn't find ) but if what we are filtering out now is okay then it's working as expected
- How I tested Made sure the documents being tested didn't have new documents uploaded since the file had been handed to me (To ensure that we are calling the same amount of attachments that old PDD was) Then Checked the total attachments array in Postman and then compared to attachment array LDD called from encompass api, Then in Postman went through everything that should be filtered out (Unassigned, removed, 'trash', and inactive) and counted total documents we should have after filter and compared with LDD which in every case was filtering out the same amount expected to filtered out. 




**Current Task:** 
Look all the apps in GitHub that have the word velocify 

Concentrate here
[https://github.com/AngelOakCompanies/VelocifyDataTransfer](https://github.com/AngelOakCompanies/VelocifyDataTransfer "https://github.com/angeloakcompanies/velocifydatatransfer")
Find out what's going on here

Learn some Vue (Udemy Course)
[https://github.com/AngelOakCompanies/ClosingPackageUpload](https://github.com/AngelOakCompanies/ClosingPackageUpload "https://github.com/angeloakcompanies/closingpackageupload")



# Understanding Velocify

## Index.ts 
1. Ran on a timer trigger that runs sync function 
	**Update Call Report Data:**
    - Fetches “yesterday’s” call data from the Velocify API.
    - Stores that data in the SQL database.
	**Soft Deleting Leads:**
    - Looks at leads present in the SQL database from the last six months.
    - Compares with what Velocify currently returns. 
    - Marks any leads missing from the API response (but present in DB) as “soft deleted” (not actually deleting, but flagging as deleted).
	
	**Fetching Updated/Modified Leads:**
    - Fetches leads from the Velocify API that changed within the last X minutes (configurable).
    - Inserts or updates these leads in the SQL database.


Auth is HMAC SIGNATURE AUTH