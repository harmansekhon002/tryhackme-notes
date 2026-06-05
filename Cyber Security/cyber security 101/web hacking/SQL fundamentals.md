# Introduction to Databases (Databases 101)
Databases are organized collections of structured information or data that can be easily accessed, managed, and updated. They are ubiquitous, powering everything from user authentication to social media feeds and e-commerce transactions.

## Types of Databases
While there are multiple implementations, they generally fall into two primary categories:

| Type | Description | Best Use Case |
| :--- | :--- | :--- |
| **Relational (SQL)** | Stores highly structured data in a tabular format. Tables can be linked to form relationships. | When data formatting is consistent and accuracy is critical (e.g., e-commerce transactions, user accounts). |
| **Non-relational (NoSQL)** | Stores data in a flexible, non-tabular format (e.g., JSON-like documents, key-value pairs). | When incoming data structures vary greatly but need to be centralized (e.g., social media content, varied document scans). |

## Relational Database Structure
All data in a relational database is stored in structured tables.

* **Tables:** The overarching container for a specific category of data (e.g., a "Books" table).
* **Columns (Fields):** Define the specific *attributes* needed for each entry (e.g., "ID", "Name", "Published_Date"). Each column strictly enforces a specific **Data Type** (e.g., Strings, Integers, Floats, Times/Dates). If data doesn't match the type, the database rejects it.
* **Rows (Records):** An individual, complete entry in the table containing data that corresponds to the defined columns.

## Relationships & Keys
To connect data across different tables (e.g., linking a "Books" table to an "Authors" table), databases use specific identifier columns called keys.

| Key Type | Function | Constraints |
| :--- | :--- | :--- |
| **Primary Key** | Uniquely identifies a specific record within its own table (e.g., a Matriculation Number or Book ID). | There can only be **one** primary key column per table. Every value in this column must be completely unique. |
| **Foreign Key** | A column in one table that directly references the Primary Key in *another* table, establishing the link/relationship. | There can be **multiple** foreign key columns in a single table, allowing it to connect to several other tables. |
# SQL and Database Management Systems (DBMS)
To practically build and interact with a database, you typically use a Database Management System (DBMS). 

## Database Management System (DBMS)
A software program that acts as the interface between the end user and the database itself. It allows users to create, retrieve, update, and manage the stored data.
* **Examples:** MySQL, Oracle Database, MariaDB (Relational), and MongoDB (Non-relational).

## Structured Query Language (SQL)
SQL is the standard programming language used to query, define, and manipulate data stored specifically within **relational databases**.

### Benefits of SQL & Relational Databases
| Benefit | Description |
| :--- | :--- |
| **Fast** | High processing speeds and efficient storage management allow for the near-instantaneous retrieval of massive datasets. |
| **Easy to Learn** | The syntax relies heavily on plain English, making it highly readable and much easier to pick up than traditional programming languages. |
| **Reliable** | Enforces strict structural rules and data types (as discussed in Databases 101), ensuring a high level of data accuracy and integrity. |
| **Flexible** | Offers powerful querying capabilities that allow users to perform complex data analysis tasks very efficiently. |

## Hands-On: Accessing MySQL via Terminal


To interact with a MySQL database via the command line interface, you must authenticate. Use the following command structure:

```bash
# Connect to MySQL as the 'root' user (-u) and prompt for a password (-p)
mysql -u root -p

# Once prompted, type your password (e.g., tryhackme) and press Enter.
```

# SQL: Database & Table Management (DDL)
These commands are part of Data Definition Language (DDL), which is used to build and modify the overarching structure of your databases and tables before you start inserting actual data.

## Database Statements
Commands used to manage the database container itself.

| Command | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`CREATE DATABASE`** | Creates a brand new database. | `CREATE DATABASE thm_bookmarket_db;` |
| **`SHOW DATABASES`** | Lists all databases present on the server. | `SHOW DATABASES;` |
| **`USE`** | Selects a specific database to interact with. **(Required before running table commands!)** | `USE thm_bookmarket_db;` |
| **`DROP DATABASE`** | Permanently deletes a database and everything inside it. | `DROP DATABASE database_name;` |

## Table Statements
Commands used to create and modify the structured tables within your active database.

### 1. Creating a Table (`CREATE TABLE`)
When creating a table, you must define the columns, their data types, and any specific constraints.

    CREATE TABLE book_inventory (
        book_id INT AUTO_INCREMENT PRIMARY KEY,
        book_name VARCHAR(255) NOT NULL,
        publication_date DATE
    );

