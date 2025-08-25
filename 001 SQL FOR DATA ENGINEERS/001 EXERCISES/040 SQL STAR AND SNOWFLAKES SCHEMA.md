# STAR AND SNOWFLAKES SCHEMA

---

## 🔑KEYWORDS

* **Star Schema**
* **Snowflake Schema**
* **Fact Table**
* **Dimension Table**
* **Normalization**
* **One-to-Many Relationship**
* **Many-to-Many Relationship**
* **Data Integrity**

---

## 📖DEFINITION

* **Star Schema** – A dimensional model where a central fact table is connected to denormalized dimension tables, forming a star-like structure.
* **Snowflake Schema** – An extension of the star schema where dimension tables are normalized into multiple related tables, reducing redundancy and increasing data integrity.
* **Fact Table** – Contains measurable metrics (e.g., `movie_ratings.rating`).
* **Dimension Table** – Provides descriptive attributes (e.g., `movies`, `directors`, `actors`).
* **Normalization** – The process of splitting tables to remove redundancy and improve data integrity.

---

## 🧱QUERY FORMAT

* **Star Schema:** Denormalized dimension tables directly linked to the fact table.
* **Snowflake Schema:** Normalized dimension tables with separate tables for repeating groups, linked via foreign keys.

---

## 💡TIP TO REMEMBER

* Star schema is easier to query, but snowflake schema is better for data integrity and large datasets.
* Fact tables should only contain metrics and foreign keys to dimensions.
* Use junction tables (`movie_actors`) for many-to-many relationships.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### Exercise 1: List all movies with their director names.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id;
```
</details>

### Exercise 2: List all movies along with their average rating.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```
</details>

### Exercise 3: Find all actors who acted in more than 2 movies.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
HAVING COUNT(ma.movie_id) > 2;
```
</details>

### Exercise 4: Get all movies released after 2010 along with director nationality.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, d.nationality
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year > 2010;
```
</details>

### Exercise 5: List the top 3 highest-rated movies.
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

### Exercise 6: Show all actors with nationality and number of movies they acted in.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT a.actor_name, a.nationality, COUNT(ma.movie_id) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name, a.nationality;
```
</details>

### Exercise 7: Find movies with no ratings.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE r.rating_id IS NULL;
```
</details>

### Exercise 8: Find average rating for each genre.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.genre, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre;
```
</details>

### Exercise 9: List movies with multiple actors.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, COUNT(ma.actor_id) AS actor_count
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title
HAVING COUNT(ma.actor_id) > 1;
```
</details>

### Exercise 10: Find all ratings given in January 2024.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT m.title, r.user_name, r.rating, r.review_date
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
WHERE r.review_date BETWEEN '2024-01-01' AND '2024-01-31';
```
</details>

---

## 🧠PRACTICE QUESTIONS

1. Retrieve all Sci-Fi movies along with their ratings.
2. Count how many movies each director has directed.
3. List all actors in "Inception".
4. Find the average rating of movies directed by Christopher Nolan.
5. Get the top 5 actors who appeared in the most movies.
6. Show all movies released in 1993 with their ratings.
7. Retrieve the total number of ratings for each movie.
8. Find all directors who have directed more than 2 movies.
9. List all movies along with actor names (one row per actor).
10. Get movies rated above 9 by at least 2 users.

---

## ✅SOLUTIONS

**1. Sci-Fi movies with ratings:**

```sql
SELECT m.title, r.rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.genre = 'Sci-Fi';
```

**2. Count of movies per director:**

```sql
SELECT d.director_name, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```

**3. Actors in "Inception":**

```sql
SELECT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
WHERE m.title = 'Inception';
```

**4. Average rating of Christopher Nolan movies:**

```sql
SELECT AVG(r.rating) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE d.director_name = 'Christopher Nolan';
```

**5. Top 5 actors by number of movies:**

```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS movies_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY movies_count DESC
LIMIT 5;
```

**6. Movies released in 1993 with ratings:**

```sql
SELECT m.title, r.rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.release_year = 1993;
```

**7. Total number of ratings per movie:**

```sql
SELECT m.title, COUNT(r.rating_id) AS total_ratings
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```

**8. Directors with more than 2 movies:**

```sql
SELECT d.director_name, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name
HAVING COUNT(m.movie_id) > 2;
```

**9. Movies with actor names:**

```sql
SELECT m.title, a.actor_name
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id;
```

**10. Movies rated above 9 by at least 2 users:**

```sql
SELECT m.title, COUNT(r.rating_id) AS rating_count
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE r.rating > 9
GROUP BY m.title
HAVING COUNT(r.rating_id) >= 2;
```

---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
