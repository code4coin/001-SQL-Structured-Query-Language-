# 📊 Data Warehouse Architectures

---
## 🔑KEYWORDS
- **Inmon Architecture (Top-Down)**
- **Kimball Architecture (Bottom-Up)**
- **Normalized Data**
- **Denormalized Data**
- **Enterprise Data Warehouse (EDW)**
- **Data Marts**
- **Star Schema**
---

## 📖DEFINITION
- **Inmon (Top-Down)** – An approach where a centralized enterprise data warehouse is built first in a normalized format. Data marts are created later for departmental analysis.  
- **Kimball (Bottom-Up)** – An approach where departmental data marts are built first in a denormalized star schema and later integrated into a data warehouse.  

---

## 🧱QUERY FORMAT
### Example of Top-Down (Inmon):
```sql
SELECT 
    d.director_name,
    ROUND(AVG(r.rating), 2) AS avg_director_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name;
```

### Example of Bottom-Up (Kimball):
```sql
SELECT 
    r.user_name,
    m.title,
    d.director_name,
    r.rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id;
```

---

## 💡TIP TO REMEMBER
- **Inmon** → “Warehouse first, marts later” → Consistency & governance.  
- **Kimball** → “Marts first, warehouse later” → Fast results, user-friendly.  

---

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 1. Find the **top 3 movies with the highest average rating**.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT 
    m.title,
    ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

---

### 2. Get the **average rating of each director**.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT 
    d.director_name,
    ROUND(AVG(r.rating), 2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name
ORDER BY avg_rating DESC;
```
</details>

---

### 3. List all movies along with their **lead actors**.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT 
    m.title,
    a.actor_name
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
ORDER BY m.title;
```
</details>

---

### 4. Find the **total number of movies** released by each director.  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT 
    d.director_name,
    COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name
ORDER BY total_movies DESC;
```
</details>

---

### 5. Which actor has appeared in the **most movies**?  
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT 
    a.actor_name,
    COUNT(ma.movie_id) AS movies_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY movies_count DESC
LIMIT 1;
```
</details>

---

## 🧠Practise
Here are some practice questions for you. Try solving them before checking the solutions.  

1. Find all movies released after the year **2010** with an average rating above **9.0**.  
2. Show the list of actors who worked in **Christopher Nolan** movies.  
3. Find the **longest movie** (by runtime) along with its director.  
4. Which movie has the **highest number of user reviews**?  
5. Get the **average rating per genre**.  

---

## ✅SOLUTIONS

### 1. Movies after 2010 with avg rating > 9.0
```sql
SELECT 
    m.title,
    m.release_year,
    ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.release_year > 2010
GROUP BY m.title, m.release_year
HAVING AVG(r.rating) > 9.0;
```

---

### 2. Actors in Christopher Nolan movies
```sql
SELECT DISTINCT 
    a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```

---

### 3. Longest movie with director
```sql
SELECT 
    m.title,
    m.runtime_minutes,
    d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
ORDER BY m.runtime_minutes DESC
LIMIT 1;
```

---

### 4. Movie with most reviews
```sql
SELECT 
    m.title,
    COUNT(r.rating_id) AS total_reviews
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY total_reviews DESC
LIMIT 1;
```

---

### 5. Average rating per genre
```sql
SELECT 
    m.genre,
    ROUND(AVG(r.rating), 2) AS avg_genre_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre
ORDER BY avg_genre_rating DESC;
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
