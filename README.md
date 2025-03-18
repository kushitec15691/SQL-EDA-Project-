# SQL-EDA-Project

## Overview
This repository contains SQL scripts that perform various analytical techniques for data exploration and business insights. The scripts include database creation, data loading, and analysis using structured queries. The project follows a **Bronze-Silver-Gold** data architecture model, ensuring efficient data transformation and structuring.

## Features
- **Database Creation & Schema Setup**: Creates a `DataWarehouseAnalytics` database with schemas (`gold`) and essential tables.
- **Data Import**: Loads data into `gold.dim_customers`, `gold.dim_products`, and `gold.fact_sales` tables using `BULK INSERT`.
- **Exploratory Analysis**: Queries to explore database structure, customer demographics, and product categories.
- **Business Insights**:
  - Sales trends over time
  - Customer segmentation
  - Revenue and product performance
  - Order and sales distribution by region and gender
  - Top and worst-performing products/customers
  
## Database & Table Structure
### Database: `DataWarehouseAnalytics`
#### Schema: `gold`
- **`gold.dim_customers`**: Stores customer demographic details.
- **`gold.dim_products`**: Contains product details and categories.
- **`gold.fact_sales`**: Stores transactional sales data.

## Key SQL Queries
- **General Data Exploration**
  - View all tables: `SELECT * FROM INFORMATION_SCHEMA.TABLES;`
  - View table columns: `SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'dim_customers';`
  
- **Business Metrics & Reports**
  - First & last order dates, order range: `SELECT MIN(order_date), MAX(order_date), DATEDIFF(month, MIN(order_date), MAX(order_date)) FROM gold.fact_sales;`
  - Total sales, quantity sold, and average price:
    ```sql
    SELECT SUM(sales_amount) AS total_sales, SUM(quantity) AS total_quantity, AVG(price) AS avg_price FROM gold.fact_sales;
    ```
  - Top 5 products by revenue:
    ```sql
    SELECT TOP 5 p.product_name, SUM(f.sales_amount) AS total_revenue FROM gold.fact_sales f
    LEFT JOIN gold.dim_products p ON p.product_key = f.product_key
    GROUP BY p.product_name ORDER BY total_revenue DESC;
    ```
  - Customer revenue contribution:
    ```sql
    SELECT c.customer_key, c.first_name, c.last_name, SUM(f.sales_amount) AS total_revenue
    FROM gold.fact_sales f
    LEFT JOIN gold.dim_customers c ON c.customer_key = f.customer_key
    GROUP BY c.customer_key, c.first_name, c.last_name ORDER BY total_revenue DESC;
    ```

## How to Use
1. **Setup Database**: Run the script to create `DataWarehouseAnalytics` and `gold` schema.
2. **Load Data**: Use `BULK INSERT` to import CSV files into the respective tables.
3. **Explore & Analyze**: Execute SQL queries for business insights.
4. **Modify & Extend**: Customize the script for additional analytics.

## Warning
**Executing the setup script will drop and recreate the database, permanently deleting all existing data. Ensure you have backups before running the script.**

## Contributions
Feel free to contribute by optimizing queries, adding new analytics, or improving data transformation steps.

