# 🗄️ SQL, Databases & RDBMS — Fundamentals

> *The three pillars every data-driven developer must understand.*

---

## 📋 Table of Contents

- [What is SQL?](#-what-is-sql)
- [What is a Database?](#-what-is-a-database)
- [What is RDBMS?](#-what-is-rdbms)
- [How They Connect](#-how-they-connect)
- [Quick Reference](#-quick-reference)

---

## 💬 What is SQL?

**SQL (Structured Query Language)** is the standard language used to communicate with relational databases.

### Core Idea
SQL is a **declarative language** — you tell the database ***what*** you want, not ***how*** to get it. The database engine figures out the rest.

```sql
-- You say WHAT you want:
SELECT name, age FROM students WHERE grade = 'A';

-- You don't say HOW to scan the table or which index to use.
-- The database handles that automatically.
```

---

### ⚙️ The 4 CRUD Operations

| Operation | SQL Command | What it Does |
|-----------|-------------|--------------|
| ➕ **Create** | `INSERT INTO` | Add new records |
| 📖 **Read** | `SELECT` | Retrieve/query data |
| ✏️ **Update** | `UPDATE` | Modify existing records |
| ❌ **Delete** | `DELETE FROM` | Remove records |

> 💡 **CRUD** = **C**reate, **R**ead, **U**pdate, **D**elete — the four fundamental operations of any data-driven application.

---

### 🏗️ SQL Also Handles Structure

Beyond data manipulation, SQL manages the *structure* of databases too:

```sql
CREATE TABLE students (id INT, name VARCHAR(100), grade CHAR(1));  -- Create a table
ALTER TABLE students ADD COLUMN age INT;                            -- Modify a table
DROP TABLE students;                                                -- Delete a table
```

> **SQL is the backbone of almost every data-driven application in the world.**

---

## 🗃️ What is a Database?

A **database** is an organized collection of data, stored and managed so it can be easily accessed, updated, and maintained.

### The Filing Cabinet Analogy

```
🗂️ Traditional Filing Cabinet          🗄️ Database
─────────────────────────────          ──────────────────────────────
📁 Drawer: "Students"          →       📊 Table: students
   📄 File: John Doe           →          🔢 Row: { id:1, name:"John" }
   📄 File: Jane Smith         →          🔢 Row: { id:2, name:"Jane" }

📁 Drawer: "Courses"           →       📊 Table: courses
   📄 File: Mathematics        →          🔢 Row: { id:101, name:"Maths" }
```

### Real-World Example — A School Database

<details>
<summary>📌 <strong>Click to expand: See how a school database is structured</strong></summary>

```
🏫 School Database
├── 📊 students       → id, name, age, email
├── 📊 teachers       → id, name, subject, email
├── 📊 courses        → id, course_name, teacher_id
└── 📊 grades         → student_id, course_id, score
```

All four tables are **related** to each other — a student has grades, grades belong to courses, courses have teachers.

</details>

---

### 🔧 The Role of a DBMS

A database is managed by a **Database Management System (DBMS)** — software that acts as the interface between you and your data.

```
👤 You / Application
       │
       ▼
  ┌─────────────┐
  │    DBMS     │  ← Software that manages the database
  └─────────────┘
       │
       ▼
  ┌─────────────┐
  │  Database   │  ← Where data actually lives
  └─────────────┘
```

> Without a DBMS, raw data would be scattered files with no easy way to search, update, or manage them.

---

## 🔗 What is RDBMS?

**RDBMS (Relational Database Management System)** is a type of DBMS that stores data in **tables** — and more importantly, allows tables to **relate** to each other.

### Tables = Relations

Every table in an RDBMS has:

| Concept | Also Called | Description |
|---------|-------------|-------------|
| **Table** | Relation | A grid of data (like a spreadsheet) |
| **Row** | Record / Tuple | One single entry of data |
| **Column** | Attribute / Field | A specific type of data |

```
📊 students table
┌────┬─────────────┬─────┬───────────────────────┐
│ id │ name        │ age │ email                 │
├────┼─────────────┼─────┼───────────────────────┤
│  1 │ John Doe    │  20 │ john@example.com      │  ← Row (Record)
│  2 │ Jane Smith  │  22 │ jane@example.com      │
│  3 │ Bob Lee     │  19 │ bob@example.com       │
└────┴─────────────┴─────┴───────────────────────┘
  ↑
Primary Key (uniquely identifies each row)
```

---

### 🔑 Keys — The "Relational" Part

The power of RDBMS comes from **linking tables together using keys**:

<details>
<summary>📌 <strong>Click to expand: Primary Key vs Foreign Key explained</strong></summary>

**Primary Key** — Uniquely identifies every row in a table. No duplicates, no NULLs.
```sql
CREATE TABLE students (
    id INT PRIMARY KEY,   -- ← Primary Key
    name VARCHAR(100)
);
```

**Foreign Key** — Links one table to another, creating the "relationship."
```sql
CREATE TABLE grades (
    student_id INT,
    course_id  INT,
    score      DECIMAL,
    FOREIGN KEY (student_id) REFERENCES students(id)  -- ← Foreign Key
);
```

This means every grade *belongs to* a student — the relationship is enforced by the database itself.

</details>

---

### 🌍 Popular RDBMS Systems

| System | Best For | License |
|--------|----------|---------|
| ![MySQL](https://img.shields.io/badge/MySQL-005C84?style=flat&logo=mysql&logoColor=white) | Web apps, general use | Open Source |
| ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white) | Complex queries, enterprise | Open Source |
| ![SQLite](https://img.shields.io/badge/SQLite-07405E?style=flat&logo=sqlite&logoColor=white) | Mobile apps, embedded | Public Domain |
| ![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white) | Microsoft ecosystem | Commercial |
| ![Oracle](https://img.shields.io/badge/Oracle-F80000?style=flat&logo=oracle&logoColor=white) | Large enterprises | Commercial |

---

### 🏛️ ACID Properties

RDBMS follows **ACID** — four guarantees that make database transactions reliable:

| Property | Meaning | Simple Explanation |
|----------|---------|-------------------|
| **A**tomicity | All or nothing | A transaction either fully completes or fully fails — no partial updates |
| **C**onsistency | Always valid | Data always moves from one valid state to another |
| **I**solation | No interference | Concurrent transactions don't affect each other |
| **D**urability | Permanent | Once committed, data survives crashes and failures |

> ACID properties were defined by **E.F. Codd**, the mathematician who invented the relational model at IBM in **1970**.

---

## 🔄 How They Connect

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│   SQL  ──────────►  RDBMS  ──────────►  Database   │
│                                                     │
│  Language you      Software that       Where data   │
│  write queries     manages data        is stored    │
│  in                using relations                  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

In one sentence:

> **A database stores the data. An RDBMS is the software that manages that database using the relational model. SQL is the language you use to interact with the RDBMS.**

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: SQL Commands Cheat Sheet</strong></summary>

```sql
-- ── DATA QUERYING ─────────────────────────────────────
SELECT * FROM table_name;                          -- Get all rows
SELECT col1, col2 FROM table WHERE condition;      -- Filter rows
SELECT * FROM table ORDER BY col DESC LIMIT 10;    -- Sort & limit

-- ── DATA MANIPULATION ─────────────────────────────────
INSERT INTO table (col1, col2) VALUES ('a', 'b'); -- Add a row
UPDATE table SET col1 = 'new' WHERE id = 1;       -- Edit a row
DELETE FROM table WHERE id = 1;                   -- Remove a row

-- ── STRUCTURE (DDL) ───────────────────────────────────
CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(100));
ALTER TABLE users ADD COLUMN email VARCHAR(255);
DROP TABLE users;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **SQL** | Language used to communicate with a relational database |
| **Database** | Organized collection of structured data |
| **DBMS** | Software that manages a database |
| **RDBMS** | DBMS that uses the relational (table-based) model |
| **Table** | Grid of rows and columns storing one type of entity |
| **Row** | A single record in a table |
| **Column** | A single attribute/field in a table |
| **Primary Key** | Unique identifier for each row in a table |
| **Foreign Key** | A column that references the primary key of another table |
| **CRUD** | Create, Read, Update, Delete — the four data operations |
| **ACID** | Atomicity, Consistency, Isolation, Durability |
| **E.F. Codd** | Inventor of the relational model (1970, IBM) |

</details>

---

<div align="center">

**Part of the SQL Mastery Journey — March to April 2026**

[![YouTube](https://img.shields.io/badge/YouTube-code4coin-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@code4coin)
[![GitHub](https://img.shields.io/badge/GitHub-code4coin-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/code4coin)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nitin22-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/nitin22/)

*⭐ Star this repo to follow the journey — one query at a time*

</div>
