# SLOWLY CHANGING DIMENSION SCD
---

## 🔑KEYWORDS

* **SCD**
* **Type I**
* **Type II**
* **Type III**
* **Snapshot**
* **Historical data**
* **Dimension table**
* **Fact table**

---

## 📖DEFINITION

* **Slowly Changing Dimensions (SCD)** – A methodology in data warehousing to manage changes in dimension tables over time.
* **Type I** – Overwrites old data, losing history.
* **Type II** – Adds new rows to preserve historical values.
* **Type III** – Adds new columns to store limited historical changes.
* **Modern Snapshot Approach** – Stores periodic snapshots of the entire dimension table to maintain historical accuracy.

---

## 🧱QUERY FORMAT

* **Type I:** `UPDATE` dimension table to overwrite old values
* **Type II:** `INSERT` new rows with updated values
* **Type III:** `ALTER TABLE` to add historical column, then `UPDATE`
* **Snapshot:** Store full table snapshots for historical reporting

---

## 💡TIP TO REMEMBER

* Fact tables should **never be updated**; only dimension tables change.
* Type I is simple but **loses history**.
* Type II preserves **full history** but increases table size.
* Type III is **limited to tracking a few historical changes**.
* Modern snapshots leverage **low-cost storage** for historical accuracy.

---

## 💪EXERCISE

Before moving to the exercises, we need a platform with tables and data.
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Type I Exercise: Update the genre of “Inception” to “Sci-Fi Thriller”

<details>
<summary>✅ Solution:</summary>

```sql
UPDATE movies
SET genre = 'Sci-Fi Thriller'
WHERE title = 'Inception';
```

</details>

### 2. Type II Exercise: Insert a new row for “Interstellar” with genre “Space Adventure” and movie_id = 16

<details>
<summary>✅ Solution:</summary>

```sql
INSERT INTO movies (movie_id, title, genre, director_id, release_year, country, runtime_minutes)
VALUES (16, 'Interstellar', 'Space Adventure', 1, 2014, 'USA', 169);
```

</details>

### 3. Type III Exercise: Add column previous_genre, update “Gladiator” from “Action” to “Epic Action”

<details>
<summary>✅ Solution:</summary>

```sql
ALTER TABLE movies
ADD COLUMN previous_genre VARCHAR(50);

UPDATE movies
SET previous_genre = genre,
    genre = 'Epic Action'
WHERE title = 'Gladiator';
```

</details>

### 4. Modern Snapshot Exercise: Create a snapshot of movies table as of 2024-01-01

<details>
<summary>✅ Solution:</summary>

```sql
CREATE TABLE movies_snapshot_20240101 AS
SELECT *
FROM movies;
```

</details>

---

## 🧠Practise

1. Update the nationality of “Christopher Nolan” to “British” (Type I).
2. Add a new record for “Titanic” with genre “Romance Drama” and new movie\_id (Type II).
3. Add a previous\_country column and update “The Martian” country from “USA” to “UK” (Type III).
4. Create a snapshot table called movies\_snapshot to store current state of movies.

---

## ✅SOLUTIONS

1. **Type I Update Director**

```sql
UPDATE directors
SET nationality = 'British'
WHERE director_name = 'Christopher Nolan';
```

2. **Type II Insert Movie**

```sql
INSERT INTO movies (movie_id, title, genre, director_id, release_year, country, runtime_minutes)
VALUES (17, 'Titanic', 'Romance Drama', 5, 1997, 'USA', 195);
```

3. **Type III Update Country**

```sql
ALTER TABLE movies
ADD COLUMN previous_country VARCHAR(50);

UPDATE movies
SET previous_country = country,
    country = 'UK'
WHERE title = 'The Martian';
```

4. **Modern Snapshot Table**

```sql
CREATE TABLE movies_snapshot AS
SELECT *
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

---

