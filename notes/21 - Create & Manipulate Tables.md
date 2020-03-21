---
tags: [Notebooks/MySQL CC]
title: 21 - Create & Manipulate Tables
created: '2020-03-12T06:39:35.614Z'
modified: '2020-03-18T08:52:21.786Z'
---

# 21 - Create & Manipulate Tables

> basics of table creation, alteration, & deletion

> :warning: These statements should be used with extreme caution, and only after backups have been made.

## Create Tables: `CREATE TABLE`

There are generally two ways to create database tables:
  - Using an **administration tool** (like the ones discussed in [[02 - Introducing MySQL]]) that can be used to create and manage database tables interactively.
  - Tables may also be manipulated directly with **MySQL statements**.

To create tables programmatically, the `CREATE TABLE` SQL statement is used. It is worth noting that **when you use interactive tools, you are actually using MySQL statements**. Instead of your writing these statements, however, **the interface generates and executes the MySQL seamlessly for you** (the same is true for changes to existing tables).

> :bulb: **More Examples**: For additional examples of table creation scripts, see the code used to create the sample tables used in this book.

### Basic Table Creation

To create a table using `CREATE TABLE`, you must specify the following information:
  - The **name of the new table** specified after the keywords `CREATE TABLE`.
  - The **name and definition of the table columns** separated by commas.

The `CREATE TABLE` statement can also include other keywords and options, but at a minimum you need the table name and column details.

```mysql
CREATE TABLE customers
(
  cust_id      int       NOT NULL AUTO_INCREMENT,
  cust_name    char(50)  NOT NULL ,
  cust_address char(50)  NULL ,
  cust_city    char(50)  NULL ,
  cust_state   char(5)   NULL ,
  cust_zip     char(10)  NULL ,
  cust_country char(50)  NULL ,
  cust_contact char(50)  NULL ,
  cust_email   char(255) NULL ,
  PRIMARY KEY (cust_id)
) ENGINE=InnoDB;
```

> :bulb: **Statement Formatting**: As you will recall, whitespace is ignored in MySQL statements. Statements can be typed on one long line or broken up over many lines. It makes no difference at all. This enables you to format your SQL as best suits you. The preceding `CREATE TABLE` statement is a good example of MySQL statement formattingthe code is specified over multiple lines, with the column definitions indented for easier reading and editing. Formatting your MySQL in this way is entirely optional, but highly recommended.

> :bulb: **Handling Existing Tables**: When you create a new table, the table name specified must not exist or you'll generate an error. To *prevent accidental overwriting*, SQL requires that you first manually remove a table (see later sections for details) and then re-create it, rather than just overwriting it.
>> If you want to create a table only if it does not already exist, specify `IF NOT EXISTS` after the table name. This does not check to see that the schema of the existing table matches the one you are about to create. It simply checks to see if the table name exists, and only proceeds with table creation if it does not.

### Work with NULL Values

NULL values are no values or the lack of a value. A column that allows NULL values also allows rows to be inserted with no value at all in that column. A column that does not allow NULL values does not accept rows with no value: in other words, that column will always be required when rows are inserted or updated.

Every table column is either a NULL column or a NOT NULL column, and that state is **specified in the table definition at creation time**.

```mysql
CREATE TABLE orders
(
  order_num  int      NOT NULL AUTO_INCREMENT,
  order_date datetime NOT NULL ,
  cust_id    int      NOT NULL ,
  PRIMARY KEY (order_num)
) ENGINE=InnoDB;
```

```mysql
CREATE TABLE vendors
(
  vend_id      int      NOT NULL AUTO_INCREMENT,
  vend_name    char(50) NOT NULL ,
  vend_address char(50) NULL ,
  vend_city    char(50) NULL ,
  vend_state   char(5)  NULL ,
  vend_zip     char(10) NULL ,
  vend_country char(50) NULL ,
  PRIMARY KEY (vend_id)
) ENGINE=InnoDB;
```

**NULL is the default setting, so if NOT NULL is not specified, NULL is assumed.**

> :bulb: **Understanding NULL**: Don't confuse NULL values with empty strings. A NULL value is the lack of a value; it is not an empty string. If you were to specify '' (two single quotes with nothing in between them), that would be allowed in a NOT NULL column. ***An empty string is a valid value; it is not no value.*** NULL values are specified with the keyword NULL, not with an empty string.

### Primary Keys Revisited

Primary key values must be unique. That is, every row in a table must have a unique primary key value. If a single column is used for the primary key, it must be unique; if multiple columns are used, the combination of them must be unique.

To create **a primary key made up of multiple columns**, simply specify the column names as a comma delimited list.

```mysql
CREATE TABLE orderitems
(
  order_num  int          NOT NULL ,
  order_item int          NOT NULL ,
  prod_id    char(10)     NOT NULL ,
  quantity   int          NOT NULL ,
  item_price decimal(8,2) NOT NULL ,
  PRIMARY KEY (order_num, order_item)
) ENGINE=InnoDB;
```
*The 'orderitems' table contains the order specifics for each order in the 'orders' table. There may be multiple items per order, but each order will only ever have one first item, one second item, and so on. As such, the combination of order number (column order_num) and order item (column order_item) are unique, and thus suitable to be the primary key*

Primary keys may be defined at table creation time (as seen here) or after table creation (as will be discussed later in this chapter).

> :bulb: **Primary Keys and NULL Values**: Primary keys are columns whose values uniquely identify every row in a table. Only columns that do not allow NULL values can be used in primary keys. Columns that allow no value at all cannot be used as unique identifiers.

### Using `AUTO_INCREMENT`

Some unique identifiers (PKs) have **no special significance, other than the fact that they are unique**. When a new customer or order is added, a new customer ID or order number is needed. The numbers can be anything, so long as they are unique. Obviously, **the simplest number to use would be whatever comes next, whatever is one higher than the current highest number**.

`AUTO_INCREMENT` tells MySQL that this column is to be automatically incremented each time a row is added. Each time an `INSERT` operation is performed MySQL automatically increments (and thus `AUTO_INCREMENT`) the column, assigning it the next available value. This way each row is assigned a unique cust_id which is then used as the primary key value.

**Only one ``AUTO_INCREMENT`` column is allowed per table, and it must be indexed (for example, by making it a primary key).**

> :bulb: **Overriding `AUTO_INCREMENT`**: Need to use a specific value if a column designated as `AUTO_INCREMENT`? You can simply ***specify a value in the `INSERT` statement and as long as it is unique*** (has not been used yet), that value will be used instead of an automatically generated one. ***Subsequent incrementing will start using the value manually inserted.*** (See the table population scripts used in this book for examples of this.)

> :bulb: **Determining the `AUTO_INCREMENT` Value**: One downside of having MySQL generate (via auto increment) primary keys for you is that you don't know what those values are.
>> Consider this scenario: You are adding a new order. This requires creating a single row in the 'orders' table and then a row for each item ordered in the 'orderitems' table. The order_num is stored along with the order details in 'orderitems'. This is how the 'orders' and 'orderitems' table are related to each other. And that obviously requires that you know the generated order_num after the 'orders' row was inserted and before the 'orderitems' rows are inserted.
>
>> To obtain this value when an `AUTO_INCREMENT` column is used, use the `last_insert_id()` function: `SELECT last_insert_id();`. This returns the last `AUTO_INCREMENT` value, which you can then use in subsequent MySQL statements.

### Specify Default Values

MySQL enables you to specify default values to be used if no value is specified when a row is inserted. Default values are specified using the `DEFAULT` keyword in the column definitions in the `CREATE TABLE` statement.

```mysql
CREATE TABLE orderitems
(
  order_num  int          NOT NULL ,
  order_item int          NOT NULL ,
  prod_id    char(10)     NOT NULL ,
  quantity   int          NOT NULL  DEFAULT 1,
  item_price decimal(8,2) NOT NULL ,
  PRIMARY KEY (order_num, order_item)
) ENGINE=InnoDB;
```

> :exclamation: **Functions Are Not Allowed**: Unlike most DBMSs, MySQL does not allow the use of functions as `DEFAULT` values; ***only constants are supported.***

> :bulb: **Use DEFAULT Instead of NULL Values**: Many database developers use `DEFAULT` values instead of NULL columns, especially in columns that will be used in calculations or data groupings.

### Engine Types

Like every other DBMS, MySQL has an internal engine that actually manages and manipulates data. When you use the `CREATE TABLE` statement, that engine is used to actually create the tables, and when you use the `SELECT` statement or perform any other database processing, the engine is used internally to process your request. For the most part, the engine is buried within the DBMS and you need not pay much attention to it.

But **unlike every other DBMS, MySQL does not come with a single engine. Rather, it ships with several engines, all buried within the MySQL server**, and all capable of executing commands such as `CREATE TABLE` and `SELECT`.

So why bother shipping multiple engines? Because they **each have different capabilities and features**, and being able to pick the right engine for the job gives you unprecedented power and flexibility.

**Of course, you are free to totally ignore database engines. If you omit the `ENGINE=` statement, the default engine is used (most likely MyISAM), and most of your SQL statements will work as is.** But not all of them will, and that is why this is important (and why two engines are used in the same tables used in this book).

Here are several engines of which to be aware:
  - **`InnoDB`** is a ***transaction***-safe engine (see [[26 - Manage Transaction Processing]]). It does not support full-text searching.
  - **`MEMORY`** is functionally equivalent to MyISAM, but as data is stored in memory (instead of on disk) it is ***extremely fast*** (and ideally suited for temporary tables).
  - **`MyISAM`** is a very high-performance engine. It supports ***full-text searching*** (see [[18 - Full-Text Searching]]), but does not support transactional processing.

Engine types may be mixed. The example tables used throughout this book all use `InnoDB` with the exception of the productnotes table, which uses `MyISAM`. The reason for this is that I wanted support for transactional processing (and thus used `InnoDB`) but also needed full-text searching support in productnotes (and thus `MyISAM` for that table).

> :exclamation: **Foreign Keys Can't Span Engines**: There is one big downside to mixing engine types. Foreign keys (used to enforce referential integrity) cannot span engines. That is, ***a table using one engine cannot have a foreign key referring to a table that uses another engine***.

So, which should you use? Well, that will depend on what features you need. `MyISAM` tends to be the most popular engine because of its performance and features. But if you do need transaction-safe processing, you will need to use another engine.

## Update Tables: `ALTER TABLE`

To update table definitions, the `ALTER TABLE` statement is used. But, **ideally, tables should never be altered after they contain data**. You should spend sufficient time **anticipating future needs during the table design process** so extensive changes are not required later on.

To change a table using `ALTER TABLE`, you must specify the following information:
  - The **name of the table** to be altered after the keywords `ALTER TABLE`. (The table must exist or an error will be generated.)
  - The **list of changes** to be made.

```mysql
ALTER TABLE vendors
ADD vend_phone CHAR(20);
```

```mysql
ALTER TABLE Vendors
DROP COLUMN vend_phone;
```

One common use for `ALTER TABLE` is to **define foreign keys**.

```mysql
ALTER TABLE orderitems
ADD CONSTRAINT fk_orderitems_orders
FOREIGN KEY (order_num) REFERENCES orders (order_num);
ALTER TABLE orderitems
ADD CONSTRAINT fk_orderitems_products FOREIGN KEY (prod_id)
REFERENCES products (prod_id);

ALTER TABLE orders
ADD CONSTRAINT fk_orders_customers FOREIGN KEY (cust_id)
REFERENCES customers (cust_id);

ALTER TABLE products
ADD CONSTRAINT fk_products_vendors
FOREIGN KEY (vend_id) REFERENCES vendors (vend_id);
```

Complex table structure changes usually require **a manual move process** involving these steps:
  1. Create a new table with the new column layout.
  2. Use the `INSERT SELECT` statement (see [[19 - Insert Data]]) to copy the data from the old table to the new table. Use conversion functions and calculated fields, if needed.
  3. Verify that the new table contains the desired data.
  4. Rename the old table (or delete it, if you are really brave).
  5. Rename the new table with the name previously used by the old table.
  6. Re-create any triggers, stored procedures, indexes, and foreign keys as needed.

> :exclamation: **Use `ALTER TABLE` Carefully**: Use `ALTER TABLE` with extreme caution, and be sure you have a complete set of backups (both schema and data) before proceeding. Database table changes cannot be undone; and if you add columns you don't need, you might not be able to remove them. Similarly, if you drop a column that you do need, you might lose all the data in that column.

## Delete Tables: `DROP TABLE`

Deleting tables (actually removing the entire table, not just the contents) is very easy: **arguably too easy**.

```mysql
DROP TABLE customers2;
```

There is **no confirmation, nor is there an undo**: executing the statement will **permanently remove the table**. :exclamation:

## Rename Tables: `RENAME TABLE`

```mysql
RENAME TABLE customers2 TO customers;
```

Multiple tables may be renamed in one operation using the syntax:

```mysql
RENAME TABLE backup_customers TO customers,
             backup_vendors TO vendors,
             backup_products TO products;
```

