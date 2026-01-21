- Find out the right way to describe which properties are assigned to each level 
and if the api is doing the right way by assigning the level in the body vs the path or if need to change it to maybe a path param to differ each body  to level 
- The api looks at the rules to see what fields the rules are using to see what's required and not you can rely on the api because it gets required fields constinally 
- Check with Austin how fields are being required (if they're truly required or conditionally required)
- Check if swagger docs requires optional params 
- Look at the stored procs 
- ![[Current task proc.png]]
- The db will tell the values that are required and negvalues are the values not allowed 


TODO 
Get access to test db
and download quick quote price 




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





QQ Notes from Austin
- Quick Quote uses QuickMapping to get the schemas  (like JOI)
- QQ and QM are the only ones to be focused on 
- Price adjustments if you are in the black you add it to the rate and percentage of the number in 30 yr fixed rate based pricing table 
- Quick Quote returns an array of results for every program and you specify a program it will only return for that program and also only validate fields for that program 
- Required means if its in the program and its required then you need it, otherwise if its in the program and its not required you don't necessary need it \
- QQRequestProperties are all of the fields that are rules
- QQRequestvalues are all of the values you can select for the text properties 
- A get and post request are the same its just how you pass the data
- The validation folder is important when write the DOCS 
- Program needs to be required > level 1 
- QuickQuote > QuickQual > QDU > QDUCreditReport 
- QuickMapping Creates the schema for QQ to access 
- QuickMapping is a good place to start because you can get all of the params 
- Instance fields is an array and is a normal numeric field 
- All array fields are instance fields (other than credit report)
- Don't worry about level 4 right now 