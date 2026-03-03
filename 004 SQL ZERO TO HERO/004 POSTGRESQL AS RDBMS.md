# 🐘 PostgreSQL — Complete Guide

> *One of the most advanced open-source database systems in the world.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Open Source](https://img.shields.io/badge/Open%20Source-✓-22C55E?style=for-the-badge)
![License](https://img.shields.io/badge/License-PostgreSQL-blue?style=for-the-badge)
![SQL Conformance](https://img.shields.io/badge/SQL%20Core%20Features-177%20Mandatory-F59E0B?style=for-the-badge)

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

- [What is PostgreSQL?](#-what-is-postgresql)
- [Key Features](#-key-features)
- [Procedural Languages](#-procedural-languages-pls)
- [How PL Handlers Work](#-how-procedural-language-handlers-work)
- [Quick Reference](#-quick-reference)

---

## 🐘 What is PostgreSQL?

**PostgreSQL** is a powerful, open-source **object-relational database system (RDBMS)** known for its reliability, feature richness, and standards compliance.

```
🎓 Origin:  University of California, Berkeley, United States
📅 Born:    1986 (originally called POSTGRES)
🌍 Status:  One of the most advanced & widely used databases in the world
💰 Cost:    Completely FREE — forever
```

### PostgreSQL vs Standard RDBMS

| Feature | Standard RDBMS | PostgreSQL |
|---------|---------------|------------|
| Tables & Rows | ✅ | ✅ |
| SQL Support | ✅ | ✅ (177 mandatory features) |
| Custom Data Types | ❌ | ✅ |
| Custom Functions | Limited | ✅ Full support |
| Multiple Prog. Languages | ❌ | ✅ (PL/pgSQL, Python, Perl, Tcl) |
| Extensibility | Limited | ✅ Highly extensible |
| Open Source | Varies | ✅ Always |

---

## ✨ Key Features

### 1. 🆓 Open Source & Free to Use

PostgreSQL is **completely open source** — no licensing fees, no restrictions.

```
Anyone can:
  ✅ Use it           — in personal or commercial projects
  ✅ Modify it        — customize the source code
  ✅ Distribute it    — share with others freely
  ✅ Build on it      — create products powered by PostgreSQL
```

> Licensed under the **PostgreSQL License** — one of the most permissive open-source licenses, similar to MIT/BSD.

---

### 2. 🔧 Highly Extensible

PostgreSQL lets you extend the database itself — **without recompiling** the database engine.

<details>
<summary>📌 <strong>Click to expand: What "extensible" means in practice</strong></summary>

You can add your own:

| Extension Type | What You Can Do | Example |
|---------------|-----------------|---------|
| **Custom Data Types** | Define types beyond INT, VARCHAR, DATE | `CREATE TYPE mood AS ENUM ('happy', 'sad')` |
| **Custom Functions** | Write your own SQL functions | `CREATE FUNCTION add(a INT, b INT) RETURNS INT` |
| **Custom Operators** | Define new operators like `+`, `-` | `CREATE OPERATOR + (...)` |
| **Custom Index Types** | New ways to index your data | GIN, GiST indexes for full-text search |
| **Custom Languages** | Write functions in Python, Perl, Tcl | `CREATE FUNCTION py_func() LANGUAGE plpython3u` |

```sql
-- Example: Creating a custom ENUM type
CREATE TYPE skill_level AS ENUM ('beginner', 'intermediate', 'advanced', 'expert');

CREATE TABLE developers (
    id         INT PRIMARY KEY,
    name       VARCHAR(100),
    sql_level  skill_level        -- Using our custom type!
);

INSERT INTO developers VALUES (1, 'Nitin', 'advanced');
```

</details>

---

## 🧩 Procedural Languages (PLs)

PostgreSQL's most powerful extensibility feature is its support for **Procedural Languages** — the ability to write custom database functions in languages beyond SQL.

### What is a Procedural Language?

A **Procedural Language (PL)** allows you to write **User-Defined Functions (UDFs)** inside PostgreSQL using a full programming language — complete with loops, conditionals, variables, and logic.

```
SQL alone:                PostgreSQL with PLs:
─────────────             ──────────────────────────────
SELECT, INSERT  →         + Loops, IF/ELSE, variables
UPDATE, DELETE  →         + Error handling & exceptions
No logic        →         + Complex business logic in DB
```

---

### 🌐 The 4 Native Procedural Languages

PostgreSQL ships with **4 built-in Procedural Languages** out of the box:

| Language | Full Name | Best For |
|----------|-----------|----------|
| 🟦 **PL/pgSQL** | Procedural Language / PostgreSQL | General-purpose DB functions, triggers |
| 🟣 **PL/Tcl** | Procedural Language / Tcl | String processing, scripting |
| 🟡 **PL/Perl** | Procedural Language / Perl | Text manipulation, regex-heavy tasks |
| 🐍 **PL/Python** | Procedural Language / Python | Data science, complex logic, ML |

---

<details>
<summary>📌 <strong>Click to expand: PL/pgSQL example — a function with logic</strong></summary>

```sql
-- A function that returns a grade label based on a score
CREATE OR REPLACE FUNCTION get_grade(score INT)
RETURNS VARCHAR AS $$
BEGIN
    IF score >= 90 THEN
        RETURN 'A';
    ELSIF score >= 75 THEN
        RETURN 'B';
    ELSIF score >= 60 THEN
        RETURN 'C';
    ELSE
        RETURN 'F';
    END IF;
END;
$$ LANGUAGE plpgsql;

-- Using the function:
SELECT name, get_grade(score) AS grade FROM students;
```

This is **not possible with plain SQL** — it requires a Procedural Language.

</details>

<details>
<summary>📌 <strong>Click to expand: PL/Python example — using Python inside PostgreSQL</strong></summary>

```sql
-- A Python function running inside PostgreSQL
CREATE OR REPLACE FUNCTION word_count(text_input TEXT)
RETURNS INT AS $$
    words = text_input.split()
    return len(words)
$$ LANGUAGE plpython3u;

-- Using it:
SELECT word_count('PostgreSQL is incredibly powerful');
-- Returns: 4
```

Yes — that's **real Python running inside your database!**

</details>

---

## ⚙️ How Procedural Language Handlers Work

To support these languages, PostgreSQL uses a special component called a **Handler**.

### What is a Handler?

A **Handler** is a bridge written in **C** that connects a procedural language to PostgreSQL's internal engine. Every supported language has its own handler.

```
Your PL Function (Python / Perl / Tcl / pgSQL)
              │
              ▼
    ┌─────────────────┐
    │    Handler (C)  │  ← Compiled C code, loaded by PostgreSQL
    └─────────────────┘
              │
    ┌─────────┴──────────┐
    ▼                    ▼
Do All the Work      Act as Glue
(Self-contained)     (Bridge mode)
```

---

### Two Modes a Handler Can Operate In

<details>
<summary>📌 <strong>Click to expand: Mode 1 — Do All the Work</strong></summary>

In this mode, the handler is **fully self-contained**. It handles everything internally:

```
Handler responsibilities:
  ✅ Parsing the procedural code
  ✅ Syntax checking
  ✅ Compiling the function
  ✅ Executing the result
  ✅ Returning output to PostgreSQL

Used by: PL/pgSQL (manages its own execution pipeline)
```

</details>

<details>
<summary>📌 <strong>Click to expand: Mode 2 — Act as Glue (Bridge)</strong></summary>

In this mode, the handler acts as a **translator** between the procedural language and PostgreSQL:

```
Your Python code
      │
      ▼
 PL/Python Handler (C)   ← Translates between PostgreSQL & Python
      │
      ▼
 Python Interpreter      ← Executes the actual Python
      │
      ▼
 Result → Handler → PostgreSQL

Used by: PL/Python, PL/Perl, PL/Tcl
```

The handler doesn't execute the code itself — it delegates to the external language runtime and passes results back to PostgreSQL.

</details>

---

### Handler Lifecycle

```
1. LOAD      PostgreSQL loads the handler (compiled C library)
      │
      ▼
2. CALL      A function using that PL is invoked
      │
      ▼
3. HANDLE    Handler parses + executes (or delegates) the code
      │
      ▼
4. RETURN    Result is passed back to the PostgreSQL engine
```

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: Enable Procedural Languages in PostgreSQL</strong></summary>

```sql
-- Enable PL/pgSQL (usually enabled by default)
CREATE EXTENSION IF NOT EXISTS plpgsql;

-- Enable PL/Python
CREATE EXTENSION plpython3u;

-- Enable PL/Perl
CREATE EXTENSION plperl;

-- Enable PL/Tcl
CREATE EXTENSION pltcl;

-- Check which languages are enabled
SELECT lanname, lanispl FROM pg_language;
```

</details>

<details>
<summary>📌 <strong>Click to expand: Key Terms Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **PostgreSQL** | Open-source object-relational database system from UC Berkeley |
| **RDBMS** | Relational Database Management System — stores data in tables |
| **Open Source** | Free to use, modify, and distribute |
| **Extensible** | Can be customized without modifying core database code |
| **Procedural Language (PL)** | A programming language used to write functions inside PostgreSQL |
| **User-Defined Function (UDF)** | A custom function written by the user, stored in the database |
| **Handler** | A C-based component that enables a PL to work inside PostgreSQL |
| **PL/pgSQL** | PostgreSQL's native procedural language (most commonly used) |
| **PL/Python** | Allows Python code to run as database functions |
| **PL/Perl** | Allows Perl code to run as database functions |
| **PL/Tcl** | Allows Tcl code to run as database functions |
| **SQL Conformance** | As of 2023, SQL has **177 mandatory core features** |

</details>

<details>
<summary>📌 <strong>Click to expand: PostgreSQL at a Glance</strong></summary>

```
Name         PostgreSQL (also called "Postgres")
Type         Object-Relational Database Management System (ORDBMS)
Origin       UC Berkeley, United States (1986)
License      PostgreSQL License (open source, permissive)
Cost         Free
Native PLs   PL/pgSQL · PL/Tcl · PL/Perl · PL/Python
SQL Features 177 mandatory core features (as of 2023)
Known For    Reliability · Extensibility · Standards compliance
```

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
