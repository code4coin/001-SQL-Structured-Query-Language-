# 🔍 Querying Data — FROM, SELECT & Logical Operators — PostgreSQL

> *The most powerful tool in SQL — retrieving exactly the data you need, from simple lookups to complex transformations.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-SELECT%20%26%20Queries-10B981?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-3B82F6?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-7.1%20|%207.2%20|%209.1-gray?style=for-the-badge)

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

- [What is a Query?](#-what-is-a-query)
- [SELECT Anatomy](#-select-anatomy)
- [7.1 The SELECT Command](#-71-the-select-command)
- [7.2.1 The FROM Clause](#-721-the-from-clause)
- [The Query Pipeline](#-the-query-pipeline)
- [9.1 Logical Operators](#-91-logical-operators)
- [Putting It All Together](#-putting-it-all-together)
- [Quick Reference](#-quick-reference)

---

## 🤔 What is a Query?

> **Query** = the process of *retrieving* data from a database. In SQL, that means `SELECT`.

```
Writing data   →   INSERT, UPDATE, DELETE   (DML - covered in Chapter 6)
Reading data   →   SELECT                   (Queries - this chapter)
```

```
SELECT is the most frequently used SQL command.
Every dashboard, every report, every API call that reads data
ultimately becomes a SELECT statement at the database level.
```

---

## 🧬 SELECT Anatomy

```
[WITH with_queries]  SELECT  select_list  FROM  table_expression  [sort_specification]
 ↑                    ↑       ↑             ↑     ↑                  ↑
 Optional CTEs        keyword What columns  keyword Which table(s)   ORDER BY
 (advanced)                   to return                              (optional)
```

The **pipeline** of transformations inside a query:

```
FROM clause
  └── produces a raw virtual table from base tables / joins / subqueries
       └── WHERE clause
             └── filters rows
                  └── GROUP BY clause
                        └── groups rows
                             └── HAVING clause
                                   └── filters groups
                                        └── SELECT list
                                              └── picks columns / computes expressions
                                                   └── ORDER BY
                                                         └── final sorted result ✅
```

---

## 📥 7.1 The SELECT Command

### The Simplest Query — All Rows, All Columns

```sql
-- Retrieve everything from student.stud_dtl:
SELECT * FROM student.stud_dtl;
--      ↑
--      * means "all columns the table provides"
```

**Result:**

```
 id │ name   │ age │ uni_id │ is_active │ status
────┼────────┼─────┼────────┼───────────┼──────────
  1 │ Nitin  │  20 │  101   │     t     │ enrolled
  2 │ Robert │  35 │  102   │     t     │ graduated
  3 │ Alice  │  22 │  101   │     f     │ enrolled
```

> ⚠️ Using `SELECT *` is fine for exploration, but in production code always name your columns explicitly — schema changes can break code that relies on column order.

---

### Selecting Specific Columns

```sql
-- Only retrieve name and age:
SELECT name, age FROM student.stud_dtl;
```

```
 name   │ age
────────┼─────
 Nitin  │  20
 Robert │  35
 Alice  │  22
```

---

### Computed Columns — Expressions in the Select List

The select list isn't limited to column names — you can write any expression:

```sql
-- Calculate age in months and years displayed differently:
SELECT
    name,
    age,
    age * 12               AS age_in_months,
    age + 5                AS age_in_5_years
FROM student.stud_dtl;
```

```
 name   │ age │ age_in_months │ age_in_5_years
────────┼─────┼───────────────┼────────────────
 Nitin  │  20 │     240       │      25
 Robert │  35 │     420       │      40
 Alice  │  22 │     264       │      27
```

```sql
-- Combine columns into a single display string:
SELECT
    id,
    name || ' (Age: ' || age || ')' AS student_summary,
    uni_id
FROM student.stud_dtl;
```

```
 id │ student_summary      │ uni_id
────┼──────────────────────┼────────
  1 │ Nitin (Age: 20)      │  101
  2 │ Robert (Age: 35)     │  102
  3 │ Alice (Age: 22)      │  101
```

---

### SELECT Without a Table — Using SELECT as a Calculator

You can run SELECT without any FROM clause:

```sql
-- Pure calculations:
SELECT 3 * 4;           -- returns 12
SELECT 100 / 4.0;       -- returns 25.0

-- String operations:
SELECT UPPER('nitin');  -- returns 'NITIN'
SELECT LENGTH('code4coin');  -- returns 9

-- Date functions:
SELECT CURRENT_DATE;           -- today's date
SELECT CURRENT_TIMESTAMP;      -- current date + time
SELECT AGE('2006-03-01');      -- age from that date to today

-- Random number:
SELECT random();        -- returns a float between 0 and 1, different each call
```

```
 ?column?
──────────
    12

 ?column?
──────────────────────
 2026-03-13 10:30:00
```

> 💡 This is useful for quickly testing expressions, functions, or type conversions without needing a table.

---

## 📋 7.2.1 The FROM Clause

The `FROM` clause is where the data comes from. It can be a single table, multiple tables, a subquery, a join, or combinations of all of these.

### Single Table

```sql
-- Most common — reading from one table:
SELECT name, age, status
FROM student.stud_dtl;
--   ↑ schema-qualified table name
```

---

### Multiple Tables in FROM — Cartesian Product ⚠️

Listing multiple tables without a JOIN produces a **Cartesian product** — every row from table A combined with every row from table B:

```sql
-- Two tables with no join condition:
SELECT s.name, u.university_name
FROM student.stud_dtl s, student.university_detail u;
```

```
If stud_dtl has 3 rows and university_detail has 2 rows:
Result = 3 × 2 = 6 rows (every student paired with every university)

 name   │ university_name
────────┼─────────────────
 Nitin  │ MIT
 Nitin  │ HAVARDS
 Robert │ MIT
 Robert │ HAVARDS
 Alice  │ MIT
 Alice  │ HAVARDS
```

> ⚠️ Almost always unintentional — use `JOIN` with a condition to get meaningful results (covered in the Joins chapter).

---

### Schema-Qualified Table Names

```sql
-- With schema qualifier (explicit, unambiguous):
SELECT * FROM student.stud_dtl;

-- Without schema (PostgreSQL looks in search_path):
SELECT * FROM stud_dtl;
-- ✅ Works if 'student' schema is in search_path
-- ❌ Fails or picks the wrong table if not
```

<details>
<summary>📌 <strong>Click to expand: Table inheritance and the ONLY keyword</strong></summary>

```sql
-- By default, querying a parent table also returns rows from child tables:
SELECT * FROM parent_table;     -- includes rows from all child tables

-- To query ONLY the parent table itself:
SELECT * FROM ONLY parent_table;

-- Explicit inclusion of children (redundant — it's the default):
SELECT * FROM parent_table *;
```

> This applies when using PostgreSQL's table inheritance feature. For most day-to-day work with `student.stud_dtl`, a single non-inherited table, this distinction doesn't apply.

</details>

---

## ⚙️ The Query Pipeline

Here's the full pipeline applied to `student.stud_dtl`:

```sql
SELECT
    name,
    age,
    uni_id
FROM   student.stud_dtl          -- Step 1: start with this table
WHERE  is_active = TRUE           -- Step 2: keep only active students
  AND  age >= 20                  -- Step 3: keep only age >= 20
ORDER BY age ASC;                 -- Step 4: sort youngest first
```

```
Step 1 — FROM: all 3 rows loaded
┌────────┬─────┬────────┬───────────┐
│ name   │ age │ uni_id │ is_active │
├────────┼─────┼────────┼───────────┤
│ Nitin  │ 20  │ 101    │ t         │
│ Robert │ 35  │ 102    │ t         │
│ Alice  │ 22  │ 101    │ f         │
└────────┴─────┴────────┴───────────┘

Step 2 — WHERE is_active = TRUE: Alice filtered out
┌────────┬─────┬────────┐
│ Nitin  │ 20  │ 101    │
│ Robert │ 35  │ 102    │
└────────┴─────┴────────┘

Step 3 — WHERE age >= 20: both pass (20 >= 20 ✅, 35 >= 20 ✅)

Step 4 — ORDER BY age ASC:
┌────────┬─────┬────────┐
│ Nitin  │ 20  │ 101    │  ← youngest first
│ Robert │ 35  │ 102    │
└────────┴─────┴────────┘
```

---

## ⚡ 9.1 Logical Operators

Logical operators combine Boolean conditions in `WHERE` clauses. PostgreSQL uses a **three-valued logic** system: `TRUE`, `FALSE`, and `NULL` (unknown).

### The Three Operators

```sql
condition1 AND condition2    -- both must be true
condition1 OR  condition2    -- at least one must be true
NOT condition                -- inverts the result
```

---

### AND — Both Must Be True

```sql
-- Students who are both active AND under 30:
SELECT name, age, is_active
FROM student.stud_dtl
WHERE is_active = TRUE
  AND age < 30;
```

```
 name  │ age │ is_active
───────┼─────┼───────────
 Nitin │ 20  │     t      ← active AND under 30 ✅
```

---

### OR — At Least One Must Be True

```sql
-- Students who are either from uni 101 OR are graduated:
SELECT name, uni_id, status
FROM student.stud_dtl
WHERE uni_id = 101
   OR status = 'graduated';
```

```
 name   │ uni_id │ status
────────┼────────┼──────────
 Nitin  │  101   │ enrolled   ← uni_id = 101 ✅
 Robert │  102   │ graduated  ← status = 'graduated' ✅
 Alice  │  101   │ enrolled   ← uni_id = 101 ✅
```

---

### NOT — Inverts the Condition

```sql
-- Students who are NOT graduated:
SELECT name, status
FROM student.stud_dtl
WHERE NOT status = 'graduated';

-- Equivalent shorthand:
SELECT name, status
FROM student.stud_dtl
WHERE status <> 'graduated';
```

```
 name  │ status
───────┼──────────
 Nitin │ enrolled
 Alice │ enrolled
```

---

### Combining AND + OR — Operator Precedence Matters

`AND` binds more tightly than `OR` — use parentheses to be explicit:

```sql
-- ⚠️ Ambiguous intent — what does this return?
SELECT name FROM student.stud_dtl
WHERE uni_id = 101 OR uni_id = 102 AND age > 25;

-- PostgreSQL evaluates AND first:
-- → uni_id = 101  OR  (uni_id = 102 AND age > 25)
-- → Returns: Nitin (uni_id=101), Alice (uni_id=101), Robert (uni_id=102 AND age=35>25)

-- ✅ Use parentheses to make intent explicit:
SELECT name FROM student.stud_dtl
WHERE (uni_id = 101 OR uni_id = 102) AND age > 25;
-- → Returns only: Robert (age 35 > 25, and uni_id is 101 or 102)
```

> 💡 **Always use parentheses** when mixing AND and OR — it prevents bugs and makes your intent clear to anyone reading the query.

---

### The Three-Valued Truth Tables

PostgreSQL's three-valued logic (`TRUE`, `FALSE`, `NULL`) means NULL behaves differently than you might expect:

**AND truth table:**

| a | b | a AND b |
|---|---|---------|
| TRUE | TRUE | **TRUE** |
| TRUE | FALSE | **FALSE** |
| TRUE | NULL | **NULL** ← unknown |
| FALSE | FALSE | **FALSE** |
| FALSE | NULL | **FALSE** ← FALSE wins |
| NULL | NULL | **NULL** |

**OR truth table:**

| a | b | a OR b |
|---|---|--------|
| TRUE | TRUE | **TRUE** |
| TRUE | FALSE | **TRUE** |
| TRUE | NULL | **TRUE** ← TRUE wins |
| FALSE | FALSE | **FALSE** |
| FALSE | NULL | **NULL** ← unknown |
| NULL | NULL | **NULL** |

**NOT truth table:**

| a | NOT a |
|---|-------|
| TRUE | **FALSE** |
| FALSE | **TRUE** |
| NULL | **NULL** ← NOT unknown = still unknown |

---

### NULL in WHERE Clauses — The Common Trap

```sql
-- Add a student with no university assigned:
INSERT INTO student.stud_dtl (name, age) VALUES ('Dave', 29);
-- uni_id = NULL for Dave

-- ❌ This does NOT find Dave:
SELECT name FROM student.stud_dtl WHERE uni_id = NULL;
-- Returns: (0 rows) — NULL = NULL is NULL, not TRUE

-- ❌ This does NOT find Dave either:
SELECT name FROM student.stud_dtl WHERE uni_id <> NULL;
-- Returns: (0 rows) — NULL <> NULL is NULL, not TRUE

-- ✅ Correct way to check for NULL:
SELECT name FROM student.stud_dtl WHERE uni_id IS NULL;
-- Returns: Dave ✅

-- ✅ Correct way to check for NOT NULL:
SELECT name FROM student.stud_dtl WHERE uni_id IS NOT NULL;
-- Returns: Nitin, Robert, Alice ✅
```

<details>
<summary>📌 <strong>Click to expand: Why does NULL behave this way?</strong></summary>

```
NULL means "unknown value" — not zero, not empty string, just unknown.

Comparing unknown = anything → still unknown (NULL)
Because: if you don't know what the value is,
         you can't know whether it equals something.

This is why:
  NULL =  5   → NULL (not FALSE)
  NULL <> 5   → NULL (not TRUE)
  NULL =  NULL → NULL (not TRUE — even unknown = unknown is unknown)

The ONLY correct way to test for NULL:
  IS NULL      → returns TRUE if the value is NULL
  IS NOT NULL  → returns TRUE if the value is not NULL

And NULL in WHERE:
  WHERE condition   only keeps rows where condition = TRUE
  Rows where condition = NULL are also discarded (like FALSE)
```

</details>

---

### Real Queries on student.stud_dtl

```sql
-- 1. Active students from university 101:
SELECT name, age, status
FROM student.stud_dtl
WHERE is_active = TRUE
  AND uni_id = 101;

-- 2. Students who are enrolled OR on leave:
SELECT name, status
FROM student.stud_dtl
WHERE status = 'enrolled'
   OR status = 'on_leave';

-- 3. Students NOT from university 102 who are active:
SELECT name, uni_id, is_active
FROM student.stud_dtl
WHERE NOT uni_id = 102
  AND is_active = TRUE;

-- 4. Students with no university assigned yet:
SELECT name, uni_id
FROM student.stud_dtl
WHERE uni_id IS NULL;

-- 5. Active students aged 20-30 in universities 101 or 102:
SELECT name, age, uni_id
FROM student.stud_dtl
WHERE is_active = TRUE
  AND age BETWEEN 20 AND 30
  AND (uni_id = 101 OR uni_id = 102);
```

---

## 🔁 Putting It All Together

```sql
-- Full query using everything from this chapter:
SELECT
    id,
    name || ' — ' || status        AS student_info,   -- computed column
    age,
    age * 12                        AS age_months,     -- expression
    CASE
        WHEN age < 25 THEN 'Young'
        WHEN age < 35 THEN 'Mid'
        ELSE 'Senior'
    END                             AS age_group       -- conditional expression
FROM   student.stud_dtl                               -- FROM clause
WHERE  is_active = TRUE                               -- logical AND
  AND  (uni_id = 101 OR uni_id = 102)                 -- logical OR with parens
  AND  status IS NOT NULL                             -- NULL check
ORDER BY age ASC;                                     -- sort specification
```

**Result:**

```
 id │ student_info          │ age │ age_months │ age_group
────┼───────────────────────┼─────┼────────────┼───────────
  1 │ Nitin — enrolled      │ 20  │    240     │ Young
  2 │ Robert — graduated    │ 35  │    420     │ Senior
```

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All SELECT patterns at a glance</strong></summary>

```sql
-- ── BASIC SELECT ──────────────────────────────────────────────────
SELECT * FROM student.stud_dtl;                        -- all columns
SELECT name, age FROM student.stud_dtl;                -- specific columns
SELECT name, age * 12 AS months FROM student.stud_dtl; -- computed column

-- ── SELECT WITHOUT TABLE ──────────────────────────────────────────
SELECT 3 * 4;
SELECT CURRENT_DATE;
SELECT random();
SELECT UPPER('nitin');

-- ── WHERE WITH LOGICAL OPERATORS ──────────────────────────────────
WHERE is_active = TRUE AND age < 30
WHERE uni_id = 101 OR uni_id = 102
WHERE NOT status = 'dropped'
WHERE (uni_id = 101 OR uni_id = 102) AND age > 20   -- parentheses for clarity

-- ── NULL CHECKS ───────────────────────────────────────────────────
WHERE uni_id IS NULL                                  -- find NULLs
WHERE uni_id IS NOT NULL                              -- exclude NULLs
-- Never use: WHERE uni_id = NULL  ← always returns 0 rows

-- ── FULL QUERY STRUCTURE ──────────────────────────────────────────
SELECT column1, expression AS alias
FROM   schema.table
WHERE  condition1 AND condition2
ORDER BY column1 ASC, column2 DESC;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Query** | The process of retrieving data from a database |
| **SELECT** | The SQL command used to write queries |
| **select list** | The part of SELECT that specifies which columns/expressions to return |
| **`*`** | Wildcard — means "all columns the table provides" |
| **alias** | A temporary name for a column or expression, using `AS` |
| **FROM clause** | Specifies which table(s) the query reads from |
| **table expression** | The full FROM + WHERE + GROUP BY + HAVING block |
| **Cartesian product** | Result of joining two tables with no join condition — all rows × all rows |
| **schema-qualified** | Using `schema.table` notation to be explicit about which table |
| **WHERE clause** | Filters rows based on a condition |
| **pipeline** | The sequence of transformations: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY |
| **AND** | Logical operator: both conditions must be TRUE |
| **OR** | Logical operator: at least one condition must be TRUE |
| **NOT** | Logical operator: inverts a condition |
| **three-valued logic** | SQL's TRUE / FALSE / NULL (unknown) system |
| **NULL** | Unknown or missing value — never equal to anything, including itself |
| **IS NULL** | The correct way to test if a value is NULL |
| **IS NOT NULL** | The correct way to test if a value is not NULL |
| **operator precedence** | AND binds tighter than OR — use parentheses to be explicit |

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
