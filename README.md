# Olist E-Commerce Delivery Performance Analysis

## Project Overview
This project analyzes delivery performance for **Olist**, a Brazilian e-commerce company, focusing on identifying bottlenecks and late delivery patterns across different states. The analysis connects customer demographic data with order fulfillment records to provide actionable insights for logistics optimization.

<img width="601" height="338" alt="image" src="https://github.com/user-attachments/assets/591b38af-9385-43af-a206-69b3cba58539" />


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

#Insight: São Paulo (SP) accounts for the vast majority of volume (41,746 orders), followed by Rio de Janeiro (RJ) and Minas Gerais (MG).

#2. Overall Delivery Performance (On Time vs. Late)
#A calculated field using conditional logic was built to categorize deliveries.

SELECT 
    CASE 
        WHEN order_delivered_customer_date > order_estimated_delivery_date THEN 'Late'
        ELSE 'On Time'
    END AS delivery_status,
    COUNT(order_id) AS total_orders
FROM orders
WHERE order_status = 'delivered' 
  AND order_delivered_customer_date IS NOT NULL
GROUP BY delivery_status;

#Insight: Approximately 8.11% of all delivered orders arrived past their estimated delivery date.

#3. Analyzing Late Deliveries by State
Combining volume tracking with delivery failure categorization:

SQL
