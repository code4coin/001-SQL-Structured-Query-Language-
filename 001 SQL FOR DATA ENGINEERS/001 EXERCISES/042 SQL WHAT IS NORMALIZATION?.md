# Normalization – 1NF, 2NF, 3NF (Movie DB)

---

## 🔑KEYWORDS

* Normalization, Normal Forms (1NF, 2NF, 3NF)
* Functional Dependency
* Composite Key
* Transitive Dependency
* Update/Insertion/Deletion Anomalies, Movies Schema

---

## 📖DEFINITION

* **Normalization** – A systematic process of structuring relational tables to reduce redundancy and dependency, improving data integrity. We focus on the first three normal forms:

  * **1NF:** Atomic values and unique rows.
  * **2NF:** Remove partial dependencies (all non-key attributes depend on the *whole* key when the primary key is composite).
  * **3NF:** Remove transitive dependencies (non-key attributes must not depend on other non-key attributes).

---

## 🧱QUERY FORMAT

Use these common SQL patterns on the provided **Movies** dataset:

```sql
-- 1) Basic JOIN to combine movie, director, actors
SELECT m.title, d.director_name, a.actor_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id;

-- 2) Aggregation with GROUP BY (e.g., average rating per movie)
SELECT m.title, ROUND(AVG(mr.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC;

-- 3) Filtering by attributes (e.g., Sci‑Fi movies by year)
SELECT title, release_year
FROM movies
WHERE genre = 'Sci-Fi' AND release_year >= 2010
ORDER BY release_year;
```

---

Based on the above content summarizing data and the tables below (use **only** these tables for all examples, exercises, and solutions):

```sql
CREATE TABLE movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    director_id INT,
    release_year INT,
    country VARCHAR(50),
    runtime_minutes INT
);

CREATE TABLE directors (
    director_id INT PRIMARY KEY,
    director_name VARCHAR(100),
    nationality VARCHAR(50)
);

CREATE TABLE actors (
    actor_id INT PRIMARY KEY,
    actor_name VARCHAR(100),
    nationality VARCHAR(50)
);

CREATE TABLE movie_actors (
    movie_id INT,
    actor_id INT,
    PRIMARY KEY (movie_id, actor_id),
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id),
    FOREIGN KEY (actor_id) REFERENCES actors(actor_id)
);

CREATE TABLE movie_ratings (
    rating_id INT PRIMARY KEY,
    movie_id INT,
    user_name VARCHAR(50),
    rating FLOAT,
    review_date DATE,
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id)
);
```

> **Note:** These tables are already in 3NF: actors and directors are separated from movies, and ratings are in their own table. This avoids partial/transitive dependencies and typical anomalies.

---

## 💡TIP TO REMEMBER

* **1NF =** no lists in a cell; every column holds a single value; rows are unique.
* **2NF =** if the primary key is composite, every non-key column must depend on **all** parts of that key.
* **3NF =** eliminate transitive dependencies; keep descriptive attributes in their own entity tables (e.g., `directors`, `actors`).
* Normalization reduces **update / insertion / deletion** anomalies.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: **[CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)**

### 1. List all actors for the movie **'Interstellar'** (checks 1NF usage via relationship table).

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT m.title, a.actor_name
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
WHERE m.title = 'Interstellar';
-- Expected rows (based on seed): Matt Damon, Leonardo DiCaprio
```
</details>

### 2. Find the **average rating** for each movie and list the **top 5**.

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT m.title, ROUND(AVG(mr.rating), 2) AS avg_rating, COUNT(*) AS num_reviews
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC, num_reviews DESC
LIMIT 5;
```
</details>

