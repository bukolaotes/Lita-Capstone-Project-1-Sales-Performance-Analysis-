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
select *  from salesdata




---- the total sales value------
Select SUM(totalsales) AS totalsalesvalue
FROM salesdata

---- retrieve the total sales for each product category------
Select product,Sum(totalsales) AS totalsalesvalue
FROM salesdata
group by product
order by totalsalesvalue desc
 


--------find the number of sales transactions in each region----
Select region,count(*) AS
number_of_sales
from salesdata
group by region;

------ find the highest-selling product by total sales value----- 

select product,sum(totalsales) as totalsalesvalue
from salesdata 
group by product
order by totalsalesvalue DESC
LIMIT 1;

 
-----calculate total revenue per product------

SELECT product,
sum(TotalRevenue)  as totalrevenue
from salesdata
group by product
order by totalrevenue desc


------calculate monthly sales totals for the current year------
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
	
---------find the top 5 customers by total purchase amount------
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

---------	calculate the percentage of total sales contributed by each region------
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
 
   
   
   -------identify products with no sales in the last quarter-----
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