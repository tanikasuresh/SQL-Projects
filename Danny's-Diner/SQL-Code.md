# Here is my code for the Danny's Diner Case Study!
### 1. What is the total amount each customer spent at the restaurant?
```sql
SELECT s.customer_id, sum(m.price) AS total_price from dannys_diner.menu m
JOIN dannys_diner.sales s ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id
```
- First we join 'menu' with 'sales' to match the product_id with its price
- Then, we group by the customer_id so we can find the sum of the prices for each customer
  
| customer_id | total_price |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

- Customer A spent a total of $76
- Customer B spent a total of $74
- Customer C spent a total of $36
---
### 2. How many days has each customer visited the restaurant?
```sql
SELECT customer_id, count(distinct(order_date)) AS days FROM dannys_diner.sales
GROUP BY customer_id
ORDER BY customer_id
```
- By grouping the table by the customer_id, we can count the number of days each customer visited the restaurant
- keeping in mind that there are repeated dates if the customer ordered multiple items
- we tackle this by using distinct to count only the unique dates
  
| customer_id | days |
| ----------- | ---- |
| A           | 4    |
| B           | 6    |
| C           | 2    |

- Customer A visited four times
- Customer B visited six times
- Customer C visited two times
---
### 3. What was the first item from the menu purchased by each customer?
```sql
WITH orders_cte AS
(
  SELECT s.customer_id, men.product_name, ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY order_date, s.product_id) AS ranking FROM dannys_diner.sales s
JOIN dannys_diner.menu men ON men.product_id = s.product_id
ORDER BY s.customer_id, s.order_date, s.product_id
  )
 SELECT customer_id, product_name FROM orders_cte
 WHERE ranking = 1
```
- we can use a cte to display all the orders and their row_number
- then select for the order whose row numbers are one
- giving us the first item that was ordered

| customer_id | product_name |
| ----------- | ------------ |
| A           | sushi        |
| B           | curry        |
| C           | ramen        |
- Customer A ordered sushi as their first item
- Customer B ordered curry as their first item
- Customer C ordered ramen as their first item
---
### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
```sql
SELECT count(s.product_id) AS num_purchases, m.product_name AS name FROM dannys_diner.sales s
JOIN dannys_diner.menu m ON m.product_id = s.product_id
GROUP BY s.product_id, m.product_name
ORDER BY num_purchases DESC
LIMIT 1
```
- we join sales and menu to match the product_id and the product_name
- next, we group by the product_id and product_name so we can count the number of times that item was purchased
- finally, we can order by the count (num_purchases) in descending order and limit by 1 to get the most purchased item

| num_purchases | name  |
| ------------- | ----- |
| 8             | ramen |
- Ramen is the most purchased item with eight purchases
---
### 5. Which item was the most popular for each customer?
```sql
WITH all_orders_cte AS
(
  SELECT s.customer_id, men.product_name, count(men.product_name) as num_of_orders,
  RANK() OVER(PARTITION BY s.customer_id ORDER BY count(men.product_name) desc) as ranking FROM dannys_diner.sales s
JOIN dannys_diner.menu men ON men.product_id = s.product_id
GROUP BY s.customer_id, men.product_name
ORDER BY s.customer_id, ranking
  )
  SELECT customer_id, product_name, num_of_orders FROM all_orders_cte
  WHERE ranking = 1
```
- popularity can be found by ranking the number of times an item was purchased by each customer
- then selecting for the rows that have a ranking as one

| customer_id | product_name | num_of_orders |
| ----------- | ------------ | ------------- |
| A           | ramen        | 3             |
| B           | ramen        | 2             |
| B           | curry        | 2             |
| B           | sushi        | 2             |
| C           | ramen        | 3             |

- The most popular item for Customer A was ramen, ordered thrice
- The most popular item for Customer B was ramen, curry, and sushi, ordered twice each
- The most popular item for Customer C was ramen, ordered thrice
---

