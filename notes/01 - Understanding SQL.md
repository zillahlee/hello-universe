---
tags: [Notebooks/MySQL CC]
title: 01 - Understanding SQL
created: '2020-03-11T17:46:02.731Z'
modified: '2020-03-16T10:30:28.284Z'
---

# 01 - Understanding SQL

## Database Basics

> :new: **Database**: A container (usually a file or set of files) to store organized data.
> :new: **Database Management System (DBMS)**: Database software you use to create and manipulate databases.

> :new: **Table**: A structured list of data of a specific type. Tables have characteristics and properties that define how data is stored in them. These include information about what data may be stored, how it is broken up, how individual pieces of information are named, and much more.
> :new: **Schema**: Information about database and table layout and properties. The set of information that describes a table is known as a schema, and schema are used to describe specific tables within a database, as well as entire databases (and the relationship between tables in them, if any).

> :new: **Column**: A single field in a table. All tables are made up of one or more columns.
> :new: **Datatype**: A type of allowed data. Every table column has an associated datatype that restricts (or allows) specific data in that column.

> :new: **Row**: A record in a table.
> :new: **Primary Key**: A column (or set of columns) whose values uniquely identify every row in a table.

- **Primary Key Rules**
  *enforced by MySQL itself*
  - No two rows can have the same primary key value.
  - Every row must have a primary key value (primary key columns may not allow NULL values).
- **Primary Key Best Practices**
  - Don't update values in primary key columns.
  - Don't reuse values in primary key columns.
  - Don't use values that might change in primary key columns.

## What is SQL?

SQL (pronounced as the letters S-Q-L or as sequel) is an abbreviation for Structured Query Language. SQL is a language designed specifically for communicating with databases.

> :exclamation: **DBMS-Specific SQL**: Although SQL is not a proprietary language and there is a standards committee which tries to define SQL syntax that can be used by all DBMSs, the reality is that no two DBMSs implement SQL identically. The SQL taught in this book is specific to MySQL, and while much of the language taught will be usable with other DBMSs, do not assume complete SQL syntax portability.

