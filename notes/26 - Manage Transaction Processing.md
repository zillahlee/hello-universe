---
tags: [Notebooks/MySQL CC]
title: 26 - Manage Transaction Processing
created: '2020-03-12T06:41:05.076Z'
modified: '2020-03-18T15:44:34.132Z'
---

# 26 - Manage Transaction Processing

## Transaction Processing

> :exclamation: **Not All Engines Support Transactions**: As explained in [[21 - Create & Manipulate Tables]], MySQL supports the use of several underlying database engines. Not all engines support explicit management of transaction processing, as will be explained in this chapter. The two most commonly used engines are MyISAM and InnoDB. The former does not support explicit transaction management and the latter does. This is why the sample tables used in this book were created to use InnoDB instead of the more commonly used MyISAM. If you need transaction processing functionality in your applications, be sure to use the correct engine type.

Transaction processing is used **to maintain database integrity by ensuring that batches of MySQL operations execute completely or not at all**.

Relational databases are designed so data is stored in multiple tables to facilitate easier data manipulation, management, and reuse. Without going in to the hows and whys of relational database design, take it as a given that well-designed database schemas are relational to some degree.

The orders tables you've been using in prior chapters are a good example of this. Orders are stored in two tables: orders stores actual orders, and orderitems stores the individual items ordered. These two tables are related to each other using unique IDs called primary keys. These tables, in turn, are related to other tables containing customer and product information.

The process of adding an order to the system is as follows:
  1. Check if the customer is already in the database (present in the customers table). If not, add him or her.
  2. Retrieve the customer's ID.
  3. Add a row to the orders table associating it with the customer ID.
  4. Retrieve the new order ID assigned in the orders table.
  5. Add one row to the orderitems table for each item ordered, associating it with the orders table by the retrieved ID (and with the products table by product ID).

Now imagine that some database failure (for example, out of disk space, security restrictions, table locks) prevents this entire sequence from completing. What would happen to your data?

Well, if the failure occurred after the customer was added and before the orders table was added, there is no real problem. It is perfectly valid to have customers without orders. When you run the sequence again, the inserted customer record will be retrieved and used. You can effectively pick up where you left off.

But what if the failure occurred after the orders row was added, but before the orderitems rows were added? Now you'd have an empty order sitting in your database.

Worse, what if the system failed during adding the orderitems rows? Now you'd end up with a partial order in your database, but you wouldn't know it.

How do you solve this problem? That's where transaction processing comes in. :new: **Transaction processing is a mechanism used to manage sets of MySQL operations that must be executed in batches to ensure that databases never contain the results of partial operations.** With transaction processing, you can ensure that sets of operations are not aborted mid-processingthey either execute in their entirety or not at all (unless explicitly instructed otherwise). **If no error occurs, the entire set of statements is committed (written) to the database tables. If an error does occur, a rollback (undo) can occur to restore the database to a known and safe state.**

So, looking at the same example, this is how the process would work:
  1. Check if the customer is already in the database; if not, add him or her.
  2. Commit the customer information.
  3. Retrieve the customer's ID.
  4. Add a row to the orders table.
  5. If a failure occurs while adding the row to orders, roll back.
  6. Retrieve the new order ID assigned in the orders table.
  7. Add one row to the orderitems table for each item ordered.
  8. If a failure occurs while adding rows to orderitems, roll back all the orderitems rows added and the orders row.
  9. Commit the order information.

When working with transactions and transaction processing, there are a few keywords that'll keep reappearing. Here are the terms you need to know:
  - :new: **Transaction** A block of SQL statements
  - :new: **Rollback** The process of *undoing* specified SQL statements
  - :new: **Commit** Writing *unsaved* SQL statements to the database tables
  - :new: **Savepoint** A *temporary placeholder in a transaction set* to which you can issue a rollback (as opposed to rolling back an entire transaction)

## Controll Transactions

Now that you know what transaction processing is, let's look at what is involved in managing transactions.

The key to managing transactions involves **breaking your SQL statements into logical chunks** and **explicitly stating when data should be rolled back and when it should not**.

The MySQL statement used to mark the start of a transaction is

```mysql
START TRANSACTION;
```

### `ROLLBACK`

