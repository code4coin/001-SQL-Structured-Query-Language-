# 011 SUMMARIZING SUBSETS WITH FILTERS
---
## 🔑KEYWORDS
- **Aggregate Functions** – `SUM()`, `AVG()`, `MIN()`, `MAX()`, `COUNT()`
- **Summarizing Subset Data**
- **WHERE Clause**
- **ROUND()**
---
## 📖DEFINITION
- **Summarizing Subset Data** – Use SQL aggregate functions to calculate totals, averages, minimums, maximums, and counts for a subset/ portion of records.  
- **WHERE Clause** – Filters rows before aggregation to focus on a specific subset.  
- **ROUND()** – Rounds numerical results to a specified number of decimal places.
---
## 🧱QUERY FORMAT
```sql
SELECT AGG_FUNC(column) AS alias
FROM   movies
WHERE  your_condition;
```
---
## 💡TIP TO REMEMBER
- Filter **first**, crunch **after**: `WHERE` runs before any `AVG`, `SUM`, `MIN`, `MAX` or `COUNT`.
- WHERE filters rows before aggregation.
- COUNT(column_name) counts only non-NULL values.
- ROUND(number, decimals) can round right (positive decimals) or left (negative decimals) of the decimal point.
- Use rounding for currency, ratings, or runtime totals to clean up results.
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/002%20PATRONS%20DATA.md)

### 1. Average rating of films released in 2010 or later
```sql
SELECT AVG(rating) AS avg_rating
FROM   movies
WHERE  release_year >= 2010;
```
### 2. Total runtime (minutes) of all Animation movies
```sql
SELECT SUM(runtime_minutes) AS total_anim_min
FROM   movies
WHERE  genre = 'Animation';
```
### 3. Lowest and highest rating in the Thriller genre
```sql
SELECT MIN(rating) AS min_thrill,
       MAX(rating) AS max_thrill
FROM   movies
WHERE  genre = 'Thriller';
```
### 4. Count how many Science Fiction titles exist
```sql
SELECT COUNT(*) AS sci_fi_count
FROM   movies
WHERE  genre = 'Science Fiction';
```
### 5. Average rating of USA-made movies, rounded to one decimal place
```sql
SELECT ROUND(AVG(rating), 1) AS usa_avg
FROM   movies
WHERE  country = 'USA';
```
### 6. Sum of runtime for every film longer than 120 minutes
```sql
SELECT SUM(runtime_minutes) AS long_runtime_total
FROM   movies
WHERE  runtime_minutes > 120;
```
### 7. Count how many Japanese movies are listed
```sql
SELECT COUNT(*) AS jp_count
FROM   movies
WHERE  country = 'Japan';
```

## 🧠Practise
1. Average rating rounded to 1 decimal for USA movies.
2. Sum of runtime for films longer than 120 min.
3. Count how many Japanese movies exist.
---
## ✅SOLUTIONS
### 1. USA average (1-decimal)
```sql
SELECT ROUND(AVG(rating), 1) AS usa_avg
FROM   movies
WHERE  country = 'USA';
```
### 2. Total runtime > 120 min
```sql
SELECT SUM(runtime_minutes) AS long_runtime_total
FROM   movies
WHERE  runtime_minutes > 120;
```
### 3. Count Japanese films
```sql
SELECT COUNT(*) AS jp_count
FROM   movies
WHERE  country = 'Japan';
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
