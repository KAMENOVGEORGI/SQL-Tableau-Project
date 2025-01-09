
                                                 "Day of a Data Analyst"

### SQL|Tableau Project 

## Overview
This project showcases a step-by-step workflow of extracting insights from a database using SQL queries, visualizing the data using Tableau, and creating an interactive dashboard for business decision-making. The primary focus is on analyzing sales performance, customer behavior, and product trends.

---

## **Database Structure**
The database contains the following tables:

1. **Orders**: Stores information about customer orders.
   - `order_id`
   - `customer_id`
   - `order_date`

2. **Order_Details**: Contains details of items in each order.
   - `order_id`
   - `product_id`
   - `quantity`

3. **Products**: Stores product information.
   - `product_id`
   - `product_name`
   - `price`

4. **Customers**: Contains customer data.
   - `customer_id`
   - `customer_name`
   - `country`

---

## **SQL Queries for Data Extraction**
The following SQL queries were executed to extract meaningful insights from the database:

### 1. **Monthly Sales and Orders**
```sql
SELECT DATE_TRUNC('month', o.order_date) AS month, 
       COUNT(o.order_id) AS total_orders,
       SUM(od.quantity * p.price) AS total_sales
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
GROUP BY month
ORDER BY month;
```
- **Purpose**: Provides the total orders and sales aggregated by month.

### 2. **Top 5 Most Purchased Products**
```sql
SELECT p.product_name, SUM(od.quantity) AS total_quantity_sold
FROM order_details od
JOIN products p ON od.product_id = p.product_id
GROUP BY p.product_name
ORDER BY total_quantity_sold DESC
LIMIT 5;
```
- **Purpose**: Identifies the top 5 products with the highest quantity sold.

### 3. **High-Value Orders**
```sql
SELECT o.order_id, c.customer_name, SUM(od.quantity * p.price) AS order_total
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
GROUP BY o.order_id, c.customer_name
HAVING SUM(od.quantity * p.price) > 1000
ORDER BY order_total DESC;
```
- **Purpose**: Extracts orders with a total value exceeding $1000.

### 4. **Top Customers by Total Spend**
```sql
SELECT c.customer_name, COUNT(o.order_id) AS order_count,
       AVG(SUM(od.quantity * p.price)) OVER (PARTITION BY c.customer_name) AS avg_order_value,
       SUM(od.quantity * p.price) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
GROUP BY c.customer_name
ORDER BY total_spent DESC;
```
- **Purpose**: Identifies the top customers by total spend and provides average order value.

### 5. **Most Profitable Days of the Week**
```sql
SELECT TO_CHAR(o.order_date, 'Day') AS day_of_week,
       SUM(od.quantity * p.price) AS total_revenue
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
GROUP BY day_of_week
ORDER BY total_revenue DESC;
```
- **Purpose**: Shows revenue generated on each day of the week.

### 6. **Revenue by Country**
```sql
SELECT c.country, SUM(od.quantity * p.price) AS total_revenue
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
GROUP BY c.country
ORDER BY total_revenue DESC;
```
- **Purpose**: Analyzes total revenue generated by each country.

---

## **Data Visualization in Tableau**

### **Sheets Created**:
1. **Monthly Sales Performance**
   - Line chart showing monthly sales and orders trends.

2. **Top 5 Most Purchased Products**
   - Horizontal bar chart for product popularity.

3. **High-Value Orders**
   - Table showing orders exceeding $1000 with customer names and total value.

4. **Top 10 Customers by Lifetime Value**
   - Bar chart displaying the top customers based on total spend.

5. **Most Profitable Days of the Week**
   - Bar chart analyzing revenue by weekday.

6. **Revenue by Country**
   - Map view or bar chart displaying revenue by country.

7. **TOP 50 Customers |RFM|**
   - Scatter Plot for the Top 50 Customers with ranks based on Recency,Frequency and Monetery Value

---


