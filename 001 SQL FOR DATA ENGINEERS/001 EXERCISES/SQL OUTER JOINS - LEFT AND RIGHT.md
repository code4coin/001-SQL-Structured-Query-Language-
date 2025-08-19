# OUTER JOINS - LEFT AND RIGHT
---

## 🔑KEYWORDS
- **LEFT JOIN** 
- **RIGHT JOIN**
- **OUTER**
---
## 📖DEFINITION
- **LEFT JOIN** – returns **all** rows from the **left** table plus matching rows from the right table; where no match exists, right-side columns show `NULL`.  
- **RIGHT JOIN** – returns **all** rows from the **right** table plus matching rows from the left table; where no match exists, left-side columns show `NULL`.
- **OUTER** – unmatched rows are kept; missing columns filled with `NULL` 

---
## 🧱QUERY FORMAT
```sql
-- LEFT JOIN
SELECT <columns>
FROM   left_table  AS l
LEFT  JOIN right_table AS r
       ON l.key = r.key;
```
```sql
-- RIGHT JOIN
SELECT <columns>
FROM   left_table  AS l
RIGHT JOIN right_table AS r
       ON l.key = r.key;
```
---
## 💡TIP TO REMEMBER
**Think “Left keeps left, Right keeps right.”
Any unmatched side gets NULL.**

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. List every movie along with its director’s name—even if the director is unknown (director_id is missing).
```sql
SELECT m.title,
       d.director_name
FROM   movies   AS m
LEFT  JOIN directors AS d
       ON m.director_id = d.director_id;
```
### 2. List every director along with every movie they have directed—even if they have directed none yet.
```sql
SELECT d.director_name,
       m.title
FROM   movies   AS m
RIGHT JOIN directors AS d
       ON m.director_id = d.director_id;
```
### 3. Rewrite the second exercise as a LEFT JOIN (swap tables) instead of using RIGHT JOIN.
```sql
-- LEFT JOIN version of exercise 2
SELECT d.director_name,
       m.title
FROM   directors AS d
LEFT  JOIN movies  AS m
       ON d.director_id = m.director_id;
```
---
## 🧠Practise
1. List all movies and their ratings (include movies even if they don’t have a rating).
2. List all movies and their directors (include movies even if they don’t have a director).
3. List all directors and the movies they directed (include directors even if they haven’t directed a movie).
4. List all movies along with any rating and director information (show NULLs where missing).
---

## ✅SOLUTIONS
### 1. List all movies and their ratings (include movies even if they don’t have a rating).
```sql
SELECT m.title, r.rating
FROM movies m
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id;
```
### 2. List all movies and their directors (include movies even if they don’t have a director).
```sql
SELECT m.title, d.director_name
FROM movies m
LEFT JOIN directors d ON m.director_id = d.director_id;
```
### 3. List all directors and the movies they directed (include directors even if they haven’t directed a movie).
```sql
SELECT d.director_name, m.title
FROM directors d
LEFT JOIN movies m ON d.director_id = m.director_id;
```
### 4. List all movies along with any rating and director information (show NULLs where missing).
```sql
SELECT m.title, d.director_name, r.rating
FROM movies m
LEFT JOIN directors d ON m.director_id = d.director_id
LEFT JOIN movie_ratings r ON m.movie_id = r.movie_id;
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
