# Library-Management-System-Project-2
**Project Title** Library Management System-Project-2
**Level** Intermediate
**Database** library_db
This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

## Objectives
**1.Set up the Library Management System Database**: Create and populate the database with tables for branches, employees, members, books, issued status, and return status
**2.CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
**3.CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.

**Database Creation**: Created a database named library_db.
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
## 2. CRUD Operations
**1.Create**: Inserted sample records into the books table.
**2.Read**: Retrieved and displayed data from various tables.
**3.Update**: Updated records in the employees table.
**4.Delete**: Removed records from the members table as needed.

**Task 1. Create a New Book Record**-- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.'
```sql
select * from book
insert into book (isbn,book_title,category,rental_price,status,author,publisher)
VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')
```
**Task 2: Update an Existing Member's Address**
```sql
select * from member
UPDATE member
SET member_address = '125 Oak St'
WHERE member_id = 'C103';
```
**Task 3: Delete a Record from the Issued Status Table** -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
```sql
 delete from issued_status
 where issued_id='IS121'
```
**Task 4: Retrieve All Books Issued by a Specific Employee** -- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
 SELECT * FROM issued_status
WHERE issued_emp_id = 'E101'
```
**Task 5: List Members Who Have Issued More Than One Book** -- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
SELECT
    issued_emp_id,
    COUNT(issued_id)
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 1
```
## 3. CTAS (Create Table As Select)
**Task 6: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**
```sql
CREATE TABLE book_issued_cnt  AS 
select 
count(issued_id) as Total_bookIssued,
b.isbn,b.book_title
from book b
inner join issued_status st
on b.isbn= st.issued_book_isbn
group by 2,3
```
## 4. Data Analysis & Findings
**Task 7. Retrieve All Books in a Specific Category:**
```sql
SELECT * from BOOK
WHERE category='Classic'
```
**Task 8: Find Total Rental Income by Category:**
```sql
select b.category,
sum(rental_price) as Rental_income
from book b
inner join issued_status st
on b.isbn=st.issued_book_isbn
group by b.category
```
**Task 9: List Members Who Registered in the Last 180 Days:**
```sql
--Method-1
select member_id,member_name
from member
where reg_date >=DATEADD(DAY, -180, GETDATE())

-- Method-2
SELECT member_id, member_name
FROM member
WHERE reg_date >= CURDATE() - INTERVAL 180 DAY;

```
**Task 10:List Employees with Their Branch Manager's Name and their branch details:**
```sql
SELECT 
    e1.emp_id,
    e1.emp_name,
    e1.position,
    e1.salary,
    b.*,
    e2.emp_name as manager
FROM employee as e1
JOIN 
branch as b
ON e1.branch_id = b.branch_id    
JOIN
employee as e2
ON e2.emp_id = b.manager_id
```
**Task 11. Create a Table of Books with Rental Price Above a Certain Threshold:**
```sql

CREATE TABLE rentalPriceThresold 
AS
select * from book
where rental_price>7.0
```
**Task 12: Retrieve the List of Books Not Yet Returned**
```sql
select  DISTINCT issued_book_name
from issued_status st
left join return_status rs
on st.issued_id=rs.issued_id
where return_id is NULL
```
