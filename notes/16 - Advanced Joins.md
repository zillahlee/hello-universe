---
tags: [Notebooks/MySQL CC]
title: 16 - Advanced Joins
created: '2020-03-12T06:37:23.702Z'
modified: '2020-03-17T11:59:40.590Z'
---

# 16 - Advanced Joins

- table aliases
- other join types
- using aggregate functions with joined tables

## Table Aliases

In addition to using aliases for column names and calculated fields, SQL also enables you to alias table names. There are **two primary reasons** to do this:
  1. To **shorten** the SQL syntax
  2. To enable **multiple uses of the same table** within a single `SELECT` statement

```sql
SELECT cust_name, cust_contact
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
  AND oi.order_num = o.order_num
  AND prod_id = 'TNT2';
```
In this example, the table aliases were used only in the `WHERE` clause, but aliases are not limited to just `WHERE`. You can use aliases in the `SELECT` list, the `ORDER BY` clause, and in any other part of the statement as well.

It is also worth noting that table aliases are only used during query execution. *Unlike column aliases, table aliases are never returned to the client.*

## Different Join Types

### Self Joins

As mentioned earlier, one of the primary reasons to use table aliases is to be able to refer to the same table more than once in a single SELECT statement.

Suppose that a problem was found with a product (item id DTNTR), and you therefore wanted to know all of the products made by the same vendor so as to determine if the problem applied to them, too. This query requires that you first find out which vendor creates item DTNTR, and next find which other products are made by the same vendor.

```sql
SELECT prod_id, prod_name
FROM products
WHERE vend_id = (SELECT vend_id
                 FROM products
                 WHERE prod_id = 'DTNTR');
```
***achieves the same result as***
```sql
SELECT p1.prod_id, p1.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id
  AND p2.prod_id = 'DTNTR';
```
*The two tables needed in this query are actually the same table, and so the products table appears in the `FROM` clause twice. Although this is perfectly legal, any references to table products would be **ambiguous** because MySQL could not know to **which instance of the products table** you are referring. To resolve this problem, table aliases are used.*

> :exclamation: **Self Joins Instead of Subqueries**: Self joins are often used to replace statements using subqueries that retrieve data from the same table as the outer statement. Although the end result is the same, ***sometimes*** these joins execute ***far more quickly*** than they do subqueries. It is ***usually worth experimenting with both*** to determine which performs better.

### Natural Joins

Whenever tables are joined, at least one column appears in more than one table (the columns being joined). Standard joins (the inner joins you learned about in the previous chapter) return all data, even multiple occurrences of the same column. **A natural join simply eliminates those multiple occurrences so only one of each column is returned.**

How does it do this? The answer is it doesn't: **you do it**. A natural join is a join in which **you select only columns that are unique**. This is typically done using a wildcard (`SELECT *`) for one table and explicit subsets of the columns for all other tables.

```sql
SELECT c.*, o.order_num, o.order_date,
       oi.prod_id, oi.quantity, oi.item_price
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
  AND oi.order_num = o.order_num
  AND prod_id = 'FB';
```

:joy: *The truth is, every inner join you have created thus far is actually a natural join, and you will probably never even need an inner join that is not a natural join.*

### Outer Joins

Most joins relate rows in one table with rows in another. But occasionally, you want to **include rows that have no related rows**. For example, you might use joins to accomplish the following tasks:
  - Count how many orders each customer placed, including customers who have yet to place an order
  - List all products with order quantities, including products not ordered by anyone
  - Calculate average sale sizes, taking into account customers who have not yet placed an order

In each of these examples, the join includes table rows that have no associated rows in the related table. This type of join is called an outer join.

```sql
SELECT customers.cust_id, orders.order_num
FROM customers LEFT OUTER JOIN orders
 ON customers.cust_id = orders.cust_id;
```
*retrieves a list of all customers, including those who have placed no orders*

When using `OUTER JOIN` syntax you must use the `RIGHT` or `LEFT` keywords to specify the table from which to include all rows (`RIGHT` for the one on the right of `OUTER JOIN`, and `LEFT` for the one on the left). The previous example uses `LEFT OUTER JOIN` to select all the rows from the table on the left in the `FROM` clause (the customers table).

> :bulb: **Outer Join Types**: There are two basic forms of outer joins: the left outer join and the right outer join. The only difference between them is the order of the tables they are relating. In other words, a left outer join can be turned into a right outer join simply by reversing the order of the tables in the `FROM` or `WHERE` clause. As such, the two types of outer join can be used ***interchangeably***, and the decision about which one is used is ***based purely on convenience***.

> :exclamation: **No `*=`**: MySQL does not support the use of the simplified `*=` and `=*` syntax popularized by other DBMSs.

## Joins with Aggregate Functions

Aggregate functions are used to summarize data. Although all the examples of aggregate functions thus far only summarized data from a single table, these functions can also be used with joins.

```sql
SELECT customers.cust_name,
       customers.cust_id,
       COUNT(orders.order_num) AS num_ord
FROM customers INNER JOIN orders
 ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;
```
*counts the number of orders for each customer who has made an order*

```sql
SELECT customers.cust_name,
       customers.cust_id,
       COUNT(orders.order_num) AS num_ord
FROM customers LEFT OUTER JOIN orders
 ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;
```
*includes all customers, even those who have not placed any order*

## Using Join & Join Conditions

Before wrapping up this two-chapter discussion on joins, it is worthwhile to summarize some key points regarding joins and their use:
  - Pay careful attention to the **type of join** being used. More often than not, you'll want an *inner join*, but there are often valid uses for outer joins, too.
  - Make sure you use the **correct join condition**, or you'll return incorrect data.
  - Make sure you **always provide a join condition, or you'll end up with the Cartesian product**.
  - You may include multiple tables in a join and even have different join types for each. Although this is legal and often useful, make sure you **test each join separately before testing them together**. This makes troubleshooting far simpler.

