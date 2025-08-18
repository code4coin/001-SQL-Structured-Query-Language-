# Filtering Grouped Data in SQL
---

## 🔑KEYWORDS
- **GROUP BY** 
- **HAVING** 
- **WHERE**
- **Aggregate Functions** 
---

## 📖DEFINITION
- **GROUP BY** – Allows grouping of rows by one or more columns to calculate aggregate values per group.
- **HAVING** – Filters aggregated results after GROUP BY; useful when filtering based on aggregates.
- **WHERE** – Filters raw data rows before any grouping occurs.
- **WHERE vs HAVING** – WHERE filters rows; HAVING filters groups
- **Aggregate functions** – `COUNT`, `AVG`, `SUM`, `MIN`, `MAX`
- **Execution order** – `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`
---

## 🧱QUERY FORMAT
```sql
SELECT grouping_column(s),
       aggregate_function(column) AS alias_name
FROM   table_name
[WHERE  row_filter]                 -- runs before grouping
GROUP  BY grouping_column(s)
HAVING aggregate_filter_condition   -- runs after grouping
[ORDER  BY alias_name [ASC|DESC]]
[LIMIT  n];
```
---
## 💡TIP TO REMEMBER
- **“WHERE trims rows; HAVING trims groups. Use WHERE for raw rows, HAVING for aggregated rows.”**
- Always place HAVING after GROUP BY in queries.
- Aliases can be used in ORDER BY, but not in HAVING.
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

### 1. Find the average runtime of movies for each genre, only including genres where the average runtime is greater than 100 minutes.
```sql
SELECT genre, AVG(runtime_minutes) AS avg_runtime
FROM movies
GROUP BY genre
HAVING AVG(runtime_minutes) > 100
ORDER BY avg_runtime DESC;
```
### 2. List the total number of movies directed by each director for movies released after 2015. Include only directors with more than 3 movies.
```sql
SELECT director, COUNT(title) AS total_movies
FROM movies
WHERE release_year > 2015
GROUP BY director
HAVING COUNT(title) > 3
ORDER BY total_movies DESC;
```
### 3. Retrieve the number of movies per country with a rating higher than 8. Filter to show only countries with at least 2 such movies.
```sql
SELECT country, COUNT(title) AS high_rating_movies
FROM movies
WHERE rating > 8
GROUP BY country
HAVING COUNT(title) >= 2
ORDER BY high_rating_movies DESC;
```
---
## 🧠Practise
1. Show only directors who have made more than one movie. Display director name and total titles.
2. List release years whose average rating is ≥ 8.5, sorted by average rating descending.
3. Return every country whose average runtime is > 140 minutes. Also show the number of movies contributing to that average.
4. Show genres with fewer than 3 movies and display their average rating. 
---
## ✅SOLUTIONS


### 1. Show only directors who have made more than one movie. Display director name and total titles.
```sql
SELECT director,
       COUNT(*) AS movie_cnt
FROM   movies
GROUP  BY director
HAVING COUNT(*) > 1
ORDER  BY movie_cnt DESC;
```
### 2. List release years whose average rating is ≥ 8.5, sorted by average rating descending.
```sql
SELECT release_year,
       AVG(rating) AS avg_rating
FROM   movies
GROUP  BY release_year
HAVING AVG(rating) >= 8.5
ORDER  BY avg_rating DESC;
```
### 3. Return every country whose average runtime is > 140 minutes. Also show the number of movies contributing to that average.
```sql
SELECT country,
       AVG(runtime_minutes) AS avg_runtime,
       COUNT(*)            AS movie_cnt
FROM   movies
GROUP  BY country
HAVING AVG(runtime_minutes) > 140;
```
### 4. Show genres with fewer than 3 movies and display their average rating.
```sql
SELECT genre,
       COUNT(*)  AS movie_cnt,
       AVG(rating) AS avg_rating
FROM   movies
GROUP  BY genre
HAVING COUNT(*) < 3
ORDER  BY genre;
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
