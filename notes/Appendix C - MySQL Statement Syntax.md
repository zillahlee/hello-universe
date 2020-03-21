---
tags: [Notebooks/MySQL CC]
title: Appendix C - MySQL Statement Syntax
created: '2020-03-12T06:43:10.060Z'
modified: '2020-03-18T09:12:27.362Z'
---

# Appendix C - MySQL Statement Syntax

> *To help you find the syntax you need when you need it, this appendix lists the syntax for the most frequently used MySQL operations. Each statement starts with a brief description and then displays the appropriate syntax. For added convenience, you'll also find cross-references to the chapters where specific statements are taught.*

When reading statement syntax, remember the following:
- The `|` symbol is used to indicate one of several options, so `NULL|NOT NULL` means specify either NULL or NOT NULL.
- Keywords or clauses contained within square parentheses `[like this]` are optional.
- Not all MySQL statements are listed, nor is every clause and option listed.


## `ALTERE TABLE`

`ALTER TABLE` is used to update the schema of an existing table. To create a new table, use `CREATE TABLE`. See Chapter 21, "Creating and Manipulating Tables," for more information.

```sql
ALTER TABLE tablename
(
    ADD     column          datatype  [NULL|NOT NULL]  [CONSTRAINTS],
    CHANGE  column columns  datatype  [NULL|NOT NULL]  [CONSTRAINTS],
    DROP    column,
    ...
);
```

## `COMMIT`

`COMMIT` is used to write a transaction to the database. See Chapter 26, "Managing Transaction Processing," for more information.

```sql
COMMIT;
```

## `CREATE INDEX`

`CREATE INDEX` is used to create an index on one or more columns. See Chapter 21 for more information.

```sql
CREATE INDEX indexname
ON tablename (column [ASC|DESC], ...);
```

## `CREATE PROCEDURE`

`CREATE PROCEDURE` is used to create a stored procedure. See Chapter 23, "Working with Stored Procedures," for more information.

```sql
CREATE PROCEDURE procedurename( [parameters] )
BEGIN
...
END;
```

## `CREATE TABLE`

`CREATE TABLE` is used to create new database tables. To update the schema of an existing table, use `ALTER TABLE`. See Chapter 21 for more information.
```sql
CREATE TABLE tablename
(
       column    datatype    [NULL|NOT NULL]    [CONSTRAINTS],
       column    datatype    [NULL|NOT NULL]    [CONSTRAINTS],
       ...
);
```

## `CREATE USER`

`CREATE USER` is used to add a new user account to the system. See Chapter 28, "Managing Security," for more information.

```sql
CREATE USER username[@hostname]
[IDENTIFIED BY [PASSWORD] 'password'];
```

## `CREATE VIEW`

`CREATE VIEW` is used to create a new view of one or more tables. See Chapter 22, "Using Views," for more information.

```sql
CREATE [OR REPLACE] VIEW viewname
AS
SELECT ...;
```

## `DELETE`

`DELETE` deletes one or more rows from a table. See Chapter 20, "Updating and Deleting Data," for more information.

```sql
DELETE FROM tablename
[WHERE ...];
```

## `DROP`

`DROP` permanently removes database objects (tables, views, indexes, and so forth). See Chapters 21; 22; 23; 24, "Using Cursors"; 26; and 28 for more information.

```sql
DROP DATABASE|INDEX|PROCEDURE|TABLE|TRIGGER|USER|VIEW
    itemname;
```

## `INSERT`

`INSERT` adds a single row to a table. See Chapter 19, "Inserting Data," for more information.

```sql
INSERT INTO tablename [(columns, ...)]
VALUES(values, ...);
```

## `INSERT SELECT`

`INSERT SELECT` inserts the results of a `SELECT` into a table. See Chapter 19 for more information.

```sql
INSERT INTO tablename [(columns, ...)]
SELECT columns, ... FROM tablename, ...
[WHERE ...];
```

## `ROLLBACK`

`ROLLBACK` is used to undo a transaction block. See Chapter 26 for more information.

```sql
ROLLBACK [ TO savepointname];
```

## `SAVEPOINT`

`SAVEPOINT` defines a savepoint for use with a `ROLLBACK` statement. See Chapter 26 for more information.

```sql
SAVEPOINT sp1;
```

## `SELECT`

`SELECT` is used to retrieve data from one or more tables (or views). See Chapter 4, "Retrieving Data"; Chapter 5, "Sorting Retrieved Data"; and Chapter 6, "Filtering Data," for more basic information. (Chapters 417 all cover aspects of `SELECT`.)

```sql
SELECT columnname, ...
FROM tablename, ...
[WHERE ...]
[UNION ...]
[GROUP BY ...]
[HAVING ...]
[ORDER BY ...];
```

## `START TRANSACTION`

`START TRANSACTION` is used to start a new transaction block. See Chapter 26 for more information.

```sql
START TRANSACTION;
```

## `UPDATE`

`UPDATE` updates one or more rows in a table. See Chapter 20 for more information.

```sql
UPDATE tablename
SET columname = value, ...
[WHERE ...];
```

