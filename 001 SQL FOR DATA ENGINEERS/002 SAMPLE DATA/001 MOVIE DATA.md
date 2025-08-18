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
-- DROP TABLES IF EXISTS
DROP TABLE IF EXISTS ratings;
DROP TABLE IF EXISTS movie_actors;
DROP TABLE IF EXISTS actors;
DROP TABLE IF EXISTS directors;
DROP TABLE IF EXISTS movies;

-- CREATE TABLES

CREATE TABLE movies (
    movie_id SERIAL PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    director_id INT,
    release_year INT,
    country VARCHAR(50),
    runtime_minutes INT
);

CREATE TABLE directors (
    director_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    country VARCHAR(50)
);

CREATE TABLE actors (
    actor_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    country VARCHAR(50)
);

CREATE TABLE movie_actors (
    movie_id INT,
    actor_id INT,
    PRIMARY KEY (movie_id, actor_id),
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id),
    FOREIGN KEY (actor_id) REFERENCES actors(actor_id)
);

CREATE TABLE ratings (
    rating_id SERIAL PRIMARY KEY,
    movie_id INT,
    user_name VARCHAR(50),
    rating FLOAT,
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id)
);

-- INSERT DATA

-- Directors (~25)
INSERT INTO directors (name, country) VALUES
('Steven Spielberg','USA'),('Christopher Nolan','UK'),('Bong Joon-ho','South Korea'),
('Hayao Miyazaki','Japan'),('Martin Scorsese','USA'),('Quentin Tarantino','USA'),
('Akira Kurosawa','Japan'),('Satyajit Ray','India'),('Alfonso Cuarón','Mexico'),
('Pedro Almodóvar','Spain'),('Wes Anderson','USA'),('Ang Lee','Taiwan'),
('Ridley Scott','UK'),('James Cameron','Canada'),('David Fincher','USA'),
('Denis Villeneuve','Canada'),('Guillermo del Toro','Mexico'),('Spike Lee','USA'),
('Danny Boyle','UK'),('Paul Thomas Anderson','USA'),('Zhang Yimou','China'),
('Jean-Pierre Jeunet','France'),('Federico Fellini','Italy'),('George Miller','Australia'),
('Peter Jackson','New Zealand');

-- Movies (~40)
INSERT INTO movies (title, genre, director_id, release_year, country, runtime_minutes) VALUES
('Inception','Sci-Fi',2,2010,'USA',148),
('Parasite','Thriller',3,2019,'South Korea',132),
('Spirited Away','Animation',4,2001,'Japan',125),
('Pulp Fiction','Crime',6,1994,'USA',154),
('The Godfather','Crime',5,1972,'USA',175),
('The Dark Knight','Action',2,2008,'USA',152),
('Seven Samurai','Action',7,1954,'Japan',207),
('Life of Pi','Adventure',12,2012,'Taiwan',127),
('Roma','Drama',9,2018,'Mexico',135),
('Amélie','Romance',22,2001,'France',122),
('Schindler''s List','Drama',1,1993,'USA',195),
('The Grand Budapest Hotel','Comedy',11,2014,'USA',99),
('Dune','Sci-Fi',16,2021,'USA',155),
('Avatar','Sci-Fi',14,2009,'USA',162),
('The Revenant','Adventure',15,2015,'USA',156),
('Oldboy','Thriller',3,2003,'South Korea',120),
('Train to Busan','Horror',3,2016,'South Korea',118),
('Slumdog Millionaire','Drama',19,2008,'UK',120),
('Crouching Tiger, Hidden Dragon','Action',12,2000,'Taiwan',120),
('Life is Beautiful','Comedy',24,1997,'Italy',116),
('The Matrix','Sci-Fi',13,1999,'USA',136),
('Interstellar','Sci-Fi',2,2014,'USA',169),
('City of God','Crime',10,2002,'Brazil',130),
('The Pianist','Drama',5,2002,'France',150),
('The Lord of the Rings','Fantasy',25,2001,'New Zealand',178),
('The Shape of Water','Fantasy',17,2017,'Mexico',123),
('Kill Bill','Action',6,2003,'USA',111),
('Psycho','Horror',13,1960,'USA',109),
('La La Land','Musical',11,2016,'USA',128),
('Life of Brian','Comedy',24,1979,'UK',94),
('Hero','Action',21,2002,'China',120),
('The Lives of Others','Drama',22,2006,'Germany',137),
('Pan''s Labyrinth','Fantasy',17,2006,'Spain',118),
('Birdman','Comedy',15,2014,'USA',119),
('Mad Max: Fury Road','Action',25,2015,'Australia',120),
('Gladiator','Action',13,2000,'UK',155),
('Black Panther','Action',16,2018,'USA',134),
('Your Name','Animation',4,2016,'Japan',106),
('Cinema Paradiso','Drama',24,1988,'Italy',155),
('The Irishman','Crime',15,2019,'USA',209),
('Spirited Away 2','Animation',4,2003,'Japan',130),
('The Departed','Crime',15,2006,'USA',151);

