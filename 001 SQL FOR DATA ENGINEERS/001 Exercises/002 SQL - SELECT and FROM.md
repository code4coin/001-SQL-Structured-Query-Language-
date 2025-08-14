# SQL - SELECT and FROM
---
## KEYWORDS
- **`SELECT`** – Used to retrieve data from one or more tables in a relational database.  
- **`FROM`** – Specifies the table(s) from which to retrieve the data.
---
## TIP
Order of execution for keywords `SELECT` and `FROM`
  1. **`FROM`** – Identify the table(s) involved.   
  2. **`SELECT`** – Choose the columns or expressions to return. 
---
## EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

---
### 1. Retrieving 1 field from patrons table
```sql
SELECT name FROM patrons;
```
### 2. Retrieving 2 fields from patrons table
```sql
SELECT card_id, name FROM patrons;
```
### 3. Retrieving 3 fields from patrons table
```sql
SELECT card_id, name, join_year FROM patrons;
```
### 4. Retrieving multiple fields from patrons table
```sql
SELECT card_id, name, join_year, fines FROM patrons;
```
---
## **Contributing** 🤝

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---
## **License** 📄

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---
## **More Resources** 🔗

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
