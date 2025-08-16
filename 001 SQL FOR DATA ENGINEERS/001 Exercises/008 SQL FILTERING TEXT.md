# SQL FILTERING TEXT
---
## 🔑KEYWORDS
- **LIKE**
- **NOT LIKE**
- **IN**
- **WILDCARD (%)**
- **WILDCARD (_)**
- **PATTERN MATCHING**
---
## 📖DEFINITION
- **Filtering Text** – Use SQL to select records based on specific text values or patterns in columns. Ideal for searching names, titles, or other string data.  
- **LIKE** – Find values that match a specific pattern in a column. Useful for partial matches, e.g., names starting with “J”.  
- **NOT LIKE** – Exclude values that match a specific pattern in a column. Helps filter out unwanted text results.  
- **IN** – Check if a column’s value exists within a specified list of values. Simplifies multiple OR conditions into a concise query.
---
## 🧱QUERY FORMAT
```sql
-- 1. LIKE query format
  SELECT column_names
    FROM table_name
   WHERE column_name LIKE pattern_detail; 
```
```sql
-- 2. NOT LIKE query format
  SELECT column_names
    FROM table_name
   WHERE column_name NOT LIKE pattern_detail; 
```
```sql
-- 3. IN query format
  SELECT column_names
    FROM table_name
   WHERE column_name IN (list_of_numbers or list_of_text); 
```
---
## 💡TIP TO REMEMBER
- Use **`%`** to match zero, one, or many characters in a text.
- Use **`_`** to match a single character in a text.
- **NOT LIKE** is case-sensitive.
- **IN** is cleaner than multiple OR conditions for multiple values.
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

### 1. Find movies with titles ending in 'man'
```sql
SELECT * FROM movies
WHERE title LIKE '%man';
```
### 2.Find movies NOT from 'USA'
```sql
SELECT * FROM movies
WHERE country NOT LIKE 'USA';
```
### 3. Find movies from Germany or France
```sql
SELECT * FROM movies
WHERE country IN ('Germany', 'France');

```
---
## 🧠Practise
1. Find movies whose titles start with 'A'.
2. Find movies whose titles have 't' as the second character.
3. Exclude movies from 'Canada' or 'Mexico'.
---
## ✅SOLUTIONS
### 1. Find movies whose titles start with 'A'.
```sql
SELECT *
  FROM movies
 WHERE title LIKE 'A%';
```
### 2. Find movies whose titles have 't' as the second character.
```sql
SELECT *
  FROM movies
 WHERE title LIKE '_t%';
```
### 3. Exclude movies from 'Canada' or 'Mexico'.
```sql
SELECT *
  FROM movies
 WHERE country NOT IN ('Canada', 'Mexico');
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
