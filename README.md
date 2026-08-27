# Olist E-Commerce Delivery Performance Analysis

## Project Overview
This project analyzes delivery performance for **Olist**, a Brazilian e-commerce company, focusing on identifying bottlenecks and late delivery patterns across different states. The analysis connects customer demographic data with order fulfillment records to provide actionable insights for logistics optimization.

<img width="602" height="338" alt="image" src="https://github.com/user-attachments/assets/4ce9c7ad-ca58-4bb4-8fdf-e0ecfffb9c1a" />


## Data Preparation & Processing
The analysis was conducted using an in-memory SQLite database built from the Olist E-commerce dataset. Key steps included:
* Loading raw CSV data (`olist_customers_dataset.csv`, `olist_orders_dataset.csv`) into Pandas DataFrames.
* Establishing a local SQLite connection to emulate a relational database environment.

## Key Business Queries & SQL Implementation

### 1. Order Volume by State
To understand geographical sales distribution, customer and order tables were joined.
```sql
SELECT 
    c.customer_state, 
    COUNT(o.order_id) AS total_orders
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_state
ORDER BY total_orders DESC
LIMIT 10;
