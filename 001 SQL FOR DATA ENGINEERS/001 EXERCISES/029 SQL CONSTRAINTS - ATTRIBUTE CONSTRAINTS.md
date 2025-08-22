# 🎬 Attribute Constraints (Data Types) in SQL
---
## 🔑KEYWORDS
- **Attribute Constraints**
- **Data Types**
- **Data Quality**
- **Integrity**
- **Validation**
---
## 📖DEFINITION
- **Attribute Constraints** – Rules applied at the column level to ensure valid data entry.  
- **Data Types** – Define what kind of values can be stored in a column (e.g., `INT`, `VARCHAR`, `DATE`, `FLOAT`).  
- **Purpose** – Improve **data quality**, maintain **consistency**, and prevent **invalid inputs**.  
---
## 🧱QUERY FORMAT
```sql
-- General syntax for creating a table with attribute constraints
CREATE TABLE table_name (
    column_name DATA_TYPE CONSTRAINTS
);
```
```sql
-- Example with constraints
CREATE TABLE movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    release_year INT CHECK (release_year > 1800), 
    runtime_minutes INT CHECK (runtime_minutes > 0)
);
```
---
## 💡TIP TO REMEMBER
- **Always choose the right data type**:  
  - Use `INT` for whole numbers (e.g., release year).  
  - Use `VARCHAR(n)` for variable-length text (e.g., movie title).  
  - Use `DATE` for proper date values (e.g., review_date).  
- **Constraints prevent bad data before it even enters the database.**  
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Insert a new valid rating
Insert a rating of **8.7** for movie_id `2` (`Interstellar`) by user `user6` on `2024-05-10`.

✅ **Solution:**
```sql
INSERT INTO movie_ratings (rating_id, movie_id, user_name, rating, review_date)
VALUES (16, 2, 'user6', 8.7, '2024-05-10');
```

---

### 2. Try inserting invalid data (should fail)
Insert a movie with text `"year2020"` as release_year.  

✅ **Solution:**
```sql
INSERT INTO movies (movie_id, title, genre, director_id, release_year, country, runtime_minutes)
VALUES (20, 'Invalid Movie', 'Drama', 1, 'year2020', 'USA', 120);
-- ❌ This will fail because 'year2020' is not an INT
```

---

### 3. Add a constraint on ratings
Ensure ratings must be between **0 and 10**. Then test it by inserting a rating of 15.

✅ **Solution:**
```sql
ALTER TABLE movie_ratings
ADD CONSTRAINT rating_check CHECK (rating >= 0 AND rating <= 10);

-- Test with invalid rating
INSERT INTO movie_ratings (rating_id, movie_id, user_name, rating, review_date)
VALUES (17, 1, 'user7', 15, '2024-05-01');
-- ❌ Fails because 15 > 10
```

---

### 4. Find the average runtime of Sci-Fi movies
Check how attribute constraints ensure only valid numeric runtimes are considered.

✅ **Solution:**
```sql
SELECT genre, AVG(runtime_minutes) AS avg_runtime
FROM movies
WHERE genre = 'Sci-Fi'
GROUP BY genre;
```

---

## 🧠Practise
1. Insert a valid new movie with `movie_id=21`, title `"Oppenheimer"`, genre `"Drama"`, director_id `1`, release_year `2023`, country `"USA"`, runtime `180`.  

2. Try inserting a movie with a runtime of `-120`. (Should fail after adding constraint.)  

3. Update the movie `"Titanic"` runtime to `NULL`. What happens?  

4. Find the **earliest release year** of any movie in the database.  

5. Retrieve all movies where the `release_year` is greater than 2010 and less than 2020.  

---
## ✅SOLUTIONS
### Practise
1. Insert a valid new movie with `movie_id=21`, title `"Oppenheimer"`, genre `"Drama"`, director_id `1`, release_year `2023`, country `"USA"`, runtime `180`.  
```sql
INSERT INTO movies (movie_id, title, genre, director_id, release_year, country, runtime_minutes)
VALUES (21, 'Oppenheimer', 'Drama', 1, 2023, 'USA', 180);
```

2. Try inserting a movie with a runtime of `-120`. (Should fail after adding constraint.)  
```sql
INSERT INTO movies (movie_id, title, genre, director_id, release_year, country, runtime_minutes)
VALUES (22, 'Invalid Runtime', 'Action', 3, 2022, 'USA', -120);
-- ❌ Fails due to runtime constraint (must be > 0)
```

3. Update the movie `"Titanic"` runtime to `NULL`. What happens?   
```sql
UPDATE movies
SET runtime_minutes = NULL
WHERE title = 'Titanic';
-- ✅ Succeeds unless "NOT NULL" constraint is added.
```

4. Find the **earliest release year** of any movie in the database.  
```sql
SELECT MIN(release_year) AS earliest_release
FROM movies;
```

5. Retrieve all movies where the `release_year` is greater than 2010 and less than 2020.  
```sql
SELECT title, release_year
FROM movies
WHERE release_year > 2010 AND release_year < 2020;
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
