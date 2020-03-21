---
tags: [Notebooks/MySQL CC]
title: 22 - Views
created: '2020-03-12T06:40:19.790Z'
modified: '2020-03-18T15:23:37.430Z'
---

# 22 - Views

> :new: Views are **virtual tables**. They do not contain data, but they **contain queries that retrieve data as needed**, instead.
>> Views provide a level of encapsulation around MySQL `SELECT` statements and can be used to **simplify data manipulation**, as well as to **reformat or secure underlying data**.

## Understanding Views

Views are **virtual tables**. Unlike tables that contain data, views simply **contain queries that dynamically retrieve data when used**.

Back in Chapter 15, "Joining Tables," you used the following `SELECT` statement to retrieve data from three tables:

```sql
SELECT cust_name, cust_contact
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num
  AND prod_id = 'TNT2';
```
That query was used to retrieve the customers who had ordered a specific product. Anyone needing this data would have to understand the table structure, as well as how to create the query and join the tables. To retrieve the same data for another product (or for multiple products), the last `WHERE` clause would have to be modified.

Now imagine that you could **wrap that entire query in a virtual table** called 'productcustomers'. You could then simply do the following to retrieve the same data:

```sql
SELECT cust_name, cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';
```
'productcustomers' is a view, and as a view, it does not contain any actual columns or data as a table would. Instead, **it contains a SQL query**: the same query used previously to join the tables properly.

### Why Use Views?

Here are some other common uses:
  - To **reuse SQL statements**.
  - To **simplify complex SQL operations**. After the query is written, it can be reused easily, without having to know the details of the underlying query itself.
  - To **expose parts of a table** instead of complete tables.
  - To **secure data**. Users can be given access to specific subsets of tables instead of to entire tables.
  - To **change data formatting and representation**. Views can return data formatted and presented differently from their underlying tables.

**For the most part, after views are created, they can be used in the same way as tables.** You can perform `SELECT` operations, filter and sort data, join views to other views or tables, and possibly even add and update data. (There are some restrictions on this last item. More on that in a moment.)

The important thing to remember is **views are just that, views into data stored *elsewhere***. Views **contain no data themselves**, so **the data they return is retrieved from other tables**. When data is added or changed in those tables, the views will return that changed data.

> :exclamation: **Performance Issues**: Because views contain no data, any retrieval needed to execute a query must be processed every time the view is used. If you create complex views with multiple joins and filters, or if you nest views, you may find that performance is dramatically degraded. Be sure you ***test execution before deploying applications that use views extensively***.

### View Rules & Restrictions

Here are some of the most common rules and restrictions governing view creation and usage:
  - Like tables, views must be **uniquely named**. (They cannot be named with the name of any other table or view).
  - There is **no limit to the number of views** that can be created.
  - To create views, you must have **security access**. This is usually granted by the database administrator.
  - Views **can be nested**; that is, a view may be built using a query that retrieves data from another view.
  - `ORDER BY` may be used in a view, but it will be **overridden if** `ORDER BY` is also used in the `SELECT` that retrieves data from the view.
  - **Views cannot be indexed, nor can they have triggers or default values associated with them.**
  - Views **can be used in conjunction with tables**, for example, to create a `SELECT` statement which *joins a table and a view*.

## Use Views

- Views are **created** using the `CREATE VIEW` statement.
- To view the statement used to create a view, use `SHOW CREATE VIEW viewname;`.
- To **remove** a view, the `DROP` statement is used. The syntax is simply `DROP VIEW viewname;`.
- To **update** a view you may use the `DROP` statement and then the `CREATE` statement again, or just use `CREATE OR REPLACE VIEW`, which will create it if it does not exist and replace it if it does.

### Use Views to Simplify Complex Joins

One of the most common uses of views is to hide complex SQL, and this often involves joins.

```sql
CREATE VIEW productcustomers AS
SELECT cust_name, cust_contact, prod_id
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num;
```
*This statement creates a view named productcustomers, which joins three tables to return a list of all customers who have ordered any product. If you were to `SELECT * FROM productcustomers`, you'd list every customer who ordered anything.*

To retrieve a list of customers who ordered product 'TNT2':

