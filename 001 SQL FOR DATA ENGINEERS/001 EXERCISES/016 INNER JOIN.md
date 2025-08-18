# INNER JOIN
---

## 🔑KEYWORDS
- **JOIN**
- **INNER JOIN**
- **USING**
- **ON**
- **Aliases**
- **Matching Records**
- **Relational Tables**

---
## 📖DEFINITION  
- **JOIN** - combines rows from two or more tables based on a related column between them.
- **INNER JOIN** – a relational operation that combines two tables by matching values in a specified column (or columns) and keeps **only** the rows that satisfy the match.
- **ON / USING clause** – tells the database **how** to match rows  
- **Table aliases (AS)** – shorten names (e.g. `m`, `d`) and remove ambiguity  
- **Dot-notation** – `table.column` prevents “ambiguous column” errors  
- **Primary Key / Foreign Key** – the columns used to relate tables  
---
## 🧱QUERY FORMAT  
```sql
SELECT  <alias1>.col1,
        <alias2>.col2,
        ...
FROM    left_table  AS alias1
INNER JOIN right_table AS alias2
        ON alias1.key = alias2.key;
```
-- When joining column have identical name
```sql
... INNER JOIN right_table AS alias2 USING (key);
```
---
---
## 💡TIP TO REMEMBER
- **“INNER JOIN keeps intersections; if either side has no match, the row disappears.”** Always use aliases and dot-notation when the same column name exists in both tables.
- Use aliases to simplify queries.
- Use USING when joining on columns with identical names in both tables.
- INNER JOIN can be chained to multiple tables for more complex queries (e.g., movies → actors → ratings).
- ALIAS 'AS' is optional when using for aliasing columns or table; though it is good to use `AS` for clear queries 

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

### 1.Join movies and directors on director_id to get title, genre, and director_name.
```sql
SELECT m.title, m.genre, d.director_name
FROM movies m
INNER JOIN directors d
ON m.director_id = d.director_id;
```
### 2.Join movies, movie_actors, and actors to get title and actor_name.
```sql
SELECT m.title, a.actor_name
FROM movies m
INNER JOIN movie_actors ma ON m.movie_id = ma.movie_id
INNER JOIN actors a ON ma.actor_id = a.actor_id
ORDER BY m.movie_id;
```
### 3.Join movies and movie_ratings to get title and average rating, grouped by movie_id.
```sql
SELECT m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
INNER JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id
ORDER BY m.movie_id;
```
---
## 🧠Practise
1. List every movie title together with its director name and nationality.
2. Show the title, rating, and director name for all movies whose genre = 'Animation', ordered by rating descending.
3. Return every actor name and the titles of the movies they appeared in.
4. Find all director–actor pairs where the movie runtime is > 140 min. Display director name, actor name, movie title.
---
## ✅SOLUTIONS
### 1. List every movie title together with its director name and nationality.
```sql
SELECT m.title,
       d.director_name,
       d.nationality
FROM   movies     AS m
JOIN   directors  AS d
  ON   m.director_id = d.director_id;
```
### 2. Show the title, rating, and director name for all movies whose genre = 'Animation', ordered by rating descending.
```sql
SELECT m.title,
       r.rating,
       d.director_name
FROM   movies        AS m
JOIN   directors     AS d ON m.director_id = d.director_id
JOIN   movie_ratings AS r ON m.movie_id = r.movie_id
WHERE  m.genre = 'Animation'
ORDER  BY r.rating DESC;
```
### 3. Return every actor name and the titles of the movies they appeared in.
```sql
SELECT a.actor_name,
       m.title
FROM   actors        AS a
JOIN   movie_actors  AS ma ON a.actor_id = ma.actor_id
JOIN   movies        AS m  ON ma.movie_id = m.movie_id
ORDER  BY a.actor_name, m.title;
```
### 4. Find all director–actor pairs where the movie runtime is > 140 min. Display director name, actor name, movie title.
```sql
SELECT d.director_name,
       a.actor_name,
       m.title
FROM   movies        AS m
JOIN   directors     AS d ON m.director_id = d.director_id
JOIN   movie_actors  AS ma ON m.movie_id = ma.movie_id
JOIN   actors        AS a ON ma.actor_id = a.actor_id
WHERE  m.runtime_minutes > 140
ORDER  BY d.director_name, a.actor_name;
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
