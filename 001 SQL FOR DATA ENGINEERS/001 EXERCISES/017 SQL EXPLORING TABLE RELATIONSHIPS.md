# EXPLORING TABLE RELATIONSHIPS
## 🔑KEYWORDS
- **One-to-Many Relationship**
- **One-to-One Relationship**
- **Many-to-Many Relationship**
- **Primary Key**
- **Foreign Key**
- **Bridge / Junction Table**
---
## 📖DEFINITION  
- **Table relationship** – The way in which data/record in one table is connected to data/record in another. Relationships are defined using **primary keys** (unique identifiers) and **foreign keys** (columns that reference primary keys in another table).
- **One-to-Many** – one director → many movies  
- **Many-to-Many** – many movies ↔ many actors  
- **Junction Table** – `movie_actors` resolves M:N (many-to-many) relationships  
- **Foreign Key** – links child rows to parent rows  
- **Normalization** – splitting data into related tables to reduce redundancy  
- **Cardinality** – how many rows on each side of the relationship (1:1, 1:N, M:N).
- **Order of Execution**

	| Step | Clause             | Mnemonic | Typical Purpose                                   |
	| ---- | ------------------ | -------- | ------------------------------------------------- |
	| 1    | `FROM` + `JOIN`    | **F**    | Build the raw data set (cartesian product → join) |
	| 2    | `WHERE`            | **W**    | Row-level filtering (before grouping)             |
	| 3    | `GROUP BY`         | **G**    | Form groups                                       |
	| 4    | `HAVING`           | **H**    | Group-level filtering (after aggregation)         |
	| 5    | `SELECT`           | **S**    | Compute columns & aliases                         |
	| 6    | `DISTINCT`         | -        | Remove duplicate rows                             |
	| 7    | `ORDER BY`         | **O**    | Sort the final projection                         |
	| 8    | `LIMIT` / `OFFSET` | **L**    | Slice the sorted result set                       |

- **FROM → WHERE → GROUP → HAVING → SELECT → ORDER → LIMIT**

   **F W G H S O L ≈ “Frodo Went Ghost Hunting, Sam Ordered Lembas.”**

---
## 🧱QUERY FORMAT
```sql
-- 1-to-Many: list every movie by a given director
SELECT m.title
FROM   movies m
JOIN   directors d ON m.director_id = d.director_id
WHERE  d.director_name = 'Christopher Nolan';
```
```sql
-- Many-to-Many: list all actors in a movie
SELECT a.actor_name
FROM   actors a
JOIN   movie_actors ma ON a.actor_id = ma.actor_id
WHERE  ma.movie_id = 3;
```
```sql
-- Aggregated M:N: average rating per movie
SELECT m.title,
       ROUND(AVG(r.rating), 2) AS avg_rating
FROM   movies m
JOIN   movie_ratings r ON m.movie_id = r.movie_id
GROUP  BY m.title;
```
---
## 💡TIP TO REMEMBER
- Think of relationships as LEGO bricks:
    - 1×N bricks stack vertically (1 director → N movies).
    - M×N plates need a special connector (junction table) to lock together.
- If you see a table that contains two foreign keys (and they reference two different parent tables), it is acting as a bridge table → the relationship is many-to-many.
- If a table contains just one foreign key directly referencing another table (and no bridge table is used), it is almost always a one-to-many relationship.

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Write a query to list all movies and their actors.
```sql
SELECT d.director_name, m.title
FROM directors d
JOIN movies m ON d.director_id = m.director_id;
```
### 2. Write a query to show each movie and its average rating.
```sql
SELECT m.title, a.actor_name
FROM movies m
JOIN movie_actors ma ON m.movie_id = ma.movie_id
JOIN actors a        ON ma.actor_id = a.actor_id;
```
### 3. Find all actors who worked with Christopher Nolan.
```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m        ON ma.movie_id = m.movie_id
JOIN directors d     ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```
---
## 🧠Practise
1. Identify & Count Relationships: Write a query that returns:
director name
number of movies directed
average runtime of those movies

3. Actor Collaboration Matrix: Find every pair of actors who have worked together in at least one movie.
Return: actor 1 name, actor 2 name, movie title.
4. High-Rated Genre Snapshot: List all genres with an average rating ≥ 8.0 based on user reviews.
Return: genre, average rating, total reviews.
---
## ✅SOLUTIONS
### 1. Identify & Count Relationships
Write a query that returns:
director name
number of movies directed
average runtime of those movies
```sql
SELECT d.director_name,
       COUNT(m.movie_id)        AS movie_cnt,
       ROUND(AVG(m.runtime_minutes), 1) AS avg_runtime
FROM   directors d
JOIN   movies m ON d.director_id = m.director_id
GROUP  BY d.director_name;
```
### 2. Actor Collaboration Matrix
Find every pair of actors who have worked together in at least one movie.
Return: actor 1 name, actor 2 name, movie title.
```sql
SELECT DISTINCT
       a1.actor_name AS actor_1,
       a2.actor_name AS actor_2,
       m.title
FROM   movie_actors ma1
JOIN   movie_actors ma2  ON ma1.movie_id = ma2.movie_id
                           AND ma1.actor_id < ma2.actor_id
JOIN   actors a1         ON ma1.actor_id = a1.actor_id
JOIN   actors a2         ON ma2.actor_id = a2.actor_id
JOIN   movies m          ON m.movie_id = ma1.movie_id
ORDER  BY m.title, actor_1, actor_2;
```
The previous solution works well **only when IDs are in sequential order**.  