```sql
SELECT cust_name, cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';
```
*This statement retrieves specific data from the view by issuing a `WHERE` clause. When MySQL processes the request, it adds the specified `WHERE` clause to any existing `WHERE` clauses in the view query so the data is filtered correctly.*

As you can see, views can greatly simplify the use of complex SQL statements. Using views, you can write the underlying SQL once and then reuse it as needed.

> :bulb: **Creating Reusable Views**: It is a good idea to create views that are not tied to specific data. For example, the view created in this example returns customers for all products, not just product 'TNT2' (for which the view was first created). ***Expanding the scope of the view*** enables it to be reused, making it even more useful. It also eliminates the need for you to create and maintain multiple similar views.

### Use Views to Reformat Retrieved Data

Another common use of views is for reformatting retrieved data. The following `SELECT` statement (from Chapter 10, "Creating Calculated Fields") returns vendor name and location in a single combined calculated column:

```sql
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')')
       AS vend_title
FROM vendors
ORDER BY vend_name;
```
Suppose that you regularly needed results in this format. Rather than perform the concatenation each time it was needed, you could create a view and use that instead.

```sql
CREATE VIEW vendorlocations AS
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')')
       AS vend_title
FROM vendors
ORDER BY vend_name;
```
To retrieve the data to create all mailing labels:

```sql
SELECT *
FROM vendorlocations;
```

### Use Views to Filter Unwanted Data

Views are also useful for applying common `WHERE` clauses. For example, you might want to define a 'customeremaillist' view so it filters out customers without email addresses.

```sql
CREATE VIEW customeremaillist AS
SELECT cust_id, cust_name, cust_email
FROM customers
WHERE cust_email IS NOT NULL;
```

View 'customeremaillist' can now be used for data retrieval just like any table.

```sql
SELECT *
FROM customeremaillist;
```

> :bulb: **`WHERE` Clauses and `WHERE` Clauses**: If a `WHERE` clause is used when retrieving data from the view, the two sets of clauses (***the one in the view and the one passed to it***) will be ***combined automatically***.

### Use Views with Calculated Fields

Views are exceptionally useful for simplifying the use of calculated fields. The following is a `SELECT` statement introduced in Chapter 10. It retrieves the order items for a specific order, calculating the expanded price for each item:

```sql
SELECT prod_id,
       quantity,
       item_price,
       quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 20005;
```
To turn this into a view:

```sql
CREATE VIEW orderitemsexpanded AS
SELECT order_num,
       prod_id,
       quantity,
       item_price,
       quantity*item_price AS expanded_price
FROM orderitems;
```
To use the view to retrieve details of order 20005:

```sql
SELECT *
FROM orderitemsexpanded
WHERE order_num = 20005;
```

As you can see, views are easy to create and even easier to use. Used correctly, views can greatly simplify complex data manipulation.

### Update Views

All of the views thus far have been used with `SELECT` statements. But **can view data be updated?** The answer is that **it depends**.

As a rule, yes, views are updateable (that is, you can use `INSERT`, `UPDATE`, and `DELETE` on them). Updating a view updates the underlying table (the view, you will recall, has no data of its own); if you add or remove rows from a view you are actually removing them from the underlying table.

But **not all views are updateable**. Basically, if MySQL is unable to correctly ascertain the underlying data to be updated, updates (this includes inserts and deletes) are not allowed. In practice, this means that **if any of the following are used, you'll not be able to update the view**:
  - **Grouping** (using `GROUP BY` and `HAVING`)
  - **Joins**
  - **Subqueries**
  - Unions
  - **Aggregate functions** (`Min()`, `Count()`, `Sum()`, and so forth)
  - **`DISTINCT`**
  - **Derived (calculated) columns**

In other words, many of the examples used in this chapter would not be updateable. This might sound like a serious restriction, but in reality it isn't because views are primarily used for data retrieval anyway.

> *The previous list was accurate as of MySQL 5. Future MySQL updates will likely remove some of these restrictions*.

> **Use Views for Retrieval**: As a rule, use views for data retrieval (`SELECT` statements) and **not for updates** (`INSERT`, `UPDATE`, and `DELETE`).

