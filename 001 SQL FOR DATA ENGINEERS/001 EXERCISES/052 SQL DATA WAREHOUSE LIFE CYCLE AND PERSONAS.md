# DATA WAREHOUSE LIFE CYCLE AND PERSONAS

---

## 🔑KEYWORDS

* **Data Warehouse**
* **ETL (Extract, Transform, Load)**
* **BI (Business Intelligence)**
* **Data Modeling**
* **Personas** (Analyst, Data Scientist, Data Engineer, DBA)
* **Planning, Implementation, Maintenance**

---

## 📖DEFINITION

* **Data Warehouse** – A central repository of integrated data used for reporting, analytics, and business decision-making.
* **ETL Process** – A sequence of steps to extract data from sources, transform it into a suitable format, and load it into the warehouse.
* **BI Tools** – Software like Tableau, Power BI, or Looker that interacts with the warehouse to create dashboards and reports.
* **Personas** – Roles such as Analyst, Scientist, Engineer, and DBA that contribute to different stages of the warehouse life cycle.

---

## 🧱QUERY FORMAT

* SQL queries typically use **JOINs** to combine tables like `movies`, `directors`, `actors`, `movie_actors`, and `movie_ratings`.
* Aggregations like **AVG(), COUNT(), MAX()** are used for reporting metrics.
* Filtering is done with **WHERE**, sorting with **ORDER BY**, and limiting results with **LIMIT**.

---

## 💡TIP TO REMEMBER

* Always understand **business requirements** before designing a data model.
* Map **personas to stages** for clear responsibilities.
* Test queries on sample data before deploying dashboards.
* Use **aggregations and joins** to answer analytical questions efficiently.

---

## 💪EXERCISE

Before moving to the exercises, make sure you have the tables and sample data loaded.
[CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Retrieve the top 3 highest-rated movies
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

### 2. List all Sci-Fi movies with directors
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Sci-Fi';
```
</details>

### 3. Average rating for each director
<details>
<summary>✅ Solution</summary>

```sql
SELECT d.director_name, ROUND(AVG(r.rating),2) AS avg_rating
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name
ORDER BY avg_rating DESC;
```
</details>

### 4. Top 5 actor-director collaborations
<details>
<summary>✅ Solution</summary>

```sql
SELECT d.director_name, a.actor_name, COUNT(*) AS collaborations
FROM directors d
JOIN movies m ON d.director_id = m.director_id
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
GROUP BY d.director_name, a.actor_name
ORDER BY collaborations DESC
LIMIT 5;
```
</details>

### 5. Movies without ratings
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE r.movie_id IS NULL;
```
</details>

---

## 🧠Practise

1. Retrieve all Action movies with runtime greater than 150 minutes.
2. Find all actors who starred in more than 2 movies.
3. List all movies released before the year 2000 along with director names.
4. Find the highest-rated movie for each genre.
5. Count how many movies each actor has acted in, ordered by highest count.

---

## ✅SOLUTIONS

1. Action movies >150 minutes

```sql
SELECT title, runtime_minutes
FROM movies
WHERE genre='Action' AND runtime_minutes > 150;
```

2. Actors in more than 2 movies

```sql
SELECT a.actor_name, COUNT(*) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
HAVING COUNT(*) > 2;
```

3. Movies before 2000 with director names

```sql
SELECT m.title, d.director_name, m.release_year
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year < 2000;
```

4. Highest-rated movie per genre

```sql
SELECT genre, title, AVG(rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY genre, title
HAVING AVG(r.rating) = (
    SELECT MAX(avg_rating)
    FROM (
        SELECT AVG(r2.rating) AS avg_rating
        FROM movies m2
        JOIN movie_ratings r2 ON m2.movie_id = r2.movie_id
        WHERE m2.genre = m.genre
        GROUP BY m2.title
    ) AS genre_avg
);
```

5. Count movies per actor

```sql
SELECT a.actor_name, COUNT(*) AS movie_count
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY movie_count DESC;
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
