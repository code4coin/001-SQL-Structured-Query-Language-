# ERROR AND DEBUGGING
---
## KEYWORDS
- **Misspelled Field Names**
- **Incorrect Table Reference**
- **Missing Commas**
- **Case Sensitive**
- **Incorrect Keyword**
---
## DEFINITION
- **Misspelled Field Names**: Using a column name that does not exist in the table.  
- **Incorrect Table Reference**: Referring to a table that does not exist in the database.  
- **Missing Commas**: Forgetting commas between column names in a SELECT statement.  
- **Case Sensitive**: Using incorrect capitalization for table or column names in a case-sensitive database.  
- **Incorrect Keyword**: Typing an SQL keyword incorrectly, causing a syntax error.

---
## TIPS
1. Error debugging is an art; mastering it makes you a faster developer.
2. Maintain a concise log of the most common mistakes—misspelled keywords, missing commas, incorrect table names—and you’ll turn every “ugh” into a thirty-second fix.
---
## EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [000 EXECUTE THIS FIRST.md](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/001%20Exercises/000%20EXECUTE%20THIS%20FIRST.md)

---
### 1. **Misspelled Field Names**: Correct below query
```sql
SELECT COUNT(nam) AS count_name
FROM people;
```
### 2. **Incorrect Table Reference**: Correct below query
```sql
SELECT COUNT(nam) AS count_name
FROM peeple;
```
### 3. **Missing Commas**: Correct below query
```sql
SELECT name birthdate
FROM peeple;
```
### 4. **Case Sensitive**: Correct below query
```sql
SELECT "Name", birthdate
FROM peeple;
```
### 5. **Incorrect Keyword**: Correct below query
```sql
SELECT name, birthdate
FRM peeple;
```
---
## PRACTISE
### 1. Identify errors for the following codes
```sql
SELECT cardid, name
FROM patron;
```
### 2. Identify errors for the following codes
```sql
SELECT COUNT(card_id) AS count_cards
COUNNT(name) AS count_names,
FROM patrons
```
---
## SOLUTIONS
### 1. Query' Errors: 
- Misspelled field name (cardid -> card_id)
- Incorrect table reference/ name (patron -> patrons)
### 2. Query's Errors:
- Missing commans ( BETWEEN COUNT() functions)
- Extra comma (After alias count_names, -> count_name)
- Incorrect SQL keyword (COUNNT(name) -> COUNT(name))
---
## **CONTRIBUTING** 🤝

We welcome contributions! You can:

- Add new SQL exercises
- Improve existing chapters or examples
- Share interview questions or projects

Please open a **pull request** or **issue** to contribute.

---
## **LICENSE** 📄

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.

---
## **MORE RESOURCES** 🔗

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)
