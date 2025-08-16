# SQL FILTERING NULL VALUES
---
## 🔑KEYWORDS
- **NULL**
- **IS NULL**
- **IS NOT NULL**
---
## 📖DEFINITION
- **NULL** – Represents a missing or unknown value in a database. Common in real-world data due to human error or unavailable information.  
- **IS NULL** – Filters and selects rows where a column contains NULL values. Useful for identifying missing data.  
- **IS NOT NULL** – Filters and selects rows where a column does not contain NULL values. Useful for excluding missing data from analysis.
---
## 🧱QUERY FORMAT
```sql
-- 1. IS NULL query format
SELECT column_names
  FROM table_name
 WHERE column_name IS NULL;
```
```sql
-- 2. IS NOT NULL query format
SELECT column_names
  FROM table_name
 WHERE column_name IS NOT NULL;
```
---
## 💡TIP TO REMEMBER
- NULL is not the same as 0 or an empty string; it represents missing information.
- Use IS NULL to check for missing values.
- Use IS NOT NULL to focus on records with valid data.
- Counting NULL vs non-NULL values helps ensure accurate analysis.
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)
### 1. Find movies with unknown rating
```sql
SELECT * FROM movies
WHERE rating IS NULL;
```
### 2. Find movies with a known rating
```sql
SELECT * FROM movies
WHERE rating IS NOT NULL;
```
---
## 🧠Practise
1. Count how many movies have no rating.
2. Count how many movies have a rating.
3. List movies with missing director names.
4. List movies with a release year.
---
## ✅SOLUTIONS
### 1. Count how many movies have no rating.
```sql
SELECT COUNT(*) 
  FROM movies
 WHERE rating IS NULL;
```
### 2. Count how many movies have a rating.
```sql
SELECT COUNT(*) 
  FROM movies
 WHERE rating IS NOT NULL;
```
### 3. List movies with missing director names.
```sql
SELECT * 
  FROM movies
 WHERE director IS NULL;
```
### 4. List movies with a release year.
```sql
SELECT * 
  FROM movies
 WHERE release_year IS NOT NULL;
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