Example:

| movie_id | actor_id |
|----------|----------|
| 1        | 1        |
| 1        | 2        |
| 1        | 3        |

- Using the logic with `>` operator:  
  - Record 1 matches with 2 and 3  
  - Record 2 matches only with 3 (ignores 1 due to `>` logic)  

✅ Works because IDs are **sequential**.

---

### ❌ Issue with Non-Sequential IDs

If IDs are **not sequential**:

| movie_id | actor_id |
|----------|----------|
| 1        | a1       |
| 1        | g23      |
| 1        | wd3      |

- Using the same logic now results in **duplicate pairs**:

| movie_id | actor1 | actor2 |
|----------|--------|--------|
| 1        | a1     | g23    |
| 1        | a1     | wd3    |
| 1        | g23    | a1     |
| 1        | wd3    | a1     |

- Swapping positions creates duplicates.  
- Problem: **pair of actors are duplicated** because logic relies on sequential IDs.

---

## 🔹 Solution Approach

1. **Select only one record per movie** from `movie_actors` using `ROW_NUMBER()`:
```sql
WITH one_actor_per_movie AS (
    SELECT
        movie_id,
        actor_id,
        ROW_NUMBER() OVER (PARTITION BY movie_id ORDER BY actor_id) AS rn
    FROM movie_actors
)
SELECT *
FROM one_actor_per_movie
WHERE rn = 1;
```
Processed Table (one record per movie):
| movie_id | actor_id |
|----------|----------|
| 1        | a1       |

Step 2: Join back with original movie_actors table
```sql
SELECT o.movie_id, o.actor_id AS actor1, m.actor_id AS actor2
FROM one_actor_per_movie o
INNER JOIN movie_actors m
    ON o.movie_id = m.movie_id
    AND o.actor_id <> m.actor_id;
```
Resulting Pairs Table:
| movie_id | actor1 | actor2 |
|----------|--------|--------|
| 1        | a1     | g23    |
| 1        | a1     | wd3    |

FULL SOLUTION
```sql

SELECT MOVIES.TITLE,
ACT_01.ACTOR_NAME,
ACT_02.ACTOR_NAME
FROM (SELECT MOVIE_ACTORS_02.MOVIE_ID,
MOVIE_ACTORS_02.ACTOR_ID AS ACTOR_ID_01,
MOVIE_ACTORS.ACTOR_ID AS ACTOR_ID_02
FROM (SELECT MOVIE_ID, ACTOR_ID
FROM (SELECT ROW_NUMBER() OVER (PARTITION BY MOVIE_ID) AS ROW_NUM,
MOVIE_ID,
ACTOR_ID
      FROM MOVIE_ACTORS) AS MOVIE_ACTORS_01
      WHERE ROW_NUM = 1) AS MOVIE_ACTORS_02
      INNER JOIN MOVIE_ACTORS 
      ON MOVIE_ACTORS_02.MOVIE_ID = MOVIE_ACTORS.MOVIE_ID
      AND MOVIE_ACTORS_02.ACTOR_ID <> MOVIE_ACTORS.ACTOR_ID ) AS MOVIE_ACTORS_03
      INNER JOIN MOVIES ON MOVIE_ACTORS_03.MOVIE_ID = MOVIES.MOVIE_ID
      INNER JOIN ACTORS AS ACT_01 ON MOVIE_ACTORS_03.ACTOR_ID_01 = ACT_01.ACTOR_ID
INNER JOIN ACTORS AS ACT_02 ON MOVIE_ACTORS_03.ACTOR_ID_02 = ACT_02.ACTOR_ID;
```

### 3. High-Rated Genre Snapshot
List all genres with an average rating ≥ 8.0 based on user reviews.
Return: genre, average rating, total reviews.
```sql
SELECT m.genre,
       ROUND(AVG(r.rating), 2) AS avg_rating,
       COUNT(*)                AS total_reviews
FROM   movies m
JOIN   movie_ratings r ON m.movie_id = r.movie_id
GROUP  BY m.genre
HAVING AVG(r.rating) >= 8.0;
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
