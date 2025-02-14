# SQL_Retail_Sales1
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
	total_sale FLOAT)

	SELECT * from Retail_sales

	SELECT * FROM  
	Retail_sales
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
	total_sale IS NULL
	
	--Delete null values-- 
	DELETE FROM  
	Retail_sales
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
	total_sale IS NULL

	SELECT * FROM Retail_sales

	--Total Sales--
	SELECT sum(total_sale) AS Total_sales FROM Retail_sales

	--Count of Customer--
	SELECT COUNT(DISTINCT customer_id) AS total_Customer FROM Retail_sales

	--Total Qty Sold--
	SELECT sum(quantiy) AS Total_quantity FROM Retail_sales
	
	--Total Categories--
	SELECT DISTINCT category  FROM Retail_sales
	
	--All Sales made on 22-11-5--
	SELECT * FROM Retail_sales
	WHERE sale_date = '2022-11-5'

	--All transactions where category is clothing and qty sold is 4 or more than 4--
	SELECT * FROM Retail_sales
	WHERE category = 'Clothing' AND quantiy >= 4

	--Total Sales for each category--
	SELECT category, SUM(total_sale) as Total_sales
	FROM Retail_sales
	GROUP BY category
	ORDER BY 2 DESC

	--Average age of customers who purchased item from Beauty category--
	SELECT AVG(age)
	FROM Retail_sales
	WHERE category = 'Beauty'

	--Find all transactions where total sale is greater than 1000--
	SELECT * 
	FROM Retail_sales
	WHERE total_sale >1000

	--Find number of transaction made by each gender in each category--
	SELECT category, gender, COUNT(transactions_id) as no_of_transactions
	FROM Retail_sales
	GROUP BY category, gender
	ORDER BY 1

	--Average sale of each month, best selling in each month of year--
	
	SELECT * FROM
	(
	SELECT EXTRACT(YEAR FROM sale_date ) as year, 
	EXTRACT(MONTH FROM sale_date) as month, 
	AVG(total_sale) as total_sale,
	RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date ) ORDER BY AVG(total_sale)) as rank 
	FROM Retail_sales
	GROUP BY 1,2
	ORDER BY 1,2
	) AS t1
	WHERE rank = 1

	-- Top 5 Customers based on highest total sales--

	SELECT customer_id, SUM(total_sale) as Total_sales
	FROM Retail_sales
	GROUP BY customer_id
	ORDER BY 2 DESC
	LIMIT 5

	-- No. of customers who purchased from each category--
	
	SELECT category, COUNT(DISTINCT customer_id) AS No_of_Customers
	FROM Retail_sales
	GROUP BY category
	ORDER BY 2 DESC

	-- Each shift, number of orders, example: Morning <=12, Afternoon 12-17, Evening >=17--

	WITH Hourly AS 
	(SELECT  
		*,
		CASE WHEN EXTRACT(HOUR FROM sale_time) <12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
		END AS Shift
	FROM Retail_sales)
	SELECT shift, COUNT(transactions_id) AS Total_orders
	FROM Hourly
	GROUP BY shift
	ORDER BY 2 DESC
	
