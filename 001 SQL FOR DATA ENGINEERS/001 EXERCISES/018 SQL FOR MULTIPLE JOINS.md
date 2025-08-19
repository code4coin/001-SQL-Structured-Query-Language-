# SQL FOR MULTIPLE JOINS
---

## 🔑KEYWORDS
- **INNER JOIN**
- **Multiple Joins**
- **Chaining Joins**
- **Joining on Multiple Keys**
- **bridge table**
---

## 📖DEFINITION
- **Multiple Joins** – Combining more than two tables in a single SQL query using INNER JOIN, LEFT JOIN, or other join types.
- **Chaining Joins** – Sequentially joining tables where the result of one join is used in the next join.
- **Joining on Multiple Keys** – Using multiple columns in the ON clause to restrict join results.
- **bridge table** – resolves many-to-many relationships (e.g. movie_actors) 
---

## 🧱QUERY FORMAT  
```sql
FROM left_table AS l  
INNER JOIN bridge_or_right_table AS r ON l.key = r.key  
INNER JOIN next_table AS n ON r.key = n.key …  
(continue as needed)
```
---

## 💡TIP TO REMEMBER  
- Always start from the **core entity** (usually the fact table) and move outward to the dimension tables.  
- Alias every table (m, d, a, r, etc.) so the ON clauses stay short and readable.  
- When you see a many-to-many relationship, look for a bridge table (movie_actors) that contains the foreign keys of both sides.
- The order of joins matters if filters depend on previous join results.
- Joining on multiple keys can prevent duplicate records.
---

## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. List every movie with its director  
Return title and director_name.
```sql
SELECT m.title, d.director_name  
FROM movies AS m  
INNER JOIN directors AS d ON m.director_id = d.director_id;
```
### 2. Movie + director + actors (post-2010)  
Show title, director_name, and actor_name for films released in or after 2010.
```sql
SELECT m.title, d.director_name, a.actor_name  
FROM movies AS m  
INNER JOIN directors AS d   ON m.director_id = d.director_id  
INNER JOIN movie_actors AS ma ON m.movie_id   = ma.movie_id  
INNER JOIN actors AS a       ON ma.actor_id   = a.actor_id  
WHERE m.release_year >= 2010;
```
### 3. Sci-Fi average rating + director nationality  
Display each Sci-Fi movie, its director’s nationality, and the average rating.
```sql
SELECT m.title, d.nationality, AVG(r.rating) AS avg_rating  
FROM movies AS m  
INNER JOIN directors AS d     ON m.director_id = d.director_id  
INNER JOIN movie_ratings AS r ON m.movie_id    = r.movie_id  
WHERE m.genre = 'Sci-Fi'  
GROUP BY m.title, d.nationality;
```
### 4. Actor pairs who worked together  
Find every unique pair of actors who starred in the same film.
```sql
SELECT DISTINCT a1.actor_name AS actor_1, a2.actor_name AS actor_2, m.title  
FROM movie_actors AS ma1  
INNER JOIN movie_actors AS ma2  
  ON ma1.movie_id = ma2.movie_id  
 AND ma1.actor_id < ma2.actor_id  
INNER JOIN actors AS a1 ON ma1.actor_id = a1.actor_id  
INNER JOIN actors AS a2 ON ma2.actor_id = a2.actor_id  
INNER JOIN movies AS m  ON ma1.movie_id = m.movie_id;
```
---
## 🧠Practise
1. Chain joins to list movies, directors, and actors for movies released in 2020.
2. Join movie_ratings with movies to find the top 5 highest-rated movies.
3. Find actors who acted with a specific director in more than 2 movies.
4. List movies, genres, directors, and average ratings for movies from the USA.
5. List all actors and the number of movies they have acted in.

---
## ✅SOLUTIONS
### 1. Chain joins to list movies, directors, and actors for movies released in 2020.
```sql
SELECT m.title, d.director_name, a.actor_name
FROM movies m
INNER JOIN directors d ON m.director_id = d.director_id
INNER JOIN movie_actors ma ON m.movie_id = ma.movie_id
INNER JOIN actors a ON ma.actor_id = a.actor_id
WHERE m.release_year = 2020;
```
### 2. Join movie_ratings with movies to find the top 5 highest-rated movies.
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
INNER JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 5;
```
### 3. Find actors who acted with a specific director in more than 2 movies.
```sql
SELECT a.actor_name, d.director_name, COUNT(*) AS movie_count
FROM actors a
INNER JOIN movie_actors ma ON a.actor_id = ma.actor_id
INNER JOIN movies m ON ma.movie_id = m.movie_id
INNER JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan'
GROUP BY a.actor_name, d.director_name
HAVING COUNT(*) > 2;
```
### 4. List movies, genres, directors, and average ratings for movies from the USA.
```sql
SELECT m.title, m.genre, d.director_name, AVG(r.rating) AS avg_rating
FROM movies m
INNER JOIN directors d ON m.director_id = d.director_id
INNER JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.country = 'USA'
GROUP BY m.title, m.genre, d.director_name;
```
### 5. List all actors and the number of movies they have acted in.
```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS total_movies
FROM actors a
INNER JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name;
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
