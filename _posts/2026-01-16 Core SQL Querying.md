---
layout: post
title: "Core SQL Querying"
date: 2026-01-16 00:00:00 +0900
author: kang
categories: [SQL, Querying]
tags: [SQL, Python, DataBase, DBMS, RDMS, Overview]
pin: false
math: true
mermaid: true
---

# Part 2 – Core SQL Querying

## SELECT, WHERE, GROUP BY, HAVING, ORDER BY in Real Systems

This section focuses on **core SQL querying techniques used in real production systems**.
Rather than simple syntax examples, it demonstrates **how queries are structured to solve real business problems** and how performance considerations influence query design.

---

## 1. Why Querying Matters

In production systems, SQL queries are used to:

* Retrieve business data
* Generate reports
* Support APIs
* Power dashboards
* Feed analytics pipelines

Poorly written queries lead to:

* Slow response times
* High database load
* Scalability issues

Good queries:

* Are readable
* Are efficient
* Reflect business intent clearly

---

## 2. SELECT – Retrieving Data

The `SELECT` statement is the foundation of all queries.

```sql
SELECT * FROM member;
```

However, in real systems, we avoid `SELECT *` and retrieve only required columns:

```sql
SELECT member_id, member_name, addr FROM member;
```

### Why this matters:

* Reduces network overhead
* Improves performance
* Makes intent clearer

---

## 3. WHERE – Filtering Data

`WHERE` is used to filter rows based on conditions.

### Basic Filtering

```sql
SELECT * FROM member 
    WHERE height >= 180;
```

### Multiple Conditions (AND / OR)

```sql
SELECT * FROM member
    WHERE height >= 180 AND member_cnt > 6;
```

```sql
SELECT * FROM member
    WHERE addr = 'harrisdale' OR addr = 'cockburn';
```

---

### BETWEEN

```sql
SELECT * FROM member
    WHERE height BETWEEN 180 AND 190;
```

---

### IN

```sql
SELECT * FROM member
    WHERE addr IN ('harrisdale', 'cockburn', 'success');
```

---

### LIKE (Pattern Matching)

```sql
SELECT * FROM member
    WHERE mem_name LIKE 'Kim%';
```

```sql
SELECT * FROM member
    WHERE mem_name LIKE '__Kang';
```

---

### Engineering Perspective

Filtering at the database level:

* Reduces data transfer
* Improves performance
* Keeps logic close to data

➡️ **Always filter in SQL, not in application code.**

---

## 4. ORDER BY – Sorting Results

Sorting is used to present data in meaningful order.

```sql
SELECT * FROM member
    ORDER BY member_cnt DESC;
```

```sql
SELECT * FROM member
    ORDER BY member_cnt DESC, debut_date ASC;
```

### Why it matters:

* Enables ranking
* Improves UX
* Supports reporting

---

## 5. LIMIT – Controlling Result Size

```sql
SELECT * FROM member LIMIT 3;
```

```sql
SELECT * FROM member
    ORDER BY member_cnt DESC
    LIMIT 3;
```

```sql
SELECT * FROM member
    ORDER BY member_cnt DESC
    LIMIT 3, 2;
```

### Engineering Perspective:

* Essential for pagination
* Prevents large result sets
* Improves API performance

---

## 6. Aggregate Functions – Summarising Data

Aggregate functions are used to compute values across multiple rows.

### Common Functions
()++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

```sql
SUM()
AVG()
MIN()
MAX()
COUNT()
COUNT(DISTINCT)
```

### Examples

```sql
SELECT COUNT(*) FROM member;
```

```sql
SELECT AVG(amount) FROM buy;
```

```sql
SELECT member_id, SUM(amount)
    FROM buy
    GROUP BY member_id;
```

---

## 7. GROUP BY – Grouping Data

`GROUP BY` is used to aggregate data by a specific key.

```sql
SELECT member_id, SUM(price * amount) AS total_amount
    FROM buy
    GROUP BY member_id;
```

### Real-world use cases:

* Total sales per user
* Orders per day
* Revenue per product

---

## 8. HAVING – Filtering Aggregated Data

`HAVING` filters after aggregation.
`WHERE` filters before aggregation.

```sql
SELECT member_id, SUM(price * amount) AS total
    FROM buy
    GROUP BY member_id
    HAVING SUM(price * amount) > 1000;
```

### Combined Example

```sql
SELECT mmember_id, SUM(price * amount) AS total
    FROM buy
    WHERE price >= 50
    GROUP BY member_id
    HAVING total >= 200;
```

---

## 9. Aliasing – Improving Readability

```sql
SELECT member_id AS "Member ID",
       SUM(price * amount) AS "Total Balance"
    FROM buy
    GROUP BY member_id;
```

Aliasing improves:

* Readability
* Report clarity
* API response quality

---

## 10. DISTINCT – Removing Duplicates

```sql
SELECT DISTINCT addr FROM member;
```

Used when:

* Generating dropdowns
* Building filters
* Removing duplicates

---

## 11. Practical Business Scenarios

### Scenario 1 – Top Customers by Spend

```sql
SELECT member_id, SUM(price * amount) AS total_spent
    FROM buy
    GROUP BY member_id
    ORDER BY total_spent DESC
    LIMIT 5;
```

---

### Scenario 2 – Customers with No Orders
()++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```sql
SELECT m.member_id, m.member_name
    FROM member m
        LEFT JOIN buy b ON m.member_id = b.member_id
        WHERE b.member_id IS NULL;
```

---

### Scenario 3 – Average Order Size per Customer

```sql
SELECT mem_id, AVG(amount) AS avg_order_size
    FROM buy
    GROUP BY mem_id;
```

---

## 12. Performance Considerations

### Avoid:

```sql
SELECT * FROM large_table;
```

### Prefer:

```sql
SELECT id, name FROM large_table WHERE status = 'ACTIVE';
```

---

### Index-Aware Queries

```sql
SELECT mem_id, mem_name
FROM member
WHERE mem_name = 'pink';
```

This query uses an index efficiently.

But:

```sql
SELECT mem_id, mem_name
FROM member
WHERE mem_cnt * 2 < 10;
```

This prevents index usage.

➡️ **Write queries that allow index usage.**

---

## 13. Engineering Mindset

When writing queries, I focus on:

* Clear intent
* Minimal data retrieval
* Index-friendly conditions
* Readable structure

Because:

> **SQL is not just a query language – it is part of system design.**

---

## 14. Summary

This section covered:

* SELECT for data retrieval
* WHERE for filtering
* ORDER BY for sorting
* LIMIT for pagination
* Aggregate functions for summarisation
* GROUP BY for grouping
* HAVING for aggregated filtering
* Performance-aware querying

These skills are essential for:

* Backend development
* Data engineering
* Analytics pipelines
* Scalable system design

---

## Next Part

➡️ **Part 3 – Joins & Relationships (INNER, LEFT, RIGHT, SELF, CROSS JOIN)**
Focus on relational thinking and real data modelling.
