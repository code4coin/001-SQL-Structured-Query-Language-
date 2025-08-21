# DATA REDUNDANCY, NORMALIZATION AND DENORMALIZATION
---

## 🔑KEYWORDS

* **Normalization** 
* **Denormalization** 
* **Data Redundancy** 

---

## 📖DEFINITION

* **Normalization** – Dividing a large table into smaller tables and defining relationships to minimize duplicate data.
* **Denormalization** – Combining tables or adding redundant data to optimize read performance, sacrificing some storage efficiency.
* **Data Redundancy** – Occurs when the same piece of data exists in multiple places in a database, increasing storage use and potential for inconsistency.

---

## 💡TIP TO REMEMBER

* **Normalization** reduces data redundancy.
* **Denormalization** may increase redundancy but improves query performance.
* Balance between **redundancy** and **performance** is key.

---

## 🧱QUERY FORMAT

1. **Normalized Tables** – split data into related tables.
2. **Denormalized Tables** – combine columns into one table with some redundancy.

```sql
-- Query format to create table

CREATE TABLE table_name (
    column1 datatype [column_constraint],
    column2 datatype [column_constraint],
    ...
    [table_constraint],
    ...
);
```
---

## 💪EXERCISE

Before moving to the exercises, execute the setup file with movie, director, actor, and ratings tables:
[CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Create a **denormalized** table combining movies, directors, and ratings in a single table.

**Solution:**

```sql
CREATE TABLE movie_denormalized AS
SELECT
    m.movie_id,
    m.title,
    m.genre,
    d.director_name,
    d.nationality AS director_nationality,
    m.release_year,
    m.country,
    m.runtime_minutes,
    r.user_name,
    r.rating,
    r.review_date
FROM movies m
JOIN directors d ON m.director_id = d.director_id
JOIN movie_ratings r ON m.movie_id = r.movie_id;
```

### 2. Create **normalized tables** to reduce redundancy for movies, directors, and ratings.

**Solution:**

```sql
-- Movies table
CREATE TABLE normalized_movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    director_id INT,
    release_year INT,
    country VARCHAR(50),
    runtime_minutes INT,
    FOREIGN KEY (director_id) REFERENCES normalized_directors(director_id)
);
```
```sql
-- Directors table
CREATE TABLE normalized_directors (
    director_id INT PRIMARY KEY,
    director_name VARCHAR(100),
    nationality VARCHAR(50)
);
```
```sql
-- Ratings table
CREATE TABLE normalized_ratings (
    rating_id INT PRIMARY KEY,
    movie_id INT,
    user_name VARCHAR(50),
    rating FLOAT,
    review_date DATE,
    FOREIGN KEY (movie_id) REFERENCES normalized_movies(movie_id)
);
```
### 3. Create table professor with columns firstname, lastname (both data type as string)

```sql
CREATE TABLE professor(
firstname text,
lastname text
);
```
### 4. Create a table universities with three text columns: university_shortname, university, and university_city.
```sql
CREATE TABLE universities
(
 university_shortname text,
 university text,
 university_city text
);
```
---

## 🧠PRACTICE

1. Identify **data redundancy** in the denormalized table.
2. Write a query to calculate the **average rating per director** from normalized tables.
3. Add a new rating and observe how normalization affects **redundancy** vs denormalization.

---

## ✅SOLUTIONS

### 1. Identify Data Redundancy in Denormalized Table

```sql
-- Count duplicate director information
SELECT director_name, COUNT(*) AS occurrences
FROM movie_denormalized
GROUP BY director_name
HAVING COUNT(*) > 1;
```

### 2. Average Rating per Director (Normalized Tables)

```sql
SELECT d.director_name, AVG(r.rating) AS avg_rating
FROM normalized_movies m
JOIN normalized_directors d ON m.director_id = d.director_id
JOIN normalized_ratings r ON m.movie_id = r.movie_id
GROUP BY d.director_name;
```

### 3. Add a New Rating

```sql
INSERT INTO normalized_ratings (rating_id, movie_id, user_name, rating, review_date)
VALUES (16, 1, 'user5', 9.3, '2024-05-01');
```

* **Observation:** Only `ratings` table is updated; no redundant director or movie data is repeated.

---

## 🤝**CONTRIBUTING**

We welcome contributions!

* Add new SQL exercises
* Improve examples with normalization/denormalization
* Share interview questions

---

## 📄**LICENSE**

Free to use for learning purposes. Credit the source if used.

---

## 🔗**MORE RESOURCES**

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
