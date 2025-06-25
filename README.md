# walmart_sqlProject

create database walmart;
use walmart;
select * from walmart;
desc walmart;
drop table walmart;


------------------- Feature Engineering -----------------------------
1. Time_of_day

SELECT time,
(CASE 
	WHEN `time` BETWEEN "00:00:00" AND "12:00:00" THEN "Morning"
	WHEN `time` BETWEEN "12:01:00" AND "16:00:00" THEN "Afternoon"
	ELSE "Evening" 
END) AS time_of_day
FROM walmart;

ALTER TABLE walmart ADD COLUMN time_of_day VARCHAR(20);

UPDATE walmart
SET time_of_day = (
	CASE 
		WHEN `time` BETWEEN "00:00:00" AND "12:00:00" THEN "Morning"
		WHEN `time` BETWEEN "12:01:00" AND "16:00:00" THEN "Afternoon"
		ELSE "Evening" 
	END
);



-- ------Day_name

SELECT date,
DAYNAME(date) AS day_name
FROM walmart;

ALTER TABLE walmart ADD COLUMN day_name VARCHAR(10);

UPDATE walmart
SET day_name = DAYNAME(date);

select * from walmart;
-- ----------Month_name

SELECT date,MONTHNAME(date) AS month_name
FROM walmart;

ALTER TABLE walmart ADD COLUMN month_name VARCHAR(10);

UPDATE walmart
SET month_name = MONTHNAME(date);



select * from walmart;


----------------Exploratory Data Analysis (EDA)----------------------

-- 1.How many distinct cities are present in the dataset?

select distinct(city) as City from walmart;
 
 -- 2.In which city is each branch situated?
 select distinct branch, city from walmart;
 
-- Product Analysis
-- 1.How many distinct product lines are there in the dataset?
select count(distinct product_line) from walmart;

-- 2.What is the most common payment method?
select payment, count(payment) as common_methodof_payment from walmart
group by payment order by common_methodof_payment DESC LIMIT 1;

-- 3.What is the most selling product line?
select product_line, count(product_line) as MostSelling_Product_line from walmart
group by product_line order by MostSelling_Product_line DESC LIMIT 1;

-- 4.What is the total revenue by month?
select * from walmart;
SELECT month_name, SUM(total) AS total_revenue
FROM walmart GROUP BY month_name ORDER BY total_revenue DESC;

-- 5.Which month recorded the highest Cost of Goods Sold (COGS)?
SELECT month_name, SUM(cogs) AS total_cogs
FROM walmart GROUP BY month_name ORDER BY total_cogs DESC;

-- 6.Which product line generated the highest revenue?
select * from walmart;
select product_line, sum(total) as highest_revenue from walmart
group by product_line order by highest_revenue desc limit 1 ;

-- 7.Which city has the highest revenue?
select city, sum(total) as Highest_revenue from walmart 
group by city order by highest_revenue  desc limit 1;

-- 8.Which product line incurred the highest VAT?
select product_line, sum(tax) as Highest_vat
FROM walmart GROUP BY product_line order by highest_vat  desc limit 1;

-- 10.Which branch sold more products than average product sold?
select branch, sum(quantity) as quantity 
from walmart group by branch having sum(quantity)>avg(quantity) order by quantity desc limit 1 ;

-- 11.What is the most common product line by gender?
select * from walmart;
select product_line, product_line,  count(gender) as total_gender from walmart
group by gender,product_line  order by total_gender desc;

-- 12.What is the average rating of each product line?
select product_line, round(avg(rating),2) as avg_rating 
from walmart group by product_line order by avg_rating desc;



----- ---------------------Sales Analysis
-- 1.Number of sales made in each time of the day per weekday
select * from walmart;
select day_name, time_of_day, count(invoice_id) as Total_sale 
from walmart group by day_name, time_of_day having day_name not in ('saturday','sunday') ;

-- 2.Identify the customer type that generates the highest revenue.
select customer_type , sum(total) as highest_revenue from
walmart group by customer_type  order by highest_revenue desc;

-- 3.Which city has the largest tax percent/ VAT (Value Added Tax)?
select city, sum(tax) as highest_tax from walmart
group by city order by highest_tax desc;

-- 4.Which customer type pays the most in VAT?
select customer_type, sum(tax) as highest_tax from walmart
group by customer_type order by highest_tax desc limit 1;



-- -------------------Customer Analysis

-- 1.How many unique customer types does the data have?
select count(distinct customer_type) from walmart;

-- 2.How many unique payment methods does the data have?
select count(distinct payment) from walmart;

