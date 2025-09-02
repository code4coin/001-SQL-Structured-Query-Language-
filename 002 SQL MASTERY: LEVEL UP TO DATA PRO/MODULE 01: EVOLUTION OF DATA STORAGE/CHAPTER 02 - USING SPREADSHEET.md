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

---

## 🔍 Observations from the MINI EXERCISES

> 📌 **Searching for a single record:** Instant with FILTER or VLOOKUP. Much faster than scanning pages manually.  

> 📌 **Counting employees in a department:** COUNTIF ensures accurate counts without human error.  

> 📌 **Summing large numbers:** SUM and AVERAGE functions make calculations **reliable and fast**.  

> 💡 **Key takeaway:**  
Spreadsheets reduce errors, save time, and make data manipulation much easier. They are the **bridge between manual records and databases**.

---

## 📎 NEXT CHAPTER
➡️ [Go to Chapter 3 – Databases 🗄️](chapter-03-database.md)

---

## 🔗 Resources & Links
- 📕 [Download Ebook](https://code4coin.gumroad.com/)
- 🎥 [YouTube](https://www.youtube.com/@code4coin)
- 💼 [LinkedIn](https://www.linkedin.com/in/nitin22/)
- 📸 [Instagram](https://www.instagram.com/code4coin/)