### 3. Show each **director** with their movies and the **average movie rating**.

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT d.director_name,
       m.title,
       ROUND(AVG(mr.rating), 2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY d.director_name, m.title
ORDER BY d.director_name, avg_rating DESC;
```
</details>

### 4. List movies that feature **more than one actor** (demonstrates many-to-many and aggregation).

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT m.title, COUNT(ma.actor_id) AS actor_count
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title
HAVING COUNT(ma.actor_id) > 1
ORDER BY actor_count DESC, m.title;
```
</details>

### 5. For each **genre**, show the **highest-rated movie** (by average rating) and its director.

<details>
  <summary>✅ Solution:</summary>

```sql
WITH avg_ratings AS (
  SELECT m.movie_id, m.title, m.genre, m.director_id,
         AVG(mr.rating) AS avg_rating
  FROM movies m
  LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
  GROUP BY m.movie_id, m.title, m.genre, m.director_id
), ranked AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY genre ORDER BY avg_rating DESC NULLS LAST, title) AS rn
  FROM avg_ratings
)
SELECT r.genre, r.title, ROUND(r.avg_rating, 2) AS avg_rating, d.director_name
FROM ranked r
JOIN directors d ON r.director_id = d.director_id
WHERE r.rn = 1
ORDER BY r.genre;
```
</details>

### 6. Find **actors** who have worked with **more than one director**.

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT a.actor_name, COUNT(DISTINCT d.director_id) AS directors_worked_with
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
GROUP BY a.actor_name
HAVING COUNT(DISTINCT d.director_id) > 1
ORDER BY directors_worked_with DESC, a.actor_name;
```
</details>

### 7. Show **Brad Pitt** movies with their **release year**, **director**, and **average rating**.

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT a.actor_name,
       m.title,
       m.release_year,
       d.director_name,
       ROUND(AVG(mr.rating), 2) AS avg_rating
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
WHERE a.actor_name = 'Brad Pitt'
GROUP BY a.actor_name, m.title, m.release_year, d.director_name
ORDER BY m.release_year;
```
</details>

### 8. List all **Sci‑Fi** movies released **after 2009** with runtime and their **director**.

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT m.title, m.release_year, m.runtime_minutes, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Sci-Fi' AND m.release_year > 2009
ORDER BY m.release_year, m.title;
```
</details>

### 9. For each **director**, count how many **distinct actors** appeared across their movies.

<details>
  <summary>✅ Solution:</summary>

```sql
SELECT d.director_name, COUNT(DISTINCT ma.actor_id) AS distinct_actors
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY d.director_name
ORDER BY distinct_actors DESC, d.director_name;
```
</details> 

### 10. Find the **most recent review** (by `review_date`) for each movie along with the rating.

<details>
  <summary>✅ Solution:</summary>

```sql
WITH latest AS (
  SELECT mr.movie_id,
         mr.rating,
         mr.review_date,
         ROW_NUMBER() OVER (PARTITION BY mr.movie_id ORDER BY mr.review_date DESC, mr.rating DESC) AS rn
  FROM movie_ratings mr
)
SELECT m.title, l.rating, l.review_date
FROM latest l
JOIN movies m ON l.movie_id = m.movie_id
WHERE l.rn = 1
ORDER BY l.review_date DESC, m.title;
```
</details>
---

## 🧠Practise

Add more reps! Write the queries yourself first, then compare with the **Solutions** section below.

1. List **all movies** directed by **Christopher Nolan** with their **average rating**.
2. Show **actors** who have appeared in **Sci‑Fi** movies, alphabetically.
3. For each **country**, list the **count of movies** and the **average runtime**.
4. Find movies that **Brad Pitt** and **Samuel L. Jackson** both acted in.
5. Show the **top 3 highest‑rated movies** (by average rating) released **before 2010**.
6. List directors who have at least **two movies** in the dataset and show their **best‑rated movie**.
7. Show movies with **no ratings** yet (if any in your environment).
8. For each **genre**, list the **number of distinct actors** who appeared in that genre.
9. List **Tom Hanks** movies with their **director** and **review count**.
10. Find the **earliest released movie** for each **director** with its title.

---

## ✅SOLUTIONS

### 1. (Practise Q1) Nolan movies with average rating

```sql
SELECT d.director_name, m.title, ROUND(AVG(mr.rating), 2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
WHERE d.director_name = 'Christopher Nolan'
GROUP BY d.director_name, m.title
ORDER BY avg_rating DESC NULLS LAST, m.title;
```

### 2. (Practise Q2) Actors who appeared in Sci‑Fi movies

```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
WHERE m.genre = 'Sci-Fi'
ORDER BY a.actor_name;
```

### 3. (Practise Q3) Movies count and average runtime per country

```sql
SELECT m.country,
       COUNT(*) AS movie_count,
       ROUND(AVG(m.runtime_minutes), 2) AS avg_runtime
FROM movies m
GROUP BY m.country
ORDER BY movie_count DESC, m.country;
```

### 4. (Practise Q4) Movies with both Brad Pitt and Samuel L. Jackson

```sql
SELECT m.title
FROM movies m
JOIN movie_actors ma1 ON m.movie_id = ma1.movie_id
JOIN actors a1 ON ma1.actor_id = a1.actor_id
JOIN movie_actors ma2 ON m.movie_id = ma2.movie_id
JOIN actors a2 ON ma2.actor_id = a2.actor_id
WHERE a1.actor_name = 'Brad Pitt'
  AND a2.actor_name = 'Samuel L. Jackson'
ORDER BY m.title;
```

### 5. (Practise Q5) Top 3 highest‑rated movies before 2010

```sql
SELECT m.title, m.release_year, ROUND(AVG(mr.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
WHERE m.release_year < 2010
GROUP BY m.title, m.release_year
ORDER BY avg_rating DESC, m.title
LIMIT 3;
```

### 6. (Practise Q6) Directors with ≥2 movies and their best‑rated movie

```sql
WITH avg_per_movie AS (
  SELECT m.movie_id, m.title, m.director_id, AVG(mr.rating) AS avg_rating
  FROM movies m
  LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
  GROUP BY m.movie_id, m.title, m.director_id
), two_plus AS (
  SELECT director_id
  FROM movies
  GROUP BY director_id
  HAVING COUNT(*) >= 2
), ranked AS (
  SELECT a.*, ROW_NUMBER() OVER (PARTITION BY a.director_id ORDER BY a.avg_rating DESC NULLS LAST, a.title) AS rn
  FROM avg_per_movie a
  WHERE a.director_id IN (SELECT director_id FROM two_plus)
)
SELECT d.director_name, r.title, ROUND(r.avg_rating, 2) AS avg_rating
FROM ranked r
JOIN directors d ON r.director_id = d.director_id
WHERE r.rn = 1
ORDER BY d.director_name;
```

### 7. (Practise Q7) Movies with no ratings yet

```sql
SELECT m.title
FROM movies m
LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
WHERE mr.movie_id IS NULL
ORDER BY m.title;
```

### 8. (Practise Q8) Distinct actors per genre

```sql
SELECT m.genre, COUNT(DISTINCT ma.actor_id) AS distinct_actors
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.genre
ORDER BY distinct_actors DESC, m.genre;
```

### 9. (Practise Q9) Tom Hanks movies with director and review count

```sql
SELECT a.actor_name, m.title, d.director_name, COUNT(mr.rating_id) AS review_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
LEFT JOIN movie_ratings mr ON m.movie_id = mr.movie_id
WHERE a.actor_name = 'Tom Hanks'
GROUP BY a.actor_name, m.title, d.director_name
ORDER BY m.title;
```

### 10. (Practise Q10) Earliest released movie for each director

```sql
WITH ranked AS (
  SELECT m.director_id, m.title, m.release_year,
         ROW_NUMBER() OVER (PARTITION BY m.director_id ORDER BY m.release_year, m.title) AS rn
  FROM movies m
)
SELECT d.director_name, r.title, r.release_year
FROM ranked r
JOIN directors d ON r.director_id = d.director_id
WHERE r.rn = 1
ORDER BY r.release_year, d.director_name;
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
