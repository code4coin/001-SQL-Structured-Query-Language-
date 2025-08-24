# SQL - SELECT AND FROM
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
> SQL QUERY: TO RETRIEVE DATA FROM A DATABASE TABLE
```sql
SELECT field_name
FROM table_name;
```
---
## 💡TIP TO REMEMBER
> **ORDER OF EXECUTION:**
```sql 
FROM → SELECT
```
- <b>FROM *executes first*</b>: SQL looks at which table to query.
- <b>SELECT *executes second*</b>: SQL picks the columns to return.

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/002%20PATRONS%20DATA.md)

---
### 1. Retrieving 1 field from patrons table
✅ Solution:
```sql
SELECT name
FROM patrons;
```
### 2. Retrieving 2 fields from patrons table
✅ Solution:
```sql
SELECT card_id, name
FROM patrons;
```
### 3. Retrieving 3 fields from patrons table
✅ Solution:
```sql
SELECT card_id, name, join_year
FROM patrons;
```
### 4. Retrieving multiple fields from patrons table
✅ Solution:
```sql
SELECT card_id, name, join_year, fines
FROM patrons;
```
### 5. Retrieving ALL fields from patrons table
✅ Solution:
```sql
SELECT *
FROM patrons;
```
---
## 🧠Practise
1. Write a query to retrieve field `book_title` from `checkouts` table
2. Write a query to retrieve all fields from `checkouts` table, using `wildcard (*)`
---
## ✅ SOLUTIONS
### 1. Write a query to retrieve field `book_title` from `checkouts` table
```sql
SELECT book_title
FROM checkouts;
```
### 2. Write a query to retrieve all fields from `checkouts` table, using `wildcard (*)`
```sql
SELECT *
FROM checkouts;
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
