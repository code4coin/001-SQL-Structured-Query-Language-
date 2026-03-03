# 🗄️ SQL — Structured Query Language

> **An interactive reference guide to understanding SQL's origins, meaning, and design philosophy.**

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

- [What is SQL?](#what-is-sql)
- [Breaking Down the Name](#breaking-down-the-name)
  - [Structured](#-structured)
  - [Query](#-query)
  - [Language](#-language)
- [A Brief History](#a-brief-history)
- [Quick Reference Card](#quick-reference-card)

---

## What is SQL?

**SQL (Structured Query Language)** is the standard language used to communicate with relational databases. It was invented in the **1970s**, based on E.F. Codd's *relational data model*, and was originally known as:

> 🔤 **SEQUEL** — *Structured English Query Language*

The name was later shortened to **SQL** for trademark reasons, but the core ideas behind the name have always remained central to the language.

---

## Breaking Down the Name

### 🏗️ **Structured**

Data in SQL is organized in a **highly structured format**:

| Element | Description |
|--------|-------------|
| 📊 Tables | Data lives in rows and columns |
| 🔢 Data Types | Each column has a defined type (INT, VARCHAR, DATE, etc.) |
| 🔒 Constraints | Rules enforced on data (NOT NULL, UNIQUE, FOREIGN KEY) |
| 🔗 Relationships | Tables connect to each other through keys |
| ✍️ Syntax | Queries follow strict, structured grammar |

**Example of structured SQL syntax:**
```sql
SELECT first_name, last_name
FROM employees
WHERE department = 'Engineering'
ORDER BY last_name ASC;
```

---

### 🔍 **Query**

The **primary purpose** of SQL is to *query* a database — to ask it questions and retrieve meaningful answers.

```sql
-- "Query" in action: asking the database a question
SELECT product_name, price
FROM products
WHERE price < 50
  AND category = 'Books';
```

Beyond querying, SQL also handles:

| Operation | SQL Command | Purpose |
|-----------|-------------|---------|
| ➕ Insert | `INSERT INTO` | Add new records |
| ✏️ Update | `UPDATE` | Modify existing records |
| ❌ Delete | `DELETE FROM` | Remove records |
| 📖 Select | `SELECT` | **Retrieve data (core function)** |

> 💡 **Key insight:** While SQL can do much more, *querying* is the fundamental operation it was built around.

---

### 🗣️ **Language**

SQL is a **formal language** with its own syntax, keywords, and grammar rules — but with an important twist:

| Traditional Programming Languages | SQL |
|-----------------------------------|-----|
| Imperative — you describe *how* to do it | **Declarative** — you describe *what* you want |
| You write step-by-step logic | The database engine figures out *how* |
| e.g., Python, Java, C++ | e.g., SQL |

**Declarative example:**
```sql
-- You say WHAT you want:
SELECT AVG(salary) FROM employees WHERE department = 'Sales';

-- You don't say HOW to scan the table, which index to use,
-- or how to compute the average — the database handles that.
```

---

## A Brief History

```
1970 ──────────────────────────────────────────────────────── Present
  │
  ├── 1970: E.F. Codd publishes the Relational Model paper at IBM
  │
  ├── 1973: IBM researchers Donald Chamberlin & Raymond Boyce
  │         develop SEQUEL (Structured English Query Language)
  │
  ├── 1977: Name shortened to SQL for trademark reasons
  │
  ├── 1986: SQL becomes an ANSI standard (SQL-86)
  │
  ├── 1992: Major revision — SQL-92 (SQL2)
  │
  ├── 1999: SQL:1999 adds procedural elements
  │
  └── 2023: SQL:2023 — latest standard revision
```

### 🧠 Key Figures

| Person | Contribution |
|--------|-------------|
| **E.F. Codd** | Invented the relational model (1970) — the foundation SQL is built on |
| **Donald Chamberlin** | Co-developed SEQUEL at IBM |
| **Raymond Boyce** | Co-developed SEQUEL at IBM |

---

## Quick Reference Card

<details>
<summary>📌 <strong>Click to expand: SQL in one sentence</strong></summary>

> SQL is a **declarative**, **structured** language for **querying** and managing data in **relational databases**, invented at IBM in the 1970s based on E.F. Codd's relational model.

</details>

<details>
<summary>📌 <strong>Click to expand: Why "declarative" matters</strong></summary>

With SQL, you focus on **what result you want**, not **how to compute it**. This makes SQL:
- Easier to read and write
- Database-agnostic in logic
- Optimizable by the database engine automatically

</details>

<details>
<summary>📌 <strong>Click to expand: The SEQUEL → SQL rename</strong></summary>

The language was originally called **SEQUEL** (Structured English Query Language), developed by IBM researchers **Donald Chamberlin** and **Raymond Boyce** in the early 1970s. The name was shortened to **SQL** for trademark reasons — the word "SEQUEL" was already trademarked by another company.

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
