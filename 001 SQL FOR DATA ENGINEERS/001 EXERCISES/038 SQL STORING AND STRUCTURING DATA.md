# STORING AND STRUCTURING DATA
---
## 🔑KEYWORDS
- Structured Data
- Unstructured Data
- Semi-Structured Data
- Data Warehouse
- Data Lake
- ETL (Extract, Transform, Load)
- ELT (Extract, Load, Transform)

---
## 📖DEFINITION
- **Structured Data** – Clean, organized data following strict schemas for easy querying  
- **Unstructured Data** – Raw, non-schema data that is flexible but harder to analyze  
- **Semi-Structured Data** – Data with ad-hoc self-describing structure, balancing flexibility and order  
- **Data Warehouse** – Central repository for analytics from multiple sources, optimized for query performance  
- **Data Lake** – Storage system for huge volumes of raw data, allows schema-on-read  
- **ETL** – Traditional data flow method; transform before loading  
- **ELT** – Modern data flow method; load before transforming  

---
## 🧱QUERY FORMAT
> Note: These are sample SQL formats for interacting with structured data in relational databases.

```sql
-- Select all columns from a table
SELECT * FROM table_name;

-- Filter data with WHERE
SELECT column1, column2 
FROM table_name 
WHERE condition;

-- Insert data into a table
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);

-- Update existing data
UPDATE table_name
SET column1 = value1
WHERE condition;

-- Delete data from table
DELETE FROM table_name
WHERE condition;
```

---
## 💡TIP TO REMEMBER
- Structured data → Easier analysis, less flexible  
- Unstructured data → More flexible, requires preprocessing  
- Semi-structured → Balanced, common in NoSQL  
- Warehouses → Optimized for queries, denormalized  
- Lakes → Optimized for scale and cost, schema-on-read  
- ETL → Transform before load, ELT → Load then transform  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### Exercises with Solutions

1. List all movies released after 2010. 

✅ Solution:
```sql
SELECT * FROM movies
WHERE release_year > 2010;
```

2. Count the total number of movies per genre. 

✅ Solution:
```sql
SELECT genre, COUNT(*) AS total_movies
FROM movies
GROUP BY genre;
```

3. Find movies with rating above 8.5.

✅ Solution:
```sql
SELECT title, rating
FROM movies
WHERE rating > 8.5;
```

4. Insert a new movie record into the table. 

✅ Solution:
```sql
INSERT INTO movies (title, genre, release_year, rating)
VALUES ('New Movie', 'Action', 2025, 7.8);
```

5. Update the rating of a specific movie. 

✅ Solution:
```sql
UPDATE movies
SET rating = 9.0
WHERE title = 'Inception';
```

6. Delete movies released before 2000. 

✅ Solution:
```sql
DELETE FROM movies
WHERE release_year < 2000;
```

7. List top 5 highest-rated movies. 

✅ Solution:
```sql
SELECT * FROM movies
ORDER BY rating DESC
LIMIT 5;
```

8. Find all movies in the 'Action' genre released after 2015. 

✅ Solution:
```sql
SELECT * FROM movies
WHERE genre = 'Action' AND release_year > 2015;
```

9. Count movies per year and order descending. 

✅ Solution:
```sql
SELECT release_year, COUNT(*) AS total_movies
FROM movies
GROUP BY release_year
ORDER BY total_movies DESC;
```

10. Find average rating per genre. 

✅ Solution:
```sql
SELECT genre, AVG(rating) AS avg_rating
FROM movies
GROUP BY genre;
```

---
## 🧠Practise
### Practice Questions

1. List all movies released after 2010.
2. Count the total number of movies per genre.
3. Find movies with rating above 8.5.
4. Insert a new movie record into the table.
5. Update the rating of a specific movie.
6. Delete movies released before 2000.
7. List top 5 highest-rated movies.
8. Find all movies in the 'Action' genre released after 2015.
9. Count movies per year and order descending.
10. Find average rating per genre.

### Solutions
```sql
-- 1. List all movies released after 2010
SELECT * FROM movies
WHERE release_year > 2010;
```
```sql
-- 2. Count the total number of movies per genre
SELECT genre, COUNT(*) AS total_movies
FROM movies
GROUP BY genre;
```
```sql
-- 3. Find movies with rating above 8.5
SELECT title, rating
FROM movies
WHERE rating > 8.5;
```
```sql
-- 4. Insert a new movie record into the table
INSERT INTO movies (title, genre, release_year, rating)
VALUES ('New Movie', 'Action', 2025, 7.8);
```
```sql
-- 5. Update the rating of a specific movie
UPDATE movies
SET rating = 9.0
WHERE title = 'Inception';
```
```sql
-- 6. Delete movies released before 2000
DELETE FROM movies
WHERE release_year < 2000;
```
```sql
-- 7. List top 5 highest-rated movies
SELECT * FROM movies
ORDER BY rating DESC
LIMIT 5;
```
```sql
-- 8. Find all movies in the 'Action' genre released after 2015
SELECT * FROM movies
WHERE genre = 'Action' AND release_year > 2015;
```
```sql
-- 9. Count movies per year and order descending
SELECT release_year, COUNT(*) AS total_movies
FROM movies
GROUP BY release_year
ORDER BY total_movies DESC;
```
```sql
-- 10. Find average rating per genre
SELECT genre, AVG(rating) AS avg_rating
FROM movies
GROUP BY genre;
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
