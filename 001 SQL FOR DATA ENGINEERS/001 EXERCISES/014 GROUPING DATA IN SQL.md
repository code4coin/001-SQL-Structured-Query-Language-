# GROUPING DATA IN SQL
---
## 🔑KEYWORDS
- **GROUP BY**
- **Aggregate functions (SUM, COUNT, AVG, MAX, MIN)**
- **Multiple field grouping**
- **GROUP BY with ORDER BY**
- **SQL order of execution**
---
## 📖DEFINITION
- **GROUP BY** – collapses rows into groups based on one or more columns
- **Aggregate Functions** – COUNT, SUM, AVG, MIN, MAX operate on each group
- **HAVING** – filters groups after aggregation
- **ORDER BY with GROUP BY** – sorts the aggregated summary
- **Execution Order** – FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT

---
## 🧱QUERY FORMAT

```text
SELECT grouping_column(s),
       aggregate_function(other_column) AS alias_name
FROM   table_name
[WHERE  non-aggregated_filter]
GROUP  BY grouping_column(s)
[HAVING aggregated_filter]
[ORDER  BY alias_name [ASC|DESC]]
[LIMIT  n];
```
---
## 💡TIP TO REMEMBER
- **“GROUP first, then SELECT.”**
- Columns in the SELECT list must either be grouped or aggregated.
- The order of columns in GROUP BY affects the grouping hierarchy.
- You can use aliases from aggregate functions in the ORDER BY clause. Due to order of execution
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

### 1. Count the number of movies per director.
```sql
SELECT director, COUNT(*) AS total_movies
FROM movies
GROUP BY director;
```
### 2. Find the average rating for each country.
```sql
SELECT country, AVG(rating) AS avg_rating
FROM movies
GROUP BY country;
```
### 3. Count the number of movies by genre and release year.
```sql
SELECT genre, release_year, COUNT(*) AS total_movies
FROM movies
GROUP BY genre, release_year;
```
### 4. Group by genre and order by the average runtime descending.
```sql
SELECT genre, AVG(runtime_minutes) AS avg_runtime
FROM movies
GROUP BY genre
ORDER BY avg_runtime DESC;
```
### 5. Find the total runtime of movies for each genre-country combination.
```sql
SELECT genre, country, SUM(runtime_minutes) AS total_runtime
FROM movies
GROUP BY genre, country;
```
---
## 🧠Practise
1. Show each genre together with its average runtime and number of movies, ordered from most movies to least.
2. List every director and how many distinct titles they have directed, but only include directors with at least 2 movies.
3. Country Ratings For each country, return the highest rating achieved and the earliest release year. Sort by highest rating (descending) and then by earliest year (ascending).
---
## ✅SOLUTIONS
### 1. Show each genre together with its average runtime and number of movies, ordered from most movies to least.
```sql
SELECT genre,
       AVG(runtime_minutes) AS avg_runtime,
       COUNT(*)             AS movie_count
FROM   movies
GROUP  BY genre
ORDER  BY movie_count DESC;
```
### 2. List every director and how many distinct titles they have directed.
```sql
SELECT director,
       COUNT(DISTINCT title) AS total_movies
FROM   movies
GROUP  BY director
ORDER  BY total_movies DESC;
```
### 3. Country Ratings For each country, return the highest rating achieved and the earliest release year. Sort by highest rating (descending) and then by earliest year (ascending).
```sql
SELECT country,
       MAX(rating)        AS highest_rating,
       MIN(release_year)  AS earliest_year
FROM   movies
GROUP  BY country
ORDER  BY highest_rating DESC,
          earliest_year   ASC;
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
