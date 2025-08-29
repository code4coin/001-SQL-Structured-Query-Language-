# ROW AND COLUMN DATA STORAGE
---

## 🔑KEYWORDS

* **Row Store**
* **Column Store**
* **Transactional Workload (OLTP)**
* **Analytical Workload (OLAP)**
* **Data Blocks**
* **Data Compression**

---

## 📖DEFINITION

* **Row Store** – Data is stored row by row in blocks. Ideal for transactional workloads where individual records are frequently inserted, updated, or deleted.
* **Column Store** – Data is stored column by column in blocks. Ideal for analytical workloads, where aggregates and summaries are common, with better compression and faster query speed.

---

## 🧱QUERY FORMAT

* Row store queries typically access **all columns for a few rows**.

```sql
SELECT * FROM movies WHERE movie_id = 1;
```

* Column store queries typically access **a few columns for many rows**.

```sql
SELECT AVG(runtime_minutes) AS avg_runtime
FROM movies
WHERE genre = 'Sci-Fi';
```

---

## 💡TIP TO REMEMBER

* Row store = fast for **INSERT/UPDATE/DELETE**
* Column store = fast for **SELECT with aggregates**
* Column store = better **data compression**
* Choose storage format depending on **workload type** (OLTP vs OLAP)

---

## 💪EXERCISE

Before moving to the exercises, ensure your platform has the tables and data loaded.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find all movies directed by 'Christopher Nolan'.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT title
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```
</details>

### 2. Calculate the average runtime of 'Action' genre movies.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT AVG(runtime_minutes) AS avg_runtime
FROM movies
WHERE genre = 'Action';
```
</details>

### 3. List all movies released after 2010.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT title, release_year
FROM movies
WHERE release_year > 2010;
```
</details>

### 4. Count the number of movies for each genre.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT genre, COUNT(*) AS movie_count
FROM movies
GROUP BY genre;
```
</details>

### 5. Find the highest-rated movie for each user from the `movie_ratings` table.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT user_name, movie_id, MAX(rating) AS highest_rating
FROM movie_ratings
GROUP BY user_name, movie_id;
```
</details>

---

## 🧠Practise

1. List the titles of movies with runtime greater than 150 minutes.
2. Find all actors who acted in movies directed by 'Quentin Tarantino'.
3. Calculate the average rating of all 'Sci-Fi' movies.
4. List the top 3 longest movies with their director name.
5. Find movies with no ratings in `movie_ratings` table.

---

## ✅SOLUTIONS

### Practise Solutions

1. Titles of movies with runtime greater than 150 minutes:

```sql
SELECT title
FROM movies
WHERE runtime_minutes > 150;
```

2. Actors in movies directed by 'Quentin Tarantino':

```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Quentin Tarantino';
```

3. Average rating of all 'Sci-Fi' movies:

```sql
SELECT AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.genre = 'Sci-Fi';
```

4. Top 3 longest movies with director name:

```sql
SELECT m.title, m.runtime_minutes, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
ORDER BY m.runtime_minutes DESC
LIMIT 3;
```

5. Movies with no ratings:

```sql
SELECT title
FROM movies
WHERE movie_id NOT IN (SELECT movie_id FROM movie_ratings);
```

---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)

