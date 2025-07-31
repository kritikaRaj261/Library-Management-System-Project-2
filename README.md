# Library-Management-System-Project-2
**Project Title** Library Management System-Project-2
**Level** Intermediate
**Database** library_db
This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

## Objectives
**1.Set up the Library Management System Database**: Create and populate the database with tables for branches, employees, members, books, issued status, and return status
**2.CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
**3.CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.

**Database Creation***: Created a database named library_db.
```sql
CREATE DATABASE library_db
use library_db
```
**Table Creation**: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.
```sql
DROP TABLE IF EXISTS branch;
CREATE TABLE Branch
(
            branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(30),
            contact_no VARCHAR(15)
);

-- CREATE TABLE EMPLOYEE---
DROP TABLE IF EXISTS employees;
CREATE TABLE employee
(
            emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
);


-- Create table "Member"
DROP TABLE IF EXISTS member;
CREATE TABLE Member
(
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(30),
            reg_date DATE
);

-- CREATE BOOK TABLE ---
DROP TABLE IF EXISTS Book;

CREATE TABLE Book (
    isbn VARCHAR(100) PRIMARY KEY,
    book_title VARCHAR(100),
    category VARCHAR(100),
    rental_price DECIMAL(10,2),
    status VARCHAR(100),
    author VARCHAR(100),
    publisher VARCHAR(100)
);

-- -- Create table "IssueStatus"---
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES member(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employee(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES book(isbn) 
);

-- Create table "ReturnStatus"
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES book(isbn)
);
```
