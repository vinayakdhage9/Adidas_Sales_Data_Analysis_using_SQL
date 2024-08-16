# Adidas_Sales_Data_Analysis_using_SQL

1.What is the average price per unit of products sold?
select avg(Price_per_Unit) as avg_price from adidas_sales.adidas ;
 
 -- How many total units were sold
select sum(Units_Sold) from adidas_sales.adidas;

-- What is the total sales amount for the dataset?
select sum(Total_Sales) from adidas_sales.adidas;


-- How many unique retailers are there?
select count(distinct(Retailer)) as unique_retail from adidas_sales.adidas; 

-- How many sales transactions occurred in each month?
select Month, count(Total_Sales) as transaction_count from adidas_sales.adidas group by Month;

-- Top 5 Products by Sales:
select Product,sum(Total_Sales) as sales from adidas_sales.adidas group by Product order by sales DESC LIMIT 5;

-- Monthly Sales Trends:
select Month, sum(Total_Sales) Sales from adidas_sales.adidas group by Month ;

-- Sales by Region:
select Region,Round(sum(Total_Sales),0) as Sales from adidas_sales.adidas group by Region;

-- Sales Growth Over Time:
select Year ,sum(Total_Sales) as Sale from adidas_sales.adidas group by Year;

-- Calculate the monthly sales for each product in each region and find the top 3 products by sales amount for each region for the year 2021.
with Monthly_Sale as 
( select Month,Product,Region,sum(Total_Sales) as Sales from adidas_sales.adidas where Year=2021 group by Month,Product,Region) , 
Ranked_Sales as ( select Month,Product, Region,Sales ,
Rank() over (partition by Month,Region order by Sales desc) as Sales_Rank
from Monthly_Sales)
select Month, Product, Region,Sales from Ranked_Sales where Sales_Rank <=3 order by Month ,Region, Sales_Rank;
 
-- Find the total sales for each region and product category for the first quarter of 2021.
select Region,Product ,round(sum(Total_Sales),0) as Sales from adidas_sales.adidas where Invoice_Date between '01-01-2021' and '31-03-2021' 
group by Region, Product;

-- Identify customers who made purchases in both online and retail channels in 2021.
select distinct Retailer_ID,Sales_Method from adidas_sales.adidas where Year=2021 and Sales_Method in ('Online' ,'In-store') ;

-- Calculate the average sales amount per transaction for each month in 2021.
create view max as select Month ,round(avg (Total_Sales),0) as Sales from adidas_sales.adidas where year=2021 group by Month;

select Month ,round(avg (Total_Sales),0) as Sales,max(Total_Sales) as max_Sales from adidas_sales.adidas where year=2021 group by Month;

-- Find the top 2 customers by total sales amount in 2021.
select Retailer_ID,round(sum(Total_Sales),0) as Sales from adidas_sales.adidas where year=2021 group by Retailer_ID order by Sales desc limit 2 ;

-- Calculate the total number of sales transactions and the total sales amount for each product in 2021.
select count(*) as total_transaction ,Product, sum(Total_Sales) as Sales from adidas_sales.adidas where year=2021 group by Product;

-- Identify the month with the highest sales amount in 2021.
select Month,sum(Total_Sales) as Sales from adidas_sales.adidas where year=2021 group by Month order by Sales desc limit 1;
