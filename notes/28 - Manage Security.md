---
tags: [Notebooks/MySQL CC]
title: 28 - Manage Security
created: '2020-03-12T06:41:27.305Z'
modified: '2020-03-18T16:39:13.573Z'
---

# 28 - Manage Security

> Database servers usually contain critical data, and ensuring the safety and integrity of that data requires that access control be used.

## Access Control

The basis of security for your MySQL server is this: **Users should have appropriate access to the data they need, no more and no less.** In other words, *users should not have too much access to too much data*.

:thinking: :open_mouth: Consider the following:
  - Most users need to read and write data from tables, but few users will ever need to be able to create and drop tables.
  - Some users might need to read tables but might not need to update them.
  - You might want to allow users to add data, but not delete data.
  - Some users (managers or administrators) might need rights to manipulate user accounts, but most should not.
  - You might want users to access data via stored procedures, but never directly.
  - You might want to restrict access to some functionality based on from where the user is logging in.

These are just examples, but they help demonstrate an important point. You need to **provide users with the access they need and just the access they need. :new: This is known as access control, and managing access control requires creating and managing user accounts.**

> :bulb: **Use MySQL Administrator**: The MySQL Administrator (described in Chapter 2, "Introducing MySQL") provides a graphical user interface that can be used to manage users and account rights. *Internally*, MySQL Administrator uses the statements described in this chapter, enabling you to manage access control interactively and simply.

Back in Chapter 3, "Working with MySQL," you learned that you need to log in to MySQL in order to perform any operations. When first installed, MySQL creates a user account named root which has complete and total control over the entire MySQL server. You might have been using the root login throughout the chapters in this book, and that is fine when experimenting with MySQL on non-live servers. But **in the real world you'd never use root on a day-to-day basis. Instead, you'd create a series of accounts, some for administration, some for users, some for developers, and so on.**

> :bulb: **Preventing Innocent Mistakes**: It is important to note that access control is not just intended to keep out users with malicious intent. More often than not, data nightmares are the result of an inadvertent mistake, a mistyped MySQL statement, being in the wrong database, or some other user error. Access control helps avoid these situations by ensuring that users are unable to execute statements they should not be executing.

> :no_entry: **Don't Use `root`**: The `root` login should be **considered sacred**. Use it only when absolutely needed (perhaps if you cannot get in to other administrative accounts). `root` should never be used in day-to-day MySQL operations. :open_mouth:

## Manage Users

**MySQL user accounts and information are stored in a MySQL database named 'mysql'.** You usually do not need to access the 'mysql' database and tables directly (as you will soon see), but sometimes you might. One of those times is when you want to obtain a list of all user accounts. 

```mysql
USE mysql;
SELECT user FROM user;
```
*The mysql database contains a table named 'user' which contains all user accounts. 'user' contains a column named 'user' that contains the user login name. A newly installed server might have a single user listed (as seen here); established servers will likely have far more.*

> :bulb: **Test with Multiple Clients**: The ***easiest way to test changes made to user accounts and rights is to open multiple database clients*** (multiple copies of the mysql command-line utility, for example), one logged in with the administrative login and the others logged in as the users being tested.

### Create User Accounts: `CREATE USER`

```mysql
CREATE USER ben IDENTIFIED BY 'p@$$w0rd';
```
*`CREATE USER` creates a new user account. A password need not be specified at user account creation time, but this example does specify a password using `IDENTIFIED BY` 'p@$$w0rd'.*

> :bulb: **Specifying a Hashed Password**: The password specified by `IDENTIFIED BY` is plain text that MySQL will ***encrypt before saving it in the user table***. *To specify the password as a hashed value, use `IDENTIFIED BY PASSWORD` instead.*

> :bulb: **Using `GRANT` or `INSERT`**: The `GRANT` statement (which we will get to shortly) can also create user accounts, but ***generally `CREATE USER` is the cleanest and simplest syntax***. In addition, it is possible to add users by inserting rows into user directly, but to be safe this is generally not recommended. The tables used by MySQL to store user account information (as well as table schemas and more) are extremely important, and any damage to them could seriously harm the MySQL server. As such, *it is always better to use tags and functions to manipulate these tables as opposed to manipulating them directly*.

```mysql
RENAME USER ben TO bforta;
```
*renames the user account*

### Delete User Accounts: `DROP USER`

To delete a user account (along with any associated rights & privileges):

```mysql
DROP USER bforta;
```

### Set User Rights: `GRANT`

With user accounts created, you must next assign access rights and privileges. **Newly created user accounts have no access at all.** They can log into MySQL but will see no data and will be unable to perform any database operations.

To see the rights granted to a user account：

```mysql
SHOW GRANTS FOR bforta;
```
*The output shows that user bforta has a single right granted, `USAGE ON *.*. USAGE` means no rights at all (not overly intuitive, I know), so the results mean no rights to anything on any database and any table.*

