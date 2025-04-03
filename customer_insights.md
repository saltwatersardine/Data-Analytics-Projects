## 🎯 Top 5 Highest-Paying Customers with Most-Spent Product

### 📌 Overview
This analysis identifies the **top 5 customers** based on their **total spend after discounts**, and shows the **product each customer spent the most money on**, including how many units of that product they purchased.

### 💡 Why
Understanding who your top-paying customers are — and what products they value most — helps marketing, sales, and strategy teams:
- Better understand customer behavior
- Improve targeting and segmentation
- Tailor offers to drive revenue from high-value audiences

### 🛠️ How
The full logic is broken into 3 steps:
- Calculate total spend per customer (including discount impact)
- Identify each customer's most-spent product
- Join the two to return a complete view of spend + product insights

### 📊 Results
___

### Total Spend Per Customer - Top 5

```sql
SELECT
    customer_id,
    CONCAT(customer_first_name, ' ', customer_last_name) AS full_name,
    CAST(SUM(price * quantity) AS INT) AS total_purchase,    
    ROUND(SUM(COALESCE(coupon, 0)),2) AS total_discount,
    ROUND(SUM(price * quantity) - SUM(COALESCE(coupon, 0)), 2) AS total_spend, 
FROM
    orders
GROUP BY customer_id, customer_first_name, customer_last_name, 
ORDER BY total_spend DESC
LIMIT 5;
),
```
<img width="984" alt="Screenshot 2025-04-02 at 18 33 03" src="https://github.com/user-attachments/assets/2be17650-cb2c-4a7c-b9e8-0a6f4f83b8c2" />

<img width="491" alt="Screenshot 2025-04-02 at 18 58 05" src="https://github.com/user-attachments/assets/0795433d-00e4-4c9c-afea-d3471081a5c7" />

### ✅ What This Query Does

This query calculates overall spend for each customer, and returns the **top 5 highest-spending customers**

### 📊 Steps:

- Groups all orders by customer
- Calculates:
  - 💰 The total value of items purchased
  - 🎟️ The total discount applied using coupons
  - 🧾 Final amount paid, after subtracting coupon discounts

### 🧠 Think of it like:
> "Who are my 5 biggest customers overall, and how much did they really spend after discounts?"
___

### Top Product by Customer Spend - Top 5

```sql
SELECT *
FROM (
    SELECT
        customer_id,
        CONCAT(customer_first_name, ' ', customer_last_name) AS full_customer_name,
        CAST(SUM(price * quantity) AS INT) AS total_customer_purchase,    
        ROUND(SUM(COALESCE(coupon, 0)), 2) AS total_coupon_discount,
        ROUND(SUM(price * quantity) - SUM(COALESCE(coupon, 0)), 2) AS total_customer_spend,
        product_name
    FROM orders
    GROUP BY customer_id, customer_first_name, customer_last_name, product_name
    QUALIFY ROW_NUMBER() OVER (
        PARTITION BY customer_id
        ORDER BY SUM(price * quantity) DESC
    ) = 1
) AS top_products
ORDER BY total_customer_spend DESC
LIMIT 5;
)
```

<img width="994" alt="Screenshot 2025-04-02 at 18 59 47" src="https://github.com/user-attachments/assets/155f71a0-1ea7-462d-83e9-205dfac6eae7" />

<img width="479" alt="Screenshot 2025-04-03 at 13 16 35" src="https://github.com/user-attachments/assets/bcb7bba4-1af5-4eb4-abfb-0bc12a75a266" />

### ✅ What This Query Does

This query returns the **top 5 customers** based on their total spend and includes the **item they spent the most on**.

### 📊 Steps:

- Looks at each customer’s purchases, **grouped by product**
- Calculates:
  - 💰 Total amount spent on each product
  - 🎟️ Total discount applied
  - 🧾 Final spend after discount

- Finds the **one product** each customer spent the **most money on** 

### 🧠 Think of it like:
> "Show me the 5 customers who spent the most, and tell me which single product they spent the most on."
___

### Step 3: Join and return top 5 paying customers with their top product, and how many they ordered. 

```sql
-- Step 1: Total spend per customer (after discount)
WITH customer_totals AS (
  SELECT
      customer_id,
      CONCAT(customer_first_name, ' ', customer_last_name) AS full_name,
      CAST(SUM(price * quantity) AS INT) AS total_purchase,
      ROUND(SUM(COALESCE(coupon, 0)), 2) AS total_discount,
      ROUND(SUM(price * quantity - COALESCE(coupon, 0)), 2) AS total_spend
  FROM orders
  GROUP BY customer_id, customer_first_name, customer_last_name
),

-- Step 2: Top product per customer, including spend and quantity (after discount)
top_product_per_customer AS (
  SELECT *
  FROM (
    SELECT
        customer_id,
        product_name,
        ROUND(SUM(price * quantity - COALESCE(coupon, 0)), 2) AS top_product_spend,
        SUM(quantity) AS top_product_quantity,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY SUM(price * quantity - COALESCE(coupon, 0)) DESC
        ) AS rn
    FROM orders
    GROUP BY customer_id, product_name
  ) ranked
  WHERE rn = 1
)

-- Step 3: Join and return top 5 customers by total spend
SELECT
    ct.customer_id,
    ct.full_name,
    ct.total_purchase,
    ct.total_discount,
    ct.total_spend,
    tp.product_name AS top_product,
    tp.top_product_spend,
    tp.top_product_quantity
FROM customer_totals ct
LEFT JOIN top_product_per_customer tp
  ON ct.customer_id = tp.customer_id
ORDER BY ct.total_spend DESC
LIMIT 5;
```
### ✅ What This Query Does

This query identifies the **top 5 customers** based on their **total spend (after discounts)** and shows details about the **product they spent the most on**.

### 📊 Steps:

#### 🔹 Step 1: Total Spend per Customer
- Aggregates total purchase data for each customer:
  - 💰 Total value before discounts 
  - 🎟️ Total coupon discounts applied
  - 🧾 Final spend after applying discounts

#### 🔹 Step 2: Top Product per Customer
- For each customer, finds the **one product** they spent the **most money on (after discounts)**
  - 🛒 Total spend on that product (after coupon)
  - 🔢 Total quantity of that product purchased

#### 🔹 Step 3: Final Result
- Joins customer spend data with top product data
- Sorts by **total spend descending**
- Returns the **top 5 customers**, showing:
  - Customer name
  - Total purchase
  - Total discount
  - Final spend
  - Most purchased product (by spend)
  - Spend on that product
  - Quantity of that product purchased

### 🧠 Think of it like:
> “Show me my 5 biggest customers, how much they spent overall (after discounts), and which product they spent the most on — including how much and how many units.”
