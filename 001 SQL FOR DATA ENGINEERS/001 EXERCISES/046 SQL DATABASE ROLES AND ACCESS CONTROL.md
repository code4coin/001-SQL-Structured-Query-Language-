# 📘 DATABASE ROLES AND ACCESS CONTROL
---
## 🔑KEYWORDS
- **Role**
- **Privileges**
- **GRANT**
- **REVOKE**
- **User role**
- **Group role**
- **View**
- **Access Control**

---
## 📖DEFINITION
- **Database Role** – An entity in PostgreSQL that can represent a **user** or a **group** and is used to manage database permissions.  
- **Access Control** – The process of controlling which users/roles have permission to perform specific actions on database objects.  
- **Privileges** – Rights that can be granted or revoked from roles, such as `SELECT`, `UPDATE`, `INSERT`, `DELETE`, etc.

---
## 🧱QUERY FORMAT

```sql
-- Create a role
CREATE ROLE role_name;
```
```sql
-- Create a role with attributes
CREATE ROLE role_name LOGIN PASSWORD 'password' VALID UNTIL 'date';
```
```sql
-- Grant privileges
GRANT privilege_name ON object TO role_name;
```
```sql
-- Revoke privileges
REVOKE privilege_name ON object FROM role_name;
```
```sql
-- Add user to group role
GRANT group_role TO user_role;
```
```sql
-- Remove user from group role
REVOKE group_role FROM user_role;
```

---
## 💡TIP TO REMEMBER
- Roles in PostgreSQL can be **both users and groups**.  
- Always grant the **least required privileges** (principle of least privilege).  
- Use **views** to simplify access control and restrict sensitive columns.  

---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory:  
👉 [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

---

### 1. Create a role named `data_analyst`. Grant it **SELECT** privilege on the `movies` table.
<details>
  <summary>✅ Solution:</summary>

```sql
CREATE ROLE data_analyst;
GRANT SELECT ON movies TO data_analyst;
```
</details>

---

### 2. Create a role `intern` with login access, password `intern123`, valid until `2025-12-31`. Grant it access to only the `top_rated_movies` view.
<details>
  <summary>✅ Solution:</summary>

```sql
CREATE ROLE intern LOGIN PASSWORD 'intern123' VALID UNTIL '2025-12-31';

CREATE VIEW top_rated_movies AS
SELECT m.title, ROUND(AVG(r.rating),1) AS avg_rating
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title
HAVING AVG(r.rating) > 9.0;

GRANT SELECT ON top_rated_movies TO intern;
```
</details>

---

### 3. Revoke `UPDATE` privilege on the `movie_ratings` table from `data_analyst`.
<details>
  <summary>✅ Solution:</summary>

```sql
REVOKE UPDATE ON movie_ratings FROM data_analyst;
```
</details>

---

### 4. Create a user role `alex`. Add `alex` to the `data_analyst` group.
<details>
  <summary>✅ Solution:</summary>

```sql
CREATE ROLE alex LOGIN PASSWORD 'alexpass';
GRANT data_analyst TO alex;
```
</details>

---

### 5. Show a query where analysts can only see the **average rating per movie**, not individual ratings.
<details>
<summary>Solution</summary>

```sql
CREATE VIEW analyst_movie_ratings AS
SELECT m.title, ROUND(AVG(r.rating),1) AS avg_rating, COUNT(r.rating) AS total_reviews
FROM movies m
JOIN movie_ratings r ON m.movie_id = r.movie_id
GROUP BY m.title;

GRANT SELECT ON analyst_movie_ratings TO data_analyst;
```

✅ Expected Output (sample):  
```
       title          | avg_rating | total_reviews
----------------------+------------+---------------
 Inception            |    9.3     |       2
 Titanic              |    9.5     |       1
 Avatar               |    9.7     |       1
 Interstellar         |    9.8     |       1
 The Dark Knight      |    9.4     |       1
```
</details>

---
## 🧠Practise
1. Create a role `critic` who can only `INSERT` ratings into `movie_ratings`.  
2. Grant `UPDATE` privilege on the `directors` table to an `admin` role.  
3. Revoke `SELECT` privilege on the `actors` table from the `intern` role.  
4. Create a user role `emma` and assign her to the `critic` group.  

---
## ✅SOLUTIONS

```sql
-- 1. Critic role
CREATE ROLE critic;
GRANT INSERT ON movie_ratings TO critic;
```
```sql
-- 2. Admin update privilege
GRANT UPDATE ON directors TO admin;
```
```sql
-- 3. Revoke select from intern
REVOKE SELECT ON actors FROM intern;
```
```sql
-- 4. Create user and assign group
CREATE ROLE emma LOGIN PASSWORD 'emmapass';
GRANT critic TO emma;
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

