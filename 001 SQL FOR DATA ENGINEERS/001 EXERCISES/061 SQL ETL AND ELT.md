# ETL AND ELT
---

## 🔑KEYWORDS
- **ETL (Extract, Transform, Load)**
- **ELT (Extract, Load, Transform)**
- **Data Warehouse**
- **Data Transformation**
- **Cloud Data Warehousing**
---

## 📖DEFINITION
- **ETL** – A process where data is extracted from source systems, transformed (cleaned, aggregated, or enriched), and then loaded into a data warehouse.
- **ELT** – A process where data is extracted from source systems, loaded into a data warehouse first, and then transformed using the warehouse’s compute resources.
---

## 🧱QUERY FORMAT
- **ETL Example:** Transform before loading  
```sql
SELECT d.director_name,
       AVG(mr.rating) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY d.director_name;
```

- **ELT Example:** Load first, then transform  
```sql
SELECT m.title,
       AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 5;
```
---

## 💡TIP TO REMEMBER
- **ETL**: Transform first → Load later → Lower storage cost → Easier PII compliance  
- **ELT**: Load first → Transform later → Retain raw data → Scales well with cloud
- Use **ETL** for batch processing and strict compliance; **ELT** for real-time and cloud-based analytics
---

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find the average rating for each genre.  
<details>
  <summary>✅ Solution:</summary>

  ```sql
SELECT genre, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY genre;
```
</details>

### 2. List all directors with the number of movies they have directed.  
<details>
  <summary>✅ Solution:</summary>

```sql
SELECT d.director_name, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```
</details>

### 3. Identify actors who have acted in more than 2 movies.  
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

### 4. Show the top 3 highest-rated movies along with director name.  
<details>
  <summary>✅ Solution:</summary>

```sql
SELECT m.title, d.director_name, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title, d.director_name
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

### 5. Find the total runtime of all movies by each director.  
<details>
  <summary>✅ Solution:</summary>

```sql
SELECT d.director_name, SUM(m.runtime_minutes) AS total_runtime
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```
</details>

---

## 🧠PRACTISE
1. List all movies released after 2010 with their genres.  
2. Find the actor with the highest average movie rating.  
3. List all movies along with actors, sorted by movie title.  
4. Find movies with average rating greater than 9.0.  
5. Find the average runtime of movies per genre.

---

## ✅SOLUTIONS
### 1. List all movies released after 2010 with their genres
```sql
SELECT title, genre, release_year
FROM movies
WHERE release_year > 2010;
```

### 2. Find the actor with the highest average movie rating
```sql
SELECT a.actor_name, AVG(mr.rating) AS avg_rating
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movie_ratings mr ON ma.movie_id = mr.movie_id
GROUP BY a.actor_name
ORDER BY avg_rating DESC
LIMIT 1;
```

### 3. List all movies along with actors, sorted by movie title
```sql
SELECT m.title, a.actor_name
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
ORDER BY m.title;
```

### 4. Find movies with average rating greater than 9.0
```sql
SELECT m.title, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
HAVING AVG(mr.rating) > 9.0;
```

### 5. Find the average runtime of movies per genre
```sql
SELECT genre, AVG(runtime_minutes) AS avg_runtime
FROM movies
GROUP BY genre;
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

---

