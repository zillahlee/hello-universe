---
tags: [Notebooks/MySQL CC]
title: 30 - Improve Performance
created: '2020-03-12T06:41:48.528Z'
modified: '2020-03-18T17:11:32.174Z'
---

# 30 - Improve Performance 

Database administrators spend a significant portion of their lives tweaking and experimenting to improve DBMS performance. **Poorly performing databases (and database queries, for that matter) tend to be the most frequent culprits when diagnosing application sluggishness and performance problems.**

What follows is not, by any stretch of the imagination, the last word on MySQL performance. This is intended to **review key points made in the previous 29 chapters**, as well as to provide a springboard from which to launch performance optimization discussion and analysis.

So, here goes:
  - First and foremost, MySQL (like all DBMSs) has specific hardware recommendations. Using any old computer as a database server is fine when learning and playing with MySQL. But production servers should adhere to all recommendations.
  - As a rule, critical production DBMSs should run on their own dedicated servers.
  - MySQL is preconfigured with a series of default settings that are usually a good place to start. But after a while you might need to tweak memory allocation, buffer sizes, and more. (To see the current settings use `SHOW VARIABLES;` and `SHOW STATUS;`.)
  - MySQL is a multi-user multi-threaded DBMS; in other words, it often performs multiple tasks at the same time. And if one of those tasks is executing slowly, all requests will suffer. If you are experiencing unusually poor performance, use `SHOW PROCESSLIST` to display all active processes (along with their thread IDs and execution time). You can also use the `KILL` command to terminate a specific process (you'll need to be logged in as an administrator to use that one).
  - **There is almost always more than one way to write a `SELECT` statement. Experiment with joins, unions, subqueries, and more to find what is optimum for you and your data.**
  - **Use the `EXPLAIN` statement to have MySQL explain how it will execute a `SELECT` statement.** :open_mouth:
  - As a general rule, **stored procedures** execute quicker than individual MySQL statements.
  - Use the right **data types**, always.
  - **Never retrieve more data than you need.** In other words, no SELECT * (unless you truly do need each and every column).
  - Some operations (including `INSERT`) support an optional `DELAYED` keyword that, if used, returns control to the calling application immediately and actually performs the operation as soon as possible.
  - **When importing data**, turn off auto-commit. You may also want to drop indexes (including `FULLTEXT` indexes) and then re-create them after the import has completed.
  - Database tables must be indexed to improve the performance of data retrieval. Determining **what to index** is not a trivial task, and involves analyzing used `SELECT` statements to find recurring `WHERE` and `ORDER BY` clauses. If a simple `WHERE` clause is taking too long to return results, you can bet that the column (or columns) being used is a good candidate for indexing.
  - **Have a series of complex `OR` conditions in your `SELECT` statement? You might see a significant performance improvement by using multiple SELECT statements and `UNION` statements to connect them.**
  - Indexes improve the performance of data retrieval, but hurt the performance of data insertion, deletion, and updating. If you have tables that collect data and are not often searched, **don't index them until needed**. (Indexes can be added and dropped as needed).
  - **`LIKE` is slow. As a general rule, you are better off using `FULLTEXT` rather than `LIKE`.**
  - **Databases are living entities.** A well-optimized set of tables might not be so after a while. As table usage and contents change, so might the ideal optimization and configuration.
  - And the most important rule is simply this: **every rule is meant to be broken at some point**. :smile:

> :bulb: **Browse the Docs**: The MySQL documentation is full of useful tips and tricks (and even user-provided comments and feedback). Be sure to check out this invaluable resource.

