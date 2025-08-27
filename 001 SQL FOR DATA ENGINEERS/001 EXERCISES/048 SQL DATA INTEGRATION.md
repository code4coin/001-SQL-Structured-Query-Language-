# Data Integration

---

## 🔑KEYWORDS

* **Data Integration** 
* **Unified Data Model**
* **ETL**
* **Transformation**
* **Lineage** 
* **Update Cadence** 

---

## 📖DEFINITION

* **Data Integration** – The process of combining data from different sources, formats, or technologies to provide a consistent and unified view for users and applications.
* **Unified Data Model** – A consolidated data structure for analysis and reporting.
* **ETL** – Extract, Transform, Load.
* **Transformation** – Converting source data to fit the unified model.
* **Lineage** – Tracking data origin and usage.
* **Update Cadence** – Frequency of data updates.

---

## 🧱QUERY FORMAT

Basic SQL format to query integrated data:

```sql
SELECT columns
FROM source_table
JOIN another_table ON condition
WHERE conditions
GROUP BY columns
ORDER BY columns;
```

---

## 💡TIP TO REMEMBER

* Always define your **final goal** before integrating data (dashboards, reports, or products).
* Keep track of **update cadence** for each source.
* Ensure **security** and **lineage** are maintained during ETL.
* Use **automated testing** to verify transformations.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Create a unified view of movies with their directors, actors, and average rating.

<details>
  <summary>✅ Solution:</summary>
  
  ```sql
  SELECT
      m.title,
      d.director_name,
      STRING_AGG(a.actor_name, ', ') AS actors,
      AVG(r.rating) AS avg_rating
  FROM movies m
  JOIN directors d ON m.director_id = d.director_id
  JOIN movie_actors ma ON m.movie_id = ma.movie_id
  JOIN actors a ON ma.actor_id = a.actor_id
  LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
  GROUP BY m.movie_id, d.director_name, m.title;
  ```
</details>

### 2. List all movies released after 2010 with ratings above 9.

<details>
  <summary>✅ Solution:</summary>
  
  ```sql
  SELECT m.title, AVG(r.rating) AS avg_rating
  FROM movies m
  JOIN movie_ratings r ON m.movie_id = r.movie_id
  WHERE m.release_year > 2010
  GROUP BY m.title
  HAVING AVG(r.rating) > 9;
  ```
</details>

### 3. Find all actors who have worked with Christopher Nolan.

<details>
  <summary>✅ Solution:</summary>
  
  ```sql
  SELECT DISTINCT a.actor_name
  FROM actors a
  JOIN movie_actors ma ON a.actor_id = ma.actor_id
  JOIN movies m ON ma.movie_id = m.movie_id
  JOIN directors d ON m.director_id = d.director_id
  WHERE d.director_name = 'Christopher Nolan';
  ```
</details>

### 4. Get the top 3 highest-rated movies.

<details>
  <summary>✅ Solution:</summary>
  
  ```sql
  SELECT m.title, AVG(r.rating) AS avg_rating
  FROM movies m
  JOIN movie_ratings r ON m.movie_id = r.movie_id
  GROUP BY m.title
  ORDER BY avg_rating DESC
  LIMIT 3;
  ```
</details>

### 5. Count the number of movies per genre.

<details>
  <summary>✅ Solution:</summary>
  
  ```sql
  SELECT genre, COUNT(*) AS total_movies
  FROM movies
  GROUP BY genre;
  ```
</details>

---

## 🧠Practise

1. Find all movies and their director nationality.
2. List movies with more than one actor.
3. Retrieve the average rating per director.
4. Find actors who acted in movies rated above 9.
5. List all movies along with the number of actors in each movie.
6. Retrieve all movies that have no ratings yet.
7. Find the earliest released movie per genre.
8. Get the total number of ratings per user.

---

## ✅SOLUTIONS

### Practice Solutions

1. **Find all movies and their director nationality:**

```sql
SELECT m.title, d.nationality AS director_nationality
FROM movies m
JOIN directors d ON m.director_id = d.director_id;
```

2. **List movies with more than one actor:**

```sql
SELECT m.title, COUNT(ma.actor_id) AS actor_count
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title
HAVING COUNT(ma.actor_id) > 1;
```

3. **Retrieve the average rating per director:**

```sql
SELECT d.director_name, AVG(r.rating) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name;
```

4. **Find actors who acted in movies rated above 9:**

```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movie_ratings r ON ma.movie_id = r.movie_id
WHERE r.rating > 9;
```

5. **List all movies along with the number of actors in each movie:**

```sql
SELECT m.title, COUNT(ma.actor_id) AS actor_count
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title;
```

6. **Retrieve all movies that have no ratings yet:**

```sql
SELECT m.title
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE r.rating IS NULL;
```

7. **Find the earliest released movie per genre:**

```sql
SELECT genre, MIN(release_year) AS earliest_year
FROM movies
GROUP BY genre;
```

8. **Get the total number of ratings per user:**

```sql
SELECT user_name, COUNT(*) AS total_ratings
FROM movie_ratings
GROUP BY user_name;
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
