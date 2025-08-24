# Referential Integrity: CASCADE, SET NULL, NO ACTION

---

## 🔑KEYWORDS

* **CASCADE** 
* **SET NULL** 
* **NO ACTION**
* **Foreign Key Behavior**

---

## 📖DEFINITION

* Referential integrity ensures that relationships between tables remain consistent.
* Foreign keys can have actions like **CASCADE**, **SET NULL**, or **NO ACTION** to define how deletions or updates propagate to child tables.

---

## 🧱QUERY FORMAT

```sql
-- ON DELETE CASCADE
ALTER TABLE child_table
ADD CONSTRAINT fk_name
FOREIGN KEY (column_name)
REFERENCES parent_table(column_name)
ON DELETE CASCADE;
```
```sql
-- ON DELETE SET NULL
ALTER TABLE child_table
ADD CONSTRAINT fk_name
FOREIGN KEY (column_name)
REFERENCES parent_table(column_name)
ON DELETE SET NULL;
```
```sql
-- ON DELETE NO ACTION
ALTER TABLE child_table
ADD CONSTRAINT fk_name
FOREIGN KEY (column_name)
REFERENCES parent_table(column_name)
ON DELETE NO ACTION;
```

---

## 💡TIP TO REMEMBER

* **CASCADE**: Deletes child rows automatically with the parent.
* **SET NULL**: Preserves child rows but removes parent reference.
* **NO ACTION**: Prevents deletion if child rows exist.
* Always check dependencies before applying CASCADE to avoid accidental data loss.
---

## 🖼 Visual Diagram: Referential Integrity Effects

**1. CASCADE Example**

```
Directors
+-------------+--------------+
| director_id | director_name|
+-------------+--------------+
| 5           | James Cameron|
+-------------+--------------+

Movies
+---------+----------------+
| movie_id| title          |
+---------+----------------+
| 10      | Avatar         |
| 11      | Titanic        |
+---------+----------------+

Action: DELETE James Cameron (ON DELETE CASCADE)

Result:
Movies 'Avatar' and 'Titanic' are deleted automatically.
```

**2. SET NULL Example**

```
Directors
+-------------+--------------+
| director_id | director_name|
+-------------+--------------+
| 1           | Christopher Nolan|
+-------------+--------------+

Movies
+---------+----------------+------------+
| movie_id| title          | director_id|
+---------+----------------+------------+
| 1       | Inception      | 1          |
| 2       | Interstellar   | 1          |
+---------+----------------+------------+

Action: DELETE Christopher Nolan (ON DELETE SET NULL)

Result:
Movies now have director_id = NULL
```

**3. NO ACTION Example**

```
Directors
+-------------+------------------+
| director_id | director_name    |
+-------------+------------------+
| 2           | Steven Spielberg |
+-------------+------------------+

Movies
+---------+----------------+
| movie_id| title          |
+---------+----------------+
| 4       | Jurassic Park  |
| 5       | Schindler's List|
+---------+----------------+

Action: DELETE Steven Spielberg (ON DELETE NO ACTION)

Result:
ERROR: Deletion prevented because movies reference the director.
```

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Display all movies and directors before deletion.
✅ **Solution**
```sql
SELECT m.title, d.director_name
FROM movies m
LEFT JOIN directors d ON m.director_id = d.director_id;
```

### 2. Change `movies.director_id` foreign key to **CASCADE** and delete 'James Cameron'. Observe the effect.
✅ **Solution**
```sql
ALTER TABLE movies
DROP CONSTRAINT movies_director_id_fkey;

ALTER TABLE movies
ADD CONSTRAINT movies_director_id_fkey
FOREIGN KEY (director_id)
REFERENCES directors(director_id)
ON DELETE CASCADE;

DELETE FROM directors WHERE director_name = 'James Cameron';

-- Check result
SELECT * FROM movies; -- 'Avatar' and 'Titanic' deleted automatically
```

### 3. Change `movies.director_id` foreign key to **SET NULL** and delete 'Christopher Nolan'. Observe the effect.
✅ **Solution**
```sql
ALTER TABLE movies
DROP CONSTRAINT movies_director_id_fkey;

ALTER TABLE movies
ADD CONSTRAINT movies_director_id_fkey
FOREIGN KEY (director_id)
REFERENCES directors(director_id)
ON DELETE SET NULL;

DELETE FROM directors WHERE director_name = 'Christopher Nolan';

SELECT * FROM movies WHERE director_id IS NULL; -- Movies now have NULL directors
```

### 4. Change `movies.director_id` foreign key to **NO ACTION** and attempt to delete 'Steven Spielberg'.
✅ **Solution**
```sql
ALTER TABLE movies
DROP CONSTRAINT movies_director_id_fkey;

ALTER TABLE movies
ADD CONSTRAINT movies_director_id_fkey
FOREIGN KEY (director_id)
REFERENCES directors(director_id)
ON DELETE NO ACTION;

DELETE FROM directors WHERE director_name = 'Steven Spielberg';
-- ERROR: Cannot delete due to existing movies
```

### 5. Delete a movie and observe the effect on `movie_ratings` and `movie_actors`.
✅ **Solution**
```sql
-- CASCADE effect on movie_ratings
ALTER TABLE movie_ratings
DROP CONSTRAINT movie_ratings_movie_id_fkey;

ALTER TABLE movie_ratings
ADD CONSTRAINT movie_ratings_movie_id_fkey
FOREIGN KEY (movie_id)
REFERENCES movies(movie_id)
ON DELETE CASCADE;

DELETE FROM movies WHERE title = 'Inception';
SELECT * FROM movie_ratings WHERE movie_id = 1; -- Ratings deleted

-- CASCADE effect on movie_actors
ALTER TABLE movie_actors
DROP CONSTRAINT movie_actors_movie_id_fkey;

ALTER TABLE movie_actors
ADD CONSTRAINT movie_actors_movie_id_fkey
FOREIGN KEY (movie_id)
REFERENCES movies(movie_id)
ON DELETE CASCADE;

DELETE FROM movies WHERE title = 'Pulp Fiction';
SELECT * FROM movie_actors WHERE movie_id = 6; -- Related actors removed
```

---

## 🧠Practise

1. List all movies along with ratings and reviewers.

```sql
SELECT m.title, r.user_name, r.rating
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id;
```

2. Identify movies without any actors assigned.

```sql
SELECT m.title
FROM movies m
LEFT JOIN movie_actors ma ON m.movie_id = ma.movie_id
WHERE ma.actor_id IS NULL;
```

3. Find all actors who acted in Christopher Nolan movies.

```sql
SELECT DISTINCT a.actor_name
FROM actors a
JOIN movie_actors ma ON a.actor_id = ma.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan';
```

4. Show all movies that now have `NULL` directors after deleting a director with SET NULL.

```sql
SELECT title FROM movies WHERE director_id IS NULL;
```

5. Demonstrate CASCADE effect on `movie_actors` when a movie is deleted.

```sql
SELECT * FROM movie_actors WHERE movie_id = 6;
```


---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises demonstrating referential integrity.
* Include more tables and scenarios.
* Suggest better visualization for CASCADE, SET NULL, and NO ACTION effects.

---

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if reused.

---

## 🔗**MORE RESOURCES**

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
