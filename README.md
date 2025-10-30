
# Library Management System (PostgreSQL)

## Overview

This is a simple Library Management System built using PostgreSQL.  
It manages books, authors, and patrons.  

This file contains all SQL commands required to:
- Create tables
- Insert data
- Query data
- Update data
- Delete data
- Run advanced queries

You can run these commands in **pgAdmin 4** or **psql**.

## How to Run

### In pgAdmin 4:
1. Open pgAdmin 4 and connect to your PostgreSQL server.  
2. Create a new database named `librarydb`.  
3. Open the Query Tool for `librarydb`.  
4. Copy and paste the SQL commands below.  
5. Click Execute ▶ to run all commands.  

### In psql:
1. Open Command Prompt or Terminal.  
2. Connect to PostgreSQL: `psql -U postgres`  
3. Connect to your database: `\c librarydb`  
4. Copy and paste the SQL commands below and execute them.

---

-- CREATE TABLES
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    nationality VARCHAR(50),
    birth_year INT,
    death_year INT
);

CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(150),
    author_id INT REFERENCES authors(id) ON DELETE CASCADE,
    genres TEXT[],
    published_year INT,
    available BOOLEAN DEFAULT TRUE
);

CREATE TABLE patrons (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    borrowed_books INT[]
);

-- INSERT SAMPLE DATA
INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);

INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
(1, '1984', 1, ARRAY['Dystopian','Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic','Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian','Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel','Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);

INSERT INTO patrons (id, name, email, borrowed_books) VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1,2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4,5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7,8]);

-- READ QUERIES
SELECT * FROM books;
SELECT * FROM books WHERE title='1984';
SELECT b.* FROM books b JOIN authors a ON b.author_id=a.id WHERE a.name='George Orwell';
SELECT * FROM books WHERE available=TRUE;

-- UPDATE QUERIES
UPDATE books SET available=FALSE WHERE id=1;
UPDATE books SET genres=array_append(genres,'Classic') WHERE id=3;
UPDATE patrons SET borrowed_books=array_append(borrowed_books,3) WHERE id=1;

-- DELETE QUERIES
DELETE FROM books WHERE title='The Catcher in the Rye';
DELETE FROM authors WHERE id=5;

-- ADVANCED QUERIES
SELECT * FROM books WHERE published_year>1950;
SELECT * FROM authors WHERE nationality='American';
UPDATE books SET available=TRUE;
SELECT * FROM books WHERE available=TRUE AND published_year>1950;
SELECT * FROM authors WHERE name ILIKE '%George%';
UPDATE books SET published_year=published_year+1 WHERE published_year=1869;
