# UPDATING DATABASE STRUCTURE - ALTER AND INSERT
---
## 🔑KEYWORDS
- **ALTER TABLE**
- **ALTER TABLE … ADD COLUMN**    
- **ALTER TABLE … RENAME COLUMN**  
- **ALTER TABLE … DROP COLUMN**
- **DATA MIGRATION**   
- **INSERT INTO … SELECT DISTINCT**
- **DROP TABLE**


---
## 📖DEFINITION
- **ALTER TABLE** – Used to change the structure of an existing table (e.g., add, drop, or rename columns).
- **TABLE STRUCTURE CHANGE** - changing structure of table instead of data (make modification on the data container, use SQL keyword ALTER)
- **ALTER TABLE … ADD COLUMN**   - adding additional column to the table
- **ALTER TABLE … RENAME COLUMN**  - renaming existing column
- **ALTER TABLE … DROP COLUMN** - deleting column fromt the table
- **DATA MIGRATION** - coping data from one source to another; for this session, it consist of moving data from one table to another using INSERT & SELECT combination
- **INSERT INTO SELECT DISTINCT** – Copies data from one table into another while avoiding duplicate rows.
- **DROP TABLE** - At times, we create temporary tables to store intermediate data in the database. Once these tables have served their purpose, it is good practice to delete them to prevent unnecessary resource usage and keep the database clean.

---
## 🧱QUERY FORMAT

```sql
-- Rename a column
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```
```sql
-- Drop a column
ALTER TABLE table_name
DROP COLUMN column_name;
```
```sql
-- Add a column
ALTER TABLE table_name
ADD COLUMN column_name datatype;
```
```sql
-- Insert distinct records from another table
INSERT INTO target_table (col1, col2, ...)
SELECT DISTINCT col1, col2, ...
FROM source_table;
```
```sql
-- Deleting database table
DROP TABLE table_name;
```

---
## 💡TIP TO REMEMBER
- **“Migrate → Verify → Celebrate.”** Always run a quick SELECT COUNT(*) before and after the insert to be sure the row counts are what you expect.
Always use **DISTINCT** when migrating data into lookup tables (like directors or actors) to avoid duplicates.  
- **Dropping a column** permanently removes data – be sure before executing!  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Rename a column  
Rename the column `nationality` in the **directors** table to `country_origin`.  
```sql
ALTER TABLE directors
RENAME COLUMN nationality TO country_origin;
```
### 2. Drop a column
Suppose we no longer need the column `runtime_minutes` in the **movies** table. Remove it.  
```sql
ALTER TABLE movies
DROP COLUMN runtime_minutes;
```
### 3. Add a new column 
Add a new column `budget_million_usd` (decimal with 2 digits after decimal) to the **movies** table.  
```sql
ALTER TABLE movies
ADD COLUMN budget_million_usd DECIMAL(10,2);
```
### 4. Insert distinct data into a lookup table  
Imagine we had a temporary denormalized table `movies_temp` with director details repeated. Insert distinct directors into the **directors** table.  
```sql
INSERT INTO directors (director_id, director_name, country_origin)
SELECT DISTINCT director_id, director_name, country_origin
FROM movies_temp;
```
---
## 🧠Practise
1. Add a new column `box_office_million_usd` to the **movies** table.  
2. Rename the column `country` in the **movies** table to `production_country`.  
3. Drop the column `review_date` from the **movie_ratings** table.  
4. Find the number of **distinct directors** from the **movies** table.  
5. Insert a new actor (`id=7, actor_name='Christian Bale', nationality='UK'`) into the **actors** table.  

---
## ✅SOLUTIONS

1. **Add a new column `box_office_million_usd` to the movies table.**
```sql
ALTER TABLE movies
ADD COLUMN box_office_million_usd DECIMAL(10,2);
```

2. **Rename the column `country` in the movies table to `production_country`.**
```sql
ALTER TABLE movies
RENAME COLUMN country TO production_country;
```

3. **Drop the column `review_date` from the movie_ratings table.**
```sql
ALTER TABLE movie_ratings
DROP COLUMN review_date;
```

4. **Find the number of distinct directors from the movies table.**
```sql
SELECT COUNT(DISTINCT director_id) AS distinct_directors
FROM movies;
```

5. **Insert a new actor into the actors table.**
```sql
INSERT INTO actors (actor_id, actor_name, nationality)
VALUES (7, 'Christian Bale', 'UK');
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
