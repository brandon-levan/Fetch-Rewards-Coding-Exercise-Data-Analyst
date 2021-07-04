# Fourth: Communicate with Stakeholders

## Subject : Resolving Data Quality Issues and Concerns

Hello Product/Business Team Lead,

I'm working on a request to determine the top brands for the last month by quantity of transactions and spend, and I noticed a few data quality issues in our Receipts, Brands, and Users tables that could be of concern to us as we continue to scale up the number of receipts that are scanned and prcocessed. 

I'm will continue to dig into these data issues, but below is the current list of data quliaty issues I've uncovered, and I was hoping that you could answer a few questions that would help me resolve some of these issues and understand how big of a concern these issues are to you. 

* There are many null values in Receipts across various colunns, but most importantly, there are null values in __rewardsReceiptItemList__. This is critical because this is where the information about what is pruchased per each receipt is found in this field. Without it, we cannot map data to the brand information. 
* For bonusPointsEarned, pointsEarned, purchasedItemCount, and totalSpent I created box and whisker plots to determine if there were outliers for these fields. There are outliers in all columns except for bonusPointsEarned. If these receipts are not already flagged by Fetch's receipt review logic, they should be looked at again because there may be issues with these receipts.
* There are duplicated user ids (id) in the User table and duplicated barcode ids (barcode) in the brands table
    * There can be different brands associated to one barcode. For example, barcode 511111305125 can be either Chris Image Test	or Rachael Ray Everyday. 
    * There are multiple rows for the same user id. User id is not unique 
* There are 148 User IDs in Receipts that do not have a matching User ID in the User table
* There are various types of barcode formats in receipts. For example, there are barcodes with varying lengths and data types (041129412152, 2700719497082, 4067, B07BRRLSVC)

With these data quality issues in mind, could you please help me understand, 
1. Who manages the brands table? I'd like to speak with the manager of that table in order to work with him or her to create a 1 to 1 list of brands and barcodes so that we don't run into more discrepencies down the line. Maybe we should do a reconciliation of this table monthly to ensure it's accurate? Additionally, the format of these barcodes are not consistent (e.g (041129412152, 2700719497082, 4067, B07BRRLSVC); maybe I can assist with creating a tool that flags barcodes when not entered in the correct format. 
2. I've uncovered some outliers in the data, but I'd like to get a better understanding if the outliers I flagged are truely outliers? Are there certain thresholds in which Fetch considers a value an outlier? 
3. How are users in the user table generated? I've uncovered some instances were there is a user in the receipts table that is not in the user table? Maybe these are some use cases in which users aren't properly being added to the user table. I would assume that the user needs to be created before they scan, but could you please confirm this?
4. Lastly, one of the most concerning issues I uncovered were blank rewardsReceiptItemList, when there is an 

What do you need to know to resolve the data quality issues?
What other information would you need to help you optimize the data assets you're trying to create?
What performance and scaling concerns do you anticipate in production and how do you plan to address them?





Thank you
\n Brandon
