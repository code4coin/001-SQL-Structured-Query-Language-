# SQL - NULL AND COUNT
---
## KEYWORDS
- **NULL**
- **COUNT**
- ---
## DEFINITION
- **NULL** - A marker in a table that represents missing, undefined, or unknown data. It’s not zero or blank, just the absence of a value.
- **COUNT** – count number of records in a table
---
## QUERY FORMAT
```sql
--1. COUNT - use `COUNT` and `AS` keyword
SELECT COUNT(single_field_name) AS count_non_null_values,
COUNT(*) AS count_total_records
FROM table_name;
```
---
## TIP
- Wildcard (*) --> All fields in the a record
- The COUNT() function returns the number of non-NULL values in a column, or the total number of rows when using COUNT(*).
- Single entity only: COUNT() can only operate on one column at a time or the entire table with *.
- Why multiple columns fail: SQL does not allow multiple columns directly in COUNT() because it expects a single expression to aggregate. To count unique combinations of multiple columns, you need COUNT(DISTINCT ROW(col1, col2)) in PostgreSQL or concatenate columns as COUNT(DISTINCT col1 + " " + col2) in MySQL.
- **`Order of Execution`**
  1. `FROM`– Determines the tables to retrieve data from and performs joins if any.
  2. `SELECT`– Chooses which columns or expressions to return.
     - `DISTINCT`– Removes duplicate rows from the result set.
     - `COUNT` - Counts rows or values
  3. `LIMIT` - – Restricts the number of rows returned or skips rows.
---
## EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

---
### 1. Count total names in people table
```sql
SELECT COUNT(name) AS count_name
FROM people;
```
### 2. Count names and birthdate from people table
```sql
SELECT COUNT(name) AS count_name,
COUNT(birthdate) AS count_birthdate
FROM people;
```
### 3. Count names and distinct name in same query from people table
```sql
SELECT COUNT(name) AS count_name,
COUNT(DISTINCT name) AS distinct_count_name
FROM people;
```
---
## PRACTISE
1. Count total emails in people table
2. Count birthdate and email from people table
3. Count email and distinct email in same query from people table
---
## SOLUTIONS
### 1. Count total emails in people table
```sql
SELECT COUNT(email) AS count_email
FROM people;
```
### 2. Count birthdate and email from people table
```sql
SELECT COUNT(birthdate) AS count_birthdate,
COUNT(email) AS count_email
FROM people;
```
### 3. Count email and distinct email in same query from people table
```sql
SELECT COUNT(email) AS count_email,
COUNT(DISTINCT email) AS distinct_count_email
FROM people;
```
---
## **CONTRIBUTING** 🤝

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---
## **LICENSE** 📄

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---
## **MORE RESOURCES** 🔗

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
