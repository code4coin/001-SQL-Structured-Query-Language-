# 🔒 Constraints — PostgreSQL

> *Your database's bodyguards — enforcing rules so bad data never gets in.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Constraints-EF4444?style=for-the-badge)
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

- [Why Constraints?](#-why-constraints)
- [5.5.1 CHECK Constraints](#-551-check-constraints)
- [5.5.2 NOT NULL Constraints](#-552-not-null-constraints)
- [5.5.3 UNIQUE Constraints](#-553-unique-constraints)
- [5.5.4 PRIMARY KEY](#-554-primary-key)
- [5.5.5 FOREIGN KEY](#-555-foreign-key)
- [5.5.6 EXCLUSION Constraints](#-556-exclusion-constraints)
- [Side-by-Side Comparison](#-side-by-side-comparison)
- [Quick Reference](#-quick-reference)

---

## 🛡️ Why Constraints?

Data types alone aren't enough. A column typed `numeric` will accept `-999` for a product price — but that's clearly wrong. Constraints are the next layer of defence.

```
Layer 1 — Data Type     → "must be a number"
Layer 2 — Constraint    → "must be a POSITIVE number"
Layer 3 — App Logic     → additional business rules
```

```
Without constraints:                With constraints:
──────────────────────              ──────────────────────────────
INSERT price = -50  ✅ allowed      INSERT price = -50  ❌ rejected
INSERT price = NULL ✅ allowed      INSERT price = NULL ❌ rejected
Duplicate product   ✅ allowed      Duplicate product   ❌ rejected
Orphan order        ✅ allowed      Orphan order        ❌ rejected
```

> 💡 If a user attempts to store data that violates a constraint, PostgreSQL raises an **error immediately** — even if the value came from a default.

---

## ✅ 5.5.1 CHECK Constraints

A **CHECK constraint** lets you define any Boolean expression a column value must satisfy.

### Basic Syntax

```sql
-- Column constraint style:
CREATE TABLE products (
    product_no  integer,
    name        text,
    price       numeric CHECK (price > 0)   -- ← must be positive
);
```

---

### Named CHECK Constraints

Give your constraints a name for clearer error messages and easier modification later:

```sql
CREATE TABLE products (
    product_no  integer,
    name        text,
    price       numeric CONSTRAINT positive_price CHECK (price > 0)
    --                  ↑ CONSTRAINT keyword     ↑ your chosen name
);
```

> 💡 Without a name, PostgreSQL auto-generates one like `products_price_check` — readable but less precise.

---

### Multi-Column CHECK — Table Constraint

When a rule involves **more than one column**, write it as a **table constraint** (separate from any column):

```sql
CREATE TABLE products (
    product_no        integer,
    name              text,
    price             numeric CHECK (price > 0),              -- column constraint
    discounted_price  numeric CHECK (discounted_price > 0),  -- column constraint
    CHECK (price > discounted_price)                          -- table constraint ← spans both columns
);
```

**Three equivalent ways to write the same thing:**

<details>
<summary>📌 <strong>Click to expand: All 3 syntax styles compared</strong></summary>

```sql
-- Style 1: Mixed (column + table constraints)
CREATE TABLE products (
    product_no        integer,
    name              text,
    price             numeric CHECK (price > 0),
    discounted_price  numeric CHECK (discounted_price > 0),
    CHECK (price > discounted_price)
);

-- Style 2: All as table constraints
CREATE TABLE products (
    product_no        integer,
    name              text,
    price             numeric,
    CHECK (price > 0),
    discounted_price  numeric,
    CHECK (discounted_price > 0),
    CHECK (price > discounted_price)
);

-- Style 3: Combined into fewer constraints
CREATE TABLE products (
    product_no        integer,
    name              text,
    price             numeric CHECK (price > 0),
    discounted_price  numeric,
    CHECK (discounted_price > 0 AND price > discounted_price)
);

-- Style 4: Named table constraint
CREATE TABLE products (
    product_no        integer,
    name              text,
    price             numeric CHECK (price > 0),
    discounted_price  numeric CHECK (discounted_price > 0),
    CONSTRAINT valid_discount CHECK (price > discounted_price)
);
```

All four are functionally identical — it's a matter of style.

</details>

---

### ⚠️ CHECK and NULL — An Important Gotcha

```sql
-- A CHECK passes if the expression is TRUE or NULL
-- It only fails if the expression is FALSE

INSERT INTO products VALUES (1, 'Widget', NULL);
-- price CHECK (price > 0) → NULL > 0 → NULL → ✅ PASSES (not FALSE)
-- The row is inserted even though price is NULL!
```

```
CHECK expression result:
  TRUE  → ✅ row accepted
  NULL  → ✅ row accepted  ← surprising!
  FALSE → ❌ row rejected
```

> ⚠️ To prevent NULLs, you must **also** add a `NOT NULL` constraint (section 5.5.2).

<details>
<summary>📌 <strong>Click to expand: PostgreSQL CHECK constraint limitations</strong></summary>

```
❌ CHECK constraints CANNOT reference other rows or tables:
   CHECK (price < (SELECT MAX(price) FROM products))  -- NOT supported

   Why? PostgreSQL cannot guarantee consistency across rows in a dump/restore.

✅ Use these instead for cross-row/cross-table rules:
   • UNIQUE       → for uniqueness across rows
   • FOREIGN KEY  → for referential integrity across tables
   • EXCLUDE      → for overlap/range rules
   • TRIGGER      → for one-time checks at insert time (not maintained)

⚠️ CHECK expressions are assumed IMMUTABLE:
   If you use a user-defined function in CHECK and later change the function,
   PostgreSQL won't recheck existing rows — causing dump/restore failures.
   Fix: DROP constraint → update function → re-ADD constraint.
```

</details>

---

## 🚫 5.5.2 NOT NULL Constraints

A **NOT NULL constraint** ensures a column can never hold a NULL value.

### Syntax

```sql
CREATE TABLE products (
    product_no  integer  NOT NULL,   -- ← cannot be NULL
    name        text     NOT NULL,   -- ← cannot be NULL
    price       numeric              -- ← can be NULL (no constraint)
);
```

---

### Named NOT NULL

```sql
CREATE TABLE products (
    product_no  integer  NOT NULL,
    name        text     CONSTRAINT products_name_not_null NOT NULL,
    price       numeric
);
```

---

### Combining Multiple Constraints

Constraints stack — just write them one after another in any order:

```sql
CREATE TABLE products (
    product_no  integer  NOT NULL,
    name        text     NOT NULL,
    price       numeric  NOT NULL  CHECK (price > 0)
    --                   ↑          ↑
    --              NOT NULL    AND must be positive
);
```

<details>
<summary>📌 <strong>Click to expand: NOT NULL vs CHECK(col IS NOT NULL) — which to use?</strong></summary>

```sql
-- These two are functionally equivalent:
price  numeric  NOT NULL
price  numeric  CHECK (price IS NOT NULL)

-- But NOT NULL is:
--   ✅ More efficient in PostgreSQL (optimized internally)
--   ✅ More readable and conventional
--   ✅ The standard approach for all production tables

-- The NULL constraint (inverse of NOT NULL):
price  numeric  NULL
-- This simply restates the default behaviour (column may be NULL).
-- Not in the SQL standard — avoid in portable applications.
```

> 💡 **Best practice:** Mark the **majority of columns** as `NOT NULL` in your table designs. Use NULL only when "no value" is genuinely meaningful for that field.

</details>

---

## 🔑 5.5.3 UNIQUE Constraints

A **UNIQUE constraint** ensures no two rows share the same value in the specified column(s).

### Column-Level Syntax

```sql
CREATE TABLE products (
    product_no  integer  UNIQUE,   -- ← no two rows can share a product_no
    name        text,
    price       numeric
);
```

### Table-Level Syntax

```sql
CREATE TABLE products (
    product_no  integer,
    name        text,
    price       numeric,
    UNIQUE (product_no)            -- ← same effect, written as table constraint
);
```

---

### Composite UNIQUE — Multiple Columns Together

```sql
CREATE TABLE example (
    a  integer,
    b  integer,
    c  integer,
    UNIQUE (a, c)    -- ← the COMBINATION of a+c must be unique, not each individually
);
```

```
Valid rows:              Invalid rows:
a=1, c=1  ✅            a=1, c=1 twice  ❌  (same a+c combination)
a=1, c=2  ✅
a=2, c=1  ✅            But a=1 alone can repeat — the constraint is on the pair
```

---

### Named UNIQUE Constraint

```sql
CREATE TABLE products (
    product_no  integer  CONSTRAINT must_be_different UNIQUE,
    name        text,
    price       numeric
);
```

<details>
<summary>📌 <strong>Click to expand: UNIQUE and NULL behaviour</strong></summary>

```sql
-- By default, two NULLs are NOT considered equal in UNIQUE:
INSERT INTO products (product_no) VALUES (NULL);
INSERT INTO products (product_no) VALUES (NULL);
-- ✅ Both rows accepted! Two NULLs are allowed even with UNIQUE

-- To treat NULLs as equal (prevent duplicate NULLs), use NULLS NOT DISTINCT:
CREATE TABLE products (
    product_no  integer  UNIQUE NULLS NOT DISTINCT,
    name        text,
    price       numeric
);

-- Now:
INSERT INTO products (product_no) VALUES (NULL);
INSERT INTO products (product_no) VALUES (NULL);
-- ❌ Second insert fails — NULLs are now treated as equal
```

> ⚠️ NULL behaviour in UNIQUE constraints varies across database systems — be careful in portable applications.

</details>

> 💡 Adding a UNIQUE constraint **automatically creates a B-tree index** on those columns — giving you a performance boost on lookups for free.

---

## 🗝️ 5.5.4 Primary Key

A **PRIMARY KEY** is the unique identifier for every row in a table. It combines `UNIQUE` + `NOT NULL` in one declaration.

### The Equivalence

```sql
-- These two are functionally identical:
CREATE TABLE products (
    product_no  integer  UNIQUE NOT NULL,   -- manual combination
    name        text,
    price       numeric
);

CREATE TABLE products (
    product_no  integer  PRIMARY KEY,       -- ← cleaner, conventional
    name        text,
    price       numeric
);
```

---

### Composite Primary Key

```sql
CREATE TABLE order_items (
    product_no  integer,
    order_id    integer,
    quantity    integer,
    PRIMARY KEY (product_no, order_id)   -- ← both columns together form the PK
);
```

---

### Key Rules About Primary Keys

| Rule | Detail |
|------|--------|
| **UNIQUE + NOT NULL** | PK values must be unique and can never be NULL |
| **One per table** | A table can have **at most one** PRIMARY KEY |
| **Auto-index** | PostgreSQL automatically creates a B-tree index on PK columns |
| **FK target** | The PK is the default target for FOREIGN KEY references |
| **Best practice** | Every table should have a primary key |

<details>
<summary>📌 <strong>Click to expand: PRIMARY KEY with IDENTITY — the modern pattern</strong></summary>

```sql
-- Most common modern pattern for a primary key:
CREATE TABLE student_personal.university_detail (
    university_id      integer  GENERATED ALWAYS AS IDENTITY  PRIMARY KEY,
    university_name    text     NOT NULL,
    university_address text     NOT NULL
);

-- This gives you:
--   ✅ Auto-incrementing ID (identity column)
--   ✅ Unique values enforced (PRIMARY KEY)
--   ✅ NOT NULL enforced (PRIMARY KEY implies NOT NULL)
--   ✅ Automatic B-tree index for fast lookups
```

</details>

---

## 🔗 5.5.5 Foreign Key

A **FOREIGN KEY** ensures that values in one table's column match values in another table — maintaining **referential integrity** between related tables.

### Basic Example

```sql
-- Referenced table (must exist first):
CREATE TABLE products (
    product_no  integer  PRIMARY KEY,
    name        text,
    price       numeric
);

-- Referencing table (points to products):
CREATE TABLE orders (
    order_id    integer  PRIMARY KEY,
    product_no  integer  REFERENCES products (product_no),  -- ← FK
    quantity    integer
);
```

```
products table          orders table
──────────────          ──────────────────────────
product_no (PK)  ◄───── product_no (FK)
name                    order_id
price                   quantity

Any value in orders.product_no MUST exist in products.product_no
```

> 💡 You can shorten `REFERENCES products (product_no)` to just `REFERENCES products` — PostgreSQL automatically uses the referenced table's primary key.

---

### Many-to-Many with a Junction Table

```sql
CREATE TABLE products (
    product_no  integer  PRIMARY KEY,
    name        text,
    price       numeric
);

CREATE TABLE orders (
    order_id          integer  PRIMARY KEY,
    shipping_address  text
);

-- Junction table — one order can have many products, one product in many orders:
CREATE TABLE order_items (
    product_no  integer  REFERENCES products,   -- ← FK to products
    order_id    integer  REFERENCES orders,     -- ← FK to orders
    quantity    integer,
    PRIMARY KEY (product_no, order_id)          -- ← composite PK
);
```

---

### ON DELETE Actions — What Happens When the Referenced Row is Deleted?

```sql
CREATE TABLE order_items (
    product_no  integer  REFERENCES products  ON DELETE RESTRICT,
    --                                         ↑ block the delete if referenced
    order_id    integer  REFERENCES orders    ON DELETE CASCADE,
    --                                         ↑ auto-delete this row too
    quantity    integer,
    PRIMARY KEY (product_no, order_id)
);
```

| Action | Behaviour | Use When |
|--------|-----------|----------|
| `NO ACTION` | Allow delete to proceed; error if FK still violated at end of transaction | Default — flexible, deferred checking |
| `RESTRICT` | **Block** the delete immediately — stricter than NO ACTION | Products that must never be orphaned |
| `CASCADE` | **Auto-delete** all referencing rows | Child records that belong to a parent (e.g. order items) |
| `SET NULL` | Set FK column(s) to NULL in referencing rows | Optional relationships (e.g. optional manager) |
| `SET DEFAULT` | Set FK column(s) to their default value | Reassign to a default record |

<details>
<summary>📌 <strong>Click to expand: ON DELETE in plain English with examples</strong></summary>

```
Scenario: We try to DELETE product_no = 5

ON DELETE NO ACTION:
  → Allowed IF no order_items reference product 5 by end of transaction
  → Can use other commands first to "fix" it, then the check runs
  → Error if still violated at commit time

ON DELETE RESTRICT:
  → Immediately blocked if any order_item references product 5
  → Cannot be deferred — checked right now

ON DELETE CASCADE:
  → Product 5 deleted ✅
  → All order_items with product_no=5 auto-deleted too ✅
  → Useful: orders own their items, so deleting an order deletes its items

ON DELETE SET NULL:
  → Product 5 deleted ✅
  → order_items.product_no set to NULL for affected rows
  → Use when the relationship is optional

ON DELETE SET DEFAULT:
  → Product 5 deleted ✅
  → order_items.product_no set to its DEFAULT value for affected rows
```

</details>

<details>
<summary>📌 <strong>Click to expand: ON UPDATE actions</strong></summary>

```sql
-- ON UPDATE works the same way — triggered when a referenced column VALUE changes

CREATE TABLE order_items (
    product_no  integer  REFERENCES products  ON UPDATE CASCADE,
    --                                         ↑ if product_no changes in products,
    --                                           update it here too automatically
    order_id    integer  REFERENCES orders    ON DELETE CASCADE,
    quantity    integer,
    PRIMARY KEY (product_no, order_id)
);
```

| ON UPDATE Action | Behaviour |
|-----------------|-----------|
| `NO ACTION` | Allow update; check FK at end of transaction |
| `RESTRICT` | Block update immediately |
| `CASCADE` | **Copy** new value into all referencing rows |
| `SET NULL` | Set referencing column(s) to NULL |
| `SET DEFAULT` | Set referencing column(s) to default |

</details>

<details>
<summary>📌 <strong>Click to expand: Self-referential foreign key — tree structures</strong></summary>

```sql
-- A table that references itself — useful for hierarchies (org charts, categories, etc.)
CREATE TABLE categories (
    node_id    integer  PRIMARY KEY,
    parent_id  integer  REFERENCES categories,   -- ← points to same table!
    name       text
);

-- Top-level nodes have NULL parent_id:
INSERT INTO categories VALUES (1, NULL, 'Electronics');    -- root node
INSERT INTO categories VALUES (2, 1,    'Phones');         -- child of Electronics
INSERT INTO categories VALUES (3, 1,    'Laptops');        -- child of Electronics
INSERT INTO categories VALUES (4, 2,    'Smartphones');    -- child of Phones

-- Structure:
-- Electronics (1)
--   ├── Phones (2)
--   │    └── Smartphones (4)
--   └── Laptops (3)
```

</details>

---

## 🚧 5.5.6 Exclusion Constraints

An **EXCLUSION constraint** ensures that for any two rows, at least one specified comparison between them returns false or null — preventing overlaps or conflicts that UNIQUE alone can't handle.

### Syntax

```sql
CREATE TABLE circles (
    c  circle,
    EXCLUDE USING gist (c WITH &&)
    --             ↑ index type   ↑ operator (overlaps)
);
-- No two circles in this table are allowed to overlap
```

<details>
<summary>📌 <strong>Click to expand: When to use EXCLUSION constraints</strong></summary>

```
UNIQUE handles: "no two rows have the same value"
EXCLUSION handles: "no two rows satisfy some comparison"

Real-world use cases:
  🏨 Hotel bookings   → no two bookings for the same room overlap in time
  🗓️ Calendar events  → no two events occupy the same time slot for the same resource
  🗺️  Map regions      → no two geographic regions overlap

-- Example: No overlapping time bookings for a room:
CREATE TABLE room_bookings (
    room_id  integer,
    during   tsrange,    -- time range
    EXCLUDE USING gist (room_id WITH =, during WITH &&)
    -- room_id must be equal AND during must overlap = violation
);
```

> 💡 EXCLUSION constraints automatically create an index of the specified type (usually GiST) on the constrained columns.

</details>

---

## ↔️ Side-by-Side Comparison

| Constraint | What It Enforces | NULL Allowed? | Spans Multiple Columns? | Auto-creates Index? |
|------------|-----------------|:---:|:---:|:---:|
| **CHECK** | Any Boolean expression | ⚠️ NULLs pass! | ✅ Yes | ❌ No |
| **NOT NULL** | Column must have a value | ❌ Never | ❌ One column only | ❌ No |
| **UNIQUE** | No duplicate values | ⚠️ Multiple NULLs allowed by default | ✅ Yes | ✅ B-tree |
| **PRIMARY KEY** | Unique identifier for each row | ❌ Never | ✅ Yes (composite PK) | ✅ B-tree |
| **FOREIGN KEY** | Value must exist in referenced table | ✅ NULL skips the check | ✅ Yes | ❌ No (add manually) |
| **EXCLUSION** | No two rows satisfy operator comparison | ✅ Nulls return null | ✅ Yes | ✅ GiST (or specified) |

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: All constraint syntax at a glance</strong></summary>

```sql
-- ── CHECK ──────────────────────────────────────────────────────
price  numeric  CHECK (price > 0)
price  numeric  CONSTRAINT positive_price CHECK (price > 0)
CHECK (price > discounted_price)                          -- table constraint

-- ── NOT NULL ───────────────────────────────────────────────────
name  text  NOT NULL
name  text  CONSTRAINT name_required NOT NULL

-- ── UNIQUE ─────────────────────────────────────────────────────
product_no  integer  UNIQUE
product_no  integer  UNIQUE NULLS NOT DISTINCT            -- treat NULLs as equal
UNIQUE (a, c)                                             -- composite unique
CONSTRAINT must_be_different UNIQUE (product_no)          -- named

-- ── PRIMARY KEY ────────────────────────────────────────────────
product_no  integer  PRIMARY KEY                          -- single column
PRIMARY KEY (a, c)                                        -- composite

-- ── FOREIGN KEY ────────────────────────────────────────────────
product_no  integer  REFERENCES products                  -- uses PK of products
product_no  integer  REFERENCES products (product_no)     -- explicit column
product_no  integer  REFERENCES products  ON DELETE CASCADE
product_no  integer  REFERENCES products  ON DELETE RESTRICT
product_no  integer  REFERENCES products  ON DELETE SET NULL
product_no  integer  REFERENCES products  ON UPDATE CASCADE
FOREIGN KEY (b, c) REFERENCES other_table (c1, c2)        -- composite FK

-- ── EXCLUSION ──────────────────────────────────────────────────
EXCLUDE USING gist (c WITH &&)
EXCLUDE USING gist (room_id WITH =, during WITH &&)
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Constraint** | A rule enforced by PostgreSQL on column or table data |
| **Column constraint** | A constraint attached directly to a single column definition |
| **Table constraint** | A constraint written separately — can span multiple columns |
| **CHECK** | Constraint requiring a Boolean expression to be true (or null) |
| **NOT NULL** | Constraint preventing NULL values in a column |
| **UNIQUE** | Constraint ensuring no duplicate values in a column or column group |
| **NULLS NOT DISTINCT** | Makes UNIQUE treat NULL values as equal |
| **PRIMARY KEY** | Unique + NOT NULL identifier for each row; one per table |
| **FOREIGN KEY** | Ensures a column's value exists in another table's column |
| **Referencing table** | The table that contains the foreign key column |
| **Referenced table** | The table being pointed to by the foreign key |
| **Referential integrity** | The guarantee that FK values always point to valid rows |
| **ON DELETE CASCADE** | Auto-delete referencing rows when the referenced row is deleted |
| **ON DELETE RESTRICT** | Block deletion of a row that is still referenced |
| **ON DELETE SET NULL** | Set FK column to NULL when referenced row is deleted |
| **EXCLUSION** | Constraint preventing two rows from satisfying a set of operator comparisons |
| **B-tree index** | Auto-created by UNIQUE and PRIMARY KEY for fast lookups |
| **GiST index** | Auto-created by EXCLUSION constraints |

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
