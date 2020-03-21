---
favorited: true
tags: [Mine]
title: 04 - SQL Summary
created: '2020-03-11T18:03:53.205Z'
modified: '2020-03-16T16:26:21.730Z'
---

# 04 - SQL Summary

## Relational Database: Design Steps
1. represent **entities** as tables
2. determine **relationships**: 1-1, 1-N, N-N
3. list **fields** (>=2)
4. identify **keys**: PK, FK
5. determine **data types**

## Relational Database Normalization (normal forms)
- 1NF. all fields contain single values only
- 2NF. (1NF+) each non-key field is determined by the **whole PK**
- 3NF. (2NF+) none non-key field determines another non-key field
- BCNF. every determinant is a key
- 4NF. (in an all-key table) part of the key can determine multiple values of <=1 other (key) field
*fixing a normalization violation -> redesign DB -> 5 steps again*

## Relational Operation
- **restriction**: horizontal slice (some rows/records)
- **projection**: vertical slice (some fields)
- **join**: combine parent & child relations

## SQL Definition
**SQL** = **S**tructured **Q**uery **L**anguage
**CRUD** = **C**reat **R**ead **U**pdate **D**elete

#### SQL as a declarative (non-procedural) language
- **DDL**: data definition language
  *define/modify data structures and objects*
  ```CREATE TABLE sales (purchase_no INT);```
  ```ALTER TABLE sales ADD COLUMN date DATE;```
  ```DROP TABLE customers;``` *table disappears*
  ```RENAME TABLE * TO *;```
  ```TRUNCATE TABLE customers;``` *data disappears -> empty table*
- **DML**: data manipulation language :star:
  ```SELECT ... FROM ... WHERE ...;```
  ```INSERT INTO ... VALUES ...;```
  ```UPDATE ... SET ... WHERE ...;```
  ```DELETE FROM ... WHERE;```
- **DCL**: data control language
  ```GRANT```
  ```REVOKE```
- **TCL**: transaction control language
  ```COMMIT```
  ```ROLLBACK```

## Common Queries

> `-- text` inline comment
> `/* text */` block comment

[[Appendix C - MySQL Statement Syntax]]
#### Select Queries
```sql
SELECT DISTINCT col1, col4, SUM(col2) AS total
FROM table1
<jointype> JOIN table2
ON table1.k = table2.id
WHERE col3 = 'T'
GROUP BY col4
HAVING total > 500
ORDER BY total, col1
LIMIT 10;
```

**`SELECT` clauses in the order they must be used**

| Clause | Description | Required |
| :--- | :--- | :--- |
| `SELECT` | Columns or expressions to be returned | Yes | 
| `FROM` | Table to retrieve data from | Only if selecting data from a table |
| `WHERE` | Row-level filtering | No | 
| `GROUP BY` | Group specification | Only if calculating aggregates by group |
| `HAVING` | Group-level filtering | No | 
| `ORDER BY` | Output sort order | No | 
| `LIMIT` | Number of rows to retrieve | No | 

#### Operators & Keywords

*common table expression (CTE), temp table*
`WITH (query1) AS alias1`

*`%`: any number of CHARs*
`LIKE '%bcd%'`

`AND`, `OR`, `IN`, `BETWEEN`, `IS`, `NOT`
`=`, `>`, `<`, `>=`, `<=`, `!=`

`EXTRACT(DAY FROM DATE) AS day`
`EXTRACT(WEEK FROM DATE) AS week`
`EXTRACT(DAYOFWEEK FROM DATE) AS dayofweek`
`EXTRACT(MONTH FROM DATE) AS month`
`EXTRACT(QUARTER FROM DATE) AS quarter`
`EXTRACT(YEAR FROM DATE) AS year`

#### Aggregate Functions
> to be used with `GROUP BY`

`SUM()`, `AVG()`, `COUNT()`, `COUNT(*)`, `MIN()`, `MAX()`

#### JOIN types
| Join Type | Set Operation |
| ---: | :--- |
| inner join | A ⋂ B |
| left (outer) join | A |
| right (outer) join | B |
| full join | A ⋃ B |
| self join |   |

#### Union Queries
```sql
SELECT age FROM table1
UNION ALL/DISTINCT
SELECT age FROM table2;
```

#### Analytic Window Function

```sql
___ OVER (PARTITION BY *
      ORDER BY *
      ROWS BETWEEN current/unbounded/n preceding
      AND current/unbounded/n following
) AS alias1;
```
- `___` can be any of:
  - **analytic aggregation functions**
    - MIN/MAX(), AVG/SUM(), COUNT()
  - **analytic navigation functions**
    - FIRST/LAST_VALUE, LEAD/LAG() 
  - **analytic numbering functions**
    - ROW_NUMBER(), RANK()

## Data Types
[[Appendix D - MySQL Datatypes.md]]
- string data types (aka. alphanumeric)
  - CHAR(n), 
- numeric data types
  - integer, fixed-point, floating-point
- date & time
  - DATE, DATETIME, TIMESTAMP
- BLOB (Binary Large OBject)
  - files instead of values, e.g., *.jpeg

## Efficient Queries
*with or without query optimizer*
`show_amount_of_data_scanned()`
`show_time_to_run()`
- Do not use `SELECT *`. Only choose the columns you need.
- Read less data, esp. for 1-1 relationships
- Avoid N-N JOINs.

