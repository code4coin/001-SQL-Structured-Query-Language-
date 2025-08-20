# INTERSECT
---

## 🔑 KEYWORDS
- **INTERSECT** – set operator that returns only the rows that appear in **both** result sets  
- **Set Operation** – vertical combination of queries (vs. joins which are horizontal)  
- **De-duplication** – INTERSECT removes duplicate rows automatically  
- **Column Compatibility** – left and right SELECTs must have the same number and order of **compatible** columns  

---

## 📖 DEFINITION
- **INTERSECT** – an ANSI-SQL set operator that compares two SELECT statements row-by-row and returns **only the distinct rows that exist in both outputs**.

---
## 🧱QUERY FORMAT
```sql
SELECT column1, column2, ... FROM table1
INTERSECT
SELECT column1, column2, ... FROM table2;
```
---
## 💡TIP TO REMEMBER
- INTERSECT is the Venn-diagram overlap between two queries. Think: **“Give me rows that appear on both sides, once.”**
- INTERSECT returns only distinct records common to both tables.
- Column order and data type must match in both tables.
- INTERSECT removes duplicates automatically, unlike INNER JOIN which can return duplicates.
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Find movies that were directed by Christopher Nolan and also have ratings above 9.0.
```sql
SELECT title FROM movies WHERE director_id = 1
INTERSECT
SELECT title FROM movies m JOIN movie_ratings r ON m.movie_id = r.movie_id WHERE rating > 9.0;
```
### 2. List actors who acted in both 'Inception' and 'The Dark Knight'.
```sql
SELECT actor_id FROM movie_actors WHERE movie_id = 1
INTERSECT
SELECT actor_id FROM movie_actors WHERE movie_id = 12;
```
### 3. Retrieve all movies that are Sci-Fi and also released in the USA.
```sql
SELECT title FROM movies WHERE genre = 'Sci-Fi'
INTERSECT
SELECT title FROM movies WHERE country = 'USA';
```
---
## 🧠PRACTISE
1. List all countries that have produced both Sci-Fi and Action movies.
2. Find actors who have appeared in movies directed by both Christopher Nolan and Quentin Tarantino.
3. Identify runtimes (in minutes) that are shared between movies released in 2010 and movies released in 2015.
---
## ✅SOLUTIONS
### 1. List all countries that have produced both Sci-Fi and Action movies.
```sql
SELECT country
FROM   movies
WHERE  genre = 'Sci-Fi'

INTERSECT

SELECT country
FROM   movies
WHERE  genre = 'Action';
```
### 2. Find actors who have appeared in movies directed by both Christopher Nolan and Quentin Tarantino.
```sql
SELECT DISTINCT a.actor_name
FROM   actors a
JOIN   movie_actors ma USING (actor_id)
JOIN   movies m USING (movie_id)
WHERE  m.director_id = 1               -- Christopher Nolan

INTERSECT

SELECT DISTINCT a.actor_name
FROM   actors a
JOIN   movie_actors ma USING (actor_id)
JOIN   movies m USING (movie_id)
WHERE  m.director_id = 3;              -- Quentin Tarantino
```
### 3. Identify runtimes (in minutes) that are shared between movies released in 2010 and movies released in 2015.
```sql
SELECT runtime_minutes
FROM   movies
WHERE  release_year = 2010

INTERSECT

SELECT runtime_minutes
FROM   movies
WHERE  release_year = 2015;
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