### 6. Which item was purchased first by the customer after they became a member?
```sql
WITH dates_cte AS
(
  SELECT s.customer_id, s.order_date, men.product_name, m.join_date,
  RANK() OVER(PARTITION BY s.customer_id ORDER BY s.order_date) as ranking
  FROM dannys_diner.sales s
JOIN dannys_diner.members m on m.customer_id=s.customer_id
JOIN dannys_diner.menu men on men.product_id=s.product_id
WHERE s.order_date>m.join_date
ORDER BY s.customer_id
)
SELECT customer_id, product_name FROM dates_cte
WHERE ranking = 1
```
- we can use a cte to find all of the rows where the order date is greater than the join date
- then select the rows for which the ranking is one

| customer_id | product_name |
| ----------- | ------------ |
| A           | ramen        |
| B           | sushi        |

- The first item Customer A ordered after becoming a member is ramen
- The first item Customer B ordered after becoming a member is sushi
- Customer C never became a member
---

### 7. Which item was purchased just before the customer became a member?
```sql
WITH dates_cte AS
(
  SELECT s.customer_id, s.order_date, men.product_name, m.join_date,
  ROW_NUMBER() OVER(PARTITION BY s.customer_id ORDER BY s.order_date DESC) as ranking
  FROM dannys_diner.sales s
JOIN dannys_diner.members m on m.customer_id=s.customer_id
JOIN dannys_diner.menu men on men.product_id=s.product_id
WHERE s.order_date<m.join_date
ORDER BY s.customer_id
)
SELECT customer_id, product_name FROM dates_cte
WHERE ranking = 1
```
- this is similar to the previous query, except we select the rows where the
- order date is less than the join date
- then we rank based on the order date so the closest date to the join date is highest
- we then select the rows where the rank is one

| customer_id | product_name |
| ----------- | ------------ |
| A           | sushi        |
| B           | sushi        |

- The first item Customer A and Customer B ordered prior to membership is sushi
---
### 8. What is the total items and amount spent for each member before they became a member?
```sql
SELECT mem.customer_id AS member, count(s.product_id) AS total_items, sum(m.price) AS total_amount FROM dannys_diner.sales s
JOIN dannys_diner.menu m ON m.product_id = s.product_id
JOIN dannys_diner.members mem ON s.customer_id = mem.customer_id
WHERE s.order_date < mem.join_date
GROUP BY mem.customer_id
ORDER BY mem.customer_id
```
- first we join the sales, menu, and member tables to get the customer, what item they purchased, and the price
- next we group by the customer_id so we can find the total count of the items bought
- and the sum of all the prices


| member | total_items | total_amount |
| ------ | ----------- | ------------ |
| A      | 2           | 25           |
| B      | 3           | 40           |

- Before becoming a member, Customer A ordered two items worth $25
- Before membership, Customer B ordered three items worth $40
---

### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier how many points would each customer have?
```sql
WITH point_system_cte AS
(
	SELECT m.product_id, m.price, CASE
	WHEN m.product_id = 2 OR m.product_id = 3 THEN m.price*10
	WHEN m.product_id = 1 THEN m.price*20
	END AS points
	FROM dannys_diner.menu m
 )
 SELECT s.customer_id, sum(ps.points) FROM dannys_diner.sales s
 JOIN point_system_cte ps ON ps.product_id = s.product_id
 GROUP BY s.customer_id
 ORDER BY s.customer_id
```
- first we need a cte that detail the amount of points each product gets using a case
- to account for all the products
- next we will join the points_system_cte with the sales to match the product
- the customer order with the points it receives
- lastly, by grouping based on the customer, we can find the sum of all the points

| customer_id | sum |
| ----------- | --- |
| A           | 860 |
| B           | 940 |
| C           | 360 |
- Customer A has a total of 860 points
- Customer B has 940 points
- Customer C has 360 points
---

