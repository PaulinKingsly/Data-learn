# SQL запросы по вопросам первого модуля 

--Total Sales = 2297201

SELECT ROUND(SUM(Sales), 0 ) AS Total_Sales FROM [dbo].[Orders]

-- Total Profit = 286397

SELECT ROUND(SUM(Profit), 0 ) AS Total_Sales FROM [dbo].[Orders]

--Profit per Order

SELECT SUM(Profit)  FROM [dbo].[Orders]
GROUP BY [Order ID]

--Sales per Customer

SELECT ROUND(SUM(Sales), 0) AS Total_Sales_Per_Customer, [Customer Name] FROM [dbo].[Orders]
GROUP BY [Customer Name]
Order by Total_Sales_Per_Customer desc

--Avg. Discount = 16%

SELECT Round(AVG(Discount), 2) * 100 as avg_disc  FROM [dbo].[Orders]


--Monthly Sales by Segment

SELECT ROUND(SUM(Sales), 0) AS Sales, Segment, DATEPART(month, [Order Date]) as month,  DATEPART(year, [Order Date]) as year FROM [dbo].[Orders]
GROUP BY Segment, DATEPART(month, [Order Date]),  DATEPART(year, [Order Date])
ORDER BY year, month

--Monthly Sales by Product Category 

SELECT ROUND(SUM(Sales), 0) AS Sales, Category, DATEPART(month, [Order Date]) as month,  DATEPART(year, [Order Date]) as year FROM [dbo].[Orders]
GROUP BY Category, DATEPART(month, [Order Date]),  DATEPART(year, [Order Date])
ORDER BY year, month

--Sales and Profit by Customer + ranking

SELECT ROUND(SUM(Sales), 0) AS Total_Sales_by_Customer, 
ROUND(SUM(Profit), 0) AS Total_Profit_by_Customer, [Customer Name], COUNT([Order ID]) AS mumber_of_orders
FROM [dbo].[Orders]
GROUP BY [Customer Name]
ORDER BY Total_Profit_by_Customer DESC

--Sales per region

with max_sales as (SELECT ROUND(SUM(Sales), 0) as sum_sale FROM [dbo].[Orders] GROUP BY Region)

SELECT ROUND(SUM(Sales), 0) AS Total_Sales_per_Region, Region, (SELECT MAX(sum_sale) from max_sales) - ROUND(SUM(Sales), 0) as sales_difference_with_max_sales
FROM [dbo].[Orders]
GROUP BY Region
ORDER BY Total_Sales_per_Region DESC

# Концептуальная модель данных

![Image alt](https://github.com/PaulinKingsly/Data-learn/blob/main/module_2/%D0%BA%D0%BE%D0%BD%D1%86%D0%B5%D0%BF%D1%82.PNG)

# Логическая модель данных
![Image alt](https://github.com/PaulinKingsly/Data-learn/blob/main/module_2/%D0%BB%D0%BE%D0%B3%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F.PNG)


