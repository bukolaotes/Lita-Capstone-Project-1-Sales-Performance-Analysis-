# Sales Performance Analysis

---
### Project Objective


This project aims to analyze monthly sales performance across different regions and product lines to identify trends and areas for improvement.

### Data Description:

The dataset "salesdata.xls" file  contains sales transaction records for the last two years, including columns like;

- Region
- Product
- Unit price
- Quantity
- Customer ID
- Order ID
- Order date


### Tools

- Excel - Data cleaniing and creating of pivot tables
- postgresql - Data Analytics
- Power BI - Creating of reports


### Data Cleaning/Prepearation


1.	Removed duplicate rows .
2.	standardized product names for consistent analysis across the dataset.


### Exploratory Data Analysis (EDA)

EDA involved exploring the sales data to answer key questions,such as :


- What is  total sales for each product category?
- What is the number of sales transactions in each region?
- Which product is the highest-selling  by total sales value?
- What is total revenue per product?
- What is the monthly sales totals for the current year?
- Who are the top 5 customers by total purchase amount?
- What is the percentage of total sales contributed by each region?
- which  products are with no sales in the last quarter?

### Data Analytics



```
Create table salesdata(Order_ID integer not null,
					   Customer_Id varchar(50),
					   Product varchar(50),
					   Region	varchar(50),
					   Order_Date	date,
					   Quantity integer,
					   Unit_Price integer,
					   TotalRevenue integer,
					   totalsales integer
					   );

```



```
select *  from salesdata;
```

------- What is  total sales for each product category--------
 
```
Select SUM(totalsales) AS totalsalesvalue,
FROM salesdata;
```
![image](https://github.com/user-attachments/assets/d5260f0c-c762-41f7-bc95-d5f86532e399)



------- What is the number of sales transactions in each region?------

```
Select product,Sum(totalsales) AS totalsalesvalue,
FROM salesdata,
group by product
order by totalsalesvalue desc;
```
![image](https://github.com/user-attachments/assets/6fb3f422-72a4-4690-a33c-83470b556947)



```
Select region,count(*)
AS
number_of_sales
FROM salesdata
group by region;
```


 

```
select product,sum(totalsales) 
as totalsalesvalue
FROM salesdata 
group by product
order by totalsalesvalue DESC
LIMIT 1;
```




```
 SELECT product,
sum(TotalRevenue)
AS totalrevenue
FROM  salesdata
group by product
order by totalrevenue desc;
```




```
SELECT  DATE_TRUNC('month',order_date)
AS month,
     sum(totalsales) AS totalsalesvalue
from salesdata
where
    EXTRACT(YEAR FROM order_date) =
EXTRACT(YEAR FROM CURRENT_DATE)
GROUP BY
    month
order by 
	month;
 ```



```
SELECT
Customer_Id,
sum(Quantity * Unit_Price) AS
Total_purchaseamount
from
   salesdata
 group by
    Customer_id
	order by
	  Total_purchaseamount DESC
	  LIMIT 5;
```




 ```
WITH total_sales AS (
SELECT
SUM(totalsales) AS TOTAL
FROM 
salesdata
)
 SELECT
 Region,
 SUM(totalsales) AS
 region_sales,
   (SUM(totalsales) * 100.0 /
   (SELECT total FROM total_sales)) AS
   percentage_of_total
   FROM
     salesdata
	 GROUP BY 
	  region;
 
  ``` 




 -------identify products with no sales in the last quarter-----


```
  WITH all_products AS(
   SELECT DISTINCT product
   FROM salesdata
),
recent_sales AS(
 SELECT DISTINCT product
FROM salesdata
WHERE Order_Date>=NOW() - INTERVAL '3 months'
)
SELECT
ap.product
FROM
all_products ap
WHERE
ap.product NOT IN (SELECT
product from recent_sales);

```






### RESULTS/FINDINGS

•	Shoe product has the highest sales from the south region totaling 546,300 for both 2023(247,500) and 2024(298,800), which is 26% of total sales of 2,101,090.
•	Shoes product has the highest Revenue From the south region totaling 546,300 for both 2023(247500)   and 2024(298800), which is 26% of total sales of 2,101,090.
•	East and north has the highest count of product bought  in 1489 each in 2023, all count of product bought dropped in all regions in 2024 for example in the east region it 
        dropped from 1489 to 994, in north region 1489 to 992, in south region 1488 to 992, in west region 1486 to 991
•	East and north has the highest count of customers totaling 1489 each in  2023







### Recommendation


 Focus marketing efforts on top-performing regions and product lines.
 Address underperforming regions or product categories with targeted strategies
 Explore opportunities for cross-selling or upselling

