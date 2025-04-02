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


Step 1: Total spend per customer ranked top 5
```sql
WITH customer_totals AS (
  SELECT
      customer_id,
      CONCAT(customer_first_name, ' ', customer_last_name) AS full_customer_name,
      CAST(SUM(price * quantity) AS INT) AS total_customer_purchase,
      ROUND(SUM(COALESCE(coupon, 0)), 2) AS total_coupon_discount,   
      ROUND(SUM(price * quantity) - SUM(COALESCE(coupon, 0)), 2) AS total_customer_paid
  FROM orders
  GROUP BY customer_id, customer_first_name, customer_last_name
),
```
<img width="996" alt="Screenshot 2025-04-02 at 15 21 42" src="https://github.com/user-attachments/assets/8bb1c126-ea25-4eb1-8ae7-695d1ba35719" />

<img width="969" alt="Screenshot 2025-04-02 at 15 46 58" src="https://github.com/user-attachments/assets/d36c1280-ac18-406b-960e-2263895b8dfa" />

Step 2: Product each customer spent the most on Ranked Top 5
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
<img width="993" alt="Screenshot 2025-04-02 at 15 42 09" src="https://github.com/user-attachments/assets/d838bb06-ba25-4144-a4a7-a8ad90c30994" />

<img width="972" alt="Screenshot 2025-04-02 at 15 50 55" src="https://github.com/user-attachments/assets/b478d792-15b1-4df4-80a1-a256d60e99dc" />

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


