<p align="center">
<a href="./CHAPTER%2002%20-%20USING%20SPREADSHEET.md">⬅️ CHAPTER 02 - USING SPREADSHEET</a>
&nbsp;&nbsp;&nbsp;|
</p>

---
<h1 align="center">CHAPTER 03 - 🗄️ DISCOVER DATABASE</h1>

---
## 🔑 KEYWORDS
> **Database**

> **Structured Storage**  

> **Query & Retrieval**  

---

## 📖 DEFINITION
> **Database**
- A structured digital storage system that organizes data into **tables** with rows and columns.  
- Supports **queries** to retrieve, update, or manipulate data efficiently.  
- Ideal for **large datasets** where spreadsheets become inefficient or error-prone.  

---

## 🧱 STORY FORMAT

> **Scenario:**  
From CHAPTER 02 - USING SPREADSHEET, we observe that spreadsheets make working with 10,000 employees faster than paper.  
However, as datasets grow even larger or need **complex relationships**, spreadsheets can become cumbersome.  
Here, a **database** is the better solution:  
- We store employee data in a structured **table** (Employee table).  
- We can quickly query specific employees, count department members, or calculate salary statistics.  
- SQL or database tools allow **accurate, repeatable, and fast operations**.

> **Lesson:** Databases are **scalable, precise, and powerful** for large datasets.

---

## 💡 TIP TO REMEMBER
- Use databases for **large or relational datasets**.  
- Learn **basic SQL commands**: `SELECT`, `WHERE`, `COUNT`, `AVG`, `SUM`.  
- Databases eliminate most **manual errors** and improve efficiency for large-scale data operations.

---

## 💪 MINI EXERCISES
> DATA SOURCE
- [employee_data_table.sql](./DATASETS/employee_data_table.sql)

> METADATA
- employee table created in database (same as spreadsheet)
- INSERT query putting employee data in employee table in database
- INSERT is same as copying data manually from paper to spreadsheet
- each row represents an individual employee  
- columns: EmployeeID, Name, Department, Salary  

> Now try these tasks using **SQL or any database tool**:

1. Find the employee id of **Robert Miller** from *HR* Department  
<details>
  <summary>✅ Solution:</summary>
  
  **SQL Query Example:**  
  ```sql
  SELECT EmployeeID 
  FROM Employee 
  WHERE Name = 'Robert Miller' AND Department = 'HR';
  ```  
  **EmployeeID:** 4014
</details>

2. Count total employees in *Finance* Department  
<details>
  <summary>✅ Solution:</summary>
  
  **SQL Query Example:**  
  ```sql
  SELECT COUNT(*) 
  FROM Employee 
  WHERE Department = 'Finance';
  ```  
  **Employees in Finance Department:** 1710
</details>

3. Find average salary of all employees  
<details>
  <summary>✅ Solution:</summary>
  
  **SQL Query Example:**  
  ```sql
  SELECT AVG(Salary) 
  FROM Employee;
  ```  
  **Employees Average Salary:** 90170.32
</details>

4. Get me average salary for every department 
<details>
  <summary>✅ Solution:</summary>

  ```sql
SELECT department AS Department, AVG(salary) AS "Average Salary"
  FROM employee
 GROUP BY department;
```

**Departmental Average Salary:**  

| Department  | Average Salary    |
|------------|-----------------|
| Finance    | 90199.59        |
| HR         | 88685.41        |
| IT         | 89627.00        |
| Marketing  | 91380.42        |
| Operations | 89799.00        |
| Sales      | 91284.20        |

  </details>
  
---

## 🔍 Observations from the MINI EXERCISES

> 📌 **Searching for a single record:** SQL queries allow instant retrieval with conditions (e.g., `WHERE Name = 'Robert Miller' AND Department = 'HR'`), no manual scanning or helper columns required.  

> 📌 **Counting employees in a department:** Using `COUNT(*)` with a `WHERE` clause gives accurate counts automatically, regardless of dataset size.  

> 📌 **Calculating averages and totals:** Aggregate functions like `AVG(Salary)` or `SUM(Salary)` can compute results for the entire table or grouped by department efficiently.  

> 📌 **Average salary for each department:** A single query with `GROUP BY Department` returns the average salary for every department at once, eliminating tedious manual steps required in spreadsheets.  

> 💡 **Key takeaway:**  
- What’s hard on paper becomes easier in spreadsheets, but SQL makes it faster, simpler, and almost like reading English.
- Databases and SQL scale effortlessly with dataset size, reduce errors, and enable **powerful, repeatable, and fast analysis** that spreadsheets cannot handle efficiently for large or complex data.

---

## 🔗 Resources & Links
- 📕 [Download Ebook](https://code4coin.gumroad.com/)
- 🎥 [YouTube](https://www.youtube.com/@code4coin)
- 💼 [LinkedIn](https://www.linkedin.com/in/nitin22/)
- 📸 [Instagram](https://www.instagram.com/code4coin/)
