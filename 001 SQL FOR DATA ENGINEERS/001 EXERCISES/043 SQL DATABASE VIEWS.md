# DATABASE VIEW

---

## 🔑KEYWORDS

* **View**
* **Virtual Table**
* **CREATE VIEW**
* **INFORMATION\_SCHEMA.views**
* **Access Control**
* **Query Simplification**

---

## 📖DEFINITION

* **Database View** – A view is a **virtual table** in SQL that is not stored physically. It stores a query that retrieves data from one or more tables, allowing users to query it like a normal table. Views help simplify queries, reuse SQL logic, and provide access control.

---

## 🧱QUERY FORMAT

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

To view all views in PostgreSQL:

```sql
SELECT table_name
FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');
```

---

## 💡TIP TO REMEMBER

* Views do **not take extra storage**, only the query is stored.
* Use views to **simplify complex joins**.
* Use views to **restrict sensitive data** for users.
* Views are always **up-to-date** with the underlying tables.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Create a view called `sci_fi_movies` that lists all Sci-Fi movies with their title, release year, and director name.
<details>
  <summary>✅ Solution:</summary>
```sql
CREATE VIEW sci_fi_movies AS
SELECT m.title, m.release_year, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Sci-Fi';
```
</details>

### 2. Create a view called `movie_actor_list` that lists each movie along with all its actors in a single column separated by commas.
<details>
  <summary>✅ Solution:</summary>
```sql
CREATE VIEW movie_actor_list AS
SELECT m.title, STRING_AGG(a.actor_name, ', ') AS actors
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
GROUP BY m.title;
```
</details>

### 3. Create a view called `user_movie_ratings` that shows movie title, user\_name, and rating from `movie_ratings` without exposing `rating_id`.
<details>
  <summary>✅ Solution:</summary>
```sql
CREATE VIEW user_movie_ratings AS
SELECT m.title, mr.user_name, mr.rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id;
```
</details>

### 4. Retrieve all views created in the database.
<details>
  <summary>✅ Solution:</summary>
```sql
SELECT table_name
FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');
```
</details>

### 5. Create a view called `recent_movies` that lists all movies released after 2015 along with director name.
<details>
  <summary>✅ Solution:</summary>
```sql
CREATE VIEW recent_movies AS
SELECT m.title, m.release_year, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year > 2015;
```
</details>

### 6. Create a view called `top_rated_movies` that lists movies with average rating >= 9.0, including title and average rating.
<details>
  <summary>✅ Solution:</summary>
```sql
CREATE VIEW top_rated_movies AS
SELECT m.title, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title
HAVING AVG(mr.rating) >= 9.0;
```
</details>
---

## 🧠Practise

1. Create a view called `action_movies` to list all Action movies with their director name and runtime in minutes.
2. Create a view called `movies_by_country` that counts how many movies each country has produced.
3. Create a view called `director_movie_count` to list each director and the number of movies they have directed.
4. Create a view called `war_movies` that lists all War genre movies with release year and director.
5. Create a view called `actor_movies` that lists all actors and the titles of movies they acted in (comma-separated).
6. Create a view called `movie_rating_summary` that lists movie title, number of ratings, and average rating.
7. Create a view called `older_movies` that lists all movies released before 2000 along with director name and genre.

---

## ✅SOLUTIONS

```sql
-- 1. Action Movies
CREATE VIEW action_movies AS
SELECT m.title, d.director_name, m.runtime_minutes
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Action';
```
```sql
-- 2. Movies by Country
CREATE VIEW movies_by_country AS
SELECT country, COUNT(*) AS movie_count
FROM movies
GROUP BY country;
```
```sql
-- 3. Director Movie Count
CREATE VIEW director_movie_count AS
SELECT d.director_name, COUNT(m.movie_id) AS movie_count
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```
```sql
-- 4. War Movies
CREATE VIEW war_movies AS
SELECT m.title, m.release_year, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'War';
```
```sql
-- 5. Actor Movies
CREATE VIEW actor_movies AS
SELECT a.actor_name, STRING_AGG(m.title, ', ') AS movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
GROUP BY a.actor_name;
```
```sql
-- 6. Movie Rating Summary
CREATE VIEW movie_rating_summary AS
SELECT m.title, COUNT(mr.rating_id) AS num_ratings, AVG(mr.rating) AS avg_rating
FROM movies m
JOIN movie_ratings mr ON m.movie_id = mr.movie_id
GROUP BY m.title;
```
```sql
-- 7. Older Movies
CREATE VIEW older_movies AS
SELECT m.title, m.release_year, d.director_name, m.genre
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year < 2000;
```

---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
