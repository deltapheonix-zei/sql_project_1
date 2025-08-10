# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `sql_project_1` 

I used this project to showcase my SQL skills and the techniques I’d apply as a data analyst to explore, clean, and analyze retail sales data. It covers setting up a retail sales database, running exploratory data analysis (EDA), and answering business questions through SQL queries. It reflects how I approach real-world data analysis and build a strong SQL foundation.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

* **Database Creation**: The project starts by creating a database named `sql_project_1`.
* **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_project_1;
USE sql_project_1;

CREATE TABLE retail_sales
(
	transactions_id INT PRIMARY KEY,
	sale_date DATE NULL,
	sale_time TIME NULL,
	customer_id INT NULL,
	gender VARCHAR(10) NULL,
	age INT NULL,
	category VARCHAR(35) NULL,
	quantity INT NULL,
	price_per_unit FLOAT NULL,
	cogs FLOAT NULL,
	total_sale FLOAT NULL
);
```

### 2. Data Exploration & Cleaning

* **Record Count**: Determine the total number of records in the dataset.
* **Customer Count**: Find out how many unique customers are in the dataset.
* **Category Count**: Identify all unique product categories in the dataset.
* **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT *
FROM retail_sales
WHERE 
    sale_date      IS NULL OR
    sale_time      IS NULL OR
    customer_id    IS NULL OR
    gender         IS NULL OR
    age            IS NULL OR
    category       IS NULL OR
    quantity       IS NULL OR
    price_per_unit IS NULL OR
    cogs           IS NULL;

DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL OR
    sale_date       IS NULL OR 
    sale_time       IS NULL OR
    gender          IS NULL OR
    category        IS NULL OR
    quantity        IS NULL OR
    cogs            IS NULL OR
    total_sale      IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05'**:

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND sale_date BETWEEN '2022-11-01' AND '2022-11-30'
  AND quantity >= 4;
```

3. **Write a SQL query to calculate the total sales (total\_sale) for each category**:

```sql
SELECT category, 
       SUM(total_sale) AS net_sales,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**:

```sql
SELECT ROUND(AVG(age), 2) AS Average_age
FROM retail_sales
WHERE category = 'Beauty';
```

5. **Write a SQL query to find all transactions where the total\_sale is greater than 1000**:

```sql
SELECT * 
FROM retail_sales
WHERE total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction\_id) made by each gender in each category**:

```sql
SELECT category,
       gender,
       COUNT(*) AS Total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category, gender;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:

```sql
SELECT year,
       month,
       avg_sale
FROM (    
    SELECT year,
           month,
           avg_sale,
           RANK() OVER(PARTITION BY year ORDER BY avg_sale DESC) AS rnk
    FROM ( 
        SELECT YEAR(sale_date) AS year,
               MONTH(sale_date) AS month,
               ROUND(AVG(total_sale), 2) AS avg_sale
        FROM retail_sales
        GROUP BY year, month
    ) AS monthly_data
) AS ranked_data
WHERE rnk = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales**:

```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category**:

```sql
SELECT category,    
       COUNT(DISTINCT customer_id) AS unique_cs
FROM retail_sales
GROUP BY category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:

```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN HOUR(sale_time) < 12 THEN 'Morning'
            WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift,
       COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

## Findings

* **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
* **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
* **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
* **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

* **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
* **Trend Analysis**: Insights into sales trends across different months and shifts.
* **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project highlights my ability to handle the full workflow of a data analyst, from database setup and data cleaning to exploratory analysis and answering business-focused questions with SQL. The insights gained show how I can use sales patterns, customer behavior, and product performance to support better business decisions.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database and Run Queries**: Use the SQL scripts provided in the `retail_sales_analysis_p1.sql` file to create and populate the database, then execute the included queries to perform your analysis.
3. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Jay Mevada

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch.

* **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/jkmevada)
