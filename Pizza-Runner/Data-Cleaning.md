## Let's take a look at a few tables, specifically, customer_orders and runner_orders
```sql
SELECT * FROM pizza_runner.customer_orders;
```
### customer_orders
| order_id | customer_id | pizza_id | exclusions | extras | order_time               |
| -------- | ----------- | -------- | ---------- | ------ | ------------------------ |
| 1        | 101         | 1        |            |        | 2020-01-01T18:05:02.000Z |
| 2        | 101         | 1        |            |        | 2020-01-01T19:00:52.000Z |
| 3        | 102         | 1        |            |        | 2020-01-02T23:51:23.000Z |
| 3        | 102         | 2        |            |        | 2020-01-02T23:51:23.000Z |
| 4        | 103         | 1        | 4          |        | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 1        | 4          |        | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 2        | 4          |        | 2020-01-04T13:23:46.000Z |
| 5        | 104         | 1        | null       | 1      | 2020-01-08T21:00:29.000Z |
| 6        | 101         | 2        | null       | null   | 2020-01-08T21:03:13.000Z |
| 7        | 105         | 2        | null       | 1      | 2020-01-08T21:20:29.000Z |
| 8        | 102         | 1        | null       | null   | 2020-01-09T23:54:33.000Z |
| 9        | 103         | 1        | 4          | 1, 5   | 2020-01-10T11:22:59.000Z |
| 10       | 104         | 1        | null       | null   | 2020-01-11T18:34:49.000Z |
| 10       | 104         | 1        | 2, 6       | 1, 4   | 2020-01-11T18:34:49.000Z |

```sql
SELECT * FROM pizza_runner.runner_orders;
```
### runner_orders
| order_id | runner_id | pickup_time         | distance | duration   | cancellation            |
| -------- | --------- | ------------------- | -------- | ---------- | ----------------------- |
| 1        | 1         | 2020-01-01 18:15:34 | 20km     | 32 minutes |                         |
| 2        | 1         | 2020-01-01 19:10:54 | 20km     | 27 minutes |                         |
| 3        | 1         | 2020-01-03 00:12:37 | 13.4km   | 20 mins    |                         |
| 4        | 2         | 2020-01-04 13:53:03 | 23.4     | 40         |                         |
| 5        | 3         | 2020-01-08 21:10:57 | 10       | 15         |                         |
| 6        | 3         | null                | null     | null       | Restaurant Cancellation |
| 7        | 2         | 2020-01-08 21:30:45 | 25km     | 25mins     | null                    |
| 8        | 2         | 2020-01-10 00:15:02 | 23.4 km  | 15 minute  | null                    |
| 9        | 2         | null                | null     | null       | Customer Cancellation   |
| 10       | 1         | 2020-01-11 18:50:20 | 10km     | 10minutes  | null                    |

#### It is evident that these tables need to be cleaned in order to perform queries and get accurate results. 
#### We need to take care of the null values and inconsistent formatting in the distance and duration columns

### Table: customer_orders
- First, we need to replace the entries that are null or say 'null' in the exclusion column to match the entries that are empty to ensure that the format is consistent

```sql
update pizza_runner.customer_orders
set exclusions = ''
where exclusions = 'null' or exclusions is null;
```
- Then, we do the same thing for the extras column
```sql
update pizza_runner.customer_orders
set extras = ''
where extras = 'null' or extras is null;
```
### Table: runner_orders
- the PICKUP_TIME column needs to be updated so that varchar null is made into a null value
- then, we need to change the type from varchar to timestamp

```sql
update pizza_runner.runner_orders
set pickup_time = null
where pickup_time = 'null';

ALTER TABLE pizza_runner.runner_orders
ALTER COLUMN pickup_time SET DATA TYPE TIMESTAMP
USING pickup_time::TIMESTAMP;
```

- the DISTANCE column needs to be updated so that varchar null is made into a null value
- Furthermore, the nonnull values should only include numbers with no letters or spaces
- so we can change its type from varchar to float

```sql
update pizza_runner.runner_orders
set distance = null
where distance = 'null';

update pizza_runner.runner_orders
set distance = replace(distance, 'km', '')
where distance like '%km%';

update pizza_runner.runner_orders
set distance = trim( distance );

ALTER TABLE pizza_runner.runner_orders
ALTER COLUMN distance SET DATA TYPE FLOAT
USING distance::FLOAT;
```
- the CANCELLATION column needs to be updated to a blank if the value is null

```sql
update pizza_runner.runner_orders
set cancellation = ''
where cancellation = 'null' or cancellation is null;
```
- the DURATION column should be updated to null if the value is a varchar 'null'
- then, we ensure that the column has only numbers, no letters and spaces
- after which, we can change the type from varchar to integer

```sql
update pizza_runner.runner_orders
set duration = null
where duration = 'null';

update pizza_runner.runner_orders
set duration = trim('min' from duration)
where duration like '%min%';

update pizza_runner.runner_orders
set duration = trim('mins' from duration)
where duration like '%mins%';

update pizza_runner.runner_orders
set duration = trim('minutes' from duration)
where duration like '%minutes%';

update pizza_runner.runner_orders
set duration = trim('minute' from duration)
where duration like '%minute%';

update pizza_runner.runner_orders
set duration = trim( duration );

ALTER TABLE pizza_runner.runner_orders
ALTER COLUMN duration SET DATA TYPE INTEGER
USING duration::INTEGER;
```
### Let's take another look at the two tables after these changes are applied
```sql
SELECT * FROM pizza_runner.customer_orders;
```
## customer_orders
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

## runner_orders
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

### Now, these tables are formatted consistently and are ready to be used with the queries.



