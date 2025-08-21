# SEMI JOINS, ANTI JOINS, EXISTS, NOT EXISTS

---

## 🔑KEYWORDS

* **Semi Join** 
* **Anti Join** 
* **Exists** 
* **Not Exists** 
* **Subquery** 
* **Additive Join** 

---

## 📖DEFINITION

* **Semi Join** – Selects records from a table where a condition is satisfied in another table. Only rows from the **first table** are returned, no new columns are added.
* **Anti Join** – Selects records from a table where a condition **does NOT match** any row in another table. Only rows from the **first table** are returned.
* **Exists** – Returns TRUE if a subquery returns at least one row; often used as a correlated subquery.
* **Not Exists** – Returns TRUE if a subquery returns **no rows**.
* **Subquery** – Nested query used for filtering
* **Additive Join** – Regular JOIN that adds columns from another table

---

## 🧱QUERY FORMAT

```sql
-- SEMI-JOIN  (IN)
SELECT <columns_from_left_table>
FROM   <left_table>
WHERE  <join_key> IN (
        SELECT <join_key>
        FROM   <right_table>
        [WHERE <optional_filters>]
      );
```
```sql
-- ANTI-JOIN (NOT IN)
SELECT <columns_from_left_table>
FROM   <left_table>
WHERE  <join_key> NOT IN (
        SELECT <join_key>
        FROM   <right_table>
        [WHERE <optional_filters>]
      );
```
```sql
-- EXISTS variant
SELECT <columns>
FROM   <left_table> AS l
WHERE  [NOT] EXISTS (
        SELECT 1
        FROM   <right_table> AS r
        WHERE  r.<join_key> = l.<join_key>
      );
```

---

## 💡TIP TO REMEMBER

* **“Semi keeps, anti skips.”** No extra columns are ever appended—only rows are filtered.
* **Semi join (IN)** → alternative to **SET INTERSECT**
* **Anti join (NOT IN)** → alternative to **SET EXCEPT**
* A semi/anti join is just an IN / NOT IN operation where the right-hand side is a subquery (dynamic list) instead of a hard-coded list of value
* **Exists / Not Exists** → often more efficient for large tables than IN / NOT IN, especially with NULLs
* Difference: SET operations check conditions on **all attributes**, whereas semi/anti joins allow **filtering on limited columns**, making them more flexible.
* **Multiple columns in IN/NOT IN require parentheses**: `(col1, col2)`

---

## 💪EXERCISE

Before moving to the exercises, ensure your platform has the tables and data loaded.
[CLICK AND EXECUTE SETUP FILE](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Retrieve the titles of movies directed by USA directors **released after 2000**.
```sql
SELECT title
FROM movies
WHERE director_id IN (
    SELECT director_id
    FROM directors
    WHERE nationality = 'USA'
)
AND release_year > 2000;
```
### 2. Retrieve titles of movies **not directed by UK directors**.
```sql
SELECT title
FROM movies
WHERE director_id NOT IN (
    SELECT director_id
    FROM directors
    WHERE nationality = 'UK'
);
```
### 3. Retrieve titles of movies where `(director_id, country)` match movies **released after 2000**.
```sql
SELECT title
FROM movies
WHERE (director_id, country) IN (
    SELECT director_id, country
    FROM movies
    WHERE release_year > 2000
);
```
### 4. Retrieve titles of movies where `(director_id, country)` **do not match** movies released after 2010.
```sql
SELECT title
FROM movies
WHERE (director_id, country) NOT IN (
    SELECT director_id, country
    FROM movies
    WHERE release_year > 2010
);
```


---

## 🧠PRACTISE

1. Find actors who acted in movies directed by 'Christopher Nolan'.
2. Find movies **without any ratings above 9.5**.
3. Find directors who did **not direct any Sci-Fi movie**.
4. Retrieve titles of movies **directed by USA directors** using EXISTS.
5. Retrieve titles of movies **not directed by UK directors** using NOT EXISTS.
6. CHECK if you can use EXISTS or NOT EXISTS for above quries
---

## ✅SOLUTIONS

### 1. Find actors who acted in movies directed by 'Christopher Nolan'.
```sql
SELECT a.actor_name
FROM actors a
WHERE a.actor_id IN (
SELECT ma.actor_id
FROM movie_actors ma
JOIN movies m ON ma.movie_id = m.movie_id
JOIN directors d ON m.director_id = d.director_id
WHERE d.director_name = 'Christopher Nolan'
);
```
### 2. Find movies **without any ratings above 9.5**.
```sql
SELECT m.title
FROM movies m
WHERE m.movie_id NOT IN (
SELECT movie_id
FROM movie_ratings
WHERE rating > 9.5
);
```
### 3. Find directors who did **not direct any Sci-Fi movie**.
```sql
SELECT d.director_name
FROM directors d
WHERE NOT EXISTS (
SELECT 1
FROM movies m
WHERE m.director_id = d.director_id
AND m.genre = 'Sci-Fi'
);
```
### 4. Retrieve titles of movies **directed by USA directors** using EXISTS.
```sql
SELECT title
FROM movies m
WHERE EXISTS (
    SELECT 1
    FROM directors d
    WHERE d.director_id = m.director_id
    AND d.nationality = 'USA'
);
```

### 5. Retrieve titles of movies **not directed by UK directors** using NOT EXISTS.
```sql
SELECT title
FROM movies m
WHERE NOT EXISTS (
    SELECT 1
    FROM directors d
    WHERE d.director_id = m.director_id
    AND d.nationality = 'UK'
);
```

---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects
  Open a **pull request** or **issue** to contribute.

---

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
