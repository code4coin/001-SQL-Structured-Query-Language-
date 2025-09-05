# SQL PLATFORM PREPARATION
---
## EXECUTE THIS FIRST
Below are the table structure and data, which will be utilized for the sql exercises, and learning new concepts.
Therefore, access the given platform and execute sql queries from the section "SQL QUERIES TO EXECUTE" to create tables with sample data.

---

## PLATFORM
We will utilize Programiz online SQL Complier website to create these tables 
- Practice SQL commands online using **(for PostgreSQL database - Recommended)** [Aiven pg compiler](https://aiven.io/tools/pg-playground?utm_source=chatgpt.com)
- Practice SQL commands online using **(for MYSQL database)**: [Programiz SQL Online Compiler](https://www.programiz.com/sql/online-compiler)

---


## SQL QUERIES
1. Below are sample sql queries for your understanding (enable understading of creating and inserting data in a table)
2. Open one of the platform from above mentioned links
3. Copy and paste below queries to create and insert data in a database table**

```sql
DROP TABLE IF EXISTS Checkouts;
DROP TABLE IF EXISTS Patrons;
DROP TABLE IF EXISTS employees;
DROP TABLE IF EXISTS people;
DROP TABLE IF EXISTS books;
DROP TABLE IF EXISTS movies;

-- 1. Create Patrons Table
CREATE TABLE Patrons (
    card_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    join_year INT,
    fines DECIMAL(5,2) DEFAULT 0.00
);

-- 2. Create Checkouts Table
CREATE TABLE Checkouts (
    checkout_id INT PRIMARY KEY,
    card_id INT,
    book_title VARCHAR(150) NOT NULL,
    checkout_date DATE,
    return_date DATE,
    FOREIGN KEY (card_id) REFERENCES Patrons(card_id)
);


-- 3. Create employees table
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    dept_id INT,
    name VARCHAR(100),
    age INT,
    year_hired INT
);

-- 4. Create people table
CREATE TABLE people (
    id INTEGER PRIMARY KEY,
    name TEXT,
    birthdate DATE,
    email TEXT
);

-- 5. Create books table
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    dept_id INT,
    title VARCHAR(255),
    borrow_date DATE,
    return_date DATE
);

-- 6. create movie table
CREATE TABLE movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    director VARCHAR(100),
    release_year INT,
    country VARCHAR(50),
    runtime_minutes INT,
    rating FLOAT
);

------------------------------------------------------------------------------------
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

-- -- Insert data into books
INSERT INTO books (book_id, dept_id, title, borrow_date, return_date) VALUES
(101, 101, 'The Great Gatsby', '2025-08-01', '2025-08-15'),
(102, 102, '1984', '2025-08-03', '2025-08-17'),
(103, 102, 'To Kill a Mockingbird', '2025-08-05', '2025-08-19'),
(104, 101, 'Pride and Prejudice', '2025-08-02', '2025-08-16'),
(105, 102, 'Moby Dick', '2025-08-04', '2025-08-18');

-- Insert into movie table
INSERT INTO movies (movie_id, title, genre, director, release_year, country, runtime_minutes, rating) VALUES
(1,  'Inception',                'Science Fiction', 'Christopher Nolan', 2010, 'USA',    148, 8.8),
(2,  'Spirited Away',            'Animation',       'Hayao Miyazaki',    2001, 'Japan',  125, 8.6),
(3,  'Parasite',                 'Thriller',        'Bong Joon-ho',      2019, 'South Korea', 132, 8.5),
(4,  'The Dark Knight',          'Action',          'Christopher Nolan', 2008, 'USA',    152, 9.0),
(5,  'Interstellar',             'Science Fiction', 'Christopher Nolan', 2014, 'USA',    169, 8.7),
(6,  'La La Land',               'Musical',         'Damien Chazelle',   2016, 'USA',    128, 8.0),
(7,  'The Grand Budapest Hotel', 'Comedy',          'Wes Anderson',      2014, 'Germany',  99, 8.1),
(8,  'Oldboy',                   'Thriller',        'Park Chan-wook',    2003, 'South Korea', 120, 8.4),
(9,  'Amélie',                   'Romance',         'Jean-Pierre Jeunet',2001, 'France', 122, 8.3),
(10, 'Your Name',                'Animation',       'Makoto Shinkai',    2016, 'Japan',  106, 8.4);

----- sql ending
```

---
## **MORE RESOURCES** 🔗

Stay connected and explore more content:

## 🔗 Resources & Links
- 📕 [Download Ebook](https://code4coin.gumroad.com/)
- 🎥 [YouTube](https://www.youtube.com/@code4coin)
- 💼 [LinkedIn](https://www.linkedin.com/in/nitin22/)
- 📸 [Instagram](https://www.instagram.com/code4coin/)
  
---