**Key Terms in the Example:**
* **`INT`, `VARCHAR(255)`, `DATE`:** Data types (Integer, Text up to 255 characters, and Date format).
* **`AUTO_INCREMENT`:** Automatically assigns the next sequential number (1, 2, 3...) when a new record is added.
* **`PRIMARY KEY`:** Marks this column as the unique identifier for each record.
* **`NOT NULL`:** Enforces a rule that this column cannot be left completely empty.

### 2. Viewing and Modifying Tables
| Command                    | Description                                                                             | Syntax / Example                                 |
| :------------------------- | :-------------------------------------------------------------------------------------- | :----------------------------------------------- |
| **`SHOW TABLES`**          | Lists all tables inside the currently active database.                                  | `SHOW TABLES;`                                   |
| **`DESCRIBE`** (or `DESC`) | Displays the detailed structure of a specific table (fields, types, nullability, keys). | `DESCRIBE book_inventory;`                       |
| **`ALTER TABLE`**          | Modifies the structure of an existing table (e.g., adding a new column).                | `ALTER TABLE book_inventory ADD page_count INT;` |
| **`DROP TABLE`**           | Permanently deletes a specific table and all its stored data.                           | `DROP TABLE table_name;`                         |
# SQL Data Manipulation: Clauses
Clauses are appended to SQL statements (like `SELECT`) to specify criteria, define the data type, and dictate how the output should be retrieved, filtered, or sorted. 

## Overview of Key Clauses
| Clause | Function |
| :--- | :--- |
| **`FROM`** | Specifies the table being accessed. |
| **`WHERE`** | Specifies conditions that individual records must meet *before* grouping. |
| **`DISTINCT`** | Removes duplicate records, returning only unique values. |
| **`GROUP BY`** | Aggregates data from multiple records and groups the results (often used with functions like `COUNT()`). |
| **`ORDER BY`** | Sorts the output in ascending (`ASC`) or descending (`DESC`) order based on a specific column. |
| **`HAVING`** | Filters grouped/aggregated results *after* the aggregation is performed. |

## 1. DISTINCT Clause
Used to avoid duplicate records when querying, returning a clean list of unique values.

    -- Without DISTINCT, 'Ethical Hacking' might appear multiple times.
    -- With DISTINCT, it only appears once:
    SELECT DISTINCT name FROM books;

## 2. GROUP BY Clause
Aggregates data and groups the results. It is highly useful when combined with aggregate functions like `COUNT()`.

    -- This counts how many times each book name appears in the table.
    -- The output will list each unique book and its respective count.
    SELECT name, COUNT(*)
    FROM books
    GROUP BY name;

## 3. ORDER BY Clause
Sorts the records returned by a query.
* **`ASC`:** Ascending order (A-Z, 1-10, Oldest-Newest). *This is usually the default.*
* **`DESC`:** Descending order (Z-A, 10-1, Newest-Oldest).

    -- Sorts the books from oldest published to newest:
    SELECT *
    FROM books
    ORDER BY published_date ASC;

    -- Sorts the books from newest published to oldest:
    SELECT *
    FROM books
    ORDER BY published_date DESC;

## 4. HAVING Clause

Used to filter aggregated data. 
> **Important Note:** The `WHERE` clause cannot be used with aggregate functions (like `COUNT()`). `WHERE` filters individual rows *before* they are grouped, while `HAVING` filters the groups *after* they are aggregated.

    -- This groups books by name, counts them, and THEN filters the final list 
    -- to only show books where the name contains the word 'Hack'.
    SELECT name, COUNT(*)
    FROM books
    GROUP BY name
    HAVING name LIKE '%Hack%';

# SQL Operators: Logic & Comparison
Operators allow you to filter, compare, and precisely retrieve data. They are most commonly used in conjunction with the `WHERE` clause to narrow down your search results.
## 1. Logical Operators
These operators evaluate conditions and return a boolean value (`TRUE` or `FALSE`).

| Operator | Description |
| :--- | :--- |
| **`LIKE`** | Filters for specific text patterns (often using `%` as a wildcard to represent any number of characters). |
| **`AND`** | Returns TRUE only if *all* combined conditions are met. |
| **`OR`** | Returns TRUE if *at least one* of the conditions is met. |
| **`NOT`** | Reverses the boolean value to exclude a specific condition. |
| **`BETWEEN`**| Checks if a value exists within a defined range (inclusive). |

