---
tags: [Notebooks/MySQL CC]
title: 25 - Triggers
created: '2020-03-12T06:40:53.095Z'
modified: '2020-03-18T15:22:46.460Z'
---

# 25 - Triggers

## Understanding Triggers

MySQL statements are executed when needed, as are stored procedures. But what if you wanted **a statement (or statements) to be executed automatically when events occur**? For example:
  - Every time a customer is added to a database table, check that the phone number is formatted correctly and that the state abbreviation is in uppercase.
  - Every time a product is ordered, subtract the ordered quantity from the number in stock.
  - Whenever a row is deleted, save a copy in an archive table.

What all these examples have in common is that they need to be processed automatically whenever a table change occurs. And that is exactly what triggers are. :new: **A trigger is a MySQL statement (or a group of statements enclosed within `BEGIN` and `END` statements) that are automatically executed by MySQL in response to any of these statements**:
  - **`DELETE`**
  - **`INSERT`**
  - **`UPDATE`**

> :exclamation: No other MySQL statements support triggers.

## `CREATE TRIGGER`

When creating a trigger you need to specify four pieces of information:
  - The **unique trigger name**
  - The **table to which the trigger is to be associated**
  - The **action that the trigger should respond to** (`DELETE`, `INSERT`, or `UPDATE`)
  - **When the trigger should be executed** (*before* or *after* processing)

> :bulb: **Keep Trigger Names Unique per Database**: In MySQL 5 trigger names must be unique per table, but not per database. This means that two tables in the same database can have triggers of the same name. This is not allowed in other DBMSs where trigger names must be unique per database, and it is very likely that MySQL will make the naming rules stricter in a future release. As such, it is a good idea to use database-wide unique trigger names now.

```sql
CREATE TRIGGER newproduct AFTER INSERT ON products
FOR EACH ROW SELECT 'Product added';
```
`CREATE TRIGGER` is used to create the new trigger named newproduct. triggers can be executed before or after an operation occurs, and here `AFTER INSERT` is specified so the trigger will execute after a successful `INSERT` statement has been executed. The trigger then specifies `FOR EACH ROW` and the code to be executed for each inserted row. In this example, the text Product added will be displayed once for each row inserted. To test this trigger, use the `INSERT` statement to add one or more rows to products; you'll see the 'Product added' message displayed for each succesful insertion.

> :exclamation: **Only Tables**: Triggers are only supported on tables, not on views (and not on temporary tables).

Triggers are defined per time per event per table, and **only one trigger per time per event per table is allowed**. As such, up to six triggers are supported per table (*before and after each of `INSERT`, `UPDATE`, and `DELETE`*). A single trigger cannot be associated with multiple events or multiple tables, so if you need a trigger to be executed for both `INSERT` and `UPDATE` operations, you'll need to define two triggers.

> :bulb: **When Triggers Fail**: If a `BEFORE` Trigger fails, MySQL will not perform the requested operation. In addition, if either a `BEFORE` trigger or the statement itself fails, MySQL will not exdcute an `AFTER` trigger (if one exists).

## `DROP TRIGGER`

```sql
DROP TRIGGER newproduct;
```

Triggers **cannot be updated or overwritten**. To modify a trigger, it **must be dropped and re-created**.

## Use Triggers

With the basics covered, we will now look at each of the supported trigger types, and the differences between them.

### `INSERT` Triggers

`INSERT` triggers are executed before or after an `INSERT` statement is executed. Be aware of the following:
  - Within `INSERT` trigger code, you can refer to a virtual table named `NEW` to access the rows being inserted.
  - In a `BEFORE INSERT` trigger, the values in `NEW` may also be updated (allowing you to change values about to be inserted).
  - For `AUTO_INCREMENT` columns, `NEW` will contain 0 before and the new automatically generated value after.

Here's an example (a really useful one, actually). `AUTO_INCREMENT` columns have values that are automatically assigned by MySQL. Chapter 21, "Creating and Manipulating Tables," suggested several **ways to determine the newly generated value**, but here is an even better solution:

