# 📊 DATA - BASE, WAREHOUSE, LAKE
---

## 🔑KEYWORDS

* **Database**
* **Data Warehouse**
* **Data Mart**
* **Data Lake**
* **Structured Data**
* **Unstructured Data**
* **ETL (Extract, Transform, Load)**

---

## 📖DEFINITION

* **Database** – Stores structured data (tables with rows & columns) for transactions.
* **Data Warehouse** – Centralized store for structured data from multiple sources, optimized for analysis.
* **Data Mart** – Subset of a data warehouse, focused on one department (e.g., Finance).
* **Data Lake** – Stores structured + unstructured data (text, images, video, logs), schema optional, highly flexible.

---

## 🧱QUERY FORMAT

Examples with the `movies` dataset:

```sql
-- Get all movies by Christopher Nolan
SELECT title, genre, release_year
FROM movies
WHERE director_id = 1;

-- Average ratings per director
SELECT d.director_name, AVG(r.rating) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name
ORDER BY avg_rating DESC;
```

---

## 💡TIP TO REMEMBER

* **Databases** = Transactions (fast inserts/updates).
* **Warehouses** = Enterprise-wide analytics.
* **Marts** = Department-specific subset.
* **Lakes** = Flexible, raw data storage (structured + unstructured).

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory:
👉 [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find all movies released after 2010 and their directors.
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, m.release_year, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year > 2010;
```
</details>

---

### 2. Get the top 3 highest-rated movies (average rating).
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

---

### 3. Which actor has acted in the most movies?
<details>
<summary>✅ Solution</summary>

```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY movie_count DESC
LIMIT 1;
```
</details>

---

### 4. Show average rating for each movie genre.
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.genre, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre
ORDER BY avg_rating DESC;
```
</details>

---

## 🧠PRACTISE

1. List all movies along with the number of actors in them.
2. Find all directors who directed more than 2 movies.
3. Show movies where Leonardo DiCaprio and Brad Pitt both acted together.
4. Get the latest movie (highest release year) for each director.
5. Find the top-rated movie for each actor.

---

## ✅SOLUTIONS

### 1. List all movies along with the number of actors in them.

```sql
SELECT m.title, COUNT(ma.actor_id) AS actor_count
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title
ORDER BY actor_count DESC;
```

### 2. Find all directors who directed more than 2 movies.

```sql
SELECT d.director_name, COUNT(m.movie_id) AS movie_count
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name
HAVING COUNT(m.movie_id) > 2;
```

### 3. Show movies where Leonardo DiCaprio and Brad Pitt both acted together.

```sql
SELECT m.title
FROM movies m
JOIN movie_actors ma1 ON m.movie_id = ma1.movie_id
JOIN movie_actors ma2 ON m.movie_id = ma2.movie_id
JOIN actors a1 ON ma1.actor_id = a1.actor_id
JOIN actors a2 ON ma2.actor_id = a2.actor_id
WHERE a1.actor_name = 'Leonardo DiCaprio'
  AND a2.actor_name = 'Brad Pitt';
```

### 4. Get the latest movie (highest release year) for each director.

```sql
SELECT d.director_name, MAX(m.release_year) AS latest_movie_year
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```

### 5. Find the top-rated movie for each actor.

```sql
SELECT a.actor_name, m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY a.actor_name, m.title
HAVING AVG(r.rating) = (
    SELECT MAX(sub.avg_rating)
    FROM (
        SELECT AVG(r2.rating) AS avg_rating
        FROM movie_actors ma2
        JOIN movie_ratings r2 ON ma2.movie_id = r2.movie_id
        WHERE ma2.actor_id = a.actor_id
        GROUP BY ma2.movie_id
    ) sub
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

---
