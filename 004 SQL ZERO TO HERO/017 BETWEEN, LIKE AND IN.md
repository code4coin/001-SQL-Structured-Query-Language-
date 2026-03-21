# 🔎 BETWEEN, LIKE & IN — PostgreSQL

> *Three powerful WHERE clause predicates — range checks, pattern matching, and membership tests — with every edge case exposed.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-BETWEEN%20%7C%20LIKE%20%7C%20IN-8B5CF6?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner–Intermediate-F59E0B?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-9.7.1%20|%209.24-gray?style=for-the-badge)

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
- [BETWEEN — Range Checks](#-between--range-checks)
- [LIKE — Pattern Matching](#-like--pattern-matching)
- [IN & NOT IN — Membership Tests](#-in--not-in--membership-tests)
- [NULL Traps — IN, NOT IN, and BETWEEN](#-null-traps--in-not-in-and-between)
- [Side-by-Side Comparison](#-side-by-side-comparison)
- [Quick Reference](#-quick-reference)

---

## 🏗️ Our Practice Tables

### sales_data — for BETWEEN examples

```sql
DROP TABLE IF EXISTS sales_data;
CREATE TABLE sales_data AS
SELECT *
FROM UNNEST(
    ARRAY['North','North','North','South','South','South','East','East','West','West'],
    ARRAY['Laptop','Phone','Laptop','Phone','Laptop','Phone','Laptop','Phone','Laptop','Phone'],
    ARRAY['Electronics','Electronics','Electronics','Electronics','Electronics',
          'Electronics','Electronics','Electronics','Electronics','Electronics'],
    ARRAY['2024','2024','2024','2024','2024','2024','2024','2024','2024','2024'],
    ARRAY['Q1','Q1','Q2','Q2','Q1','Q2','Q1','Q2','Q1','Q2'],
    ARRAY[500, 300, 450, 200, 400, 350, 600, 250, 700, 150]
) AS t(region, product, category, year, quarter, sales);
```

### pattern_data — for LIKE examples

```sql
DROP TABLE IF EXISTS pattern_data;
CREATE TABLE pattern_data AS
SELECT *
FROM UNNEST(
    ARRAY['Alice','Alicia','Bob%','Bobby','Charlie','.foo.','foo','f.o',
          '50% off','hello_world','cart','cat','cut','coat','abcdef'],
    ARRAY[85, 90, 40, 55, 70, 100, 60, 75, 30, 95, 50, 80, 65, 45, NULL],
    ARRAY[9.99,19.99,4.99,49.99,29.99,99.99,14.99,24.99,5.99,79.99,
          34.99,59.99,12.99,7.99,89.99],
    ARRAY['A','A','B','B','C','A','B','C','A','B','C','A','B','C','A'],
    ARRAY['sale_item','normal','50%_discount','normal','sale_item',
          '.special.','normal','f_o_pattern','50%','hello_world',
          'normal','cat_item','normal','sale_item','normal']
) AS t(name, score, price, category, tag);

SELECT * FROM pattern_data;
```

```
 name        │ score │  price │ category │ tag
─────────────┼───────┼────────┼──────────┼──────────────
 Alice       │   85  │   9.99 │ A        │ sale_item
 Alicia      │   90  │  19.99 │ A        │ normal
 Bob%        │   40  │   4.99 │ B        │ 50%_discount
 Bobby       │   55  │  49.99 │ B        │ normal
 Charlie     │   70  │  29.99 │ C        │ sale_item
 .foo.       │  100  │  99.99 │ A        │ .special.
 foo         │   60  │  14.99 │ B        │ normal
 f.o         │   75  │  24.99 │ C        │ f_o_pattern
 50% off     │   30  │   5.99 │ A        │ 50%
 hello_world │   95  │  79.99 │ B        │ hello_world
 cart        │   50  │  34.99 │ C        │ normal
 cat         │   80  │  59.99 │ A        │ cat_item
 cut         │   65  │  12.99 │ B        │ normal
 coat        │   45  │   7.99 │ C        │ sale_item
 abcdef      │  NULL │  89.99 │ A        │ normal
```

---

## 📏 BETWEEN — Range Checks

`BETWEEN` tests whether a value falls within a range — **both endpoints are included**.

### Syntax

```sql
value BETWEEN low AND high
-- equivalent to:  value >= low AND value <= high
```

---

### Basic BETWEEN

```sql
-- Sales between 500 and 800 (inclusive):
SELECT * FROM sales_data
WHERE sales BETWEEN 500 AND 800;
```

```
 region │ product │ sales
────────┼─────────┼───────
 North  │ Laptop  │  500   ← 500 included (≥ 500)
 North  │ Laptop  │  450   ← wait... 450 < 500
```

Actually:

```
 region │ product │ sales
────────┼─────────┼───────
 North  │ Laptop  │   500
 South  │ Laptop  │   400  ← No — 400 < 500
 East   │ Laptop  │   600  ✅
 West   │ Laptop  │   700  ✅
```

```sql
-- Correct query — sales from 500 to 800:
SELECT region, product, sales
FROM sales_data
WHERE sales BETWEEN 500 AND 800
ORDER BY sales;
```

```
 region │ product │ sales
────────┼─────────┼───────
 North  │ Laptop  │   500  ← 500 = low endpoint, included ✅
 East   │ Laptop  │   600
 West   │ Laptop  │   700
```

---

### Wrong Order — BETWEEN Fails Silently

```sql
-- ❌ Wrong: low > high → always returns 0 rows:
SELECT * FROM sales_data
WHERE sales BETWEEN 800 AND 500;
-- Returns 0 rows — 800 AND 500 means nothing ≥ 800 AND ≤ 500 simultaneously
```

```
(0 rows)   ← no error, just empty result — easy bug to miss!
```

---

### BETWEEN SYMMETRIC — Order Doesn't Matter

```sql
-- ✅ BETWEEN SYMMETRIC auto-sorts the endpoints:
SELECT region, product, sales
FROM sales_data
WHERE sales BETWEEN SYMMETRIC 800 AND 500;
-- PostgreSQL swaps: treats as BETWEEN 500 AND 800
```

```
 region │ product │ sales
────────┼─────────┼───────
 North  │ Laptop  │   500
 East   │ Laptop  │   600
 West   │ Laptop  │   700
```

---

### All BETWEEN Variants

| Syntax | Meaning | Example | Result |
|--------|---------|---------|--------|
| `x BETWEEN a AND b` | `x >= a AND x <= b` | `2 BETWEEN 1 AND 3` | `true` |
| `x BETWEEN a AND b` | Wrong order fails | `2 BETWEEN 3 AND 1` | `false` |
| `x NOT BETWEEN a AND b` | `x < a OR x > b` | `2 NOT BETWEEN 1 AND 3` | `false` |
| `x BETWEEN SYMMETRIC a AND b` | Sorts endpoints first | `2 BETWEEN SYMMETRIC 3 AND 1` | `true` |
| `x NOT BETWEEN SYMMETRIC a AND b` | NOT after sorting | `2 NOT BETWEEN SYMMETRIC 3 AND 1` | `false` |

```sql
-- BETWEEN with decimals (price column):
SELECT name, price
FROM pattern_data
WHERE price BETWEEN 10.00 AND 30.00
ORDER BY price;
```

```
 name    │  price
─────────┼────────
 cut     │  12.99
 foo     │  14.99
 Alicia  │  19.99
 f.o     │  24.99
 Charlie │  29.99
```

```sql
-- NOT BETWEEN — everything outside the range:
SELECT name, score
FROM pattern_data
WHERE score NOT BETWEEN 50 AND 80
ORDER BY score;
```

```
 name        │ score
─────────────┼───────
 50% off     │   30
 Bob%        │   40
 coat        │   45
 Alicia      │   90
 hello_world │   95
 .foo.       │  100
              (NULLs excluded silently)
```

<details>
<summary>📌 <strong>Click to expand: BETWEEN is just shorthand for >= AND <=</strong></summary>

```sql
-- These two are exactly equivalent:
WHERE sales BETWEEN 500 AND 800
WHERE sales >= 500 AND sales <= 800

-- And these:
WHERE sales NOT BETWEEN 500 AND 800
WHERE sales < 500 OR sales > 800

-- BETWEEN SYMMETRIC with swapped args:
WHERE sales BETWEEN SYMMETRIC 800 AND 500
-- PostgreSQL rewrites to: WHERE sales >= 500 AND sales <= 800

-- Works on any comparable data type:
WHERE name BETWEEN 'A' AND 'M'        -- alphabetic range
WHERE enrolled_at BETWEEN '2024-01-01' AND '2024-12-31'  -- date range
WHERE price BETWEEN 9.99 AND 49.99    -- decimal range
```

</details>

---

## 🔍 LIKE — Pattern Matching

`LIKE` tests whether a string matches a pattern using two wildcard characters.

### Wildcards

| Wildcard | Matches |
|----------|---------|
| `%` | Any sequence of **zero or more** characters |
| `_` | Exactly **one** character |

### Pattern Examples

```
'abc' LIKE 'abc'    → true   (exact match, no wildcards)
'abc' LIKE 'a%'     → true   (starts with 'a', anything after)
'abc' LIKE '_b_'    → true   (any char, then 'b', then any char)
'abc' LIKE 'c'      → false  (no match)
'abc' LIKE '%'      → true   (% matches everything)
'abc' LIKE '___'    → true   (exactly 3 chars)
```

---

### Applied to pattern_data

```sql
-- Names starting with 'A' (case-sensitive):
SELECT name FROM pattern_data
WHERE name LIKE 'A%';
```

```
 name
────────
 Alice
 Alicia
```

```sql
-- Names starting with 'Al' — still case-sensitive:
SELECT name FROM pattern_data
WHERE name LIKE 'AL%';
```

```
(0 rows)   ← 'Alice' starts with 'Al', not 'AL'
```

---

### Case-Insensitive Matching

**Method 1 — ILIKE (PostgreSQL extension):**

```sql
-- ILIKE ignores case:
SELECT name FROM pattern_data
WHERE name ILIKE 'al%';
```

```
 name
────────
 Alice
 Alicia
```

**Method 2 — Custom Collation:**

```sql
-- Create a case-insensitive collation (one-time setup):
CREATE COLLATION IF NOT EXISTS case_insensitive (
    provider = icu,
    locale   = 'und-u-ks-level2',
    deterministic = false
);

-- Use with LIKE:
SELECT name FROM pattern_data
WHERE name LIKE 'AL%' COLLATE case_insensitive;
```

```
 name
────────
 Alice    ← matched despite 'AL' vs 'Al'
 Alicia
```

---

### Escape Character — Matching Literal `%` and `_`

`%` and `_` are special in LIKE. To match them literally, escape them with `\` (default escape):

```sql
-- Match the literal string 'Bob%' (the % is part of the name, not a wildcard):
SELECT name FROM pattern_data
WHERE name LIKE 'Bob\%';
```

```
 name
──────
 Bob%   ← the row whose name literally contains '%'
```

```sql
-- Bobby does NOT match 'Bob\%' because \ escapes the %:
-- 'Bobby' LIKE 'Bob\%' → false
-- 'Bob%'  LIKE 'Bob\%' → true ✅
```

---

### Custom Escape Character — ESCAPE clause

```sql
-- Reassign the escape character from \ to ?:
SELECT name FROM pattern_data
WHERE name LIKE 'Bob?%' ESCAPE '?';
-- ? now acts as the escape character
-- ?% means: literal percent sign
```

```
 name
──────
 Bob%
```

```sql
-- Disable escaping entirely (ESCAPE ''):
SELECT name FROM pattern_data
WHERE name LIKE 'Bob%' ESCAPE '';
-- Now % is ALWAYS a wildcard — cannot match literal %
-- Returns: Bob%, Bobby (both start with 'Bob')
```

---

### NOT LIKE — Exclude Pattern Matches

```sql
-- Names that do NOT start with 'A':
SELECT name FROM pattern_data
WHERE name NOT LIKE 'A%'
ORDER BY name;
```

```
 name
─────────────
 .foo.
 50% off
 Bobby
 Charlie
 ...
```

---

### LIKE Operator Aliases

| Syntax | Alias | Notes |
|--------|-------|-------|
| `LIKE` | `~~` | Standard LIKE |
| `NOT LIKE` | `!~~` | Standard NOT LIKE |
| `ILIKE` | `~~*` | Case-insensitive (PostgreSQL only) |
| `NOT ILIKE` | `!~~*` | Case-insensitive NOT (PostgreSQL only) |

```sql
-- These are identical:
WHERE name LIKE 'A%'
WHERE name ~~ 'A%'

WHERE name ILIKE 'al%'
WHERE name ~~* 'al%'
```

<details>
<summary>📌 <strong>Click to expand: LIKE covers the whole string — use % at both ends for substring search</strong></summary>

```sql
-- ❌ This does NOT find 'Alice' or 'Alicia' — LIKE matches the full string:
WHERE name LIKE 'lic'       -- false for 'Alice', 'Alicia'

-- ✅ Wrap with % to match anywhere in the string:
WHERE name LIKE '%lic%'     -- true for 'Alice', 'Alicia'

-- Common patterns:
WHERE name LIKE 'A%'         -- starts with A
WHERE name LIKE '%a'         -- ends with a
WHERE name LIKE '%lic%'      -- contains 'lic' anywhere
WHERE name LIKE '_at'        -- any single char followed by 'at' → cat, bat, rat
WHERE name LIKE 'c_t'        -- c, any char, t → cat, cut, cot
WHERE name LIKE 'c__t'       -- c, two chars, t → cart, coat
```

</details>

---

## ✅ IN & NOT IN — Membership Tests

`IN` checks whether a value matches **any value in a list or subquery**.

### IN with a Value List

```sql
-- Rows where category is A or C:
SELECT name, category
FROM pattern_data
WHERE category IN ('A', 'C')
ORDER BY category, name;
```

```
 name    │ category
─────────┼──────────
 .foo.   │ A
 50% off │ A
 Alice   │ A
 Alicia  │ A
 abcdef  │ A
 cat     │ A
 Charlie │ C
 cart    │ C
 coat    │ C
 f.o     │ C
```

---

### IN with a Subquery

```sql
-- Rows where 1 matches any value in a subquery:
SELECT * FROM pattern_data
WHERE 1 IN (SELECT a FROM UNNEST(ARRAY[1, 2, 3]::integer[]) AS T(a));
-- Returns ALL rows from pattern_data — 1 is found in [1,2,3] ✅
```

```sql
-- 1 NOT in [1,2,3]:
SELECT * FROM pattern_data
WHERE 1 NOT IN (SELECT a FROM UNNEST(ARRAY[1, 2, 3]::integer[]) AS T(a));
-- Returns 0 rows — 1 IS in the list, so NOT IN = false
```

---

### Empty Subquery — The Trap

```sql
-- Empty subquery — no rows returned by the subquery:
SELECT * FROM pattern_data
WHERE 1 IN (SELECT a FROM UNNEST(ARRAY[]::integer[]) AS T(a));
```

```
(0 rows)   ← IN with empty set = always false
```

```sql
-- NOT IN with empty subquery:
SELECT * FROM pattern_data
WHERE 1 NOT IN (SELECT a FROM UNNEST(ARRAY[]::integer[]) AS T(a));
```

```
(15 rows)  ← NOT IN with empty set = always true ✅ all rows returned
```

---

### No Match — Value Not in List

```sql
-- 1 not in [2, 3]:
SELECT * FROM pattern_data
WHERE 1 IN (SELECT a FROM UNNEST(ARRAY[2, 3]::integer[]) AS T(a));
```

```
(0 rows)   ← 1 not found in [2, 3]
```

```sql
-- NOT IN: 1 not in [2, 3] → true for every row:
SELECT * FROM pattern_data
WHERE 1 NOT IN (SELECT a FROM UNNEST(ARRAY[2, 3]::integer[]) AS T(a));
```

```
(15 rows)  ← all rows returned ✅
```

---

## ⚠️ NULL Traps — IN, NOT IN, and BETWEEN

### NULL in IN — Returns NULL, Not False

```sql
-- 1 IN (2, 3, NULL):
SELECT * FROM pattern_data
WHERE 1 IN (SELECT a FROM UNNEST(ARRAY[2, 3, NULL]::integer[]) AS T(a));
```

```
(0 rows)   ← 1 ≠ 2, 1 ≠ 3, 1 = NULL → NULL (unknown)
           ← WHERE only keeps TRUE rows → 0 rows returned
```

```sql
-- 1 NOT IN (2, 3, NULL) — the most dangerous NULL trap:
SELECT * FROM pattern_data
WHERE 1 NOT IN (SELECT a FROM UNNEST(ARRAY[2, 3, NULL]::integer[]) AS T(a));
```

```
(0 rows)   ← SURPRISE! Expected all rows, got none!
```

**Why?** The three-valued logic breakdown:

```
1 NOT IN (2, 3, NULL)
→ 1 <> 2  AND  1 <> 3  AND  1 <> NULL
→   TRUE   AND   TRUE   AND    NULL
→                              NULL    ← any NULL in AND chain = NULL
→ WHERE discards NULL (only keeps TRUE)
→ 0 rows returned
```

> ⚠️ **This is the #1 silent bug with NOT IN.** If the subquery or list can return any NULL, `NOT IN` returns NULL (not TRUE) for those comparisons — causing you to lose ALL rows from your result unexpectedly.

---

### The IN / NOT IN / NULL Truth Table

| Scenario | IN result | NOT IN result |
|----------|-----------|---------------|
| Value found in list | `TRUE` ← rows returned | `FALSE` ← rows excluded |
| Value not found, no NULLs | `FALSE` | `TRUE` ← rows returned |
| Value not found, list has NULL | `NULL` ← rows excluded | `NULL` ← rows excluded ⚠️ |
| Empty list/subquery | `FALSE` | `TRUE` |

---

### Fix for NOT IN with NULLs — Use NOT EXISTS

```sql
-- ⚠️ Broken if subquery can return NULLs:
WHERE 1 NOT IN (SELECT a FROM some_table)

-- ✅ Safe alternative — NOT EXISTS handles NULLs correctly:
WHERE NOT EXISTS (
    SELECT 1 FROM some_table WHERE a = 1
);

-- ✅ Or filter NULLs out of the subquery explicitly:
WHERE 1 NOT IN (
    SELECT a FROM some_table WHERE a IS NOT NULL
);
```

---

### BETWEEN and NULLs

```sql
-- score IS NULL for 'abcdef' row
-- BETWEEN returns NULL (not false) for NULL values — row is excluded:
SELECT name, score
FROM pattern_data
WHERE score BETWEEN 50 AND 100;
-- 'abcdef' (score=NULL) silently excluded
-- NULL BETWEEN 50 AND 100 → NULL → WHERE discards it
```

```
 name        │ score
─────────────┼───────
 Alice       │   85
 Alicia      │   90
 Charlie     │   70
 foo         │   60
 f.o         │   75
 hello_world │   95
 cart        │   50
 cat         │   80
 cut         │   65
 .foo.       │  100
```

---

## ↔️ Side-by-Side Comparison

| Predicate | Purpose | NULL Behaviour | Notes |
|-----------|---------|----------------|-------|
| `BETWEEN a AND b` | Range check (inclusive) | NULL input → NULL result (excluded) | Order of a,b matters |
| `BETWEEN SYMMETRIC` | Range check, any order | Same | Swaps endpoints automatically |
| `NOT BETWEEN` | Outside a range | Same | |
| `LIKE 'pattern'` | Pattern match | NULL input → NULL (excluded) | `%` = any chars, `_` = one char |
| `ILIKE 'pattern'` | Case-insensitive LIKE | Same | PostgreSQL only |
| `NOT LIKE` | Pattern exclusion | Same | |
| `IN (list)` | Exact match in list | NULL in list → NULL result | Fine for value lists |
| `IN (subquery)` | Exact match in subquery | NULL in subquery → NULL result | Safe — NULLs → unknown, not false |
| `NOT IN (subquery)` | Not in subquery | ⚠️ NULL in subquery → NULL → 0 rows | Use NOT EXISTS instead |

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All syntax patterns at a glance</strong></summary>

```sql
-- ── BETWEEN ──────────────────────────────────────────────────────
WHERE sales BETWEEN 500 AND 800           -- inclusive range
WHERE sales NOT BETWEEN 500 AND 800       -- outside range
WHERE sales BETWEEN SYMMETRIC 800 AND 500 -- any endpoint order
WHERE price BETWEEN 9.99 AND 49.99        -- works on decimals
WHERE name BETWEEN 'A' AND 'M'            -- works on text
WHERE enrolled_at BETWEEN '2024-01-01' AND '2024-12-31'  -- works on dates

-- ── LIKE ─────────────────────────────────────────────────────────
WHERE name LIKE 'A%'           -- starts with A (case-sensitive)
WHERE name LIKE '%on%'         -- contains 'on' anywhere
WHERE name LIKE '_at'          -- one char then 'at': cat, bat, hat
WHERE name LIKE 'c__t'         -- c, two chars, t: cart, coat
WHERE name ILIKE 'al%'         -- case-insensitive: Alice, ALICE, alice
WHERE name NOT LIKE 'A%'       -- does not start with A
WHERE name LIKE 'Bob\%'        -- literal % in pattern (default \ escape)
WHERE name LIKE 'Bob?%' ESCAPE '?'  -- custom escape character
WHERE name LIKE '%' ESCAPE ''       -- disable escaping

-- ── IN ───────────────────────────────────────────────────────────
WHERE category IN ('A', 'B')                          -- value list
WHERE category NOT IN ('C')                           -- exclusion list
WHERE 1 IN (SELECT a FROM unnest(ARRAY[1,2,3]) AS t(a))   -- subquery
WHERE 1 NOT IN (SELECT a FROM unnest(ARRAY[2,3]) AS t(a)) -- subquery

-- ── SAFE NOT IN (handles NULLs) ──────────────────────────────────
WHERE NOT EXISTS (SELECT 1 FROM t WHERE t.col = outer.col)
WHERE col NOT IN (SELECT col FROM t WHERE col IS NOT NULL)
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **BETWEEN** | Range predicate — inclusive of both endpoints |
| **BETWEEN SYMMETRIC** | Like BETWEEN but automatically sorts endpoints — safe with any order |
| **NOT BETWEEN** | Returns true when value is outside the range |
| **LIKE** | Pattern matching predicate using `%` and `_` wildcards |
| **ILIKE** | Case-insensitive LIKE — PostgreSQL extension |
| **`%` wildcard** | Matches any sequence of zero or more characters |
| **`_` wildcard** | Matches exactly one character |
| **Escape character** | A character preceding `%` or `_` to treat them as literals (default: `\`) |
| **ESCAPE clause** | Lets you choose a different escape character for LIKE |
| **IN** | Membership test — true if value equals any item in list/subquery |
| **NOT IN** | True only if value equals no item in list/subquery |
| **NULL trap** | NULL in NOT IN list causes result to be NULL (not true) — silently returns 0 rows |
| **NOT EXISTS** | Safe alternative to NOT IN when subquery may return NULLs |
| **Collation** | Rules for string comparison — can be case-insensitive or accent-insensitive |
| **Three-valued logic** | SQL uses TRUE / FALSE / NULL — NULL comparisons return NULL, not FALSE |

</details>

---

## ☕ Support This Journey

<div align="center">

```
Every query I write, every concept I break down,
every late-night debugging session —
it all goes into making this content free for you.
```

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