**SQL Examples (Logical):**

    -- LIKE: Finds any book where the description contains the word "guide"
    SELECT * FROM books WHERE description LIKE "%guide%";

    -- AND: Must be Offensive Security AND specifically named Bug Bounty Bootcamp
    SELECT * FROM books WHERE category = "Offensive Security" AND name = "Bug Bounty Bootcamp";

    -- OR: Finds books that mention either Android or iOS in the name
    SELECT * FROM books WHERE name LIKE "%Android%" OR name LIKE "%iOS%";

    -- NOT: Finds books where the description does NOT contain "guide"
    SELECT * FROM books WHERE NOT description LIKE "%guide%";

    -- BETWEEN: Finds books with IDs 2, 3, or 4
    SELECT * FROM books WHERE id BETWEEN 2 AND 4;

## 2. Comparison Operators
These operators compare two values (like text, numbers, or dates) against each other to check if they meet specific criteria.

| Operator | Meaning | Example Usage |
| :--- | :--- | :--- |
| **`=`** | Equal to | `WHERE name = "Designing Secure Software"` |
| **`!=`** | Not equal to | `WHERE category != "Offensive Security"` |
| **`<`** | Less than | `WHERE published_date < "2020-01-01"` |
| **`>`** | Greater than | `WHERE published_date > "2020-01-01"` |
| **`<=`** | Less than or equal to | `WHERE published_date <= "2021-11-15"` |
| **`>=`** | Greater than or equal to | `WHERE published_date >= "2021-11-02"` |

**SQL Examples (Comparison):**

    -- Not Equal To (!=): Finds all books EXCLUDING those in the Offensive Security category
    SELECT * FROM books WHERE category != "Offensive Security";

    -- Greater Than (>): Finds books published strictly after Jan 1, 2020
    SELECT * FROM books WHERE published_date > "2020-01-01";

    -- Less Than or Equal To (<=): Finds books published on or before Nov 15, 2021
    SELECT * FROM books WHERE published_date <= "2021-11-15";

# SQL Functions: String & Aggregate
Functions help streamline queries, manipulate data, and perform calculations directly within the database.

## 1. String Functions
These functions perform operations on text/string data and return a modified value.

| Function | Description |
| :--- | :--- |
| **`CONCAT()`** | Adds two or more strings together (e.g., combining data from different columns into one). |
| **`GROUP_CONCAT()`**| Concatenates data from multiple rows into a single field based on a group. |
| **`SUBSTRING()`** | Retrieves a specific portion of a string, starting at a defined position for a specified length. |
| **`LENGTH()`** | Returns the total number of characters in a string (including spaces and punctuation). |

**SQL Examples (String):**

    -- CONCAT: Combines the 'name' and 'category' columns into a custom sentence
    SELECT CONCAT(name, " is a type of ", category, " book.") AS book_info FROM books;

    -- GROUP_CONCAT: Groups books by category and lists all their titles in a single comma-separated string
    SELECT category, GROUP_CONCAT(name SEPARATOR ", ") AS books 
    FROM books 
    GROUP BY category;

    -- SUBSTRING: Extracts the first 4 characters (the year) from the 'published_date' column
    SELECT SUBSTRING(published_date, 1, 4) AS published_year FROM books;

    -- LENGTH: Calculates exactly how many characters are in each book's name
    SELECT LENGTH(name) AS name_length FROM books;

## 2. Aggregate Functions
These functions combine and summarize values from multiple rows into a single, aggregated result.

| Function | Description |
| :--- | :--- |
| **`COUNT()`** | Returns the total number of records/rows matching the query. |
| **`SUM()`** | Calculates the total sum of all non-NULL numerical values in a specific column. |
| **`MAX()`** | Finds the highest/maximum value within a column (e.g., the most recent date or highest number). |
| **`MIN()`** | Finds the lowest/minimum value within a column (e.g., the earliest date or lowest number). |

**SQL Examples (Aggregate):**

    -- COUNT: Returns the total number of rows inside the books table
    SELECT COUNT(*) AS total_books FROM books;

    -- SUM: Calculates the total combined price of all books
    SELECT SUM(price) AS total_price FROM books;

    -- MAX: Retrieves the most recent (maximum) publication date
    SELECT MAX(published_date) AS latest_book FROM books;

    -- MIN: Retrieves the oldest (minimum) publication date
    SELECT MIN(published_date) AS earliest_book FROM books;