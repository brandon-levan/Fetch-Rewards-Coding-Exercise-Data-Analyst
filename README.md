# Fetch Rewards Coding Exercise - Data Analyst

Thank you for taking the time to review my exercise :smile:. **I've layed out the assignment in this [pdf](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/README.md) for easy viewing**, but I've also included all of the code and documentation below and in this repository for your reference. If you have any questions please, do not hestitate to reach out to me at brandon.levan1014@gmail.com :sunglasses:

## First: Review Existing Unstructured Data and Diagram a New Structured Relational Data Model

The requirement was to review the unstructured JSON data and diagram a new structured relational data model. Below is my relational data model. 

![Image of Relational Diagram](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/Requirement%201/Structured_Relational_Data_Model.png?raw=true)

## Second: Write a Query that Directly Answers a Predetermined Question from a Business Stakeholder

You can find these SQL queries for this assignment requirement in the **[Requirement 2 Folder](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/Requirement%202/Requirement_Two.sql)**. I've answered the six questions using only three queries. I've visualized the results below (which you can also find in the Requirement 2 folder.

## Third: Evaluate Data Quality Issues in the Data Provided

Please go to the Data Quality Issues Check section of my Jupyter Notebook found **[here](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/Requirement%203/Fetch%20Rewards%20Assignment%20-%20Data%20Cleaning%20and%20Quality%20Issue%20Check.ipynb)** (Fetch Rewards Assignment - Data Cleaning and Quality Issue Check.ipynb). 
 
From my analysis, I've found the following data quality issues - 
* There are many null values in Receipts across various colunns, but most importantly, there are null values in __rewardsReceiptItemList__. This is critical because this is where the information about what is pruchased per each receipt is found in this field. Without it, we cannot map data to the brand information. 
* For bonusPointsEarned, pointsEarned, purchasedItemCount, and totalSpent I created box and whisker plots to determine if there were outliers for these fields. There are outliers in all columns except for bonusPointsEarned. If these receipts are not already flagged by Fetch's receipt review logic, they should be looked at again because there may be issues with these receipts.
* There are duplicated user ids (id) in the User table and duplicated barcode ids (barcode) in the brands table
    * There can be different brands associated to one barcode. For example, barcode 511111305125 can be either Chris Image Test	or Rachael Ray Everyday. 
    * There are multiple rows for the same user id. User id is not unique 
* There are 148 User IDs in Receipts that do not have a matching User ID in the User table
* There are various types of barcode formats in receipts. For example, there are barcodes with varying lengths and data types (041129412152, 2700719497082, 4067, B07BRRLSVC)

## Fourth: Communicate with Stakeholders


