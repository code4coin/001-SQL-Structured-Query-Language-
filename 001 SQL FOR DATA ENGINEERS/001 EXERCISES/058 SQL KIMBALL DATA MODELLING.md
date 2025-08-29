# KIMBALL DATA MODELLING

# Kimball’s Four-Step Process for Dimensional Modeling  

---
## 🔑KEYWORDS
- **Fact Table**
- **Dimension Table**
- **Grain**
- **Organizational Process**
- **Kimball Methodology**
- **Star Schema**
---

## 📖DEFINITION
- **Kimball’s Four-Step Process** – A systematic method to design dimensional models for data warehouses by:
  1. Selecting the organizational process.  
  2. Declaring the grain (level of detail).  
  3. Identifying the dimensions.  
  4. Identifying the facts.  

This approach ensures business-focused and analysis-ready data marts.  

---

## 🧱QUERY FORMAT
The general SQL approach to apply Kimball’s method:  

```sql
-- Step 1: Select Process
-- Example: Analyzing movie ratings

-- Step 2: Declare Grain
-- Grain: One row per user’s rating of a movie per date

-- Step 3: Identify Dimensions
SELECT m.title, d.director_name, mr.user_name, mr.review_date
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings mr ON m.movie_id = mr.movie_id;

-- Step 4: Identify Facts
SELECT m.title, COUNT(mr.rating) AS total_reviews, 
       AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC;
```

---

## 💡TIP TO REMEMBER
- Always **declare the grain first** before selecting facts, otherwise metrics may not align.  
- Dimensions = **context** (Who, What, Where, When).  
- Facts = **measurements** (How much, How many).  
- Work closely with **business users** to ensure data marts are relevant.  

---

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)  

### 1. Find the top 3 movies with the highest average ratings.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT m.title, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

### 2. Show each director with the total number of movies they directed.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT d.director_name, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name
ORDER BY total_movies DESC;
```
</details>

### 3. List all actors who appeared in movies directed by *Christopher Nolan*.  
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

### 4. For each year, find the movie with the highest average rating. 
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT m.release_year, m.title, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.release_year, m.title
HAVING AVG(mr.rating) = (
    SELECT MAX(sub.avg_rating)
    FROM (
        SELECT m2.release_year, m2.title, AVG(mr2.rating) AS avg_rating
        FROM movies m2
        JOIN movie_ratings mr2 ON m2.movie_id = mr2.movie_id
        GROUP BY m2.release_year, m2.title
        HAVING m2.release_year = m.release_year
    ) sub
)
ORDER BY m.release_year;
```
</details>

---

## 🧠Practise
1. Find the number of reviews given by each user.  
2. Which actor has acted in the most number of movies?  
3. List all movies released after 2010 with an average rating above 9.0.  
4. Show the top 2 directors with the highest-rated movies on average.  

---

## ✅SOLUTIONS

### 1. Find the number of reviews given by each user.  
```sql
SELECT mr.user_name, COUNT(mr.rating) AS total_reviews
FROM movie_ratings mr
GROUP BY mr.user_name
ORDER BY total_reviews DESC;
```

### 2. Which actor has acted in the most number of movies?  
```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS total_movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY total_movies DESC
LIMIT 1;
```

### 3. List all movies released after 2010 with an average rating above 9.0.  
```sql
SELECT m.title, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
WHERE m.release_year > 2010
GROUP BY m.title
HAVING AVG(mr.rating) > 9.0
ORDER BY avg_rating DESC;
```

### 4. Show the top 2 directors with the highest-rated movies on average.  
```sql
SELECT d.director_name, AVG(mr.rating) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY d.director_name
ORDER BY avg_rating DESC
LIMIT 2;
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
