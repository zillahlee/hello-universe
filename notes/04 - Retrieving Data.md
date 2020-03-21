---
tags: [Notebooks/MySQL CC]
title: 04 - Retrieving Data
created: '2020-03-12T04:13:00.940Z'
modified: '2020-03-16T10:39:45.689Z'
---

# 04 - Retrieving Data

> :exclamation: **Terminating Statements**: *Multiple SQL statements* must be separated by semicolons (the ; character). MySQL (like most DBMSs) does not require that a semicolon be specified after **single statement**s. Of course, you can always add a semicolon if you wish. It'll do no harm, even if it isn't needed. If you are using the mysql command-line client, the semicolon is always needed

> :exclamation: **SQL Statements and Case**: It is important to note that SQL statements are not case sensitive, so SELECT is the same as select, which is the same as Select. Many SQL developers find that using uppercase for all SQL keywords and lowercase for column and table names makes code easier to read and debug. However, be aware that while the SQL language is not case sensitive, identifiers (the names of databases, tables, and columns) might be. In MySQL 4.1 and earlier, identifiers were case sensitive by default, and as of MySQL 4.1.1, identifiers are not case sensitive by default. As a best practice, **pick a case convention, and use it consistently**.

> :exclamation: **Use of White Space**: All extra white space within a SQL statement is ignored when that statement is processed. SQL statements can be specified on one long line or broken up over many lines. Most SQL developers find that **breaking up statements over multiple lines** makes them easier to read and debug.

> :exclamation: **Presentation of Data**: SQL statements typically return raw, unformatted data. **Data formatting is a presentation issue, not a retrieval issue.** Therefore, presentation (for example, alignment and displaying the price values as currency amounts with the currency symbol and commas) is **typically specified in the application that displays the data**. Actual raw retrieved data (without application-provided formatting) is rarely displayed as is.


## The `SELECT` Statement

Two pieces of info needed to retrieve data
- `SELECT` *what information you want*
- `FROM` *where the information is stored*

## Retrieve Individual Columns

```sql
SELECT column1
FROM table1;
```

## Retrieve Multiple Columns

```sql
SELECT column1, column3, column6
FROM table2;
```

## Retrive All Columns

```sql
SELECT *
FROM table1;
```
*here, `*` is a wildcard*
> :exclamation: As a rule, you are **better off not using the `*` wildcard** unless you really do need every column in the table. Even though use of wildcards might save you the time and effort needed to list the desired columns explicitly, retrieving unnecessary columns usually *slows down the performance of your retrieval and your application*.
>> :bulb: There is one big advantage to using wildcards. As you do not explicitly specify column names (because the asterisk retrieves every column), it is possible to retrieve columns whose names are unknown.

## Retrieve Distinct Rows

```sql
SELECT DISTINCT col4
FROM table2;
```
> :exclamation: **Can't Be Partially `DISTINCT`**: The `DISTINCT` keyword **applies to all columns, not just the one it precedes**. If you were to specify `SELECT DISTINCT vend_id, prod_price`, all rows would be retrieved unless both of the specified columns were distinct.

## Limiting Results

```sql
SELECT col1
FROM table3
LIMIT 5;
```
*return no more than 5 rows*

*To get the next five rows, specify both where to start and the number of rows to retrieve:*
```sql
SELECT col1
FROM table3
LIMIT 5,5;
```
*`LIMIT 5,5` instructs MySQL to return five rows starting from row 5. The first number is **where to start**, and the second is the **number of rows to retrieve**.*

> :exclamation: **Row 0**: The first row retrieved is row 0, not row 1. As such, `LIMIT 1,1` will retrieve the second row, not the first one.

> :exclamation: **When There Aren't Enough Rows**: The number of rows to retrieve specified in `LIMIT` is the maximum number to retrieve. If there aren't enough rows (for example, you specified `LIMIT 10,5`, but there were only 13 rows), MySQL returns **as many as it can**.

> :exclamation: **MySQL 5 LIMIT Syntax**: Does `LIMIT 3,4` mean 3 rows starting from row 4, or 4 rows starting from row 3? As you just learned, it means 4 rows starting from row 3, but it is a bit ambiguous.
>> For this reason, MySQL 5 supports an **alternative syntax** for LIMIT. **LIMIT 4 OFFSET 3** means to get 4 rows starting from row 3, **just like LIMIT 3,4**.

## Use Fully Qualified Table Names

```sql
SELECT table2.col1
FROM database9.table2
```
- functionally identical
- There are situations where fully qualified names are required
