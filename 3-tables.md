# SQL Server Tables – Complete Documentation

This document provides a complete and detailed guide about **Tables** in SQL Server, including concepts, structure, creation, modification, constraints, relationships, indexing basics, and best practices.

---

# Navigation (Concept-Wise)

- [Introduction](#introduction)
- [What Is a Table?](#what-is-a-table)
- [Table Structure](#table-structure)
- [Table Components](#table-components)
  - [Columns](#columns)
  - [Data Types](#data-types)
  - [Nullability](#nullability)
  - [Constraints](#constraints)
- [Creating a Table](#creating-a-table)
- [Altering a Table](#altering-a-table)
- [Dropping a Table](#dropping-a-table)
- [Primary Key](#primary-key)
- [Foreign Key](#foreign-key)
- [Unique Constraint](#unique-constraint)
- [Check Constraint](#check-constraint)
- [Default Constraint](#default-constraint)
- [Identity Column](#identity-column)
- [Temporary Tables](#temporary-tables)
- [Table Variables](#table-variables)
- [Listing Tables](#listing-tables)
- [Best Practices](#best-practices)
- [License](#license)

---

# Introduction

A **Table** in SQL Server is a database object used to store structured data in rows and columns. It is the core component of relational database design.

---

# What Is a Table?

A table:

- Stores data in a tabular format  
- Contains rows (records) and columns (fields)  
- Can contain constraints to maintain data integrity  
- Can relate to other tables through keys  

---

# Table Structure

A SQL Server table is defined by:

- **Columns** (field definitions)
- **Data types** (int, varchar, datetime, etc.)
- **Constraints** (PK, FK, CHECK, UNIQUE, DEFAULT)
- **Indexes** (optional)
- **Identity columns** (for auto-increment values)

---

# Table Components

## Columns

Columns define:

- Name  
- Data type  
- Nullability  
- Constraints  

Example column definition:
``` sql
    FirstName VARCHAR(50) NOT NULL
```
---

## Data Types

Common SQL Server data types:

| **Numeric** | **String** | **Date/Time** | **Boolean** |
| ----------- | ----------- | -------------- | ----------- |
| INT | VARCHAR(n) | DATETIME | BIT |
| BIGINT | NVARCHAR(n) | DATE | - |
| SMALLINT | CHAR(n) | TIME | - |
| DECIMAL(p, s) | TEXT *(deprecated)*  | DATETIME2 | - |
| FLOAT | - | - | - |

---

## Nullability

Specifies whether a column can store NULL values.

Examples:
``` sql
    Age INT NULL
    Email VARCHAR(100) NOT NULL
```
---

## Constraints

Constraints enforce rules at the table level:

- PRIMARY KEY  
- FOREIGN KEY  
- UNIQUE  
- CHECK  
- DEFAULT  

---

# Creating a Table

## Basic Example
``` sql
    CREATE TABLE Employees (
        EmployeeID INT,
        FirstName VARCHAR(50),
        LastName VARCHAR(50),
        Salary DECIMAL(10,2)
    );
```
---

## Create Table with Constraints

``` sql
    CREATE TABLE Employees (
        EmployeeID INT PRIMARY KEY,
        FirstName VARCHAR(50) NOT NULL,
        LastName VARCHAR(50) NOT NULL,
        Salary DECIMAL(10,2) CHECK (Salary > 0),
        CreatedDate DATETIME DEFAULT GETDATE()
    );
```
---

# Altering a Table

**Add Column**
``` sql 
ALTER TABLE Employees ADD Email VARCHAR(100); 
``` 
**Modify Column**
``` sql 
ALTER TABLE Employees ALTER COLUMN Salary DECIMAL(12,2); 
```
**Drop Column**
``` sql 
ALTER TABLE Employees ALTER COLUMN Salary DECIMAL(12,2); 
```

---

# Dropping a Table

**Simple Drop**
  ``` sql 
    DROP TABLE Employees;
  ```
**Drop Only if Exists**
  ``` sql 
    DROP TABLE IF EXISTS Employees;
  ``` 
---

# Primary Key

A **Primary Key (PK)** uniquely identifies each row.

**Add PK While Creating Table**
  ``` sql 
    CREATE TABLE Departments (
        DeptID INT PRIMARY KEY,
        DeptName VARCHAR(100)
    );
  ``` 
**Add PK to Existing Table**
  ``` sql 
    ALTER TABLE Employees 
    ADD CONSTRAINT PK_Employees PRIMARY KEY (EmployeeID);
  ``` 
---

# Foreign Key

A **Foreign Key (FK)** creates a relationship between tables.

**Create Table with FK**
  ``` sql 
    CREATE TABLE Orders (
        OrderID INT PRIMARY KEY,
        EmployeeID INT,
        OrderDate DATETIME,
        CONSTRAINT FK_Orders_Employees FOREIGN KEY (EmployeeID)
            REFERENCES Employees(EmployeeID)
    );
  ``` 
**Add FK to Existing Table**
  ``` sql 
    ALTER TABLE Orders
    ADD CONSTRAINT FK_Orders_Employees FOREIGN KEY (EmployeeID)
        REFERENCES Employees(EmployeeID);
  ``` 
---

# Unique Constraint

Ensures all values in a column are unique.

**Add Unique Constraint**
  ``` sql 
    ALTER TABLE Employees
    ADD CONSTRAINT UQ_Email UNIQUE (Email);
  ``` 
---

# Check Constraint

Validates values before they are inserted.

**Create with CHECK**
  ``` sql 
    CREATE TABLE Products (
        ProductID INT PRIMARY KEY,
        Price DECIMAL(10,2),
        CONSTRAINT CHK_Price CHECK (Price > 0)
    );
  ``` 
---

# Default Constraint

Sets a default value when no value is provided.

**Add DEFAULT**
  ``` sql 
    ALTER TABLE Employees
    ADD CONSTRAINT DF_CreatedDate DEFAULT GETDATE() FOR CreatedDate;
  ``` 
---

# Identity Column

Automatically generates incremental values.

**Create Identity Column**
  ``` sql 
    CREATE TABLE Customers (
        CustomerID INT IDENTITY(1,1) PRIMARY KEY,
        CustomerName VARCHAR(100) NOT NULL
    );
  ``` 
---

# Temporary Tables

Stored in **tempdb**.

**Local Temporary Table**
  ``` sql 
    CREATE TABLE #TempData (
        ID INT,
        Value VARCHAR(50)
    );
  ``` 
**Global Temporary Table**
  ``` sql 
    CREATE TABLE ##GlobalTemp (
        ID INT
    );
  ``` 
---

# Table Variables

Stored in memory (with tempdb backing).

**Example**
  ``` sql 
    DECLARE @MyTable TABLE (
        ID INT,
        Name VARCHAR(50)
    );

    INSERT INTO @MyTable VALUES (1, 'John');
  ``` 
---

# Listing Tables

**Get All Tables in a Database**
  ``` sql 
    SELECT * 
    FROM sys.tables;
  ``` 
**Get Tables with Schema**
  ``` sql 
    SELECT TABLE_SCHEMA, TABLE_NAME
    FROM INFORMATION_SCHEMA.TABLES;
  ``` 
---

# Best Practices

- Always define a primary key  
- Use proper data types  
- Avoid using `SELECT *` in production queries  
- Normalize data when appropriate  
- Use identity columns for surrogate keys  
- Add constraints for data integrity  
- Avoid excessive indexes  
- Use NVARCHAR for multilingual support  

---

# License

This documentation is free for educational and internal use.
