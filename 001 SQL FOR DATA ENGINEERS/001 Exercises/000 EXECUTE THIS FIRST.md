# EXECUTE THIS FIRST
Below are the table structure and data, which will be utilized for the sql exercises, and learning new concepts.
Therefore, access the given platform and execute sql queries from the section "SQL QUERIES TO EXECUTE" to create tables with sample data.

---

## PLATFORM
We will utilize Programiz online SQL Complier website to create these tables 
Practice SQL commands online using: [Programiz SQL Online Compiler](https://www.programiz.com/sql/online-compiler)

---

## SQL QUERIES TO EXECUTE
**SQL Foundation: Below SQL table will be used across all exercises**

```sql
-- 1. Create Checkouts Table
DROP TABLE IF EXISTS Checkouts;
CREATE TABLE Checkouts (
    checkout_id INT PRIMARY KEY,
    card_id INT,
    book_title VARCHAR(150) NOT NULL,
    checkout_date DATE,
    return_date DATE,
    FOREIGN KEY (card_id) REFERENCES Patrons(card_id)
);

-- 2. Create Patrons Table
DROP TABLE IF EXISTS Patrons;
CREATE TABLE Patrons (
    card_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    join_year YEAR,
    fines DECIMAL(5,2) DEFAULT 0.00
);

-- 3. Create employees table
DROP TABLE IF EXISTS employees;
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    dept_id INT,
    name VARCHAR(100),
    age INT,
    year_hired INT
);

-- 4. Create people table
DROP TABLE IF EXISTS people;
CREATE TABLE people (
    id INTEGER PRIMARY KEY,
    name TEXT,
    birthdate DATE,
    email TEXT
);

-- Insert sample data into Patrons
INSERT INTO Patrons (card_id, name, join_year, fines) VALUES
(1, 'Jasmin Lee', 2022, 2.05),
(2, 'Izzy Khan', 2021, 0.00),
(3, 'Maham Patel', 2023, 1.50),
(4, 'Leo Brown', 2020, 0.00),
(5, 'Sophia Chen', 2022, 3.25);

-- Insert sample data into Checkouts
INSERT INTO Checkouts (checkout_id, card_id, book_title, checkout_date, return_date) VALUES
(101, 1, 'The Great Gatsby', '2025-08-01', '2025-08-15'),
(102, 2, '1984', '2025-08-03', '2025-08-17'),
(103, 2, 'To Kill a Mockingbird', '2025-08-05', '2025-08-19'),
(104, 3, 'Pride and Prejudice', '2025-08-02', '2025-08-16'),
(105, 5, 'Moby Dick', '2025-08-04', '2025-08-18');

-- Insert sample data into employee
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (1, 101, 'John',     30, 2020);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (2, 102, 'Jane',     28, 2021);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (3, 101, 'Michael',  35, 2019);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (4, 103, 'Emily',    29, 2021);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (5, 104, 'David',    32, 2020);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (6, 102, 'Sarah',    26, 2018);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (7, 103, 'Chris',    31, 2020);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (8, 101, 'Megan',    27, 2021);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (9, 104, 'Robert',   38, 2019);
INSERT INTO employees (emp_id, dept_id, name, age, year_hired) VALUES (10,104,'Patricia',  33, 2021);

-- Insert sample data into people
INSERT INTO people (id, name, birthdate, email) VALUES
(1, 'Alice', '1990-05-12', 'alice@example.com'),
(2, 'Bob', '1985-07-23', 'bob@example.com'),
(3, 'Charlie', '1992-01-15', 'charlie@example.com'),
(4, 'Alice', '1990-05-12', 'alice@example.com'),
(5, 'David', NULL, 'david@example.com'),
(6, 'Eve', '1988-11-02', NULL),
(7, NULL, '1995-03-10', 'unknown@example.com'),
(8, 'Frank', NULL, NULL),
(9, 'Bob', '1985-07-23', 'bob@example.com'),
(10, 'Grace', '1993-08-30', 'grace@example.com');
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
