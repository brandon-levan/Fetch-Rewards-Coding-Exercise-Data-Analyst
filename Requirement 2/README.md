# Requirement 2 Solutions

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
