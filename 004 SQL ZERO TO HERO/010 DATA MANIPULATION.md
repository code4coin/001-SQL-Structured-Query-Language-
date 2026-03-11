# 📝 Inserting, Updating & Deleting Data — PostgreSQL

> *The four data manipulation essentials — INSERT, UPDATE, DELETE, and RETURNING — all demonstrated with a real student database.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-DML-22C55E?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-3B82F6?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-6.1--6.4-gray?style=for-the-badge)

---
## ☕ Support This Journey

<div align="center">
Every query I write, every concept I break down,
every late-night debugging session —
it all goes into making this content free for you.

### If this helped you, consider buying me a coffee! 🙏

<a href="https://buymeacoffee.com/code4coin">
  <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-%23FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" alt="Buy Me A Coffee" height="50"/>
</a>

&nbsp;

| ☕ | What your support means |
|:---:|:---|
| **1 Coffee** | Keeps me caffeinated through a 2-hour study session |
| **3 Coffees** | Funds a full week of content research & notes |
| **5 Coffees** | Powers an entire month of the SQL Mastery Journey |

&nbsp;

> 💬 *"I'm learning in public, sharing everything for free.*
> *Your support helps me keep going — one coffee at a time."*
> — Nitin, **@code4coin**

&nbsp;
</div>
---

## 📋 Table of Contents

