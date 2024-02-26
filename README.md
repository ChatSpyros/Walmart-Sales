# Walmart Sales Data Analysis



## About
This project aims to explore the Walmart Sales data to understand top performing branches and products, sales trend of of different products, customer behaviour. The aims is to study how sales strategies can be improved and optimized. The dataset was obtained from the [Kaggle Walmart Sales Forecasting Competition](https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting).


## Purposes Of The Project


The major aim of thie project is to gain insight into the sales data of Walmart to understand the different factors that affect sales of the different branches.


## About Data

This dataset contains sales transactions from a three different branches of Walmart, respectively located in Mandalay, Yangon and Naypyitaw. The data contains 17 columns and 1000 rows:


Column	Description	Data Type
invoice_id	Invoice of the sales made	VARCHAR(30)
branch	Branch at which sales were made	VARCHAR(5)
city	The location of the branch	VARCHAR(30)
customer_type	The type of the customer	VARCHAR(30)
gender	Gender of the customer making purchase	VARCHAR(10)
product_line	Product line of the product solf	VARCHAR(100)
unit_price	The price of each product	DECIMAL(10, 2)
quantity	The amount of the product sold	INT
VAT	The amount of tax on the purchase	FLOAT(6, 4)
total	The total cost of the purchase	DECIMAL(10, 2)
date	The date on which the purchase was made	DATE
time	The time at which the purchase was made	TIMESTAMP
payment_method	The total amount paid	DECIMAL(10, 2)
cogs	Cost Of Goods sold	DECIMAL(10, 2)
gross_margin_percentage	Gross margin percentage	FLOAT(11, 9)
gross_income	Gross Income	DECIMAL(10, 2)
rating	Rating	FLOAT(2, 1)



### Analysis List


1. Product Analysis
> Conduct analysis on the data to understand the different product lines, the products lines performing best and the product lines that need to be improved.
2. Sales Analysis
> This analysis aims to answer the question of the sales trends of product. The result of this can help use measure the effectiveness of each sales strategy the business applies and what modificatoins are needed to gain more sales.
3. Customer Analysis
> This analysis aims to uncover the different customers segments, purchase trends and the profitability of each customer segment.


## Approach Used


1. **Data Wrangling:** This is the first step where inspection of data is done to make sure **NULL** values and missing values are detected and data replacement methods are used to replace, missing or **NULL** values.
> 1. Build a database
> 2. Create table and insert the data.
> 3. Select columns with null values in them. There are no null values in our database as in creating the tables, we set **NOT NULL** for each field, hence null values are filtered out.


2. **Feature Engineering:** This will help use generate some new columns from existing ones.
> 1. Add a new column named `time_of_day` to give insight of sales in the Morning, Afternoon and Evening. This will help answer the question on which part of the day most sales are made.
> 2. Add a new column named `day_name` that contains the extracted days of the week on which the given transaction took place (Mon, Tue, Wed, Thur, Fri). This will help answer the question on which week of the day each branch is busiest.
> 3. Add a new column named `month_name` that contains the extracted months of the year on which the given transaction took place (Jan, Feb, Mar). Help determine which month of the year has the most sales and profit.

3. ** Exploratory Data Analysis (EDA): Exploratory data analysis is done to answer the listed questions and aims of this project.

## Business Questions To Answer
### Generic Question
1. How many unique cities does the data have?
2. In which city is each branch?


### Product
1. How many unique product lines does the data have?
2. What is the most common payment method?
3. What is the most selling product line?
4. What is the total revenue by month?
5. What month had the largest COGS?
6. What product line had the largest revenue?
7. What is the city with the largest revenue?
8. What product line had the largest VAT?
9. Which branch sold more products than average product sold?
10. What is the most common product line by gender?
11. What is the average rating of each product line?
### Sales
1. Number of sales made in each time of the day per weekday
2. Which of the customer types brings the most revenue?
3. Which city has the largest tax percent/ VAT (**Value Added Tax**)?
4. Which customer type pays the most in VAT?
### Customer
1. How many unique customer types does the data have?
2. How many unique payment methods does the data have?
3. What is the most common customer type?
4. Which customer type buys the most?
5. What is the gender of most of the customers?
6. What is the gender distribution per branch?
7. Which time of the day do customers give most ratings?
8. Which time of the day do customers give most ratings per branch?
9. Which day of the week has the best avg ratings?
10. Which day of the week has the best average ratings per branch?