-- Actors (~30)
INSERT INTO actors (name, country) VALUES
('Leonardo DiCaprio','USA'),('Brad Pitt','USA'),('Robert De Niro','USA'),
('Tom Hanks','USA'),('Morgan Freeman','USA'),('Jack Nicholson','USA'),
('Ken Watanabe','Japan'),('Song Kang-ho','South Korea'),('Ryuichi Sakamoto','Japan'),
('Saoirse Ronan','UK'),('Emma Watson','UK'),('Daniel Day-Lewis','UK'),
('Kate Winslet','UK'),('Penélope Cruz','Spain'),('Antonio Banderas','Spain'),
('Shah Rukh Khan','India'),('Aamir Khan','India'),('Jet Li','China'),
('Zhang Ziyi','China'),('Tilda Swinton','UK'),('Ryan Gosling','Canada'),
('Hugh Jackman','Australia'),('Cate Blanchett','Australia'),('Rami Malek','USA'),
('Michael Fassbender','Germany'),('Oscar Isaac','Guatemala'),('Bill Murray','USA'),
('Adam Driver','USA'),('Tao Okamoto','Japan'),('Ken Jeong','USA'),('Margot Robbie','Australia');

-- Movie_Actors (~70)
INSERT INTO movie_actors (movie_id, actor_id) VALUES
(1,1),(1,2),(2,8),(2,1),(3,7),(3,9),(4,2),(4,6),(5,3),(5,6),
(6,1),(6,2),(7,7),(7,17),(8,12),(8,20),(9,15),(9,14),(10,11),(10,15),
(11,3),(11,5),(12,26),(12,11),(13,1),(13,20),(14,1),(14,2),(15,3),(15,24),
(16,8),(16,17),(17,8),(17,24),(18,17),(18,10),(19,12),(19,21),(20,12),(20,19),
(21,1),(21,2),(22,10),(22,11),(23,10),(23,19),(24,25),(24,21),(25,17),(25,21),
(26,17),(26,12),(27,1),(27,6),(28,13),(28,11),(29,20),(29,21),(30,7),(30,9),
(31,12),(31,24),(32,1),(32,2),(33,7),(33,9),(34,12),(34,19),(35,1),(35,2),
(36,3),(36,5),(37,8),(37,17),(38,1),(38,2),(39,4),(39,12),(40,7),(40,9);

-- Ratings (~60)
INSERT INTO ratings (movie_id, user_name, rating) VALUES
(1,'User1',9.0),(1,'User2',8.5),(2,'User3',9.5),(2,'User4',8.7),
(3,'User5',9.2),(3,'User6',9.0),(4,'User7',8.9),(4,'User8',9.1),
(5,'User9',9.8),(5,'User10',9.7),(6,'User11',9.0),(6,'User12',8.8),
(7,'User13',9.4),(7,'User14',9.1),(8,'User15',8.7),(8,'User16',8.9),
(9,'User17',9.3),(9,'User18',9.0),(10,'User19',8.8),(10,'User20',8.9),
(11,'User21',9.5),(11,'User22',9.6),(12,'User23',8.9),(12,'User24',9.0),
(13,'User25',9.1),(13,'User26',9.3),(14,'User27',8.8),(14,'User28',8.9),
(15,'User29',8.7),(15,'User30',8.9),(16,'User31',9.0),(16,'User32',8.8),
(17,'User33',8.9),(17,'User34',9.1),(18,'User35',9.0),(18,'User36',8.8),
(19,'User37',8.7),(19,'User38',8.9),(20,'User39',9.2),(20,'User40',9.1),
(21,'User41',9.0),(21,'User42',8.9),(22,'User43',8.7),(22,'User44',8.8),
(23,'User45',9.1),(23,'User46',9.2),(24,'User47',8.8),(24,'User48',8.9),
(25,'User49',8.7),(25,'User50',8.9),(26,'User51',9.0),(26,'User52',8.8),
(27,'User53',9.2),(27,'User54',9.0),(28,'User55',8.9),(28,'User56',8.8),
(29,'User57',8.7),(29,'User58',8.9),(30,'User59',8.8),(30,'User60',9.0);

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
