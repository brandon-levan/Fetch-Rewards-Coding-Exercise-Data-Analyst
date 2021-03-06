# Third: Evaluate Data Quality Issues in the Data Provided

Please go to the Data Quality Issues Check section of my Jupyter Notebook found **[here](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/Requirement%203/Fetch%20Rewards%20Assignment%20-%20Data%20Cleaning%20and%20Quality%20Issue%20Check.ipynb)** (Fetch Rewards Assignment - Data Cleaning and Quality Issue Check.ipynb). 
 
From my analysis, I've found the following data quality issues - 
* There are many null values in Receipts across various colunns, but most importantly, there are null values in __rewardsReceiptItemList__. This is critical because this is where the information about what is pruchased per each receipt is found in this field. Without it, we cannot map data to the brand information. 
* For bonusPointsEarned, pointsEarned, purchasedItemCount, and totalSpent I created box and whisker plots to determine if there were outliers for these fields. There are outliers in all columns except for bonusPointsEarned. If these receipts are not already flagged by Fetch's receipt review logic, they should be looked at again because there may be issues with these receipts.
* There are duplicated user ids (id) in the User table and duplicated barcode ids (barcode) in the brands table
    * There can be different brands associated to one barcode. For example, barcode 511111305125 can be either Chris Image Test	or Rachael Ray Everyday. 
    * There are multiple rows for the same user id. User id is not unique 
* There are 148 User IDs in Receipts that do not have a matching User ID in the User table
* There are various types of barcode formats in receipts. For example, there are barcodes with varying lengths and data types (041129412152, 2700719497082, 4067, B07BRRLSVC)
