---
tags: [Notebooks/MySQL CC]
title: 10 - Creating Calculated Fields
created: '2020-03-12T04:14:43.426Z'
modified: '2020-03-16T11:39:44.536Z'
---

# 10 - Creating Calculated Fields

> create such fields, & refer to them with aliases

> :new: **Field**: Essentially means the same thing as column and often is used interchangeably, although database columns are typically called columns and the term fields is normally used in conjunction with calculated fields.

## Understanding Calculated Fields
Data stored within a database's tables is often not available in the exact format needed by your applications.

Rather than retrieve the data as it is and then reformat it within your client application or report, what you really want is to retrieve converted, calculated, or reformatted data directly from the database.

This is where ***calculated fields*** come in. Unlike all the columns we retrieved in the chapters thus far, calculated fields don't actually exist in database tables. Rather, a calculated field is **created on-the-fly** within a SQL `SELECT` statement.

It is important to note that only the database knows which columns in a `SELECT` statement are actual table columns and which are calculated fields. From the perspective of a client (for example, your application), a calculated field's data is returned in the same way as data from any other column.

> :exclamation: **Client vs. Server Formatting**: Many of the conversions and reformatting that can be performed within SQL statements can also be performed directly in your client application. However, as a rule, it is far quicker to perform these operations on the database server than it is to perform them within the client because Database Management Systems (DBMS) are built to perform this type of processing quickly and efficiently.

## Concatenate Fields: `Concat()`
> :new: **Concatenate**: Joining values together (by appending them to each other) to form a single long value.

> :exclamation: **MySQL Is Different**: Most DBMSs use operators + or || for concatenation; MySQL uses the Concat() function. Keep this in mind when converting SQL statements to MySQL.

```sql
SELECT Concat(vend_name, ' (', vend_country, ')')
FROM vendors
ORDER BY vend_name;
```

### Remove Spaces: `Trim()`, `LTrim()`, `RTrim()`
- `Trim()` trims all spaces from **both** the right and left of a value
- `LTrim()` trims all spaces from the left of a value
- `RTrim()` trims all spaces from the right of a value

To trim data so as to remove any trailing spaces:
```sql
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')')
FROM vendors
ORDER BY vend_name;
```

### Using Aliases: `AS`
What is the name of this new calculated column? Well, the truth is, it has no name; it is simply a value. Although this can be fine if you are just looking at the results in a SQL query tool, an unnamed column cannot be used within a client application because the client has no way to refer to that column.

To solve this problem, SQL supports column aliases. An alias is just that, an alternative name for a field or value. Aliases are assigned with the `AS` keyword.

```sql
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') AS
vend_title
FROM vendors
ORDER BY vend_name;
```

> :bulb: **Other Uses for Aliases**: Aliases have other uses, too. Some common uses include *renaming a column* if the real table column name contains *illegal characters* (for example, spaces) and *expanding column names* if the original names are either *ambiguous* or easily *misread*.
>> :new: **Derived Columns**: Aliases are also sometimes referred to as derived columns, so regardless of the term you run across, they mean the same thing.

## Perform Mathematical Calculations
Another frequent use for calculated fields is performing mathematical calculations on retrieved data. 

```sql
SELECT prod_id,
       quantity,
       item_price,
       quantity * item_price AS expanded_price
FROM orderitems
WHERE order_num = 20005;
```
*The client application can now use this new calculated column just as it would any other column.*

### MySQL Mathematical Operators
| Operator | Description |
| :--- | :--- |
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |

***parentheses** can be used to establish **order of precedence***

> :bulb: **How to Test Calculations**: `SELECT` provides a great way to test and experiment with functions and calculations. Although `SELECT` is usually used to retrieve data from a table, the `FROM` clause may be omitted to simply access and work with expressions. For example, `SELECT 3 * 2;` would return 6, `SELECT Trim(' abc ');` would return abc, and `SELECT Now();` uses the `Now()` function to return the current date and time. You get the idea use `SELECT` to experiment as needed.

