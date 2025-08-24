# OLTP vs OLAP
---
## 🔑KEYWORDS
- **OLTP (Online Transaction Processing)**
- **OLAP (Online Analytical Processing)**
- **Database Design**
- **Transactions**
- **Analytics**
- **Data Warehouse**
- **Real-Time vs Historical Data**
---
## 📖DEFINITION
- **OLTP (Online Transaction Processing)** – A database system optimized for managing day-to-day operations. It handles a large number of short, simple transactions (INSERT, UPDATE, DELETE) efficiently.  
- **OLAP (Online Analytical Processing)** – A database system optimized for analytical queries. It processes complex queries over large volumes of historical data to support decision-making.  
---
## 🧱QUERY FORMAT
**OLTP Example (Transactional System):**
```sql
-- Insert a new book purchase
INSERT INTO Sales (customer_id, book_id, purchase_date, price)
VALUES (101, 202, CURRENT_DATE, 15.99);

-- Update book stock
UPDATE Books
SET stock = stock - 1
WHERE book_id = 202;
```

**OLAP Example (Analytical System):**
```sql
-- Find top 5 best-selling books
SELECT book_id, COUNT(*) AS total_sales
FROM Sales
GROUP BY book_id
ORDER BY total_sales DESC
LIMIT 5;

-- Analyze monthly revenue trends
SELECT DATE_TRUNC('month', purchase_date) AS month, SUM(price) AS revenue
FROM Sales
GROUP BY month
ORDER BY month;
```
---
## 📊 OLTP vs OLAP: Comparison Table

| Feature                | **OLTP** (Online Transaction Processing) | **OLAP** (Online Analytical Processing) |
|-------------------------|------------------------------------------|------------------------------------------|
| **Primary Purpose**     | Running day-to-day operations            | Analyzing data for decision-making       |
| **Users**               | Employees, customers, front-line staff   | Analysts, data scientists, managers      |
| **Data Type**           | Current, real-time transactional data    | Historical, aggregated, consolidated data |
| **Query Type**          | Simple, frequent, fast (INSERT/UPDATE)   | Complex, analytical (aggregations, joins) |
| **Data Volume**         | Smaller, frequently updated              | Larger, less frequently updated           |
| **Performance Needs**   | High-speed transactions                  | High processing power for complex queries |
| **Examples**            | ATM transactions, online booking systems | Sales trend analysis, business reporting |

---
## 💡TIP TO REMEMBER
- **OLTP = Operational, Fast, Real-time (many small transactions)**  
- **OLAP = Analytical, Complex, Historical (fewer but heavier queries)**  
- Think **OLTP = Running the Business** vs **OLAP = Analyzing the Business**.  
---
## 💪EXERCISE
Before moving to the exercises, we need a platform with tables and data.  
For this, we have a setup file available inside the same directory: [CLICK AND EXECUTE FILE FIRST](https://github.com/code4coin/001-SQL-Structured-Query-Language-/blob/main/001%20SQL%20FOR%20DATA%20ENGINEERS/002%20SAMPLE%20DATA/001%20MOVIE%20DATA.md)

### 1. Insert a new customer record into the `Customers` table.
```sql
INSERT INTO Customers (customer_id, name, email, join_date)
VALUES (201, 'Alice Johnson', 'alice@example.com', CURRENT_DATE);
```

### 2. Record a new movie rental transaction in the `Rentals` table.
```sql
INSERT INTO Rentals (rental_id, customer_id, movie_id, rental_date)
VALUES (1001, 201, 501, CURRENT_DATE);
```

### 3. Find the top 3 customers who rented the most movies.
```sql
SELECT customer_id, COUNT(*) AS total_rentals
FROM Rentals
GROUP BY customer_id
ORDER BY total_rentals DESC
LIMIT 3;
```

### 4. Analyze monthly rental trends.
```sql
SELECT DATE_TRUNC('month', rental_date) AS rental_month, COUNT(*) AS total_rentals
FROM Rentals
GROUP BY rental_month
ORDER BY rental_month;
```

---
## 🧠Practise
1. Insert 2 new movies into the `Movies` table with different genres.  
2. Update the stock of one movie by reducing it after a rental.  
3. Write a query to find the top 5 most rented movies.  
4. Find the total revenue generated from rentals per genre.  
5. Identify the customer who has spent the most money on rentals.  

---
## ✅SOLUTIONS
### 1. Insert 2 new movies into the `Movies` table with different genres.
```sql
INSERT INTO Movies (movie_id, title, genre, release_year, stock)
VALUES (601, 'Inception', 'Sci-Fi', 2010, 20),
       (602, 'Titanic', 'Romance', 1997, 15);
```

### 2. Update the stock of one movie by reducing it after a rental.
```sql
UPDATE Movies
SET stock = stock - 1
WHERE movie_id = 601;
```

### 3. Write a query to find the top 5 most rented movies.
```sql
SELECT movie_id, COUNT(*) AS total_rentals
FROM Rentals
GROUP BY movie_id
ORDER BY total_rentals DESC
LIMIT 5;
```

### 4. Find the total revenue generated from rentals per genre.
```sql
SELECT M.genre, SUM(R.price) AS total_revenue
FROM Rentals R
JOIN Movies M ON R.movie_id = M.movie_id
GROUP BY M.genre
ORDER BY total_revenue DESC;
```

### 5. Identify the customer who has spent the most money on rentals.
```sql
SELECT C.customer_id, C.name, SUM(R.price) AS total_spent
FROM Rentals R
JOIN Customers C ON R.customer_id = C.customer_id
GROUP BY C.customer_id, C.name
ORDER BY total_spent DESC
LIMIT 1;
```

---
## 🤝**CONTRIBUTING** 

We welcome contributions! You can:

- Add new SQL exercises  
- Improve existing chapters or examples  
- Share interview questions or projects  

Please open a **pull request** or **issue** to contribute.  

---
## 📄**LICENSE** 

This repository is free to use for learning purposes. Please give credit if used in your projects or materials.  

---
## 🔗**MORE RESOURCES** 

Stay connected and explore more content:

- **LinkedIn:** [https://www.linkedin.com/in/nitin22/](https://www.linkedin.com/in/nitin22/)  
- **YouTube:** [https://www.youtube.com/@code4coin](https://www.youtube.com/@code4coin)  
- **Instagram:** [https://www.instagram.com/code4coin/](https://www.instagram.com/code4coin/)  