> :bulb: **Users Are Defined As `user@host`**: MySQL privileges are defined using a combination of user name and hostname. If no host name is specified, a default hostname of % is used (effectively granting access to the user regardless of the hostname).

To set rights the `GRANT` statement is used. At a minimum, `GRANT` requires that you specify
  - The **privilege** being granted
  - The **database or table** being granted access to
  - The **user** name

```mysql
GRANT SELECT ON crashcourse.* TO beforta;
```
*This `GRANT` allows the use of `SELECT` on 'crashcourse.*' (crashcourse database, all tables). By granting `SELECT` access only, user bforta has **read-only** access to all data in the crashcourse database.*

Each `GRANT` adds (or updates) a permission statement for the user. MySQL reads all of the grants and determines the rights and permissions based on them.

The opposite of GRANT is `REVOKE`, which is used to revoke specific rights and permissions.

```mysql
REVOKE SELECT ON crashcourse.* FROM beforta;
```
*This `REVOKE` statement takes away the `SELECT` access just granted to user bforta. The access being revoked must exist or an error will be thrown.*

`GRANT` and `REVOKE` can be used to control access at several levels:
  - **Entire server**, using `GRANT ALL` and `REVOKE ALL`
  - **Entire database**, using `ON database.*`
  - **Specific tables**, using `ON database.table`
  - **Specific columns**
  - **Specific stored procedures**

The following table lists each of the rights and privileges that may be granted or revoked.

| Privilege | Description |
| :--- | :--- |
| `ALL` | All privileges except `GRANT OPTION` |
| `ALTER` | Use of `ALTER TABLE` |
| `ALTER ROUTINE` | Use of `ALTER PROCEDURE` and `DROP PROCEDURE` |
| `CREATE` | Use of `CREATE TABLE` |
| `CREATE ROUTINE` | Use of `CREATE PROCEDURE` |
| `CREATE TEMPORARY TABLES` | Use of `CREATE TEMPORARY TABLE` |
| `CREATE USER` | Use of `CREATE USER`, `DROP USER`, `RENAME USER`, and `REVOKE ALL PRIVILEGS` |
| `CREATE VIEW` | Use of `CREATE VIEW` |
| `DELETE` | Use of `DELETE` |
| `DROP` | Use of `DROP TABLE` |
| `EXECUTE` | Use of `CALL` and stored procedures |
| `FILE` | Use of `SELECT INTO OUTFILE` and `LOAD DATA INFILE` |
| `GRANT OPTION` | Use of `GRANT` and `REVOKE` |
| `INDEX` | Use of `CREATE INDEX` and `DROP INDEX` |
| `INSERT` | Use of `INSERT` |
| `LOCK TABLES` | Use of `LOCK TABLES` |
| `PROCESS` | Use of `SHOW FULL PROCESSLIST` |
| `RELOAD` | Use of `FLUSH` |
| `REPLICATION CLIENT` | Access to location of servers |
| `REPLICATION SLAVE` | Used by replication slaves :open_mouth: |
| `SELECT` | Use of `SELECT` |
| `SHOW DATABASES` | Use of `SHOW DATABASES` |
| `SHOW VIEW` | Use of `SHOW CREATE VIEW` |
| `SHUTDOWN` | Use of `mysqladmin shutdown` (used to shut down MySQL) |
| `SUPER` | Use of `CHANGE MASTER`, `KILL`, `LOGS`, `PURGE MASTER`, and `SET GLOBAL`. Also allows `mysqladmin debug` login. |
| `UPDATE` | Use of `UPDATE` |
| `USAGE` | No access |

Using `GRANT` and `REVOKE` in conjunction with the privileges listed above, you have complete control over what users can and cannot do with your precious data.

> :bulb: **Granting for the Future**: When using `GRANT` and `REVOKE`, the user account must exist, but the objects being referred to need not. This allows administrators to ***design and implement security before databases and tables are even created***.
>> A ***side effect*** of this is that if a database or table is removed (with a `DROP` statement) any associated access rights will still exist. And if the database or table is re-created in the future, those rights will apply to them.

> :bulb: **Simplifying Multiple Grants**: Multiple `GRANT` statements may be strung together by listing the privileges and comma delimiting them: `GRANT SELECT, INSERT ON crashcourse.* TO beforta;`

### Change Passwords: `SET PASSWORD`

To change user passwords use the `SET PASSWORD` statement. New passwords must be encrypted as seen here:

```mysql
SET PASSWORD FOR bforta = Password('n3w p@$$w0rd');
```
*`SET PASSWORD` updates a user password. The new password **must be encrypted by being passed to the `Password()` function**.*

To set your own password:

```mysql
SET PASSWORD = Password('n3w p@$$w0rd');
```
*When no user name is specified, `SET PASSWORD` updates the password for the currently logged in user.*

