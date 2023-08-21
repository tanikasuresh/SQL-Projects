# Danny's Diner
### Danny's Diner is Case Study #1 from Danny Ma's 8-Week SQL Challenge
Link: https://8weeksqlchallenge.com/case-study-1/

#### "Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen."

#### "Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite."

---
### Entity Relationship Diagram
![image](https://github.com/tanikasuresh/SQL-Projects/assets/139067248/d96a6ecc-38b1-41fa-ad50-c08cdee381f5)

---
### Table #1: sales
```sql
SELECT * FROM dannys_diner.sales;
```
| customer_id | order_date | product_id |
| ----------- | ---------- | ---------- |
| A           | 2021-01-01 | 1          |
| A           | 2021-01-01 | 2          |
| A           | 2021-01-07 | 2          |
| A           | 2021-01-10 | 3          |
| A           | 2021-01-11 | 3          |
| A           | 2021-01-11 | 3          |
| B           | 2021-01-01 | 2          |
| B           | 2021-01-02 | 2          |
| B           | 2021-01-04 | 1          |
| B           | 2021-01-11 | 1          |
| B           | 2021-01-16 | 3          |
| B           | 2021-02-01 | 3          |
| C           | 2021-01-01 | 3          |
| C           | 2021-01-01 | 3          |
| C           | 2021-01-07 | 3          |

---
### Table #2: members
```sql
SELECT * FROM dannys_diner.members;
```

| customer_id | join_date  |
| ----------- | ---------- |
| A           | 2021-01-07 |
| B           | 2021-01-09 |

---

### Table #3: menu
```sql
SELECT * FROM dannys_diner.menu;
```

| product_id | product_name | price |
| ---------- | ------------ | ----- |
| 1          | sushi        | 10    |
| 2          | curry        | 15    |
| 3          | ramen        | 12    |

---
