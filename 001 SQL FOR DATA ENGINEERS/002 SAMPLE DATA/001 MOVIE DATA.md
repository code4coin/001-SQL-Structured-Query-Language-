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
DROP TABLE IF EXISTS movie_ratings;
DROP TABLE IF EXISTS movie_actors;
DROP TABLE IF EXISTS actors;
DROP TABLE IF EXISTS directors;
DROP TABLE IF EXISTS movies;

CREATE TABLE movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    director_id INT,
    release_year INT,
    country VARCHAR(50),
    runtime_minutes INT
);

CREATE TABLE directors (
    director_id INT PRIMARY KEY,
    director_name VARCHAR(100),
    nationality VARCHAR(50)
);

CREATE TABLE actors (
    actor_id INT PRIMARY KEY,
    actor_name VARCHAR(100),
    nationality VARCHAR(50)
);

CREATE TABLE movie_actors (
    movie_id INT,
    actor_id INT,
    PRIMARY KEY (movie_id, actor_id),
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id),
    FOREIGN KEY (actor_id) REFERENCES actors(actor_id)
);

CREATE TABLE movie_ratings (
    rating_id INT PRIMARY KEY,
    movie_id INT,
    user_name VARCHAR(50),
    rating FLOAT,
    review_date DATE,
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id)
);
-- Directors
INSERT INTO directors (director_id, director_name, nationality) VALUES
(1, 'Christopher Nolan', 'UK'),
(2, 'Steven Spielberg', 'USA'),
(3, 'Quentin Tarantino', 'USA'),
(4, 'Ridley Scott', 'UK'),
(5, 'James Cameron', 'Canada');

-- Movies
INSERT INTO movies (movie_id, title, genre, director_id, release_year, country, runtime_minutes) VALUES
(1, 'Inception', 'Sci-Fi', 1, 2010, 'USA', 148),
(2, 'Interstellar', 'Sci-Fi', 1, 2014, 'USA', 169),
(3, 'Dunkirk', 'War', 1, 2017, 'UK', 106),
(4, 'Jurassic Park', 'Adventure', 2, 1993, 'USA', 127),
(5, 'Schindler''s List', 'History', 2, 1993, 'USA', 195),
(6, 'Pulp Fiction', 'Crime', 3, 1994, 'USA', 154),
(7, 'Kill Bill: Vol. 1', 'Action', 3, 2003, 'USA', 111),
(8, 'Gladiator', 'Action', 4, 2000, 'USA', 155),
(9, 'Alien', 'Horror', 4, 1979, 'USA', 117),
(10, 'Avatar', 'Sci-Fi', 5, 2009, 'USA', 162),
(11, 'Titanic', 'Romance', 5, 1997, 'USA', 195),
(12, 'The Dark Knight', 'Action', 1, 2008, 'USA', 152),
(13, 'Saving Private Ryan', 'War', 2, 1998, 'USA', 169),
(14, 'Once Upon a Time in Hollywood', 'Drama', 3, 2019, 'USA', 161),
(15, 'The Martian', 'Sci-Fi', 4, 2015, 'USA', 144);

-- Actors
INSERT INTO actors (actor_id, actor_name, nationality) VALUES
(1, 'Leonardo DiCaprio', 'USA'),
(2, 'Brad Pitt', 'USA'),
(3, 'Samuel L. Jackson', 'USA'),
(4, 'Matt Damon', 'USA'),
(5, 'Uma Thurman', 'USA'),
(6, 'Tom Hanks', 'USA');

-- Movie_Actors (many-to-many relations)
INSERT INTO movie_actors (movie_id, actor_id) VALUES
(1,1), (2,1), (6,1), (14,1),           -- Leonardo DiCaprio
(2,4), (15,4), (13,4),                 -- Matt Damon
(6,3), (7,3), (12,3),                  -- Samuel L. Jackson
(7,5),                                 -- Uma Thurman
(12,2), (14,2), (15,2),                -- Brad Pitt
(5,6), (13,6);                         -- Tom Hanks

-- Movie_Ratings
INSERT INTO movie_ratings (rating_id, movie_id, user_name, rating, review_date) VALUES
(1, 1, 'user1', 9.0, '2024-01-01'),
(2, 1, 'user2', 9.5, '2024-02-01'),
(3, 2, 'user3', 9.8, '2024-01-05'),
(4, 3, 'user1', 8.0, '2024-03-10'),
(5, 4, 'user2', 8.5, '2024-01-15'),
(6, 5, 'user4', 9.2, '2024-01-10'),
(7, 6, 'user3', 8.9, '2024-04-12'),
(8, 7, 'user1', 8.3, '2024-04-20'),
(9, 8, 'user2', 9.0, '2024-01-08'),
(10, 9, 'user3', 8.4, '2024-02-14'),
(11, 10, 'user4', 9.7, '2024-03-01'),
(12, 11, 'user3', 9.5, '2024-04-05'),
(13, 12, 'user2', 9.4, '2024-01-18'),
(14, 13, 'user1', 9.0, '2024-02-20'),
(15, 15, 'user4', 8.8, '2024-03-25');

----- sql ending
```

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
