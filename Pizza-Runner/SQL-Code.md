# Here is my code for the Pizza Runner Case Study!
### 1. How many pizzas were ordered?
```sql
SELECT COUNT(*) FROM pizza_runner.customer_orders;
```
- Here, we simply count all the records in the customer_orders table
  
| count |
| ----- |
| 14    |

- Fourteen pizzas were ordered
---
### 2. How many unique customer orders were made?
```sql
SELECT COUNT(DISTINCT(order_id)) FROM pizza_runner.customer_orders;
```
- the order can be identified by the order_id so we count the distinct order_ids that exist
  
| count |
| ----- |
| 10    |

- There were ten unique orders
---
### 3. How many successful orders were delivered by each runner?
```sql
SELECT runner_id, COUNT(order_id) AS successful_orders FROM pizza_runner.runner_orders
WHERE runner_orders.cancellation = ''
GROUP BY runner_id;
```
- we group by the runner then count the order_ids, making sure that we are only counting the orders that we not cancelled

| runner_id | successful_orders |
| --------- | ----------------- |
| 1         | 4                 |
| 2         | 3                 |
| 3         | 1                 |

- Runner 1 successfully delivered four orders
- Runner 2 successfully delivered three orders
- Runner 3 successfully delivered one order
---
### 4. How many of each type of pizza was delivered?
```sql
SELECT p.pizza_name, COUNT(c.pizza_id) AS pizza FROM pizza_runner.customer_orders c
JOIN pizza_runner.pizza_names p ON p.pizza_id = c.pizza_id
JOIN pizza_runner.runner_orders r ON c.order_id = r.order_id
WHERE cancellation = ''
GROUP BY c.pizza_id, p.pizza_name
ORDER BY c.pizza_id;
```
- First, we join customer_orders with pizza_names and runner_orders to get a table that shows the pizza that was ordered, the name, and whether it what cancelled
- We only want pizza that were not cancelled
- So, we group by the type of pizza and count the number of pizza's that were delivered
  
| pizza_name | pizza |
| ---------- | ----- |
| Meatlovers | 9     |
| Vegetarian | 3     |

- Nine Meatlovers pizzas were delivered
- Three Vegetarian pizzas were delivered
---
### 5. How many Vegetarian and Meatlovers were ordered by each customer?
```sql
SELECT c.customer_id, p.pizza_name, count(c.pizza_id) FROM pizza_runner.customer_orders c
JOIN pizza_runner.pizza_names p ON c.pizza_id = p.pizza_id
GROUP BY c.customer_id, p.pizza_name
ORDER BY c.customer_id;
```
- First, we join the order table with the names of the pizza
- then, we group by the customer and the type of pizza, and count the number of pizzas that were ordered

| customer_id | pizza_name | count |
| ----------- | ---------- | ----- |
| 101         | Meatlovers | 2     |
| 101         | Vegetarian | 1     |
| 102         | Meatlovers | 2     |
| 102         | Vegetarian | 1     |
| 103         | Meatlovers | 3     |
| 103         | Vegetarian | 1     |
| 104         | Meatlovers | 3     |
| 105         | Vegetarian | 1     |

- Customer 101 ordered 2 meatlovers and 1 vegetarian pizzas
- Customer 102 ordered 2 meatlovers and 1 vegetarian pizzas
- Customer 103 ordered 3 meatlovers and 1 vegetarian pizzas
- Customer 104 ordered 3 meatlovers pizzas
- Customer 105 ordered 1 vegetarian pizza
---

### 6. What was the maximum number of pizzas delivered in a single order?
```sql
SELECT r.order_id, count(c.pizza_id) as num_of_pizzas FROM pizza_runner.customer_orders c
JOIN pizza_runner.runner_orders r ON r.order_id = c.order_id
GROUP BY r.order_id
ORDER BY num_of_pizzas DESC
LIMIT 1;
```
- first, we group by the order_id that were delivered to count how many pizzas were in the delivery
- then, we order by the number of pizzas from greatest to least, and select the first row
  
| order_id | num_of_pizzas |
| -------- | ------------- |
| 4        | 3             |

- Three was the maximum number of pizzas delivered
---

### 8. How many pizzas were delivered that had both exclusions and extras?
```sql
SELECT COUNT(r.order_id) AS number_of_pizzas FROM pizza_runner.runner_orders r
JOIN pizza_runner.customer_orders c ON r.order_id=c.order_id
WHERE r.cancellation = '' AND c.exclusions !='' AND c.extras !='';
```
- Here, we select for the rows where cancellation is blank, therefore, delivered, and exclusions and extras are not blank
- then we count the resulting rows
  
| number_of_pizzas |
| ---------------- |
| 1                |

- One pizza was delivered that had both exlusions and extras
---
### 9. What was the total volume of pizzas ordered for each hour of the day?
```sql
SELECT extract(hour from c.order_time) AS hour, COUNT(*) FROM pizza_runner.customer_orders c
GROUP BY hour
ORDER BY hour;
```
- we use extract to get only the hour of the timestamp,
- then, by grouping by the hour, we can count how many orders there were for each hour
  
| hour | count |
| ---- | ----- |
| 11   | 1     |
| 13   | 3     |
| 18   | 3     |
| 19   | 1     |
| 21   | 3     |
| 23   | 3     |

- At hour 11, there was one order
- At hour 13, there were three orders
- At hour 18, there were three orders
- At hour 19, there was one order
- At hour 21, there were three orders
- At hour 23, there were three orders
---

### 10. What was the volume of orders for each day of the week?
```sql
SELECT extract(dow from c.order_time) AS dow, case
WHEN extract(dow from c.order_time) = 1 THEN 'Monday'
WHEN extract(dow from c.order_time) = 2 THEN 'Tuesday'
WHEN extract(dow from c.order_time) = 3 THEN 'Wednesday'
WHEN extract(dow from c.order_time) = 4 THEN 'Thursday'
WHEN extract(dow from c.order_time) = 5 THEN 'Friday'
WHEN extract(dow from c.order_time) = 6 THEN 'Saturday'
WHEN extract(dow from c.order_time) = 7 THEN 'Sunday'
END AS name,
count(*) as num_of_orders FROM pizza_runner.customer_orders c
GROUP BY dow
ORDER BY dow;
```
- here we use extract and dow to get the day corresponding to the timestamp
- then, we use a case statement to account for all the numerical values of dow and result in the name of the day
- then by grouping by the dow, we can count how many orders were made during that day

| dow | name      | num_of_orders |
| --- | --------- | ------------- |
| 3   | Wednesday | 5             |
| 4   | Thursday  | 3             |
| 5   | Friday    | 1             |
| 6   | Saturday  | 5             |

- On Wednesday, there were five orders
- On Thursday, there were three orders
- On Friday, there was one order
- On Saturday, there were five orders
---

