---
layout: post
title: "What is SQL"
date: 2026-01-16 00:00:00 +0900
author: kang
categories: [SQL, Fundamental]
tags: [SQL, Python, DataBase, DBMS, RDMS, Overview]
pin: false
math: true
mermaid: true
---

# Part 1 – SQL & RDBMS Fundamentals

## Database Concepts, Modeling, System Design, and Integration

This document demonstrates my understanding of **relational databases, schema design, system architecture, and real-world integration patterns**.
It focuses not only on SQL syntax, but on **how databases are designed and used in production systems**.

---

## 1. Introduction

Databases are a core component of almost every modern system.
Understanding how data is structured, managed, and integrated with applications is essential for building **scalable, reliable, and maintainable systems**.

This section covers:

* DB / DBMS concepts
* RDBMS architecture
* Database modeling
* System design lifecycle
* Advanced column types
* Indexing concepts
* Python integration
* Comparison with Excel and Python

---

## 2. What is a Database and DBMS?

![DB-vs-DBMS](/assets/img/develop/2026-01-16-DB-vs-DBMS.png)

### Database

A **database** is an organized collection of data stored and accessed **electronically**.

### DBMS (Database Management System)

A **DBMS** is software that manages databases and provides:

* Multi-user access
* Large-scale data handling
* Transaction control
* Data integrity and security

Key characteristics:

* Concurrent access
* High-volume data processing
* Recovery and consistency

---

## 3. Major DBMS Vendors

| DBMS       | Company    |
| ---------- | ---------- |
| MySQL      | Oracle     |
| MariaDB    | MariaDB    |
| PostgreSQL | PostgreSQL |
| Oracle DB  | Oracle     |
| SQL Server | Microsoft  |
| DB2        | IBM        |
| Access     | Microsoft  |
| SQLite     | SQLite     |

In practice, **MySQL, PostgreSQL, and SQL Server** are most commonly used in production systems.

---

## 4. Types of Database Models

### 4.1 Hierarchical DBMS (Legacy)

* Tree structure
* Rigid and difficult to scale

### 4.2 Network DBMS

* Many-to-many relationships
* Complex structure

### 4.3 Relational DBMS (RDBMS)

* Data stored in tables (rows and columns)
* Relationships defined using keys
* Standard in enterprise systems

➡️ **Most modern systems use RDBMS.**

---

## 5. SQL (Structured Query Language)

SQL is used to:

* Query data
* Insert, update, delete records
* Define schemas
* Control access

Why SQL matters:

* Declarative language
* Optimised by query planners
* Portable across systems

---

## 6. Database Modeling & Schema Design in Real Projects

### 6.1 What is Database Modeling?

Database modeling is the process of designing:

* Tables
* Columns and data types
* Primary and foreign keys
* Relationships

It defines how real-world business concepts are represented in a system.

---

### 6.2 Why Schema Design Requires Deep Understanding

Good schema design requires understanding:

* Business rules
* Data relationships
* Access patterns
* Future scalability

Poor design leads to:

* Data duplication
* Complex queries
* Performance issues
* High maintenance cost

Good design provides:

* Clean structure
* Simple queries
* Strong integrity
* Long-term stability

---

### 6.3 From Business Requirements to Computer Systems

Typical flow:

1. Understand business requirements
2. Translate to system design
3. Design database schema
4. Implement application logic
5. Integrate and test
6. Deploy and maintain

Databases are a core part of system architecture.

---

### 6.4 Waterfall Model and Database Design

The Waterfall model shows where database design fits.

1. **Requirements Analysis**
2. **System Design**
3. **Implementation (Coding)**
4. **Integration and Testing**
5. **Deployment**
6. **Maintenance**

➡️ Database design belongs in **System Design**, not as an afterthought.

---

### 6.5 Why Database Design is Architecture

Database design affects:

* API structure
* Business logic
* Performance
* Scalability

➡️ **Designing a database is designing the system.**

---

## 7. Keys and Constraints

### Primary Key (PK)

* Uniquely identifies each row
* Cannot be NULL

```sql
PRIMARY KEY (member_id)
```

---

### Foreign Key (FK)

* Enforces relationships

```sql
FOREIGN KEY (member_id) REFERENCES member(member_id)
```

