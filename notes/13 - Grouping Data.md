---
tags: [Notebooks/MySQL CC]
title: 13 - Grouping Data
created: '2020-03-12T06:36:37.595Z'
modified: '2020-03-17T08:17:44.095Z'
---

# 13 - Grouping Data

> summarizing **subsets** of data

## Data Grouping

Grouping enables you to divide data into logical sets so you can perform aggregate calculations on each group.

## Create Groups: the `GROUP BY` Clause

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id;
```
*sorts the data and group it by vend_id, and calculate num_prods once per vend_id (rather than once for the entire table)*

Because you used `GROUP BY`, you did **not have to specify each group** to be evaluated and calculated. That was **done automatically**. The `GROUP BY` clause instructs MySQL to group the data and then perform the aggregate on each group rather than on the entire result set.

### :bulb: Important `GROUP BY` Rules
- `GROUP BY` clauses can contain as many columns as you want. This enables you to **nest groups**, providing you with more granular control over how data is grouped.
- If you have nested groups in your `GROUP BY` clause, **data is summarized at the last specified group**. In other words, all the columns specified are evaluated together when grouping is established (so you won't get data back for each individual column level).
- Every column listed in `GROUP BY` must be a retrieved column or a valid expression (but not an aggregate function). If an expression is used in the `SELECT`, that same expression must be specified in `GROUP BY`. **Aliases cannot be used.** :question:
- **Aside from the aggregate calculations statements, every column in your `SELECT` statement should be present in the `GROUP BY` clause.**
- If the grouping column contains a row with a NULL value, **NULL will be returned as a group**. If there are multiple rows with NULL values, they'll all be grouped together.
- The `GROUP BY` clause must come **after** any `WHERE` clause and **before** any `ORDER BY` clause.

### Using `ROLLUP`
To obtain values at each group and at a summary level (for each group), use the `WITH ROLLUP` keyword.

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id WITH ROLLUP;
```

## Filter Groups: the `HAVING` Clause
In addition to being able to group data using `GROUP BY`, MySQL also allows you to **filter which groups to include and which to exclude**. For example, you might want a list of all customers who have made at least two orders. To obtain this data you must **filter based on the complete group, not on individual rows**.

> :bulb: **`HAVING` vs. `WHERE`**: `WHERE` does not work here because `WHERE` filters specific rows, not groups. As a matter of fact, `WHERE` has no idea what a group is. `HAVING` is very similar to `WHERE`. The only difference is that ***`WHERE` filters rows and `HAVING` filters groups***. Here's another way to look at it: `WHERE` filters ***before data is grouped***, and `HAVING` filters ***after data is grouped***. This is an important distinction; ***rows that are eliminated by a `WHERE` clause are not included in the group***. This could change the calculated values, which in turn could affect which groups are filtered based on the use of those values in the `HAVING` clause.
>> :bulb: **`HAVING` Supports All of `WHERE`'s Operators**: You've learned about `WHERE` clause conditions (including wildcard conditions and clauses with multiple operators). All the techniques and options you learned about `WHERE` can be applied to `HAVING`. ***The syntax is identical; just the keyword is different.***

```sql
SELECT cust_id, COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```
*counts the number of orders of every customer who has made at least two orders*

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
WHERE prod_price >= 10
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```
*counts the number of $10 and more costly products of every vender who owns at least two such ($10 and more costly) products*

## Grouping & Sorting

`GROUP BY` and `ORDER BY` are very different, even though they often accomplish the same thing.

| `ORDER BY` | `GROUP BY` |
| :--- | :--- |
| Sorts generated output. | Groups rows. The output might not be in group order, however. |
| Any columns (even columns not selected) may be used. | Only selected columns or expressions columns may be used, and every selected column expression must be used. |
| Never required. | Required if using columns (or expressions) with aggregate functions. |

> :exclamation: **Don't Forget `ORDER BY`**: As a rule, anytime you use a `GROUP BY` clause, you should also specify an `ORDER BY` clause. That is the only way to ensure that data is sorted properly. ***Never rely on `GROUP BY` to sort your data.***
>> The first difference above is extremely important. More often than not, you will find that data grouped using `GROUP BY` will indeed be output in group order. But that is not always the case, and it is not actually required by the SQL specifications. Furthermore, you might actually want it sorted differently than it is grouped. Just because you group data one way (to obtain group-specific aggregate values) does not mean that you want the output sorted that same way. You should always provide an explicit `ORDER BY` clause as well, even if it is identical to the `GROUP BY` clause.

```sql
SELECT order_num, SUM(quantity*item_price) AS ordertotal
FROM orderitems
GROUP BY order_num
HAVING SUM(quantity*item_price) >= 50
ORDER BY ordertotal;
```

## `SELECT` Clause Ordering

This is probably a good time to review the order in which `SELECT` statement clauses are to be specified. The table below lists all the clauses you have learned thus far, **in the order they must be used**.

| Clause | Description | Required |
| :--- | :--- | :--- |
| `SELECT` | Columns or expressions to be returned | Yes | 
| `FROM` | Table to retrieve data from | Only if selecting data from a table |
| `WHERE` | Row-level filtering | No | 
| `GROUP BY` | Group specification | Only if calculating aggregates by group |
| `HAVING` | Group-level filtering | No | 
| `ORDER BY` | Output sort order | No | 
| `LIMIT` | Number of rows to retrieve | No | 

