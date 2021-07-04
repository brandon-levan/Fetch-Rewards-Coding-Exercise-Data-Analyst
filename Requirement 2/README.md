# Second: Write a Query that Directly Answers a Predetermined Question from a Business Stakeholder
 
Below are the sql queries that answer all of the questions for requirement two of the assessment. I cleaned the data using **[Python](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/Requirement%203/Fetch%20Rewards%20Assignment%20-%20Data%20Cleaning%20and%20Quality%20Issue%20Check.ipynb)**. From each of the JSONs (Users, Receipts, Brands), I generated a cleaned CSV and loaded them into a local instance of MySQL database (Fetch schema) for writing these SQL queries and producing results. I did both the data cleansing and data quality issue investiagation in the same **[Notebook](https://github.com/brandon-levan/Fetch-Rewards-Coding-Exercise-Data-Analyst/blob/main/Requirement%203/Fetch%20Rewards%20Assignment%20-%20Data%20Cleaning%20and%20Quality%20Issue%20Check.ipynb)**. 

## Questions 1 & 2 
1. What are the top 5 brands by receipts scanned for most recent month?
2. How does the ranking of the top 5 brands by receipts scanned for the recent month compare to the ranking for the previous month?

```sql
with receipt_info as (
SELECT b.name AS Brand_Name,
case
    WHEN a.dateScanned between DATE_SUB((select max(createDate) from Fetch.receipts), INTERVAL 1 MONTH) and (select max(createDate) from Fetch.receipts) THEN "Last Month"
    WHEN a.dateScanned between DATE_SUB((select max(createDate) from Fetch.receipts), INTERVAL 2 MONTH) and DATE_SUB((select max(createDate) from Fetch.receipts), INTERVAL 1 MONTH) THEN "Month Before Last"
	ELSE "Out of Range"
end as Date_Range,
count(distinct(a._id)) AS Number_of_Scans
FROM Fetch.receipts a 
left join Fetch.brands b on a.barcode = b.barcode
where a.dateScanned >= '2021-01-01 00:00:00.000'
group by 1,2
order by 2,3 desc),

ranking as (
select 
Date_Range,
Brand_Name,
Number_of_Scans,
	RANK() OVER (
		PARTITION BY Date_Range
        ORDER BY Number_of_Scans DESC
	) Brand_Ranking
from receipt_info
where date_range != "Out of Range"
)

select *
from ranking
where Brand_Ranking <= 5;
```
| Date_Range        | Brand_Name            | Number_of_Scans | Brand_Ranking |
|-------------------|-----------------------|-----------------|---------------|
| Last Month        | NULL                  | 449             | 1             |
| Month Before Last | NULL                  | 631             | 1             |
| Month Before Last | Swanson               | 11              | 2             |
| Month Before Last | Tostitos              | 11              | 2             |
| Month Before Last | Cracker Barrel Cheese | 10              | 4             |
| Month Before Last | Diet Chris Cola       | 4               | 5             |
| Month Before Last | Prego                 | 4               | 5             |

## Questions 3 & 4 
3. When considering average spend from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
4. When considering total number of items purchased from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?

```sql
with receipt_info as (
select _id,
totalSpent,
purchasedItemCount,
case
	WHEN rewardsReceiptStatus = 'FINISHED' THEN "Accepted"
    WHEN rewardsReceiptStatus = 'REJECTED' THEN "Rejected"
	ELSE "Other" 
end as Receipt_Status
FROM Fetch.receipts 
where rewardsReceiptStatus in ('FINISHED','REJECTED')
group by 1,2,3,4
)

select Receipt_Status,
round(avg(totalSpent),2) AS Average_Spend_Per_Receipt,
sum(purchasedItemCount) AS Total_Items_Purchased
from receipt_info
group by 1;
```
| Receipt_Status | Average_Spend_Per_Receipt | Total_Items_Purchased |
|----------------|---------------------------|-----------------------|
| Accepted       | 84.18                     | 8070                  |
| Rejected       | 23.33                     | 173                   |

## Questions 5 & 6 
5. Which brand has the most spend among users who were created within the past 6 months?
6. Which brand has the most transactions among users who were created within the past 6 months?

```sql
with receipt_info as (
SELECT 
a._id,
a.barcode,
a.quantityPurchased,
a.finalPrice,
case
	WHEN rewardsReceiptStatus = 'FINISHED' THEN "Accepted"
    WHEN rewardsReceiptStatus = 'REJECTED' THEN "Rejected"
	ELSE "Other" 
end as Receipt_Status
FROM Fetch.receipts a 
left join Fetch.users b on a.userId = b._id
where b.createdDate > DATE_SUB((select max(createDate) from Fetch.receipts), INTERVAL 6 MONTH)
and a.rewardsReceiptStatus in ('FINISHED','REJECTED')
group by 1,2,3,4,5
)

select name,
round(sum(finalPrice),2) AS Total_Spend,
sum(quantityPurchased) AS Total_Items_Purchased
from receipt_info a
left join Fetch.brands b on a.barcode=b.barcode
group by 1
order by 3 desc;
```
| name                  | Total_Spend | Total_Items_Purchased |
|-----------------------|-------------|-----------------------|
| NULL                  | 15738.23    | 3438                  |
| Swanson               | 61.38       | 22                    |
| Tostitos              | 76.67       | 22                    |
| Cracker Barrel Cheese | 196.98      | 7                     |
| Kettle Brand          | 11.07       | 3                     |
| V8                    | 13.49       | 3                     |
| Jell-O                | 4.99        | 2                     |
| Cheetos               | 22.00       | 2                     |
| Prego                 | 2.69        | 1                     |
| Diet Chris Cola       | 2.69        | 1                     |
| Grey Poupon           | 3.29        | 1                     |
| Pepperidge Farm       | 3.00        | 1                     |
| Quaker                | 3.99        | 1                     |
