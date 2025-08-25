# Normalized vs Denormalized Databases

---

## 🔑KEYWORDS

* **Normalization**
* **Denormalization**
* **OLTP**
* **OLAP**
* **Star Schema**
* **Snowflake Schema**

---

## 📖DEFINITION

* **Normalized Database** – A database design where tables are divided to reduce redundancy and ensure consistency.
* **Denormalized Database** – A database design where tables are combined for faster query performance at the cost of redundancy.
* **OLTP** – Online Transaction Processing (write-intensive)
* **OLAP** – Online Analytical Processing (read-intensive)
* **Star Schema** – Denormalized structure
* **Snowflake Schema** – Normalized structure

---

## 🧱QUERY FORMAT

```sql
-- Normalized example
SELECT <columns>
FROM   fact_table
JOIN   dim_table1 ON fact_table.key = dim_table1.key
JOIN   dim_table2 ON fact_table.key = dim_table2.key
WHERE  <filters>;
```
```sql
-- Denormalized example
SELECT <columns>
FROM   wide_table
WHERE  <filters>;
```
---

## 🗂 Visual Schema Example

**Denormalized (Star Schema)**

TABLE: movies_star
| title     | genre  | director          | actor             | rating |
| --------- | ------ | ----------------- | ----------------- | ------ |
| Inception | Sci-Fi | Christopher Nolan | Leonardo DiCaprio | 9.0    |
| ...       | ...    | ...               | ...               | ...    |


**Normalized (Snowflake Schema)**

TABLES: directors, movies, actors, movie_actors, movie_rating
| directors    | movies        | actors    | movie_actors | movie_ratings |
| ------------ | ------------- | --------- | ------------ | -------------- |
| director_id  | movie_id      | actor_id  | movie_id     | rating_id      |
| name         | title         | name      | actor_id     | movie_id       |
| ...          | genre         | ...       | ...          | user_name      |
|              | director_id   |           |              | rating         |
|              | release_year  |           |              | ...            |


---

## 💡TIP TO REMEMBER

* **Normalization reduces redundancy** and ensures data integrity.
* **Denormalization improves read performance** for analytics-heavy queries.
* OLTP → prefers **normalized tables**.
* OLAP → prefers **denormalized tables**.
* More joins = slower queries but safer updates.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)


### 1. List all movies directed by Steven Spielberg with their genres.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, m.genre
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Steven Spielberg';
```
</details>

### 2. Find the average rating of all Action movies.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.genre = 'Action';
```
</details>

### 3. Get all actors who worked with Leonardo DiCaprio.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN movie_actors ma2 ON m.movie_id = ma2.movie_id
JOIN actors a2 ON ma2.actor_id = a2.actor_id
WHERE a2.actor_name = 'Leonardo DiCaprio'
  AND a.actor_name <> 'Leonardo DiCaprio';
```
</details>
    
### 4. List the top 3 highest-rated Sci-Fi movies with director names.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, d.director_name, AVG(r.rating) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.genre = 'Sci-Fi'
GROUP BY m.title, d.director_name
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

### 5. Count the number of movies per director.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT d.director_name, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```
</details>

### 6. List all movies released in 1993 along with director names.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year = 1993;
```
</details>

### 7. Find all actors who appeared in more than one movie.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
HAVING COUNT(ma.movie_id) > 1;
```
</details>

### 8. Get the total number of ratings per movie.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, COUNT(r.rating_id) AS total_ratings
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```
</details>

### 9. Find movies with average rating greater than 9.0.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
HAVING AVG(r.rating) > 9.0;
```
</details>

### 10. List movies along with all actors in a comma-separated format.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, STRING_AGG(a.actor_name, ', ') AS actors
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
GROUP BY m.title;
```

</details>

---

## 🧠Practise

1. List all Horror movies with directors.
2. Find all Sci-Fi movies released after 2010.
3. List all actors and the number of movies they acted in.
4. Get the highest-rated movie per genre.
5. Find all directors who have worked with Leonardo DiCaprio.
6. List all movies with their average ratings in descending order.
7. Show all movies and the count of actors in each movie.

### Solutions

1. **Horror movies with directors**

```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Horror';
```

2. **Sci-Fi movies released after 2010**

```sql
SELECT title, release_year
FROM movies
WHERE genre = 'Sci-Fi' AND release_year > 2010;
```

3. **Actors and number of movies**

```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS total_movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name;
```

4. **Highest-rated movie per genre**

```sql
SELECT m.genre, m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre, m.title
HAVING AVG(r.rating) = (
    SELECT MAX(avg_r)
    FROM (
        SELECT AVG(r2.rating) AS avg_r
        FROM movies m2
        JOIN movie_ratings r2 ON m2.movie_id = r2.movie_id
        WHERE m2.genre = m.genre
        GROUP BY m2.title
    ) AS sub
);
```

5. **Directors who worked with Leonardo DiCaprio**

```sql
SELECT DISTINCT d.director_name
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
WHERE a.actor_name = 'Leonardo DiCaprio';
```

6. **Movies with average rating descending**

```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC;
```

7. **Movies with count of actors**

```sql
SELECT m.title, COUNT(ma.actor_id) AS actor_count
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title;
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
