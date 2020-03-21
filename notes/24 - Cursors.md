---
tags: [Notebooks/MySQL CC]
title: 24 - Cursors
created: '2020-03-12T06:40:45.288Z'
modified: '2020-03-18T15:22:57.141Z'
---

# 24 - Cursors

## Understanding Cursors

As you have seen in previous chapters, MySQL retrieval operations work with sets of rows known as **result sets**. The rows returned are all the rows that match a SQL statementzero or more of them. *Using simple `SELECT` statements, there is no way to get the first row, the next row, or the previous 10 rows, for example. Nor is there an easy way to process all rows, one at a time (as opposed to all of them in a batch).*

Sometimes there is a need to **step through rows forward or backward** and **one or more at a time**. This is what cursors are used for. :new: **A cursor is a *database query* stored on the MySQL server: not a `SELECT` statement, but *the result set retrieved* by that statement.** Once the cursor is stored, applications can scroll or browse up and down through the data as needed.

Cursors are **used primarily by interactive applications** in which users need to scroll up and down through screens of data, browsing or making changes.

> :exclamation: **Only in Stored Procedures**: *Unlike most DBMSs*, MySQL cursors may only be used within stored procedures (and functions).

## Work with Cursors

Using cursors involves several distinct steps:
  1. Before a cursor can be used it must be **declared (defined)**. This process does not actually retrieve any data; it merely defines the `SELECT` statement to be used.
  2. After it is declared, the cursor must be **opened for use**. This process *actually retrieves the data* using the previously defined `SELECT` statement.
  3. With the cursor populated with data, **individual rows can be fetched (retrieved) as needed**.
  4. When it is done, the cursor must be **closed**.
 
After a cursor is declared, it may be opened and closed as often as needed. After it is open, fetch operations can be performed as often as needed.

### Create Cursors: `DECLARE`

`DECLARE` names the cursor and takes a `SELECT` statement, complete with `WHERE` and other clauses if needed.

```sql
CREATE PROCEDURE processorders()
BEGIN
   DECLARE ordernumbers CURSOR
   FOR
   SELECT ordernum FROM orders;
END;
```
This stored procedure does not do a whole lot. A `DECLARE` statement is used to define and name the cursorin this case ordernumbers. Nothing is done with the cursor, and as soon as the stored procedure finishes processing it will cease to exist (as it is local to the stored procedure itself).

### `OPEN` & `CLOSE` Cursors

```sql
OPEN ordernumbers;
```
When the `OPEN` statement is processed, **the query is executed, and the retrieved data is stored for subsequent browsing and scrolling**.

```sql
CLOSE ordernumbers;
```
`CLOSE` **frees up any internal memory and resources used by the cursor**, and so every cursor should be closed when it is no longer needed.

After a cursor is closed, it cannot be reused without being opened again. However, a cursor does not need to be declared again to be used; an `OPEN` statement is sufficient.

> :bulb: **Implicit Closing**: If you do not explicitly close a cursor, MySQL will close it **automatically** when the `END` statement is reached.

An updated version of the previous example:

```sql
CREATE PROCEDURE processorders()
BEGIN
   -- Declare the cursor
   DECLARE ordernumbers CURSOR
   FOR
   SELECT order_num FROM orders;

   -- Open the cursor
   OPEN ordernumbers;

   -- Close the cursor
   CLOSE ordernumbers;

END;
```
*This stored procedure declares, opens, and closes a cursor. However, nothing is done with the retrieved data.*

### Use Cursors Data: `FETCH`

After a cursor is opened, each row can be accessed individually using a `FETCH` statement. `FETCH` specifies **what is to be retrieved (the desired columns)** and **where retrieved data should be stored**. It also **advances the internal row pointer within the cursor** so **the next `FETCH` statement will retrieve the next row** (and not the same one over and over).

