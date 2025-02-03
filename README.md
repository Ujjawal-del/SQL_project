# Pizza Sales Analysis Project

## Overview

This project involves analyzing pizza sales data using SQL to answer key business questions. The analysis is divided into three sections: Basic, Intermediate, and Advanced, each with specific objectives to uncover insights related to total orders, revenue, popular pizza types, and order distribution.

## Basic Analysis

### Objectives

1. **Retrieve the total number of orders placed.**
2. **Calculate the total revenue generated from pizza sales.**
3. **Identify the highest-priced pizza.**
4. **Identify the most common pizza size ordered.**
5. **List the top 5 most ordered pizza types along with their quantities.**

### SQL Queries

1. **Total Number of Orders:**
   ```sql
   SELECT COUNT(order_id) 
   FROM orders;
   ```

2. **Total Revenue from Pizza Sales:**
   ```sql
   SELECT ROUND(SUM(order_details.quantity*pizzas.price),2) AS total_sales
   FROM order_details
   JOIN pizzas
   ON pizzas.pizza_id = order_details.pizza_id;
   ```

3. **Highest-Priced Pizza:**
   ```sql
   SELECT name, price
   FROM pizza_types
   JOIN pizzas
   ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   ORDER BY pizzas.price DESC
   LIMIT 1;
   ```

4. **Most Common Pizza Size Ordered:**
   ```sql
   SELECT size, COUNT(order_details_id) AS order_count
   FROM pizzas
   JOIN order_details
   ON pizzas.pizza_id = order_details.pizza_id
   GROUP BY pizzas.size
   ORDER BY order_count DESC;
   ```

5. **Top 5 Most Ordered Pizza Types:**
   ```sql
   SELECT pizza_types.name, SUM(order_details.quantity) AS quantity
   FROM pizza_types
   JOIN pizzas
   ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   JOIN order_details
   ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.name
   ORDER BY  quantity DESC
   LIMIT 5;
   ```

## Intermediate Analysis

### Objectives

1. **Total quantity of each pizza category ordered.**
2. **Distribution of orders by hour of the day.**
3. **Category-wise distribution of pizzas.**
4. **Average number of pizzas ordered per day.**
5. **Top 3 most ordered pizza types based on revenue.**

### SQL Queries

1. **Total Quantity of Each Pizza Category:**
   ```sql
   SELECT category, SUM(quantity) AS quantity
   FROM pizza_types
   JOIN pizzas 
   ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   JOIN order_details
   ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY category;
   ```

2. **Distribution of Orders by Hour:**
   ```sql
   SELECT HOUR(time) AS hour, COUNT(order_id) AS order_id
   FROM orders
   GROUP BY HOUR(time);
   ```

3. **Category-wise Distribution of Pizzas:**
   ```sql
   SELECT COUNT(name), category
   from pizza_types
   GROUP BY category;
   ```

4. **Average Number of Pizzas Ordered Per Day:**
   ```sql
   SELECT ROUND(AVG(quantity),0) AS avd_pizza_order_perDay
   FROM (SELECT orders.date, SUM(order_details.quantity) AS quantity
	FROM orders
   JOIN order_details 
   ON orders.order_id = order_details.order_id
   GROUP BY orders.date) AS order_quantity;
   ```

5. **Top 3 Most Ordered Pizza Types by Revenue:**
   ```sql
   SELECT pizza_types.name, SUM(pizzas.price * order_details.quantity) AS revenue
   FROM pizzas
   JOIN pizza_types
   ON pizzas.pizza_type_id = pizza_types.pizza_type_id
   JOIN order_details
   ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.name
   ORDER BY revenue DESC
   LIMIT 3;
   ```

## Advanced Analysis

### Objectives

1. **Percentage contribution of each pizza type to total revenue.**
2. **Cumulative revenue generated over time.**
3. **Top 3 most ordered pizza types based on revenue for each pizza category.**

### SQL Queries

1. **Percentage Contribution of Each Pizza Type to Total Revenue:**
   ```sql
   SELECT pizza_types.category, ROUND(SUM(pizzas.price * order_details.quantity) / (SELECT
											SUM(order_details.quantity * pizzas.price)
                                            FROM order_details
                                            JOIN pizzas
                                            ON order_details.pizza_id = pizzas.pizza_id) * 100,2)
                                            AS revenue;
   ```

2. **Cumulative Revenue Over Time:**
   ```sql
   SELECT date,
   SUM(revenue) OVER (ORDER BY date) AS cm_revenue
   FROM 
   (SELECT orders.date,
   ROUND(SUM(order_details.quantity * pizzas.price),2) AS revenue
   FROM order_details
   JOIN pizzas
   ON order_details.pizza_id = pizzas.pizza_id
   JOIN orders
   ON orders.order_id = order_details.order_id
   GROUP BY orders.date) AS sales;
   ```

3. **Top 3 Most Ordered Pizza Types by Revenue for Each Category:**
   ```sql
   SELECT pizza_types.name, SUM(pizzas.price * order_details.quantity) AS revenue
   FROM pizzas
   JOIN pizza_types
   ON pizzas.pizza_type_id = pizza_types.pizza_type_id
   JOIN order_details
   ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.name
   ORDER BY revenue DESC
   LIMIT 3;
   ```

## Conclusion

This project demonstrates the use of SQL for data analysis in a business context, providing insights into pizza sales performance and customer ordering patterns. The structured approach from basic to advanced analysis allows us to extract meaningful information that can drive business decisions and strategy.

## Repository Structure

- **/queries**: Contains all the SQL query files.
- **/data**: Placeholder for raw data files.
- **/results**: Contains results from SQL queries.
- **README.md**: Project documentation.

---

If you found this project helpful, please give it a star and follow the repository for more data analysis projects!

Happy querying!
