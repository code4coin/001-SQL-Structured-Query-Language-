# INTRODUCTION TO RELATIONAL DATABASES

---
## 🔑KEYWORDS
- **Relational Database** – Organizes data into related tables.
- **Meta Database**  
- **Primary Key**
- **Foreign Key** 
- **Join** 
- **Referential Integrity**

---
## 📖DEFINITION
- **Primary Key** – A unique identifier for each record in a table.
- **Meta Database** - store information about all the databases in the storage or system. Example information_schema in PostgreSQL
- **Foreign Key** – A reference linking data across tables.  
- **Join** – Combines rows from two or more tables based on related columns.  
- **Referential Integrity** – Ensures relationships between tables remain consistent. 
- **Relational Database** – A collection of tables connected by relationships, used to efficiently store and query structured data.  
- **Normalization** – Organizing data into separate tables to reduce redundancy.  
- **Schema** – The structure of the database: tables, columns, and relationships.  

---
## 🧱QUERY FORMAT
Basic SQL structure for querying relational databases:  

```sql
SELECT <columns>
FROM   <table>
JOIN   <other_table> ON <join_condition>
WHERE  <filter_conditions>
GROUP  BY <columns>
HAVING <aggregate_filter>
ORDER  BY <columns>;
```

---

## 💡TIP TO REMEMBER
- Always identify **entities** (movies, directors, actors) before designing tables.  
- Use **JOINs** to combine related data.  
- Use **GROUP BY** with aggregate functions (`AVG`, `COUNT`, `SUM`) for summaries.  
- Ensure **primary keys** are unique and **foreign keys** are consistent.
- **Meta Database** - Every database system has catalog where they store all information regarding databases, table, columns etc usually referred as **meta database**
  Postgresql's called **information_schema**
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 🔎 INFORMATION_SCHEMA EXERCISES  

#### 1. List all tables in the current database.  
```sql
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public';
```

#### 2. Show all columns of the `movies` table with their data types.  
```sql
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = 'movies';
```

#### 3. Find all foreign key constraints in the schema.  
```sql
SELECT constraint_name, table_name, column_name
FROM information_schema.key_column_usage
WHERE position_in_unique_constraint IS NOT NULL;
```

#### 4. Check which columns are nullable in the `actors` table.  
```sql
SELECT column_name, is_nullable
FROM information_schema.columns
WHERE table_name = 'actors';
```

#### 5. Find the primary keys of all tables.  
```sql
SELECT kcu.table_name, kcu.column_name
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kcu
  ON tc.constraint_name = kcu.constraint_name
WHERE tc.constraint_type = 'PRIMARY KEY';
```
---
### 🔎 EXERCISES - TSTING PREVIOUSLY LEARNT CONCEPTS  

### 1. List all movies directed by **Christopher Nolan**.
```sql
SELECT m.title, m.release_year
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```
---

### 2. Find all movies in the **Sci-Fi** genre along with their directors.
```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Sci-Fi';
```
---

### 3. Find all movies in which **Leonardo DiCaprio** has acted.
```sql
SELECT m.title, m.release_year
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
WHERE a.actor_name = 'Leonardo DiCaprio';
```
---

### 4. Get the **average rating** of the movie *Titanic*.
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.title = 'Titanic'
GROUP BY m.title;
```
---

### 5. Find the **highest-rated movie** in the database.
```sql
SELECT m.title, MAX(r.rating) AS highest_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY highest_rating DESC
LIMIT 1;
```
---

## 🧠Practise
Try these on your own first 👇  

1. List all **Action movies** and their directors.  
2. Find all movies where **Brad Pitt** has acted.  
3. Show the **average rating** of each Sci-Fi movie.  
4. List movies released **before 2000** with their directors.  
5. Find the **top 3 highest-rated movies**.  
6. Show how many movies each **director** has directed.  
7. List all actors who have worked with **Steven Spielberg**.  

---

## ✅SOLUTIONS
Here are the SQL queries for the practice problems:  

### 1. List all Action movies and their directors.
```sql
SELECT m.title, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.genre = 'Action';
```

---

### 2. Find all movies where Brad Pitt has acted.
```sql
SELECT m.title, m.release_year
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a ON ma.actor_id = a.actor_id
WHERE a.actor_name = 'Brad Pitt';
```

---

### 3. Show the average rating of each Sci-Fi movie.
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
WHERE m.genre = 'Sci-Fi'
GROUP BY m.title;
```

---

### 4. List movies released before 2000 with their directors.
```sql
SELECT m.title, m.release_year, d.director_name
FROM movies m
JOIN directors d ON m.director_id = d.director_id
WHERE m.release_year < 2000;
```

---

### 5. Find the top 3 highest-rated movies.
```sql
SELECT m.title, AVG(r.rating) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
ORDER BY avg_rating DESC
LIMIT 3;
```

---

### 6. Show how many movies each director has directed.
```sql
SELECT d.director_name, COUNT(m.movie_id) AS total_movies
FROM directors d
JOIN movies m ON d.director_id = m.director_id
GROUP BY d.director_name;
```

---

### 7. List all actors who have worked with Steven Spielberg.
```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Steven Spielberg';
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

---
