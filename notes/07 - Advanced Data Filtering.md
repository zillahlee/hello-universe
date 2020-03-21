---
tags: [Notebooks/MySQL CC]
title: 07 - Advanced Data Filtering
created: '2020-03-12T04:13:46.758Z'
modified: '2020-03-16T08:32:16.582Z'
---

# 07 - Advanced Data Filtering

## Combine `WHERE` Clauses
> For a greater degree of filter control, MySQL allows you to specify multiple `WHERE` clauses. These clauses may be used in two ways: as `AND` clauses or as `OR` clauses.

> :new: **Operator**: A special keyword used to join or change clauses within a `WHERE` clause. Also known as **logical operators**.

#### `AND` Operator
- A keyword used in a `WHERE` clause to specify that **only rows matching** **all** the specified **conditions** should be retrieved.
```sql
SELECT prod_id, prod_price, prod_name
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;
```

#### `OR` Operator
- A keyword used in a `WHERE` clause to specify that **any rows matching either** of the specified **conditions** should be retrieved.

```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id = 1002 OR vend_id = 1003;
```

#### The Order of Evaluation
- SQL (like most languages) processes `AND` operators ***before*** `OR` operators.
- Whenever you write `WHERE` clauses that use both `AND` and `OR` operators, use **parentheses** to **explicitly** group operators. **Don't ever rely on the default evaluation order, even if it is exactly what you want.** There is no downside to using parentheses, and you are **always better off eliminating any ambiguity**.

## Use the `IN` Operator
- A keyword used in a `WHERE` clause to specify **a list of values to be matched** using an `OR` comparison.
  - *The `IN` operator accomplishes the **same goal** as `OR`.*
-The `IN` operator is used to specify a range of conditions, **any of which can be matched**. IN takes a comma-delimited list of valid values, all enclosed within parentheses.
```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id IN (1002,1003)
ORDER BY prod_name;
```
#### Advantages of Using `IN` Operator
- When you are working with long lists of valid options, the `IN` operator syntax is far cleaner and easier to read.
- The order of evaluation is easier to manage when `IN` is used (as there are fewer operators used).
- IN operators almost always execute more quickly than lists of OR operators.
- The biggest advantage of `IN` is that the `IN` operator can contain another `SELECT` statement, enabling you to build highly dynamic `WHERE` clauses. You'll look at this in detail in [[14 - Sub-Queries.md]].

## Use the `NOT` Operator
- A keyword used in a `WHERE` clause to **negate a condition**.
- The `WHERE` clause's `NOT` operator has one function and **one function only**: it negates whatever condition comes next.
```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id NOT IN (1002,1003)
ORDER BY prod_name;
```

#### Advantages of Using `NOT` Operator
 - For **simple** `WHERE` clauses, there really is **no advantage** to using `NOT`.
 - `NOT` is useful in more **complex clauses**. For example, using `NOT` **in conjunction** with an `IN` operator makes it simple to find all rows that do not match a list of criteria.

> :exclamation: **MySQL** supports the use of `NOT` to negate `IN`, `BETWEEN`, and `EXISTS` clauses. This is quite **different from most other DBMSs** that allow `NOT` to be used to negate any conditions.