---

### NOT NULL (NN)

```sql
mem_name VARCHAR(10) NOT NULL
```

---

### UNIQUE

```sql
UNIQUE (email)
```

---

### DEFAULT

```sql
status CHAR(1) DEFAULT 'A'
```

---

### CHECK

```sql
CHECK (phone1 IN ('02', '031', '053'))
```

---

## 8. ON UPDATE / ON DELETE Rules

```sql
FOREIGN KEY(member_id) REFERENCES member(member_id)
ON UPDATE CASCADE
ON DELETE CASCADE
```

Benefits:

* Prevents orphan records
* Keeps data consistent
* Reduces manual cleanup logic

---

## 9. Advanced Column Types and Attributes

### 9.1 BINARY / VARBINARY

Stores raw binary data.

```sql
token BINARY(32)
```

Used for:

* Hashes
* Encrypted values
* UUIDs

---

### 9.2 UNSIGNED

Removes negative range.

```sql
age TINYINT UNSIGNED
```

Used for:

* Age
* Count
* Quantity
* IDs

---

### 9.3 ZEROFILL (Legacy)

Pads numbers with leading zeros.

```sql
order_no INT(10) ZEROFILL
```

Note: Deprecated in modern MySQL.

---

### 9.4 AUTO_INCREMENT

Automatically generates sequential values.

```sql
id INT AUTO_INCREMENT PRIMARY KEY
```

---

### 9.5 Generated Columns

Automatically computed columns.

```sql
total INT AS (price * quantity) STORED
```

Benefits:

* Consistency
* Reduced application logic
* Maintainability

---

## 10. Database vs Excel vs Python

### Excel

Strengths:

* Easy
* Quick analysis
* Small datasets

Limitations:

* No integrity
* No concurrency
* Not scalable

---

### Python

Strengths:

* Data processing
* Automation
* Analytics

Limitations:

* In-memory
* Not persistent
* Not safe for multi-user state

---

### Database

Strengths:

* Persistent storage
* Data integrity
* Concurrency
* Scalability

---

### Summary

| Tool     | Role                 |
| -------- | -------------------- |
| Database | Storage & integrity  |
| Python   | Processing & logic   |
| Excel    | Reporting & analysis |

➡️ **Database = source of truth**

➡️ **Python = processing layer**

➡️ **Excel = reporting layer**

---

## 11. Database and Python Integration

Modern systems combine databases and Python.

### Common Patterns

1. CRUD applications
2. Data processing pipelines
3. Automation scripts
4. Backend services

---

### Example: Python + MySQL

```python
import pymysql

conn = pymysql.connect(
    host='127.0.0.1',
    user='root',
    password='0000',
    db='soloDB',
    charset='utf8'
)

cur = conn.cursor()
cur.execute("SELECT * FROM userTable")

for row in cur.fetchall():
    print(row)

conn.close()
```

---

### Web Backend Architecture

```
![Connecting](/assets/img/develop/2026-01-16-Connecting.jpg)
```

---

### Engineering Perspective

* Database = state & integrity
* Python = logic & processing

This separation leads to:

* Clean architecture
* Easier maintenance
* Better scalability

---

## 12. Index, View, and Stored Programs (Concept Overview)

### View

```sql
CREATE VIEW v_member AS
    SELECT mem_id, mem_name, addr FROM member;
```

Used for:

* Security
* Abstraction
* Simplification

---

### Index

```sql
CREATE INDEX idx_member_addr ON member(addr);
```

* Improves search performance
* Trade-off: storage + write cost

---

### Stored Procedure

```sql
CREATE PROCEDURE getMember()
BEGIN
    SELECT * FROM member;
END;
```

Encapsulates business logic in DB.

---

## 13. Why This Matters in Real Systems

Good database design:

* Improves performance
* Reduces technical debt
* Simplifies application code
* Enables scalability

As an engineer, I focus on:

* Correct data modeling
* Strong integrity
* Clear architecture
* Performance-aware design

---

## 14. Summary

This section covered:

* RDBMS fundamentals
* Database modeling
* System design lifecycle
* Keys and constraints
* Advanced column types
* Python integration
* Tool comparison
* Architecture perspective

This foundation supports:

* Backend development
* Data engineering
* Scalable system design
