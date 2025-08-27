
# 📌 TABLE PARTITION

---
## 🔑KEYWORDS
- **Partitioning**
- **Vertical Partitioning**
- **Horizontal Partitioning**
- **Declarative Partitioning**
- **Sharding**

---
## 📖DEFINITION
- **Partitioning** – the process of splitting a large table into smaller, more manageable parts without changing the logical structure of the data.  
- **Vertical Partitioning** – splitting a table by columns.  
- **Horizontal Partitioning** – splitting a table by rows.  
- **Sharding** – distributing partitions across multiple machines.  

---
## 🧱QUERY FORMAT
**Vertical Partitioning:**
```sql
CREATE TABLE table_core (...);
CREATE TABLE table_extra (...);
```

**Horizontal Partitioning (PostgreSQL Declarative):**
```sql
CREATE TABLE parent_table (...) PARTITION BY RANGE (column_name);

CREATE TABLE child_partition
    PARTITION OF parent_table
    FOR VALUES FROM (...) TO (...);
```

---
## 💡TIP TO REMEMBER
- Use **vertical partitioning** when some columns are rarely used.  
- Use **horizontal partitioning** when queries are often filtered by ranges (e.g., dates, IDs).  
- Partitioning improves performance, but requires planning for indexes and constraints.  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory:  
👉 [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 1. Vertical Partitioning Example
Split the `movies` table into two:  
- `movies_core` with frequently used columns (`movie_id, title, genre, director_id, release_year, country`)  
- `movies_runtime` with (`movie_id, runtime_minutes`).  

👉 Write SQL queries to:  
a) Create both tables  
b) Insert data into them from the original `movies` table  
c) Retrieve the title and runtime for the movie *Inception*.  

<details>
  <summary>✅ Solution:</summary>

```sql
-- a) Create partitioned tables
CREATE TABLE movies_core (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    director_id INT,
    release_year INT,
    country VARCHAR(50)
);

CREATE TABLE movies_runtime (
    movie_id INT PRIMARY KEY,
    runtime_minutes INT,
    FOREIGN KEY (movie_id) REFERENCES movies_core(movie_id)
);

-- b) Insert data
INSERT INTO movies_core 
SELECT movie_id, title, genre, director_id, release_year, country 
FROM movies;

INSERT INTO movies_runtime 
SELECT movie_id, runtime_minutes 
FROM movies;

-- c) Query for Inception runtime
SELECT m.title, r.runtime_minutes
FROM movies_core m
JOIN movies_runtime r ON m.movie_id = r.movie_id
WHERE m.title = 'Inception';
```
</details>

---

### 2. Horizontal Partitioning Example
Partition `movie_ratings` table by **quarters of 2024** (Q1: Jan–Mar, Q2: Apr–Jun).  
👉 Write SQL queries to create partitions and then query the average rating for Q1 2024.

<details>
  <summary>✅ Solution:</summary>

```sql
-- Parent table
CREATE TABLE ratings_partitioned (
    rating_id INT,
    movie_id INT,
    user_name VARCHAR(50),
    rating FLOAT,
    review_date DATE
) PARTITION BY RANGE (review_date);

-- Q1 2024
CREATE TABLE ratings_q1_2024
    PARTITION OF ratings_partitioned
    FOR VALUES FROM ('2024-01-01') TO ('2024-04-01');

-- Q2 2024
CREATE TABLE ratings_q2_2024
    PARTITION OF ratings_partitioned
    FOR VALUES FROM ('2024-04-01') TO ('2024-07-01');

-- Query Q1 ratings average
SELECT AVG(rating) AS avg_rating_q1
FROM ratings_partitioned
WHERE review_date BETWEEN '2024-01-01' AND '2024-03-31';
```
</details>

---

### 3. Sharding Conceptual Exercise
Suppose you want to distribute `movie_ratings` across multiple machines:  
- Machine A stores Q1 ratings  
- Machine B stores Q2 ratings  

👉 How does this differ from partitioning?  

<details>
  <summary>✅ Solution:</summary>

- Partitioning happens inside **one database instance**.  
- Sharding distributes data across **multiple servers** (nodes).  
- Query engines in sharded systems need to collect results from multiple machines.
</details>

---
## 🧠Practise

1. Find all **Sci-Fi movies released after 2010** using the partitioned `movies_partitioned` table.
2. Find the **average rating of movies directed by Christopher Nolan** for Q1 2024 using the partitioned ratings table.  

---
## ✅SOLUTIONS

### Practice Question 1
```sql
SELECT title, release_year
FROM movies_partitioned
WHERE genre = 'Sci-Fi' AND release_year > 2010;
```

### Practice Question 2
```sql
SELECT d.director_name, AVG(r.rating) AS avg_rating
FROM ratings_partitioned r
JOIN movies m ON r.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan'
  AND r.review_date BETWEEN '2024-01-01' AND '2024-03-31'
GROUP BY d.director_name;
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
- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)  
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)  
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)  

---
