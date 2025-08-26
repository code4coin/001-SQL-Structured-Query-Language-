# 📘MANAGINING DATABASE VIEW
---
## 🔑KEYWORDS
- **View**
- **CREATE VIEW**
- **CREATE OR REPLACE VIEW**
- **GRANT / REVOKE**
- **UPDATE via View**
- **INSERT via View**
- **DROP VIEW**
- **CASCADE / RESTRICT**

---
## 📖DEFINITION
- **View** – A saved SQL query that acts like a virtual table. It doesn’t store data itself but pulls data from underlying tables when queried.  
- Views are useful for **simplifying queries**, **improving security**, and **controlling access** to data.  

---
## 🧱QUERY FORMAT

### Create a Simple View
```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

### Create or Replace a View
```sql
CREATE OR REPLACE VIEW view_name AS
SELECT ...
```

### Grant / Revoke Access
```sql
GRANT privilege_list ON object_name TO role_name;
REVOKE privilege_list ON object_name FROM role_name;
```

### Drop a View
```sql
DROP VIEW view_name [CASCADE | RESTRICT];
```

---

## 💡TIP TO REMEMBER
- Views can **hide sensitive data** while exposing only the necessary columns.  
- **Updatable views** work only on simple queries (single-table, no aggregates/joins).  
- Use **read-only views** for reporting to prevent accidental data modification.  
- Always check dependencies before dropping a view.  

---

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---
### 1. Create a view `top_rated_movies` that shows movies with an average rating above 9.0.  
<details><summary>✅ Solutions:</summary>

```sql
CREATE VIEW top_rated_movies AS
SELECT m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
HAVING AVG(r.rating) > 9.0;
```
</details>

---

### 2. Create a view `actor_movie_count` that lists each actor and how many movies they have acted in.  
<details><summary>✅ Solutions:</summary>

```sql
CREATE VIEW actor_movie_count AS
SELECT a.actor_name,
       COUNT(ma.movie_id) AS total_movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name;
```
</details>

---

### 3. Redefine the `director_avg_ratings` view to also include the number of movies each director has directed.  
<details><summary>✅ Solutions:</summary>

```sql
CREATE OR REPLACE VIEW director_avg_ratings AS
SELECT d.director_name,
       ROUND(AVG(r.rating), 2) AS avg_rating,
       COUNT(DISTINCT m.movie_id) AS movie_count
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name;
```
</details>


---

## 🧠Practise
1. Create a view to show each director and their average movie rating. 
2. Grant SELECT privilege on the `director_avg_ratings` view to all users.
3. Create a view named `nolan_movies` that lists only Christopher Nolan’s movies (title, genre, year).
4. Update the genre of "Inception" to "Science Fiction" using the `nolan_movies` view.
5. Drop the `nolan_movies` view safely (restrict).  
---

## ✅SOLUTIONS

### 1. Create a view to show each director and their average movie rating.  
✅ **Solution**
```sql
CREATE VIEW director_avg_ratings AS
SELECT d.director_name,
       ROUND(AVG(r.rating), 2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name;
```

---

### 2. Grant SELECT privilege on the `director_avg_ratings` view to all users.  
✅ **Solution**
```sql
GRANT SELECT ON director_avg_ratings TO PUBLIC;
```

---

### 3. Create a view named `nolan_movies` that lists only Christopher Nolan’s movies (title, genre, year).  
✅ **Solution**
```sql
CREATE VIEW nolan_movies AS
SELECT title, genre, release_year
FROM movies
WHERE director_id = 1;
```

---

### 4. Update the genre of "Inception" to "Science Fiction" using the `nolan_movies` view.  
✅ **Solution**
```sql
UPDATE nolan_movies
SET genre = 'Science Fiction'
WHERE title = 'Inception';
```

---

### 5. Drop the `nolan_movies` view safely (restrict).  
✅ **Solution**
```sql
DROP VIEW nolan_movies RESTRICT;
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

