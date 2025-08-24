<!-- 
---

<p align="center">
  <img src="https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/000%20DOCUMENTS/GITHUB%20BANNER%20LOGO.png" 
       alt="Banner Logo" style="max-width:100%; height:auto;">
</p>
-->
---
<h1 align="center">SQL - SELECT & FROM</h1>

---
## 🔑KEYWORDS
> **SELECT**

> **FROM**
---
## 📖DEFINITION
> **SELECT**
  - Choose columns or expressions to return.
  - Can include calculations, functions, aliases, and subqueries.
> **FROM**
  - Specifies the table(s) to query.
  - Can reference multiple tables with JOINs.
---

## 🧱QUERY FORMAT

> **SQL QUERY STRUCTURE:**
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

Table: **`people`**
| id | name    | birthdate   | email               |
|----|---------|------------|-------------------|
| 1  | Alice   | 1990-05-12 | alice@example.com |
| 2  | Bob     | 1985-07-23 | bob@example.com   |
| 3  | Alice   | 1990-05-12 | alice@example.com |

<br>

> **SQL QUERY EXAMPLE:**

❓**Problem Statement**: From `people` table, retrieve all `names` stored inside.
```sql
SELECT
  name
FROM people;
```
<br>

> **TRY YOURSELF: [CLICK TO EXECUTE QUERIES](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/002%20PATRONS%20DATA.md)**

<br>

> **SQL QUERY TEST:**

❓**Problem Statement**: Retrieve `names` and `emails` from `people` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT
  name,
  email
FROM people;
```
</details>

---
## 💡TIP TO REMEMBER
> <b>ORDER OF EXECUTION:
```sql 
FROM → SELECT
```
- <b><i>FROM executes first</i></b>: SQL looks at which table to query.
- <b><i>SELECT executes second</i></b>: SQL picks the columns to return.
  
> **RETRIEVE ALL COLUMNS**
- Replace specific column names with the wildcard (*) in a SELECT statement to fetch all columns from the table.
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/002%20PATRONS%20DATA.md)

---
1. Retrieving 1 field from patrons table
<details>
  <summary>✅ Solution:</summary>

```sql
SELECT name
FROM patrons;
```
</details>

2. Retrieving 2 fields from patrons table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT card_id, name
FROM patrons;
```
</details>

3. Retrieving 3 fields from patrons table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT card_id, name, join_year
FROM patrons;
```
</details>

4. Retrieving multiple fields from patrons table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT card_id, name, join_year, fines
FROM patrons;
```
</details>

5. Retrieving ALL fields from patrons table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT *
FROM patrons;
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
