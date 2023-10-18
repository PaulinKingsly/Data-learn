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



# Физическая модель данных
![Image alt](https://github.com/PaulinKingsly/Data-learn/blob/main/module_2/%D1%84%D0%B8%D0%B7%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F_%D0%BC%D0%B4.PNG)

#DDL 
create schema dw;

CREATE TABLE dw.calendar
(
 "date"     date NOT NULL,
 year     int NOT NULL,
 quarter  varchar(50) NOT NULL,
 month    int NOT NULL,
 week     int NOT NULL,
 week_day int NOT NULL,
 CONSTRAINT PK_1 PRIMARY KEY ( "date" )
);

CREATE TABLE dw.geography
(
 geo_id      serial NOT NULL,
 country     varchar(50) NOT NULL,
 city        varchar(50) NOT NULL,
 "state"       varchar(50) NOT NULL,
 region      varchar(50) NOT NULL,
 postal_code int4range NOT NULL,
 CONSTRAINT PK_2 PRIMARY KEY ( geo_id )
);


CREATE TABLE dw.product
(
 product_id   serial NOT NULL,
 category     varchar(50) NOT NULL,
 subcategory  varchar(50) NOT NULL,
 product_name varchar(50) NOT NULL,
 CONSTRAINT PK_3 PRIMARY KEY ( product_id )
);

CREATE TABLE dw.shipping_dim
(
 ship_id   serial NOT NULL,
 ship_mode varchar(14) NOT NULL,
 CONSTRAINT PK_4 PRIMARY KEY ( ship_id )
);

CREATE TABLE dw.sales_fact
(
 row_id     int4range NOT NULL,
 order_id   varchar(14) NOT NULL,
 sales      numeric(9,4) NOT NULL,
 quantity   int4range NOT NULL,
 profit     numeric(21,16) NOT NULL,
 product_id int NOT NULL,
 "date"       date NOT NULL,
 ship_id    int NOT NULL,
 geo_id     int NOT NULL,
 CONSTRAINT PK_5 PRIMARY KEY ( row_id ),
 CONSTRAINT FK_1 FOREIGN KEY ( product_id ) REFERENCES dw.product ( product_id ),
 CONSTRAINT FK_2 FOREIGN KEY ( "date" ) REFERENCES dw.calendar ( "date" ),
 CONSTRAINT FK_3 FOREIGN KEY ( ship_id ) REFERENCES dw.shipping_dim ( ship_id ),
 CONSTRAINT FK_4 FOREIGN KEY ( geo_id ) REFERENCES dw.geography ( geo_id )
);

CREATE INDEX FK_1 ON dw.sales_fact
(
 product_id
);

CREATE INDEX FK_2 ON dw.sales_fact
(
 "date"
);

CREATE INDEX FK_3 ON dw.sales_fact
(
 ship_id
);

CREATE INDEX FK_4 ON dw.sales_fact
(
 geo_id
);


insert into dw.shipping_dim 
select 100+row_number() over (), ship_mode from (select distinct ship_mode from public.orders) a

select * from dw.shipping_dim 











