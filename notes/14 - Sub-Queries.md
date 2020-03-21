---
tags: [Notebooks/MySQL CC]
title: 14 - Sub-Queries
created: '2020-03-12T06:36:55.005Z'
modified: '2020-03-17T09:05:15.832Z'
---

# 14 - Sub-Queries

The most common uses for subqueries are in `WHERE` clauses, in `IN` operators, and for populating calculated columns.

## Sub-Queries

> :new: **Query**: Any SQL statement. However, the term is usually used to refer to `SELECT` statements.

`SELECT` statements are SQL queries. All the `SELECT` statements you have seen thus far are **simple** queries: single statements retrieving data from individual database tables.

SQL also enables you to create subqueries: queries that are **embedded** into other queries.

## Filter by Sub-Query

Now suppose you wanted a list of all the customers who ordered item TNT2. What would you have to do to retrieve this information? Here are the steps:
  1. Retrieve the order numbers of all orders containing item TNT2.
  2. Retrieve the customer ID of all the customers who have orders listed in the order numbers returned in the previous step.
  3. Retrieve the customer information for all the customer IDs returned in the previous step.

Each of these steps can be executed as a **separate query**. By doing so, you use the results returned by one `SELECT` statement to populate the `WHERE` clause of the next `SELECT` statement.

```sql
SELECT order_num
FROM orderitems
WHERE prod_id = 'TNT2';
-- -> 20005, 20007

SELECT cust_id
FROM orders
WHERE order_num IN (20005, 20007);
-- -> 10001, 10004

SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (10001, 10004);
```

You can also use subqueries to combine all three queries into one single statement. Subqueries are always processed **starting with the innermost** `SELECT` statement and working outward.

```sql
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2'));
```
*To execute this `SELECT` statement, MySQL had to actually perform three `SELECT` statements. The innermost subquery returned a list of order numbers that were then used as the `WHERE` clause for the subquery above it. That subquery returned a list of customer IDs that were used as the `WHERE` clause for the top-level query. The top-level query actually returned the desired data.*

> :bulb: **Formatting Your SQL**: `SELECT` statements containing subqueries can be difficult to read and debug, especially as they grow in complexity. Breaking up the queries over multiple lines and indenting the lines appropriately as shown here can greatly simplify working with subqueries.

There is no limit imposed on the number of subqueries that can be nested, although in practice you will find that performance tells you when you are nesting too deeply.

Although usually used in conjunction with the `IN` operator, subqueries can also be used to test for equality `=`, non-equality `<>`, and so on.

> :exclamation: **Columns Must Match**: When using a subquery in a `WHERE` clause (as seen here), make sure that the `SELECT` statements have the same number of columns as in the `WHERE` clause. Usually, a single column will be returned by the subquery and matched against a single column, but multiple columns may be used if needed.

> :exclamation: **Subqueries & Performance**: The code shown here works, and it achieves the desired result. However, using subqueries is ***not always the most efficient way*** to perform this type of data retrieval, ***although it might be***. More on this is in the next chapter.

## Use Subqueries As Calculated Fields

Another way to use subqueries is in creating calculated fields.

Suppose you want to display the total number of orders placed by every customer in your customers table. Orders are stored in the orders table along with the appropriate customer ID. To perform this operation, follow these steps:
  1.  Retrieve the list of customers from the customers table.
  2.  For each customer retrieved, count the number of associated orders in the orders table.

```sql
SELECT cust_name,
       cust_state,
       (SELECT COUNT(*)
        FROM orders
        WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name;
```
*orders. orders is a calculated field that is set by a subquery provided in parentheses; that subquery is executed once for every customer retrieved.*

> :new: **Correlated Subquery**: A subquery that refers to the outer query.

`WHERE orders.cust_id = customers.cust_id` tells SQL to compare the cust_id in the orders table to the one currently being retrieved from the customers table. This type of subquery is called a *correlated subquery*. This syntax, the table name and the column name separated by a period (i.e., **fully qualified column names**), must be used whenever there is possible ambiguity about column names. Without fully qualifying the column names, MySQL assumes you are comparing the cust_id in the orders table to itself: `SELECT COUNT(*) FROM orders WHERE cust_id = cust_id;` always returns the total number of orders in the orders table (because MySQL checks to see that every order's cust_id matches itself, which it always does, of course).

> :exclamation: **Always More Than One Solution**: As explained earlier in this chapter, although the sample code shown here works, it is often not the most efficient way to perform this type of data retrieval. You will revisit this example in a later chapter.

> :bulb: **Build Queries with Subqueries Incrementally**: Testing and debugging queries with subqueries can be tricky, particularly as these statements grow in complexity. The safest way to build (and test) queries with subqueries is to do so incrementally, in much ***the same way as MySQL processes them***. *Build and test the innermost query first*. Then *build and test the outer query with hard-coded data*, and only after you have *verified* that it is working, *embed the subquery*. Then *test it again*. And keep *repeating these steps as for each additional query*. This will take just a little longer to construct your queries, but doing so saves you lots of time later (when you try to figure out why queries are not working) and significantly increases the likelihood of them working the first time.

