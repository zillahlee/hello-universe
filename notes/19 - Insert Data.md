---
tags: [Notebooks/MySQL CC]
title: 19 - Insert Data
created: '2020-03-12T06:39:18.386Z'
modified: '2020-03-17T16:10:56.964Z'
---

# 19 - Insert Data

> manipulate your table contents with the `INSERT` statement

## Understanding Data Insertion

`SELECT` is undoubtedly the most frequently used SQL statement (which is why the past 17 chapters were dedicated to it). But there are three other frequently used SQL statements that you should learn. The first one is `INSERT`. (You'll get to the other two in the next chapter.)

As its name suggests, `INSERT` is used to insert (add) rows to a database table. `INSERT` can be used in several ways:
  - To insert a **single complete row**
  - To insert a **single partial row**
  - To insert **multiple rows**
  - To insert the **results of a query**

> **`INSERT` and System Security**: Use of the `INSERT` statement can be disabled per table or per user using MySQL security, as will be explained in [[28 - Manage Security]].

## `INSERT` Complete Rows

The simplest way to insert data into a table is to use the basic `INSERT` syntax, which requires that you **specify the table name and the values to be inserted into the new row**.

> :exclamation: **No Output**: `INSERT` statements usually generate no output.

```sql
INSERT INTO customers
VALUES(NULL,
   'Pep E. LaPew',
   '100 Main Street',
   'Los Angeles',
   'CA',
   '90046',
   'USA',
   NULL,
   NULL);
```
*The preceding example inserts a new customer into the customers table. The data to be stored in each table column is specified in the `VALUES` clause, and a value must be provided for every column. If a column has no value (for example, the cust_contact and cust_email columns), the NULL value should be used (assuming the table allows no value to be specified for that column). The columns must be populated in the order in which they appear in the table definition. The first column, cust_id, is also NULL. This is because that column is automatically incremented by MySQL each time a row is inserted. You'd not want to specify a value (that is MySQL's job), and nor could you omit the column (as already stated, every column must be listed), and so a NULL value is specified (it is ignored by MySQL, which inserts the next available cust_id value in its place).*

:no_entry: Although **the above syntax** is indeed simple, it is **not at all safe** and should **generally be avoided at all costs**. The previous SQL statement is **highly dependent on the order in which the columns are defined in the table**. It also **depends on information about that order being readily available**. Even if it is available, there is no guarantee that the columns will be in the exact same order the next time the table is reconstructed. **Therefore, writing SQL statements that depend on specific column ordering is very unsafe. If you do so, something will inevitably break at some point.**

The safer (and unfortunately more cumbersome) way to write the `INSERT` statement is as follows:

```sql
INSERT INTO customers(cust_name,
   cust_address,
   cust_city,
   cust_state,
   cust_zip,
   cust_country,
   cust_contact,
   cust_email)
VALUES('Pep E. LaPew',
   '100 Main Street',
   'Los Angeles',
   'CA',
   '90046',
   'USA',
   NULL,
   NULL);
```
*This example does the exact same thing as the previous `INSERT` statement, but this time the **column names are explicitly stated in parentheses after the table name**. When the row is inserted MySQL will match each item in the columns list with the appropriate value in the `VALUES` list. The first entry in `VALUES` corresponds to the first specified column name. The second value corresponds to the second column name, and so on.*

Because column names are provided, the `VALUES` must match the specified column names in the order in which they are specified, and not necessarily in the order that the columns appear in the actual table. **The advantage of this is that, even if the table layout changes, the `INSERT` statement will still work correctly.** You'll also notice that the NULL for cust_id was not needed, the cust_id column was not listed in the column list and so no value was needed.

```sql
INSERT INTO customers(cust_name,
   cust_contact,
   cust_email,
   cust_address,
   cust_city,
   cust_state,
   cust_zip,
   cust_country)
VALUES('Pep E. LaPew',
   NULL,
   NULL,
   '100 Main Street',
   'Los Angeles',
   'CA',
   '90046',
   'USA');
```
*The above `INSERT` statement populates all the row columns (just as before), but it does so in a different order. Because the column names are specified, the insertion will work correctly.*

> :bulb: **Always Use a Columns List**: As a rule, never use `INSERT` without explicitly specifying the column list. This will greatly increase the probability that your SQL will continue to function in the event that table changes occur.

