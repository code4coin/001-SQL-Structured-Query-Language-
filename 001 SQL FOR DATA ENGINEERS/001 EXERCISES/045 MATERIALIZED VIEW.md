# 📘MATERIALIZED VIEW
---
## 🔑KEYWORDS
- **Materialized View**
- **Non-Materialized View**
- **Refresh**
- **Dependency Chain**
- **Directed Acyclic Graph (DAG)**  

---
## 📖DEFINITION
- **Materialized View** – A database object that stores the results of a query physically on disk. Unlike normal views (which are virtual), materialized views return results instantly but need to be **refreshed** when underlying data changes.  

---
## 🧱QUERY FORMAT
```sql
-- Create a materialized view
CREATE MATERIALIZED VIEW view_name AS
SELECT ...
FROM ...
WHERE ...;

-- Refresh a materialized view
REFRESH MATERIALIZED VIEW view_name;
```

---
## 💡TIP TO REMEMBER
- Use **materialized views** for **complex queries** that take time to execute.  
- They are best suited for **analytics and reporting (OLAP)**, where data does not change frequently.  
- Always plan **refresh schedules** carefully when multiple materialized views depend on each other.  

---
## 💪EXERCISE  
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory:  
[CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)  

---

### 1. Create a materialized view that stores the average rating and number of reviews for each movie.  

<details>  
<summary>✅ Solution</summary>  

```sql
CREATE MATERIALIZED VIEW movie_avg_ratings AS
SELECT 
    m.title,
    ROUND(AVG(r.rating), 2) AS avg_rating,
    COUNT(r.rating_id) AS total_reviews
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```

```sql
SELECT * FROM movie_avg_ratings;
```

</details>  

---

### 2. Create a materialized view that shows the top 3 highest-rated movies.  

<details>  
<summary>✅ Solution</summary>  

```sql
CREATE MATERIALIZED VIEW top_movies AS
SELECT title, avg_rating
FROM movie_avg_ratings
ORDER BY avg_rating DESC
LIMIT 3;
```

```sql
SELECT * FROM top_movies;
```

</details>  

---

### 3. Create a materialized view showing each director and their average movie rating.  

<details>  
<summary>✅ Solution</summary>  

```sql
CREATE MATERIALIZED VIEW director_avg_ratings AS
SELECT 
    d.director_name,
    ROUND(AVG(mar.avg_rating), 2) AS director_avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_avg_ratings mar ON m.title = mar.title
GROUP BY d.director_name;
```

```sql
SELECT * FROM director_avg_ratings ORDER BY director_avg_rating DESC;
```

</details>  

---
## 🧠Practise  
1. Create a materialized view to show all actors and the **total number of movies** they have acted in.  
2. Create a materialized view that lists movies released after 2010 with their **average ratings**.  
3. Create a materialized view that shows the **highest-rated movie per director**.
---

## ✅SOLUTIONS  
### 1. Create a materialized view to show all actors and the **total number of movies** they have acted in.  

<details>
<summary>💡 Solution</summary>  

```sql
CREATE MATERIALIZED VIEW actor_movie_count AS
SELECT 
    a.actor_name,
    COUNT(ma.movie_id) AS total_movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name;
```

```sql
SELECT * FROM actor_movie_count ORDER BY total_movies DESC;
```

</details>  

---
### 2. Create a materialized view that lists movies released after 2010 with their **average ratings**.  

<details>
<summary>💡 Solution</summary>  

```sql
CREATE MATERIALIZED VIEW movies_after_2010_ratings AS
SELECT 
    m.title,
    m.release_year,
    ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.release_year > 2010
GROUP BY m.title, m.release_year;
```

```sql
SELECT * FROM movies_after_2010_ratings;
```

</details>  

---

### 3. Create a materialized view that shows the **highest-rated movie per director**.  

<details>
<summary>💡 Solution</summary>  

```sql
CREATE MATERIALIZED VIEW director_best_movie AS
SELECT DISTINCT ON (d.director_name)
    d.director_name,
    m.title,
    mar.avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_avg_ratings mar ON m.title = mar.title
ORDER BY d.director_name, mar.avg_rating DESC;
```

```sql
SELECT * FROM director_best_movie;
```

</details>  

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
