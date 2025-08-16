# MULTIPLE CRITERIA
---
## 🔑KEYWORDS
- **OR**
- **AND**
- **BETWEEN**
---
## 📖DEFINITION
- **OR** – returns rows that match at least one of the specified conditions
- **AND** – returns rows that match all of the specified conditions
- **BETWEEN** – returns rows where a value falls within a specified range (inclusive)
---
## 🧱QUERY FORMAT
```sql
-- 1. SQL for OR clause
SELECT column_names
FROM table_names
WHERE (filter_condtion_01
   OR filter_condtion_02);
```
```sql
-- 2. SQL for AND clause
SELECT column_names
FROM table_names
WHERE (filter_condtion_01
   AND filter_condtion_02);
```
```sql
-- 3. SQL for OR clause
SELECT column_names
FROM table_names
WHERE column_name BETWEEN number_01 AND number_02;
```
---
## 💡Tip TO REMEMBER

Think of it like interviewing employees:
- **OR** = “I'll accept *this* OR *that* candidate.”
- **AND** = “The candidate must have *this* AND *that* skill.”
- **BETWEEN** = “I want someone with *between X and Y* years of experience.”
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

### 1. Return employees who are in **department 101 OR 104**
```sql
SELECT *
FROM employees
WHERE dept_id = 101
   OR dept_id = 104;
```
### 2. Return employees who are in department 101 AND hired after 2019
```sql
SELECT *
FROM employees
WHERE dept_id = 101
  AND year_hired > 2019;
```
### 3. Return employees whose age is between 28 and 33
```sql
SELECT *
FROM employees
WHERE age BETWEEN 28 AND 33;
```
### 4. Return employees who are **in department 101 OR 102**, **AND** were **hired after 2019**.

```sql
SELECT *
FROM employees
WHERE (dept_id = 101 OR dept_id = 102)
  AND year_hired > 2019;
```
### 5. Return employees who are in department 103 OR 104, AND have an age between 30 and 35.
```sql
SELECT *
FROM employees
WHERE (dept_id = 103 OR dept_id = 104)
  AND age BETWEEN 30 AND 35;
```
---
## 🧠Practise
1. Return all books that are in **department 101 OR 103**.
2. Return all books that are in **department 102 AND borrowed after '2025-08-03'**.
3. Return all books borrowed **between '2025-08-02' and '2025-08-04'**.
4. Return all books that are in **department 102 OR 105**, **AND** have a **return date before '2025-08-18'**.
5. Return all books that are **in department 101 OR 104**, **AND** borrowed **between '2025-08-01' and '2025-08-03'**.
---
## ✅SOLUTIONS
### 1. Return all books that are in **department 101 OR 103**.
```sql
SELECT * 
FROM books
WHERE dept_id = 101 OR dept_id = 103;
```
### 2. Return all books that are in **department 102 AND borrowed after '2025-08-03'**.
   ```sql
   SELECT * 
   FROM books
   WHERE dept_id = 102 AND borrow_date > '2025-08-03';
   ```
### 3. Return all books borrowed **between '2025-08-02' and '2025-08-04'**.
 ```sql
  SELECT * 
  FROM books
  WHERE borrow_date BETWEEN '2025-08-02' AND '2025-08-04';
  ```
### 4. Return all books that are in **department 102 OR 105**, **AND** have a **return date before '2025-08-18'**.
  ```sql
    SELECT * 
    FROM books
    WHERE (dept_id = 102 OR dept_id = 105) 
      AND return_date < '2025-08-18';
  ```
### 5. Return all books that are **in department 101 OR 104**, **AND** borrowed **between '2025-08-01' and '2025-08-03'**.
  ```sql
    SELECT * 
    FROM books
    WHERE (dept_id = 101 OR dept_id = 104) 
      AND borrow_date BETWEEN '2025-08-01' AND '2025-08-03';
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
