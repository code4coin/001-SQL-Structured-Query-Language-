# 🎯 Select List, Column Labels, DISTINCT & NULL Handling — PostgreSQL

> *Control exactly what your query returns — rename columns, eliminate duplicates, and handle missing data gracefully.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-SELECT%20%7C%20DISTINCT%20%7C%20NULL-8B5CF6?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-3B82F6?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-7.3.1%20|%207.3.2%20|%207.3.3-gray?style=for-the-badge)

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

- [Our Practice Tables](#-our-practice-tables)
- [7.3.1 Select-List Items](#-731-select-list-items)
- [7.3.2 Column Labels — AS](#-732-column-labels--as)
- [7.3.3 DISTINCT — Removing Duplicates](#-733-distinct--removing-duplicates)
- [NULL Handling](#-null-handling)
- [Quick Reference](#-quick-reference)

---

## 🏗️ Our Practice Tables

```sql
-- Sample table for DISTINCT examples:
DROP TABLE IF EXISTS sample;
CREATE TABLE sample (
    a int,
    b char(1),
    c int
);

INSERT INTO sample VALUES
    (1, 'A', 3),
    (2, 'B', 2),
    (3, 'C', 1),
    (1, 'A', 3),   -- ← duplicate of row 1
    (2, 'B', 2);   -- ← duplicate of row 2

SELECT a, b, c FROM sample;
```

```
 a │ b │ c
───┼───┼───
 1 │ A │ 3
 2 │ B │ 2
 3 │ C │ 1
 1 │ A │ 3   ← duplicate
 2 │ B │ 2   ← duplicate
```

```sql
-- Student table for NULL examples:
DROP SCHEMA IF EXISTS student CASCADE;
CREATE SCHEMA student;

CREATE TABLE student.stud_dtl (
    id        integer GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name      text    NOT NULL,
    age       integer CHECK (age >= 18),
    uni_id    integer,
    tuition_fee numeric(8,2),
    mentor_id integer   -- NULL means no mentor assigned
);

INSERT INTO student.stud_dtl (name, age, uni_id, tuition_fee, mentor_id) VALUES
    ('Nitin',   20, 101, 12500.00, 5),
    ('Robert',  35, 102,  8750.00, NULL),
    ('Alice',   22, 101, 12500.00, 3),
    ('Bob',     28, 103,  9900.00, NULL),
    ('Charlie', 19, 102,  8750.00, 5),
    ('Diana',   22, 101, 11000.00, NULL),
    ('Evan',    31, NULL,     NULL, 3);
```

---

## 📋 7.3.1 Select-List Items

### `*` — All Columns

```sql
SELECT * FROM sample;
-- Returns all columns in the order they were defined in the table
```

---

### Specific Columns

```sql
SELECT a, b, c FROM sample;
```

```
 a │ b │ c
───┼───┼───
 1 │ A │ 3
 2 │ B │ 2
 3 │ C │ 1
 1 │ A │ 3
 2 │ B │ 2
```

---

### Computed Columns — Expressions in the Select List

```sql
-- Add a computed column D = a * c:
SELECT a, b, c, a*c AS D FROM sample;
```

```
 a │ b │ c │  d
───┼───┼───┼────
 1 │ A │ 3 │  3   ← 1×3
 2 │ B │ 2 │  4   ← 2×2
 3 │ C │ 1 │  3   ← 3×1
 1 │ A │ 3 │  3   ← 1×3 (duplicate row)
 2 │ B │ 2 │  4   ← 2×2 (duplicate row)
```

> 💡 The expression `a*c` creates a **virtual column** — it doesn't exist on disk. It's calculated fresh for each row at query time.

---

### Multi-Table Selects — Qualifying Column Names

When multiple tables share a column name, qualify with the table name:

```sql
-- Without qualification — ambiguous if both tables have column 'a':
SELECT a FROM sample, other_table;   -- ERROR if both have 'a'

-- Qualified — unambiguous:
SELECT sample.a, other_table.a FROM sample, other_table;

-- Select all columns from one table, specific from another:
SELECT sample.*, other_table.a FROM sample, other_table;
```

<details>
<summary>📌 <strong>Click to expand: When does column name ambiguity happen?</strong></summary>

```sql
-- Example: joining student table to itself (self-join)
SELECT s1.name AS student, s2.name AS mentor
FROM student.stud_dtl s1
JOIN student.stud_dtl s2 ON s1.mentor_id = s2.id;

-- Both tables have a 'name' column — must qualify:
-- s1.name → the student
-- s2.name → their mentor

-- Without alias and qualification, this would be ambiguous:
SELECT name FROM student.stud_dtl s1 JOIN student.stud_dtl s2 ...
-- ERROR: column reference "name" is ambiguous
```

</details>

---

## 🏷️ 7.3.2 Column Labels — AS

The `AS` keyword gives a column or expression a **display name** in the result — without changing the underlying data.

### Basic Alias

```sql
-- Rename columns and give expressions meaningful names:
SELECT
    a        AS row_number,
    b        AS category,
    c        AS score,
    a * c    AS weighted_score
FROM sample;
```

```
 row_number │ category │ score │ weighted_score
────────────┼──────────┼───────┼────────────────
          1 │ A        │     3 │              3
          2 │ B        │     2 │              4
          3 │ C        │     1 │              3
```

---

### Applied to student.stud_dtl

```sql
SELECT
    id                              AS student_id,
    name                            AS student_name,
    age                             AS current_age,
    age * 12                        AS age_in_months,
    COALESCE(uni_id::text, 'N/A')   AS university
FROM student.stud_dtl;
```

```
 student_id │ student_name │ current_age │ age_in_months │ university
────────────┼──────────────┼─────────────┼───────────────┼────────────
          1 │ Nitin        │          20 │           240 │ 101
          2 │ Robert       │          35 │           420 │ 102
          7 │ Evan         │          31 │           372 │ N/A
```

---

### AS is Optional — But Recommended

```sql
-- These are identical:
SELECT a AS row_number FROM sample;
SELECT a row_number    FROM sample;   -- AS omitted — works but less readable

-- ✅ Always write AS for clarity and safety
```

---

### When AS is REQUIRED — Reserved Keywords as Aliases

```sql
-- ❌ 'from' is a reserved keyword — this fails:
SELECT a from FROM sample;
-- ERROR: syntax error at or near "from"

-- ✅ Fix with AS:
SELECT a AS from FROM sample;

-- ✅ Or double-quote the alias:
SELECT a "from" FROM sample;
```

<details>
<summary>📌 <strong>Click to expand: Alias rules — what works where</strong></summary>

```sql
-- ✅ Alias usable in ORDER BY:
SELECT a * c AS weighted_score FROM sample ORDER BY weighted_score DESC;

-- ✅ Alias usable in DISTINCT ON:
SELECT DISTINCT ON (a * c) a, b, c FROM sample;

-- ❌ Alias NOT usable in WHERE:
SELECT a * c AS score FROM sample WHERE score > 3;
-- ERROR: column "score" does not exist
-- Fix: repeat the expression in WHERE:
SELECT a * c AS score FROM sample WHERE a * c > 3;

-- ❌ Alias NOT usable in HAVING without GROUP BY:
-- Use a subquery or repeat the expression instead

-- 💡 Why? WHERE and HAVING are evaluated BEFORE the select list aliases are assigned.
--    ORDER BY is evaluated AFTER — so aliases work there.
```

</details>

---

## 🔁 7.3.3 DISTINCT — Removing Duplicates

`DISTINCT` eliminates duplicate rows from the result. Two rows are duplicates if **every column value is equal** (NULLs are treated as equal in this comparison).

### Without DISTINCT

```sql
SELECT a, b, c FROM sample;
```

```
 a │ b │ c
───┼───┼───
 1 │ A │ 3
 2 │ B │ 2
 3 │ C │ 1
 1 │ A │ 3   ← duplicate
 2 │ B │ 2   ← duplicate
```

---

### With DISTINCT

```sql
SELECT DISTINCT a, b, c FROM sample;
```

```
 a │ b │ c
───┼───┼───
 1 │ A │ 3   ← kept
 2 │ B │ 2   ← kept
 3 │ C │ 1   ← kept
             (duplicates removed)
```

> 💡 `SELECT ALL` is the explicit opposite of `SELECT DISTINCT` — it keeps all rows including duplicates. Since ALL is the default, it's rarely written.

---

### DISTINCT ON — Keep One Row Per Group

`DISTINCT ON (expression)` keeps only the **first row** for each unique value of the expression — giving you one representative per group:

```sql
-- Keep one row per unique value of a*c:
SELECT DISTINCT ON (a*c) a, b, c FROM sample;
```

```
 a │ b │ c
───┼───┼───
 3 │ C │ 1   ← a*c = 3 (first row with a*c=3 after sort)
 2 │ B │ 2   ← a*c = 4 (only one with a*c=4)
 1 │ A │ 3   ← a*c = 3? No — PostgreSQL keeps first per group
```

> ⚠️ Which row is "first" is **unpredictable** without an `ORDER BY`. Always combine `DISTINCT ON` with `ORDER BY` to get deterministic results:

```sql
-- ✅ Predictable: lowest 'a' value kept per a*c group:
SELECT DISTINCT ON (a*c) a, b, c
FROM sample
ORDER BY a*c, a ASC;
```

```
 a │ b │ c
───┼───┼───
 1 │ A │ 3   ← a*c=3, lowest a=1 kept (rows 1 and 3 both have a*c=3)
 2 │ B │ 2   ← a*c=4, only one row
 3 │ C │ 1   ← a*c=3? No — a*c=3 has rows (1,A,3) and (3,C,1). Lowest a=1 wins.
```

<details>
<summary>📌 <strong>Click to expand: DISTINCT ON applied to student.stud_dtl — real use case</strong></summary>

```sql
-- Get the youngest student per university (one student per uni_id):
SELECT DISTINCT ON (uni_id) uni_id, name, age
FROM student.stud_dtl
WHERE uni_id IS NOT NULL
ORDER BY uni_id, age ASC;   -- ORDER BY matches DISTINCT ON expr first
```

```
 uni_id │ name    │ age
────────┼─────────┼─────
   101  │ Nitin   │  20   ← youngest in uni 101
   102  │ Charlie │  19   ← youngest in uni 102
   103  │ Bob     │  28   ← only student in uni 103
```

> `DISTINCT ON` is PostgreSQL-specific. The SQL-standard alternative is a subquery with `ROW_NUMBER()` window function — but `DISTINCT ON` is often much cleaner for this pattern.

</details>

---

## 🚫 NULL Handling

NULL means **unknown** — not zero, not empty string. It requires special treatment throughout SQL.

### 1. Identifying NULLs — IS NULL / IS NOT NULL

```sql
-- ❌ These NEVER work for NULL checks:
SELECT * FROM student.stud_dtl WHERE mentor_id = NULL;    -- always 0 rows
SELECT * FROM student.stud_dtl WHERE mentor_id <> NULL;   -- always 0 rows
-- Because: NULL = anything → NULL (not TRUE), and WHERE needs TRUE

-- ✅ Correct approach:
-- Find students with NO mentor assigned:
SELECT name, mentor_id
FROM student.stud_dtl
WHERE mentor_id IS NULL;
```

```
 name   │ mentor_id
────────┼───────────
 Robert │   NULL
 Bob    │   NULL
 Diana  │   NULL
```

```sql
-- Find students WHO HAVE a mentor:
SELECT name, mentor_id
FROM student.stud_dtl
WHERE mentor_id IS NOT NULL;
```

```
 name    │ mentor_id
─────────┼───────────
 Nitin   │     5
 Alice   │     3
 Charlie │     5
 Evan    │     3
```

---

### 2. COALESCE — Replace NULL with a Default

`COALESCE(val1, val2, ...)` returns the **first non-NULL value** in the list. Essential for display and calculations where NULL would break things.

```sql
-- Replace NULL uni_id with 'Not Assigned':
SELECT
    name,
    COALESCE(uni_id::text, 'Not Assigned') AS university
FROM student.stud_dtl;
```

```
 name    │ university
─────────┼────────────
 Nitin   │ 101
 Robert  │ 102
 Alice   │ 101
 Bob     │ 103
 Charlie │ 102
 Diana   │ 101
 Evan    │ Not Assigned  ← NULL replaced with default text
```

```sql
-- Replace NULL tuition_fee with 0 for calculations:
SELECT
    name,
    COALESCE(tuition_fee, 0) AS fee_or_zero,
    COALESCE(tuition_fee, 0) * 0.1 AS ten_percent
FROM student.stud_dtl;
```

```
 name    │ fee_or_zero │ ten_percent
─────────┼─────────────┼─────────────
 Nitin   │   12500.00  │   1250.000
 Robert  │    8750.00  │    875.000
 Evan    │       0.00  │      0.000  ← NULL treated as 0
```

```sql
-- Multiple fallbacks — COALESCE returns the first non-NULL:
SELECT
    name,
    COALESCE(mentor_id::text, uni_id::text, 'No Assignment') AS primary_contact
FROM student.stud_dtl;
-- Returns mentor_id if not NULL, else uni_id if not NULL, else 'No Assignment'
```

---

### 3. NULLIF — Turn a Value INTO NULL

`NULLIF(expr1, expr2)` returns `NULL` if `expr1 = expr2`, otherwise returns `expr1`. The opposite of COALESCE.

```sql
-- Prevent division by zero — if tuition_fee = 0, treat as NULL instead:
SELECT
    name,
    tuition_fee,
    10000 / NULLIF(tuition_fee, 0) AS inverse_fee
FROM student.stud_dtl;
-- If tuition_fee = 0 → NULLIF returns NULL → division returns NULL (not error)
```

```sql
-- Convert empty strings to NULL:
SELECT
    name,
    NULLIF(name, '')    AS name_or_null
FROM student.stud_dtl;
-- If name = '' → returns NULL
-- If name = 'Nitin' → returns 'Nitin'
```

```sql
-- Practical: flag students who paid exactly 8750 as NULL (e.g. scholarship amount):
SELECT
    name,
    NULLIF(tuition_fee, 8750.00) AS adjusted_fee
FROM student.stud_dtl;
```

```
 name    │ adjusted_fee
─────────┼──────────────
 Nitin   │   12500.00
 Robert  │      NULL    ← 8750 converted to NULL
 Alice   │   12500.00
 Bob     │    9900.00
 Charlie │      NULL    ← 8750 converted to NULL
 Diana   │   11000.00
 Evan    │      NULL    ← was already NULL
```

---

### COALESCE vs NULLIF — The Pair

```
COALESCE → NULL becomes a value    "Fill in the blank"
NULLIF   → a value becomes NULL    "Erase this value"

COALESCE(mentor_id, 0)          0 when NULL        → useful for math
NULLIF(tuition_fee, 0)          NULL when zero     → safe division
COALESCE(NULLIF(name, ''), 'Unknown')  empty string → NULL → 'Unknown'
```

---

### NULL in SELECT List — Computed Columns

```sql
-- Any arithmetic with NULL produces NULL:
SELECT
    name,
    tuition_fee,
    tuition_fee * 0.1                              AS ten_percent_raw,
    COALESCE(tuition_fee, 0) * 0.1                 AS ten_percent_safe
FROM student.stud_dtl
WHERE uni_id IS NULL OR tuition_fee IS NULL;
```

```
 name │ tuition_fee │ ten_percent_raw │ ten_percent_safe
──────┼─────────────┼─────────────────┼──────────────────
 Evan │    NULL     │      NULL       │      0.000
```

> ⚠️ `NULL * anything = NULL`. Wrap with COALESCE before arithmetic if you need a numeric result.

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All patterns at a glance</strong></summary>

```sql
-- ── SELECT LIST ──────────────────────────────────────────────────
SELECT *                           FROM sample;    -- all columns
SELECT a, b, c                     FROM sample;    -- specific columns
SELECT a, b, a*c AS d              FROM sample;    -- computed column with alias
SELECT sample.a, other.b           FROM sample, other;  -- qualified names
SELECT sample.*                    FROM sample;    -- all columns from one table

-- ── COLUMN LABELS (AS) ───────────────────────────────────────────
SELECT a AS row_number             FROM sample;    -- rename column
SELECT a * c AS weighted_score     FROM sample;    -- name an expression
SELECT a AS "from"                 FROM sample;    -- reserved word as alias
SELECT a "from"                    FROM sample;    -- double-quote alternative
-- Alias works in: ORDER BY ✅   DISTINCT ON ✅
-- Alias fails in: WHERE ❌      HAVING ❌ (repeat the expression instead)

-- ── DISTINCT ─────────────────────────────────────────────────────
SELECT DISTINCT a, b, c            FROM sample;    -- remove duplicate rows
SELECT ALL a, b, c                 FROM sample;    -- keep all (default)
SELECT DISTINCT ON (a*c) a, b, c
FROM sample
ORDER BY a*c, a ASC;                               -- one row per a*c group

-- ── NULL CHECKS ──────────────────────────────────────────────────
WHERE mentor_id IS NULL                            -- ✅ find NULLs
WHERE mentor_id IS NOT NULL                        -- ✅ exclude NULLs
WHERE mentor_id = NULL                             -- ❌ always 0 rows

-- ── COALESCE ─────────────────────────────────────────────────────
COALESCE(uni_id::text, 'N/A')                     -- replace NULL with default
COALESCE(tuition_fee, 0) * 0.1                    -- safe arithmetic
COALESCE(val1, val2, val3)                         -- first non-NULL wins

-- ── NULLIF ───────────────────────────────────────────────────────
NULLIF(tuition_fee, 0)                             -- NULL if zero (safe division)
NULLIF(name, '')                                   -- NULL if empty string
100 / NULLIF(total, 0)                             -- division-by-zero safe

-- ── COMBINED ─────────────────────────────────────────────────────
COALESCE(NULLIF(name, ''), 'Unknown')              -- empty string → NULL → default
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **select list** | The comma-separated list of columns/expressions between SELECT and FROM |
| **`*`** | Wildcard — returns all columns the table expression provides |
| **virtual column** | A computed expression in the select list — calculated at query time, not stored |
| **AS** | Keyword that assigns an alias (display name) to a column or expression |
| **alias** | A temporary name for a column or expression used in query output |
| **DISTINCT** | Eliminates duplicate rows from the result set |
| **ALL** | Explicit keyword to keep all rows including duplicates (the default) |
| **DISTINCT ON** | PostgreSQL extension — keeps one row per unique value of the given expression |
| **NULL** | Unknown or missing value — not zero, not empty string |
| **IS NULL** | Correct way to test whether a value is NULL |
| **IS NOT NULL** | Correct way to test whether a value is not NULL |
| **COALESCE** | Returns the first non-NULL value from its argument list |
| **NULLIF** | Returns NULL if two expressions are equal; otherwise returns the first |
| **three-valued logic** | SQL's TRUE / FALSE / NULL system — NULL comparisons return NULL, not FALSE |

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
<sub>Built in public · Learned in public · Shared for free · One query at a time 🗄️</sub>

</div>
