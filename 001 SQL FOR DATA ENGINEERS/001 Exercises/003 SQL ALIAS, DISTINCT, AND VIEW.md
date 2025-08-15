# SQL - ALIAS, DISTINCT, AND VIEW
---
## KEYWORDS
- **`ALIAS`** – Rename colummn and table names in the result set  
- **`DISTINCT`** – returns unique values of records.
- **`VIEW`** – store query in sql, can be accessed as a table.
---
## QUERY FORMAT
```sql
--1. ALIAS - use `AS` keyword
SELECT field_name AS alias_column_name
FROM table_name AS alias_table_name;
```
```sql
--2. DISTINCT - use `DISTINCT` keyword
SELECT DISTINCT field_names
FROM table_name;
```
```sql
--3. view - use `VIEW` and `AS` keyword
CREATE VIEW view_name AS
QUERY;
```
---
## EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

---
### 1. Aliasing column name from `employees` table
```sql
SELECT name AS first_name
FROM employees;
```
### 2. Retrieve unique values of year_hired from `employees` table
```sql
SELECT DISTINCT year_hired 
FROM employees;
```
### 3. Create and retrieve `View` from `employees` table, consisting of columns emp_id and year_hired
```sql
CREATE VIEW employee_hired_years AS
SELECT emp_id, year_hired
FROM employees;

SELECT * 
FROM employee_hired_years;
```
---
## PRACTISE
### 1. Write a query to alias column `year_hired` as `joining_year` from `employees` table
```sql
SELECT year_hired as joining_year
FROM employees;
```
### 2. Retrieve distinct values for `year_hired` as `joining_year` from `employees` table
```sql
SELECT DISTINCT year_hired as joining_year
FROM employees;
```
### 3. Create `VIEW` for distinct values for `year_hired` as `joining_year` from `employees` table
```sql
CREATE VIEW employee_hired_years AS
SELECT DISTINCT year_hired as joining_year
FROM employees;

SELECT * 
FROM employee_hired_years;
```
---
## **Contributing** 🤝

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---
## **License** 📄

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---
## **More Resources** 🔗

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
