# UNION AND UNION ALL
---
## 🔑KEYWORDS  
- **Set Operations**  
- **UNION**  
- **UNION ALL**
- **Column-Shape Rule**
- **First-SELECT Alias Rule**
---

## 📖DEFINITION  
- **UNION** – vertical stack of two result-sets, duplicates removed  
- **UNION ALL** – same stack but duplicates kept  
- **Set Operation** – combines entire rows instead of joining on keys  
- **Column-Shape Rule** – both SELECTs must return identical number & type of columns  
- **First-SELECT Alias Rule** – final column names come from the first SELECT 
---

## 🧱QUERY FORMAT  

```sql
SELECT column_list FROM table1
UNION [ALL]
SELECT column_list FROM table2;
```
---
## 💡TIP TO REMEMBER
- **“UNION throws away the photocopies; UNION ALL keeps them.”**
- If you need performance and you already know there are no duplicates (or you simply don’t care), prefer UNION ALL.
- Use UNION when you want unique records only
- Use UNION ALL when you want all records, including duplicates
- No ON clause is needed (no matching of keys)

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Write a query using UNION ALL to return every name (including duplicates) from both directors and actors tables.
```sql
SELECT director_name AS name FROM directors
UNION ALL
SELECT actor_name AS name FROM actors;
```
### 2. How would you identify someone who appears in both tables?
```sql
SELECT name
FROM (
    SELECT director_name AS name FROM directors
    UNION ALL
    SELECT actor_name AS name FROM actors
) t
GROUP BY name
HAVING COUNT(*) > 1;
```
---
## 🧠Practise
1. Master List – no duplicates: Create one alphabetical list of every movie title and every director name.
2. Master List – with duplicates: Repeat exercise 1 but keep duplicates (if any).
3. Who wears two hats?: Use UNION ALL followed by
---
## ✅SOLUTIONS
### 1. Master List – no duplicates: Create one alphabetical list of every movie title and every director name.
```sql
SELECT title AS name
FROM   movies
UNION
SELECT director_name
FROM   directors
ORDER  BY name;
```
### 2. Master List – with duplicates: Repeat exercise 1 but keep duplicates (if any).
```sql
SELECT title AS name
FROM   movies
UNION ALL
SELECT director_name
FROM   directors
ORDER  BY name;
```
### 3. Who wears two hats?: Use UNION ALL followed by
```sql
SELECT name, COUNT(*) AS appearances
FROM (
    SELECT director_name AS name
    FROM   directors
    UNION ALL
    SELECT actor_name
    FROM   actors
) AS everyone
GROUP BY name
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
