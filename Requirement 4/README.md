# Fourth: Communicate with Stakeholders

## Subject : Resolving Data Quality Issues and Concerns

Hello Product/Business Team Lead,

I'm working on a request to determine the top brands for the last month by quantity of transactions and spend, and I noticed a few data quality issues in our Receipts, Brands, and Users tables that could be of concern as we continue to scale the number of receipts that are scanned and prcocessed each day. 

I will continue to dig into these data issues, but below is the current list of data quality issues that I've uncovered. I'm hoping that you could answer a few questions that follow that would help me resolve some of these issues and determine the amount of risk associated with each issue. 

Issues - 
* There are many null values in Receipts across various colunns, but most importantly, there are null values in __rewardsReceiptItemList__. This is critical because this is where the information about each item purchased per each receipt is found. Without it, we cannot map ra brand back to the receipt or items on the receipt.
* For bonusPointsEarned, pointsEarned, purchasedItemCount, and totalSpent I created box and whisker plot to determine if there were outliers for each of these fields. There are outliers in all columns except for bonusPointsEarned. If these receipts are not already flagged by Fetch's receipt review logic, they should be looked at again because there may be issues with these receipts.
* There are duplicated user ids (id) in the User table and duplicated barcode ids (barcode) in the brands table
    * There can be different brands associated to one barcode. For example, barcode 511111305125 can be either Chris Image Test	or Rachael Ray Everyday. 
    * There are multiple rows for the same user id. User id is not unique in the User table.
* There are 148 User IDs in Receipts that do not have a matching User ID in the User table
* There are various types of barcode formats in Receipts. For example, there are barcodes with varying lengths and data types (041129412152, 2700719497082, 4067, B07BRRLSVC)

With these data quality issues in mind, could you please help me understand the following - 
1. Who manages the brands table? I'd like to speak with the manager/owner of that table in order to work with him or her to create a 1-to-1 list of brands and barcodes so that we don't run into more discrepencies down the line. Maybe we should do a reconciliation of this table monthly to ensure it's accurate? Additionally, the format of these barcodes are not consistent (e.g (041129412152, 2700719497082, 4067, B07BRRLSVC); maybe I can assist with creating a tool that flags barcodes when not entered into the system in the correct format. 
2. I've uncovered some outliers in the data, but I'd like to get a better understanding of whether or not the outliers I flagged are truely outliers? Are there certain thresholds in which Fetch considers a value an outlier? 
3. How are users in the user table generated? I've uncovered instances were there is a user in the receipts table that is not in the user table? Maybe these are some use cases in which users aren't properly being added to the user table. I would assume that the user needs to be created before they scan, but could you please confirm this?
4. Lastly, one of the most concerning issues I uncovered were non-existant values in the rewardsReceiptItemList where there is an ID already generated for the receipt? Do you know what the cause of this is? Without information in rewardsReceiptItemList we will not know what the user purchased and we cannot attirbute any awards for their purchases. 

If it's easier for you, please let me know if you want to set some time up to review this note. I'll continue to look into these discrepencies and uncover more in the meantime, but your responses will help me determine the severity of these issues. 

Thank you<br/>
Brandon
