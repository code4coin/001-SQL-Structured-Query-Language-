# 🎬DATA WAREHOUSE DATA MODELLING.
---

## 🔑KEYWORDS

* **Data Modeling**
* **Fact Table**
* **Dimension Table**
* **Star Schema**
* **Snowflake Schema**
* **Movies Dataset**

---

## 📖DEFINITION

* **Data Modeling** – The process of organizing data into structured tables and defining relationships to make querying easier and more meaningful in a data warehouse.
* **Fact Table** – Stores measurable, quantitative data (e.g., ratings).
* **Dimension Table** – Stores descriptive, contextual data (e.g., movie titles, directors, actors).
* **Star Schema** – A central fact table connected directly to dimension tables.
* **Snowflake Schema** – A fact table with dimensions that are further normalized into sub-dimensions.

---

## 🧱QUERY FORMAT

General format of joining **fact** and **dimension** tables:

```sql
SELECT d.dimension_attribute,
       ROUND(AVG(f.measure), 2) AS metric
FROM fact_table f
JOIN dimension_table d
     ON f.key = d.key
GROUP BY d.dimension_attribute
ORDER BY metric DESC;
```

---

## 💡TIP TO REMEMBER

* Fact tables = *numbers (measures)*
* Dimension tables = *words (descriptions)*
* Star schema = *simple, fewer joins, faster queries*
* Snowflake schema = *normalized, more joins, more detailed queries*

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory:
👉 [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 1. Find the **average rating for each movie** and order from highest to lowest.

<details>
  <summary>✅ Solution:</summary>
  

```sql
SELECT m.title, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC;
```
</details>

### 2. Show the **average rating for each director**.

<details>
  <summary>✅ Solution:</summary>
  

```sql
SELECT d.director_name, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
GROUP BY d.director_name
ORDER BY avg_rating DESC;
```
</details>

### 3. Find the **top-rated genre** based on average ratings.

<details>
  <summary>✅ Solution:</summary>
  

```sql
SELECT m.genre, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
GROUP BY m.genre
ORDER BY avg_rating DESC
LIMIT 1;
```
</details>

### 4. Show the **average rating by director nationality** (snowflake example).

<details>
  <summary>✅ Solution:</summary>
  

```sql
SELECT d.nationality, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
GROUP BY d.nationality
ORDER BY avg_rating DESC;
```
</details>

---

## 🧠Practise

### 1. Find all movies where **Leonardo DiCaprio** acted and their **average ratings**.

### 2. Find the **top 3 highest-rated movies released after 2000**.

### 3. Show the **average movie rating by actor nationality**.

---

## ✅SOLUTIONS

### 1. Movies with Leonardo DiCaprio and their average ratings

```sql
SELECT a.actor_name, m.title, ROUND(AVG(r.rating), 2) AS avg_rating
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE a.actor_name = 'Leonardo DiCaprio'
GROUP BY a.actor_name, m.title;
```

### 2. Top 3 highest-rated movies released after 2000

```sql
SELECT m.title, m.release_year, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
WHERE m.release_year > 2000
GROUP BY m.title, m.release_year
ORDER BY avg_rating DESC
LIMIT 3;
```

### 3. Average movie rating by actor nationality

```sql
SELECT a.nationality, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movie_ratings r
JOIN movies m ON r.movie_id = m.movie_id
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
GROUP BY a.nationality
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
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@youtube/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
