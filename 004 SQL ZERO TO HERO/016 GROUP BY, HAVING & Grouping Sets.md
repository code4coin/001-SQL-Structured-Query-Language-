# 🗂️ WHERE, GROUP BY, HAVING & Grouping Sets — PostgreSQL

> *Filter rows, group data, compute aggregates, and generate multi-level summaries — all in one query.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-GROUP%20BY%20%7C%20HAVING%20%7C%20ROLLUP%20%7C%20CUBE-10B981?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Intermediate-F59E0B?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-7.2.2%20|%207.2.3%20|%207.2.4-gray?style=for-the-badge)

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

- [The Query Pipeline — Where Each Clause Fits](#-the-query-pipeline--where-each-clause-fits)
- [Our Practice Table — sales_data](#-our-practice-table--sales_data)
- [7.2.2 WHERE Clause](#-722-where-clause)
- [7.2.3 GROUP BY & HAVING](#-723-group-by--having)
- [7.2.4 GROUPING SETS](#-724-grouping-sets)
- [7.2.4 ROLLUP](#-724-rollup)
- [7.2.4 CUBE](#-724-cube)
- [Side-by-Side: GROUPING SETS vs ROLLUP vs CUBE](#-side-by-side-grouping-sets-vs-rollup-vs-cube)
- [Quick Reference](#-quick-reference)

---

## ⚙️ The Query Pipeline — Where Each Clause Fits

```
FROM clause
  └── produces the raw virtual table
       └── WHERE clause          ← filters individual ROWS (before grouping)
             └── GROUP BY clause ← groups rows by shared column values
                  └── HAVING clause ← filters GROUPS (after aggregation)
                       └── SELECT list ← picks columns + computes aggregates
                            └── ORDER BY ← sorts the final result
```

> 💡 **WHERE vs HAVING in one sentence:**
> `WHERE` filters rows **before** grouping. `HAVING` filters groups **after** aggregation.

---

## 🏗️ Our Practice Table — sales_data

```sql
-- Build a real table from UNNEST — no manual INSERTs needed:
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

SELECT * FROM sales_data;
```

```
 region │ product │ category    │ year │ quarter │ sales
────────┼─────────┼─────────────┼──────┼─────────┼───────
 North  │ Laptop  │ Electronics │ 2024 │ Q1      │   500
 North  │ Phone   │ Electronics │ 2024 │ Q1      │   300
 North  │ Laptop  │ Electronics │ 2024 │ Q2      │   450
 South  │ Phone   │ Electronics │ 2024 │ Q2      │   200
 South  │ Laptop  │ Electronics │ 2024 │ Q1      │   400
 South  │ Phone   │ Electronics │ 2024 │ Q2      │   350
 East   │ Laptop  │ Electronics │ 2024 │ Q1      │   600
 East   │ Phone   │ Electronics │ 2024 │ Q2      │   250
 West   │ Laptop  │ Electronics │ 2024 │ Q1      │   700
 West   │ Phone   │ Electronics │ 2024 │ Q2      │   150
(10 rows)
```

---

## 🔍 7.2.2 WHERE Clause

`WHERE` filters rows **before** any grouping or aggregation occurs. Every row that doesn't satisfy the condition is discarded.

### Basic Filtering

```sql
-- Only Q1 sales:
SELECT region, product, sales
FROM sales_data
WHERE quarter = 'Q1';
```

```
 region │ product │ sales
────────┼─────────┼───────
 North  │ Laptop  │   500
 North  │ Phone   │   300
 South  │ Laptop  │   400
 East   │ Laptop  │   600
 West   │ Laptop  │   700
```

---

### WHERE with Common Operators

```sql
-- Greater than:
SELECT region, sales FROM sales_data WHERE sales > 400;

-- IN list — match any of several values:
SELECT region, product FROM sales_data WHERE region IN ('North', 'East');

-- BETWEEN — inclusive range:
SELECT * FROM sales_data WHERE sales BETWEEN 300 AND 500;

-- Subquery in WHERE:
SELECT region, sales
FROM sales_data
WHERE sales IN (SELECT MAX(sales) FROM sales_data GROUP BY region);
-- Returns the top-selling row from each region

-- EXISTS — check if subquery returns any rows:
SELECT DISTINCT region
FROM sales_data s
WHERE EXISTS (
    SELECT 1 FROM sales_data s2
    WHERE s2.region = s.region AND s2.sales > 600
);
-- Regions that have at least one sale above 600
```

<details>
<summary>📌 <strong>Click to expand: WHERE for join conditions — ON vs WHERE</strong></summary>

```sql
-- These two are equivalent for INNER JOINs:

-- Style 1: Join condition in WHERE:
FROM sales_data s, regions r
WHERE s.region = r.region_name AND r.active = TRUE

-- Style 2: Join condition in ON (preferred — clearer intent):
FROM sales_data s
INNER JOIN regions r ON s.region = r.region_name
WHERE r.active = TRUE

-- ⚠️ For OUTER JOINS, they are NOT equivalent:
-- Conditions in ON include unmatched rows (with NULLs)
-- Conditions in WHERE eliminate those unmatched rows
-- → Always use ON for outer join conditions
```

</details>

---

## 📊 7.2.3 GROUP BY & HAVING

### GROUP BY — Collapsing Rows into Groups

`GROUP BY` combines all rows sharing the same values in the listed columns into a single summary row. After grouping, you can only reference grouped columns or aggregate expressions in the SELECT list.

```sql
-- One row per unique region+product combination:
SELECT
    region,
    product,
    SUM(sales) AS total_sales
FROM sales_data
GROUP BY region, product
ORDER BY region, product;
```

```
 region │ product │ total_sales
────────┼─────────┼─────────────
 East   │ Laptop  │     600
 East   │ Phone   │     250
 North  │ Laptop  │     950     ← 500 + 450
 North  │ Phone   │     300
 South  │ Laptop  │     400
 South  │ Phone   │     550     ← 200 + 350
 West   │ Laptop  │     700
 West   │ Phone   │     150
```

---

### GROUP BY with CASE — Labelling Groups

```sql
SELECT
    CASE
        WHEN SUM(sales) <= 400 THEN 'Low Performer'
        WHEN SUM(sales) >  400 THEN 'High Performer'
    END                              AS performance,
    region,
    product,
    SUM(sales)                       AS total_sales
FROM sales_data
GROUP BY region, product
ORDER BY region DESC, product DESC;
```

```
 performance    │ region │ product │ total_sales
────────────────┼────────┼─────────┼─────────────
 Low Performer  │ West   │ Phone   │     150
 High Performer │ West   │ Laptop  │     700
 High Performer │ South  │ Phone   │     550
 Low Performer  │ South  │ Laptop  │     400
 High Performer │ North  │ Laptop  │     950
 Low Performer  │ North  │ Phone   │     300
 Low Performer  │ East   │ Phone   │     250
 High Performer │ East   │ Laptop  │     600
```

---

### GROUP BY Rules

```
✅ In SELECT after GROUP BY, you CAN reference:
   • Columns listed in GROUP BY
   • Aggregate expressions: SUM(), AVG(), COUNT(), MIN(), MAX()

❌ In SELECT after GROUP BY, you CANNOT reference:
   • Columns NOT in GROUP BY (unless inside an aggregate)

-- ❌ This fails:
SELECT region, product, sales   -- 'sales' not grouped or aggregated
FROM sales_data GROUP BY region;

-- ✅ This works:
SELECT region, SUM(sales)       -- SUM is an aggregate
FROM sales_data GROUP BY region;
```

> 💡 `GROUP BY column` without any aggregate effectively gives you `SELECT DISTINCT column` — same result, different syntax.

---

### HAVING — Filtering Groups After Aggregation

`HAVING` is to groups what `WHERE` is to rows — it filters **after** aggregation:

```sql
-- Only year+quarter combos where average sales > some threshold:
SELECT
    year,
    quarter,
    AVG(sales)::NUMERIC(6, 2) AS avg_sales
FROM sales_data
WHERE quarter = 'Q1'           -- ← WHERE filters rows BEFORE grouping
GROUP BY year, quarter
HAVING quarter = 'Q1';         -- ← HAVING filters groups AFTER aggregation
```

```
 year │ quarter │ avg_sales
──────┼─────────┼───────────
 2024 │ Q1      │    500.00
```

```sql
-- Regions where total sales exceed 900:
SELECT
    region,
    SUM(sales) AS total_sales
FROM sales_data
GROUP BY region
HAVING SUM(sales) > 900;
```

```
 region │ total_sales
────────┼─────────────
 North  │    1250
 West   │     850     ← West: 700+150=850 ... not >900
 East   │     850     ← East: 600+250=850 ... not >900
```

```
Actually:
North = 500+300+450 = 1250 ✅ > 900
South = 200+400+350 = 950  ✅ > 900
East  = 600+250     = 850  ❌
West  = 700+150     = 850  ❌
```

```sql
-- Correct result:
SELECT region, SUM(sales) AS total_sales
FROM sales_data
GROUP BY region
HAVING SUM(sales) > 900
ORDER BY total_sales DESC;
```

```
 region │ total_sales
────────┼─────────────
 North  │    1250
 South  │     950
```

<details>
<summary>📌 <strong>Click to expand: WHERE vs HAVING — when to use which</strong></summary>

```
WHERE (before grouping):                HAVING (after grouping):
──────────────────────────              ─────────────────────────────
Filters individual rows                 Filters groups
Runs BEFORE GROUP BY                    Runs AFTER GROUP BY + aggregation
Can reference any column                Can reference grouped cols + aggregates
Cannot use aggregate functions          Must use aggregates or grouped cols
Faster — eliminates rows early          Slower — computes all groups first

Rule of Thumb:
  If the condition is on a RAW column value → WHERE
  If the condition is on an AGGREGATE result → HAVING

-- Raw column filter → WHERE:
WHERE quarter = 'Q1'

-- Aggregate filter → HAVING:
HAVING SUM(sales) > 900
HAVING AVG(sales) > 400
HAVING COUNT(*) >= 2
```

</details>

---

## 🧩 7.2.4 GROUPING SETS

`GROUPING SETS` lets you compute multiple groupings in a **single query** — the result is a UNION of separate GROUP BY queries stacked together.

```sql
-- Average sales grouped by region separately, and by year separately:
SELECT
    region,
    year,
    AVG(sales)::NUMERIC(6, 2) AS avg_sales
FROM sales_data
GROUP BY GROUPING SETS ( (region), (year) );
```

```
 region │ year │ avg_sales
────────┼──────┼───────────
 East   │ NULL │    425.00   ← grouped by region: East average
 North  │ NULL │    416.67   ← grouped by region: North average
 South  │ NULL │    316.67   ← grouped by region: South average
 West   │ NULL │    425.00   ← grouped by region: West average
  NULL  │ 2024 │    390.00   ← grouped by year: 2024 average
```

> 💡 NULL in a grouping column means **"this row was not grouped by that column"** — it's a placeholder, not a missing value.

---

### Equivalent Using UNION ALL

```sql
-- GROUPING SETS ((region), (year)) is exactly the same as:
SELECT region, NULL::text AS year, AVG(sales)::NUMERIC(6,2) AS avg_sales
FROM sales_data GROUP BY region
UNION ALL
SELECT NULL, year, AVG(sales)::NUMERIC(6,2)
FROM sales_data GROUP BY year;
-- GROUPING SETS is just cleaner shorthand for this
```

---

### With an Empty Set — Grand Total

```sql
-- Three grouping levels: by region, by year, and a grand total:
SELECT
    region,
    year,
    SUM(sales) AS total_sales
FROM sales_data
GROUP BY GROUPING SETS ( (region), (year), () );
--                                          ↑ empty set = grand total row
```

```
 region │ year │ total_sales
────────┼──────┼─────────────
 East   │ NULL │     850     ← region subtotal
 North  │ NULL │    1250     ← region subtotal
 South  │ NULL │     950     ← region subtotal
 West   │ NULL │     850     ← region subtotal
  NULL  │ 2024 │    3900     ← year subtotal
  NULL  │ NULL │    3900     ← grand total (empty grouping set)
```

---

## 📈 7.2.4 ROLLUP

`ROLLUP` generates a **hierarchy of subtotals** — from most detailed to grand total, drilling from right to left.

```
ROLLUP(e1, e2, e3) expands to GROUPING SETS:
  (e1, e2, e3)   ← most detailed
  (e1, e2)       ← drop e3
  (e1)           ← drop e2
  ()             ← grand total
```

### ROLLUP on region, product, category

```sql
SELECT
    region,
    product,
    category,
    SUM(sales) AS total_sales
FROM sales_data
GROUP BY ROLLUP(region, product, category)
ORDER BY region NULLS LAST, product NULLS LAST, category NULLS LAST;
```

```
 region │ product │ category    │ total_sales
────────┼─────────┼─────────────┼─────────────
 East   │ Laptop  │ Electronics │     600     ← detail: region+product+category
 East   │ Laptop  │ NULL        │     600     ← subtotal: region+product
 East   │ Phone   │ Electronics │     250
 East   │ Phone   │ NULL        │     250
 East   │ NULL    │ NULL        │     850     ← subtotal: region only
 North  │ Laptop  │ Electronics │     950
 North  │ Laptop  │ NULL        │     950
 North  │ Phone   │ Electronics │     300
 North  │ Phone   │ NULL        │     300
 North  │ NULL    │ NULL        │    1250
 South  │ ...     │ ...         │     ...
 West   │ ...     │ ...         │     ...
 NULL   │ NULL    │ NULL        │    3900     ← grand total
```

---

### ROLLUP on year, quarter

```sql
SELECT
    year,
    quarter,
    SUM(sales) AS total_sales
FROM sales_data
GROUP BY ROLLUP(year, quarter)
ORDER BY year NULLS LAST, quarter NULLS LAST;
```

```
 year │ quarter │ total_sales
──────┼─────────┼─────────────
 2024 │ Q1      │    2500     ← detail: year + quarter
 2024 │ Q2      │    1400     ← detail: year + quarter
 2024 │ NULL    │    3900     ← subtotal: year only
 NULL │ NULL    │    3900     ← grand total
```

> 💡 **ROLLUP is ideal for hierarchical reports** — e.g., sales by region → sub-region → store → grand total. Each level is a prefix of the previous.

---

## 🎲 7.2.4 CUBE

`CUBE` generates **every possible combination** of the listed columns — the power set. Use it when you need subtotals from every angle, not just hierarchically.

```
CUBE(a, b, c) expands to GROUPING SETS:
  (a, b, c)  ← all three
  (a, b)     ← drop c
  (a, c)     ← drop b
  (a)        ← only a
  (b, c)     ← drop a
  (b)        ← only b
  (c)        ← only c
  ()         ← grand total
  = 2³ = 8 grouping sets
```

```sql
SELECT
    region,
    product,
    category,
    SUM(sales) AS total_sales
FROM sales_data
GROUP BY CUBE(region, product, category)
ORDER BY region NULLS LAST, product NULLS LAST, category NULLS LAST;
```

<details>
<summary>📌 <strong>Click to expand: CUBE result — all combinations shown</strong></summary>

```
 region │ product │ category    │ total_sales
────────┼─────────┼─────────────┼─────────────
 East   │ Laptop  │ Electronics │     600   ← (region, product, category)
 East   │ Laptop  │ NULL        │     600   ← (region, product)
 East   │ NULL    │ Electronics │     850   ← (region, category)
 East   │ NULL    │ NULL        │     850   ← (region)
 East   │ Phone   │ Electronics │     250
 East   │ Phone   │ NULL        │     250
 North  │ ...     │ ...         │     ...
 ...
 NULL   │ Laptop  │ Electronics │    2250   ← (product, category) — all Laptops
 NULL   │ Laptop  │ NULL        │    2250   ← (product) — all Laptops
 NULL   │ Phone   │ Electronics │    1650   ← (product, category) — all Phones
 NULL   │ Phone   │ NULL        │    1650   ← (product)
 NULL   │ NULL    │ Electronics │    3900   ← (category)
 NULL   │ NULL    │ NULL        │    3900   ← grand total ()
```

> CUBE on 3 columns produces 2³ = **8 grouping sets** and many result rows — great for pivot-table style analysis but can be large on many columns.

</details>

---

## ↔️ Side-by-Side: GROUPING SETS vs ROLLUP vs CUBE

| Feature | GROUPING SETS | ROLLUP | CUBE |
|---------|--------------|--------|------|
| **What it generates** | Exactly what you specify | All prefixes from right to left | All possible subsets (power set) |
| **Number of groupings** | You decide | n+1 (for n columns) | 2ⁿ (for n columns) |
| **Best for** | Custom subtotal combinations | Hierarchical drill-down reports | Cross-dimensional analysis |
| **Grand total included?** | Only if you add `()` | ✅ Always (empty set at end) | ✅ Always (empty set) |
| **Equivalent to** | Manual UNION ALL | GROUPING SETS with prefix pattern | GROUPING SETS with all subsets |

### Equivalence at a Glance

```sql
-- ROLLUP(a, b, c) = GROUPING SETS:
GROUPING SETS ( (a,b,c), (a,b), (a), () )

-- CUBE(a, b) = GROUPING SETS:
GROUPING SETS ( (a,b), (a), (b), () )

-- CUBE(a, b, c) = GROUPING SETS:
GROUPING SETS ( (a,b,c), (a,b), (a,c), (a), (b,c), (b), (c), () )
```

---

### Removing Duplicates from Combined Groupings

```sql
-- Two ROLLUPs together can create duplicate grouping sets:
GROUP BY ROLLUP(region, product), ROLLUP(region, quarter)
-- → Creates duplicates like (region) appearing multiple times

-- Fix with GROUP BY DISTINCT:
GROUP BY DISTINCT ROLLUP(region, product), ROLLUP(region, quarter)
-- → Deduplicates the grouping sets before computing
```

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All GROUP BY syntax at a glance</strong></summary>

```sql
-- ── WHERE ────────────────────────────────────────────────────────
WHERE sales > 400
WHERE region IN ('North', 'South')
WHERE sales BETWEEN 300 AND 600
WHERE quarter = 'Q1' AND region = 'North'

-- ── GROUP BY ─────────────────────────────────────────────────────
GROUP BY region
GROUP BY region, product
GROUP BY region, product, category

-- ── GROUP BY with aggregates ──────────────────────────────────────
SELECT region, SUM(sales), AVG(sales)::NUMERIC(6,2), COUNT(*), MIN(sales), MAX(sales)
FROM sales_data GROUP BY region;

-- ── HAVING ───────────────────────────────────────────────────────
HAVING SUM(sales) > 900
HAVING AVG(sales) > 400
HAVING COUNT(*) >= 2
HAVING SUM(sales) > 900 AND COUNT(*) >= 2

-- ── GROUPING SETS ────────────────────────────────────────────────
GROUP BY GROUPING SETS ( (region), (year) )
GROUP BY GROUPING SETS ( (region), (year), () )     -- includes grand total
GROUP BY GROUPING SETS ( (region, product), (region), () )

-- ── ROLLUP ───────────────────────────────────────────────────────
GROUP BY ROLLUP(region, product, category)
-- = GROUPING SETS ( (region,product,category), (region,product), (region), () )

GROUP BY ROLLUP(year, quarter)
-- = GROUPING SETS ( (year,quarter), (year), () )

-- ── CUBE ─────────────────────────────────────────────────────────
GROUP BY CUBE(region, product)
-- = GROUPING SETS ( (region,product), (region), (product), () )

GROUP BY CUBE(region, product, category)
-- = all 8 subsets (2³)

-- ── DEDUPLICATION ────────────────────────────────────────────────
GROUP BY DISTINCT ROLLUP(a, b), ROLLUP(a, c)   -- remove duplicate grouping sets
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **WHERE** | Filters individual rows before grouping — cannot use aggregates |
| **GROUP BY** | Collapses rows sharing the same column values into one summary row |
| **Aggregate function** | Computes one result from many rows: SUM, AVG, COUNT, MIN, MAX |
| **HAVING** | Filters groups after aggregation — can use aggregate expressions |
| **GROUPING SETS** | Runs multiple GROUP BY groupings in a single query |
| **ROLLUP** | Shorthand for hierarchical subtotals — all left-to-right prefixes + grand total |
| **CUBE** | Shorthand for all possible subset combinations of the listed columns |
| **Grand total** | The aggregate of all rows — produced by the empty grouping set `()` |
| **NULL in grouping** | Indicates a row was not grouped by that column in this grouping set |
| **Power set** | All possible subsets of a set — CUBE generates the power set of its columns |
| **GROUP BY DISTINCT** | Removes duplicate grouping sets produced by combined ROLLUP/CUBE clauses |
| **Subtotal** | An aggregate computed over a subset of dimensions (not the full detail) |
| **Hierarchical grouping** | Grouping from finest detail to grand total — what ROLLUP produces |
| **Cross-dimensional** | Aggregates across every possible combination of dimensions — what CUBE produces |

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
