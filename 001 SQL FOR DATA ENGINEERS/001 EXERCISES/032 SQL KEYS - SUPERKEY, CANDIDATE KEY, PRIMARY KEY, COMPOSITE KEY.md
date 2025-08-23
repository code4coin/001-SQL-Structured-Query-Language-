# KEYS - SUPERKEY, CANDIDATE KEY, PRIMARY KEY, COMPOSITE KEY
---

## 🔑KEYWORDS
- **Primary Key**  
- **Superkey**  
- **Candidate Key**  
- **Minimal Superkey**  
- **Composite Key**  

---
## 📖DEFINITION
- **Superkey** – A set of one or more attributes that uniquely identify a row.  
- **Key** – A minimal superkey; removing any attribute would break uniqueness.  
- **Candidate Key** – Potential primary keys; minimal superkeys.  
- **Primary Key** – The officially chosen candidate key.  
- **Composite Key** – A key consisting of multiple attributes.  

---
## 🧱QUERY FORMAT
Check uniqueness of attributes:  
```sql
SELECT attr1, attr2, COUNT(*) 
FROM table
GROUP BY attr1, attr2
HAVING COUNT(*) > 1;
```

---
## 💡TIP TO REMEMBER
- **“Every primary key is a candidate key, every candidate key is a minimal superkey, but not every superkey is a key.”** When in doubt, check uniqueness with GROUP BY … HAVING COUNT(*) > 1.
- **Composite keys** are useful in many-to-many relationships (like `movie_actors`).  
- Choose **short, stable attributes** for primary keys.  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 1. Find all candidate keys for the `movies` table and prove that `(title, release_year, director_id)` uniquely identifies rows.
✅ **Solution:**
```sql
SELECT title, release_year, director_id, COUNT(*) 
FROM movies
GROUP BY title, release_year, director_id
HAVING COUNT(*) > 1;
```

---

### 2. Verify that `movie_id` is a primary key in `movies` by checking for duplicates.
✅ **Solution:**
```sql
SELECT movie_id, COUNT(*) 
FROM movies
GROUP BY movie_id
HAVING COUNT(*) > 1;
```
---

### 3. Show that `(movie_id, actor_id)` is a composite key in `movie_actors`.
✅ **Solution:**
```sql
SELECT movie_id, actor_id, COUNT(*)
FROM movie_actors
GROUP BY movie_id, actor_id
HAVING COUNT(*) > 1;
```

---

### 4. Use a candidate key `(title, release_year, director_id)` to fetch ratings for *Inception (2010)*.
✅ **Solution:**
```sql
SELECT r.rating_id, r.user_name, r.rating, m.title
FROM movies m
JOIN movie_ratings r ON r.movie_id = m.movie_id
JOIN directors d ON d.director_id = m.director_id
WHERE m.title = 'Inception'
  AND m.release_year = 2010
  AND d.director_name = 'Christopher Nolan';
```

---
## 🧠Practise
1. Find all candidate keys in the `directors` table.  
2. Use `actor_name` (candidate key) to fetch all movies of *Leonardo DiCaprio*.  
3. Show that `rating_id` is the primary key of `movie_ratings`.  
4. Write a query to prove `(title, release_year)` **alone is not a candidate key** in the `movies` table.  
5. Prove `(movie_id, user_name)` in `movie_ratings` is **not unique**.  

---
## ✅SOLUTIONS
### 1. Candidate keys in `directors`:
```sql
-- director_id is primary key
SELECT director_id, COUNT(*)
FROM directors
GROUP BY director_id
HAVING COUNT(*) > 1;

-- director_name can be alternate candidate key
SELECT director_name, COUNT(*)
FROM directors
GROUP BY director_name
HAVING COUNT(*) > 1;
```
---

### 2. Movies of Leonardo DiCaprio:
```sql
SELECT a.actor_name, m.title, m.release_year
FROM actors a
JOIN movie_actors ma ON ma.actor_id = a.actor_id
JOIN movies m ON m.movie_id = ma.movie_id
WHERE a.actor_name = 'Leonardo DiCaprio'
ORDER BY m.release_year;
```
---

### 3. `rating_id` uniqueness:
```sql
SELECT rating_id, COUNT(*)
FROM movie_ratings
GROUP BY rating_id
HAVING COUNT(*) > 1;
```
---

### 4. Prove `(title, release_year)` is not candidate key:
```sql
SELECT title, release_year, COUNT(*)
FROM movies
GROUP BY title, release_year
HAVING COUNT(*) > 1;
```
---

### 5. `(movie_id, user_name)` not unique:
```sql
SELECT movie_id, user_name, COUNT(*)
FROM movie_ratings
GROUP BY movie_id, user_name
HAVING COUNT(*) > 1;
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

---
