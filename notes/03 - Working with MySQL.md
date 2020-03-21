---
tags: [Notebooks/MySQL CC]
title: 03 - Working with MySQL
created: '2020-03-12T00:46:15.824Z'
modified: '2020-03-12T05:22:00.240Z'
---

# 03 - Working with MySQL

## Making the Connection

## Selecting a Database

`USE database1;`
-> "Database Changed"

## Learning about Databases and Tables

`SHOW DATABASES;`
-> a list of available databases

`SHOW TABLES;`
-> a list of available tables in the currently selected database

`SHOW COLUMNS FROM table1;`
*shortcut* `DESCRIBE table1;`
-> a row for each field: filed name, data type, NULL allowed/not, key information, default value, extra information (e.g., `auto_increment` for id).

`SHOW STATUS;`
-> extensive server status information

`SHOW CREATE DATABASE/TABLE;`
-> display the MySQL statements used to create specified databases or tables respectively

`SHOW GRANTS;`
-> display security rights granted to users (all users or a specific user)

`SHOW ERRORS/WARNINGS;`
-> display server error or warning messages

in mysql command-line utility, type `HELP SHOW` to get a list of allowed `SHOW` statements
