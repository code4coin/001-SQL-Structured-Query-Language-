# THE PRESENTATION LAYER
---

## 🔑KEYWORDS

* **Presentation Layer**
* **Data Warehouse**
* **Dashboarding**
* **Business Intelligence (BI)**
* **SQL Queries**
* **Data Analytics**

---

## 📖DEFINITION

* **Presentation Layer** – The layer in a data warehouse where users access, visualize, and analyze data. It includes tools for automated reporting, dashboards, business intelligence, and direct queries using SQL or programming languages.

---

## 🧱QUERY FORMAT

* SQL queries can be used to retrieve, aggregate, and analyze data from the data warehouse.
* Example:

```sql
SELECT title, genre, release_year
FROM movies
WHERE release_year > 2010;
```

---

## 💡TIP TO REMEMBER

* Use **automated reporting tools** (Tableau, Power BI) for quick insights.
* Use **BI/data analytics tools** (KNIME, RapidMiner) to explore patterns.
* Use **direct queries** with SQL, Python, or R for custom analysis.
* Always **join related tables** to enrich insights (e.g., movies + directors + ratings).

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Retrieve all movies released after 2010 with their genre and director name.
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, m.genre, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year > 2010;
```
</details>

### 2. Find the average rating for each genre.
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.genre, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.genre;
```
</details>

### 3. List all actors who acted in "Inception".
<details>
<summary>✅ Solution</summary>

```sql
SELECT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
WHERE m.title = 'Inception';
```
</details>

### 4. Identify the top 3 highest-rated movies.
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
</details>

### 5. Find all movies directed by "Christopher Nolan" with their average rating.
<details>
<summary>✅ Solution</summary>

```sql
SELECT m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE d.director_name = 'Christopher Nolan'
GROUP BY m.title;
```
</details>

---

## 🧠Practise

1. List all directors along with the number of movies they directed.
2. Find the actor who has acted in the most movies.
3. Show all movies with their director and rating, sorted by rating descending.
4. Retrieve all Sci-Fi movies released before 2015.
5. Find all movies starring "Brad Pitt" and their directors.

---

## ✅SOLUTIONS

1. List all directors along with the number of movies they directed.

```sql
SELECT d.director_name, COUNT(m.movie_id) AS num_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```

2. Find the actor who has acted in the most movies.

```sql
SELECT a.actor_name, COUNT(ma.movie_id) AS num_movies
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name
ORDER BY num_movies DESC
LIMIT 1;
```

3. Show all movies with their director and rating, sorted by rating descending.

```sql
SELECT m.title, d.director_name, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title, d.director_name
ORDER BY avg_rating DESC;
```

4. Retrieve all Sci-Fi movies released before 2015.

```sql
SELECT title, release_year
FROM movies
WHERE genre = 'Sci-Fi' AND release_year < 2015;
```

5. Find all movies starring "Brad Pitt" and their directors.

```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
WHERE a.actor_name = 'Brad Pitt';
```

---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects

## Please open a **pull request** or **issue** to contribute.

## 📄**LICENSE**

## This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