### Join All The Things: Join the tables to create a single table that shows the customers_id, order_date, product_name, price, and whether the customer was a member at the time of the order
```sql
SELECT s.customer_id, s.order_date, m.product_name, m.price, CASE
	WHEN s.customer_id not in (select mem.customer_id from dannys_diner.members mem)
	OR s.order_date < mem.join_date THEN 'N'
	ELSE 'Y'
END AS member
FROM dannys_diner.sales s
LEFT JOIN dannys_diner.members mem ON mem.customer_id = s.customer_id
JOIN dannys_diner.menu m ON s.product_id = m.product_id
ORDER BY s.customer_id, s.order_date, m.product_name
```
- the case in the select clause functions to classify whether the customer
- was a member or not at the time of the purchase
- in other words, if the customer has never been a member or the made the purchase
- prior to their joindate

**Schema (PostgreSQL v13)**

    CREATE SCHEMA dannys_diner;
    SET search_path = dannys_diner;
    
    CREATE TABLE sales (
      "customer_id" VARCHAR(1),
      "order_date" DATE,
      "product_id" INTEGER
    );
    
    INSERT INTO sales
      ("customer_id", "order_date", "product_id")
    VALUES
      ('A', '2021-01-01', '1'),
      ('A', '2021-01-01', '2'),
      ('A', '2021-01-07', '2'),
      ('A', '2021-01-10', '3'),
      ('A', '2021-01-11', '3'),
      ('A', '2021-01-11', '3'),
      ('B', '2021-01-01', '2'),
      ('B', '2021-01-02', '2'),
      ('B', '2021-01-04', '1'),
      ('B', '2021-01-11', '1'),
      ('B', '2021-01-16', '3'),
      ('B', '2021-02-01', '3'),
      ('C', '2021-01-01', '3'),
      ('C', '2021-01-01', '3'),
      ('C', '2021-01-07', '3');
     
    
    CREATE TABLE menu (
      "product_id" INTEGER,
      "product_name" VARCHAR(5),
      "price" INTEGER
    );
    
    INSERT INTO menu
      ("product_id", "product_name", "price")
    VALUES
      ('1', 'sushi', '10'),
      ('2', 'curry', '15'),
      ('3', 'ramen', '12');
      
    
    CREATE TABLE members (
      "customer_id" VARCHAR(1),
      "join_date" DATE
    );
    
    INSERT INTO members
      ("customer_id", "join_date")
    VALUES
      ('A', '2021-01-07'),
      ('B', '2021-01-09');

---

**Query #1**

    SELECT s.customer_id, s.order_date, m.product_name, m.price, CASE
    	WHEN s.customer_id not in (select mem.customer_id from dannys_diner.members mem)
    	OR s.order_date < mem.join_date THEN 'N'
    	ELSE 'Y'
    END AS member
    FROM dannys_diner.sales s
    LEFT JOIN dannys_diner.members mem ON mem.customer_id = s.customer_id
    JOIN dannys_diner.menu m ON s.product_id = m.product_id
    ORDER BY s.customer_id, s.order_date, m.product_name;
    
- the case in the select clause functions to classify whether the customer
- was a member or not at the time of the purchase
- in other words, if the customer has never been a member or the made the purchase
- prior to their joindate
  

| customer_id | order_date | product_name | price | member |
| ----------- | -----------| ------------ | ----- | ------ |
| A           | 2021-01-01 | curry        | 15    | N      |
| A           | 2021-01-01 | sushi        | 10    | N      |
| A           | 2021-01-07 | curry        | 15    | Y      |
| A           | 2021-01-10 | ramen        | 12    | Y      |
| A           | 2021-01-11 | ramen        | 12    | Y      |
| A           | 2021-01-11 | ramen        | 12    | Y      |
| B           | 2021-01-01 | curry        | 15    | N      |
| B           | 2021-01-02 | curry        | 15    | N      |
| B           | 2021-01-04 | sushi        | 10    | N      |
| B           | 2021-01-11 | sushi        | 10    | Y      |
| B           | 2021-01-16 | ramen        | 12    | Y      |
| B           | 2021-02-01 | ramen        | 12    | Y      |
| C           | 2021-01-01 | ramen        | 12    | N      |
| C           | 2021-01-01 | ramen        | 12    | N      |
| C           | 2021-01-07 | ramen        | 12    | N      |
  
---
  











