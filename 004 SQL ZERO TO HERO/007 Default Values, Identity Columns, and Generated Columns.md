# ⚙️ Default Values, Identity & Generated Columns — PostgreSQL

> *Letting PostgreSQL do the heavy lifting — demonstrated with a real student & university database.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Column%20Features-6366F1?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Intermediate-F59E0B?style=for-the-badge)

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

- [The Big Picture](#-the-big-picture)
- [Our Practice Schema](#-our-practice-schema)
- [5.2 Default Values & SERIAL](#-52-default-values--serial)
- [5.3 Identity Columns](#-53-identity-columns)
- [5.4 Generated Columns](#-54-generated-columns)
- [Side-by-Side Comparison](#-side-by-side-comparison)
- [Quick Reference](#-quick-reference)

---

## 🗺️ The Big Picture

PostgreSQL gives you three powerful ways to have columns fill themselves automatically:

```
┌─────────────────────────────────────────────────────────────────┐
│              AUTOMATIC COLUMN VALUE STRATEGIES                  │
├──────────────────┬──────────────────┬───────────────────────────┤
│  DEFAULT VALUE   │ IDENTITY COLUMN  │   GENERATED COLUMN        │
│                  │                  │                           │
│  A preset value  │  Auto-increment  │  Always computed from     │
│  used when no    │  integer — like  │  other columns in the     │
│  value supplied  │  an ID counter   │  same row                 │
│                  │                  │                           │
│  university_id   │  university_id   │  full_address GENERATED   │
│  DEFAULT 1       │  SERIAL          │  ALWAYS AS                │
│                  │                  │  (name || ', ' || addr)   │
└──────────────────┴──────────────────┴───────────────────────────┘
```

---

## 🏗️ Our Practice Schema

All examples in this guide use a `student_personal` schema with two real tables:

```sql
-- Step 1: Set up base tables (no auto-increment yet)
DROP TABLE student_personal.student_detail;
DROP TABLE student_personal.university_detail;

CREATE TABLE student_personal.student_detail (
    name          text,
    age           integer,
    address       text,
    university_id integer
);

CREATE TABLE student_personal.university_detail (
    university_id      integer,
    university_name    text,
    university_address text
);

-- Verify they exist:
SELECT * FROM student_personal.student_detail;
SELECT * FROM student_personal.university_detail;
```

```
📂 student_personal (schema)
├── 📊 student_detail
│    ├── name          text
│    ├── age           integer
│    ├── address       text
│    └── university_id integer
│
└── 📊 university_detail
     ├── university_id      integer
     ├── university_name    text
     └── university_address text
```

> 💡 Notice `university_id` in `student_detail` — this will link a student to their university once we set up proper IDs.

---

## 🔢 5.2 Default Values & SERIAL

A **default value** is a fallback — if no value is provided for a column during `INSERT`, PostgreSQL uses the default instead.

### The Rule

```
No default declared  →  column defaults to NULL
Default declared     →  column uses that value when nothing is provided
```

---

### Upgrading university_detail with SERIAL

The plain `integer` for `university_id` means we have to supply IDs manually. Let's fix that with `SERIAL`:

```sql
-- Drop and recreate with SERIAL auto-increment:
DROP TABLE IF EXISTS student_personal.university_detail;

CREATE TABLE student_personal.university_detail (
    university_id      SERIAL,   -- ← auto-incrementing, starts at 1
    university_name    text,
    university_address text
);
```

---

### Inserting Without Specifying the ID

Now we don't need to supply `university_id` at all — PostgreSQL handles it:

```sql
-- Method 1: Skip the id column entirely in the column list
INSERT INTO student_personal.university_detail
    (university_name, university_address)
VALUES
    ('MIT',     'MIT BUILDING'),
    ('HAVARDS', 'HAVARDS BUILDING');

SELECT * FROM student_personal.university_detail;
```

**Result:**

```
university_id │ university_name │ university_address
──────────────┼─────────────────┼────────────────────
      1       │ MIT             │ MIT BUILDING        ← auto-generated
      2       │ HAVARDS         │ HAVARDS BUILDING    ← auto-generated
```

---

### Using the DEFAULT Keyword Explicitly

```sql
-- Method 2: Include the column but use DEFAULT keyword
INSERT INTO student_personal.university_detail
VALUES
    (DEFAULT, 'MIT',     'MIT BUILDING'),
    (DEFAULT, 'HAVARDS', 'HAVARDS BUILDING');

SELECT * FROM student_personal.university_detail;
```

**Result:**

```
university_id │ university_name │ university_address
──────────────┼─────────────────┼────────────────────
      1       │ MIT             │ MIT BUILDING
      2       │ HAVARDS         │ HAVARDS BUILDING
      3       │ MIT             │ MIT BUILDING        ← DEFAULT continued sequence
      4       │ HAVARDS         │ HAVARDS BUILDING    ← DEFAULT continued sequence
```

> 💡 `DEFAULT` in the values list explicitly tells PostgreSQL: *"use the default for this column"* — the same as omitting the column entirely.

<details>
<summary>📌 <strong>Click to expand: SERIAL variants — which size do you need?</strong></summary>

| Type | Underlying Type | Range | Use When |
|------|----------------|-------|----------|
| `SMALLSERIAL` | `smallint` | 1 to 32,767 | Tiny lookup tables |
| `SERIAL` | `integer` | 1 to 2,147,483,647 | Most use cases ✅ |
| `BIGSERIAL` | `bigint` | 1 to 9,223,372,036,854,775,807 | Very large tables |

```sql
-- For a high-volume student log, use BIGSERIAL:
CREATE TABLE student_personal.student_activity_log (
    log_id     BIGSERIAL,
    student    text,
    action     text,
    logged_at  timestamp DEFAULT CURRENT_TIMESTAMP
);
```

> ⚠️ `SERIAL` is a convenience shorthand — PostgreSQL actually creates a **sequence object** behind the scenes. Consider `IDENTITY` columns (section 5.3) for a more modern, SQL-standard approach.

</details>

---

## 🪪 5.3 Identity Columns

An **identity column** is the modern, SQL-standard replacement for `SERIAL`. It gives you more control over how values are generated and whether users can override them.

### Two Variants

```sql
-- GENERATED ALWAYS — PostgreSQL fully controls the value
CREATE TABLE student_personal.university_detail (
    university_id      bigint GENERATED ALWAYS AS IDENTITY,
    university_name    text,
    university_address text
);

-- GENERATED BY DEFAULT — user can supply their own value too
CREATE TABLE student_personal.university_detail (
    university_id      integer GENERATED BY DEFAULT AS IDENTITY,
    university_name    text,
    university_address text
);
```

---

### Live Example — GENERATED BY DEFAULT in Action

```sql
-- Recreate with GENERATED BY DEFAULT:
DROP TABLE IF EXISTS student_personal.university_detail;

CREATE TABLE student_personal.university_detail (
    university_id      integer GENERATED BY DEFAULT AS IDENTITY,
    university_name    text,
    university_address text
);

-- Mix of DEFAULT and explicit values — all allowed with BY DEFAULT:
INSERT INTO student_personal.university_detail
VALUES
    (DEFAULT, 'MIT',     'MIT BUILDING'),      -- ← sequence generates: 1
    (1,       'HAVARDS', 'HAVARDS BUILDING'),   -- ← user supplies: 1 (allowed!)
    (DEFAULT, 'MIT',     'MIT BUILDING'),       -- ← sequence generates: 2
    (1,       'HAVARDS', 'HAVARDS BUILDING');   -- ← user supplies: 1 again (allowed!)

SELECT * FROM student_personal.university_detail;
```

**Result:**

```
university_id │ university_name │ university_address
──────────────┼─────────────────┼────────────────────
      1       │ MIT             │ MIT BUILDING        ← DEFAULT (sequence)
      1       │ HAVARDS         │ HAVARDS BUILDING    ← explicit 1 (user supplied)
      2       │ MIT             │ MIT BUILDING        ← DEFAULT (sequence)
      1       │ HAVARDS         │ HAVARDS BUILDING    ← explicit 1 again
```

> ⚠️ Notice `university_id = 1` appears **3 times** — identity columns do **not** guarantee uniqueness on their own. Add a `PRIMARY KEY` constraint if you need that.

---

### Checking NULL Behaviour

```sql
-- NULL compared to any value always returns NULL in SQL:
SELECT NULL > university_id
FROM student_personal.university_detail;
```

**Result:**

```
?column?
──────────
  (null)
  (null)
  (null)
  (null)
```

> 💡 This is a fundamental SQL rule — **any comparison involving NULL returns NULL** (not TRUE or FALSE). Use `IS NULL` / `IS NOT NULL` to check for null values.

---

### ALWAYS vs BY DEFAULT — What's the Difference?

<details>
<summary>📌 <strong>Click to expand: Full comparison with university_detail examples</strong></summary>

```sql
-- With GENERATED ALWAYS:
INSERT INTO student_personal.university_detail
VALUES (99, 'STANFORD', 'STANFORD BUILDING');
-- ❌ ERROR — cannot supply your own university_id

-- Must use OVERRIDING SYSTEM VALUE to force it:
INSERT INTO student_personal.university_detail
    OVERRIDING SYSTEM VALUE
VALUES (99, 'STANFORD', 'STANFORD BUILDING');
-- ✅ Works — but you're explicitly forcing it

-- With GENERATED BY DEFAULT (our current setup):
INSERT INTO student_personal.university_detail
VALUES (99, 'STANFORD', 'STANFORD BUILDING');
-- ✅ Works — user value simply takes precedence
```

| Scenario | `GENERATED ALWAYS` | `GENERATED BY DEFAULT` |
|----------|--------------------|------------------------|
| Omit id / use DEFAULT | ✅ Auto-generated | ✅ Auto-generated |
| Supply explicit value | ❌ Error | ✅ Allowed |
| Force explicit value | ✅ With `OVERRIDING SYSTEM VALUE` | ✅ Just insert normally |
| Protection level | 🔒 High | 🔓 Flexible |
| Best for | Strict primary keys | Data migrations, flexibility |

</details>

<details>
<summary>📌 <strong>Click to expand: What identity columns guarantee — and what they don't</strong></summary>

```
✅ Identity columns ARE automatically:
   • NOT NULL — university_id can never be NULL
   • Incrementing — each DEFAULT insert gets the next sequence value

⚠️  Identity columns DO NOT automatically guarantee:
   • UNIQUENESS — as seen above, duplicate IDs are possible with BY DEFAULT

Fix it by adding PRIMARY KEY:
CREATE TABLE student_personal.university_detail (
    university_id      integer GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    university_name    text,
    university_address text
);
```

</details>

---

## 🔄 5.4 Generated Columns

A **generated column** is always automatically computed from other columns in the same row. You never write to it — PostgreSQL maintains it for you whenever the row changes.

> 💡 Think of it like a **formula cell in a spreadsheet** — it recalculates automatically whenever its source values change.

### Two Kinds

```
VIRTUAL generated column      STORED generated column
──────────────────────────    ──────────────────────────
Computed when READ            Computed when WRITTEN
Occupies no storage           Occupies storage on disk
Like a VIEW                   Like a MATERIALIZED VIEW (always fresh)
```

---

### Example — Full University Info as a Generated Column

```sql
DROP TABLE IF EXISTS student_personal.university_detail;

CREATE TABLE student_personal.university_detail (
    university_id       integer GENERATED BY DEFAULT AS IDENTITY,
    university_name     text,
    university_address  text,
    university_full_info text GENERATED ALWAYS AS
                             (university_name || ' — ' || university_address) STORED
    --                        ↑ concatenates name + address automatically
);

-- Insert without touching university_full_info:
INSERT INTO student_personal.university_detail
    (university_name, university_address)
VALUES
    ('MIT',     'MIT BUILDING'),
    ('HAVARDS', 'HAVARDS BUILDING');

SELECT * FROM student_personal.university_detail;
```

**Result:**

```
university_id │ university_name │ university_address  │ university_full_info
──────────────┼─────────────────┼─────────────────────┼───────────────────────────────
      1       │ MIT             │ MIT BUILDING        │ MIT — MIT BUILDING
      2       │ HAVARDS         │ HAVARDS BUILDING    │ HAVARDS — HAVARDS BUILDING
```

> The `university_full_info` column was never written to — it was **computed automatically** from the other two columns.

---

### Update Triggers Re-computation

```sql
-- If we update the address, full_info updates automatically:
UPDATE student_personal.university_detail
SET university_address = 'NEW MIT CAMPUS'
WHERE university_name = 'MIT';

SELECT university_name, university_full_info
FROM student_personal.university_detail;
```

**Result:**

```
university_name │ university_full_info
────────────────┼──────────────────────────────
MIT             │ MIT — NEW MIT CAMPUS          ← auto-recomputed!
HAVARDS         │ HAVARDS — HAVARDS BUILDING
```

---

### Key Restrictions on Generated Columns

<details>
<summary>⚠️ <strong>Click to expand: What you CANNOT do with generated columns</strong></summary>

```sql
-- ❌ Cannot write to them directly:
UPDATE student_personal.university_detail
SET university_full_info = 'CUSTOM VALUE';
-- ERROR: cannot update column "university_full_info" of table "university_detail"

-- ❌ Cannot use volatile functions (random values, current time):
last_updated text GENERATED ALWAYS AS (CURRENT_TIMESTAMP::text) STORED
-- ERROR — CURRENT_TIMESTAMP is volatile
-- Use DEFAULT CURRENT_TIMESTAMP on a separate column instead

-- ❌ Cannot reference another generated column:
-- If university_full_info is generated, you can't use it in another generated column

-- ❌ Cannot use subqueries:
-- GENERATED ALWAYS AS (SELECT name FROM other_table LIMIT 1)  -- ❌

-- ✅ CAN use immutable built-in functions:
lower(university_name)                              -- fine
upper(university_address)                           -- fine
university_name || ' — ' || university_address      -- fine
```

</details>

---

## ↔️ Side-by-Side Comparison

| Feature | SERIAL (Default) | Identity Column | Generated Column |
|---------|-----------------|-----------------|-----------------|
| **Example** | `university_id SERIAL` | `university_id GENERATED BY DEFAULT AS IDENTITY` | `full_info GENERATED ALWAYS AS (name \|\| addr)` |
| **Purpose** | Auto-increment ID | Auto-increment ID (SQL standard) | Computed from other columns |
| **Can user override?** | ✅ Yes | ⚠️ Depends on `ALWAYS` vs `BY DEFAULT` | ❌ Never |
| **References other columns?** | ❌ No | ❌ No | ✅ Yes — that's the point |
| **Evaluated when?** | At INSERT | At INSERT | At INSERT **and** UPDATE |
| **Guarantees NOT NULL?** | ❌ No | ✅ Yes | ❌ No |
| **Guarantees uniqueness?** | ❌ No | ❌ No (add PRIMARY KEY) | ❌ No |
| **SQL Standard?** | ❌ PostgreSQL-specific | ✅ Yes | ✅ Yes |

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: Full session SQL — all commands from this guide</strong></summary>

```sql
-- ── SETUP ───────────────────────────────────────────────────────
DROP TABLE student_personal.student_detail;
DROP TABLE student_personal.university_detail;

CREATE TABLE student_personal.student_detail (
    name          text,
    age           integer,
    address       text,
    university_id integer
);

-- ── SERIAL (DEFAULT shorthand) ───────────────────────────────────
DROP TABLE IF EXISTS student_personal.university_detail;
CREATE TABLE student_personal.university_detail (
    university_id      SERIAL,
    university_name    text,
    university_address text
);

INSERT INTO student_personal.university_detail
    (university_name, university_address)
VALUES ('MIT', 'MIT BUILDING'), ('HAVARDS', 'HAVARDS BUILDING');

INSERT INTO student_personal.university_detail
VALUES (DEFAULT, 'MIT', 'MIT BUILDING'), (DEFAULT, 'HAVARDS', 'HAVARDS BUILDING');

-- ── IDENTITY COLUMN ──────────────────────────────────────────────
DROP TABLE IF EXISTS student_personal.university_detail;
CREATE TABLE student_personal.university_detail (
    university_id      integer GENERATED BY DEFAULT AS IDENTITY,
    university_name    text,
    university_address text
);

INSERT INTO student_personal.university_detail
VALUES
    (DEFAULT, 'MIT',     'MIT BUILDING'),
    (1,       'HAVARDS', 'HAVARDS BUILDING'),
    (DEFAULT, 'MIT',     'MIT BUILDING'),
    (1,       'HAVARDS', 'HAVARDS BUILDING');

-- ── NULL BEHAVIOUR ────────────────────────────────────────────────
SELECT NULL > university_id FROM student_personal.university_detail;
-- Returns NULL for every row — NULL comparisons always return NULL

-- ── VERIFY ────────────────────────────────────────────────────────
SELECT * FROM student_personal.student_detail;
SELECT * FROM student_personal.university_detail;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Default value** | A fallback value used when no value is supplied during INSERT |
| **NULL** | The absence of a value — any comparison with NULL returns NULL |
| **`DEFAULT` keyword** | Used in INSERT/UPDATE to explicitly request the default/generated value |
| **SERIAL** | PostgreSQL shorthand for an auto-incrementing integer using a sequence |
| **Sequence** | A database object that generates successive unique integers |
| **Identity column** | A column auto-generated from a sequence using SQL-standard syntax |
| **`GENERATED ALWAYS`** | Identity mode — PostgreSQL controls the value, user overrides blocked |
| **`GENERATED BY DEFAULT`** | Identity mode — user-supplied values take precedence over sequence |
| **`OVERRIDING SYSTEM VALUE`** | Forces an explicit value into a `GENERATED ALWAYS` column |
| **Generated column** | A column always computed from other columns using an expression |
| **STORED** | Generated column computed on write, saved to disk |
| **VIRTUAL** | Generated column computed on read, uses no storage |
| **Immutable function** | Always returns same result for same input — required for generated columns |

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
