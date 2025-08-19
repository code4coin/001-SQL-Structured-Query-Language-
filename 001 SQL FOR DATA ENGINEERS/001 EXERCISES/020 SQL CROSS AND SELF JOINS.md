# CROSS AND SELF JOINS
---
## 🔑KEYWORDS
- **CROSS JOIN**
- **CARTESIAN PRODUCT**
- **Self Join**
- **Alias**
---
## 📖DEFINITION
- **CROSS JOIN** – A type of SQL join that produces all possible combinations of rows from two tables, also called a **Cartesian product**. Unlike INNER or OUTER JOINs, it does not require any relationship between the tables.
- **SELF JOIN** – a regular `INNER`, `LEFT`, or `RIGHT` JOIN in which the **same table appears on both sides** of the `JOIN` keyword. Aliases (e.g., `m1`, `m2`) are used to treat one physical table as two logical tables so rows can be compared to other rows inside the same table.
---
## 🧱QUERY FORMAT
```sql
-- 1. CROSS JOIN QUERY STRUCTURE
SELECT <columns>
FROM   left_table
CROSS JOIN right_table;
```
```sql
-- 2. SELF JOIN QUERY STRUCTURE
-- NOTE: both tables used here are same tables, defer by assigning different aliases 
SELECT <alias1>.<col>,
       <alias2>.<col>
FROM   <table> AS <alias1> 
JOIN   <table> AS <alias2>
  ON   <alias1>.<key> = <alias2>.<key>   -- matching condition
 AND   <alias1>.<id>  <> <alias2>.<id>; -- optional anti-self filter
```
---
## 💡TIP TO REMEMBER
- A CROSS JOIN multiplies: rows × rows = grid.
- Use only when you really need all pairings.
- CROSS JOIN generates all possible combinations of rows.
- Use carefully on large tables to avoid extremely large result sets.
- Can be filtered using WHERE for meaningful combinations.
- Think of a self-join as **“make two copies of the table, then treat them like any other join.”**
- Self join: Always add a <> condition to avoid pairing a row with itself.
- Use INNER, LEFT, or other join types depending on the use-case — the keyword self is never written in SQL.

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find all possible combinations of movies and reviewers.
```sql
SELECT m.title AS movie_title, a.actor_name AS actor
FROM movies m
CROSS JOIN actors a
LIMIT 10;
```
### 2. Filter the combinations to only include Action movies and USA actors.
```sql
SELECT m.title AS movie_title, d.director_name AS director
FROM movies m
CROSS JOIN directors d
WHERE m.genre = 'Sci-Fi' AND d.nationality = 'USA';
```
### 3. Count the total number of combinations when CROSS JOINing movies and directors.
```sql
SELECT COUNT(*) AS total_combinations
FROM movies m
CROSS JOIN directors d;
```
### 4. Find all pairs of movies released in the same year
```sql
SELECT m1.title  AS movie_1,
       m2.title  AS movie_2,
       m1.release_year
FROM movies AS m1
INNER JOIN movies AS m2
  ON m1.release_year = m2.release_year
WHERE m1.movie_id <> m2.movie_id;
```

### 5. List all combinations of movies directed by the same director, showing director name along with both movie titles
```sql
SELECT d.director_name,
       m1.title AS movie_1,
       m2.title AS movie_2
FROM movies AS m1
INNER JOIN movies AS m2
  ON m1.director_id = m2.director_id
INNER JOIN directors d
  ON m1.director_id = d.director_id
WHERE m1.movie_id <> m2.movie_id;
```

### 6. Count how many Sci-Fi movie pairs exist where both movies are directed by Christopher Nolan
```sql
SELECT COUNT(*) AS action_pairs_count
FROM movies AS m1
INNER JOIN movies AS m2
  ON m1.director_id = m2.director_id
INNER JOIN directors d
  ON m1.director_id = d.director_id
WHERE m1.movie_id <> m2.movie_id
  AND d.director_name = 'Christopher Nolan'
  AND m1.genre = 'Sci-Fi'
  AND m2.genre = 'Sci-Fi';
```
---
## 🧠Practise
1. Show every possible pairing of actor names with movie titles.
2. How many rows are produced when you CROSS JOIN movies and actors?
3. Rewrite exercise 1 using the older comma syntax: FROM actors, movies
---
## ✅SOLUTIONS


### 1. Show every possible pairing of actor names with movie titles.
```sql
SELECT a.actor_name,
       m.title
FROM   actors AS a
CROSS JOIN movies AS m;
```
### 2. How many rows are produced when you CROSS JOIN movies and actors?
```sql
SELECT COUNT(*) AS total_rows
FROM movies
CROSS JOIN actors;

```
### 3. Rewrite exercise 1 using the older comma syntax: FROM actors, movies
```sql
-- Comma syntax
SELECT a.actor_name,
       m.title
FROM   actors a, movies m;
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
