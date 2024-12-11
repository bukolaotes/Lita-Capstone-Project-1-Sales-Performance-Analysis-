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


-  retrieve the total sales for each product category.
-  find the number of sales transactions in each region.
-  find the highest-selling product by total sales value.
-  calculate total revenue per product.
-  calculate monthly sales totals for the current year.
-  find the top 5 customers by total purchase amount.
-  calculate the percentage of total sales contributed by each region.
-  identify products with no sales in the last quarter.

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
![image](https://github.com/user-attachments/assets/6fb3f422-72a4-4690-a33c-83470b556947)




------- What is the number of sales transactions in each region?------

```
Select product,Sum(totalsales) AS totalsalesvalue,
FROM salesdata,
group by product
order by totalsalesvalue desc;
```
![image](https://github.com/user-attachments/assets/da522e15-bbe2-4c6f-95be-30f379a47b8d)


----- Which product is the highest-selling  by total sales value-------

```
select product,sum(totalsales) as totalsalesvalue
from salesdata 
group by product
order by totalsalesvalue DESC
LIMIT 1;
```
![image](https://github.com/user-attachments/assets/fd45d72a-b443-404b-864a-bdcb823c7ce6)


 
-----calculate total revenue per product------

```
SELECT product,
sum(TotalRevenue)  as totalrevenue
from salesdata
group by product
order by totalrevenue desc

```
![image](https://github.com/user-attachments/assets/6dc0105e-58e3-4213-8d34-5029cd7445f7)



------calculate monthly sales totals for the current year------
```

select DATE_TRUNC('month',order_date)
AS month,
     sum(  totalsales) AS totalsalesvalue

from salesdata
where
    EXTRACT(YEAR FROM order_date) =
EXTRACT(YEAR FROM CURRENT_DATE)
GROUP BY
    month
order by 
	month;
	
 
```
![image](https://github.com/user-attachments/assets/36271504-815b-44b3-a099-4d8e65288a98)



---------find the top 5 customers by total purchase amount------
```
select 
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
![image](https://github.com/user-attachments/assets/8100bd5d-1151-4b79-a820-e5d8e28af40f)



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

