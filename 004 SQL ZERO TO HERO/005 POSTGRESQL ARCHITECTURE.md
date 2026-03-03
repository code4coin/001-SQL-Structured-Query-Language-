# 🏛️ PostgreSQL Architecture — Fundamentals

> *How PostgreSQL's client/server model works under the hood.*

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Architecture](https://img.shields.io/badge/Topic-Architecture-6366F1?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner%20Friendly-22C55E?style=for-the-badge)

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

- [The Client/Server Model](#-the-clientserver-model)
- [The Two Key Players](#-the-two-key-players)
- [Communicating Over a Network](#-communicating-over-a-network)
- [Handling Multiple Connections](#-handling-multiple-connections)
- [The Full Picture](#-the-full-picture)
- [Quick Reference](#-quick-reference)

---

## 🔄 The Client/Server Model

PostgreSQL is built on a **client/server architecture** — the same model used by web browsers, email clients, and most networked applications.

```
                    CLIENT/SERVER MODEL
    ┌──────────────────────────────────────────────────┐
    │                                                  │
    │   👤 Client  ◄──── requests/responses ────►  🖥️ Server  │
    │  (your app)           (TCP/IP or            (postgres)  │
    │                      local socket)                │
    │                                                  │
    └──────────────────────────────────────────────────┘
```

> A **PostgreSQL session** is made up of two cooperating sides — a **server process** and a **client application** — working together to perform database operations.

---

## 👥 The Two Key Players

### 🖥️ 1. The Server Process — `postgres`

The server is the brain of PostgreSQL. It runs continuously in the background and is responsible for everything database-related.

```
postgres (server process)
├── 📂 Manages database files on disk
├── 🔌 Accepts incoming connections from clients
└── ⚙️  Executes database actions on behalf of clients
```

| Responsibility | Description |
|----------------|-------------|
| **File Management** | Reads and writes all database files on disk |
| **Connection Handling** | Listens for and accepts client connections |
| **Query Execution** | Runs SQL queries sent by clients |
| **Security** | Authenticates and authorizes client access |

> 🔑 **Key fact:** The database server program is specifically called **`postgres`** — that's its actual process name on your system.

---

### 💻 2. The Client (Frontend) Application

The client is *any* application that wants to talk to the database. PostgreSQL doesn't care what the client looks like — it just needs to speak the PostgreSQL protocol.

<details>
<summary>📌 <strong>Click to expand: Types of PostgreSQL clients</strong></summary>

| Client Type | Examples | Use Case |
|-------------|----------|----------|
| 🖥️ **Text-oriented tools** | `psql` (built-in CLI) | Running SQL from the terminal |
| 🎨 **Graphical applications** | pgAdmin, DBeaver, TablePlus | Visual database management |
| 🌐 **Web servers** | Node.js, Django, Rails apps | Serving web pages with DB data |
| 🔧 **Maintenance tools** | pg_dump, pg_restore | Backup, restore, migration |
| 📊 **BI & analytics tools** | Tableau, Metabase, Grafana | Data visualization & reporting |
| 🐍 **Custom applications** | Your Python/Java/Go app | Business logic + data access |

> Some clients are **shipped with PostgreSQL** itself (like `psql`). Most are **built by the community** or by you.

</details>

---

## 🌐 Communicating Over a Network

One of the most important design decisions in PostgreSQL is that **the client and server do not need to be on the same machine**.

```
  SAME MACHINE                      DIFFERENT MACHINES
  ─────────────                     ───────────────────

  ┌─────────────────┐               ┌─────────┐     🌍 Network     ┌─────────────┐
  │  Client + Server│               │  Client │ ◄──── TCP/IP ────► │   Server    │
  │  (local socket) │               │  (your  │                    │  (postgres) │
  └─────────────────┘               │  laptop)│                    │  (DB host)  │
                                    └─────────┘                    └─────────────┘
  Fast, no network overhead         Flexible, deployable anywhere
```

### ⚠️ Important Implication — File Access

Because the client and server can be on **different machines**, there's a critical distinction you must remember:

<details>
<summary>📌 <strong>Click to expand: The file access gotcha explained</strong></summary>

```
Your Client Machine                 Database Server Machine
───────────────────                 ───────────────────────
📁 /home/nitin/data.csv   ≠         ❌ /home/nitin/data.csv
   (exists here)                        (does NOT exist here)

A file path that works on          ...may not exist at all on
your local machine...              the database server.
```

**Practical example:**

```sql
-- This tries to load a file from the SERVER's filesystem, not yours:
COPY students FROM '/home/nitin/students.csv' CSV HEADER;
--                  ^^^^^^^^^^^^^^^^^^^^^^^^
--                  This path must exist on the SERVER machine!

-- To load from YOUR local machine, use psql's \copy instead:
\copy students FROM '/home/nitin/students.csv' CSV HEADER;
--                  ^^^^^^^^^^^^^^^^^^^^^^^^
--                  This path is on YOUR local machine ✅
```

> **Rule of thumb:** `COPY` = server path. `\copy` (psql) = client path.

</details>

---

## ⚡ Handling Multiple Connections

PostgreSQL is built to serve **many clients simultaneously**. Here's exactly how it achieves this:

### The Forking Model

```
                    SUPERVISOR postgres process
                    (always running, waiting...)
                              │
              ┌───────────────┼───────────────┐
              │               │               │
         Client A         Client B         Client C
          connects         connects         connects
              │               │               │
              ▼               ▼               ▼
        ┌──────────┐    ┌──────────┐    ┌──────────┐
        │ postgres │    │ postgres │    │ postgres │
        │ process 1│    │ process 2│    │ process 3│
        │ (forked) │    │ (forked) │    │ (forked) │
        └──────────┘    └──────────┘    └──────────┘
              │               │               │
         Handles A       Handles B       Handles C
         directly        directly        directly
         (no parent      (no parent      (no parent
         involvement)    involvement)    involvement)
```

### Step-by-Step: What Happens When You Connect

```
Step 1 — Client knocks
  👤 Client sends a connection request to the postgres supervisor

Step 2 — Server forks a new process
  🖥️ The supervisor "forks" (spawns) a brand-new postgres child process
                            just for this client

Step 3 — Direct communication begins
  👤 Client ◄──────────────────────────► 🖥️ Child process
       Direct line — no supervisor involvement

Step 4 — Supervisor goes back to waiting
  🖥️ Supervisor returns to listening for the next connection
```

<details>
<summary>📌 <strong>Click to expand: Why forking? What does it mean?</strong></summary>

**Forking** means creating an exact copy of the current process. In PostgreSQL's case:

- The supervisor `postgres` process **forks itself** when a new client connects
- The forked (child) process gets its own **memory space** and **execution context**
- The child handles **only that one client** — completely independent
- If one client crashes, **other clients are unaffected**

```
Benefits of forking:
  ✅ Isolation     — each connection is its own process
  ✅ Stability     — one bad query doesn't kill other connections
  ✅ Simplicity    — no complex thread management inside each session
  ✅ Concurrency   — many clients served simultaneously
```

</details>

<details>
<summary>📌 <strong>Click to expand: See it yourself — active connections in PostgreSQL</strong></summary>

```sql
-- See all active connections right now:
SELECT pid, usename, application_name, client_addr, state
FROM pg_stat_activity;

-- Count total active connections:
SELECT COUNT(*) FROM pg_stat_activity;

-- See max allowed connections:
SHOW max_connections;
```

Each row in `pg_stat_activity` corresponds to **one forked postgres process**.

</details>

---

## 🗺️ The Full Picture

Putting it all together — here's the complete PostgreSQL architecture at a glance:

```
                    ╔══════════════════════════════════════════╗
                    ║         PostgreSQL Server Host           ║
                    ║                                          ║
                    ║   ┌─────────────────────────────────┐   ║
                    ║   │  Supervisor: postgres (master)  │   ║
                    ║   │  • Always running               │   ║
                    ║   │  • Manages database files       │   ║
                    ║   │  • Listens for connections      │   ║
                    ║   └──────────────┬──────────────────┘   ║
                    ║                  │ forks                 ║
                    ║      ┌───────────┼───────────┐          ║
                    ║      ▼           ▼           ▼          ║
                    ║  [process 1] [process 2] [process 3]    ║
                    ║      │           │           │          ║
                    ╚══════╪═══════════╪═══════════╪══════════╝
                     TCP/IP│      TCP/IP│      TCP/IP│
                    ╔══════╪═══════════╪═══════════╪══════════╗
                    ║      ▼           ▼           ▼          ║
                    ║  [Client A]  [Client B]  [Client C]     ║
                    ║  psql / app  web server  pgAdmin        ║
                    ║              Client Side                 ║
                    ╚══════════════════════════════════════════╝
```

---

## 📖 Quick Reference

<details>
<summary>📌 <strong>Click to expand: Key Concepts Cheat Sheet</strong></summary>

| Concept | Detail |
|---------|--------|
| **Architecture** | Client / Server model |
| **Server program name** | `postgres` |
| **Client examples** | psql, pgAdmin, web apps, custom tools |
| **Network protocol** | TCP/IP (when client & server on different machines) |
| **Local connection** | Unix domain socket (faster, same machine) |
| **Multi-connection method** | Forking — one child process per client |
| **Supervisor role** | Always-on, listens for new connections, forks child processes |
| **Child process role** | Handles exactly one client session, then exits |
| **File access rule** | `COPY` uses server paths; `\copy` uses client paths |

</details>

<details>
<summary>📌 <strong>Click to expand: Glossary</strong></summary>

| Term | Definition |
|------|------------|
| **Client/Server model** | Architecture where a client requests services from a server over a network |
| **`postgres` process** | The actual server program that runs PostgreSQL |
| **Frontend** | Another name for the client application |
| **Backend** | Another name for the server process |
| **TCP/IP** | The network protocol used for client-server communication across machines |
| **Fork** | Creating a new child process as a copy of the parent process |
| **Supervisor process** | The main `postgres` process that manages connections and forks children |
| **Child process** | A forked postgres process dedicated to a single client connection |
| **Concurrent connections** | Multiple clients connected and being served at the same time |
| **`pg_stat_activity`** | System view showing all current database connections and their state |

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
