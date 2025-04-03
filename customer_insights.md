## 🎯 Top 5 Highest-Paying Customers with Most-Spent Product

### 📌 Overview
This query identifies the top 5 customers based on total amount spent, and shows the product each customer spent the most money on.

### 💡 Why
Understanding who your top-paying customers are — and what products they value most — helps marketing, sales, and strategy teams better target high-value segments and tailor offers to their preferences.

### 🛠️ How
The query:
- Aggregates total spend per customer (price × quantity - coupon)
- Finds the product each customer spent the most money on
- Combines both into a single, readable result

### 📊 Results

**Total Spend Per Customer - Top 5**

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

<img width="691" alt="Screenshot 2025-04-02 at 18 58 05" src="https://github.com/user-attachments/assets/0795433d-00e4-4c9c-afea-d3471081a5c7" />

## ✅ What This Query Does

This query calculates overall spend for each customer.

### 📊 Steps:

- Groups all orders by customer
- Calculates:
  - 💰 `total_purchase`: The total value of items purchased (`price × quantity`)
  - 🎟️ `total_discount`: The total discount applied using coupons
  - 🧾 `total_spend`: Final amount paid, after subtracting coupon discounts

- Sorts the results by **total spend (after discounts)** in descending order

- Returns the **top 5 highest-spending customers**

### 🧠 Think of it like:
> "Who are my 5 biggest customers overall, and how much did they really spend after discounts?"

**Top Product by Customer Spend - Top 5**

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


## ✅ What This Query Does

This query analyzes customer purchase behavior and returns a ranked list based on total spend.

### 📊 Steps:

- Looks at each customer’s purchases, **grouped by product**
- Calculates:
  - 💰 Total amount spent on each product (`price × quantity`)
  - 🎟️ Total discount applied (`coupon`)
  - 🧾 Final spend after discount

- Finds the **one product** each customer spent the **most money on** (based on total price × quantity, before discount)

- Then it returns the **top 5 customers** who spent the most **after discounts**, and shows:
  - Customer name
  - Total purchase amount
  - Total discount
  - Final amount paid
  - The product they spent the most on

### 🧠 Think of it like:
> "Show me the 5 customers who spent the most, and tell me which single product they spent the most on."

Step 3: Join and return top 5 paying customers with their top product
```sql
SELECT
    ct.customer_id,
    ct.full_customer_name,
    ct.total_customer_purchase,
    ct.total_coupon_discount,
    ct.total_customer_paid,
    ctp.product_name AS most_spent_product,
    ctp.total_spent_on_product
FROM customer_totals ct
LEFT JOIN customer_top_product ctp
  ON ct.customer_id = ctp.customer_id AND ctp.rn = 1
ORDER BY ct.total_customer_paid DESC
LIMIT 5;
```


