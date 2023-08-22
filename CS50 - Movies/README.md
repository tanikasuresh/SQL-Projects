# _Movies_
### _Movies_ is a database and problem set provided by Harvard's 2022 CS50: Introduction to Computer Science course
Link: https://cs50.harvard.edu/x/2022/psets/7/movies/ 
#### "Provided . . . is a file called movies.db, a SQLite database that stores data from IMDb about movies, the people who directed and starred in them, and their ratings."
#### "The challenge ahead of you is to write SQL queries to answer a variety of different questions by selecting data from one or more of these tables."
---
### Entity Relationship Diagram
![image](https://github.com/tanikasuresh/SQL-Projects/assets/139067248/f3822f66-58ca-43ea-9ff3-fcd842ceb363)

---
### Table #1: movies
```sql
SELECT COUNT(*) AS num_of_rows FROM movies;
```
| num_of_rows |
|-------------|
| 384939      |
```sql
SELECT * FROM movies
LIMIT 5;
```
|  id   |         title          | year |
| ----- | ---------------------- | ---- |
| 11216 | Spanish Fiesta         | 2019 |
| 11801 | Tötet nicht mehr       | 2019 |
| 15414 | La tierra de los toros | 2000 |
| 15724 | Dama de noche          | 1993 |
| 16906 | Frivolinas             | 2014 |

___
### Table #2: people
```sql
SELECT COUNT(*) AS num_of_rows FROM people;
```
| num_of_rows |
|-------------|
| 1178573     |

```sql
SELECT * FROM people
LIMIT 5;
```
| id |      name       | birth |
| -- | --------------- | ----- |
| 1  | Fred Astaire    | 1899  |
| 2  | Lauren Bacall   | 1924  |
| 3  | Brigitte Bardot | 1934  |
| 4  | John Belushi    | 1949  |
| 5  | Ingmar Bergman  | 1918  |

___
### Table #3: stars
```sql
SELECT COUNT(*) AS num_of_rows FROM stars;
```
| num_of_rows |
|-------------|
| 1350873     |
```sql
SELECT * FROM stars
LIMIT 5;
```
| movie_id | person_id |
| -------- | --------- |
| 11216    | 290157    |
| 11216    | 300388    |
| 11216    | 869559    |
| 11216    | 595321    |
| 11801    | 459029    |

___

### Table #4: directors
```sql
SELECT COUNT(*) AS num_of_rows FROM directors;
```
| num_of_rows |
|-------------|
| 378967      |
```sql
SELECT * FROM directors
LIMIT 5;
```
| movie_id | person_id |
|----------|-----------|
| 11216    | 241273    |
| 15724    | 529960    |
| 16906    | 136068    |
| 25541    | 560935    |
| 31458    | 649563    |

___

### Table #5: ratings
```sql
SELECT COUNT(*) AS num_of_rows FROM ratings;
```
| num_of_rows |
|-------------|
| 213665      |
```sql
SELECT * FROM ratings
LIMIT 5;
```
| movie_id | rating | votes |
|----------|--------|-------|
| 11216    | 6.9    | 29    |
| 15414    | 5.4    | 14    |
| 15724    | 6.0    | 25    |
| 16906    | 5.6    | 19    |
| 25541    | 7.5    | 21    |

___
