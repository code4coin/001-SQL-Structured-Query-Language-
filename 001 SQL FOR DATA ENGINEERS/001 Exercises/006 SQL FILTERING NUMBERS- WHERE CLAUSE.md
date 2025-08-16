# FILTERING NUMBERS - WHERE CLAUSE
---
## KEYWORDS
- **WHERE**
- **CONDITIONAL OPERATORS**
---
## DEFINITION
- **WHERE** - The WHERE clause is used to filter rows in a table based on a condition. It lets you pick only the data you need instead of returning everything.
- **CONDITIONAL OPERATORS** - Conditional operators are used in the WHERE clause to filter data based on specific conditions.

  | Operator   | Meaning                        | Example                                      |
  |----------- |------------------------------- |-------------------------------------------- |
  | `=`        | Equal to                       | `salary = 50000` → Employees earning €50,000 |
  | `!=` or `<>` | Not equal to                  | `department != 'IT'` → Employees not in IT |
  | `>`        | Greater than                   | `age > 30` → Employees older than 30       |
  | `<`        | Less than                      | `age < 30` → Employees younger than 30     |
  | `>=`       | Greater than or equal to       | `salary >= 50000` → Employees earning €50,000 or more |
  | `<=`       | Less than or equal to          | `salary <= 50000` → Employees earning €50,000 or less |

---
## QUERY FORMAT
```sql
-- 1. SQL for WHERE filter clause
SELECT column_names
FROM table_name
WHERE filter_condition;
```
---
## TIPS
1. **Remember Execution Order** - SQL first evaluates FROM, then WHERE, and finally SELECT. Filtering happens before selecting columns.
---
## EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)


# SQL WHERE Clause – Age Comparison Examples

Using the `employees` table with fields: `emp_id`, `dept_id`, `name`, `age`, `year_hired`

```sql
-- Comparing with string value
SELECT * FROM employees WHERE name = 'David';
```

```sql
-- Greater Than: employees older than 30
SELECT * FROM employees WHERE age > 30;
```

```sql
-- Less Than: employees younger than 25
SELECT * FROM employees WHERE age < 25;
```

```sql
-- Equal To: employees exactly 35 years old
SELECT * FROM employees WHERE age = 35;
```

```sql
-- Greater Than or Equal To: employees aged 30 or older
SELECT * FROM employees WHERE age >= 30;
```

```sql
-- Less Than or Equal To: employees aged 40 or younger
SELECT * FROM employees WHERE age <= 40;
```

```sql
-- Not Equal To: employees not 28 years old
SELECT * FROM employees WHERE age <> 28;
SELECT * FROM employees WHERE age != 28;
```
---

## PRACTISE
Using patrons table with fields - card_id, name, join_year, fines
1. Get patrons count joined after year 2010
2. Get patrons count joined before year 2020
3. Get patrons count joined in or after year 2019
4. Get patrons count joined in or before year 2019
5. Get patrons count joined in year 2023
6. Get patrons details whose name is 'Jasmin Lee'
7. Get patrons who did not joined in year 2022
---
## SOLUTIONS
1. Get patrons count joined after year 2010
   ```sql
   SELECT COUNT(*) as count_patrons
   FROM patrons
   WHERE join_year > 2010;
   ``` 
2. Get patrons count joined before year 2020
   ```sql
   SELECT COUNT(*) as count_patrons
   FROM patrons
   WHERE join_year < 2020;
   ```
3. Get patrons count joined in or after year 2019
      ```sql
   SELECT COUNT(*) as count_patrons
   FROM patrons
   WHERE join_year => 2019;
   ```
4. Get patrons count joined in or before year 2019
      ```sql
   SELECT COUNT(*) as count_patrons
   FROM patrons
   WHERE join_year <= 2019;
   ```
5. Get patrons count joined in year 2023
      ```sql
   SELECT COUNT(*) as count_patrons
   FROM patrons
   WHERE join_year = 2023;
   ```
6. Get patrons details whose name is 'Jasmin Lee'
   ```sql
   SELECT * 
   FROM patrons
   WHERE name = 'Jasmin Lee';
   ```
7. Get patrons who did not joined in year 2022
   ```sql
   SELECT * 
   FROM patrons
   WHERE join_year <> 2022;
   ```
---
## **Contributing** 🤝

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---
## **License** 📄

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---
## **More Resources** 🔗

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
