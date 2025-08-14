# Welcome to SQL Library Exercises 📚

This is a beginner-friendly collection of SQL exercises designed to help you master the essentials of working with data.  

You'll start by:

- Retrieving data using **SELECT** statements  
- Filtering, grouping, and joining tables  
- Shaping and transforming data with **calculated columns**, **window functions**, and **CTEs**  
- Manipulating real-world datasets with **INSERT**, **UPDATE**, and **DELETE** while respecting data integrity and transactions  

Each exercise includes clear explanations, annotated solutions, and mini-challenges to reinforce best practices like writing readable code, using indexes wisely, and thinking in sets.  

Whether you're preparing for interviews, automating reports, or building data-driven applications, these exercises will strengthen your understanding of relational logic, improve query performance intuition, and give you practical, repeatable practice to turn raw data into insights.

---

## **Getting Started**

1. The very first file to execute is `01_create_tables.sql`. It will create the **Patrons** and **Checkouts** tables and insert sample data to get you started.  

2. Practice SQL commands online using: [Programiz SQL Online Compiler](https://www.programiz.com/sql/online-compiler)  

3. Explore folders and chapters to learn step by step.  
4. You can practice SQL using any environment:
   - MySQL
   - PostgreSQL
   - SQLite
   - Microsoft SQL Server

---

## **SQL Foundation: Below SQL table will be used across all exercises**

```sql
-- 1. Create Patrons Table
DROP TABLE IF EXISTS Patrons;
CREATE TABLE Patrons (
    card_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    join_year YEAR,
    fines DECIMAL(5,2) DEFAULT 0.00
);

-- 2. Create Checkouts Table
DROP TABLE IF EXISTS Checkouts;
CREATE TABLE Checkouts (
    checkout_id INT PRIMARY KEY,
    card_id INT,
    book_title VARCHAR(150) NOT NULL,
    checkout_date DATE,
    return_date DATE,
    FOREIGN KEY (card_id) REFERENCES Patrons(card_id)
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