## Revenue And Profit Calculations



•	COGS (Cost Of Goods Sold) = unitsPrice * quantity

•	VAT (Value Added Taxes) = 5 %* COGS 

•	total(gross_sales) = VAT + COGS

•	grossProfit(grossIncome) = total(gross_sales) - COGS


##SQL queries

```sql

CREATE DATABASE WalmartSales;

```
```sql
CREATE TABLE Sales(
   invoice_id VARCHAR(30) NOT NULL PRIMARY KEY,
   branch VARCHAR(5) NOT NULL,
   city VARCHAR(30) NOT NULL,
   customer_type VARCHAR(30) NOT NULL,
   gender VARCHAR(10) NOT NULL,
   product_line VARCHAR(100) NOT NULL,
   unit_price DECIMAL(10, 2) NOT NULL,
   quantity INT NOT NULL,
   VAT FLOAT(6, 4) NOT NULL,
   total DECIMAL(12, 5) NOT NULL,
   date DATETIME NOT NULL,
   time TIME NOT NULL,
   payment_method VARCHAR(15) NOT NULL,
   cogs DECIMAL(10, 2) NOT NULL,
   gross_margin_pct FLOAT(11, 9) NOT NULL,
   gross_income DECIMAL(12, 4) NOT NULL,
   rating FLOAT(2, 1)
   );
```

