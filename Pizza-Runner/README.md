# Pizza Runner
### Pizza Runner is Case Study #2 from Danny Ma's 8-Week SQL Challenge
Link: https://8weeksqlchallenge.com/case-study-2/

#### "Danny was scrolling through his Instagram feed when something really caught his eye - '80s Retro Styling and Pizza Is The Future!'"

#### "Danny was sold on the idea, but he knew that pizza alone was not going to help him get seed funding to expand his new Pizza Empire - so he had one more genius idea to combine with it - he was going to Uberize it - and so Pizza Runner was launched!"

#### "Danny started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters"
---
### Entity Relationship Diagram
![image](https://github.com/tanikasuresh/SQL-Projects/assets/139067248/e0edd6af-dc4a-443e-9d39-a1e2d7d817a5)

---
### Table #1: customer_orders
```sql
SELECT * FROM pizza_runner.customer_orders;
```
| order_id | customer_id | pizza_id | exclusions | extras | order_time               |
| -------- | ----------- | -------- | ---------- | ------ | ------------------------ |
| 1        | 101         | 1        |            |        | 2020-01-01T18:05:02.000Z |
| 2        | 101         | 1        |            |        | 2020-01-01T19:00:52.000Z |
| 3        | 102         | 1        |            |        | 2020-01-02T23:51:23.000Z |
| 4        | 103         | 1        | 4          |        | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 1        | 4          |        | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 2        | 4          |        | 2020-01-04T13:23:46.000Z |
| 9        | 103         | 1        | 4          | 1, 5   | 2020-01-10T11:22:59.000Z |
| 10       | 104         | 1        | 2, 6       | 1, 4   | 2020-01-11T18:34:49.000Z |
| 5        | 104         | 1        |            | 1      | 2020-01-08T21:00:29.000Z |
| 7        | 105         | 2        |            | 1      | 2020-01-08T21:20:29.000Z |
| 3        | 102         | 2        |            |        | 2020-01-02T23:51:23.000Z |
| 6        | 101         | 2        |            |        | 2020-01-08T21:03:13.000Z |
| 8        | 102         | 1        |            |        | 2020-01-09T23:54:33.000Z |
| 10       | 104         | 1        |            |        | 2020-01-11T18:34:49.000Z |

---
### Table #2: runners
```sql
SELECT * FROM pizza_runner.runners;
```
| runner_id | registration_date|
| --------- | -----------------|
| 1         | 2021-01-01       |
| 2         | 2021-01-03       |
| 3         | 2021-01-08       |
| 4         | 2021-01-15       |

---
### Table #3: runner_orders
```sql
SELECT * FROM pizza_runner.runner_orders;
```

| order_id | runner_id | pickup_time              | distance | duration | cancellation            |
| -------- | --------- | ------------------------ | -------- | -------- | ----------------------- |
| 4        | 2         | 2020-01-04T13:53:03.000Z | 23.4     | 40       |                         |
| 5        | 3         | 2020-01-08T21:10:57.000Z | 10       | 15       |                         |
| 6        | 3         |                          |          |          | Restaurant Cancellation |
| 9        | 2         |                          |          |          | Customer Cancellation   |
| 3        | 1         | 2020-01-03T00:12:37.000Z | 13.4     | 20       |                         |
| 7        | 2         | 2020-01-08T21:30:45.000Z | 25       | 25       |                         |
| 1        | 1         | 2020-01-01T18:15:34.000Z | 20       | 32       |                         |
| 2        | 1         | 2020-01-01T19:10:54.000Z | 20       | 27       |                         |
| 10       | 1         | 2020-01-11T18:50:20.000Z | 10       | 10       |                         |
| 8        | 2         | 2020-01-10T00:15:02.000Z | 23.4     | 15       |                         |

---

### Table #4: pizza_names
```sql
SELECT * FROM pizza_runner.pizza_names;
```

| pizza_id | pizza_name |
| -------- | ---------- |
| 1        | Meatlovers |
| 2        | Vegetarian |

---

### Table #5 pizza_recipes
```sql
SELECT * FROM pizza_runner.pizza_recipes;
```
| pizza_id | toppings                |
| -------- | ----------------------- |
| 1        | 1, 2, 3, 4, 5, 6, 8, 10 |
| 2        | 4, 6, 7, 9, 11, 12      |

---

### Table #6: pizza_toppings
```sql
SELECT * FROM pizza_runner.pizza_toppings;
```

| topping_id | topping_name |
| ---------- | ------------ |
| 1          | Bacon        |
| 2          | BBQ Sauce    |
| 3          | Beef         |
| 4          | Cheese       |
| 5          | Chicken      |
| 6          | Mushrooms    |
| 7          | Onions       |
| 8          | Pepperoni    |
| 9          | Peppers      |
| 10         | Salami       |
| 11         | Tomatoes     |
| 12         | Tomato Sauce |

---