- [DML vs DDL — Quick Distinction](#-dml-vs-ddl--quick-distinction)
- [Our Practice Table](#-our-practice-table)
- [6.1 Inserting Data](#-61-inserting-data)
- [6.2 Updating Data](#-62-updating-data)
- [6.3 Deleting Data](#-63-deleting-data)
- [6.4 RETURNING — Get Data Back from Modified Rows](#-64-returning--get-data-back-from-modified-rows)
- [Quick Reference](#-quick-reference)

---

## ⚡ DML vs DDL — Quick Distinction

```
DDL — Data Definition Language      DML — Data Manipulation Language
────────────────────────────        ────────────────────────────────
Modifies TABLE STRUCTURE            Modifies TABLE DATA
CREATE, ALTER, DROP                 INSERT, UPDATE, DELETE, SELECT

"Change the shape of the box"       "Change what's inside the box"
```

> This chapter is all about **DML** — working with the data inside your tables.

---

## 🏗️ Our Practice Table

All examples use the `student.stud_dtl` table from the previous chapter:

```sql
-- Setup (from Chapter 5):
DROP SCHEMA IF EXISTS student CASCADE;
CREATE SCHEMA student;

CREATE TABLE student.stud_dtl (
    id    integer  GENERATED ALWAYS AS IDENTITY  PRIMARY KEY,
    name  text,
    age   int  CONSTRAINT age_check_18 CHECK (age >= 18),
    uni_id integer
);
```

```
📂 student (schema)
└── 📊 stud_dtl
     ├── id      integer  GENERATED ALWAYS AS IDENTITY  PRIMARY KEY
     ├── name    text
     ├── age     int  CHECK (age >= 18)
     └── uni_id  integer
```

---

## ➕ 6.1 Inserting Data

A new table starts empty. `INSERT` is how data enters a table — **one complete row at a time**, even if some column values are defaults or NULLs.

### Method 1 — Values Only (positional)

```sql
-- Values must match the column order exactly:
INSERT INTO student.stud_dtl VALUES (DEFAULT, 'Nitin', 20, 101);
--                                   ↑         ↑        ↑    ↑
--                                   id        name    age  uni_id
```

> ⚠️ Fragile — if the table structure changes, this breaks silently or inserts wrong values.

---

### Method 2 — Explicit Column Names ✅ Recommended

```sql
-- Always name your columns — order-independent and self-documenting:
INSERT INTO student.stud_dtl (name, age, uni_id)
VALUES ('Nitin', 20, 101);

-- Columns can be in any order — PostgreSQL matches by name, not position:
INSERT INTO student.stud_dtl (uni_id, name, age)
VALUES (102, 'Robert', 35);
```

**Result:**

```
 id │ name   │ age │ uni_id
────┼────────┼─────┼────────
  1 │ Nitin  │  20 │  101
  2 │ Robert │  35 │  102
```

> 💡 **Best practice:** Always list column names explicitly. It makes inserts readable, safe, and resistant to schema changes.

---

### Method 3 — Omitting Columns (uses defaults)

```sql
-- Omit uni_id — it will be NULL (default):
INSERT INTO student.stud_dtl (name, age) VALUES ('Alice', 22);

-- Omit everything — ALL columns use their defaults:
INSERT INTO student.stud_dtl DEFAULT VALUES;
-- id = next sequence value, name = NULL, age = NULL, uni_id = NULL
```

---

### Method 4 — Explicit DEFAULT Keyword

```sql
-- Use DEFAULT for specific columns, supply values for others:
INSERT INTO student.stud_dtl (id, name, age, uni_id)
VALUES (DEFAULT, 'Bob', 28, DEFAULT);
--      ↑ identity auto-generates    ↑ uni_id gets NULL (its default)
```

---

### Method 5 — Multi-Row Insert ✅ Efficient

```sql
-- Insert multiple rows in a single command:
INSERT INTO student.stud_dtl (name, age, uni_id)
VALUES
    ('Nitin',   20, 101),
    ('Robert',  35, 102),
    ('Alice',   22, 101),
    ('Bob',     28, 103),
    ('Charlie', 19, 102);
```

**Result:**

```
 id │ name    │ age │ uni_id
────┼─────────┼─────┼────────
  1 │ Nitin   │  20 │  101
  2 │ Robert  │  35 │  102
  3 │ Alice   │  22 │  101
  4 │ Bob     │  28 │  103
  5 │ Charlie │  19 │  102
```

> 💡 Multi-row insert is significantly **faster** than running individual INSERT statements — fewer round-trips to the database.

---

### Method 6 — INSERT from a SELECT Query

```sql
-- Copy students from a staging table who enrolled today:
INSERT INTO student.stud_dtl (name, age, uni_id)
    SELECT name, age, uni_id
    FROM student.stud_staging
    WHERE enrolled_date = CURRENT_DATE;
```

<details>
<summary>📌 <strong>Click to expand: INSERT … SELECT — when is this useful?</strong></summary>

```sql
-- Use case 1: Migrate data from one table to another
INSERT INTO student.stud_dtl (name, age, uni_id)
    SELECT name, age, uni_id FROM student.old_stud_dtl;

-- Use case 2: Copy rows that match a condition
INSERT INTO student.stud_archive (name, age, uni_id)
    SELECT name, age, uni_id
    FROM student.stud_dtl
    WHERE age > 30;

-- Use case 3: Transform data during insert
INSERT INTO student.stud_upper (name, age)
    SELECT UPPER(name), age
    FROM student.stud_dtl;
```

</details>

---

## ✏️ 6.2 Updating Data

`UPDATE` modifies data already in the table. It needs three things:

```
1. Table name          → WHERE to make the change
2. New value(s)        → WHAT to change (SET clause)
3. Target row(s)       → WHICH rows to change (WHERE clause)
```

### Basic Syntax

```sql
UPDATE table_name
SET    column = new_value
WHERE  condition;
```

---

### Update a Single Row by Primary Key

```sql
-- Change Nitin's university to 105:
UPDATE student.stud_dtl
SET    uni_id = 105
WHERE  id = 1;
```

```
Before:                          After:
id=1 │ Nitin │ 20 │ uni_id=101   id=1 │ Nitin │ 20 │ uni_id=105 ✅
```

---

### Update Using an Expression

```sql
-- Age everyone in the table by 1 year (no WHERE = ALL rows):
UPDATE student.stud_dtl
SET    age = age + 1;
--          ↑ expression can reference the existing value
```

> ⚠️ **No WHERE clause = every row is updated.** Always double-check before running without WHERE.

---

### Update Multiple Columns at Once

```sql
-- Update both name and university for student id=2:
UPDATE student.stud_dtl
SET    name   = 'Rob',
       uni_id = 110
WHERE  id = 2;
```

---

### Update a Subset of Rows

```sql
-- Assign university 101 to all students under age 25:
UPDATE student.stud_dtl
SET    uni_id = 101
WHERE  age < 25;

-- Update students who have no university assigned yet:
UPDATE student.stud_dtl
SET    uni_id = 999
WHERE  uni_id IS NULL;
```

<details>
<summary>📌 <strong>Click to expand: SET uses = as assignment, WHERE uses = as comparison</strong></summary>

```sql
UPDATE student.stud_dtl
SET    age = 25          -- ← assignment: SET age TO 25
WHERE  id  = 1;          -- ← comparison: WHERE id EQUALS 1

-- Same symbol, two different meanings — but no ambiguity in context.
-- SET clause   → always assignment
-- WHERE clause → always comparison
```

</details>

---

## 🗑️ 6.3 Deleting Data

`DELETE` removes **entire rows** from a table — you cannot delete individual column values (use `UPDATE … SET col = NULL` for that).

### Delete Specific Rows

```sql
-- Delete student with id = 5:
DELETE FROM student.stud_dtl WHERE id = 5;

-- Delete all students from university 103:
DELETE FROM student.stud_dtl WHERE uni_id = 103;

-- Delete students under 18 who slipped past the CHECK constraint:
DELETE FROM student.stud_dtl WHERE age < 18;
```

---

### Delete ALL Rows ⚠️

```sql
-- This deletes every row in the table — structure remains intact:
DELETE FROM student.stud_dtl;
```

```
Before:                               After:
 id │ name   │ age │ uni_id            id │ name │ age │ uni_id
────┼────────┼─────┼────────           ────┴──────┴─────┴────────
  1 │ Nitin  │ 20  │ 101               (0 rows)
  2 │ Robert │ 35  │ 102
  3 │ Alice  │ 22  │ 101
```

> ⚠️ No WHERE = ALL rows gone. The **table structure** (columns, constraints) is preserved — only the data is removed.

<details>
<summary>📌 <strong>Click to expand: DELETE vs TRUNCATE — which is faster?</strong></summary>

```
DELETE FROM student.stud_dtl;        TRUNCATE student.stud_dtl;
──────────────────────────────       ──────────────────────────
Removes rows one-by-one              Removes all rows instantly
Fires DELETE triggers                Does NOT fire row-level triggers
Can be rolled back (transactional)   Can be rolled back (transactional)
RETURNING works                      RETURNING does NOT work
Slower on large tables               Much faster on large tables
Resets identity sequence? ❌ No      Resets identity sequence? ✅ Optional

Use DELETE when:                     Use TRUNCATE when:
• You need a WHERE clause            • You want ALL rows gone fast
• You need RETURNING                 • You want the sequence reset
• You need trigger behaviour         • Performance matters on big tables
```

</details>

---

## 🔄 6.4 RETURNING — Get Data Back from Modified Rows

`RETURNING` lets you retrieve column values from rows **as they are being inserted, updated, or deleted** — in a single command, without a follow-up `SELECT`.

```
Without RETURNING:          With RETURNING:
──────────────────          ──────────────────────────────
INSERT → (run)              INSERT → returns inserted row(s)
SELECT → (extra query)      Done in one command ✅
```

---

### RETURNING with INSERT — Get the Auto-Generated ID

```sql
-- Insert a new student and immediately get the assigned id back:
INSERT INTO student.stud_dtl (name, age, uni_id)
VALUES ('Diana', 24, 104)
RETURNING id;
```

**Result:**

```
 id
────
  6     ← the auto-generated identity value, returned instantly
```

```sql
-- Return multiple columns after insert:
INSERT INTO student.stud_dtl (name, age, uni_id)
VALUES ('Evan', 21, 101)
RETURNING id, name, age;
```

**Result:**

```
 id │ name │ age
────┼──────┼─────
  7 │ Evan │  21
```

```sql
-- Return everything:
INSERT INTO student.stud_dtl (name, age, uni_id)
VALUES ('Fiona', 26, 102)
RETURNING *;
```

---

### RETURNING with UPDATE — See the New Values

```sql
-- Raise all students' age by 1 and see what changed:
UPDATE student.stud_dtl
SET    age = age + 1
WHERE  uni_id = 101
RETURNING id, name, age;
```

**Result:**

```
 id │ name  │ age
────┼───────┼─────
  1 │ Nitin │  21   ← was 20, now 21
  3 │ Alice │  23   ← was 22, now 23
```

---

### RETURNING with UPDATE — Compare Old and New Values

```sql
-- Update uni_id for all students under 25 and compare before/after:
UPDATE student.stud_dtl
SET    uni_id = 200
WHERE  age < 25
RETURNING
    id,
    name,
    old.uni_id AS old_university,
    new.uni_id AS new_university,
    new.uni_id - old.uni_id AS difference;
```

**Result:**

```
 id │ name  │ old_university │ new_university │ difference
────┼───────┼────────────────┼────────────────┼────────────
  1 │ Nitin │      101       │      200       │     99
  3 │ Alice │      101       │      200       │     99
  5 │ Charlie│     102       │      200       │     98
```

> 💡 `old.column` and `new.column` syntax lets you see **exactly what changed** in a single query — no before/after SELECT needed.

---

### RETURNING with DELETE — Capture Deleted Rows

```sql
-- Delete all students from university 103 and capture who was removed:
DELETE FROM student.stud_dtl
WHERE  uni_id = 103
RETURNING *;
```

**Result:**

```
 id │ name │ age │ uni_id
────┼──────┼─────┼────────
  4 │ Bob  │  28 │  103    ← row is deleted, but returned here first
```

> 💡 Extremely useful for **audit logs** — you can capture deleted records before they disappear.

<details>
<summary>📌 <strong>Click to expand: RETURNING for audit logging pattern</strong></summary>

```sql
-- Log deleted students to an archive table in one step:
INSERT INTO student.stud_archive (id, name, age, uni_id, deleted_at)
SELECT id, name, age, uni_id, CURRENT_TIMESTAMP
FROM (
    DELETE FROM student.stud_dtl
    WHERE uni_id = 103
    RETURNING id, name, age, uni_id
) AS deleted_rows;

-- One command deletes the rows AND saves them to the archive.
-- No risk of the data disappearing before you can log it.
```

</details>

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All DML syntax at a glance</strong></summary>

```sql
-- ── INSERT ───────────────────────────────────────────────────────
-- Positional (fragile):
INSERT INTO student.stud_dtl VALUES (DEFAULT, 'Nitin', 20, 101);

-- Named columns (recommended):
INSERT INTO student.stud_dtl (name, age, uni_id) VALUES ('Nitin', 20, 101);

-- Omit columns (use defaults):
INSERT INTO student.stud_dtl (name, age) VALUES ('Nitin', 20);

-- All defaults:
INSERT INTO student.stud_dtl DEFAULT VALUES;

-- Explicit DEFAULT keyword:
INSERT INTO student.stud_dtl (id, name, age, uni_id) VALUES (DEFAULT, 'Nitin', 20, DEFAULT);

-- Multi-row:
INSERT INTO student.stud_dtl (name, age, uni_id) VALUES
    ('Nitin',  20, 101),
    ('Robert', 35, 102);

-- From a SELECT:
INSERT INTO student.stud_dtl (name, age, uni_id)
    SELECT name, age, uni_id FROM student.stud_staging;

-- ── UPDATE ───────────────────────────────────────────────────────
-- Single column, specific row:
UPDATE student.stud_dtl SET uni_id = 105 WHERE id = 1;

-- Multiple columns:
UPDATE student.stud_dtl SET name = 'Rob', uni_id = 110 WHERE id = 2;

-- Expression (reference existing value):
UPDATE student.stud_dtl SET age = age + 1;

-- Conditional:
UPDATE student.stud_dtl SET uni_id = 101 WHERE age < 25;

-- ── DELETE ───────────────────────────────────────────────────────
-- Specific rows:
DELETE FROM student.stud_dtl WHERE id = 5;

-- All rows (dangerous!):
DELETE FROM student.stud_dtl;

-- ── RETURNING ────────────────────────────────────────────────────
INSERT INTO student.stud_dtl (name, age) VALUES ('Nitin', 20) RETURNING id;
INSERT INTO student.stud_dtl (name, age) VALUES ('Nitin', 20) RETURNING *;

UPDATE student.stud_dtl SET age = age + 1 WHERE uni_id = 101
    RETURNING id, name, age;

UPDATE student.stud_dtl SET uni_id = 200 WHERE age < 25
    RETURNING id, old.uni_id AS before, new.uni_id AS after;

DELETE FROM student.stud_dtl WHERE uni_id = 103 RETURNING *;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **DML** | Data Manipulation Language — INSERT, UPDATE, DELETE, SELECT |
| **INSERT** | Adds one or more complete rows to a table |
| **UPDATE** | Modifies existing column values in one or more rows |
| **DELETE** | Removes entire rows from a table |
| **RETURNING** | Clause that returns column values from rows as they are modified |
| **DEFAULT** | Keyword in INSERT/UPDATE to explicitly request a column's default value |
| **DEFAULT VALUES** | Insert a complete row using all column defaults |
| **SET** | Clause in UPDATE specifying which columns to change and their new values |
| **WHERE** | Clause that filters which rows are affected by UPDATE or DELETE |
| **Multi-row INSERT** | A single INSERT command that adds multiple rows at once |
| **INSERT … SELECT** | An INSERT that sources its values from a query |
| **old.column** | In RETURNING — the value of a column before the UPDATE |
| **new.column** | In RETURNING — the value of a column after the UPDATE |
| **TRUNCATE** | A faster alternative to DELETE for removing all rows |

</details>

---

<div align="center">
  
**Every supporter gets a shoutout in the journey log.** 🎉

[![Support Now](https://img.shields.io/badge/☕%20Support%20Now-buymeacoffee.com%2Fcode4coin-FFDD00?style=for-the-badge)](https://buymeacoffee.com/code4coin)

---


**Part of the SQL Mastery Journey — March to April 2026**

[![YouTube](https://img.shields.io/badge/YouTube-code4coin-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@code4coin)
[![GitHub](https://img.shields.io/badge/GitHub-code4coin-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/code4coin)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nitin22-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/nitin22/)

*⭐ Star this repo to follow the journey — one query at a time*


</div>
