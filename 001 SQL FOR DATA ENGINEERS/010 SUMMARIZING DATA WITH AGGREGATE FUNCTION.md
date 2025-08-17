# SUMMARIZING DATA WITH AGGREGATE FUNCTION
---

## 🔑KEYWORDS  
- **COUNT()**
- **AVG()**
- **SUM()**
- **MIN()**
- **MAX()**
- **Aggregate Function**
- **Alias**

---

## 📖DEFINITION  
- **Summarizing data** – turning a whole column into a single, meaningful value instead of scrolling through every row.  
- **Aggregate functions** work on the **column**, not the individual rows, and always return **one row** of output.
- **Alias** – a short nickname (`AS avg_rating`) so results are easy to read. 

---

## 🧱QUERY FORMAT  
```sql
SELECT AGG_FUNC(column_name) AS alias_name
FROM   table_name;
```
---
## 💡TIP TO REMEMBER
- “AVG, SUM, MIN, MAX, COUNT — they all ignore NULLs, so totals stay clean.”
- Aggregate functions return **one single value**, even if the table has thousands of rows.
- Use them when you want to answer questions like **“what is the average…?”** or **“how many…?”**
- `COUNT`, `MIN` and `MAX` work on **text and numeric** fields.
- `AVG` and `SUM` work **only** on numeric fields.
- Always add an **alias** using `AS` to make the result easier to read.
---
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)
### 1. Average movie rating  
```sql
SELECT AVG(rating) AS avg_rating
FROM   movies;
```
### 2. Total runtime (minutes) of all films
```sql
SELECT SUM(runtime_minutes) AS total_minutes
FROM   movies;
```
### 3. Earliest & latest release year
```sql
SELECT MIN(release_year) AS earliest_year,
       MAX(release_year) AS latest_year
FROM   movies;
```
### 4. Count how many movies are listed
```sql
SELECT COUNT(*) AS total_movies
FROM   movies;
```
---
## 🧠Practise
1. What is the total runtime of all movies?
2. What is the earliest release year in the table?
3. What is the highest rating in the table?
4. How many movies are directed by 'Christopher Nolan'?
5. What is the alphabetically first movie title?
---
## ✅SOLUTIONS
### 1. What is the total runtime of all movies?
```sql
SELECT SUM(runtime_minutes) AS total_runtime
FROM movies;
```
### 2. What is the earliest release year in the table?
```sql
SELECT MIN(release_year) AS earliest_release_year
FROM movies;
```
### 3. What is the highest rating in the table?
```sql
SELECT MAX(rating) AS higest_rating
FROM movies;
```
### 4. How many movies are directed by 'Christopher Nolan'?
```sql
SELECT COUNT(*) AS count_movies_by_christopher
FROM movies
WHERE director = 'Christopher Nolan';
```
### 5. What is the alphabetically first movie title?
```sql
SELECT MIN(title) AS first_title_alphabetically
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



