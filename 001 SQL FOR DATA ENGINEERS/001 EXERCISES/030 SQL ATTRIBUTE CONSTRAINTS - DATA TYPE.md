# ATTRIBUTE CONSTRAINTS - DATA TYPE in PostgreSQL
---
## 🔑KEYWORDS
- **PostgreSQL**
- **Data Types**
- **VARCHAR**
- **BOOLEAN**
- **NUMERIC**
- **DATE**
- **ALTER TABLE ... TYPE ... USING**
- **USING clause**
- **DOMAIN**
---
## 📖DEFINITION
- **Data Types** – Define the kind of values a column can store (text, number, date, etc.), enforce consistency, and ensure meaningful operations on data.
- **DOMAIN** – the set of permissible values determined by the data type.
- **ALTER TABLE … TYPE ... USING** – command used to change a column’s data type after table creation.
- **USING clause** – optional clause in ALTER TABLE that tells PostgreSQL how to convert old values into the new type.
---
## 🧱QUERY FORMAT
```sql
CREATE TABLE table_name (
    column_name DATA_TYPE [constraint],
    ...
);

-- Changing column data type
ALTER TABLE table_name
ALTER COLUMN column_name TYPE NEW_DATA_TYPE
USING transformation_expression;
```
---
## 💡TIP TO REMEMBER
- **“Type first, data second.”** Pick the narrowest type that still fits every future value; widening is easy, shrinking is painful.
- **VARCHAR(n)**: Stores strings up to `n` characters.  
- **BOOLEAN**: Stores `TRUE`, `FALSE`, or `NULL`.  
- **NUMERIC(p, s)**: Precise decimal values with total `p` digits and `s` digits after the decimal point.  
- **DATE**: Stores calendar dates.  
- Always choose the **smallest sufficient type** for efficiency.  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 1. Find all movies longer than 150 minutes and mark them as long movies (Boolean check).
✅Solution
```sql
SELECT title,
       runtime_minutes > 150 AS is_long_movie
FROM movies;
```
---

### 2. Show the top 3 movies with the highest average ratings (rounded to 2 decimals).
✅Solution
```sql
SELECT m.title, ROUND(AVG(r.rating), 2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```
---

### 3. List all reviews made between January and March 2024.
✅Solution
```sql
SELECT user_name, movie_id, rating, review_date
FROM movie_ratings
WHERE review_date BETWEEN '2024-01-01' AND '2024-03-31';
```
---

### 4. Change the `user_name` column in `movie_ratings` to allow 100 characters.
✅Solution
```sql
ALTER TABLE movie_ratings
ALTER COLUMN user_name TYPE VARCHAR(100);
```

---

### 5. Convert `rating` column to NUMERIC(3,2) with 2 decimal precision.
✅Solution
```sql
ALTER TABLE movie_ratings
ALTER COLUMN rating TYPE NUMERIC(3,2)
USING ROUND(rating, 2);
```

---
## 🧠Practise
1. Find all actors whose name length is greater than 10 characters.  
2. List all movies released between 2000 and 2010.  
3. Show each director and the number of movies they directed.  
4. Retrieve movies and their average ratings only if the average is greater than 9.0.  
5. Find all reviews where rating is a whole number (integer).  
6. Display movies along with whether they were released before or after the year 2000 (Boolean check).  

---
## ✅SOLUTIONS

### 1. Find all actors whose name length is greater than 10 characters.
```sql
SELECT actor_name
FROM actors
WHERE LENGTH(actor_name) > 10;
```

### 2. List all movies released between 2000 and 2010.
```sql
SELECT title, release_year
FROM movies
WHERE release_year BETWEEN 2000 AND 2010;
```

### 3. Show each director and the number of movies they directed.
```sql
SELECT d.director_name, COUNT(m.movie_id) AS movie_count
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```

### 4. Retrieve movies and their average ratings only if the average is greater than 9.0.
```sql
SELECT m.title, ROUND(AVG(r.rating),2) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
HAVING AVG(r.rating) > 9.0;
```

### 5. Find all reviews where rating is a whole number (integer).
```sql
SELECT *
FROM movie_ratings
WHERE rating = FLOOR(rating);
```

### 6. Display movies along with whether they were released before or after the year 2000.
```sql
SELECT title,
       release_year < 2000 AS before_2000
FROM movies;
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
