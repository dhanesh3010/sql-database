create database wallmart;
use wallmart;
show tables;
select * from walmartsalesdata limit 10;
select count(*) from walmartsalesdata;
rename table walmartsalesdata to walmart;
show tables;
select * from walmart limit 5;
#BASIC BUISINESS QUESTION
alter table walmart change `product line` product_line TEXT;
alter table walmart change gross_margin_percentage gross_margin_percentage decimal(5,2);
alter table walmart change Tax_percentage tax_percentage decimal(5,2);
alter table walmart change `Customer type` customer_type text;
alter table walmart change `gross income` gross_income decimal(5,2);
#what is the countof distinct cities in the dataset 
select count(distinct(city)) from walmart;
#for each branch what is the corresponding city
select distinct city,branch from walmart;
# what is the count of distinct product line in the dataset
select count(distinct`Product_line`) from walmart;
#which payment method occur most frequently?
select payment,count(*) as frequency from walmart group by payment order by frequency desc limit 1;
#which product line has the higest sales?
select product_line, sum(quantity) as total_sales from walmart group by product_line order by total_sales desc;
#which city has the higest revenue
select city, sum(total) as revenue$ from walmart group by city order by revenue$ desc;
#swhich  product line has the higest revenue
select product_line, avg(rating) as avg_rating from walmart group by product_line; 
#which product line has the highest profit margin and how does it vary across cities
select product_line,city,round(sum(gross_income) /sum(cogs)*100,2) as profit_margin  from walmart group by product_line,city order by profit_margin;
#Are there products with high quantity sold but low gross income? What’s the ROI?
SELECT Product_line,SUM(Quantity) AS total_quantity,SUM(gross_income) AS total_income,    
ROUND(SUM(gross_income) / SUM(cogs) * 100, 2) AS ROI_percentage
FROM walmart GROUP BY Product_line ORDER BY total_quantity DESC, total_income ASC LIMIT 5;
# Do male and female customers favor different product lines or payment methods?
SELECT Gender,Product_line,Payment,COUNT(*) AS transaction_count
FROM walmart GROUP BY Gender,product_line,payment ORDER BY Gender, transaction_count DESC;
#What’s the average customer rating for invoices over ₹500 vs. under ₹500?
select  case when total >500 then'over 500'else 'under 500' end as invoices,round(avg(rating)) as customer_rating  from walmart group by invoices 
# Which payment method results in the highest average transaction value?
select  payment,round(avg(total),2) as average_transcation from walmart group by Payment order by average_transcation desc;
 #What’s the gross margin percentage per payment type?
 select payment,count(gross_margin_percentage) as gross_percentage from walmart group by payment;
#Classify product lines as 'High', 'Medium', or 'Low' performing based on average gross income.
SELECT product_line,AVG(gross_income) AS avg_income,
CASE WHEN AVG(gross_income) > 2000 THEN 'High' WHEN AVG(gross_income) BETWEEN 1000 AND 2000 THEN 'Medium' ELSE 'Low' END AS performance_category
FROM walmart GROUP BY product_line;
