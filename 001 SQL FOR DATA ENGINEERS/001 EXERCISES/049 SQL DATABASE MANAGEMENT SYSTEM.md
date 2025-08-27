# DATABASE MANAGEMENT SYSTEM
---

## 🔑KEYWORDS

* DBMS
* SQL DBMS
* NoSQL DBMS
* RDBMS
* Key-Value Store
* Document Store
* Columnar Database
* Graph Database

---

## 📖DEFINITION

* **DBMS** – A software system that allows users to create, maintain, and interact with databases efficiently. It manages the data, database schema, and database engine, serving as an interface between applications and the database.
* **SQL DBMS** – Organizes data into tables with rows and columns, uses SQL for queries, and ensures data consistency.
* **NoSQL DBMS** – Flexible, schema-less databases for rapidly growing or unstructured data, including types like key-value, document, columnar, and graph databases.

---

## 🧱QUERY FORMAT

* SQL queries follow the standard structure:

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name
ORDER BY column_name;
```

---

## 💡TIP TO REMEMBER

* Use **SQL DBMS** when your data is structured, consistent, and schema is predefined.
* Use **NoSQL DBMS** when your data grows rapidly, is unstructured, or schema may change frequently.
* SQL queries can JOIN multiple tables to retrieve relational data.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### Questions


1. List all movies along with their director names
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id;
```
</details>

2. Find the average rating for each movie
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```
</details>

3. List all actors who acted in 'Inception'
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
WHERE m.title = 'Inception';
```
</details>


4. Find all movies directed by 'Steven Spielberg' released before 2000
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT m.title, m.release_year
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Steven Spielberg' AND m.release_year < 2000;
```
</details>


5. Find the top 3 highest-rated movies
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

---

## 🧠PRACTISE

1. List all movies with runtime greater than 150 minutes.
2. Find all actors who acted in movies directed by 'Christopher Nolan'.
3. Count the number of movies per genre.
4. List movies along with the number of ratings they have received.
5. Find movies where Leonardo DiCaprio and Brad Pitt acted together.

---

## ✅SOLUTIONS

1. List all movies with runtime greater than 150 minutes

```sql
SELECT title, runtime_minutes
FROM movies
WHERE runtime_minutes > 150;
```

2. Find all actors who acted in movies directed by 'Christopher Nolan'

```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```

3. Count the number of movies per genre

```sql
SELECT genre, COUNT(*) AS movie_count
FROM movies
GROUP BY genre;
```

4. List movies along with the number of ratings they have received

```sql
SELECT m.title, COUNT(r.rating_id) AS num_ratings
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```

5. Find movies where Leonardo DiCaprio and Brad Pitt acted together

```sql
SELECT m.title
FROM movies m
JOIN movie_actors ma1 ON m.movie_id = ma1.movie_id
JOIN actors a1 ON ma1.actor_id = a1.actor_id
JOIN movie_actors ma2 ON m.movie_id = ma2.movie_id
JOIN actors a2 ON ma2.actor_id = a2.actor_id
WHERE a1.actor_name = 'Leonardo DiCaprio' AND a2.actor_name = 'Brad Pitt';
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
