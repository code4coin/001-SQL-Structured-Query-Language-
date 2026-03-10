# 🔧 Modifying Tables — ALTER TABLE — PostgreSQL

> *Your table isn't set in stone. Change columns, constraints, types, and names without losing data.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-ALTER%20TABLE-6366F1?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Intermediate-F59E0B?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-5.7-gray?style=for-the-badge)

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

- [Why ALTER TABLE?](#-why-alter-table)
- [Our Practice Schema](#-our-practice-schema)
- [5.7.1 Adding a Column](#-571-adding-a-column)
- [5.7.2 Removing a Column](#-572-removing-a-column)
- [5.7.3 Adding a Constraint](#-573-adding-a-constraint)
- [5.7.4 Removing a Constraint](#-574-removing-a-constraint)
- [5.7.5 Changing a Column's Default](#-575-changing-a-columns-default)
- [5.7.6 Changing a Column's Data Type](#-576-changing-a-columns-data-type)
- [5.7.7 Renaming a Column](#-577-renaming-a-column)
- [5.7.8 Renaming a Table](#-578-renaming-a-table)
- [Full Session Recap](#-full-session-recap)
- [Quick Reference](#-quick-reference)

---

## 🤔 Why ALTER TABLE?

Dropping and recreating a table works only on empty tables. Once your table has data — or other objects reference it — you need `ALTER TABLE`.

```
DROP + CREATE approach:           ALTER TABLE approach:
──────────────────────            ──────────────────────────
✅ Works on empty tables          ✅ Works even with data
❌ Destroys existing data         ✅ Preserves all existing rows
❌ Breaks foreign key refs        ✅ Keeps FK relationships intact
❌ Requires full re-setup         ✅ Surgical — change only what you need
```

### What You Can Change

| # | Operation | Command |
|---|-----------|---------|
| 1 | Add a column | `ALTER TABLE … ADD COLUMN` |
| 2 | Remove a column | `ALTER TABLE … DROP COLUMN` |
| 3 | Add a constraint | `ALTER TABLE … ADD CONSTRAINT` |
| 4 | Remove a constraint | `ALTER TABLE … DROP CONSTRAINT` |
| 5 | Set / drop NOT NULL | `ALTER TABLE … ALTER COLUMN … SET/DROP NOT NULL` |
| 6 | Set / drop default value | `ALTER TABLE … ALTER COLUMN … SET/DROP DEFAULT` |
| 7 | Change column data type | `ALTER TABLE … ALTER COLUMN … TYPE` |
| 8 | Rename a column | `ALTER TABLE … RENAME COLUMN` |
| 9 | Rename a table | `ALTER TABLE … RENAME TO` |

---

## 🏗️ Our Practice Schema

All examples use a `student` schema with a `stud_dtl` table:

```sql
-- 1. Create the schema
DROP SCHEMA IF EXISTS student CASCADE;
CREATE SCHEMA student;

-- 2. Create the table
DROP TABLE IF EXISTS student.stud_dtl;
CREATE TABLE student.stud_dtl (
    id    integer  GENERATED ALWAYS AS IDENTITY  PRIMARY KEY,
    name  text,
    age   int  CONSTRAINT age_check_18 CHECK (age >= 18)
);

-- 3. Insert sample data
INSERT INTO student.stud_dtl VALUES
    (DEFAULT, 'Nitin',  20),
    (DEFAULT, 'Robert', 35);

-- Verify:
SELECT * FROM student.stud_dtl;
```

**Starting state:**

```
 id │ name   │ age
────┼────────┼─────
  1 │ Nitin  │  20
  2 │ Robert │  35
```

```
📂 student (schema)
└── 📊 stud_dtl
     ├── id    integer  GENERATED ALWAYS AS IDENTITY  PRIMARY KEY
     ├── name  text
     └── age   int  CHECK (age >= 18)
```

---

## ➕ 5.7.1 Adding a Column

```sql
ALTER TABLE student.stud_dtl ADD COLUMN uni_id integer;
```

```
Before:                          After:
──────────────────               ─────────────────────────────
id │ name   │ age                id │ name   │ age │ uni_id
───┼────────┼─────               ───┼────────┼─────┼────────
 1 │ Nitin  │ 20                  1 │ Nitin  │ 20  │ NULL   ← filled with NULL
 2 │ Robert │ 35                  2 │ Robert │ 35  │ NULL
```

> 💡 New columns are filled with `NULL` by default (or the `DEFAULT` value if you specify one).

---

### Adding a Column WITH a Default

```sql
-- New column with a constant default — very fast even on large tables:
ALTER TABLE student.stud_dtl ADD COLUMN uni_id integer DEFAULT 0;
```

<details>
<summary>📌 <strong>Click to expand: Performance tip — constant vs volatile defaults</strong></summary>

```
Constant default (DEFAULT 0, DEFAULT 'active'):
  ✅ PostgreSQL does NOT rewrite every row immediately
  ✅ Returns the default value on access — very fast ALTER even on huge tables

Volatile default (DEFAULT clock_timestamp(), DEFAULT random()):
  ⚠️ PostgreSQL MUST update every row at ALTER TABLE time
  ⚠️ Can be very slow on large tables

Better approach for volatile defaults on large tables:
  Step 1: ADD COLUMN with no default
  Step 2: UPDATE rows with the correct value
  Step 3: SET DEFAULT afterwards
```

</details>

---

### Adding a Column WITH a Constraint

```sql
ALTER TABLE student.stud_dtl ADD COLUMN uni_id integer CHECK (uni_id > 0);
-- Any column option valid in CREATE TABLE is also valid here
```

> ⚠️ The default value must satisfy any constraint you add — otherwise the `ADD COLUMN` will fail.

---

## ➖ 5.7.2 Removing a Column

```sql
ALTER TABLE student.stud_dtl DROP COLUMN uni_id;
```

```
Before:                                  After:
─────────────────────────────            ──────────────────
id │ name   │ age │ uni_id               id │ name   │ age
───┼────────┼─────┼────────              ───┼────────┼─────
 1 │ Nitin  │ 20  │ NULL                  1 │ Nitin  │ 20
 2 │ Robert │ 35  │ NULL                  2 │ Robert │ 35
```

> ⚠️ **All data in the column is permanently deleted.** Table constraints involving that column are dropped too.

---

### DROP COLUMN with CASCADE

```sql
-- If another table's FK references this column, normal DROP will fail:
ALTER TABLE student.stud_dtl DROP COLUMN uni_id;
-- ERROR: column is referenced by foreign key constraint ...

-- Use CASCADE to drop the column AND everything that depends on it:
ALTER TABLE student.stud_dtl DROP COLUMN uni_id CASCADE;
-- ✅ Drops the column + any dependent FK constraints automatically
```

---

## 🔒 5.7.3 Adding a Constraint

### Add a CHECK Constraint

```sql
ALTER TABLE student.stud_dtl ADD CONSTRAINT age_check_50 CHECK (age <= 50);
-- Now age must be >= 18 AND <= 50
```

> ⚠️ The constraint is **checked immediately** against all existing rows — if any row violates it, the `ALTER TABLE` fails.

---

### Add Other Constraint Types

```sql
-- Add a UNIQUE constraint:
ALTER TABLE student.stud_dtl ADD CONSTRAINT unique_name UNIQUE (name);

-- Add a FOREIGN KEY:
ALTER TABLE student.stud_dtl
    ADD FOREIGN KEY (uni_id) REFERENCES student.university_detail (university_id);

-- Add an unnamed CHECK (system names it for you):
ALTER TABLE student.stud_dtl ADD CHECK (name <> '');
```

---

### Add NOT NULL

```sql
-- NOT NULL uses a different syntax — ALTER COLUMN, not ADD CONSTRAINT:
ALTER TABLE student.stud_dtl ALTER COLUMN name SET NOT NULL;
```

```
Before:                    After:
name = 'Nitin'  ✅         name = 'Nitin'  ✅
name = NULL     ✅         name = NULL     ❌ rejected
```

---

## 🗑️ 5.7.4 Removing a Constraint

### Drop a Named Constraint

```sql
-- Drop the age upper limit we added earlier:
ALTER TABLE student.stud_dtl DROP CONSTRAINT age_check_50;
```

<details>
<summary>📌 <strong>Click to expand: How to find a constraint's name if you didn't name it</strong></summary>

```sql
-- In psql, describe the table to see all constraint names:
\d student.stud_dtl

-- Or query the system catalog:
SELECT conname, contype, pg_get_constraintdef(oid)
FROM pg_constraint
WHERE conrelid = 'student.stud_dtl'::regclass;

-- contype values:
--   c = CHECK
--   n = NOT NULL
--   u = UNIQUE
--   p = PRIMARY KEY
--   f = FOREIGN KEY
```

</details>

---

### Drop NOT NULL

```sql
ALTER TABLE student.stud_dtl ALTER COLUMN name DROP NOT NULL;
-- name can now be NULL again
```

> 💡 If the column doesn't have a NOT NULL constraint, this command **silently does nothing** — no error.

---

### Drop with CASCADE

```sql
-- If other constraints depend on the one you're dropping (e.g. a FK depends on a UNIQUE):
ALTER TABLE student.stud_dtl DROP CONSTRAINT some_name CASCADE;
-- Drops the constraint AND anything that depends on it
```

---

## ⚙️ 5.7.5 Changing a Column's Default

### Set a New Default

```sql
-- Set a default for uni_id:
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id SET DEFAULT NULL;
```

> ⚠️ This only affects **future INSERT** commands — existing rows are **not changed**.

```
Existing rows:   unchanged — uni_id stays as-is
Future INSERTs:  if uni_id not specified → uses new default
```

---

### Drop the Default

```sql
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id DROP DEFAULT;
-- Equivalent to setting the default to NULL
-- Not an error if the column had no default — silently does nothing
```

---

## 🔄 5.7.6 Changing a Column's Data Type

### Simple Type Change

```sql
-- Change uni_id from integer to text:
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE text;
```

```
Before: uni_id integer  (e.g. 101)
After:  uni_id text     (e.g. '101'  ← implicitly cast)
```

---

### Type Change with USING — Complex Conversion

When an implicit cast doesn't exist, use `USING` to define the conversion:

```sql
-- Change uni_id back from text to integer:
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE int USING uni_id::integer;
--                                                         ↑ USING clause
--                                   tells PostgreSQL HOW to convert the existing values
```

<details>
<summary>📌 <strong>Click to expand: USING clause — when and why you need it</strong></summary>

```sql
-- Implicit cast works (no USING needed):
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE text;
-- integer → text always works implicitly

-- Implicit cast does NOT work (USING required):
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE int;
-- text → integer may fail if values like 'abc' exist
-- Use USING to be explicit:
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE int USING uni_id::integer;

-- Custom USING expression example:
-- Convert a text column '2026-03-01' to a date:
ALTER TABLE logs ALTER COLUMN log_date TYPE date USING log_date::date;

-- Best practice before changing types:
-- Step 1: DROP any constraints on the column
-- Step 2: ALTER the type
-- Step 3: Re-add modified constraints
```

</details>

---

## ✏️ 5.7.7 Renaming a Column

```sql
-- Rename uni_id to university_id:
ALTER TABLE student.stud_dtl RENAME COLUMN uni_id TO university_id;
```

```
Before:  id │ name │ age │ uni_id
After:   id │ name │ age │ university_id
```

```sql
-- And rename it back:
ALTER TABLE student.stud_dtl RENAME COLUMN university_id TO uni_id;
```

> 💡 Renaming a column does **not** affect the data or any constraints — only the column name changes.

---

## 📋 5.7.8 Renaming a Table

```sql
-- Rename stud_dtl to detail:
ALTER TABLE student.stud_dtl RENAME TO detail;

-- And rename it back:
ALTER TABLE student.detail RENAME TO stud_dtl;
```

```
Before:  student.stud_dtl
After:   student.detail
```

> 💡 Renaming a table does **not** move it to a different schema — only the name changes. All data, constraints, and indexes remain intact.

---

## 🔁 Full Session Recap

Here's the complete sequence of all 12 operations from this guide in order:

```sql
-- ── SETUP ────────────────────────────────────────────────────────
DROP SCHEMA IF EXISTS student CASCADE;
CREATE SCHEMA student;

DROP TABLE IF EXISTS student.stud_dtl;
CREATE TABLE student.stud_dtl (
    id    integer GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name  text,
    age   int CONSTRAINT age_check_18 CHECK (age >= 18)
);

INSERT INTO student.stud_dtl VALUES
    (DEFAULT, 'Nitin',  20),
    (DEFAULT, 'Robert', 35);

-- ── 1. ADD COLUMN ────────────────────────────────────────────────
ALTER TABLE student.stud_dtl ADD COLUMN uni_id integer;

-- ── 2. DROP COLUMN ───────────────────────────────────────────────
ALTER TABLE student.stud_dtl DROP COLUMN uni_id;

-- ── 3. ADD COLUMN (again for next steps) ─────────────────────────
ALTER TABLE student.stud_dtl ADD COLUMN uni_id integer;

-- ── 4. ADD CONSTRAINT ────────────────────────────────────────────
ALTER TABLE student.stud_dtl ADD CONSTRAINT age_check_50 CHECK (age <= 50);

-- ── 5. DROP CONSTRAINT ───────────────────────────────────────────
ALTER TABLE student.stud_dtl DROP CONSTRAINT age_check_50;

-- ── 6. SET NOT NULL ──────────────────────────────────────────────
ALTER TABLE student.stud_dtl ALTER COLUMN name SET NOT NULL;

-- ── 7. DROP NOT NULL ─────────────────────────────────────────────
ALTER TABLE student.stud_dtl ALTER COLUMN name DROP NOT NULL;

-- ── 8. SET DEFAULT ───────────────────────────────────────────────
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id SET DEFAULT NULL;

-- ── 9. DROP DEFAULT ──────────────────────────────────────────────
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id DROP DEFAULT;

-- ── 10. CHANGE DATA TYPE ─────────────────────────────────────────
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE text;
ALTER TABLE student.stud_dtl ALTER COLUMN uni_id TYPE int USING uni_id::integer;

-- ── 11. RENAME COLUMN ────────────────────────────────────────────
ALTER TABLE student.stud_dtl RENAME COLUMN uni_id TO university_id;
ALTER TABLE student.stud_dtl RENAME COLUMN university_id TO uni_id;

-- ── 12. RENAME TABLE ─────────────────────────────────────────────
ALTER TABLE student.stud_dtl RENAME TO detail;
ALTER TABLE student.detail   RENAME TO stud_dtl;

-- ── VERIFY ───────────────────────────────────────────────────────
SELECT * FROM student.stud_dtl;
```

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All ALTER TABLE syntax at a glance</strong></summary>

```sql
-- ── ADD COLUMN ───────────────────────────────────────────────────
ALTER TABLE t ADD COLUMN col integer;
ALTER TABLE t ADD COLUMN col integer DEFAULT 0;
ALTER TABLE t ADD COLUMN col text CHECK (col <> '');

-- ── DROP COLUMN ──────────────────────────────────────────────────
ALTER TABLE t DROP COLUMN col;
ALTER TABLE t DROP COLUMN col CASCADE;          -- drop dependents too

-- ── ADD CONSTRAINT ───────────────────────────────────────────────
ALTER TABLE t ADD CONSTRAINT name CHECK (col > 0);
ALTER TABLE t ADD CONSTRAINT name UNIQUE (col);
ALTER TABLE t ADD FOREIGN KEY (col) REFERENCES other_table;
ALTER TABLE t ADD CHECK (col <> '');            -- unnamed

-- ── NOT NULL ─────────────────────────────────────────────────────
ALTER TABLE t ALTER COLUMN col SET NOT NULL;
ALTER TABLE t ALTER COLUMN col DROP NOT NULL;

-- ── DROP CONSTRAINT ──────────────────────────────────────────────
ALTER TABLE t DROP CONSTRAINT constraint_name;
ALTER TABLE t DROP CONSTRAINT constraint_name CASCADE;

-- ── DEFAULT VALUE ────────────────────────────────────────────────
ALTER TABLE t ALTER COLUMN col SET DEFAULT 0;
ALTER TABLE t ALTER COLUMN col SET DEFAULT NULL;
ALTER TABLE t ALTER COLUMN col DROP DEFAULT;

-- ── CHANGE DATA TYPE ─────────────────────────────────────────────
ALTER TABLE t ALTER COLUMN col TYPE text;
ALTER TABLE t ALTER COLUMN col TYPE int USING col::integer;

-- ── RENAME ───────────────────────────────────────────────────────
ALTER TABLE t RENAME COLUMN old_name TO new_name;
ALTER TABLE old_table RENAME TO new_table;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **ALTER TABLE** | DDL command for modifying an existing table's structure |
| **DDL** | Data Definition Language — SQL commands that define/modify structure (CREATE, ALTER, DROP) |
| **ADD COLUMN** | Adds a new column to an existing table; existing rows get NULL or default |
| **DROP COLUMN** | Permanently removes a column and all its data |
| **CASCADE** | Forces a DROP to also remove dependent objects (FKs, constraints, etc.) |
| **ADD CONSTRAINT** | Attaches a new CHECK, UNIQUE, or FK rule to an existing table |
| **DROP CONSTRAINT** | Removes a named constraint from a table |
| **SET NOT NULL** | Makes a column reject NULL values going forward |
| **DROP NOT NULL** | Allows NULL values in a previously NOT NULL column |
| **SET DEFAULT** | Defines or changes the default value for future INSERTs |
| **DROP DEFAULT** | Removes the default, making the column default to NULL |
| **TYPE** | Changes a column's data type; may require a USING clause |
| **USING** | Specifies how to convert existing values when changing column type |
| **RENAME COLUMN** | Changes a column's name without touching its data or constraints |
| **RENAME TO** | Changes a table's name within the same schema |
| **Implicit cast** | An automatic type conversion PostgreSQL can do without instructions |

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
