<p align="center">
<a href="./CHAPTER%2001%20-%20WRITE%20ON%20PAPER.md">⬅️ CHAPTER 01 - WRITE ON PAPER</a>
&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
<a href="./CHAPTER%2003%20-%20DISCOVER%20DATABASE.md">CHAPTER 03 – DISCOVER DATABASE ➡️</a>
</p>

---
<h1 align="center">CHAPTER 02 - 📊 USING SPREADSHEET</h1>

---
## 🔑 KEYWORDS
> **Spreadsheet**

> **Digital Storage**  

> **Formulas & Functions**  

---

## 📖 DEFINITION
> **Spreadsheet**
- A digital tool to store, organize, and manipulate data in **rows and columns**.
- Supports **formulas, functions, and filtering**, making data analysis faster.
- Searching, updating, or aggregating data is **efficient and accurate** compared to paper.

---

## 🧱 STORY FORMAT

> **Scenario:**
From CHAPTER 01 - WRITE ON PAPER, we observe utilizing paper and pen, to answer our manager's question will take eternity.
So, need to find a better way of storing data that help us to complete MINI EXERCISES faster.
And **SPREADSHEET** is our answer, therefore we move record details of **10,000 employees** digitally in a spreadsheet.  
- Firstly, copy entire data from text file to a csv (one of many variant of spreadsheet)
- You import the data from **employee_data.csv**.  
- Finding a single employee or counting department-wise employees is **instant**.  
- Calculating total salaries, averages, or department stats is **simple with formulas**.  

> **Lesson:** Spreadsheets are **fast, reliable, and scalable** for moderate-sized datasets.

---

## 💡 TIP TO REMEMBER
- Always use spreadsheets for **medium-sized datasets** (hundreds to tens of thousands of rows).  
- Learn **basic formulas**: SUM, AVERAGE, COUNTIF, VLOOKUP, FILTER.  
- Spreadsheets bridge the gap between **manual records** and **databases**.

---

## 💪 MINI EXERCISES
> DATA SOURCE
- [employee_data.csv](./DATASETS/employee_data.csv)

> METADATA
- employee_data is a CSV file  
- each row represents an individual employee  
- columns: EmployeeID, Name, Department, Salary  

> Now try these tasks in your spreadsheet:

1. What is the employee id of **Robert Miller** from *HR* Department  
<details>
  <summary>✅ Solution:</summary>
  
  **EmployeeID: 4014**  
  *(Hint: Use FILTER or VLOOKUP; make sure to press ctrl+shift+entre instead of purely enter)*- 
  - *=INDEX(A:A, MATCH(1, (B:B="Robert Miller")*(C:C="HR"), 0)) *
  - *VLOOP - =VLOOKUP("Robert MillerHR", CHOOSE({1,2}, B:B&C:C, A:A), 2, FALSE) make sure to press ctrl+shift+entre instead of purely enter*
</details>

2. Total number of employees in *Finance* Department  
<details>
  <summary>✅ Solution:</summary>
  
  **Employees in Finance Department: 1710**  
  *(Hint: Use COUNTIF function - =COUNTIF(C:C, "Finance"))*
</details>

3. Average salary of all employees  
<details>
  <summary>✅ Solution:</summary>
  
  **Employees Average Salary: 90170.32**  
  *(Hint: Use AVERAGE function on the Salary column - =AVERAGE(D:D))*
</details>

4. Get me average salary for every department 
<details>
  <summary>✅ Solution:</summary>
  
| Department  | Average Salary    |
|------------|-----------------|
| Finance    | 90199.59        |
| HR         | 88685.41        |
| IT         | 89627.00        |
| Marketing  | 91380.42        |
| Operations | 89799.00        |
| Sales      | 91284.20        |

*(Hint: Use the AVERAGE function on the Salary column – =AVERAGEIFS(D:D, C:C, "Finance"))*

To find the average salary for each department, you would have to:
1. Manually list all departments.
2. Replace "Finance" in the formula with each department name.
3. Repeat the calculation for every department.
4. If your organization has many departments, this becomes slow, repetitive, and error-prone.

💡 **Lesson:** Even spreadsheets start to struggle as data complexity grows, showing the need for a more powerful storage solution like a database.
</details>

---

## 🔍 Observations from the MINI EXERCISES

> 📌 **Searching for a single record:** Instant with FILTER or VLOOKUP. Much faster than scanning pages manually.  

> 📌 **Counting employees in a department:** COUNTIF ensures accurate counts without human error.  

> 📌 **Calculating averages and totals:** SUM and AVERAGE functions make calculations **reliable and fast**.  

> 📌 **Average salary for each department:** To find averages for every department, you would need to manually list all departments and replace the department name in the formula (`=AVERAGEIFS(D:D, C:C, "Finance")`) for each one.  
> This becomes **tedious and error-prone** when an organization has many departments.  

> 💡 **Key takeaway:**  
Spreadsheets reduce errors, save time, and make data manipulation much easier than manual records.  
However, as datasets and complexity grow, even spreadsheets can become cumbersome, highlighting the **need for more powerful storage solutions like databases**.


---

## 🔗 Resources & Links
- 📕 [Download Ebook](https://code4coin.gumroad.com/)
- 🎥 [YouTube](https://www.youtube.com/@code4coin)
- 💼 [LinkedIn](https://www.linkedin.com/in/nitin22/)
- 📸 [Instagram](https://www.instagram.com/code4coin/)

---
