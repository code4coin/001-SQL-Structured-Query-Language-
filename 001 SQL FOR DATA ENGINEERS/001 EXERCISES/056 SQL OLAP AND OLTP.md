# OLAP VS OLTP IN DATA WAREHOUSE

---
## 🔑KEYWORDS
- **OLAP (Online Analytical Processing)**
- **OLTP (Online Transaction Processing)**
- **Data Cube**
- **Slicing and Dicing**
- **Aggregation**
- **Movies Database Example**

---
## 📖DEFINITION
- **OLAP** – A system designed for **multidimensional analysis** of large volumes of data, optimized for queries, reporting, and decision-making.  
- **OLTP** – A system designed for **transaction processing**, optimized for fast inserts, updates, and deletes on small amounts of data.  

---
## 🧱QUERY FORMAT
- **OLAP Query Example** (Aggregations, Grouping, Analysis):  
```sql
SELECT genre, ROUND(AVG(rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY genre
ORDER BY avg_rating DESC;
```

- **OLTP Query Example** (Insert, Update, Delete):  
```sql
INSERT INTO movie_ratings (rating_id, movie_id, user_name, rating, review_date)
VALUES (16, 1, 'user5', 9.2, '2024-05-01');
```

---
## 💡TIP TO REMEMBER
- **OLTP → Operational**: Fast, real-time transactions (Insert, Update, Delete).  
- **OLAP → Analytical**: Complex queries across large data sets (Aggregations, Drill-down, Slice & Dice).  
- OLTP systems feed into OLAP systems via **ETL pipelines**.  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory:  
[CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)  

### 1. Insert a new rating for the movie **Titanic** by a new user.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
INSERT INTO movie_ratings (rating_id, movie_id, user_name, rating, review_date)
VALUES (18, 11, 'user7', 9.1, '2024-05-05');
```
</details>

### 2. Find the **average rating by director**.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT d.director_name, ROUND(AVG(r.rating),2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name
ORDER BY avg_rating DESC;
```
</details>

### 3. Find the **highest-rated movie in each genre**.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT genre, title, MAX(avg_rating) AS top_rating
FROM (
    SELECT m.genre, m.title, ROUND(AVG(r.rating),2) AS avg_rating
    FROM movies m
    JOIN movie_ratings r ON m.movie_id = r.movie_id
    GROUP BY m.genre, m.title
) sub
GROUP BY genre, title
ORDER BY top_rating DESC;
```
</details>

---
## 🧠Practise
1. Update the runtime of the movie **Avatar** to `165 minutes`.  
2. Find the **average rating per year** of movie releases.  
3. Count the number of movies each actor has worked in.  
4. Show the **top 3 genres** with the highest average ratings.  

---
## ✅SOLUTIONS
### 1. Update the runtime of Avatar  
```sql
UPDATE movies
SET runtime_minutes = 165
WHERE title = 'Avatar';
```

### 2. Average rating per year  
```sql
SELECT m.release_year, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.release_year
ORDER BY m.release_year;
```

### 3. Count of movies per actor  
```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY movie_count DESC;
```

### 4. Top 3 genres by rating  
```sql
SELECT genre, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY genre
ORDER BY avg_rating DESC
LIMIT 3;
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
