---
tags: [Notebooks/MySQL CC]
title: 05 - Sorting Retrieved Data
created: '2020-03-12T04:13:11.535Z'
modified: '2020-03-16T10:42:03.205Z'
---

# 05 - Sorting Retrieved Data

> :new: **Clause**: SQL statements are made up of clauses, some required and some optional. A clause usually consists of a keyword and supplied data. An example of this is the `SELECT` statement's `FROM` clause, which you saw in the previous chapter.

## Sorting Data

```sql
SELECT prod_name
FROM products
ORDER BY prod_name;
```
- :exclamation: **Sorting by Nonselected Columns**: More often than not, the columns used in an `ORDER BY` clause are ones that were selected for display. However, this is actually not required, and it is perfectly legal to sort data by a column that is not retrieved.

## Sorting by Multiple Columns

```sql
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price, prod_name;
```
- When you are sorting by multiple columns, the **sort sequence is exactly as specified**. In other words, using the output in the previous example, the products are sorted by the `prod_name` column only when multiple rows have the same `prod_price` value. If all the values in the `prod_price` column had been unique, no data would have been sorted by `prod_name`.


## Specifying Sort Direction

```sql
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price DESC;
```
- The opposite of `DESC` is `ASC` (for ascending), which may be specified to sort in ascending order. In practice, however, `ASC` is not usually used because ascending order is the default sequence (and is assumed if neither `ASC` nor `DESC` are specified).

```sql
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price DESC, prod_name;
```
- The `DESC` keyword only applies to the column name that directly precedes it. If you want to sort descending on multiple columns, be sure each column has its own `DESC` keyword.

> :exclamation: **Case Sensitivity and Sort Orders**: When you are sorting textual data, is A the same as a? And does a come before B or after Z? These are not theoretical questions, and the answers depend on how the database is set up.
>> In dictionary sort order, A is treated the same as a, and that is the default behavior in MySQL (and indeed most DBMSs). However, administrators can change this behavior if needed. (If your database contains lots of foreign language characters, this might become necessary.)
>
>> The key here is that, if you do need an alternate sort order, you cannot accomplish it with a simple ORDER BY clause. You must contact your database administrator.

Using a combination of `ORDER BY` and `LIMIT`, it is possible to find the highest or lowest value in a column:
```sql
SELECT prod_price
FROM products
ORDER BY prod_price DESC
LIMIT 1;
```
> :exclamation: **Position of `ORDER BY` Clause** When specifying an `ORDER BY` clause, be sure that it is after the `FROM` clause. If `LIMIT` is used, it must come after `ORDER BY`. Using clauses out of order will generate an error message


