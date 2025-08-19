# CROSS AND SELF JOINS
---
## 🔑KEYWORDS
- **CROSS JOIN**
- **CARTESIAN PRODUCT**
---
## 📖DEFINITION
- **CROSS JOIN** – A type of SQL join that produces all possible combinations of rows from two tables, also called a Cartesian product. Unlike INNER or OUTER JOINs, it does not require any relationship between the tables.
---
## 🧱QUERY FORMAT
```sql
SELECT <columns>
FROM   left_table
CROSS JOIN right_table;
```
---
## 💡TIP TO REMEMBER
- A CROSS JOIN multiplies: rows × rows = grid.
- Use only when you really need all pairings.
- CROSS JOIN generates all possible combinations of rows.
- Use carefully on large tables to avoid extremely large result sets.
- Can be filtered using WHERE for meaningful combinations.

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
