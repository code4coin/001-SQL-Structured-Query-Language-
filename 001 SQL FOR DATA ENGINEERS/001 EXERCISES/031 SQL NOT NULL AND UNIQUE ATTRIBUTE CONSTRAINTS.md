# THE NOT NULL AND UNIQUE ATTRIBUTE CONSTRAINTS
---

## 🔑KEYWORDS
- **NULL**
- **NOT NULL**
- **UNIQUE** 
- **ALTER TABLE** 

---
## 📖DEFINITION
- **NULL** - When a record has no information on its attribute is called NULL. For example, if in movie table title is missing etc.
- **NOT NULL Constraint** – A column-level rule that disallows storing NULL (missing/unknown) values. It must be defined only when no NULLs currently exist in the column.  
- **UNIQUE Constraint** – A rule ensuring every non-NULL value in a column (or column combination) occurs only once across the table. Duplicate NULLs are allowed.

---
## 🧱QUERY FORMAT

### NOT NULL
```sql
-- Add NOT NULL during table creation
CREATE TABLE table_name (
    column_name data_type PRIMARY KEY,
    column_name data_type **NOT NULL**
);
```
```sql
-- Add NOT NULL to an existing column (ensure no NULLs first)
ALTER TABLE table_name
ALTER COLUMN column_name SET NOT NULL;
```
```sql
-- Remove NOT NULL
ALTER TABLE table_name
ALTER COLUMN column_name DROP NOT NULL;
```

### UNIQUE
```sql
-- Add UNIQUE during table creation
CREATE TABLE table_name (
    column_name data_type PRIMARY KEY,
    column_name data_type **UNIQUE**
);
```
```sql
-- Add UNIQUE to an existing column (ensure no duplicates first)
ALTER TABLE table_name
ADD CONSTRAINT constraint_name UNIQUE (column_name);
```
```sql
-- Drop UNIQUE
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;
```

---
## 💡TIP TO REMEMBER
- **“NN-U” – Not Null first, then Unique.** Always check for existing NULLs or duplicates before adding either constraint.
- **NOT NULL** = Value must always exist.  
- **UNIQUE** = No two rows can have the same value.  
- Together, **NOT NULL + UNIQUE** behaves like a primary key (but without being the official key).  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Add a NOT NULL constraint on the `title` column of the `movies` table.  
✅ **Solution:**
```sql
ALTER TABLE movies
ALTER COLUMN title SET NOT NULL;
```

---

### 2. Try inserting a new movie with a `NULL` title. What happens?  
✅ **Solution:**
```sql
INSERT INTO movies (movie_id, genre, director_id, release_year, country, runtime_minutes)
VALUES (16, NULL, 1, 2025, 'USA', 120);
-- ❌ This will fail because "title" is NOT NULL.
```

---

### 3. Add a UNIQUE constraint to ensure no two directors have the same `director_name`.  
✅ **Solution:**
```sql
ALTER TABLE directors
ADD CONSTRAINT unique_director_name UNIQUE (director_name);
```

---

### 4. Try inserting another director named "Christopher Nolan".  
✅ **Solution:**
```sql
INSERT INTO directors (director_id, director_name, nationality)
VALUES (6, 'Christopher Nolan', 'UK');
-- ❌ This will fail due to UNIQUE constraint.
```

---

### 5. Make `runtime_minutes` a NOT NULL column, then try inserting a movie without runtime.  
✅ **Solution:**
```sql
ALTER TABLE movies
ALTER COLUMN runtime_minutes SET NOT NULL;

INSERT INTO movies (movie_id, title, genre, director_id, release_year, country)
VALUES (17, 'New Sci-Fi Movie', 'Sci-Fi', 1, 2025, 'USA');
-- ❌ This will fail because runtime_minutes cannot be NULL.
```

---
## 🧠Practise
1. Add a UNIQUE constraint on `actors.actor_name`.  
2. Insert a duplicate actor name and see what happens.  
3. Drop the UNIQUE constraint on `directors.director_name`.  
4. Modify the `movies.country` column to **NOT NULL**.  
5. Try inserting a new movie without specifying the `country`.  

---
## ✅SOLUTIONS

### Practice
### 1. Add a UNIQUE constraint on `actors.actor_name`.  
```sql
ALTER TABLE actors
ADD CONSTRAINT unique_actor_name UNIQUE (actor_name);
```

### 2. Insert a duplicate actor name and see what happens.  
```sql
INSERT INTO actors (actor_id, actor_name, nationality)
VALUES (7, 'Brad Pitt', 'USA');
-- ❌ Will fail because "Brad Pitt" already exists.
```

### 3. Drop the UNIQUE constraint on `directors.director_name`.   
```sql
ALTER TABLE directors
DROP CONSTRAINT unique_director_name;
```

### 4. Modify the `movies.country` column to **NOT NULL**.  
```sql
ALTER TABLE movies
ALTER COLUMN country SET NOT NULL;
```

### 5. Try inserting a new movie without specifying the `country`. 
```sql
INSERT INTO movies (movie_id, title, genre, director_id, release_year, runtime_minutes)
VALUES (18, 'Null Country Movie', 'Drama', 2, 2025, 120);
-- ❌ Will fail because "country" is NOT NULL.
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

