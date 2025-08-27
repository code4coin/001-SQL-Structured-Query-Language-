# 📊 DATA WAREHOUSE
---

## 🔑KEYWORDS
- **Data Warehouse**
- **Central Repository**
- **ETL / ELT**
- **Business Intelligence**
- **Analytics**
---

## 📖DEFINITION
- **Data Warehouse** – A system that collects data from different sources, cleans and integrates it, and stores it in a structured format so it can be analyzed for decision-making.  
---

## 🧱QUERY FORMAT
A typical query to explore data inside a warehouse looks like:

```sql
SELECT column_name, AGG_FUNCTION(column_name)
FROM table_name
WHERE condition
GROUP BY column_name
ORDER BY column_name;
```

---

## 💡TIP TO REMEMBER
Think of a **data warehouse** like a **movie library**:  
- DVDs (data) come from many studios (sources).  
- They are cataloged and placed in a single library (warehouse).  
- Viewers (analysts) can then find patterns, such as "Top Sci-Fi movies" or "Best-rated directors".  

---

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find the **average rating of each director**.  

<details>
<summary>✅ Solution</summary>

```sql
SELECT d.director_name, ROUND(AVG(r.rating), 2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name
ORDER BY avg_rating DESC;
```
</details>

---

### 2. List the **top 3 highest-rated movies** with their director names.  

<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, d.director_name, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title, d.director_name
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

---

### 3. Count how many movies each **actor** has worked in.  

<details>
<summary>✅ Solution</summary>

```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS total_movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY total_movies DESC;
```
</details>

---

### 4. Find the **most popular genre** based on average rating.  

<details>
<summary>✅ Solution</summary>

```sql
SELECT m.genre, ROUND(AVG(r.rating), 2) AS avg_genre_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre
ORDER BY avg_genre_rating DESC
LIMIT 1;
```
</details>

---

## 🧠PRACTISE
Try solving these yourself before checking the solutions.  

1. Show the **average runtime of movies per genre**.  
2. Find the **highest-rated movie in each genre**.  
3. Display all **movies released after 2010** with ratings above 9.0.  
4. Show the **number of movies directed by each nationality of directors**.  

---

## ✅SOLUTIONS

### 1. Average runtime of movies per genre
```sql
SELECT genre, ROUND(AVG(runtime_minutes), 2) AS avg_runtime
FROM movies
GROUP BY genre
ORDER BY avg_runtime DESC;
```

---

### 2. Highest-rated movie in each genre
```sql
SELECT m.genre, m.title, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre, m.title
HAVING AVG(r.rating) = (
    SELECT MAX(avg_genre_rating) 
    FROM (
        SELECT genre, title, AVG(rating) AS avg_genre_rating
        FROM movies m2
        JOIN movie_ratings r2 ON m2.movie_id = r2.movie_id
        GROUP BY genre, title
    ) sub
    WHERE sub.genre = m.genre
)
ORDER BY m.genre;
```

---

### 3. Movies released after 2010 with ratings above 9.0
```sql
SELECT m.title, m.release_year, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.release_year > 2010
GROUP BY m.title, m.release_year
HAVING AVG(r.rating) > 9.0
ORDER BY avg_rating DESC;
```

---

### 4. Number of movies directed by each nationality
```sql
SELECT d.nationality, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.nationality
ORDER BY total_movies DESC;
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
