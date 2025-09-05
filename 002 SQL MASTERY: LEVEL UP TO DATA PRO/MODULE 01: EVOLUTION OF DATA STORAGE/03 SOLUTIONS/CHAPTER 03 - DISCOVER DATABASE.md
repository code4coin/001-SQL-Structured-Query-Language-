---
<h1 align="center">DISCOVER DATABASE</h1>

---
## 🔑KEYWORDS
> **CREATE**

> **INSERT**

>**SELECT**

> **FROM**
---
## 📖DEFINITION
>**CREATE**
  - Defines a new table in the database.
  - Specify column names, data types, and optional constraints (like PRIMARY KEY, NOT NULL).
> **INSERT**
  - Adds new rows of data into a table.
  - Specify the table name, the columns to insert into, and the values for each column.
>**SELECT**
  - Choose columns or expressions to return.
  - Can include calculations, functions, aliases, and subqueries.
> **FROM**
  - Specifies the table(s) to query.
  - Can reference multiple tables with JOINs.
---

## 🧱QUERY FORMAT

> **SQL QUERY STRUCTURE:**

```sql
-- Format to create table in a database

CREATE TABLE table_name (
    column_name column_data_type any_constraint,
    column_name column_data_type any_constraint,
    ...
);

```

```sql
-- Format to insert data in a database table
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

```sql
-- 📝 Remember: ORDER OF EXECUTION
-- 1. FROM: specifies the table from which data will be retrieved
-- 2. SELECT: defines the columns to be displayed in the final result

SELECT
  column_names
FROM table_name;
```

<br>

> **SQL SAMPLE DATA:**

Table: **`employee`**
| EmployeeID | Name               | Department  | Salary      |
|------------|------------------|------------|------------|
| 1          | Benjamin Campbell | Finance    | 105000.36  |
| 2          | Sharon Mccormick  | Sales      | 56231.32   |
| 3          | Raymond Bass      | Operations | 98139.01   |
| 4          | Patrick Martin    | IT         | 108957.84  |


<br>

> **SQL QUERY EXAMPLE:**

❓**Problem Statement**: From `employee` table, retrieve all `names` stored inside.
```sql
SELECT
  name
FROM employee;
```
<br>

> **TRY YOURSELF: [CLICK TO EXECUTE QUERIES](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/002%20SQL%20MASTERY:%20LEVEL%20UP%20TO%20DATA%20PRO/MODULE%2000:%20INTRODUCTION%20TO%20COURSE/003%20SQL%20PLATFORM%20PREPARATION.md)**

<br>

> **SQL QUERY TEST:**

❓**PROBLEM STATEMENT 01**: Create `employee` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
CREATE TABLE employee (
    EmployeeID INT,
    Name TEXT, 
    Department	TEXT,
    Salary DECIMAL(10, 4)
);
```
</details>

❓**PROBLEM STATEMENT 02**: Insert records from above sample data into `employee` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
INSERT INTO employee VALUES
(1,'Benjamin Campbell','Finance',105000.36),
(2,'Sharon Mccormick','Sales',56231.32),
(3,'Raymond Bass','Operations',98139.01),
(4,'Patrick Martin','IT',108957.84);
```
</details>

❓**PROBLEM STATEMENT 03**: Retrieve `employeeid` and `name` from `employee` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT
  employeeid,
  name
FROM employee;
```
</details>

---
## 💡TIP TO REMEMBER
> **ORDER OF EXECUTION**
```sql 
FROM → SELECT
```
- **_FROM executes first_**: SQL looks at which table to query.
- **_SELECT executes second_**: SQL picks the columns to return.

<br>

> **RETRIEVE ALL COLUMNS**
- Replace specific column names with the wildcard (*) in a SELECT statement to fetch all columns from the table.
---
## 💪PLATFORM
Before moving to the exercises, we need a platform with tables and data: [PLATFORM](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/002%20SQL%20MASTERY:%20LEVEL%20UP%20TO%20DATA%20PRO/MODULE%2000:%20INTRODUCTION%20TO%20COURSE/003%20SQL%20PLATFORM%20PREPARATION.md)

---
## DATA PREPARATION
Before moving into exercise, we need to prepare table with data in database: [SQL QUERY](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/002%20SQL%20MASTERY%3A%20LEVEL%20UP%20TO%20DATA%20PRO/MODULE%2001%3A%20EVOLUTION%20OF%20DATA%20STORAGE/00%20DATASETS/employee_data_table.sql)

---
## 💪EXERCISE

1. Total headcount of the organization
<details>
  <summary>✅ Solution:</summary>

```sql
SELECT COUNT(*)
FROM employee;
```
</details>

2. Employees' headcount at the departmental level
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT department, COUNT(*) AS department_headcount
FROM patrons
GROUP BY department;
```
</details>

3. The average salary of an employee in the organization
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT AVG(salary) AS employee_avg_salary
FROM employee;
```
</details>

4. Average salary of employees in various departments
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT department, AVG(salary) AS employee_avg_salary
FROM employee
GROUP BY department;
```
</details>

5. Minimum and maximum salaries drawn from every department
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT department
       MIN(salary) AS employee_min_salary,
       MAX(salary) AS employee_max_salary
FROM employee
GROUP BY department;
```
</details>

---
## 🧠PRACTISE
1. Write a query to retrieve field `book_title` from `checkouts` table
2. Write a query to retrieve all fields from `checkouts` table, using `wildcard (*)`
---
## ✅ SOLUTIONS
1. Write a query to retrieve field `book_title` from `checkouts` table
```sql
SELECT book_title
FROM checkouts;
```
2. Write a query to retrieve all fields from `checkouts` table, using `wildcard (*)`
```sql
SELECT *
FROM checkouts;
```
---
## 🔗**MORE RESOURCES** 

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)  
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)  
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)  
