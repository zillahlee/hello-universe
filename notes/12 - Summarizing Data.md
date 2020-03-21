---
tags: [Notebooks/MySQL CC]
title: 12 - Summarizing Data
created: '2020-03-12T06:36:25.916Z'
modified: '2020-03-17T09:13:25.893Z'
---

# 12 - Summarizing Data

> with **aggregate functions**

It is often necessary to **summarize data without actually retrieving it all**, and MySQL provides special functions for this purpose. Using these functions, MySQL queries are often used to **retrieve data for analysis and reporting purposes**. Examples of this type of retrieval are
  - Determining the **number of rows** in a table (or the number of rows that *meet some condition or contain a specific value*)
  - Obtaining the **sum of a group of rows** in a table
  - Finding the **highest, lowest, and average values** in a table column (*either for all rows or for specific rows*)

In each of these examples, you want **a summary of the data** in a table, **not the actual data itself**. Therefore, returning the actual table data would be a waste of time and processing resources (not to mention bandwidth). *To repeat, all you really want is the summary information.*

To facilitate this type of retrieval, MySQL features a set of aggregate functions. These functions are designed to be **highly efficient**, and they usually return results far more quickly than you could calculate them yourself within your own client application.

## Aggregate Functions

> :new: **Aggregate Functions**: Functions that **operate on a set of rows** to calculate and **return a single value**.

| `Function` | Description | 
| :--- | :--- |
| `AVG()` | Returns a column's average value | 
| `COUNT()` | Returns the number of rows in a column | 
| `MAX()` | Returns a column's highest value | 
| `MIN()` | Returns a column's lowest value | 
| `SUM()` | Returns the sum of a column's values | 

> :bulb: **Standard Deviation**: A series of standard deviation aggregate functions are also supported by MySQL, but they are not covered in this book.

### `AVG()`
Return the average value of a specific column by counting both the number of rows in the table and the sum of their values. Can be used to return the average value of all columns or of specific columns or rows.

```sql
SELECT AVG(prod_price) AS avg_price
FROM products;
```
***for specific columns or rows***
```sql
SELECT AVG(prod_price) AS avg_price
FROM products
WHERE vend_id = 1003;
```

> :exclamation: **Individual Columns Only**: `AVG()` may only be used to determine the average of a specific numeric column, and that column name must be specified as the function parameter. To obtain the average value of multiple columns, multiple `AVG()` functions must be used.

> :exclamation: **NULL Values**: Column rows containing NULL values are **ignored** by the `AVG()` function.


### `COUNT()`
Help you figure out the number of rows in a table or the number of rows that match a specific criterion. Can be used in two ways:
- Use `COUNT(*)` to count the number of rows in a table, *whether columns **contain values or NULL** values*.
- Use `COUNT(column)` to count the number of rows that have values in a specific column, ***ignoring NULL** values*.

```sql
SELECT COUNT(*) AS num_cust
FROM customers;
```
***differs from***
```sql
SELECT COUNT(cust_email) AS num_cust
FROM customers;
```
*`COUNT(cust_email)` counts only rows with a value in the cust_email column. In this example, cust_email is 3 (meaning that only three of the five customers have email addresses).*

> :exclamation: **NULL Values**: Column rows with NULL values in them are **ignored** by the `COUNT()` function **if a column name is specified**, but **not ignored** if the asterisk **(*)** is used.

### `MAX()` & `MIN()`
Returns the highest and lowest value in a specified column, respectively. Requires that the column name be specified.

```sql
SELECT MAX(prod_price) AS max_price
FROM products;
```

```sql
SELECT MIN(prod_price) AS min_price
FROM products;
```

> :bulb: **Using `MAX()`/`MIN()` with Non-Numeric Data**: Although `MAX()`/`MIN()` are usually used to find the highest numeric or date values, MySQL allows them to be used to return the highest or lowest value in any column including textual columns. When used with textual data, `MAX()` ***returns the row that would be the last if the data were sorted by that column***. When used with textual data, `MIN()` ***returns the row that would be first if the data were sorted by that column***.

> :exclamation: **NULL Values**: Column rows with NULL values in them are **ignored** by the `MAX()` or `MIN()` function.

### `SUM()`
Returns the sum (total) of the values in a specific column.

```sql
SELECT SUM(quantity) AS items_ordered
FROM orderitems
WHERE order_num = 20005;
```
*The orderitems table contains the actual items in an order, and each item has an associated quantity. The above query returns the total number of items ordered (the sum of all the quantity values) by order #20005.*

***`SUM()` can also be used to total calculated values.***
```sql
SELECT SUM(item_price * quantity) AS total_price
FROM orderitems
WHERE order_num = 20005;
```
*returns the sum of all the expanded prices in order #20005*

> :exclamation: **NULL Values**: Column rows with NULL values in them are **ignored** by the `SUM()` function.

> :bulb: **Performing Calculations on Multiple Columns**: *All the aggregate functions* can be used to perform calculations on multiple columns using the standard mathematical operators.

## Aggregates on Distinct Values

The five aggregate functions can all be used in two ways:
- To perform calculations on **all rows**, specify the `ALL` argument, or specify no argument at all (because `ALL` is the **default behavior**).
- To **only include unique values**, specify the `DISTINCT` argument.

```sql
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM products
WHERE vend_id = 1003;
```
*the average only takes into account unique prices*
*in this example avg_price is higher when `DISTINCT` is used because there are multiple items with the same lower price*

> :exclamation: `DISTINCT` may only be used with `COUNT()` if a column name is specified. **`DISTINCT` may not be used with `COUNT(*)`**, and so `COUNT(DISTINCT *)` is not allowed and generates an error. Similarly, `DISTINCT` must be used with a column name and **not with a calculation or expression**.

> :exclamation: Although `DISTINCT` can technically be used with `MIN()` and `MAX()`, there is actually **no value in doing so**. The minimum and maximum values in a column are the same whether or not only distinct values are included.

## Combine Aggregate Functions

All the examples of aggregate functions used thus far have involved a single function. But actually, `SELECT` statements may contain as few or as many aggregate functions as needed.

```sql
SELECT COUNT(*) AS num_items,
       MIN(prod_price) AS price_min,
       MAX(prod_price) AS price_max,
       AVG(prod_price) AS price_avg
FROM products;
```

> :bulb: **Naming Aliases**: When specifying alias names to contain the results of an aggregate function, try to not use the name of an actual column in the table. Although there is nothing actually illegal about doing so, using unique names makes your SQL easier to understand and work with (and troubleshoot in the future).

