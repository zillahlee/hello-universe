---
tags: [Notebooks/MySQL CC]
title: 15 - Joining Tables
created: '2020-03-12T06:37:12.915Z'
modified: '2020-03-17T10:35:50.949Z'
---

# 15 - Joining Tables

## Understanding Joins

One of SQL's most powerful features is the capability to join tables on-the-fly within data retrieval queries. Joins are one of the most important operations you can perform using SQL `SELECT`,

### Relational Tables

Before you can effectively use joins, you must understand relational tables and the basics of relational database design. **Why relational databases?**
  - Saving time and storage space - *efficient storage*
  - Updating with ease - *easier manipulation*
  - Preventing in*consistency* from repetitive manual input
  - Overall, *greater scalability*

The key here is that ***having multiple occurrences of the same data is never a good thing***, and that principle is the ***basis for relational database design***. Relational tables are designed so information is split into multiple tables, one for each data type. The **tables are related to each other through common values** (and thus the relational in relational design).

> :new: **Foreign Key**: A column in one table that contains the primary key values from another table, thus defining the relationships between tables.

The ***bottom line*** is that ***relational data can be stored efficiently and manipulated easily***. Because of this, relational databases *scale* far better than non-relational databases.

> :new: **Scale**: Able to handle an increasing load without failing. A well-designed database or application is said to scale well.

### Why Use Joins?

As just explained, breaking data into multiple tables enables more efficient storage, easier manipulation, and greater scalability. But these **benefits come with a price**: If data is stored in multiple tables, how can you retrieve that data with a single `SELECT` statement?

The answer is to use a join. Simply put, a ***join*** is ***a mechanism used to associate tables within a `SELECT` statement*** (and thus the name join). Using a special syntax, multiple tables can be joined so a single set of output is returned, and the join associates the correct rows in each table on-the-fly.

> :exclamation: **Maintaining Referential Integrity**: It is important to understand that ***a join is not a physical entity***: in other words, it *does not exist in the actual database tables*. A join is created by MySQL as needed, and it persists for the duration of the query execution.
>> When using relational tables, it is important that only valid data is inserted into relational columns. MySQL can be instructed to only allow valid values (ones present in the vendors table) in the vendor ID column in the products table. This is known as maintaining referential integrity, and is achieved by ***specifying the primary and foreign keys as part of the table definitions*** (as will be explained in [[21 - Create & Manipulate Tables]]).

## Create a Join

```sql
SELECT vend_name, prod_name, prod_price
FROM vendors, products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name, prod_name;
```
*two tables are correctly joined with a `WHERE` clause that instructs MySQL to match vend_id in the vendors table with vend_id in the products table*

> :exclamation: **Fully Qualifying Column Names**: You must use the fully qualified column name (table and column separated by a period) whenever there is a possible ambiguity about to which column you are referring. MySQL returns an error message if you refer to an ambiguous column name without fully qualifying it with a table name.

### Importance of the `WHERE` Clause

It might seem strange to use a `WHERE` clause to set the join relationship, but actually, there is a very good reason for this. Remember, when tables are joined in a `SELECT` statement, that relationship is constructed on-the-fly. Nothing in the database table definitions can instruct MySQL how to join the tables. You have to do that yourself. When you join two tables, what you are actually doing is **pairing every row in the first table with every row in the second table**. The `WHERE` clause acts **as a filter to only include rows that match the specified filter condition: the join condition**, in this case. Without the `WHERE` clause, every row in the first table is paired with every row in the second table, regardless of if they logically go together or not.

### Cross Joins

> :new: **Cartesian Product**: The ***results*** returned by a table relationship ***without a join condition***. The number of rows retrieved is the number of rows in the first table multiplied by the number of rows in the second table.
>> :new: **Cross Joins**: Sometimes you'll hear the type of join that returns a Cartesian Product referred to as a cross join.

```sql
SELECT vend_name, prod_name, prod_price
FROM vendors, products
ORDER BY vend_name, prod_name;
```
*the Cartesian product is seldom what you want. The data returned here has matched every product with every vendor, including products with the incorrect vendor (and even vendors with no products at all)*

### Inner Joins

The join you have been using so far is called an ***equijoin***: a join based on the testing of equality between two tables. This kind of join is also called an *inner join*. Equijoin/inner join is the most commonly used form of join.

In fact, you may use a slightly **different syntax** for these joins, **specifying the type of join explicitly**.

```sql
SELECT vend_name, prod_name, prod_price
FROM vendors INNER JOIN products
  ON vendors.vend_id = products.vend_id;
```
*The `SELECT` in the statement is the same as the preceding `SELECT` statement, but the `FROM` clause is different. Here the relationship between the two tables is part of the `FROM` clause specified as `INNER JOIN`. When using this syntax the join condition is specified using the special **`ON` clause instead of a `WHERE` clause**. The **actual condition passed to `ON` is the same as would be passed to `WHERE`**.*

> :bulb: **Which Syntax to Use?**: Per the *ANSI (American National Standards Institute) SQL* specification, use of the `INNER JOIN` syntax is ***preferable***. Furthermore, although using the `WHERE` clause to define joins is indeed simpler, using ***explicit join syntax*** ensures that you will ***never forget the join condition***, and it can affect performance, too (in some cases).

### Joining Multiple Tables

SQL imposes no limit to the number of tables that may be joined in a SELECT statement. The basic rules for creating the join remain the same. First **list all the tables**, and then **define the relationship between each**.

```sql
SELECT prod_name, vend_name, prod_price, quantity
FROM orderitems, products, vendors
WHERE products.vend_id = vendors.vend_id
  AND orderitems.prod_id = products.prod_id
  AND order_num = 20005;
```
*The `FROM` clause here lists the three tables, and the `WHERE` clause defines both of those join conditions. An additional `WHERE` condition is then used to filter just the items for order 20005.*

> :exclamation: **Performance Considerations**: MySQL processes joins at run-time, relating each table as specified. This process can become very ***resource intensive***, so be careful ***not to join tables unnecessarily***. The more tables you join, the more performance degrades.

To revisit [[14 - Sub-Queries]]:

```sql
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2'));
```
***achieves the same result as***
```sql
SELECT cust_name, cust_contact
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num
  AND prod_id = 'TNT2';
```
*Returning the data needed in this query requires the use of **three tables**. But instead of using them within nested subqueries, here two joins are used to connect the tables. There are three `WHERE` clause conditions here. The first two connect the tables in the join, and the last one filters the data for product TNT2.*

> :exclamation: **It Pays to Experiment**: As you can see, there is often more than one way to perform any given SQL operation. And there is rarely a definitive right or wrong way. Performance can be affected by the type of operation, the amount of data in the tables, whether indexes and keys are present, and a whole slew of other criteria. Therefore, it is often worth experimenting with different selection mechanisms to find the one that works best for you.

