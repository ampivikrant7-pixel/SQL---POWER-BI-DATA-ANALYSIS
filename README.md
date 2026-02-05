# SQL---POWER-BI-DATA-ANALYSIS
SQL TO SORT AND CLEAN DATA.
WITH ORDERS_ALL as (
SELECT
 OrderID,
 customerID,
 productID,
 OrderDate,
 Quantity,
 Revenue,
 COGS
 FROM Orders_2023

 UNION ALL 
 SELECT
 OrderID,
 CustomerID,
 ProductID,
 OrderDate,
 Quantity,
 Revenue,
 COGS
 FROM Orders_2024

 UNION ALL
 SELECT
 OrderID,
 CustomerID,
 ProductID,
 OrderDate,
 Quantity,
 Revenue,
 COGS
 FROM Orders_2025)

 SELECT
 a.OrderID,
 a.CustomerID,
 c.Region,
 a.ProductID,
 a.OrderDate,
 DATEADD(WEEK, DATEDIFF(week,0,a.OrderDate),0) as week_date,
 c.CustomerJoinDate,
 a.Quantity,
 a.Revenue,
 CASE WHEN a.Revenue is null then p.price* a.Quantity else a.Revenue end as cleanedRevenue,
 a.Revenue - a.COGS as profit,
 a.COGS,
 p.ProductName,
 p.ProductCategory,
 p.Price,
 p.Base_Cost
 FROM ORDERS_ALL a
 left join customers c on a.CustomerID=c.CustomerID
 left join products p  on a.ProductID=p.ProductID
 where a.CustomerID is not null ---dropping the non customer ids
