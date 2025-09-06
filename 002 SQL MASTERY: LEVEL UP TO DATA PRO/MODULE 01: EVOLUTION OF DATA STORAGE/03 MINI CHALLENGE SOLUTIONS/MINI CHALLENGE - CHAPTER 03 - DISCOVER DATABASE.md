---
<h1 align="center">DISCOVER DATABASE</h1>

---
## 🔑KEYWORDS
> **CREATE**

> **INSERT**

>**SELECT**

> **FROM**
---
## 📖DEFINITION
>**CREATE**
  - Defines a new table in the database.
  - Specify column names, data types, and optional constraints (like PRIMARY KEY, NOT NULL).
> **INSERT**
  - Adds new rows of data into a table.
  - Specify the table name, the columns to insert into, and the values for each column.
>**SELECT**
  - Choose columns or expressions to return.
  - Can include calculations, functions, aliases, and subqueries.
> **FROM**
  - Specifies the table(s) to query.
  - Can reference multiple tables with JOINs.
---

## 🧱QUERY FORMAT

> **SQL QUERY STRUCTURE:**

```sql
-- Format to create table in a database

CREATE TABLE table_name (
    column_name column_data_type any_constraint,
    column_name column_data_type any_constraint,
    ...
);

```

```sql
-- Format to insert data in a database table
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

```sql
-- 📝 Remember: ORDER OF EXECUTION
-- 1. FROM: specifies the table from which data will be retrieved
-- 2. SELECT: defines the columns to be displayed in the final result

SELECT
  column_names
FROM table_name;
```

<br>

> **SQL SAMPLE DATA:**

Table: **`employee`**
| EmployeeID | Name               | Department  | Salary      |
|------------|------------------|------------|------------|
| 1          | Benjamin Campbell | Finance    | 105000.36  |
| 2          | Sharon Mccormick  | Sales      | 56231.32   |
| 3          | Raymond Bass      | Operations | 98139.01   |
| 4          | Patrick Martin    | IT         | 108957.84  |


<br>

> **SQL QUERY EXAMPLE:**

❓**Problem Statement**: From `employee` table, retrieve all `names` stored inside.
```sql
SELECT
  name
FROM employee;
```
<br>

> **TRY YOURSELF: [CLICK TO EXECUTE QUERIES](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/002%20SQL%20MASTERY:%20LEVEL%20UP%20TO%20DATA%20PRO/MODULE%2000:%20INTRODUCTION%20TO%20COURSE/003%20SQL%20PLATFORM%20PREPARATION.md)**

<br>

> **SQL QUERY TEST:**

❓**PROBLEM STATEMENT 01**: Create `employee` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
CREATE TABLE employee (
    EmployeeID INT,
    Name TEXT, 
    Department	TEXT,
    Salary DECIMAL(10, 4)
);
```
</details>

❓**PROBLEM STATEMENT 02**: Insert records from above sample data into `employee` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
INSERT INTO employee VALUES
(1,'Benjamin Campbell','Finance',105000.36),
(2,'Sharon Mccormick','Sales',56231.32),
(3,'Raymond Bass','Operations',98139.01),
(4,'Patrick Martin','IT',108957.84);
```
</details>

❓**PROBLEM STATEMENT 03**: Retrieve `employeeid` and `name` from `employee` table
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT
  employeeid,
  name
FROM employee;
```
</details>

---
## 💡 TIP TO REMEMBER
> **ORDER OF EXECUTION**
```sql 
FROM → SELECT
```
- **_FROM executes first_**: SQL looks at which table to query.
- **_SELECT executes second_**: SQL picks the columns to return.

<br>

> **RETRIEVE ALL COLUMNS**
- Replace specific column names with the wildcard (*) in a SELECT statement to fetch all columns from the table.
---
## 🖥️ PLATFORM
Before moving to the exercises, we need a platform with tables and data: [PLATFORM](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/002%20SQL%20MASTERY:%20LEVEL%20UP%20TO%20DATA%20PRO/MODULE%2000:%20INTRODUCTION%20TO%20COURSE/003%20SQL%20PLATFORM%20PREPARATION.md)

---
## 🧪 DATA PREPARATION
Before moving into exercise, we need to prepare table with data in database: 

```sql
-- Creating `BeveragesAndFood` table structure in database"
CREATE TABLE BeveragesAndFood (
    Soft_Drink VARCHAR(50),
    Juice VARCHAR(50),
    Alcohol VARCHAR(50),
    Non_Veg VARCHAR(50),
    Veg VARCHAR(50)
);
```

```sql
-- Inserting data in the table created from above query:
INSERT INTO BeveragesAndFood (Soft_Drink, Juice, Alcohol, Non_Veg, Veg) VALUES
('Alice', 'Emma', 'Bob', 'Alice', 'Emma'),
('Bob', 'Grace', 'Carol', 'Carol', 'Grace'),
('David', 'Henry', 'Henry', 'Henry', 'David');
```

---
## 💪EXERCISE

1. Total number of people who will have any kind of drink (soft drinks, juice, or alcohol)
<details>
  <summary>✅ Solution:</summary>

```sql
SELECT soft_drink FROM beveragesandfood
UNION
SELECT juice FROM beveragesandfood
UNION
SELECT alcohol FROM beveragesandfood
;
```
</details>

2. Total number of people who will drink juice and eat protein (meat)
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT juice FROM beveragesandfood
UNION
SELECT non_veg FROM beveragesandfood;
```
</details>

3. Total number of people who will neither drink alcohol nor eat meat
<details>
  <summary>✅ Solution:</summary>
  
```sql
SELECT people
FROM (
    SELECT Soft_Drink AS people FROM BeveragesAndFood
    UNION 
    SELECT Juice FROM BeveragesAndFood
    UNION
    SELECT Veg FROM BeveragesAndFood
) AS all_people

EXCEPT
SELECT people FROM
( 
    SELECT Alcohol AS people FROM BeveragesAndFood
    UNION
    SELECT Non_Veg FROM BeveragesAndFood
) AS non_veg_alco_people
;
```
</details>


---
## 🔗 **MORE RESOURCES** 
Stay connected and explore more content:

- 📕 [Download Ebook](https://code4coin.gumroad.com/)
- 🎥 [YouTube](https://www.youtube.com/@code4coin)
- 💼 [LinkedIn](https://www.linkedin.com/in/nitin22/)
- 📸 [Instagram](https://www.instagram.com/code4coin/)
  
---
