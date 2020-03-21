---
tags: [Notebooks/MySQL CC]
title: 17 - Combine Queries
created: '2020-03-12T06:37:45.325Z'
modified: '2020-03-17T13:19:04.703Z'
---

# 17 - Combine Queries

> combine multiple `SELECT` statements with the `UNION` operator

## Understanding Combined Queries

Most SQL queries contain a single `SELECT` statement that returns data from one or more tables. MySQL also enables you to perform multiple queries (multiple `SELECT` statements) and return the results as **a single query result set**. These combined queries are usually known as **unions or compound queries**.

There are basically two scenarios in which you'd use combined queries:
  - To return **similarly structured data from different tables** in a **single query**
  - To perform **multiple queries** against **a single table** returning the data as one query

> :bulb: **Combining Queries & Multiple WHERE Conditions**: For the most part, combining two queries to the same table accomplishes the same thing as a single query with multiple `WHERE` clause conditions. In other words, **any `SELECT` statement with multiple `WHERE` clauses can also be specified as a combined query**, as you'll see in the section that follows. The **performance** of each of the two techniques, however, can **vary** based on the queries used. As such, it is always good to experiment to determine which is preferable for specific queries.

## Create Combined Queries

SQL queries are combined using the `UNION` operator. Using `UNION`, multiple `SELECT` statements can be specified, and their results can be combined into a single result set.

### Using `UNION`

Using `UNION` is simple enough. All you do is specify each `SELECT` statement and place the keyword UNION between each.

```sql
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002);
```
*The preceding statements are made up of both of the previous `SELECT` statements separated by the `UNION` keyword. `UNION` instructs MySQL to execute both `SELECT` statements and combine the output into a single query result set.*

***achieves the same result as***
```sql
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
  OR vend_id IN (1001,1002);
```

In this simple example, the `UNION` might actually be more complicated than using a `WHERE` clause. But with more complex filtering conditions, or if the data is being retrieved from multiple tables (and not just a single table), the `UNION` could have made the process much simpler.

### `UNION` Rules

As you can see, unions are very easy to use. But a few rules govern **exactly which can be combined**:
  - A `UNION` must be comprised of two or more `SELECT` statements, each separated by the keyword `UNION` (so, if combining four `SELECT` statements, three `UNION` keywords would be used).
  - **Each query** in a `UNION` **must contain the same columns, expressions, or aggregate functions** (*although columns need not be listed in the same order*).
  - **Column datatypes must be compatible**: They *need not be the exact same type*, but they *must be of a type that MySQL can implicitly convert* (for example, different numeric types or different date types).

Aside from these basic rules and restrictions, unions can be used for any data retrieval tasks.

### Including/Eliminating Duplicate Rows

The `UNION` **automatically removes any duplicate rows from the query result set** (in other words, it *behaves just as multiple `WHERE` clause conditions in a single `SELECT` would*).

This is the **default** behavior of `UNION`, but **you can change this** if you so desire. If you do, in fact, want all occurrences of all matches returned, you can **use `UNION ALL`** instead of `UNION`.

```sql
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION ALL
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002);
```

> :bulb: **`UNION` versus `WHERE`**: The beginning of this chapter said that `UNION` almost always accomplishes the same thing as multiple `WHERE` conditions. *`UNION ALL` is the form of `UNION` that accomplishes what **cannot be done** with `WHERE` clauses.* If you do, in fact, want all occurrences of matches for every condition (**including duplicates**), you must use `UNION ALL` and not `WHERE`.

### Sorting Combined Query Results

`SELECT` statement output is sorted using the `ORDER BY` clause. When combining queries with a `UNION`, **only one `ORDER BY` clause may be used**, and it **must occur after the final `SELECT` statement**. There is very little point in sorting part of a result set one way and part another way, and so multiple `ORDER BY` clauses are not allowed.

```sql
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002)
ORDER BY vend_id, prod_price;
```

This `UNION` takes a single `ORDER BY` clause after the final `SELECT` statement. **Even though the `ORDER BY` appears to only be a part of that last `SELECT` statement, MySQL will in fact use it to sort all the results returned by all the `SELECT` statements.**

### Combining Different Tables

For the sake of simplicity, all of the examples in this chapter combined queries using the same table. However, everything you learned here also applies to using `UNION` to combine queries of different tables.

