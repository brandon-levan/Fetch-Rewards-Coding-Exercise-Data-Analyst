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
1. 





Thank you
Brandon
