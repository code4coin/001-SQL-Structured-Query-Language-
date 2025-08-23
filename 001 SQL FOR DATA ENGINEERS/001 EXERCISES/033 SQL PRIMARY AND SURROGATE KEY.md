# PRIMARY AND SURROGATE KEY
---

## 🔑KEYWORDS

* **Primary Key**
* **Surrogate Key**

---

## 📖DEFINITION

- **Primary Key** – A column (or set of columns) whose values uniquely identify every row and may **not** contain NULL or duplicates.  
- **Surrogate Key** – An **artificial**, auto-generated identifier (often `SERIAL`/`IDENTITY`) that has **no business meaning**.  
- **Composite Key** – A primary key made of **more than one column** taken together.  
- **Natural Key** – A primary key formed from **existing business data** (e.g., license number).  
- **Candidate Key** – Any minimal set of columns that **could** become the primary key.
---

## 🧱QUERY FORMAT

```sql
-- Adding a composite natural key
ALTER TABLE <table_name>
ADD CONSTRAINT <pk_name>
    PRIMARY KEY (col1, col2, ...);
```
```sql
-- Adding a surrogate key (PostgreSQL)
ALTER TABLE <table_name>
ADD COLUMN id SERIAL PRIMARY KEY;
```
```sql
-- Adding a surrogate key to an existing table (manual)
CREATE TABLE new_table (
    id SERIAL PRIMARY KEY,
    ...                     -- copy other columns
);
INSERT INTO new_table (...)
SELECT ... FROM old_table;
```
---

## 💡TIP TO REMEMBER

* **“Small, stable, single-column – if you can’t tick all three, reach for a surrogate key.”**
* Primary keys must be **unique and not null**.
* Composite keys are best used when a single column cannot guarantee uniqueness.
* Surrogate keys help maintain a stable, unchanging identifier for each row, especially useful when natural attributes may change.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Add a primary key to the `movie_ratings` table if it doesn’t exist.
✅Solution
```sql
ALTER TABLE movie_ratings
ADD CONSTRAINT pk_rating PRIMARY KEY (rating_id);
```
### 2. Identify which columns could serve as a composite primary key in `movie_actors`.
✅Solution
```sql
-- Already defined as (movie_id, actor_id)
SELECT * FROM movie_actors;
```
### 3. Add a surrogate key `rating_id` to the `movie_ratings` table using `SERIAL`.
✅Solution
```sql
ALTER TABLE movie_ratings
ADD COLUMN rating_id SERIAL PRIMARY KEY;
```
### 4. Write a query to list all movies with their directors and ensure uniqueness using primary keys.
✅Solution
```sql
SELECT m.movie_id, m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id;
```
### 5. Add a surrogate key `movie_actor_id` to the `movie_actors` table and populate it.
✅Solution
```sql
ALTER TABLE movie_actors
ADD COLUMN movie_actor_id SERIAL PRIMARY KEY;

SELECT movie_actor_id, movie_id, actor_id FROM movie_actors;
```
---

## 🧠Practise

1. Query all movies released after 2010, showing their movie_id and title.
2. Query all actors who acted in 'Inception'.
3. Insert a new movie using a surrogate key.
4. Update `movie_actors` to add a surrogate key and display all actor-movie combinations.
5. Find the average rating for each movie and ensure no duplicates using primary key constraints.

---

## ✅SOLUTIONS


### 1. Query all movies released after 2010, showing their movie_id and title.
```sql
SELECT movie_id, title
FROM movies
WHERE release_year > 2010;
```

### 2. Query all actors who acted in 'Inception'.
```sql
SELECT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
WHERE m.title = 'Inception';
```

### 3. Insert a new movie using a surrogate key.
```sql
INSERT INTO movies (title, genre, director_id, release_year, country, runtime_minutes)
VALUES ('Tenet', 'Sci-Fi', 1, 2020, 'USA', 150);
```

### 4. Update `movie_actors` to add a surrogate key and display all actor-movie combinations.
```sql
SELECT movie_actor_id, movie_id, actor_id
FROM movie_actors;
```

### 5. Find the average rating for each movie and ensure no duplicates using primary key constraints.
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id, m.title;
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
