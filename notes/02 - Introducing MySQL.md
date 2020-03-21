---
tags: [Notebooks/MySQL CC]
title: 02 - Introducing MySQL
created: '2020-03-11T17:47:27.142Z'
modified: '2020-03-16T10:30:46.727Z'
---

# 02 - Introducing MySQL

## What is MySQL?

#### Two Types of DBMSs
- shared file based
  - e.g., Microsoft Access, FileMaker
  - designed for desktop use
- client-server
  - e.g., MySQL, Oracle, Microsoft SQL Server

Client-server applications are split into two distinct parts. The **server** portion is a piece of software that is responsible for all data access and manipulation. This software runs on a computer called the database server. Only the server software interacts with the data files. All requests for data, data additions and deletions, and data updates are funneled through the server software. These requests or changes come from computers running client software. The **client** is the piece of software with which the user interacts. If you request an alphabetical list of products, for example, the client software submits that request over the network to the server software. The server software processes the request; filters, discards, and sorts data as necessary; and sends the results back to your client software.

*The client and server software may be installed on two computers or on one computer. Regardless, the client software communicates with the server software for all database interaction, be it on the same machine or not.*

To work with MySQL you'll need access to both a computer running the MySQL server software and client software with which to issue commands to MySQL:
- The server software is the MySQL DBMS. You can be running a locally installed copy, or you can connect to a copy running a remote server to which you have access.
- The **client** can be **MySQL-provided tools**, scripting languages (such as Perl), web application development languages (such as ASP, ColdFusion, JSP, and PHP), **programming languages** (such as C, C++, and Java), and more.

## MySQL Tools

There are lots of client application options, but when learning MySQL (and indeed, when writing and testing MySQL scripts) you are best off using a utility designed for just that purpose. And there are three tools in particular that warrant specific mention.
- `mysql` Command-Line Utility
- MySQL Administrator
- MySQL Query Browser

