# ALIASING AND ARITHMETIC IN SQL
---
## 🔑KEYWORDS
- **Arithmetic operations**
- **Aliasing (AS)**
- **Aggregate functions (SUM, MAX, MIN)**
- **Order of execution**
---
## 📖DEFINITION
- **Arithmetic operations** – Performing calculations like addition (+), subtraction (-), multiplication (*), and division (/) on table fields in SQL.
- **Aliasing (AS)** – Giving a temporary name to a column or calculation in a query result to improve readability.
- **Aggregate functions** – Functions such as SUM(), MAX(), MIN() that summarize data across multiple rows, unlike arithmetic which works row by row.
- **SQL order of execution** – The sequence in which SQL processes a query: `FROM` → `WHERE` → `SELECT` → `LIMIT`. Aliases cannot be referenced before they are created.
---
## 🧱QUERY FORMAT
```sql
-- 1. Arithmetic Operation Query 
SELECT
        arthimetic_operations AS alias_for_arthimetic_operations
  FROM table_name;
```
```sql
-- 2. Aggregate Function Query 
SELECT
        AGG_FUNC(column_name) AS alias_name
  FROM table_name;
```
---
## 💡TIP TO REMEMBER
- Arithmetic operations work row by row, aggregate functions work across rows
- You cannot use aliases in the WHERE clause; use the full expression instead
- Always alias every computed column before you hit “Run”. Your future-self (and every teammate who inherits the query) will thank you.

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

### 1. Convert runtime_minutes field into hours as runtime_hours make sure to convert to neartest 2 decimal values.
```sql
SELECT ROUND(runtime_minutes / 60.0, 2) AS runtime_hours
  FROM movies;
```
**NOTE - Notice, we have used 60.0 instead of 60, this is done to ensure SQL takes this as decimal division instead of integer division**

### 2. Find the total runtime of all movies and the average rating using aggregate functions.
```sql
SELECT SUM(runtime_minutes) AS total_runtime,
       AVG(rating) AS average_rating
FROM movies;
```
### 3. Retrieve the maximum and minimum `runtime_minutes` from the table with proper aliases.
```sql
SELECT MIN(runtime_minutes) AS shortest_movie,
       MAX(runtime_minutes) AS longest_movie
  FROM movies;
```
### 4. List all movies where the rating is higher than 8, and also show the difference between `runtime_minutes` and `rating` as `score_difference`.
```sql
SELECT runtime_minutes - rating AS out_of_options_for_better_examples
  FROM movies
 WHERE rating > 8;
```
---
## 🧠Practise
1. Calculate the difference between runtime and 120 minutes for each movie.
2. Increase each movie’s rating by 0.5 (but do not update the table, just show in query result).
3. Calculate half of the runtime for each movie.
4. Combine runtime and rating in an expression: runtime * rating as "weighted_score".
---
## ✅SOLUTIONS
### 1. Calculate the difference between runtime and 120 minutes for each movie.
```sql
SELECT runtime_minutes - 120 AS runtime_diff_by_120
  FROM movies;
```
### 2. Increase each movie’s rating by 0.5 (but do not update the table, just show in query result).
```sql
SELECT rating + 0.5 AS increased_rating
  FROM movies;
```
### 3. Calculate half of the runtime for each movie.
```sql
SELECT runtime_minutes / 2 AS half_runtime
  FROM movies;
```
### 4. Combine runtime and rating in an expression: (runtime * rating) as "weighted_score", and (runtime * rating)/ 100 as "std_weighted_score" plus round to nearest decimal 100. 
```sql
SELECT (runtime_minutes * rating) AS weighted_score,
       ROUND((runtime_minutes * rating)/ 100, 2) AS std_weighted_score
  FROM movies;
```
---
## 🤝**CONTRIBUTING** 

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---
## 📄**LICENSE** 

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---
## 🔗**MORE RESOURCES** 

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