```sql
CREATE PROCEDURE processorders()
BEGIN

   -- Declare local variables
   DECLARE o INT;

   -- Declare the cursor
   DECLARE ordernumbers CURSOR
   FOR
   SELECT order_num FROM orders;

   -- Open the cursor
   OPEN ordernumbers;

   -- Get order number
   FETCH ordernumbers INTO o;

   -- Close the cursor
   CLOSE ordernumbers;

END;
```
*Here `FETCH` is used to retrieve the order_num column of the current row (it'll start at the first row automatically) into a local declared variable named 'o'. Nothing is done with the retrieved data.*

```mysql
CREATE PROCEDURE processorders()
BEGIN

   -- Declare local variables
   DECLARE done BOOLEAN DEFAULT 0;
   DECLARE o INT;

   -- Declare the cursor
   DECLARE ordernumbers CURSOR
   FOR
   SELECT order_num FROM orders;

   -- Declare continue handler
   DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;

   -- Open the cursor
   OPEN ordernumbers;

   -- Loop through all rows
   REPEAT

      -- Get order number
      FETCH ordernumbers INTO o;

   -- End of loop
   UNTIL done END REPEAT;

   -- Close the cursor
   CLOSE ordernumbers;

END;
```
Like the previous example, this example uses `FETCH` to retrieve the current order_num into a declared variable named 'o'. Unlike the previous example, the `FETCH` here is within a `REPEAT` so it is repeated over and over *until done is true* (as specified by `UNTIL done END REPEAT;`). To make this work, variable 'done' is defined with a `DEFAULT 0` (false, not done). So how does 'done' get set to true when done?

`DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;` defines a `CONTINUE HANDLER`, code that will be executed when a condition occurs. Here it specifies that when `SQLSTATE '02000'` occurs, then `SET done=1`. And `SQLSTATE '02000'` is *a not found condition* and so it *occurs when `REPEAT` cannot continue because there are no more rows to loop through*.

> :exclamation: **`DECLARE` Statement Sequence**: There is specific order in which `DECLARE` statements, if used, must be issued. *Local variables* defined with `DECLARE` *must be defined before any cursors or handlers* are defined, and *handlers must be defined after any cursors*. Failure to follow this sequencing will generate an error message.

If you were to call this stored procedure it would define variables and a `CONTINUE HANDLER`, define and open a cursor, repeat through all rows, and then close the cursor. With this functionality in place you can now place any needed processing inside the loop (after the `FETCH` statement and before the end of the loop).

> :bulb: **`REPEAT` or `LOOP?`**: In addition to the `REPEAT` statement used here, MySQL also supports a `LOOP` statement that can be used to repeat code until the `LOOP` is **manually exited** using a `LEAVE` statement. In general, the syntax of the `REPEAT` statement makes it better suited for looping through cursors.

To put this all together, here is one further revision of our example stored procedure with cursor, this time with some actual processing of fetched data:

```mysql
CREATE PROCEDURE processorders()
BEGIN

   -- Declare local variables
   DECLARE done BOOLEAN DEFAULT 0;
   DECLARE o INT;
   DECLARE t DECIMAL(8,2);

   -- Declare the cursor
   DECLARE ordernumbers CURSOR
   FOR
   SELECT order_num FROM orders;
   -- Declare continue handler
   DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;

   -- Create a table to store the results
   CREATE TABLE IF NOT EXISTS ordertotals
      (order_num INT, total DECIMAL(8,2));

   -- Open the cursor
   OPEN ordernumbers;

   -- Loop through all rows
   REPEAT

      -- Get order number
      FETCH ordernumbers INTO o;

      -- Get the total for this order
      CALL ordertotal(o, 1, t);

      -- Insert order and total into ordertotals
      INSERT INTO ordertotals(order_num, total)
      VALUES(o, t);

   -- End of loop
   UNTIL done END REPEAT;

   -- Close the cursor
   CLOSE ordernumbers;

END;
```
In this example, we've added another variable named 't' (this will store the total for each order). The stored procedure also creates a new table on the fly (if it does not exist) named 'ordertotals'. This table will store the results generated by the stored procedure. `FETCH` fetches each order_num as it did before, and then used `CALL` to execute another stored procedure (the one we created in the previous chapter) to calculate the total with tax for each order (the result of which is stored in t). And then finally, `INSERT` is used to save the order number and total for each order.

This stored procedure returns no data, but it does create and populate another table that can then be viewed using a simple `SELECT` statement:
> :question: shouldn't we `CALL` the procedure first?
```sql
SELECT *
FROM ordertotals;
```

And then you have it, a complete working example of stored procedures, cursors, row-by-row processing, and even stored procedures calling other stored procedures. :confused:

