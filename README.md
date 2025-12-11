📊 Project Title: Epik E-commerce Sales Analysis

Level: Beginner–Intermediate
Database: Epik_sale_Analysis

This project demonstrates real SQL skills used in e-commerce analytics.
The dataset was extracted from Epik, an e-commerce company where I worked for 3 months.
It contains 2 years of sales data, including customer information, product categories, and transaction-level details.

The project involves creating a database, cleaning the data, performing EDA, and answering business-driven questions using SQL. It is ideal for anyone building a strong foundation in SQL for real-world business analytics.

🎯 Objectives

Database setup: Create and populate the Epik_sale_Analysis database with real sales data.

Data cleaning: Identify missing/unwanted records (e.g., removed Clothing category).

Exploratory Data Analysis (EDA): Understand customers, categories, and purchase behavior.

Business insights: Use SQL queries to answer key operational and revenue-related questions.

E-commerce analytics: Analyze trends, customer purchasing patterns, monthly performance, and peak sales times.

📁 Project Structure
1. Database Setup

A table named epik_sales was created to store transaction-level e-commerce data.
It includes transaction details like sale date, time, customer details, category, quantity sold, COGS, and total sales.

USE Epik_sale_Analysis;

CREATE TABLE epik_sales (
    transactions_id INT PRIMARY KEY AUTO_INCREMENT,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender ENUM('female','male','other'),
    age INT,
    category VARCHAR(20),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

2. Data Exploration & Cleaning

Preview the data

SELECT * FROM epik_sales LIMIT 10;


Total number of sales records

SELECT COUNT(*) AS Total_rows FROM epik_sales;


Remove entire Clothing category (business requirement)

DELETE FROM epik_sales WHERE category = 'clothing';


Number of unique customers

SELECT COUNT(DISTINCT customer_id) AS Total_Customers FROM epik_sales;


Total unique product categories

SELECT COUNT(DISTINCT category) AS Total_Categories FROM epik_sales;
SELECT DISTINCT category FROM epik_sales;

📊 3. Data Analysis & Business Questions

Below are the exact SQL queries written to extract meaningful insights from Epik sales:

Q1. Retrieve all sales made on 2022-12-07
SELECT * FROM epik_sales 
WHERE sale_date = '2022-12-07';

Q2. All Beauty category transactions with quantity >1 in Nov 2022
SELECT *
FROM epik_sales
WHERE category = 'beauty' 
  AND sale_date BETWEEN '2022-10-01' AND '2022-10-31'
  AND quantiy > 1;

Q3. Total sales & order count per category
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM epik_sales
GROUP BY category;

Q4. Average age of customers who purchased Beauty products
SELECT 
    category,
    ROUND(AVG(age), 2) 
FROM epik_sales 
WHERE category = 'beauty';

Q5. Transactions where sale amount > 1000
SELECT * FROM epik_sales 
WHERE total_sale > 1000;

Q6. Number of transactions made by each gender for each category
SELECT 
    category,
    gender,
    COUNT(*) AS total_tran
FROM epik_sales
GROUP BY category, gender
ORDER BY category, gender;

Q7. Best-selling month for each year (based on average sale)
SELECT 
    year, month, avg_sale
FROM (
    SELECT
        YEAR(sale_date) AS year,
        MONTH(sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) AS rn
    FROM epik_sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
) AS t1
WHERE rn = 1
ORDER BY year, month;

Q8. Top 5 highest-spending customers
SELECT 
    DISTINCT customer_id AS customers,
    SUM(total_sale) AS highesh_sale
FROM epik_sales
GROUP BY customers
ORDER BY highesh_sale DESC
LIMIT 5;

Q9. Unique customers per category
SELECT 
    category,
    COUNT(DISTINCT customer_id)
FROM epik_sales
GROUP BY category;

Q10. Number of orders by time-of-day (Morning, Afternoon, Evening)
SELECT 
    shift,
    COUNT(*) AS number_of_order
FROM (
    SELECT 
        CASE 
            WHEN HOUR(sale_time) < 12 THEN 'Morning'
            WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM epik_sales
) AS t1
GROUP BY shift;

📝 Findings

Customer Demographics: The sales data contains a mix of genders and age groups purchasing across Beauty, Electronics, Household, and other categories.

High-Value Transactions: Multiple sales exceeded ₹1000, showing premium product demand.

Category Performance: Beauty and other categories showed strong order volume.

Sales Trends: Monthly average sales highlight the best-performing month in each year.

Customer Insights: The top 5 customers accounted for a significant share of revenue.

Time-of-Day Trends: More orders occurred during specific time shifts (Morning/Afternoon/Evening).

📑 Reports Generated

Sales Summary Report

Customer Demographics & Insights

Category Performance Analysis

Monthly and Shift-Based Sales Trends

Top Customer Analysis

✅ Conclusion

This project provides a complete introduction to SQL for e-commerce analysis, including database creation, cleaning, EDA, and solving real business problems.
The insights help understand customer behaviour, top categories, seasonal sales trends, and order patterns.

This SQL project demonstrates strong analytical thinking and is suitable for showcasing on GitHub and your resume.