The MySQL `ROLLBACK` command is used to roll back (undo) MySQL statements.

```mysql
SELECT * FROM ordertotals;
START TRANSACTION;
DELETE FROM ordertotals;
SELECT * FROM ordertotals;
ROLLBACK;
SELECT * FROM ordertotals;
```
This example starts by displaying the contents of the 'ordertotals' table (this table was populated in Chapter 24, "Using Cursors"). First a `SELECT` is performed to show that the table is not empty. Then a transaction is started, and all of the rows in 'ordertotals' are deleted with a `DELETE` statement. Another `SELECT` verifies that, indeed, 'ordertotals' is empty. Then a `ROLLBACK` statement is used to roll back all statements until the `START TRANSACTION`, and the final `SELECT` shows that the table is no longer empty.

Obviously, `ROLLBACK` can only be used within a transaction (after a `START TRANSACTION` command has been issued).

> :bulb: **Which Statements Can You Roll Back?**: Transaction processing is used to manage `INSERT`, `UPDATE`, and `DELETE` statements. You cannot roll back `SELECT` statements. (There would not be much point in doing so anyway.) :no_entry: You ***cannot roll back `CREATE` or `DROP`*** operations. These statements may be used in a transaction block, but if you perform a rollback they will not be undone.

### `COMMIT`

MySQL statements are usually executed and written directly to the database tables. This is known as an :new: **implicit commit**: the commit (write or save) operation happens automatically.

Within a transaction block, however, commits do not occur implicitly. To force an :new: **explicit commit**, the `COMMIT` statement is used.

```mysql
START TRANSACTION;
DELETE FROM orderitems WHERE order_num = 20010;
DELETE FROM orders WHERE order_num = 20010;
COMMIT;
```
In this example, order number 20010 is deleted entirely from the system. Because this involves updating two database tables, orders and orderitems, **a transaction block is used to ensure that the order is not partially deleted**. The final `COMMIT` statement writes the change only if no error occurred. If the first `DELETE` worked, but the second failed, the `DELETE` would not be committed (it would effectively be automatically undone).

> :bulb: **Implicit Transaction Closes**: After a `COMMIT` or `ROLLBACK` statement has been executed, the transaction is automatically closed (*and future changes will implicitly commit*).

### `SAVEPOINT`

Simple `ROLLBACK` and `COMMIT` statements enable you to write or undo an *entire transaction*. Although this works for simple transactions, **more complex transactions might require partial commits or rollbacks**.

For example, the process of adding an order described previously is a single transaction. If an error occurs, you only want to roll back to the point before the orders row was added. You do not want to roll back the addition to the customers table (if there was one).

To support the rollback of partial transactions, you must be able to **put placeholders at strategic locations in the transaction block**. Then, if a rollback is required, you can roll back to one of the placeholders.

These placeholders are called ***savepoints***, and to create one use the `SAVEPOINT` statement.

```mysql
SAVEPOINT delete1;
```
Each savepoint takes a **unique name** that identifies it so that, when you roll back, MySQL knows where you are rolling back to. To roll back to this savepoint:

```mysql
ROLLBACK TO delete1;
```

> :bulb: **The More Savepoints the Better**: You can have as many savepoints as you'd like within your MySQL code, and the more the better. Why? Because the more savepoints you have the ***more flexibility*** you have in managing rollbacks exactly as you need them.

> :bulb: **Releasing Savepoints**: Savepoints are *automatically released after a transaction completes (a ROLLBACK or COMMIT is issued)*. As of MySQL 5, savepoints *can also be explicitly released using `RELEASE SAVEPOINT`*.


### Change the Default Commit Behavior

As already explained, the default MySQL behavior is to automatically commit any and all changes. In other words, any time you execute a MySQL statement, that statement is actually being performed against the tables, and the changes made occur immediately. To instruct MySQL to not automatically commit changes, you need to use the following statement:

```mysql
SET autocommit=0;
```
The **autocommit flag** determines whether changes are committed automatically without requiring a manual `COMMIT` statement. Setting autocommit to 0 (false) instructs MySQL to not automatically commit changes (until the flag is set back to true).

> :bulb: **Flag Is Connection Specific**: The autocommit flag is per connection, ***not server-wide***.

