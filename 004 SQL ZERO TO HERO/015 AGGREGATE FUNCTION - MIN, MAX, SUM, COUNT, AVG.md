# 📊 Aggregate Functions & UNNEST — PostgreSQL

> *Summarise your data with AVG, COUNT, MIN, MAX, SUM — and build test data on the fly without touching disk.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Aggregate%20Functions-F97316?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner–Intermediate-F59E0B?style=for-the-badge)

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

- [UNNEST — Sample Data Without Disk Space](#-unnest--sample-data-without-disk-space)
- [AVG — Average](#-avg--average)
- [COUNT — Counting Rows & Values](#-count--counting-rows--values)
- [MIN & MAX vs LEAST & GREATEST](#-min--max-vs-least--greatest)
- [SUM](#-sum)
- [ARRAY_AGG — Aggregating Into Arrays](#-array_agg--aggregating-into-arrays)
- [Side-by-Side Comparison](#-side-by-side-comparison)
- [Quick Reference](#-quick-reference)

---

## 🧪 UNNEST — Sample Data Without Disk Space

`UNNEST` expands arrays into rows — letting you create **in-memory sample tables instantly**, with no `CREATE TABLE`, no `INSERT`, and no disk usage.

### Basic Syntax

```sql
SELECT *
FROM UNNEST(array1, array2, ...) AS alias(col1, col2, ...);
```

---

### Single Array with NULL

```sql
-- Two columns, one with a NULL value:
SELECT *
FROM UNNEST(
    ARRAY[NULL, 1, 2, 3],
    ARRAY[9, 8, 7, 5]
) AS T(N, M);
```

```
  n   │ m
──────┼───
 NULL │ 9
    1 │ 8
    2 │ 7
    3 │ 5
```

---

### Multi-Column Sample Table

```sql
-- Recreate the sample table from Chapter 11 — in memory, no disk:
SELECT *
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', 'B'],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
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

> 💡 **Use UNNEST when you want to:**
> - Test a query without creating a permanent table
> - Demonstrate concepts in demos or documentation
> - Prototype aggregate logic before applying to real data

---

### With NULL in a Column

```sql
-- B column has a NULL in the last row — used in COUNT examples below:
SELECT *
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', NULL],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 a │  b   │ c
───┼──────┼───
 1 │ A    │ 3
 2 │ B    │ 2
 3 │ C    │ 1
 1 │ A    │ 3
 2 │ NULL │ 2   ← NULL in B
```

This sample table (5 rows, last B is NULL) is used throughout the rest of this chapter.

---

## 📐 AVG — Average

`AVG` calculates the arithmetic mean of a column. NULLs are **automatically excluded**.

```sql
-- Average of column A:
SELECT AVG(a)::NUMERIC(6, 2) AS avg_a
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', 'B'],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 avg_a
───────
  1.80   ← (1+2+3+1+2) / 5 = 9/5 = 1.8
```

```sql
-- Average of column C:
SELECT AVG(c)::NUMERIC(6, 2) AS avg_c
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', 'B'],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 avg_c
───────
  2.20   ← (3+2+1+3+2) / 5 = 11/5 = 2.2
```

> 💡 `::NUMERIC(6, 2)` casts the result to 2 decimal places. Without it, AVG returns full precision which can be many decimal digits.

---

### Applied to student.stud_dtl

```sql
SELECT
    AVG(age)::NUMERIC(5, 1)            AS avg_age,
    AVG(tuition_fee)::NUMERIC(10, 2)   AS avg_fee
FROM student.stud_dtl;
```

```
 avg_age │ avg_fee
─────────┼──────────
    25.3 │  10567.00
```

---

## 🔢 COUNT — Counting Rows & Values

> **The most important distinction in all of aggregate functions:**

```
COUNT(*)        → counts ALL rows (including NULLs)
COUNT(column)   → counts only NON-NULL values in that column
```

```sql
SELECT
    COUNT(*)  AS entity_count_in_table,
    COUNT(b)  AS attribute_count_in_column_b
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', NULL],   -- ← last B is NULL
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 entity_count_in_table │ attribute_count_in_column_b
───────────────────────┼─────────────────────────────
           5           │             4
```

```
COUNT(*) = 5   → all 5 rows counted, NULL in B doesn't matter
COUNT(b) = 4   → the NULL row is excluded from the count
```

---

### Visualising the Difference

```
Row │ a │   b   │ c    COUNT(*)?   COUNT(b)?
────┼───┼───────┼───   ─────────   ─────────
 1  │ 1 │  A    │ 3      ✅ +1       ✅ +1
 2  │ 2 │  B    │ 2      ✅ +1       ✅ +1
 3  │ 3 │  C    │ 1      ✅ +1       ✅ +1
 4  │ 1 │  A    │ 3      ✅ +1       ✅ +1
 5  │ 2 │ NULL  │ 2      ✅ +1       ❌ skip
                         ─────       ─────
  Total:                   5           4
```

---

### Applied to student.stud_dtl

```sql
SELECT
    COUNT(*)          AS total_students,
    COUNT(uni_id)     AS students_with_university,
    COUNT(mentor_id)  AS students_with_mentor
FROM student.stud_dtl;
```

```
 total_students │ students_with_university │ students_with_mentor
────────────────┼──────────────────────────┼──────────────────────
       7        │            6             │           4
```

```
total_students = 7        → all 7 rows counted
with_university = 6       → Evan has NULL uni_id, excluded
with_mentor = 4           → Robert, Bob, Diana have NULL mentor_id
```

---

## 📏 MIN & MAX vs LEAST & GREATEST

> **Critical distinction — these work at different levels:**

```
MIN / MAX     → AGGREGATE functions — work across rows in a column
LEAST / GREATEST → SCALAR functions — work across columns in a single row
```

```sql
SELECT
    MIN(a)                      AS min_col_a,
    MAX(b)                      AS max_col_b,
    LEAST(MIN(a),   MIN(c))     AS least_of_min_a_and_c,
    GREATEST(MAX(a), MAX(c))    AS greatest_of_max_a_and_c
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', NULL],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 min_col_a │ max_col_b │ least_of_min_a_and_c │ greatest_of_max_a_and_c
───────────┼───────────┼──────────────────────┼─────────────────────────
     1     │     C     │          1           │           3
```

**Step by step:**

```
MIN(a) = 1          ← smallest value across all rows in column A
MAX(b) = 'C'        ← largest value across all rows in column B (alphabetic)
MIN(c) = 1          ← smallest value across all rows in column C

LEAST(MIN(a), MIN(c))     = LEAST(1, 1)  = 1
GREATEST(MAX(a), MAX(c))  = GREATEST(3, 3) = 3
```

---

### The Conceptual Difference — Row vs Column

```
          a │ b │ c
          ──┼───┼──
          1 │ A │ 3
          2 │ B │ 2    ← MIN/MAX scan DOWN columns (across rows)
          3 │ C │ 1
          1 │ A │ 3
          2 │ B │ 2

MIN(a) = 1  MAX(b) = 'C'  ← aggregate: one result per column

LEAST(a, c)  for row 1 = LEAST(1, 3) = 1   ← scalar: one result per ROW
LEAST(a, c)  for row 2 = LEAST(2, 2) = 2
LEAST(a, c)  for row 3 = LEAST(3, 1) = 1
```

```sql
-- LEAST/GREATEST used per-row (no aggregation):
SELECT a, c, LEAST(a, c) AS smaller, GREATEST(a, c) AS larger
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', NULL],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 a │ c │ smaller │ larger
───┼───┼─────────┼────────
 1 │ 3 │    1    │   3
 2 │ 2 │    2    │   2
 3 │ 1 │    1    │   3
 1 │ 3 │    1    │   3
 2 │ 2 │    2    │   2
```

---

### Applied to student.stud_dtl

```sql
SELECT
    MIN(age)                              AS youngest,
    MAX(age)                              AS oldest,
    MIN(tuition_fee)                      AS lowest_fee,
    MAX(tuition_fee)                      AS highest_fee,
    GREATEST(MAX(age), MAX(tuition_fee))  AS greatest_of_both_maxes
FROM student.stud_dtl;
```

```
 youngest │ oldest │ lowest_fee │ highest_fee │ greatest_of_both_maxes
──────────┼────────┼────────────┼─────────────┼────────────────────────
    19    │   35   │   8750.00  │   12500.00  │        12500.00
```

---

## ➕ SUM

`SUM` adds up all non-NULL values in a column.

```sql
SELECT
    SUM(a)                          AS sum_col_a,
    SUM(c)                          AS sum_col_c,
    GREATEST(SUM(a), SUM(c))        AS greatest_of_sums,
    LEAST(SUM(a), SUM(c))           AS least_of_sums
FROM UNNEST(
    ARRAY[1, 2, 3, 1, 2],
    ARRAY['A', 'B', 'C', 'A', NULL],
    ARRAY[3, 2, 1, 3, 2]
) AS T(A, B, C);
```

```
 sum_col_a │ sum_col_c │ greatest_of_sums │ least_of_sums
───────────┼───────────┼──────────────────┼───────────────
     9     │    11     │       11         │       9
```

```
SUM(a) = 1+2+3+1+2 = 9
SUM(c) = 3+2+1+3+2 = 11
GREATEST(9, 11) = 11
LEAST(9, 11) = 9
```

---

### Applied to student.stud_dtl

```sql
SELECT
    SUM(age)                           AS total_age,
    SUM(tuition_fee)                   AS total_revenue,
    SUM(tuition_fee) / COUNT(*)        AS avg_fee_including_nulls,
    AVG(tuition_fee)::NUMERIC(10, 2)   AS avg_fee_excluding_nulls
FROM student.stud_dtl;
```

```
 total_age │ total_revenue │ avg_fee_including_nulls │ avg_fee_excluding_nulls
───────────┼───────────────┼─────────────────────────┼─────────────────────────
    177    │   63400.00    │         9057.14          │        10566.67
```

> ⚠️ Note the difference: `SUM / COUNT(*)` divides by ALL rows (7), but `AVG()` divides only by rows with non-NULL values (6) — they give different answers when NULLs are present.

---

## 📦 ARRAY_AGG — Aggregating Into Arrays

`ARRAY_AGG` collects values from multiple rows into a single PostgreSQL array — the reverse of UNNEST.

```sql
SELECT
    id,
    (array_agg(salary ORDER BY random()))[1] AS random_salary
FROM (
    VALUES
        (1, 'Alice',   100),
        (1, 'Bob',     200),
        (1, 'Charlie', 300),
        (2, 'Dave',    400),
        (2, 'Eve',     500)
) AS t(id, name, salary)
GROUP BY id;
```

```
 id │ random_salary
────┼───────────────
  1 │     200       ← one random salary from group 1 (varies each run)
  2 │     400       ← one random salary from group 2 (varies each run)
```

---

### ARRAY_AGG Variants

```sql
-- Collect all values:
array_agg(salary)

-- Collect sorted:
array_agg(salary ORDER BY salary)

-- Collect unique only:
array_agg(DISTINCT salary)

-- Get first element:
(array_agg(salary))[1]

-- Get last element:
(array_agg(salary ORDER BY salary DESC))[1]

-- Count elements (same as COUNT):
array_length(array_agg(salary), 1)
```

---

### ⚠️ The Parentheses Rule — When to Wrap

> One of the trickiest syntax rules in PostgreSQL array indexing:

| Case | Needs `()` wrapping? | Example |
|------|:---:|---------|
| Array literal `ARRAY[1,2,3][1]` | ❌ No | `ARRAY[1,2,3][1]` → `1` |
| Simple function `func()[1]` | ❌ Usually no | `string_to_array('a,b',',' )[1]` → `'a'` |
| Aggregate function `agg()[1]` | ✅ **Always** | `(array_agg(salary))[1]` |
| Aggregate with `ORDER BY`[1] | ✅ **Always** | `(array_agg(salary ORDER BY salary))[1]` |

```sql
-- ❌ Without parens — PostgreSQL can't parse this correctly:
SELECT array_agg(salary ORDER BY salary)[1] FROM t GROUP BY id;
-- ERROR or unexpected result

-- ✅ With parens — works:
SELECT (array_agg(salary ORDER BY salary))[1] FROM t GROUP BY id;
```

> **Rule of Thumb:** Whenever you index into an **aggregate function result**, always wrap the function call in `()` first.

---

### Applied to student.stud_dtl

```sql
-- Collect all student names per university into an array:
SELECT
    uni_id,
    array_agg(name ORDER BY name)       AS student_names,
    array_agg(age  ORDER BY age)        AS ages_sorted,
    array_length(array_agg(name), 1)    AS student_count
FROM student.stud_dtl
WHERE uni_id IS NOT NULL
GROUP BY uni_id
ORDER BY uni_id;
```

```
 uni_id │ student_names               │ ages_sorted  │ student_count
────────┼─────────────────────────────┼──────────────┼───────────────
  101   │ {Alice,Diana,Nitin}         │ {20,22,22}   │      3
  102   │ {Charlie,Robert}            │ {19,35}      │      2
  103   │ {Bob}                       │ {28}         │      1
```

---

## ↔️ Side-by-Side Comparison

| Function | Type | Scope | NULLs | Returns |
|----------|------|-------|-------|---------|
| `AVG(col)` | Aggregate | Across rows | Excluded | Single value |
| `COUNT(*)` | Aggregate | Across rows | Included | Row count |
| `COUNT(col)` | Aggregate | Across rows | Excluded | Non-NULL count |
| `MIN(col)` | Aggregate | Across rows | Excluded | Smallest value |
| `MAX(col)` | Aggregate | Across rows | Excluded | Largest value |
| `SUM(col)` | Aggregate | Across rows | Excluded | Total |
| `ARRAY_AGG(col)` | Aggregate | Across rows | Included by default | Array |
| `LEAST(a, b, ...)` | Scalar | Across columns, same row | Ignored | Smallest of inputs |
| `GREATEST(a, b, ...)` | Scalar | Across columns, same row | Ignored | Largest of inputs |
| `UNNEST(arr)` | Set-returning | Expands array to rows | Included | Multiple rows |

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All aggregate syntax at a glance</strong></summary>

```sql
-- ── UNNEST (in-memory table) ──────────────────────────────────────
SELECT *
FROM UNNEST(ARRAY[1,2,3], ARRAY['A','B','C']) AS T(num, letter);

-- ── AVG ──────────────────────────────────────────────────────────
SELECT AVG(age)::NUMERIC(5,1) FROM student.stud_dtl;

-- ── COUNT ────────────────────────────────────────────────────────
SELECT COUNT(*)    FROM student.stud_dtl;   -- all rows (incl. NULLs)
SELECT COUNT(col)  FROM student.stud_dtl;   -- non-NULL values only
SELECT COUNT(DISTINCT uni_id) FROM student.stud_dtl;  -- unique non-NULLs

-- ── MIN / MAX ────────────────────────────────────────────────────
SELECT MIN(age), MAX(age) FROM student.stud_dtl;

-- ── LEAST / GREATEST (per row, not aggregate) ────────────────────
SELECT LEAST(a, c), GREATEST(a, b) FROM T;    -- compare within a row
SELECT LEAST(MIN(a), MIN(c)) FROM T;          -- compare aggregate results

-- ── SUM ──────────────────────────────────────────────────────────
SELECT SUM(tuition_fee) FROM student.stud_dtl;

-- ── ARRAY_AGG ────────────────────────────────────────────────────
SELECT array_agg(name)                      -- collect all values
SELECT array_agg(name ORDER BY name)        -- collect sorted
SELECT array_agg(DISTINCT uni_id)           -- collect unique only
SELECT (array_agg(salary))[1]               -- first element ← needs ()
SELECT (array_agg(salary ORDER BY salary DESC))[1]  -- last element
SELECT array_length(array_agg(name), 1)     -- count (= COUNT)
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Aggregate function** | Operates across multiple rows and returns a single summary value |
| **Scalar function** | Operates on values within a single row, returns one value per row |
| **UNNEST** | Expands a PostgreSQL array into a set of rows |
| **AVG** | Returns the arithmetic mean of non-NULL values |
| **COUNT(\*)** | Counts all rows in the group, including those with NULL values |
| **COUNT(col)** | Counts non-NULL values in the specified column |
| **MIN** | Returns the smallest non-NULL value across rows in a column |
| **MAX** | Returns the largest non-NULL value across rows in a column |
| **LEAST** | Returns the smallest value among its arguments (per-row, scalar) |
| **GREATEST** | Returns the largest value among its arguments (per-row, scalar) |
| **SUM** | Returns the total of non-NULL values in a column |
| **ARRAY_AGG** | Collects values from multiple rows into a single array |
| **GROUP BY** | Divides rows into groups; aggregates are computed per group |
| **array_length(arr, 1)** | Returns the number of elements in a 1-dimensional array |
| **`(agg())[n]`** | Indexing into an aggregate result — parentheses always required |
| **in-memory table** | A virtual table created with UNNEST or VALUES — no disk storage |

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
