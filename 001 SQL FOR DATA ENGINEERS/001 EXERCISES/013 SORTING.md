# SORTING IN SQL
---
## 🔑KEYWORDS
- **Sorting results**
- **ORDER BY**
- **ASC (Ascending)**
- **DESC (Descending)**
- **Sorting by multiple fields**
- **Order of execution**
---
## 📖DEFINITION
- **Sorting results** – Organizing query results in a specific order to make data easier to read and analyze.
- **ORDER BY** – SQL keyword used to sort results by one or more columns. By default, it sorts in ascending order.
- **ASC (Ascending)** – Sorts data from smallest to largest or A to Z.
- **DESC (Descending)** – Sorts data from largest to smallest or Z to A.
- **Sorting by multiple fields** – Allows sorting on more than one column; subsequent columns act as tie-breakers.
- **SQL order of execution** – Sequence in which SQL processes a query: `FROM` → `WHERE` → `SELECT` → `ORDER BY` → `LIMIT`.
---
## 🧱QUERY FORMAT
```sql
-- 1. Basic sorting by a single column
SELECT column_names
FROM table_name
ORDER BY rating DESC;
```
```sql
-- 2. Sorting by multiple columns
SELECT col_01, col_02, col_03
FROM table_name
ORDER BY col_01 DESC, col_02 ASC;
```
```sql
-- 3. Sorting with NULL filtering
SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL
ORDER BY column_name DESC;
```
```sql
-- 4. Sorting without selecting the sorted column
SELECT col_01
FROM table_name
ORDER BY col_02 ASC;
```
```sql
-- 5. Different order for multiple fields
SELECT col_01, col_02, col_03
FROM table_name
ORDER BY col_01 DESC, col_02 ASC;
```
---
## 💡TIP TO REMEMBER
- “SELECT chooses the columns, ORDER BY chooses the order.
- **ORDER OF EXECUTION: FROM -> WHERE -> SELECT -> ORDER BY -> LIMIT**
- Alias in SELECT? Fine in ORDER BY; not in WHERE - Due to ORDER OF EXECUTION”
- You do not need to include the column in SELECT to sort by it, but it improves clarity.
- Use ASC for ascending order and DESC for descending order.
- When sorting by multiple columns, the second column acts as a tie-breaker if the first column has duplicate values.
- **NULLs usually sort first in ASC and last in DESC (engine-dependent).**

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/002%20PATRONS%20DATA.md)

###  1. List all movies sorted by rating descending
```sql
SELECT title, rating
FROM movies
ORDER BY rating DESC;
```
###  2. Sort movies by release_year ascending
```sql
SELECT title, release_year
FROM movies
ORDER BY release_year ASC;
```
###  3. Sort movies first by genre ascending, then by rating descending
```sql
SELECT title, genre, rating
FROM movies
ORDER BY genre ASC, rating DESC;
```
###  4. Sort movies by budget descending, ignoring NULL budgets
```sql
SELECT title, budget
FROM movies
WHERE budget IS NOT NULL
ORDER BY budget DESC;
```
---
## 🧠Practise
1. Sort movies by runtime_minutes descending
2. Sort movies by director ascending, then release_year descending
3. Sort movies by title ascending and rating descending
4. Sort movies by release_year descending and then by genre ascending

---
## ✅SOLUTIONS
### 1. Sort movies by runtime_minutes descending
```sql
SELECT title, runtime_minutes
FROM movies
ORDER BY runtime_minutes DESC;
```
### 2. Sort movies by director ascending, then release_year descending
```sql
SELECT title, director, release_year
FROM movies
ORDER BY director ASC, release_year DESC;
```
### 3. Sort movies by title ascending and rating descending
```sql
SELECT title, rating
FROM movies
ORDER BY title ASC, rating DESC;
```
### 4. Sort movies by release_year descending and then by genre ascending
```sql
SELECT title, release_year, genre
FROM movies
ORDER BY release_year DESC, genre ASC;
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
