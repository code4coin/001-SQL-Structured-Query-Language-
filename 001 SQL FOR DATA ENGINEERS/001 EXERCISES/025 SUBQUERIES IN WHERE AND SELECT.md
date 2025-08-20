# SUBQUERIES IN WHERE AND SELECT

---
## 🔑KEYWORDS
* **subquery**
* **WHERE clause**
* **SELECT clause**
* **Scalar**
* **Correlated** 
---

## 📖DEFINITION  
- **Subquery** – A query nested inside another SQL query to filter or compute values.
- **WHERE sub-query** – a SELECT nested in the WHERE clause that feeds a dynamic list (or existence test) to IN / NOT IN / EXISTS.  
- **SELECT sub-query** – a scalar SELECT nested inside the SELECT list; it runs once per outer row and must return exactly one value.
- **Scalar** – single value per row  
- **Correlated** – references outer query columns  
- **Alias** – mandatory for SELECT-list sub-queries
---

## 🧱QUERY FORMAT

```sql
-- Subquery in WHERE
SELECT column_list
FROM table_name
WHERE column_name IN (
    SELECT sub_column
    FROM other_table
);
```

```sql
-- Subquery in SELECT
SELECT column_list,
       (SELECT aggregate_function
        FROM other_table
        WHERE condition) AS alias_name
FROM table_name;
```
---

## 💡TIP TO REMEMBER
- WHERE sub-queries filter; SELECT sub-queries enrich.
- In SELECT to compute values dynamically per row (ensure to get scalar values from the subquery).
- Always alias a SELECT-list sub-query; Index the join key to avoid row-by-row pain.
- Use subqueries in WHERE to filter with values derived from another table, and

--
## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. List all movie titles directed by directors from the USA using a subquery in WHERE.
```sql
SELECT title
FROM movies
WHERE director_id IN (
    SELECT director_id
    FROM directors
    WHERE nationality = 'USA'
);
```

### 2. Find all actors who acted in movies directed by 'Christopher Nolan'.
```sql
SELECT actor_name
FROM actors
WHERE actor_id IN (
    SELECT actor_id
    FROM movie_actors
    WHERE movie_id IN (
        SELECT movie_id
        FROM movies
        WHERE director_id = (
            SELECT director_id
            FROM directors
            WHERE director_name = 'Christopher Nolan'
        )
    )
);
```

### 3. Retrieve the average rating for each movie using a subquery inside SELECT.
```sql
SELECT title,
       (SELECT AVG(rating)
        FROM movie_ratings
        WHERE movie_ratings.movie_id = movies.movie_id) AS avg_rating
FROM movies;
```

### 4. List movie titles with ratings higher than 9.
```sql
SELECT title
FROM movies
WHERE movie_id IN (
    SELECT movie_id
    FROM movie_ratings
    GROUP BY movie_id
    HAVING AVG(rating) > 9
);
```

---

## 🧠Practise

1. Count the number of movies for each director using a subquery inside SELECT.
2. Show all movies along with the number of actors in each movie.
3. Find all directors who have directed movies with average rating above 9.

---

## ✅SOLUTIONS

### 1. Count the number of movies for each director using a subquery inside SELECT.

```sql
SELECT director_name,
       (SELECT COUNT(*)
        FROM movies
        WHERE movies.director_id = directors.director_id) AS movie_count
FROM directors;
```

### 2. Show all movies along with the number of actors in each movie.

```sql
SELECT title,
       (SELECT COUNT(*)
        FROM movie_actors
        WHERE movie_actors.movie_id = movies.movie_id) AS actor_count
FROM movies;
```

### 3. Find all directors who have directed movies with average rating above 9.

```sql
SELECT director_name
FROM directors
WHERE director_id IN (
    SELECT director_id
    FROM movies
    WHERE movie_id IN (
        SELECT movie_id
        FROM movie_ratings
        GROUP BY movie_id
        HAVING AVG(rating) > 9
    )
);
```
---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
