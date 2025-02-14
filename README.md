Retail Sales Data Analysis

Overview

This project is a SQL-based retail sales data analysis that provides various insights into customer transactions. The dataset consists of sales transactions, including details such as date, time, customer demographics, product categories, quantity sold, and total sales amount.

The SQL queries used in this project cover a wide range of data analysis techniques, including aggregation, filtering, ranking, and grouping. The objective is to extract meaningful insights and trends from the retail sales data.

Database Schema

The project starts by creating a table Retail_sales with the following fields:

DROP TABLE IF EXISTS Retail_sales;
CREATE TABLE Retail_sales(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME, 
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT);

Data Cleaning

Checking for NULL values across all fields:

SELECT * FROM Retail_sales
WHERE 
    transactions_id IS NULL  OR
    sale_date IS NULL OR
    sale_time IS NULL OR
    customer_id IS NULL OR
    gender IS NULL OR
    age IS NULL OR
    category IS NULL OR
    quantiy IS NULL OR
    price_per_unit IS NULL OR
    cogs IS NULL OR
    total_sale IS NULL;

Deleting records with NULL values to ensure data consistency:

DELETE FROM Retail_sales
WHERE 
    transactions_id IS NULL  OR
    sale_date IS NULL OR
    sale_time IS NULL OR
    customer_id IS NULL OR
    gender IS NULL OR
    age IS NULL OR
    category IS NULL OR
    quantiy IS NULL OR
    price_per_unit IS NULL OR
    cogs IS NULL OR
    total_sale IS NULL;

Key Analysis Performed

1. General Sales Insights

Total Sales Revenue:

SELECT SUM(total_sale) AS Total_sales FROM Retail_sales;

Total Number of Unique Customers:

SELECT COUNT(DISTINCT customer_id) AS total_Customer FROM Retail_sales;

Total Quantity Sold:

SELECT SUM(quantiy) AS Total_quantity FROM Retail_sales;

List of Unique Product Categories:

SELECT DISTINCT category FROM Retail_sales;

2. Transaction-based Queries

Retrieve all sales made on a specific date ('2022-11-5'):

SELECT * FROM Retail_sales
WHERE sale_date = '2022-11-5';

Find all transactions where the category is 'Clothing' and quantity sold is greater than or equal to 4:

SELECT * FROM Retail_sales
WHERE category = 'Clothing' AND quantiy >= 4;

Retrieve transactions where total sales exceed 1000:

SELECT * FROM Retail_sales
WHERE total_sale > 1000;

3. Category-wise Analysis

Total Sales for Each Category:

SELECT category, SUM(total_sale) AS Total_sales
FROM Retail_sales
GROUP BY category
ORDER BY 2 DESC;

Average Age of Customers Purchasing Beauty Products:

SELECT AVG(age) FROM Retail_sales
WHERE category = 'Beauty';

Number of Transactions by Gender for Each Category:

SELECT category, gender, COUNT(transactions_id) AS no_of_transactions
FROM Retail_sales
GROUP BY category, gender
ORDER BY 1;

4. Customer Insights

Top 5 Customers Based on Highest Total Sales:

SELECT customer_id, SUM(total_sale) AS Total_sales
FROM Retail_sales
GROUP BY customer_id
ORDER BY 2 DESC
LIMIT 5;

Number of Customers Who Purchased from Each Category:

SELECT category, COUNT(DISTINCT customer_id) AS No_of_Customers
FROM Retail_sales
GROUP BY category
ORDER BY 2 DESC;

5. Time-based Analysis

Best-Selling Month in Each Year Using Ranking and Aggregation:

SELECT * FROM (
    SELECT EXTRACT(YEAR FROM sale_date ) AS year, 
    EXTRACT(MONTH FROM sale_date) AS month, 
    AVG(total_sale) AS total_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale)) AS rank 
    FROM Retail_sales
    GROUP BY 1,2
    ORDER BY 1,2
) AS t1
WHERE rank = 1;

Number of Orders in Different Shifts:
