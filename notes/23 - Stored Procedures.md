---
tags: [Notebooks/MySQL CC]
title: 23 - Stored Procedures
created: '2020-03-12T06:40:24.490Z'
modified: '2020-03-18T15:23:08.280Z'
---

# 23 - Stored Procedures

## Understanding Stored Procedures

Most of the SQL statements that we've used thus far are simple in that they use a single statement against one or more tables. Not all operations are that simple: often, multiple statements will be needed to perform a complete operation. For example, consider the following scenario:
  - To process an order, checks must be made to ensure that items are in stock.
  - If items are in stock, they need to be reserved so they are not sold to anyone else, and the available quantity must be reduced to reflect the correct amount in stock.
  - Any items not in stock need to be ordered; this requires some interaction with the vendor.
  - The customer needs to be notified as to which items are in stock (and can be shipped immediately) and which are back ordered.

This is obviously not a complete example, and it is even beyond the scope of the example tables that we have been using in this book, but it will suffice to help make a point. **Performing this process requires many MySQL statements against many tables. In addition, the exact statements that need to be performed and their order are not fixed;** they can (and will) vary according to which items are in stock and which are not.

How would you write this code? You could write each of the statements individually and execute other statements conditionally, based on the result. You'd have to do this every time this processing was needed (and in every application that needed it).

Or you could create a stored procedure. :new: **Stored procedures are simply collections of one or more MySQL statements saved for future use.** You can think of them as batch files, although in truth they are more than that.

## Why Use Stored Procedures?

Now that you know what stored procedures are, why use them? There are many reasons, but here are the primary ones:
  - To **simplify complex operations** (as seen in the previous example) by encapsulating processes into a single easy-to-use unit.
  - To **ensure data integrity** by not requiring that a series of steps be created over and over. If all developers and applications use the same (tried and tested) stored procedure, the same code will be used by all.
    - An extension of this is to **prevent errors**. The more steps that need to be performed, the more likely it is that errors will be introduced. **Preventing errors ensures data consistency.**
  - To **simplify change management**. If tables, column names, or business logic (or just about anything) changes, **only the stored procedure code needs to be updated**, and no one else will need even to be aware that changes were made.
    - An extension of this is **security**. Restricting access to underlying data via stored procedures **reduces the chance of data corruption** (unintentional or otherwise).
  - To **improve performance**, as stored procedures typically execute quicker than do individual SQL statements.
  - There are MySQL language elements and features that are available only within single requests. Stored procedures can use these to write code that is more powerful and flexible. (We'll see an example of this in the next chapter).

In other words, there are **three primary benefits: simplicity, security, and performance**. Obviously all are extremely important. Before you run off to turn all your SQL code into stored procedures, here's the **downside**:
  - Stored procedures tend to be **more complex to write** than basic SQL statements, and writing them requires a greater degree of skill and experience.
  - You might not have the **security access** needed to create stored procedures. Many database administrators restrict stored procedure creation rights, allowing users to execute them but not necessarily create them.

Nonetheless, stored procedures are very useful and should be used whenever possible.

> :bulb: **Can't Write Them? You Can Still Use Them**: MySQL distinguishes the security and access needed to write stored procedures from the security and access needed to execute them. This is a good thing; even if you can't (or don't want to) write your own stored procedures, you can still execute them when appropriate.

## Use Stored Procedures

Using stored procedures requires knowing how to execute (run) them. Stored procedures are executed far more often than they are written, so we'll start there. And then we'll look at creating and working with stored procedures.

### Execute Stored Procedures: `CALL`

MySQL refers to stored procedure execution as ***calling***, and so the MySQL statement to execute a stored procedure is simply `CALL`. `CALL` takes the name of the stored procedure and any parameters that need to be passed to it.

```sql
CALL productpricing(@pricelow,
                    @pricehigh,
                    @priceaverage);
```
*Here a stored procedure named 'productpricing' is executed; it calculates and returns the lowest, highest, and average product prices.*

Stored procedures might or might not display results, as you will see shortly.

### Create Stored Procedures: `CREATE PROCEDURE`

As already explained, writing a stored procedure is not trivial. To give you a taste for what is involved, let's look at a simple example: a stored procedure that returns the average product price.

```sql
CREATE PROCEDURE productpricing()
BEGIN
   SELECT Avg(prod_price) AS priceaverage
   FROM products;
END;
```
*Had the stored procedure accepted **parameters**, these would have been enumerated between the `(` and `)`. This stored procedure has no parameters, but the trailing `()` is still required. `BEGIN` and `END` statements are used to delimit the stored procedure body, and the body itself is just a simple `SELECT` statement*

When MySQL processes this code it creates a new stored procedure named productpricing. No data is returned because the code does not call the stored procedure, it simply creates it for future use.

> :exclamation: **mysql Command-line Client Delimiters**: The default MySQL statement delimiter is `;` (as you have seen in all of the MySQL statement used thus far). However, the mysql command-line utility also uses `;` as a delimiter. If the command-line utility were to interpret the `;` characters inside of the stored procedure itself, those would not end up becoming part of the stored procedure, and that would make the SQL in the stored procedure syntactically invalid. The solution is to temporarily change the command-line utility delimiter, as seen here:
  ```sql
    DELIMITER //
    CREATE PROCEDURE productpricing()
    BEGIN
       SELECT Avg(prod_price) AS priceaverage
       FROM products;
    END //
    DELIMITER ;
  ```
> Any character may be used as the delimiter except for `\`.

To use the just created stored procedure:

```sql
CALL productpricing();
```

As a stored procedure is actually a type of function, () characters are required after the stored procedure name (even when no parameters are being passed).

### Drop Stored Procedures: `DROP PROCEDURE`

After they are created, stored procedures remain on the server, ready for use, until dropped. The `DROP` command removes the stored procedure from the server.

```sql
DROP PROCEDURE productpricing;
```
Notice that **the trailing () is not used**; here **just the** stored procedure **name** is specified.

> :bulb: **Drop Only If It Exists**: `DROP PROCEDURE` will throw an error if the named procedure does not actually exist. To delete a procedure if it exists (and not throw an error if it does not), use `DROP PROCEDURE IF EXISTS`.

### Work with Parameters

Typically stored procedures do not display results; rather, they return them into variables that you specify.

> :new: **Variable**: A named location in memory, used for temporary storage of data.

> :new: **Variable Names**: All MySQL variable names must begin with `@`.

```sql
CREATE PROCEDURE productpricing(
   OUT pl DECIMAL(8,2),
   OUT ph DECIMAL(8,2),
   OUT pa DECIMAL(8,2)
)
BEGIN
   SELECT Min(prod_price)
   INTO pl
   FROM products;
   SELECT Max(prod_price)
   INTO ph
   FROM products;
   SELECT Avg(prod_price)
   INTO pa
   FROM products;
END;
```
*Each parameter must have its type specified; here a decimal value is used. The keyword `OUT` is used to specify that this parameter is used to send a value out of the stored procedure (back to the caller). A series of `SELECT` statements are performed to retrieve the values that are then saved into the appropriate variables (by specifying the `INTO` keyword).*

MySQL supports parameters of types `IN` (those passed to stored procedures), `OUT` (those passed from stored procedures, as we've used here), and `INOUT` (those used to pass parameters to and from stored procedures). 

> :bulb: **Parameter Datatypes**: The datatypes allowed in stored procedure parameters are the same as those used in tables (see [[Appendix D - MySQL Datatypes]]).
>> Note that a **recordset is not an allowed type**, and so multiple rows and columns could not be returned via a parameter. This is why three parameters (and three `SELECT` statements) are used in the previous example.

To call this updated stored procedure, three variable names must be specified:

```sql
CALL productpricing(@pricelow,
                    @pricehigh,
                    @priceaverage);
```
As the stored procedure expects three parameters, exactly three parameters must be passed, no more and no less. Therefore, three parameters are passed to this CALL statement. These are the names of the three variables that the stored procedure will store the results in.

When called, this statement does not actually display any data. Rather, it returns variables that can then be displayed (or used in other processing). To display the retrieved value(s):

```sql
SELECT @priceaverage;
```
```sql
SELECT @pricehigh, @pricelow, @priceaverage;
```

Here is another example, this time using both `IN` and `OUT` parameters. ordertotal accepts an order number and returns the total for that order:

```sql
CREATE PROCEDURE ordertotal(
   IN onumber INT,
   OUT ototal DECIMAL(8,2)
)
BEGIN
   SELECT Sum(item_price*quantity)
   FROM orderitems
   WHERE order_num = onumber
   INTO ototal;
END;
```

```sql
CALL ordertotal(20005, @total);
SELECT @total;
```

### Build Intelligent Stored Procedures

All of the stored procedures used thus far have basically encapsulated simple MySQL `SELECT` statements. And while they are all valid examples of stored procedures, they really don't do anything more than what you could do with those statements directly (if anything, they just make things a little more complex). The real power of stored procedures is realized when business rules and intelligent processing are included within them.

Consider this scenario. You need to obtain order totals as before, but also need to add sales tax to the total, but only for some customers (perhaps the ones in your own state). Now you need to do several things:
  1. Obtain the total (as before).
  2. Conditionally add tax to the total.
  3. Return the total (with or without tax).

```sql
-- Name: ordertotal
-- Parameters: onumber = order number
--             taxable = 0 if not taxable, 1 if taxable
--             ototal = order total variable

CREATE PROCEDURE ordertotal(
   IN onumber INT,
   IN taxable BOOLEAN,
   OUT ototal DECIMAL(8,2)
) COMMENT 'Obtain order total, optionally adding tax'
BEGIN

   -- Declare variable for total
   DECLARE total DECIMAL(8,2);
   -- Declare tax percentage
   DECLARE taxrate INT DEFAULT 6;

   -- Get the order total
   SELECT Sum(item_price*quantity)
   FROM orderitems
   WHERE order_num = onumber
   INTO total;

   -- Is this taxable?
   IF taxable THEN
      -- Yes, so add taxrate to the total
      SELECT total+(total/100*taxrate) INTO total;
   END IF;

   -- And finally, save to out variable
   SELECT total INTO ototal;

END;
```
The stored procedure has changed dramatically. First of all, comments have been added throughout (preceded by `--`). This is extremely important as stored procedures increase in complexity. An additional parameter has been addedtaxable is a BOOLEAN (specify true if taxable, false if not). Within the stored procedure body, two local variables are defined using `DECLARE` statements. `DECLARE` requires that a variable name and datatype be specified, and also supports optional default values (taxrate in this example is set to 6%). The `SELECT` has changed so the result is stored in total (the local variable) instead of ototal. Then an IF statement checks to see if taxable is true, and if it is, another `SELECT` statement is used to add the tax to local variable total. And finally, total (which might or might not have had tax added) is saved to ototal using another `SELECT` statement.

> :bulb: **The `COMMENT` Keyword**: The stored procedure for this example included a `COMMENT` value in the `CREATE PROCEDURE` statement. This is not required, but if specified, is displayed in `SHOW PROCEDURE STATUS` results.

```sql
CALL ordertotal(20005, 0, @total);
SELECT @total;
```
BOOLEAN values may be specified as 1 for true and 0 for false (actually, any non-zero value is considered true and only 0 is considered false). By specifying 0 or 1 in the middle parameter you can conditionally add tax to the order total.

```sql
CALL ordertotal(20005, 1, @total);
SELECT @total;
```

> :bulb: **The `IF` Statement**: This example showed the basic use of the MySQL `IF` statement. `IF` also supports `ELSEIF` and `ELSE` clauses (the former also uses a `THEN` clause, the latter does not). We'll be seeing additional uses of `IF` (as well as other flow control statements) in future chapters.

### Inspect Stored Procedures:

To display the `CREATE` statement used to create a stored procedure, use the `SHOW CREATE PROCEDURE` statement:

```mysql
SHOW CREATE PROCEDURE ordertotal;
```

To obtain a list of stored procedures including details on when and who created them, use `SHOW PROCEDURE STATUS`. `SHOW PROCEDURE STATUS` lists **all** stored procedures. To **restrict the output** you can use `LIKE` to specify a filter pattern, for example:

```sql
SHOW PROCEDURE STATUS LIKE 'ordertotal';
```

