# SQL Server Database – Complete Documentation

This documentation provides a full and detailed guide about **DATABASE** in SQL Server.

---

# Navigation (Concept-Wise)

Jump directly to any section below:

- [Introduction](#introduction)
- [What Is a Database?](#what-is-a-database)
- [Database Components](#database-components)
  - [Data Files](#data-files)
  - [Log File](#log-file)
  - [Filegroups](#filegroups)
  - [Schemas](#schemas)
- [Database Types](#database-types)
  - [System Databases](#system-databases)
  - [User Databases](#user-databases)
- [Installation](#installation)
- [Usage](#usage)
- [Creating a Database](#creating-a-database)
- [Altering a Database](#altering-a-database)
- [Dropping a Database](#dropping-a-database)
- [Listing Databases](#listing-databases)
- [Database Size](#database-size)
- [Backup and Restore](#backup-and-restore)
- [Database States](#database-states)
- [Database Options](#database-options)
- [Security](#security)
- [Best Practices](#best-practices)
- [License](#license)

---

# Introduction

A **Database** in SQL Server is a structured container that stores user data, metadata, logs, and SQL objects such as tables, views, and stored procedures.

---

# What Is a Database?

A SQL Server database includes:

- Primary data file (.mdf)
- Secondary data files (.ndf)
- Log file (.ldf)
- Filegroups
- Schemas
- Tables, views, procedures, functions

---

# Database Components

## Data Files
- **Primary (.mdf):** Main storage  
- **Secondary (.ndf):** Optional additional storage  

## Log File
Stores transaction details for recovery.

## Filegroups
Logical grouping of data files.

## Schemas
Logical containers for tables and other objects.

---

# Database Types

## System Databases

| Database | Purpose |
|----------|---------|
| master   | Stores metadata and configuration |
| model    | Template for new DBs |
| msdb     | Stores SQL Agent jobs |
| tempdb   | Temporary workspace |

## User Databases
Created by users or applications.

---

# Installation

## Install SQL Server
1. Install SQL Server Developer/Express  
2. Install SSMS  
3. Start SQL Server services  

## Verify Installation
    SELECT @@VERSION;

---

# Usage

To run SQL commands:

1. Open SSMS  
2. Connect to instance  
3. Open a query window  
4. Execute commands  

---

# Creating a Database

## Basic Example
    CREATE DATABASE InventoryDB;

## With File Specifications
    CREATE DATABASE HRDB  
    ON PRIMARY 
    (
        NAME = HRDB_Primary,
        FILENAME = 'C:\SQLData\HRDB_Primary.mdf',
        SIZE = 20MB,
        MAXSIZE = 200MB,
        FILEGROWTH = 5MB
    )
    LOG ON 
    (
        NAME = HRDB_Log,
        FILENAME = 'C:\SQLLogs\HRDB_Log.ldf',
        SIZE = 5MB,
        MAXSIZE = 50MB,
        FILEGROWTH = 5MB
    );

---

# Altering a Database

## Rename a Database
    ALTER DATABASE SalesDB MODIFY NAME = SalesDB_Archive;

## Change Collation
    ALTER DATABASE SalesDB COLLATE SQL_Latin1_General_CP1_CI_AS;

## Add a Data File
    ALTER DATABASE SalesDB 
    ADD FILE (
        NAME = SalesDB_Data2,
        FILENAME = 'D:\MSSQL\SalesDB_Data2.ndf',
        SIZE = 10MB
    );

## Set Read-Only
    ALTER DATABASE SalesDB SET READ_ONLY;

---

# Dropping a Database

## Simple Drop
    DROP DATABASE InventoryDB;

## Force Drop
    ALTER DATABASE InventoryDB SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
    DROP DATABASE InventoryDB;

---

# Listing Databases

    SELECT name, database_id, create_date  
    FROM sys.databases;

---

# Database Size

    EXEC sp_spaceused;

---

# Backup and Restore

## Backup
    BACKUP DATABASE SalesDB 
    TO DISK = 'D:\Backups\SalesDB.bak';

## Restore
    RESTORE DATABASE SalesDB 
    FROM DISK = 'D:\Backups\SalesDB.bak';

---

# Database States

| State            | Description |
|------------------|-------------|
| ONLINE           | Available |
| OFFLINE          | Unavailable |
| RESTORING        | Restore in progress |
| RECOVERING       | Recovery in progress |
| RECOVERY PENDING | Needs recovery |
| SUSPECT          | Possible corruption |
| EMERGENCY        | Admin-only mode |

## Change Database State
    ALTER DATABASE SalesDB SET OFFLINE;
    ALTER DATABASE SalesDB SET ONLINE;

---

# Database Options

## Disable Auto-Shrink
    ALTER DATABASE SalesDB SET AUTO_SHRINK OFF;

## Enable Auto-Close
    ALTER DATABASE SalesDB SET AUTO_CLOSE ON;

## Set Recovery Model
    ALTER DATABASE SalesDB SET RECOVERY FULL;

---

# Security

## Create Login and User
    CREATE LOGIN AdminUser WITH PASSWORD = 'Pass@123';

    USE SalesDB;
    CREATE USER AdminUser FOR LOGIN AdminUser;
    EXEC sp_addrolemember 'db_owner', 'AdminUser';

---

# Best Practices

- Perform regular backups  
- Avoid AUTO_SHRINK  
- Maintain indexes  
- Monitor transaction logs  
- Use correct recovery models  
- Follow least-privilege security  
- Separate data and log disks  

---

# License

This content is free for educational and internal use.
