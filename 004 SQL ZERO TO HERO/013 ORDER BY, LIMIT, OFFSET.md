# 🔃 Sorting & Pagination — ORDER BY, LIMIT & OFFSET — PostgreSQL

> *Control the order of your results and retrieve only the rows you need — essential for reports, dashboards, and APIs.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-ORDER%20BY%20%7C%20LIMIT%20%7C%20OFFSET-F97316?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-3B82F6?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-7.5%20|%207.6-gray?style=for-the-badge)

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

- [Why Sorting Matters](#-why-sorting-matters)
- [Our Practice Table](#-our-practice-table)
- [7.5 ORDER BY — Sorting Rows](#-75-order-by--sorting-rows)
- [7.6 LIMIT & OFFSET — Pagination](#-76-limit--offset--pagination)
- [ORDER BY + LIMIT Together](#-order-by--limit-together)
- [Common Patterns](#-common-patterns)
- [Quick Reference](#-quick-reference)

---

## 🤔 Why Sorting Matters

Without `ORDER BY`, PostgreSQL returns rows in **no guaranteed order**:

```
Without ORDER BY:              With ORDER BY:
──────────────────             ──────────────────────────────
Order depends on:              Order is exactly what you asked for
  • disk layout
  • scan type chosen           SELECT name, age
  • join strategy              FROM student.stud_dtl
  • query plan                 ORDER BY age ASC;
  • previous operations
                               Always returns youngest first ✅
Can change between
identical queries ⚠️
```

> ⚠️ **Never rely on unordered results.** If your application depends on a specific order, always use `ORDER BY`.

---

## 🏗️ Our Practice Table

```sql
DROP SCHEMA IF EXISTS student CASCADE;
CREATE SCHEMA student;

CREATE TABLE student.stud_dtl (
    id      integer  GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name    text     NOT NULL,
    age     integer  CHECK (age >= 18),
    uni_id  integer,
    tuition_fee numeric(8,2)
);

INSERT INTO student.stud_dtl (name, age, uni_id, tuition_fee) VALUES
    ('Nitin',   20, 101, 12500.00),
    ('Robert',  35, 102,  8750.00),
    ('Alice',   22, 101, 12500.00),
    ('Bob',     28, 103,  9900.00),
    ('Charlie', 19, 102,  8750.00),
    ('Diana',   22, 101, 11000.00),
    ('Evan',    31, NULL, NULL);

SELECT * FROM student.stud_dtl;
```

**Starting data:**

```
 id │ name    │ age │ uni_id │ tuition_fee
────┼─────────┼─────┼────────┼─────────────
  1 │ Nitin   │  20 │  101   │   12500.00
  2 │ Robert  │  35 │  102   │    8750.00
  3 │ Alice   │  22 │  101   │   12500.00
  4 │ Bob     │  28 │  103   │    9900.00
  5 │ Charlie │  19 │  102   │    8750.00
  6 │ Diana   │  22 │  101   │   11000.00
  7 │ Evan    │  31 │  NULL  │    NULL
```

---

## 🔃 7.5 ORDER BY — Sorting Rows

### Full Syntax

```sql
SELECT select_list
FROM table_expression
ORDER BY sort_expression1 [ASC | DESC] [NULLS { FIRST | LAST }]
       [, sort_expression2 [ASC | DESC] [NULLS { FIRST | LAST }] ...]
```

---

### ASC — Ascending (Default)

```sql
-- Sort by age, youngest first (ASC is the default — can be omitted):
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC;
```

```
 name    │ age
─────────┼─────
 Charlie │  19   ← smallest first
 Nitin   │  20
 Alice   │  22
 Diana   │  22
 Bob     │  28
 Evan    │  31
 Robert  │  35   ← largest last
```

---

### DESC — Descending

```sql
-- Sort by tuition fee, most expensive first:
SELECT name, tuition_fee
FROM student.stud_dtl
ORDER BY tuition_fee DESC;
```

```
 name    │ tuition_fee
─────────┼─────────────
 Nitin   │  12500.00   ← largest first
 Alice   │  12500.00
 Diana   │  11000.00
 Bob     │    9900.00
 Robert  │    8750.00
 Charlie │    8750.00
 Evan    │    NULL     ← NULLs sort last by default in ASC, first in DESC
```

---

### Multi-Column Sort — Tiebreaker

When more than one expression is given, the later ones act as **tiebreakers** for equal values in the earlier ones:

```sql
-- Sort by age ASC first, then name ASC to break ties:
SELECT name, age, uni_id
FROM student.stud_dtl
ORDER BY age ASC, name ASC;
```

```
 name    │ age │ uni_id
─────────┼─────┼────────
 Charlie │  19 │  102
 Nitin   │  20 │  101
 Alice   │  22 │  101   ← age=22 tie: Alice < Diana alphabetically
 Diana   │  22 │  101   ← age=22 tie: Diana comes after Alice
 Bob     │  28 │  103
 Evan    │  31 │  NULL
 Robert  │  35 │  102
```

---

### Mixed Sort Directions — Each Column is Independent

```sql
-- Sort by uni_id ASC, but within each university sort by age DESC:
SELECT name, age, uni_id
FROM student.stud_dtl
ORDER BY uni_id ASC, age DESC;
```

```
 name    │ age │ uni_id
─────────┼─────┼────────
 Alice   │  22 │  101   ← uni=101, age desc: Alice(22) > Nitin(20)? No — Diana(22)=Alice(22)
 Diana   │  22 │  101
 Nitin   │  20 │  101
 Robert  │  35 │  102   ← uni=102, age desc: Robert(35) before Charlie(19)
 Charlie │  19 │  102
 Bob     │  28 │  103
 Evan    │  31 │  NULL  ← NULL uni_id sorts last (NULLs > all non-nulls)
```

> ⚠️ `ORDER BY uni_id, age DESC` means `uni_id ASC, age DESC` — not both DESC! Each direction applies only to the column it follows.

---

### Sorting by an Expression

`ORDER BY` can use any expression, not just column names:

```sql
-- Sort by the length of the student's name:
SELECT name, age
FROM student.stud_dtl
ORDER BY LENGTH(name) ASC;
```

```
 name    │ age
─────────┼─────
 Bob     │  28   ← 3 chars
 Evan    │  31   ← 4 chars
 Alice   │  22   ← 5 chars
 Diana   │  22
 Nitin   │  20
 Robert  │  35   ← 6 chars
 Charlie │  19   ← 7 chars
```

```sql
-- Sort by a computed column alias:
SELECT name, age + 5 AS age_in_5_years
FROM student.stud_dtl
ORDER BY age_in_5_years ASC;    -- ✅ alias works in ORDER BY

-- Or by column position number:
SELECT name, age, uni_id
FROM student.stud_dtl
ORDER BY 2 ASC;                 -- ORDER BY the 2nd column (age) ✅
```

<details>
<summary>📌 <strong>Click to expand: ORDER BY alias rules — what works and what doesn't</strong></summary>

```sql
-- ✅ Alias name in ORDER BY — works:
SELECT name, age + 5 AS future_age
FROM student.stud_dtl
ORDER BY future_age;

-- ✅ Column position number — works:
SELECT name, age, uni_id
FROM student.stud_dtl
ORDER BY 2;   -- sorts by age (2nd column)

-- ❌ Alias used inside an expression — does NOT work:
SELECT name, age + 5 AS future_age
FROM student.stud_dtl
ORDER BY future_age + 1;  -- ERROR: future_age cannot be used in an expression here

-- Why? ORDER BY resolves aliases BEFORE expressions are applied.
-- If there's ambiguity between alias and table column names,
-- the OUTPUT COLUMN (alias) takes precedence.
```

</details>

---

### NULL Ordering — NULLS FIRST / NULLS LAST

By default: **NULLs sort as if larger than any non-NULL value**

```
ASC  → NULLS LAST   (NULLs appear at the bottom)
DESC → NULLS FIRST  (NULLs appear at the top)
```

```sql
-- Default ASC — NULLs at the bottom:
SELECT name, uni_id
FROM student.stud_dtl
ORDER BY uni_id ASC;
-- Evan's NULL uni_id appears last
```

```sql
-- Override: push NULLs to the top in ASC order:
SELECT name, uni_id
FROM student.stud_dtl
ORDER BY uni_id ASC NULLS FIRST;
```

```
 name    │ uni_id
─────────┼────────
 Evan    │  NULL  ← NULLS FIRST overrides default
 Nitin   │  101
 Alice   │  101
 Diana   │  101
 Robert  │  102
 Charlie │  102
 Bob     │  103
```

```sql
-- Override: push NULLs to the bottom in DESC order:
SELECT name, tuition_fee
FROM student.stud_dtl
ORDER BY tuition_fee DESC NULLS LAST;
```

```
 name    │ tuition_fee
─────────┼─────────────
 Nitin   │  12500.00
 Alice   │  12500.00
 Diana   │  11000.00
 Bob     │    9900.00
 Robert  │    8750.00
 Charlie │    8750.00
 Evan    │    NULL     ← NULLS LAST overrides default DESC behaviour
```

---

## 📄 7.6 LIMIT & OFFSET — Pagination

### Full Syntax

```sql
SELECT select_list
FROM table_expression
[ ORDER BY ... ]
[ LIMIT  { count | ALL } ]
[ OFFSET start ]
```

---

### LIMIT — Return Only N Rows

```sql
-- Return only the 3 youngest students:
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC
LIMIT 3;
```

```
 name    │ age
─────────┼─────
 Charlie │  19
 Nitin   │  20
 Alice   │  22   ← only 3 rows returned
```

---

### OFFSET — Skip N Rows First

```sql
-- Skip the first 2 rows, return the next ones:
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC
OFFSET 2;
```

```
 name    │ age
─────────┼─────
 Alice   │  22   ← first 2 rows (Charlie, Nitin) skipped
 Diana   │  22
 Bob     │  28
 Evan    │  31
 Robert  │  35
```

---

### LIMIT + OFFSET Together — Pagination

```sql
-- Page 1: rows 1-3
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC
LIMIT 3 OFFSET 0;

-- Page 2: rows 4-6
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC
LIMIT 3 OFFSET 3;

-- Page 3: rows 7+ (remaining)
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC
LIMIT 3 OFFSET 6;
```

```
Page 1 (OFFSET 0):      Page 2 (OFFSET 3):      Page 3 (OFFSET 6):
 name    │ age           name  │ age             (0 rows — end of data)
─────────┼─────         ───────┼─────
 Charlie │  19           Bob   │  28
 Nitin   │  20           Evan  │  31
 Alice   │  22           Robert│  35
```

**Formula:**
```
OFFSET = (page_number - 1) × page_size
LIMIT  = page_size

Page 1: LIMIT 3 OFFSET 0    (1-1)*3 = 0
Page 2: LIMIT 3 OFFSET 3    (2-1)*3 = 3
Page 3: LIMIT 3 OFFSET 6    (3-1)*3 = 6
```

---

### LIMIT ALL — No Limit

```sql
-- These are all equivalent — return all rows:
SELECT name FROM student.stud_dtl LIMIT ALL;
SELECT name FROM student.stud_dtl LIMIT NULL;
SELECT name FROM student.stud_dtl;               -- simplest
```

<details>
<summary>⚠️ <strong>Click to expand: OFFSET performance warning — large offsets are slow</strong></summary>

```
OFFSET does NOT skip computation — it skips DELIVERY.
PostgreSQL still computes ALL offset rows internally, then discards them.

LIMIT 10 OFFSET 10000:
  ✅ Returns only 10 rows to you
  ⚠️ Server computed 10,010 rows internally first

This becomes very slow at scale:
  OFFSET 1,000,000 on a large table → extremely slow

Better alternatives for large datasets:
  ✅ Keyset pagination (cursor-based):
     WHERE id > last_seen_id ORDER BY id LIMIT 10
     → Always fast regardless of position

  ✅ Use a WHERE clause to filter to the relevant page range
     rather than relying on large OFFSETs
```

</details>

<details>
<summary>📌 <strong>Click to expand: Why LIMIT without ORDER BY is dangerous</strong></summary>

```sql
-- ⚠️ Unpredictable — which 3 rows? Unknown!
SELECT name FROM student.stud_dtl LIMIT 3;

-- Different LIMIT values can produce different orderings
-- because PostgreSQL may choose different scan strategies:
SELECT name FROM student.stud_dtl LIMIT 2;   -- might return: Nitin, Robert
SELECT name FROM student.stud_dtl LIMIT 5;   -- might return: Charlie, Alice, Nitin, Robert, Bob
-- The order is NOT consistent!

-- ✅ Always pair LIMIT with ORDER BY:
SELECT name FROM student.stud_dtl ORDER BY id LIMIT 3;
-- Now the result is deterministic: always rows 1, 2, 3
```

> The query optimizer chooses different execution plans based on LIMIT count — a different plan can produce rows in a completely different order. **LIMIT without ORDER BY is undefined behaviour.**

</details>

---

## 🔗 ORDER BY + LIMIT Together

### Top-N Queries — Most Common Pattern

```sql
-- Top 3 students by highest tuition fee:
SELECT name, tuition_fee
FROM student.stud_dtl
ORDER BY tuition_fee DESC NULLS LAST
LIMIT 3;
```

```
 name  │ tuition_fee
───────┼─────────────
 Nitin │  12500.00
 Alice │  12500.00
 Diana │  11000.00
```

```sql
-- Youngest student in university 101:
SELECT name, age
FROM student.stud_dtl
WHERE uni_id = 101
ORDER BY age ASC
LIMIT 1;
```

```
 name  │ age
───────┼─────
 Nitin │  20
```

```sql
-- 2nd and 3rd oldest students (skipping the oldest):
SELECT name, age
FROM student.stud_dtl
ORDER BY age DESC
LIMIT 2 OFFSET 1;
```

```
 name │ age
──────┼─────
 Evan │  31   ← 2nd oldest (Robert at 35 was skipped by OFFSET 1)
 Bob  │  28   ← 3rd oldest
```

---

## 💡 Common Patterns

```sql
-- ── LEADERBOARD ─────────────────────────────────────────────────
-- Top 5 students by lowest fee (scholarship candidates):
SELECT name, tuition_fee
FROM student.stud_dtl
WHERE tuition_fee IS NOT NULL
ORDER BY tuition_fee ASC
LIMIT 5;

-- ── LATEST RECORD ───────────────────────────────────────────────
-- Most recently added student (highest id):
SELECT name, age
FROM student.stud_dtl
ORDER BY id DESC
LIMIT 1;

-- ── ALPHABETICAL LIST ───────────────────────────────────────────
-- All students A-Z:
SELECT name FROM student.stud_dtl ORDER BY name ASC;

-- ── PAGINATION (page 2, 3 rows per page) ────────────────────────
SELECT id, name, age
FROM student.stud_dtl
ORDER BY id ASC
LIMIT 3 OFFSET 3;

-- ── HANDLE NULLS EXPLICITLY ──────────────────────────────────────
-- Students sorted by university, unassigned last:
SELECT name, uni_id
FROM student.stud_dtl
ORDER BY uni_id ASC NULLS LAST, name ASC;

-- ── SORT BY EXPRESSION ───────────────────────────────────────────
-- Sort by how much above the minimum age each student is:
SELECT name, age, age - 18 AS years_above_minimum
FROM student.stud_dtl
ORDER BY years_above_minimum DESC;
```

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All ORDER BY / LIMIT / OFFSET syntax at a glance</strong></summary>

```sql
-- ── ORDER BY ─────────────────────────────────────────────────────
ORDER BY age                        -- ASC is the default
ORDER BY age ASC                    -- explicit ascending
ORDER BY age DESC                   -- descending
ORDER BY age ASC, name ASC          -- multi-column: age first, name as tiebreaker
ORDER BY uni_id ASC, age DESC       -- different directions per column
ORDER BY tuition_fee DESC NULLS LAST  -- NULLs at bottom even in DESC
ORDER BY uni_id ASC NULLS FIRST       -- NULLs at top even in ASC
ORDER BY LENGTH(name) ASC           -- sort by expression
ORDER BY age_alias ASC              -- sort by output column alias
ORDER BY 2 ASC                      -- sort by 2nd column in select list

-- ── LIMIT ────────────────────────────────────────────────────────
LIMIT 5                             -- return at most 5 rows
LIMIT 1                             -- return exactly the top 1 row
LIMIT ALL                           -- no limit (same as omitting LIMIT)
LIMIT NULL                          -- same as LIMIT ALL

-- ── OFFSET ───────────────────────────────────────────────────────
OFFSET 0                            -- no skip (same as omitting OFFSET)
OFFSET 10                           -- skip first 10 rows

-- ── COMBINED ────────────────────────────────────────────────────
SELECT name, age
FROM student.stud_dtl
ORDER BY age ASC        -- ← always include ORDER BY with LIMIT
LIMIT 3
OFFSET 6;               -- page 3 of 3-row pages: (3-1)*3 = 6
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **ORDER BY** | Clause that sorts the final result set by one or more expressions |
| **ASC** | Ascending order — smallest values first (default) |
| **DESC** | Descending order — largest values first |
| **sort expression** | Any valid expression in the select list — column name, alias, calculation, or function |
| **tiebreaker** | A secondary sort column used when the primary sort column has equal values |
| **NULLS FIRST** | Places NULL values at the top of the result, regardless of ASC/DESC |
| **NULLS LAST** | Places NULL values at the bottom of the result, regardless of ASC/DESC |
| **LIMIT** | Restricts the number of rows returned to at most N |
| **OFFSET** | Skips N rows before starting to return results |
| **pagination** | Breaking a large result set into pages using LIMIT + OFFSET |
| **keyset pagination** | A faster pagination alternative using `WHERE id > last_id` instead of OFFSET |
| **Top-N query** | A query using ORDER BY + LIMIT to get the N highest/lowest values |
| **column position** | Referring to a column by its number in the select list (e.g., `ORDER BY 2`) |

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
