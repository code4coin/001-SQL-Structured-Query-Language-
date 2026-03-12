# 🗂️ Data Types — PostgreSQL

> *Choosing the right data type is the first line of defence for data quality — and a major factor in performance.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Data%20Types-8B5CF6?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-3B82F6?style=for-the-badge)
![Section](https://img.shields.io/badge/Section-8.1%20|%208.3%20|%208.5%20|%208.6%20|%208.7-gray?style=for-the-badge)

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
- [8.1.1 Integer Types](#-811-integer-types)
- [8.1.2 Numeric — Arbitrary Precision](#-812-numeric--arbitrary-precision)
- [8.1.3 Floating-Point Types](#-813-floating-point-types)
- [8.1.4 Serial Types](#-814-serial-types)
- [8.3 Character Types](#-83-character-types)
- [8.5 Date/Time Types](#-85-datetime-types)
- [8.6 Boolean Type](#-86-boolean-type)
- [8.7 Enumerated (ENUM) Types](#-87-enumerated-enum-types)
- [Side-by-Side Cheat Sheet](#-side-by-side-cheat-sheet)
- [Quick Reference](#-quick-reference)

---

## 🗺️ The Big Picture

```
PostgreSQL Data Types
│
├── 🔢 NUMERIC
│    ├── Integer      → smallint, integer, bigint
│    ├── Exact        → numeric / decimal
│    ├── Approximate  → real, double precision
│    └── Auto-ID      → serial, bigserial
│
├── 🔤 CHARACTER
│    ├── Fixed length → char(n)
│    ├── Variable     → varchar(n)
│    └── Unlimited    → text  ← PostgreSQL native, preferred
│
├── 📅 DATE/TIME
│    ├── Date only    → date
│    ├── Time only    → time
│    ├── Both         → timestamp, timestamptz
│    └── Duration     → interval
│
├── ✅ BOOLEAN      → true / false / null
│
└── 🏷️  ENUM         → custom ordered set of labels
```

---

## 🔢 8.1.1 Integer Types

Integers store **whole numbers** — no decimal point, no fractions.

| Type | Alias | Storage | Range | Use When |
|------|-------|---------|-------|----------|
| `smallint` | `int2` | 2 bytes | -32,768 to 32,767 | Disk space is critical |
| `integer` | `int`, `int4` | 4 bytes | -2,147,483,648 to 2,147,483,647 | ✅ Default choice |
| `bigint` | `int8` | 8 bytes | -9.2 × 10¹⁸ to 9.2 × 10¹⁸ | Very large numbers |

### Applied to student.stud_dtl

```sql
CREATE TABLE student.stud_dtl (
    id      integer  GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    --      ↑ integer: perfect for row IDs — most tables will never exceed 2 billion rows

    name    text,
    age     integer,
    --      ↑ integer: age is always a whole number, range is fine

    uni_id  integer
    --      ↑ integer: university IDs won't exceed 2 billion
);
```

> 💡 **Best practice:** Use `integer` for almost everything. Reach for `bigint` only when you genuinely expect to exceed ~2 billion values (e.g., event logs, analytics tables).

---

## 🎯 8.1.2 Numeric — Arbitrary Precision

`numeric` (also called `decimal`) stores numbers with **exact precision** — no rounding errors. It's slower than integer or float, but **essential for money**.

### Syntax

```sql
NUMERIC(precision, scale)
--      ↑ total digits  ↑ digits after decimal point

-- Example: 23.5141
--   precision = 6  (total significant digits)
--   scale     = 4  (digits after decimal point)
```

### Common Declarations

```sql
-- Fee with 2 decimal places: 9999.99 max
fee  NUMERIC(6, 2)

-- Exact integer (scale = 0):
score  NUMERIC(5)

-- Unconstrained — any size (use with care):
amount  NUMERIC
```

### Applied to student.stud_dtl — Adding a Fee Column

```sql
ALTER TABLE student.stud_dtl ADD COLUMN tuition_fee NUMERIC(8, 2);
-- Stores fees like 12500.50 — up to 999999.99

INSERT INTO student.stud_dtl (name, age, uni_id, tuition_fee)
VALUES ('Nitin', 20, 101, 12500.50);

SELECT name, tuition_fee FROM student.stud_dtl;
```

```
name  │ tuition_fee
──────┼────────────
Nitin │   12500.50   ← exact — no floating-point drift
```

<details>
<summary>📌 <strong>Click to expand: NUMERIC precision and scale — edge cases</strong></summary>

```sql
-- NUMERIC(3, 1) → stores -99.9 to 99.9, rounds to 1 decimal place
-- NUMERIC(2, -3) → rounds to nearest thousand: -99000 to 99000
-- NUMERIC(3, 5) → only fractional values: -0.00999 to 0.00999

-- Special values (must be quoted):
UPDATE student.stud_dtl SET tuition_fee = 'Infinity'  WHERE id = 1;
UPDATE student.stud_dtl SET tuition_fee = '-Infinity' WHERE id = 2;
UPDATE student.stud_dtl SET tuition_fee = 'NaN'       WHERE id = 3;
-- NaN = "not a number" — result of undefined calculations
-- Infinity only works in unconstrained NUMERIC columns
```

**Rounding difference — NUMERIC vs double precision:**

```sql
SELECT x,
    round(x::numeric)          AS numeric_round,
    round(x::double precision) AS float_round
FROM generate_series(-3.5, 3.5, 1) AS x;
```

```
  x   │ numeric_round │ float_round
──────┼───────────────┼─────────────
 -3.5 │      -4       │     -4
 -2.5 │      -3       │     -2    ← float rounds to nearest EVEN
 -0.5 │      -1       │     -0    ← float rounds to nearest EVEN
  0.5 │       1       │      0    ← float rounds to nearest EVEN
  2.5 │       3       │      2    ← float rounds to nearest EVEN
```

> NUMERIC rounds **away from zero** (standard rounding). Floating-point rounds to **nearest even** — different behaviour for .5 values!

</details>

---

## 〜 8.1.3 Floating-Point Types

`real` and `double precision` are **fast but inexact** — they store approximations, not exact values.

| Type | Alias | Storage | Precision | Range |
|------|-------|---------|-----------|-------|
| `real` | `float4` | 4 bytes | ~6 decimal digits | 1E-37 to 1E+37 |
| `double precision` | `float8`, `float` | 8 bytes | ~15 decimal digits | 1E-307 to 1E+308 |

### The Key Warning

```
NUMERIC:           Exact. Always use for money.     Slow.
double precision:  Approximate. Fast.               Never use for money.

-- This can happen with floating-point:
SELECT 0.1::double precision + 0.2::double precision = 0.3::double precision;
-- Returns: FALSE  ← 0.1 + 0.2 ≠ 0.3 in floating-point arithmetic!

SELECT 0.1::numeric + 0.2::numeric = 0.3::numeric;
-- Returns: TRUE  ← numeric is always exact
```

<details>
<summary>📌 <strong>Click to expand: float(p) syntax and special values</strong></summary>

```sql
-- SQL-standard float(p) syntax:
-- float(1) to float(24)  → selects real (4 bytes)
-- float(25) to float(53) → selects double precision (8 bytes)
-- float (no p)           → means double precision

-- Special values (same as numeric, must be quoted):
UPDATE student.stud_dtl SET score = 'Infinity';
UPDATE student.stud_dtl SET score = 'NaN';
-- NaN in IEEE 754 is NOT equal to itself; PostgreSQL overrides this
-- so NaN = NaN for sorting and indexing purposes
```

</details>

---

## 🔑 8.1.4 Serial Types

`SERIAL` is **not a real data type** — it's a shorthand that creates an auto-incrementing integer column backed by a sequence.

### What SERIAL Actually Does

```sql
-- You write:
CREATE TABLE student.stud_dtl (
    id  SERIAL
);

-- PostgreSQL creates:
CREATE SEQUENCE student_stud_dtl_id_seq AS integer;

CREATE TABLE student.stud_dtl (
    id  integer  NOT NULL  DEFAULT nextval('student_stud_dtl_id_seq')
);

ALTER SEQUENCE student_stud_dtl_id_seq OWNED BY student.stud_dtl.id;
```

### Three Sizes

| Type | Alias | Underlying Type | Range |
|------|-------|----------------|-------|
| `smallserial` | `serial2` | `smallint` | 1 to 32,767 |
| `serial` | `serial4` | `integer` | 1 to 2,147,483,647 |
| `bigserial` | `serial8` | `bigint` | 1 to 9.2 × 10¹⁸ |

### SERIAL vs IDENTITY — Which to Use?

```sql
-- Old way (SERIAL):
id  SERIAL  PRIMARY KEY

-- Modern way (IDENTITY) ← recommended:
id  integer  GENERATED ALWAYS AS IDENTITY  PRIMARY KEY
```

| Feature | SERIAL | IDENTITY |
|---------|--------|----------|
| SQL Standard | ❌ PostgreSQL-specific | ✅ Yes |
| NOT NULL automatic | ✅ Yes | ✅ Yes |
| Sequence gaps possible | ✅ Yes (rollbacks) | ✅ Yes (rollbacks) |
| Primary key automatic | ❌ Add separately | ❌ Add separately |
| Preferred for new code | ❌ Legacy | ✅ Yes |

> ⚠️ SERIAL sequences can have **gaps** — if an INSERT transaction rolls back, that sequence value is consumed but the row is never written. This is normal and expected behaviour.

---

## 🔤 8.3 Character Types

| Type | Description | Trailing Spaces |
|------|-------------|----------------|
| `char(n)` / `character(n)` | Fixed length — always pads to n with spaces | Insignificant (stripped on compare) |
| `varchar(n)` / `character varying(n)` | Variable length up to n characters | Significant |
| `text` | Unlimited length | Significant |

### Key Behaviour Differences

```sql
-- char(4) always stores 4 characters — pads with spaces:
CREATE TABLE test1 (a character(4));
INSERT INTO test1 VALUES ('ok');
SELECT a, char_length(a) FROM test1;
```

```
  a   │ char_length
──────┼─────────────
 ok   │      2        ← stored as 'ok  ' (padded), but char_length ignores padding
```

```sql
-- varchar(5) rejects strings longer than 5 characters:
CREATE TABLE test2 (b varchar(5));
INSERT INTO test2 VALUES ('ok');          -- ✅
INSERT INTO test2 VALUES ('good      ');  -- ✅ trailing spaces truncated to fit
INSERT INTO test2 VALUES ('too long');   -- ❌ ERROR: value too long
INSERT INTO test2 VALUES ('too long'::varchar(5));  -- ✅ explicit cast truncates silently
SELECT b, char_length(b) FROM test2;
```

```
   b   │ char_length
───────┼─────────────
 ok    │      2
 good  │      5
 too l │      5       ← truncated by explicit cast
```

### Applied to student.stud_dtl

```sql
-- Recommended column definitions using text:
CREATE TABLE student.stud_dtl (
    id      integer GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name    text,           -- ← unlimited, no arbitrary cap needed
    address text,           -- ← addresses vary too much for varchar(n)
    email   varchar(255),   -- ← reasonable cap for email addresses
    code    char(6)         -- ← fixed codes like 'IND001' benefit from char(n)
);
```

<details>
<summary>📌 <strong>Click to expand: Performance — text vs varchar vs char</strong></summary>

```
In PostgreSQL, all three types have nearly identical performance:
  text          → ✅ Fastest for most cases. No length check overhead.
  varchar(n)    → Tiny overhead to check length on each insert/update.
  char(n)       → Slowest — extra storage for padding + length check.

char(n) is NOT faster in PostgreSQL (unlike some other databases).

Recommendation:
  ✅ Use text         → when no upper limit needed (names, descriptions, notes)
  ✅ Use varchar(n)   → when you have a meaningful business limit (email, code)
  ⚠️ Avoid char(n)   → unless storing truly fixed-width codes
```

</details>

---

## 📅 8.5 Date/Time Types

### The Full Type Table

| Type | Storage | Description | Range |
|------|---------|-------------|-------|
| `date` | 4 bytes | Date only (no time) | 4713 BC → 5874897 AD |
| `time` | 8 bytes | Time only (no date) | 00:00:00 → 24:00:00 |
| `time with time zone` | 12 bytes | Time with timezone | 00:00:00+1559 → 24:00:00-1559 |
| `timestamp` | 8 bytes | Date + time (no timezone) | 4713 BC → 294276 AD |
| `timestamptz` | 8 bytes | Date + time + timezone | 4713 BC → 294276 AD |
| `interval` | 16 bytes | Duration / time span | -178M years → 178M years |

> 💡 `timestamptz` is shorthand for `timestamp with time zone` — a PostgreSQL extension.

---

### Applied to student.stud_dtl — Adding Date Columns

```sql
ALTER TABLE student.stud_dtl ADD COLUMN date_of_birth   date;
ALTER TABLE student.stud_dtl ADD COLUMN enrolled_at     timestamptz DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE student.stud_dtl ADD COLUMN course_duration interval;

INSERT INTO student.stud_dtl (name, age, uni_id, date_of_birth, course_duration)
VALUES
    ('Nitin',  20, 101, '2006-03-01',             '3 years 6 months'),
    ('Robert', 35, 102, '1990-07-15',             'P4Y'),
    ('Alice',  22, 101, '2004-11-20 00:00:00+00', '2 years');

SELECT name, date_of_birth, enrolled_at, course_duration
FROM student.stud_dtl;
```

```
name   │ date_of_birth │ enrolled_at                  │ course_duration
───────┼───────────────┼──────────────────────────────┼─────────────────
Nitin  │  2006-03-01   │ 2026-03-12 10:30:00+00       │ 3 years 6 mons
Robert │  1990-07-15   │ 2026-03-12 10:30:01+00       │ 4 years
Alice  │  2004-11-20   │ 2026-03-12 10:30:02+00       │ 2 years
```

---

### Date Input Formats

```sql
-- All of these represent January 8, 1999:
'1999-01-08'      -- ✅ ISO 8601 — recommended, unambiguous in all modes
'January 8, 1999' -- ✅ unambiguous in any datestyle mode
'1/8/1999'        -- ⚠️  ambiguous: Jan 8 in MDY, Aug 1 in DMY mode
'19990108'        -- ✅ ISO 8601 compact format
'1999-Jan-08'     -- ✅ unambiguous in any mode
```

```sql
-- Time input examples:
'04:05:06'          -- ISO 8601
'04:05:06.789-8'    -- with UTC offset
'04:05:06 PST'      -- with timezone abbreviation
'2003-04-12 04:05:06 America/New_York'  -- with full timezone name
```

```sql
-- Timestamp examples:
'1999-01-08 04:05:06'          -- timestamp without time zone
'1999-01-08 04:05:06 -8:00'    -- with UTC offset
TIMESTAMP WITH TIME ZONE '2004-10-19 10:23:54+02'  -- explicit typed literal
```

<details>
<summary>📌 <strong>Click to expand: Special date/time values</strong></summary>

| Input String | Valid Types | Description |
|---|---|---|
| `'epoch'` | date, timestamp | 1970-01-01 00:00:00+00 (Unix time zero) |
| `'infinity'` | date, timestamp, interval | Later than all timestamps |
| `'-infinity'` | date, timestamp, interval | Earlier than all timestamps |
| `'now'` | date, time, timestamp | Current transaction start time |
| `'today'` | date, timestamp | Midnight today |
| `'tomorrow'` | date, timestamp | Midnight tomorrow |
| `'yesterday'` | date, timestamp | Midnight yesterday |

```sql
-- Safe SQL functions for current time (use these in views/functions):
SELECT CURRENT_DATE;        -- today's date
SELECT CURRENT_TIME;        -- current time with timezone
SELECT CURRENT_TIMESTAMP;   -- current date + time with timezone
SELECT LOCALTIMESTAMP;      -- current date + time without timezone

-- ⚠️ Avoid 'now', 'today', 'tomorrow' in prepared statements or views —
-- they get evaluated ONCE at definition time, not at execution time!
-- Use CURRENT_DATE + 1 instead of 'tomorrow'::date
```

</details>

<details>
<summary>📌 <strong>Click to expand: Time Zones — stored as UTC, displayed as local</strong></summary>

```sql
-- All timestamptz values are stored internally as UTC:
INSERT INTO student.stud_dtl (name, age, enrolled_at)
VALUES ('Nitin', 20, '2026-03-12 10:00:00 America/New_York');
-- Stored as: 2026-03-12 15:00:00 UTC  (New York is UTC-5)
-- Displayed as: 2026-03-12 15:00:00+00  (or converted to your session timezone)

-- Change display timezone for your session:
SET TIME ZONE 'America/New_York';
SELECT enrolled_at FROM student.stud_dtl WHERE name = 'Nitin';
-- Shows: 2026-03-12 10:00:00-05  ← converted back to New York time

-- Three ways to specify a timezone:
-- 1. Full name:    'America/New_York'  ← recommended (handles DST automatically)
-- 2. Abbreviation: 'EST'              ← fixed UTC offset, no DST awareness
-- 3. POSIX style:  'EST5EDT'          ← avoid unless necessary
```

</details>

<details>
<summary>📌 <strong>Click to expand: Interval — representing durations</strong></summary>

```sql
-- Verbose syntax:
'1 year 2 months 3 days 4 hours 5 minutes 6 seconds'::interval

-- SQL standard shorthand:
'1-2'::interval     -- 1 year 2 months
'3 4:05:06'::interval  -- 3 days 4 hours 5 mins 6 secs

-- ISO 8601 format:
'P1Y2M3DT4H5M6S'::interval  -- same as above

-- Interval arithmetic with student data:
SELECT name,
       date_of_birth,
       AGE(date_of_birth)              AS age_today,
       date_of_birth + INTERVAL '18 years' AS turns_18
FROM student.stud_dtl;

-- Interval output styles:
SET intervalstyle = 'postgres';         -- default: '1 year 2 mons'
SET intervalstyle = 'sql_standard';     -- '1-2'
SET intervalstyle = 'iso_8601';         -- 'P1Y2M'
SET intervalstyle = 'postgres_verbose'; -- '@ 1 year 2 mons'
```

</details>

---

## ✅ 8.6 Boolean Type

```
Storage: 1 byte
States:  true | false | null (unknown)
```

### Accepted Input Values

| Represents | Accepted Strings |
|-----------|-----------------|
| `TRUE` | `true`, `yes`, `on`, `1`, `t`, `y` |
| `FALSE` | `false`, `no`, `off`, `0`, `f`, `n` |

> Case-insensitive. Leading/trailing whitespace ignored.

### Applied to student.stud_dtl

```sql
ALTER TABLE student.stud_dtl ADD COLUMN is_active boolean DEFAULT TRUE;
ALTER TABLE student.stud_dtl ADD COLUMN has_scholarship boolean;

INSERT INTO student.stud_dtl (name, age, uni_id, is_active, has_scholarship)
VALUES
    ('Nitin',  20, 101, TRUE,  TRUE),
    ('Robert', 35, 102, TRUE,  FALSE),
    ('Alice',  22, 101, FALSE, NULL);   -- NULL = unknown

SELECT name, is_active, has_scholarship FROM student.stud_dtl;
```

```
name   │ is_active │ has_scholarship
───────┼───────────┼─────────────────
Nitin  │     t     │        t
Robert │     t     │        f         ← output is always t or f
Alice  │     f     │      (null)      ← NULL = unknown/not set
```

```sql
-- Filter using boolean columns directly (no = TRUE needed):
SELECT name FROM student.stud_dtl WHERE is_active;          -- active students
SELECT name FROM student.stud_dtl WHERE NOT is_active;      -- inactive students
SELECT name FROM student.stud_dtl WHERE has_scholarship IS NULL;  -- unknown
```

---

## 🏷️ 8.7 Enumerated (ENUM) Types

An **ENUM** type is a custom data type with a **fixed, ordered set of labels**. Once defined, only those exact values can be stored.

### Declaring an ENUM

```sql
-- Step 1: Create the type
CREATE TYPE student_status AS ENUM ('enrolled', 'on_leave', 'graduated', 'dropped');

-- Order matters: 'enrolled' < 'on_leave' < 'graduated' < 'dropped'
```

### Applied to student.stud_dtl

```sql
-- Step 2: Use it as a column type
ALTER TABLE student.stud_dtl ADD COLUMN status student_status DEFAULT 'enrolled';

-- Step 3: Insert valid values
UPDATE student.stud_dtl SET status = 'enrolled'  WHERE id = 1;
UPDATE student.stud_dtl SET status = 'graduated' WHERE id = 2;

-- Step 4: Invalid value is rejected:
UPDATE student.stud_dtl SET status = 'suspended' WHERE id = 3;
-- ERROR: invalid input value for enum student_status: "suspended"

SELECT name, status FROM student.stud_dtl;
```

```
name   │ status
───────┼───────────
Nitin  │ enrolled
Robert │ graduated
Alice  │ enrolled
```

---

### ENUM Ordering

ENUMs sort in **declaration order**, not alphabetically:

```sql
-- 'enrolled' < 'on_leave' < 'graduated' < 'dropped'

SELECT name, status
FROM student.stud_dtl
WHERE status > 'enrolled'      -- returns on_leave, graduated, dropped
ORDER BY status;               -- sorted by declaration order
```

```
name   │ status
───────┼───────────
Robert │ graduated   ← 'graduated' > 'enrolled' in declaration order
```

---

### ENUM Type Safety

Each ENUM type is **completely isolated** — two ENUM types cannot be compared even if they share label values:

```sql
CREATE TYPE grade_level AS ENUM ('freshman', 'sophomore', 'junior', 'senior');
CREATE TYPE employment_level AS ENUM ('junior', 'mid', 'senior');

-- These two columns cannot be compared directly:
-- student.grade = employee.level  ← ERROR: operator does not exist

-- Workaround — cast both to text:
WHERE student.grade::text = employee.level::text
```

<details>
<summary>📌 <strong>Click to expand: ENUM implementation details & ALTER TYPE</strong></summary>

```sql
-- Adding new values to an existing ENUM:
ALTER TYPE student_status ADD VALUE 'suspended' AFTER 'on_leave';
-- New order: enrolled < on_leave < suspended < graduated < dropped

-- Renaming a value:
ALTER TYPE student_status RENAME VALUE 'dropped' TO 'withdrawn';

-- You CANNOT:
-- ❌ Remove values from an ENUM
-- ❌ Change the sort order (must DROP and recreate the type)
-- ❌ Compare two different ENUM types directly

-- Each enum value occupies 4 bytes on disk.
-- Labels are case-sensitive: 'enrolled' ≠ 'Enrolled'
-- Max label length: 63 bytes

-- View all values in an ENUM:
SELECT enumlabel, enumsortorder
FROM pg_enum
JOIN pg_type ON pg_enum.enumtypid = pg_type.oid
WHERE pg_type.typname = 'student_status'
ORDER BY enumsortorder;
```

</details>

---

## ↔️ Side-by-Side Cheat Sheet

| Category | Type | Storage | Use For | Avoid For |
|----------|------|---------|---------|-----------|
| **Integer** | `smallint` | 2B | Tiny tables, lookup codes | Most cases |
| **Integer** | `integer` | 4B | ✅ General IDs, counts, ages | Values > 2 billion |
| **Integer** | `bigint` | 8B | High-volume event logs | Overkill for most |
| **Exact** | `numeric(p,s)` | Variable | ✅ Money, grades, fees | Speed-critical math |
| **Approx** | `real` | 4B | Scientific data, estimates | Money, exact values |
| **Approx** | `double precision` | 8B | Scientific data, coordinates | Money, exact values |
| **Auto-ID** | `serial` | 4B | Legacy auto-increment | New code (use IDENTITY) |
| **Auto-ID** | `bigserial` | 8B | Legacy high-volume auto-ID | New code (use IDENTITY) |
| **Text** | `text` | Variable | ✅ Names, descriptions | Fixed-width codes |
| **Text** | `varchar(n)` | Variable | Emails, codes with a limit | Unlimited fields |
| **Text** | `char(n)` | Fixed | Truly fixed codes | Most use cases |
| **Date** | `date` | 4B | ✅ Birthdays, enrollment dates | When time matters |
| **DateTime** | `timestamp` | 8B | Local datetime logging | Multi-timezone apps |
| **DateTime** | `timestamptz` | 8B | ✅ Most datetime needs | Local-only apps |
| **Duration** | `interval` | 16B | ✅ Course durations, age diff | — |
| **Boolean** | `boolean` | 1B | ✅ Flags: active, verified | Multi-state status |
| **Enum** | custom | 4B | ✅ Status, grade, category | Frequently changing sets |

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: student.stud_dtl with all data types applied</strong></summary>

```sql
DROP SCHEMA IF EXISTS student CASCADE;
CREATE SCHEMA student;

-- Custom ENUM type for student status:
CREATE TYPE student_status AS ENUM ('enrolled', 'on_leave', 'graduated', 'dropped');

CREATE TABLE student.stud_dtl (
    -- Integer + Identity
    id               integer       GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    -- Character types
    name             text          NOT NULL,                          -- unlimited text
    email            varchar(255),                                    -- capped text
    student_code     char(8),                                         -- fixed code e.g. 'STU00001'

    -- Integer
    age              integer       CONSTRAINT age_check CHECK (age >= 18),
    uni_id           integer,

    -- Exact numeric (money)
    tuition_fee      numeric(10, 2),                                  -- e.g. 12500.50

    -- Boolean flags
    is_active        boolean       DEFAULT TRUE,
    has_scholarship  boolean,

    -- Date/Time
    date_of_birth    date,
    enrolled_at      timestamptz   DEFAULT CURRENT_TIMESTAMP,
    course_duration  interval,

    -- ENUM
    status           student_status DEFAULT 'enrolled'
);

-- Insert a complete row:
INSERT INTO student.stud_dtl
    (name, email, student_code, age, uni_id, tuition_fee,
     is_active, has_scholarship, date_of_birth, course_duration, status)
VALUES
    ('Nitin', 'nitin@example.com', 'STU00001', 20, 101, 12500.50,
     TRUE, TRUE, '2006-03-01', '3 years 6 months', 'enrolled'),
    ('Robert', 'robert@example.com', 'STU00002', 35, 102, 8750.00,
     TRUE, FALSE, '1990-07-15', 'P4Y', 'graduated');

SELECT * FROM student.stud_dtl;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Data type** | Defines what kind of value a column can store and how it's stored |
| **integer** | Whole number, 4 bytes, range ±2.1 billion — the general-purpose default |
| **bigint** | Whole number, 8 bytes — for very large counts |
| **numeric(p,s)** | Exact decimal number with precision p (total digits) and scale s (decimal places) |
| **precision** | Total number of significant digits in a numeric value |
| **scale** | Number of digits after the decimal point |
| **real** | Approximate 4-byte floating-point number (~6 sig. digits) |
| **double precision** | Approximate 8-byte floating-point number (~15 sig. digits) |
| **SERIAL** | PostgreSQL shorthand for auto-incrementing integer using a sequence |
| **sequence** | Database object generating successive integers |
| **text** | PostgreSQL's native string type — unlimited length, preferred over char/varchar |
| **varchar(n)** | Variable-length string with an upper limit of n characters |
| **char(n)** | Fixed-length string, always padded to n characters with spaces |
| **date** | Calendar date only, no time component |
| **timestamp** | Date and time, no timezone |
| **timestamptz** | Date and time stored as UTC, displayed in session timezone |
| **interval** | A duration or time span |
| **boolean** | Single byte: true, false, or null (unknown) |
| **ENUM** | Custom type with a fixed, ordered set of string labels |
| **NaN** | Not a Number — result of undefined numeric operations |
| **UTC** | Coordinated Universal Time — internal storage format for timestamptz |

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
