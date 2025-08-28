# DATA WAREHOUSE LAYERS

---

## 🔑KEYWORDS

* **Data Warehouse**
* **ETL (Extract, Transform, Load)**
* **Source Layer**
* **Staging Layer**
* **Storage Layer**
* **Presentation Layer**
* **Data Marts**
* **Business Intelligence (BI)**

---

## 📖DEFINITION

* **Data Warehouse** – A centralized repository that collects, transforms, stores, and presents data for analysis and reporting.
* **Source Layer** – Raw data sources like transactional databases, files, or APIs.
* **Staging Layer** – Temporary storage for ETL-processed data, cleaning and transforming data before storage.
* **Storage Layer** – Persisted data in the warehouse and data marts ready for queries.
* **Presentation Layer** – End-user access for BI, dashboards, or direct queries.

---

## 🧱QUERY FORMAT

* **SELECT** … **FROM** … **JOIN** … **WHERE** … **GROUP BY** … **ORDER BY** …

---

## 💡TIP TO REMEMBER

* Always ensure data is **cleaned in staging** before moving to storage.
* Data marts are subsets of the warehouse for **specific reporting purposes**.
* Use **BI tools** in the presentation layer to make insights actionable.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Fetch all Sci-Fi movies released after 2010 along with their directors
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Sci-Fi' AND m.release_year > 2010;
```
</details>


### 2. Find the average rating of each movie
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;
```
</details>


### 3. List all actors who acted in movies directed by Christopher Nolan
<details>
<summary>✅ Solution</summary>

```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```
</details>


### 4. Retrieve the top 3 highest-rated movies
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


### 5. Count the number of movies in each genre
<details>
<summary>✅ Solution</summary>

```sql
SELECT genre, COUNT(*) AS total_movies
FROM movies
GROUP BY genre;
```
</details>


---

## 🧠PRACTISE

1. Fetch all movies along with the nationality of their directors.
2. List all actors who acted in “Pulp Fiction”.
3. Find all movies where Leonardo DiCaprio acted and average rating > 9.0.
4. Show the total number of actors per movie.
5. List all movies with their average ratings in descending order.

---

## ✅SOLUTIONS

### 1. Fetch all movies along with the nationality of their directors

```sql
SELECT m.title, d.nationality AS director_nationality
FROM movies m
JOIN directors d ON m.director_id = d.director_id;
```

### 2. List all actors who acted in “Pulp Fiction”

```sql
SELECT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
WHERE m.title = 'Pulp Fiction';
```

### 3. Find all movies where Leonardo DiCaprio acted and average rating > 9.0

```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE a.actor_name = 'Leonardo DiCaprio'
GROUP BY m.title
HAVING AVG(r.rating) > 9.0;
```

### 4. Show the total number of actors per movie

```sql
SELECT m.title, COUNT(ma.actor_id) AS total_actors
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
GROUP BY m.title
ORDER BY total_actors DESC;
```

### 5. List all movies with their average ratings in descending order

```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC;
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
