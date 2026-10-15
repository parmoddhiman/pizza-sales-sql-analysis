# 🍕 Pizza Sales SQL Analysis Project

## 📌 Project Overview

This project analyzes pizza sales data using SQL to uncover key business insights, including revenue trends, customer ordering patterns, best-selling pizzas, and category performance.

---

## 🛠️ Tools Used

- **SQL Concepts:** Joins, CTE, Window Functions, Aggregations, Group By and Order By.

---

## 📂 Database Tables

| Table Name | Description |
|------------|-------------|
| Orders | Stores customer order information |
| Order_Details | Stores pizza quantities for each order |
| Pizzas | Stores pizza sizes and prices |
| Pizza_Types | Stores pizza names and categories |

---

# 📊 Business Questions & SQL Solutions

## 1️⃣ Retrieve the Total Number of Orders Placed

```sql
SELECT COUNT(*) AS Total_Orders
FROM orders;
```

✅ Retrieves the total number of orders placed.

---

## 2️⃣ Calculate the Total Revenue Generated from Pizza Sales

```sql
SELECT ROUND(SUM(od.quantity * p.price), 2) AS Total_Revenue
FROM order_details od
JOIN pizzas p
ON p.pizza_id = od.pizza_id;
```

✅ Calculates total revenue generated from pizza sales.

---

## 3️⃣ Identify the Highest-Priced Pizza

```sql
SELECT pt.name, p.price
FROM pizza_types pt
JOIN pizzas p
ON pt.pizza_type_id = p.pizza_type_id
ORDER BY p.price DESC
LIMIT 1;
```

✅ Finds the most expensive pizza.

---

## 4️⃣ Identify the Most Common Pizza Size Ordered

```sql
SELECT p.size,
       COUNT(*) AS Total_Count
FROM order_details od
JOIN pizzas p
ON od.pizza_id = p.pizza_id
GROUP BY p.size
ORDER BY Total_Count DESC
LIMIT 1;
```

✅ Determines the most frequently ordered pizza size.

---

## 5️⃣ List the Top 5 Most Ordered Pizza Types

```sql
SELECT pt.name,
       SUM(od.quantity) AS Total_Qty
FROM order_details od
JOIN pizzas p
ON p.pizza_id = od.pizza_id
JOIN pizza_types pt
ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY Total_Qty DESC
LIMIT 5;
```

✅ Displays the top 5 pizzas by quantity sold.

---

## 6️⃣ Find the Total Quantity Ordered for Each Pizza Category

```sql
SELECT pt.category,
       SUM(od.quantity) AS Total_Ordered
FROM order_details od
JOIN pizzas p
ON p.pizza_id = od.pizza_id
JOIN pizza_types pt
ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
ORDER BY Total_Ordered DESC;
```

✅ Shows total pizzas sold by category.

---

## 7️⃣ Determine the Distribution of Orders by Hour

```sql
SELECT HOUR(order_time) AS Hour,
       COUNT(order_id) AS Total_Orders_Per_Hour
FROM orders
GROUP BY Hour
ORDER BY Hour;
```

✅ Identifies peak ordering hours.

---

## 8️⃣ Category-Wise Distribution of Pizzas

```sql
SELECT category,
       COUNT(name) AS Total_Count
FROM pizza_types
GROUP BY category;
```

✅ Shows the number of pizzas available in each category.

---

## 9️⃣ Calculate the Average Number of Pizzas Ordered Per Day

```sql
WITH daily_orders AS
(
    SELECT o.order_date,
           SUM(od.quantity) AS Total_Pizzas
    FROM orders o
    JOIN order_details od
    ON o.order_id = od.order_id
    GROUP BY o.order_date
)

SELECT ROUND(AVG(Total_Pizzas), 0) AS Avg_Per_Day
FROM daily_orders;
```

✅ Calculates the average pizzas sold per day.

---

## 🔟 Top 3 Pizza Types Based on Revenue

```sql
SELECT pt.name,
       SUM(od.quantity * p.price) AS Total_Revenue
FROM order_details od
JOIN pizzas p
ON p.pizza_id = od.pizza_id
JOIN pizza_types pt
ON pt.pizza_type_id = p.pizza_type_id
GROUP BY pt.name
ORDER BY Total_Revenue DESC
LIMIT 3;
```

✅ Identifies the top revenue-generating pizzas.

---

## 1️⃣1️⃣ Percentage Contribution of Each Category to Revenue

```sql
SELECT pt.category,
       CONCAT(
           ROUND(
               SUM(od.quantity * p.price) * 100 /
               SUM(SUM(od.quantity * p.price)) OVER (),
           2),
           '%'
       ) AS Percentage_Contribution
FROM order_details od
JOIN pizzas p
ON p.pizza_id = od.pizza_id
JOIN pizza_types pt
ON pt.pizza_type_id = p.pizza_type_id
GROUP BY pt.category
ORDER BY Percentage_Contribution DESC;
```

✅ Calculates the percentage contribution of each category to total revenue.

---

## 1️⃣2️⃣ Top 3 Revenue-Generating Pizzas Within Each Category

```sql
WITH pizza_revenue AS
(
    SELECT pt.category,
           pt.name,
           SUM(od.quantity * p.price) AS Total_Revenue
    FROM order_details od
    JOIN pizzas p
    ON p.pizza_id = od.pizza_id
    JOIN pizza_types pt
    ON pt.pizza_type_id = p.pizza_type_id
    GROUP BY pt.category, pt.name
),

ranked_pizzas AS
(
    SELECT *,
           DENSE_RANK() OVER
           (
               PARTITION BY category
               ORDER BY Total_Revenue DESC
           ) AS rnk
    FROM pizza_revenue
)

SELECT category,
       name,
       ROUND(Total_Revenue, 2) AS Revenue
FROM ranked_pizzas
WHERE rnk <= 3;
```

✅ Displays the top 3 pizzas by revenue within each category.

---

# 🔍 Key Insights

- 💰 Revenue analysis and business performance tracking
- 🍕 Best-selling pizza identification
- 📦 Category performance comparison
- ⏰ Peak sales hour analysis
- 📊 Revenue contribution calculation
- 🏆 Top-performing pizzas by revenue

--
