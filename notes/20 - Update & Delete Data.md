---
tags: [Notebooks/MySQL CC]
title: 20 - Update & Delete Data
created: '2020-03-12T06:39:30.309Z'
modified: '2020-03-17T16:17:16.388Z'
---

# 20 - Update & Delete Data

> further manipulate your table contents the `UPDATE` & `DELETE` statements

## `UPDATE` Data

To update (modify) data in a table the `UPDATE` statement is used. `UPDATE` can be used in two ways:
  - To update **specific rows** in a table
  - To update **all rows** in a table

> :no_entry: **Don't Omit the `WHERE` Clause**: Special care must be exercised when using `UPDATE` because it is ***all too easy to mistakenly update every row in your table***. Please read this entire section on `UPDATE` before using this statement.

> :bulb: **`UPDATE` and Security**: Use of the `UPDATE` statement can be restricted and controlled. More on this in [[28 - Manage Security]].

The `UPDATE` statement is very easy to use; some would say too easy. The basic format of an `UPDATE` statement is made up of three parts:
  - **The table** to be updated
  - **The column names** and **their new values**
  - The **filter condition** that determines **which rows** should be updated

```sql
UPDATE customers
SET cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;
```
*The `UPDATE` statement always begins with the name of the table being updated. In this example, it is the customers table. The `SET` command is then used to assign the new value to a column. The `UPDATE` statement finishes with a `WHERE` clause that tells MySQL which row to update. Without a `WHERE` clause, MySQL would update all the rows in the customers table with this new email address —— definitely not the desired effect.*

```sql
UPDATE customers
SET cust_name = 'The Fudds',
    cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;
```
*When **updating multiple columns**, only a single `SET` command is used, and each column = value pair is separated by a comma. (No comma is specified after the last column.) In this example, columns cust_name and cust_email will both be updated for customer 10005.*

> :bulb: **Using Subqueries in an `UPDATE` Statement**: Subqueries may be used in `UPDATE` statements, enabling you to ***update columns with data retrieved with a `SELECT` statement***. Refer to [[14 - Sub-Queries]] for more information on subqueries and their uses.

> :bulb: **The `IGNORE` Keyword**: If your `UPDATE` statement updates multiple rows and an error occurs while updating one or more of those rows, the entire `UPDATE` operation is cancelled (and any rows updated before the error occurred are restored to their original values). To ***continue processing updates, even if an error occurs***, use the `IGNORE` keyword: `UPDATE IGNORE customers ...`

To **delete a column's value**, you can set it to NULL (assuming the table is defined to allow NULL values).

```sql
UPDATE customers
SET cust_email = NULL
WHERE cust_id = 10005;
```
*Here the NULL keyword is used to save no value to the cust_email column.*

## `DELETE` Data

To delete (remove) data from a table, the `DELETE` statement is used. `DELETE` can be used in two ways:
  - To delete **specific rows** from a table
  - To delete **all rows** from a table

> :no_entry: **Don't Omit the `WHERE` Clause**: Special care must be exercised when using `DELETE` because it is ***all too easy to mistakenly delete every row from your table***. Please read this entire section on `DELETE` before using this statement.

I already stated that `UPDATE` is very easy to use. The good (and bad) news is that `DELETE` is even easier to use.

```sql
DELETE FROM customers
WHERE cust_id = 10006;
```
*This statement should be self-explanatory. `DELETE FROM` requires that you **specify the name of the table** from which the data is to be deleted. The `WHERE` clause filters **which rows are to be deleted**. In this example, only customer 10006 will be deleted. If the `WHERE` clause were omitted, this statement would have deleted every customer in the table.*

`DELETE` takes **no column names or wildcard characters**. `DELETE` **deletes entire rows, not columns**. To delete specific columns use an `UPDATE` statement (as seen earlier in this chapter).

> :bulb: **Table Contents, Not Tables**: The `DELETE` statement deletes rows from tables, even all rows from tables. But `DELETE` never deletes the table itself.

> :bulb: **Faster Deletes**: If you really do want to delete all rows from a table, don't use `DELETE`. Instead, use the `TRUNCATE TABLE` statement that accomplished the same thing but does it much quicker (*`TRUNCATE` actually drops and recreates the table, instead of deleting each row individually*).

## Guidelines for `UPDATE` & `DELETE` Data

The `UPDATE` and `DELETE` statements used in the previous sections all have `WHERE` clauses, and there is a very good reason for this. If you omit the `WHERE` clause, the `UPDATE` or `DELETE` is applied to every row in the table. In other words, if you execute an `UPDATE` without a `WHERE` clause, every row in the table is updated with the new values. Similarly if you execute `DELETE` without a `WHERE` clause, all the contents of the table are deleted.

Here are some best practices that many SQL programmers follow:
  - **Never execute an `UPDATE` or a `DELETE` without a `WHERE` clause** unless you really do intend to update and delete every row.
  - Make sure every table has a **primary key** (refer to [[15 - Joining Tables]]), and **use it as the `WHERE` clause** whenever possible. (You may specify individual primary keys, multiple values, or value ranges.)
  - Before you use a `WHERE` clause with an `UPDATE` or a `DELETE`, **first test it with a `SELECT`** to make sure it is filtering the right records: it is far too easy to write incorrect `WHERE` clauses.
  - Use **database enforced referential integrity** (refer to [[15 - Joining Tables]] for this one, too) so MySQL will not allow the deletion of rows that have data in other tables related to them.

> :exclamation: **Use with Caution**: The ***bottom line*** is that MySQL has ***no Undo button***. Be very careful using `UPDATE` and `DELETE`, or you'll find yourself updating and deleting the wrong data.

