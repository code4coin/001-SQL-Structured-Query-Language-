# EXCEPT
---

## 🔑keyWORDS
- **EXCEPT**
- **Set Operations**
- **SQL**
- **Left Table**
- **Right Table**
---

## 📖DEFINITION

- **EXCEPT** – Returns records from the **first query** (left table) that do **not exist** in the **second query** (right table).\
  It is useful for finding differences between two datasets.
---

## 🧱QUERY FORMAT

```sql
SELECT column1, column2, ...
FROM table1
EXCEPT
SELECT column1, column2, ...
FROM table2;
```

---

## 💡TIP TO REMEMBER

- **Think of EXCEPT as Left – Right (like subtraction)**. Both SELECTs must return the same number of columns and compatible data types.
- EXCEPT only keeps records **unique to the left query**.
- Matching is done on **all selected columns**, not just one.
- Useful for finding missing or unmatched records.

---
## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.\
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find movies directed by Christopher Nolan that **do not** appear in movies directed by Steven Spielberg.

```sql
SELECT title, release_year
FROM movies
WHERE director_id = 1
EXCEPT
SELECT title, release_year
FROM movies
WHERE director_id = 2;
```

---

## 🧺PRACTISE

1. List all Sci-Fi movies that are **not directed by James Cameron**.
2. Find actors who acted in movies **not directed by Quentin Tarantino**.
3. Find every movie that has zero ratings.
4. Find every director who has never made a Sci-Fi movie.
5. Find every actor who has never worked with Christopher Nolan.
---

## ✅SOLUTIONS

### 1. Sci-Fi movies not directed by James Cameron

```sql
SELECT title, genre
FROM movies
WHERE genre = 'Sci-Fi'
EXCEPT
SELECT title, genre
FROM movies
WHERE director_id = 5;
```

### 2. Actors not in Quentin Tarantino movies

```sql
SELECT actor_id, actor_name
FROM actors
WHERE actor_id IN (SELECT actor_id FROM movie_actors)
EXCEPT
SELECT actor_id, actor_name
FROM actors
WHERE actor_id IN (SELECT actor_id FROM movie_actors WHERE movie_id IN (SELECT movie_id FROM movies WHERE director_id = 3));
```
### 3. Find every movie that has zero ratings.
```sql
SELECT movie_id, title
FROM   movies
EXCEPT
SELECT movie_id, title
FROM   movie_ratings
JOIN   movies USING (movie_id);
```
### 4. Find every director who has never made a Sci-Fi movie.
```sql 
SELECT director_id, director_name
FROM   directors
EXCEPT
SELECT DISTINCT d.director_id, d.director_name
FROM   directors d
JOIN   movies m
       ON d.director_id = m.director_id
WHERE  m.genre = 'Sci-Fi';
```
### 5. Find every actor who has never worked with Christopher Nolan.
```sql 
SELECT actor_id, actor_name
FROM   actors
EXCEPT
SELECT DISTINCT a.actor_id, a.actor_name
FROM   actors a
JOIN   movie_actors ma ON a.actor_id = ma.actor_id
JOIN   movies m        ON ma.movie_id = m.movie_id
WHERE  m.director_id = 1;   -- Christopher Nolan
```
---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

## 📜**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)

