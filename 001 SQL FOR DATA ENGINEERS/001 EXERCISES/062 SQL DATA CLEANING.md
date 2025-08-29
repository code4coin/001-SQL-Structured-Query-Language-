# DATA CLEANING
---

## 🔑KEYWORDS

* **Data Cleaning**
* **ETL**
* **ELT**
* **Data Validation**
* **Deduplication**
* **Data Governance**
* **Data Format Standardization**

---

## 📖DEFINITION

* **Data Cleaning** – The process of detecting and correcting (or removing) corrupt, inaccurate, duplicate, or inconsistent data from a dataset to ensure it is accurate, consistent, and usable for analysis.

---

## 🧱QUERY FORMAT

* Standard SQL queries to clean and validate data in tables.
* Example operations include: `LOWER()`, `UPPER()`, `SUBSTRING_INDEX()`, `DELETE`, `WHERE`, `GROUP BY`, `COUNT()`.

---

## 💡TIP TO REMEMBER

* Always validate and standardize data before analysis.
* Deduplication prevents overcounting and inconsistent results.
* Strong data governance minimizes the need for cleaning later.
* Use consistent formats for text, dates, and numeric fields.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Data Format Cleaning: Identify all `user_name` values in `movie_ratings` that are not in lowercase and convert them to lowercase.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT LOWER(user_name) AS cleaned_user_name, rating, review_date
FROM movie_ratings;
```
</details>

### 2. Data Format Cleaning: Ensure `title` of movies in the `movies` table starts with a capital letter.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT CONCAT(UPPER(SUBSTRING(title,1,1)), LOWER(SUBSTRING(title,2))) AS cleaned_title,
       genre, release_year
FROM movies;
```
</details>

### 3. Data Validation: Find all movies with `release_year` outside the range 1900–2025.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT *
FROM movies
WHERE release_year < 1900 OR release_year > 2025;
```
</details>

### 4. Data Validation: Find all `rating` values in `movie_ratings` outside the 0–10 range.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT *
FROM movie_ratings
WHERE rating < 0 OR rating > 10;
```
</details>

### 5. Duplicate Removal: Check if there are duplicate entries in the `movie_actors` table and remove them.
<details>
  <summary>✅ Solution:</summary>
  
```sql
-- Identify duplicates
SELECT movie_id, actor_id, COUNT(*)
FROM movie_actors
GROUP BY movie_id, actor_id
HAVING COUNT(*) > 1;

-- Remove duplicates (example)
DELETE t1
FROM movie_actors t1
INNER JOIN movie_actors t2
WHERE t1.movie_id = t2.movie_id
  AND t1.actor_id = t2.actor_id
  AND t1.rowid > t2.rowid;
```
</details>

### 6. Field Parsing: Extract the first word of each movie title into a separate column called `first_word`.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT title,
       SUBSTRING_INDEX(title, ' ', 1) AS first_word
FROM movies;
```
</details>

### 7. Field Parsing: Extract the last word of each movie title into a separate column called `last_word`.
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT title,
       SUBSTRING_INDEX(title, ' ', -1) AS last_word
FROM movies;
```
</details>

---

## 🧠Practise

1. Standardize `director_name` in `directors` table to proper case (first letter capital).
2. Count how many movies each actor has appeared in using `movie_actors`.
3. Find all `Sci-Fi` movies released after 2010 and validate their `runtime_minutes` is greater than 100.
4. Remove duplicate ratings in `movie_ratings` if a user rated the same movie more than once, keeping only the latest `review_date`.
5. Parse `movie title` to extract any words longer than 5 characters.

---

## ✅SOLUTIONS

### 1. Data Format Cleaning

```sql
-- Lowercase all user names
SELECT LOWER(user_name) AS cleaned_user_name, rating, review_date
FROM movie_ratings;

-- Capitalize first letter of movie titles
SELECT CONCAT(UPPER(SUBSTRING(title,1,1)), LOWER(SUBSTRING(title,2))) AS cleaned_title,
       genre, release_year
FROM movies;
```

### 2. Data Validation

```sql
-- Check release_year range
SELECT *
FROM movies
WHERE release_year < 1900 OR release_year > 2025;

-- Check rating range
SELECT *
FROM movie_ratings
WHERE rating < 0 OR rating > 10;
```

### 3. Duplicate Removal

```sql
-- Identify duplicates in movie_actors
SELECT movie_id, actor_id, COUNT(*)
FROM movie_actors
GROUP BY movie_id, actor_id
HAVING COUNT(*) > 1;

-- Remove duplicates (example)
DELETE t1
FROM movie_actors t1
INNER JOIN movie_actors t2
WHERE t1.movie_id = t2.movie_id
  AND t1.actor_id = t2.actor_id
  AND t1.rowid > t2.rowid;
```

### 4. Field Parsing

```sql
-- Extract first word of movie title
SELECT title,
       SUBSTRING_INDEX(title, ' ', 1) AS first_word,
       SUBSTRING_INDEX(title, ' ', -1) AS last_word
FROM movies;
```

### 5. Practice Solutions

```sql
-- Standardize director names to proper case
SELECT CONCAT(UPPER(SUBSTRING(director_name,1,1)), LOWER(SUBSTRING(director_name,2))) AS proper_name
FROM directors;

-- Count movies per actor
SELECT a.actor_name, COUNT(ma.movie_id) AS movie_count
FROM actors a
LEFT JOIN movie_actors ma ON a.actor_id = ma.actor_id
GROUP BY a.actor_name;

-- Sci-Fi movies after 2010 with runtime > 100
SELECT *
FROM movies
WHERE genre='Sci-Fi' AND release_year > 2010 AND runtime_minutes > 100;

-- Remove duplicate movie_ratings, keep latest review_date
DELETE t1
FROM movie_ratings t1
INNER JOIN movie_ratings t2
WHERE t1.movie_id = t2.movie_id
  AND t1.user_name = t2.user_name
  AND t1.review_date < t2.review_date;

-- Extract words longer than 5 characters from title
SELECT title,
       REGEXP_SUBSTR(title, '\\b\\w{6,}\\b') AS long_word
FROM movies;
```

---

## 🤝**CONTRIBUTING**

We welcome contributions! You can:

* Add new SQL exercises
* Improve existing chapters or examples
* Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---

## 📄**LICENSE**

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---

## 🔗**MORE RESOURCES**

Stay connected and explore more content:

* **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
* **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
* **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)

