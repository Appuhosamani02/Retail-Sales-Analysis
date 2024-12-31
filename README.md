# Retail Sales Analysis Using SQL

## Project Description
This repository contains SQL queries and scripts used to perform retail sales analysis. The project aims to extract insights from sales data to address key business questions and support decision-making processes.

## Features
- **Database Creation**: Set up a structured database to store retail sales data.
- **Data Analysis**: Perform comprehensive SQL queries to answer business-related questions.
- **Insights Extraction**: Analyze customer behavior, sales trends, and profitability metrics.

## Tools Used
- **Database**: MySQL
- **Language**: SQL

## Table Schema
The `retail_sales` table contains the following columns:

| Column           | Data Type     | Description                                   |
|-------------------|---------------|-----------------------------------------------|
| `transaction_id` | INT           | Unique identifier for each transaction.      |
| `sale_date`      | DATE          | Date of the sale.                            |
| `sale_time`      | TIME          | Time of the sale.                            |
| `customer_id`    | INT           | Unique identifier for each customer.         |
| `gender`         | VARCHAR(15)   | Gender of the customer.                      |
| `age`            | INT           | Age of the customer.                         |
| `category`       | VARCHAR(15)   | Product category.                            |
| `quantity`       | INT           | Quantity of products sold.                   |
| `price_per_unit` | FLOAT         | Price per unit of the product.               |
| `cogs`           | FLOAT         | Cost of goods sold.                          |
| `total_sale`     | FLOAT         | Total sales value of the transaction.        |

## SQL Queries and Insights

### 1. Check for Null Values
```sql
SELECT *
FROM retail_sales
WHERE
	transaction_id IS NULL OR
    sale_date IS NULL OR
    sale_time IS NULL OR
    customer_id IS NULL OR
    gender IS NULL OR
    age IS NULL OR
    category IS NULL OR
    quantity IS NULL OR
    price_per_unit IS NULL OR
    cogs IS NULL OR
    total_sale IS NULL;
```

### 2. Total Sales Count
```sql
SELECT COUNT(*) as Total_Sales
FROM retail_sales;
```

### 3. Unique Customers Count
```sql
SELECT COUNT(DISTINCT customer_id) as Total_Customers
FROM retail_sales;
```

## Key Business Queries
- **Q1**: Retrieve all columns for sales made on `2022-11-05`.
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

- **Q2**: Retrieve transactions where category is `Clothing` and quantity sold is more than 4 in November 2022.
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
	AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
	AND quantity >= 4;
```

- **Q3**: Calculate total sales (`total_sale`) for each category.
```sql
SELECT category, SUM(total_sale) AS Total_Sales
FROM retail_sales
GROUP BY category
ORDER BY category;
```

- **Q4**: Find the average age of customers who purchased items from the `Beauty` category.
```sql
SELECT AVG(age) as Average_Age_Customer
FROM retail_sales
WHERE category = 'Beauty';
```

- **Q5**: Retrieve all transactions where the total sales value is greater than 1000.
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

- **Q6**: Find customer IDs of customers who made more than 4 purchases in November 2022.
```sql
SELECT customer_id, COUNT(transaction_id) AS Count_Purchase
FROM retail_sales
WHERE DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
GROUP BY customer_id
HAVING COUNT(transaction_id) >= 4;
```

- **Q7**: Calculate average sales for each month and identify the best-selling month in each year.
```sql
SELECT year, Best_Month, Avg_Sale
FROM (
	SELECT
		YEAR(sale_date) as Year,
		MONTH(sale_date) as Best_Month,
		ROUND(AVG(total_sale), 2) as Avg_Sale,
		ROW_NUMBER() OVER(PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) as RankSales
	FROM retail_sales
	GROUP BY YEAR(sale_date), MONTH(sale_date)
) t
WHERE RankSales = 1;
```

- **Q8**: Identify the top 5 customers based on total sales.
```sql
SELECT customer_id, SUM(total_sale) as Total_Sale
FROM retail_sales
GROUP BY customer_id
ORDER BY SUM(total_sale) DESC
LIMIT 5;
```

- **Q9**: Calculate profit margin (`total_sale - cogs`) for transactions where `cogs` is less than 300.
```sql
SELECT transaction_id, cogs, (total_sale - cogs) AS profit_margin
FROM retail_sales
WHERE cogs < 300;
```

## How to Use
1. Clone the repository:
   ```bash
   git clone https://github.com/Appuhosamani02/retail-sales-analysis.git
   ```
2. Import the SQL scripts into your database system.
3. Run the queries to analyze the data and generate insights.

---
**Author**: Appu 