```sql
--   ---------time_of_day---------------



SELECT 
   time,
   (CASE
      WHEN time BETWEEN TIME '00:00:00' AND TIME '12:00:00' THEN 'Morning'
      WHEN time BETWEEN TIME '12:01:00' AND TIME '16:00:00' THEN 'Afternoon'
      ELSE 'Evening'
    END
    ) AS time_of_date
FROM sales;



ALTER TABLE sales ADD COLUMN time_of_day VARCHAR(20);

UPDATE sales
SET time_of_day = (CASE
      WHEN time BETWEEN TIME '00:00:00' AND TIME '12:00:00' THEN 'Morning'
      WHEN time BETWEEN TIME '12:01:00' AND TIME '16:00:00' THEN 'Afternoon'
      ELSE 'Evening'
    END
);

'''
'''sql
-- ----------- day name----------

SELECT 
     date,
     DAYNAME(date) AS day_name
FROM sales;     

ALTER TABLE sales ADD COLUMN day_name VARCHAR(10);

UPDATE sales
SET day_name = DAYNAME(date);


-- month_name--------------------------

SELECT 
     date,
     MONTHNAME(date)
FROM sales;

ALTER TABLE sales ADD COLUMN month_name VARCHAR(10);

UPDATE sales
SET month_name = MONTHNAME(date);


-- ----------------------------------------------------------------------------
-- ---------------------------------------Generic-----------------------------------

-- How many unique cities does the data have?

SELECT
     DISTINCT (city)
FROM sales;

SELECT
     DISTINCT (branch)
FROM sales;

--  In which city is each brach?

SELECT 
     DISTINCT(CITY),
     branch
FROM sales;


-- ----------------------------------------------------------
-- ---------------------PRODUCTS----------------------------

-- How many unique product lines does the data have?

SELECT 
      count(DISTINCT(product_line))
FROM sales;

-- What is the most common payment method ?

SELECT
     payment_method,
     COUNT(payment_method) AS cnt
FROM sales
GROUP BY payment_method
ORDER BY cnt DESC;

-- What is the most selling product line ?

SELECT
     product_line,
     COUNT(product_line) AS cnt
FROM sales
GROUP BY product_line
ORDER BY cnt DESC;

-- What is the total revenue by month?

SELECT
      month_name as Month,
      SUM(total) AS Total_Revenue
FROM sales
GROUP BY Month
ORDER BY Total_Revenue DESC;
     
-- What month had ther largest cost of goods sold (COGS)?

SELECT
      month_name as Month,
      SUM(	cogs) AS Cogs
FROM sales
GROUP BY Month
ORDER BY Cogs DESC;

-- What product line had the largest revenue?

SELECT
     product_line,
     SUM(total) AS Total_Revenue
FROM sales
GROUP BY product_line
ORDER BY Total_Revenue DESC;

-- What is the city with the largest revenue ?

SELECT
      city,
      SUM(total) AS Total_Revenue
FROM sales
GROUP BY city
ORDER BY Total_Revenue DESC;

-- What product line had the largest Value added tax (VAT);

SELECT
      product_line,
      AVG(VAT) AS avg_tax
FROM sales
GROUP BY product_line
ORDER BY avg_tax DESC;

-- Fetch each product line and add a column to those product line showing "Good", "Bad". Good if its greater than average sales

-- Which brand sold more products than average product sold?

SELECT
      branch,
      SUM(quantity) AS qty
FROM sales
GROUP BY branch
HAVING SUM(quantity) > (SELECT AVG(quantity) FROM sales);

-- What is the most common product line by gender?

SELECT
	gender,
    product_line,
    COUNT(gender) AS total_cnt
FROM sales
GROUP BY gender, product_line
ORDER BY total_cnt DESC;

-- What is the average rating of each product line?

SELECT
     product_line,
     ROUND(AVG(rating), 2)  AS avg_rating
FROM sales
GROUP BY product_line;
     
-- -----------------------------------------------------------------------------------
-- ----------------- ----------------------------Sales----------------------

-- Number of sales made in each time of the day per weekday

SELECT
	time_of_day,
    COUNT(*) AS total_Sales
FROM sales
WHERE day_name = "Monday"
GROUP BY time_of_day
ORDER BY total_Sales DESC ;

-- Which of the customer types bring the most revenue?

SELECT
     customer_type,
     SUM(total) AS Total_Revenue
FROM sales
GROUP BY customer_type
ORDER BY Total_Revenue DESC;
    
    
-- Which city has the largest tax percent/VAT (Value Added Tax)?

SELECT
     city,
     AVG(VAT) AS VAT
FROM sales
GROUP BY city
ORDER BY VAT DESC;

-- Which customer type pays the most in VAT?

SELECT
     customer_type,
     AVG(VAT) AS VAT
FROM sales
GROUP BY customer_type
ORDER BY VAT DESC;

-- ---------------------------------------------------------------------------------
-- --------------------------------------Customer---------------------------------------

-- How many qunique customer types does the data have?

SELECT
     DISTINCT(customer_type)
FROM sales;

-- How many unique payment methods does the data have ? 

SELECT
     DISTINCT(payment_method)
FROM sales;

-- Which customer type buys the most?

SELECT
      customer_type,
      COUNT(*) AS cstm_cnt
FROM sales
GROUP BY customer_type;

-- What is the gender of the most customers?

SELECT
      gender,
      COUNT(*) AS gender_cnt
FROM sales
GROUP BY gender
ORDER BY gender_cnt DESC;

-- What is the gender distribution per branch?

SELECT
      gender,
      COUNT(*) AS gender_cnt
FROM sales
WHERE branch = "C"
GROUP BY gender
ORDER BY gender_cnt DESC;

-- What time of the day do customers give most rating?

SELECT
      time_of_day,
      AVG(rating) AS avg_rating
FROM sales
GROUP BY time_of_day
ORDER BY avg_rating DESC;

-- Which time of the day do customers give most rating per branch?

SELECT
      time_of_day,
      AVG(rating) AS avg_rating
FROM sales
WHERE branch = "A"
GROUP BY time_of_day
ORDER BY avg_rating DESC;

-- Which day of the weeek has thes best avg rating ?

SELECT
      day_name,
      AVG(rating) AS avg_rating
FROM sales
GROUP BY day_name
ORDER BY avg_rating DESC;

-- Which day of the weeek has thes best avg rating per branch?

SELECT
      day_name,
      AVG(rating) AS avg_rating
FROM sales
WHERE branch = "A"
GROUP BY day_name
ORDER BY avg_rating DESC;


'''