```mysql
CREATE TRIGGER neworder AFTER INSERT ON orders
FOR EACH ROW SELECT NEW.order_num;
```
The code creates a trigger named 'neworder' that is executed by `AFTER INSERT ON orders`. **When a new order is saved in orders, MySQL generates a new order number and saves it in order_num.**
> :question: doesn't seem to work on MySQL Workbench 8.0?

This trigger simply **obtains this value from NEW.order_num and returns it**. This trigger must be executed by `AFTER INSERT` because before the `BEFORE INSERT` statement is executed, the new order_num has not been generated yet. **Using this trigger for every insertion into orders will always return the new order number.**

To test this trigger, try inserting a new order:
```mysql
INSERT INTO orders(order_date, cust_id)
VALUES(Now(), 10001);
```
*automatically returns the new order_num (20010) of this newly inserted row*

> :bulb: **BEFORE or AFTER?** As a rule, ***use `BEFORE` for any data validation and cleanup*** (you'd want to make sure that the data inserted into the table was exactly as needed). This applies to `UPDATE` triggers, too.

### `DELETE` Triggers

`DELETE` triggers are executed before or after a `DELETE` statement is executed. Be aware of the following:
  - Within `DELETE` trigger code, you can refer to **a virtual table** named `OLD` **to access the rows being deleted**.
  - The values in `OLD` are all **read-only** and cannot be updated.

```mysql
CREATE TRIGGER deleteorder BEFORE DELETE ON orders
FOR EACH ROW
BEGIN
   INSERT INTO archive_orders(order_num, order_date, cust_id)
   VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
END;
```
*Before any order is deleted this trigger will be executed. It used an `INSERT` statement to save the values in `OLD` (the order about to be deleted) into an archive table named 'archive_orders'. (To actually use this example you'll need to create a table named 'archive_orders' with the same columns as 'orders').*

The advantage of using a `BEFORE DELETE` TRigger (as opposed to an `AFTER DELETE` TRigger) is that if, for some reason, the order could not be archived, the `DELETE` itself will be aborted.

> :bulb: **Multi-Statement Triggers**: You'll notice that trigger deleteorder uses `BEGIN` and `END` statements to mark the trigger body. This is actually not necessary in this example, although it does no harm being there. The advantage of using a `BEGIN END` block is that the trigger would then be able to accommodate multiple SQL statements (one after the other within the `BEGIN END` block).

### `UPDATE` Triggers

`UPDATE` triggers are executed before or after an `UPDATE` statement is executed. Be aware of the following:
  - Within `UPDATE` trigger code, you can refer to a virtual table named `OLD` to access the previous (pre-`UPDATE` statement) values and `NEW` to access the new updated values.
  - In a `BEFORE UPDATE` trigger, the values in `NEW` may also be updated (allowing you to change values about to be used in the `UPDATE` statement).
  - The values in `OLD` are all read-only and cannot be updated.

The following example ensures that state abbreviations are always in uppercase (regardless of how they were actually specified in the `UPDATE` statement):

```mysql
CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
```
Obviously, any data cleanup needs to occur in the `BEFORE UPDATE` statement as it does in this example. Each time a row is updated, the value in `NEW.vend_state` (the value that will be used to update table rows) is replaced with `Upper(NEW.vend_state)`.

### More on Triggers

Before wrapping this chapter, here are some important points to keep in mind when using triggers:
  - Trigger support in MySQL 5 is rather rudimentary at best when compared to other DBMSs. There are plans to improve and enhance trigger support in future versions of MySQL.
  - Creating triggers might require special security access. However, trigger execution is automatic. If an `INSERT`, `UPDATE`, or `DELETE` statement may be executed, any associated triggers will be executed, too.
  - **Triggers should be used to ensure data consistency (case, formatting, and so on). The advantage of performing this type of processing in a trigger is that it always happens, and happens transparently, regardless of client application.**
  - One very interesting use for triggers is in **creating an audit trail**. Using triggers it would be very easy to **log changes** (even before and after states if needed) to another table.
  - Unfortunately the `CALL` statement is not supported in MySQL triggers. This means that stored procedures cannot be invoked from within triggers. Any needed stored procedure code would need to be replicated within the trigger itself.

