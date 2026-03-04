# 🗃️ Databases, Schemas & Tables — PostgreSQL

> *Understanding the 3-level hierarchy at the heart of PostgreSQL.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Databases%20%26%20Tables-6366F1?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-22C55E?style=for-the-badge)

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

- [The 3-Level Hierarchy](#-the-3-level-hierarchy)
- [What is a Database?](#-what-is-a-database)
- [Creating & Deleting a Database](#-creating--deleting-a-database)
- [What is a Schema?](#-what-is-a-schema)
- [Creating & Using Schemas](#-creating--using-schemas)
- [What is a Table?](#-what-is-a-table)
- [Creating a Table](#-creating-a-table)
- [Deleting a Table](#-deleting-a-table)
- [Table Basics — Deep Dive](#-table-basics--deep-dive)
- [Quick Reference](#-quick-reference)

---

## 🏗️ The 3-Level Hierarchy

PostgreSQL organizes everything in **3 levels**, from largest to smallest:

```
🌐 PostgreSQL Server
│
├── 🗄️  DATABASE  (ecommerce)          ← Level 1 — the container
│    │
│    ├── 📂  SCHEMA  (public)          ← Level 2 — the namespace
│    │    ├── 📊 TABLE: users
│    │    ├── 📊 TABLE: products
│    │    └── 📊 TABLE: orders
│    │
│    ├── 📂  SCHEMA  (analytics)       ← another schema, same database
│    │    ├── 📊 TABLE: reports
│    │    └── 📊 TABLE: metrics
│    │
│    └── 📂  SCHEMA  (archive)
│         └── 📊 TABLE: old_orders
│
└── 🗄️  DATABASE  (blog)              ← a completely separate database
     │
     └── 📂  SCHEMA  (public)
          ├── 📊 TABLE: posts
          └── 📊 TABLE: comments
```

### At a Glance

| Level | Object | Think of it as... | Scope |
|-------|--------|-------------------|-------|
| 1️⃣ | **Database** | A building | Isolated container — separate from other databases |
| 2️⃣ | **Schema** | A floor in the building | A namespace that groups related tables |
| 3️⃣ | **Table** | A room on the floor | Where actual data lives |

> 💡 Every table lives inside a schema. Every schema lives inside a database. You always work through all 3 levels — even when you don't realize it.

---

## 🗂️ What is a Database?

A **database** is an organized collection of data, stored and managed so it can be easily accessed, updated, and maintained.

```
🗂️ Think of it like a building:

  🏢 School Database
  ├── 📂 Schema: public        ← default schema
  │    ├── 📊 students
  │    ├── 📊 teachers
  │    └── 📊 courses
  │
  └── 📂 Schema: reports       ← separate schema
       ├── 📊 grades_summary
       └── 📊 attendance
```

### The Role of a DBMS

A database is always managed by a **Database Management System (DBMS)** — the software layer between you and your data.

```
👤 You / Your App
      │
      ▼
┌─────────────┐
│    DBMS     │  ← Manages access, security, queries
└─────────────┘
      │
      ▼
┌─────────────┐
│  Database   │  ← Contains schemas → which contain tables
└─────────────┘
```

> ⚠️ Without a DBMS, raw data would be scattered files with no way to search, update, or manage them efficiently.

---

## 🛠️ Creating & Deleting a Database

### ✅ Create a Database

```sql
CREATE DATABASE database_name;
```

**Real examples:**

```sql
CREATE DATABASE school;
CREATE DATABASE ecommerce;
CREATE DATABASE blog_platform;
```

---

### ❌ Delete a Database

```sql
DROP DATABASE database_name;
```

<details>
<summary>⚠️ <strong>Click to expand: What happens when you DROP a database?</strong></summary>

```
DROP DATABASE school;

This will permanently:
  ❌ Delete ALL schemas inside the database
  ❌ Delete ALL tables inside those schemas
  ❌ Delete ALL data stored in those tables
  ❌ Delete ALL indexes, functions, and views
  ❌ This action CANNOT be undone

Always double-check before running DROP DATABASE!
```

> 💡 Use `DROP DATABASE IF EXISTS school;` to avoid an error if the database doesn't exist yet.

</details>

---

## 📂 What is a Schema?

A **schema** is a named namespace inside a database that groups related tables, views, functions, and other objects together.

```
Without schemas:                     With schemas:
────────────────                     ────────────────────────────────
📊 users                             📂 public
📊 products                               📊 users
📊 orders                                 📊 products
📊 report_users                      📂 analytics
📊 report_sales                           📊 report_users
📊 archive_orders                         📊 report_sales
📊 archive_users                     📂 archive
                                          📊 orders
Everything mixed together                 📊 users
→ hard to manage                     Clear separation by purpose ✅
```

### Why Schemas Matter

| Benefit | Description |
|---------|-------------|
| 🗂️ **Organisation** | Group related tables by feature, team, or purpose |
| 🔒 **Security** | Grant access per schema — e.g. read-only on `analytics` |
| 🏢 **Multi-tenancy** | One database, multiple tenants each with their own schema |
| 🚫 **Name isolation** | Two schemas can each have a table named `users` — no conflict |

### The `public` Schema

```sql
-- PostgreSQL creates a default schema called "public" in every new database.
-- When you create a table without specifying a schema, it goes into "public":

CREATE TABLE users (...);
-- is exactly the same as:
CREATE TABLE public.users (...);
```

> 💡 `public` is just the default — you can create as many schemas as you need.

---

## 🛠️ Creating & Using Schemas

### ✅ Create a Schema

```sql
CREATE SCHEMA schema_name;
```

**Real examples:**

```sql
CREATE SCHEMA analytics;
CREATE SCHEMA archive;
CREATE SCHEMA tenant_nike;     -- multi-tenancy pattern
```

---

### 📍 Addressing Objects: The Dot Notation

To reference a table in a specific schema, use **dot notation**:

```sql
-- Format:  schema_name.table_name

SELECT * FROM public.users;
SELECT * FROM analytics.reports;
SELECT * FROM archive.old_orders;
```

<details>
<summary>📌 <strong>Click to expand: Full 3-level addressing</strong></summary>

PostgreSQL supports addressing all the way from database down to column:

```sql
-- Full address: database.schema.table
-- (used when connecting across databases or being very explicit)

SELECT * FROM ecommerce.public.users;
--             ↑          ↑      ↑
--         database    schema  table

-- In practice, you're already connected to a database, so:
SELECT * FROM public.users;      -- most common
SELECT * FROM users;             -- shortest (uses search_path default)
```

</details>

<details>
<summary>📌 <strong>Click to expand: search_path — how PostgreSQL finds tables</strong></summary>

When you write `SELECT * FROM users` without a schema name, PostgreSQL looks through a **search path** to find the table:

```sql
-- See your current search path:
SHOW search_path;
-- Returns: "$user", public
-- This means: first look in a schema matching your username, then in "public"

-- Change your search path:
SET search_path TO analytics, public;
-- Now unqualified table names look in "analytics" first, then "public"

-- Set it permanently for a user:
ALTER ROLE myuser SET search_path TO myschema, public;
```

</details>

---

### ❌ Delete a Schema

```sql
DROP SCHEMA schema_name;            -- only works if schema is empty
DROP SCHEMA schema_name CASCADE;    -- deletes schema AND all objects inside it
```

<details>
<summary>⚠️ <strong>Click to expand: CASCADE is dangerous — read this first</strong></summary>

```sql
DROP SCHEMA analytics CASCADE;

-- This will permanently delete:
--   ❌ All tables in the "analytics" schema
--   ❌ All data in those tables
--   ❌ All views, functions, sequences in that schema
--   ❌ Cannot be undone

-- Safer approach: drop tables first, then the schema
DROP TABLE analytics.reports;
DROP TABLE analytics.metrics;
DROP SCHEMA analytics;   -- now safe — schema is empty
```

</details>

---

## 📊 What is a Table?

A **table** is where actual data lives — rows and columns organized to store one type of entity, always inside a schema.

```
📊 public.weather  ← schema.tablename
┌──────────────┬─────────┬─────────┬──────┬────────────┐
│ city         │ temp_lo │ temp_hi │ prcp │ date       │
├──────────────┼─────────┼─────────┼──────┼────────────┤
│ San Francisco│   46    │   50    │ 0.25 │ 1994-11-27 │  ← Row (record)
│ San Francisco│   43    │   57    │ 0.00 │ 1994-11-29 │
│ Hayward      │   37    │   54    │  —   │ 1994-11-29 │
└──────────────┴─────────┴─────────┴──────┴────────────┘
      ↑               ↑                           ↑
  Column (field)   Column (field)           Column (field)
```

### Key Rules About Tables

| Rule | Detail |
|------|--------|
| **Fixed columns** | Number and order of columns is set at creation time |
| **Variable rows** | Rows grow and shrink as data is added or removed |
| **No guaranteed row order** | SQL does NOT promise rows come back in any order — use `ORDER BY` |
| **No automatic unique IDs** | SQL allows completely identical duplicate rows |
| **Column limit** | Between **250 and 1,600 columns** per table depending on data types |

> 💡 Having near 1,600 columns is almost always a sign of **bad database design**. Keep tables focused.

---

## ✅ Creating a Table

### Basic Syntax

```sql
CREATE TABLE schema_name.table_name (
    column_name   data_type,
    column_name   data_type,
    column_name   data_type
);
```

---

### Example 1 — Weather Table (in public schema)

```sql
CREATE TABLE public.weather (
    city      varchar(80),   -- city name, up to 80 characters
    temp_lo   int,           -- low temperature
    temp_hi   int,           -- high temperature
    prcp      real,          -- precipitation (decimal number)
    date      date           -- the date of the reading
);
```

---

### Example 2 — Your First Table

```sql
CREATE TABLE my_first_table (
    first_column   text,     -- accepts any text
    second_column  integer   -- accepts whole numbers only
);
-- ↑ No schema specified → goes into "public" by default
```

<details>
<summary>📌 <strong>Click to expand: Common PostgreSQL Data Types</strong></summary>

| Data Type | What It Stores | Example Value |
|-----------|---------------|---------------|
| `integer` / `int` | Whole numbers | `42`, `-7`, `1000` |
| `real` | Decimal numbers (float) | `3.14`, `0.25` |
| `varchar(n)` | Text up to `n` characters | `'San Francisco'` |
| `text` | Unlimited length text | `'Any string of any length'` |
| `date` | Calendar date | `'1994-11-27'` |
| `boolean` | True or false | `true`, `false` |
| `numeric(p,s)` | Exact decimal (for money) | `9.99`, `1000.00` |
| `timestamp` | Date + time | `'2026-03-01 10:30:00'` |

```sql
CREATE TABLE products (
    id          integer,
    name        varchar(100),
    description text,
    price       numeric(10, 2),
    in_stock    boolean,
    created_at  timestamp
);
```

</details>

<details>
<summary>📌 <strong>Click to expand: Column naming rules</strong></summary>

```
✅ Valid column names:
   city, temp_lo, first_name, created_at, user_id

❌ Invalid column names:
   2fast        → cannot start with a number
   first name   → cannot contain spaces
   select       → cannot use SQL reserved keywords (without quoting)

💡 Best practices:
   • Use snake_case:    first_name, created_at, product_id
   • Be descriptive:   temp_lo  not  t
   • Avoid reserved:   "select" must be quoted if used (don't do this)
```

</details>

---

## ❌ Deleting a Table

```sql
DROP TABLE table_name;
DROP TABLE schema_name.table_name;   -- explicit schema
DROP TABLE IF EXISTS table_name;     -- safe version
```

**Examples:**

```sql
DROP TABLE IF EXISTS public.weather;
DROP TABLE IF EXISTS analytics.reports;
```

<details>
<summary>📌 <strong>Click to expand: DROP TABLE variants — which one to use?</strong></summary>

```sql
-- Standard drop — errors if table doesn't exist:
DROP TABLE students;

-- Safe drop — no error if table doesn't exist:
DROP TABLE IF EXISTS students;

-- Common pattern in SQL scripts (drop & recreate safely):
DROP TABLE IF EXISTS public.students;
CREATE TABLE public.students (
    id    integer,
    name  varchar(100)
);
```

> The `IF EXISTS` variant is especially useful in **SQL scripts** that run multiple times.

</details>

---

## 🔬 Table Basics — Deep Dive

### 1. Row Order is Never Guaranteed

```sql
-- ❌ Don't rely on insertion order:
SELECT * FROM weather;

-- ✅ Always use ORDER BY if order matters:
SELECT * FROM weather ORDER BY date ASC;
SELECT * FROM weather ORDER BY temp_hi DESC;
```

### 2. Duplicate Rows Are Possible

```sql
INSERT INTO my_first_table VALUES ('hello', 1);
INSERT INTO my_first_table VALUES ('hello', 1);  -- exact duplicate ✅ (allowed!)
-- Use PRIMARY KEY to prevent this (covered in next chapter)
```

### 3. Data Types Enforce Integrity

```sql
INSERT INTO demo VALUES (42, 'hello');      -- ✅ works
INSERT INTO demo VALUES ('abc', 'hello');   -- ❌ error: 'abc' is not an integer
INSERT INTO demo VALUES (3.14, 'hello');    -- ⚠️  truncated to 3
```

### 4. Column Limits

<details>
<summary>📌 <strong>Click to expand: Understanding the 250–1,600 column limit</strong></summary>

```
PostgreSQL column limits per table:
  • Fixed-width types (int, date, bool):  up to ~1,600 columns
  • Variable-width types (text, varchar): up to ~250–1,600 columns

In practice:
  ✅ 5–30 columns    → normal, well-designed table
  ⚠️  30–100 columns  → review your design
  ❌ 100+ columns     → almost always a design problem
```

</details>

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All commands at a glance</strong></summary>

```sql
-- ── DATABASES ──────────────────────────────────────────
CREATE DATABASE mydb;
DROP DATABASE IF EXISTS mydb;

-- ── SCHEMAS ────────────────────────────────────────────
CREATE SCHEMA analytics;
DROP SCHEMA analytics;           -- only if empty
DROP SCHEMA analytics CASCADE;   -- force delete everything inside

SHOW search_path;                -- see current schema search order
SET search_path TO analytics, public;

-- ── TABLES ─────────────────────────────────────────────
CREATE TABLE public.users (
    id    integer,
    name  varchar(100),
    email text
);

DROP TABLE IF EXISTS public.users;

-- ── VIEWING (psql) ─────────────────────────────────────
\l                    -- list all databases
\dn                   -- list all schemas
\dt                   -- list tables in current schema
\dt analytics.*       -- list tables in a specific schema
\d tablename          -- describe a table's structure
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Database** | Top-level container — holds all schemas, tables, and data |
| **Schema** | A namespace inside a database that groups related objects |
| **Table** | A grid of rows and columns — where actual data is stored |
| **`public` schema** | The default schema created in every new PostgreSQL database |
| **Dot notation** | `schema.table` syntax to explicitly address an object |
| **search_path** | The order PostgreSQL searches schemas when no schema is specified |
| **`CASCADE`** | Forces a drop to delete all dependent objects too |
| **DBMS** | Software managing storage, access, and security of databases |
| **Row** | A single record in a table |
| **Column** | A single attribute with a fixed data type |
| **`IF EXISTS`** | Prevents errors when dropping non-existent objects |

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
