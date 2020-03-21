---
tags: [Notebooks/MySQL CC]
title: 06 - Filtering Data
created: '2020-03-12T04:13:31.382Z'
modified: '2020-03-17T14:26:12.189Z'
---

# 06 - Filtering Data

> Database tables usually contain large amounts of data, and you seldom need to retrieve all the rows in a table. More often than not, you'll want to **extract a subset** of the t able's data as needed for specific operations or reports. Retrieving just the data you want involves specifying search ***criteria***, also known as a ***filter*** condition.
>> Within a `SELECT` statement, data is filtered by specifying search criteria in the `WHERE` clause.


## Using the `WHERE` Clause

```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price = 2.50;
```
*retrieves two columns from the products table, but instead of returning all rows, only rows with a prod_price value of 2.50 are returned*
> :exclamation: **`WHERE` Clause Position**: When using both `ORDER BY` and `WHERE` clauses, make sure `ORDER BY` comes after the `WHERE`; otherwise an error will be generated.

#### SQL vs. Application Filtering :no_entry:
Data can also be filtered at the application level. To do this, the SQL SELECT statement retrieves more data than is actually required for the client application, and the client code loops through the returned data to extract just the needed rows.

As a rule, this practice is strongly discouraged. Databases are optimized to perform filtering quickly and efficiently. Making the client application (or development language) do the database's job dramatically impacts application performance and creates applications that cannot scale properly. In addition, if data is filtered at the client, the server has to send unneeded data across the network connections, resulting in a waste of network bandwidth resources.

> This example uses a simple equality test: It checks to see if a column has a specified value, and it filters the data accordingly. But SQL enables you to do more than just test for equality.

## The `WHERE` Clause Operators

| Operator | Description |
| :--- | :--- |
| = | equality |
| <> | non-equality |
| != | non-equality |
| < | less than |
| <= | less than or equal to |
| > | greater than |
| >= | greater than or equal to |
| BETWEEN | beween two specified values |

#### Checking Against a Single Value
```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_name = 'fuses';
```
*By default, MySQL is not case sensitive when performing matches, and so "fuses" and "Fuses" matched.*

```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price <= 10;
```

#### Checking for Non-Matches
```sql
SELECT vend_id, prod_name
FROM products
WHERE vend_id <> 1003;
```
***is the same as***
```sql
SELECT vend_id, prod_name
FROM products
WHERE vend_id != 1003;
```

#### Checking for a Range of Values
`BETWEEN` requires two values: the beginning `AND` end of the range. The `BETWEEN` operator can be used, for example, to check for all products that cost between 5 and 10 or for all dates that fall between specified start and end dates.
```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;
```
> :bulb: `BETWEEN` matches all the values in the range, **including** the specified **range start and end values**.

#### Checking for No Value
When a table is created, the table designer can specify whether individual columns can contain no value. When a column contains no value, it is said to contain a `NULL` value.
> :new: **`NULL`**: No value, as opposed to a field containing 0, or an empty string, or just spaces.

```sql
SELECT cust_id
FROM customers
WHERE cust_email IS NULL;
```
> :exclamation::question: **`NULL` and Nonmatches**
>> You might expect that when you filter to select all rows that do not have a particular value, rows with a `NULL` will be returned. But they will not. Because of the special meaning of unknown, the database does not know whether they match, and so they are not returned when filtering for matches or when filtering for nonmatches.
>
>> When filtering data, make sure to verify that the rows with a `NULL` in the filtered column are really present in the returned data.