**Using this syntax, you can also omit columns.** This means you only provide values for some columns, but not for others. (You've actually already seen an example of this; cust_id was omitted when column names were explicitly listed).

> :exclamation: **Omitting Columns**: You may omit columns from an `INSERT` operation ***if the table definition so allows***. One of the following conditions must exist:
>> 1. The column is defined as allowing NULL values (no value at all).
>> 2. A default value is specified in the table definition. This means the default value will be used if no value is specified.
>
>> If you omit a value from a table that does not allow NULL values and does not have a default, MySQL generates an error message, and the row is not inserted.

> :exclamation: **Use `VALUES` Carefully**: Regardless of the `INSERT` syntax being used, the correct number of `VALUES` must be specified. If no column names are provided, a value must be present for every table column. If columns names are provided, a value must be present for each listed column. If none is present, an error message will be generated, and the row will not be inserted.

> :bulb: **Improving Overall Performance**: Databases are frequently accessed by multiple clients, and it is MySQL's job to manage which requests are processed and in which order. `INSERT` operations can be time consuming (especially if there are many indexes to be updated), and this can hurt the performance of `SELECT` statements that are waiting to be processed.
>> If data retrieval is of utmost importance (as is usually is), you can instruct MySQL to ***lower the priority of your `INSERT` statement*** by adding the keyword `LOW_PRIORITY` in between `INSERT` and `INTO`, like this: `INSERT LOW_PRIORITY INTO`.
>
>> Incidentally, this also applies to the `UPDATE` and `DELETE` statements that you'll learn about in the next chapter.


## `INSERT` Multiple Rows

`INSERT` inserts a single row into a table. But what if you needed to insert multiple rows? You could simply use multiple `INSERT` statements, and could even submit them all at once, each terminated by a semicolon.

Or, as long as the column names (and order) are identical in each `INSERT`, you could combine the statements.

```sql
INSERT INTO customers(cust_name,
   cust_address,
   cust_city,
   cust_state,
   cust_zip,
   cust_country)
VALUES(
        'Pep E. LaPew',
        '100 Main Street',
        'Los Angeles',
        'CA',
        '90046',
        'USA'
     ),
      (
        'M. Martian',
        '42 Galaxy Way',
        'New York',
        'NY',
        '11213',
        'USA'
   );
```
*Here a single `INSERT` statement has multiple sets of values, each enclosed within parentheses, and separated by commas.*

> :bulb: **Improving `INSERT` Performance**: This technique can improve the performance of your database possessing, as MySQL will process multiple insertions in a single `INSERT` **faster** than it will multiple `INSERT` statements.

## `INSERT` Retrieved Data

`INSERT` is usually used to add a row to a table using specified values. There is another form of `INSERT` that can be used to insert the result of a `SELECT` statement into a table. This is known as `INSERT SELECT`, and, as its name suggests, it is made up of an `INSERT` statement and a `SELECT` statement.

Suppose you want to merge a list of customers from another table into your customers table. Instead of reading one row at a time and inserting it with `INSERT`, you can do the following:

> :exclamation: *The following example imports data from a table named 'custnew' into the 'customers' table. **To try this example, create and populate the 'custnew' table first.** The format of the 'custnew' table should be the same as the 'customers' table described in Appendix B, "The Example Tables." When populating 'custnew', be sure not to use cust_id values that were already used in 'customers' (the subsequent `INSERT` operation will fail if primary key values are duplicated), or just omit that column and have MySQL generate new values during the import process.*

```sql
INSERT INTO customers(cust_id,
    cust_contact,
    cust_email,
    cust_name,
    cust_address,
    cust_city,
    cust_state,
    cust_zip,
    cust_country)
SELECT cust_id,
    cust_contact,
    cust_email,
    cust_name,
    cust_address,
    cust_city,
    cust_state,
    cust_zip,
    cust_country
FROM custnew;
```
*This example uses `INSERT SELECT` to import all the data from 'custnew' into 'customers'. Instead of listing the `VALUES` to be inserted, the `SELECT` statement retrieves them from 'custnew'. Each column in the `SELECT` corresponds to a column in the specified columns list. How many rows will this statement insert? That depends on how many rows are in the 'custnew' table. If the table is empty, no rows will be inserted (and no error will be generated because the operation is still valid). If the table does, in fact, contain data, all that data is inserted into 'customers'.*

*This example imports cust_id (and assumes that you have ensured that cust_id values are not duplicated). You could also simply omit that column (from both the `INSERT` and the `SELECT`) so MySQL would generate new values.*

> :bulb: **Column Names**: in `INSERT SELECT` This example uses the same column names in both the `INSERT` and `SELECT` statements for simplicity's sake. But there is ***no requirement that the column names match***. In fact, MySQL does not even pay attention to the column names returned by the `SELECT`. Rather, the ***column position*** is used, so the first column in the `SELECT` (regardless of its name) will be used to populate the first specified table column, and so on. This is very useful when importing data from tables that use different column names.

The `SELECT` statement used in an `INSERT SELECT` can include a `WHERE` clause to filter the data to be inserted.

> :bulb: **More Examples**: Looking for more examples of `INSERT` use? See the example table population scripts used to create the example tables used in this book.

