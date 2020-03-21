---
tags: [Notebooks/MySQL CC]
title: 29 - Database Maintenance
created: '2020-03-12T06:41:38.055Z'
modified: '2020-03-18T17:01:09.223Z'
---

# 29 - Database Maintenance

> :worried:

## Back Up Data

Like all data, MySQL data must be backed up regularly. As MySQL databases are disk-based files, normal backup systems and routines can back up MySQL data. However, as those files are always open and in use, **normal file copy backup may not always work**.

Here are possible solutions to this problem:
  - Use the command line `mysqldump` utility to dump all database contents to an external file. This utility should ideally be run before regular backups occur so the dumped file will be backed up properly.
  - The command line `mysqlhotcopy` utility can be used to copy all data from a database (this one is not supported by all database engines).
  - You can also use MySQL to dump all data to an external file using `BACKUP TABLE` or `SELECT INTO OUTFILE`. Both statements take the name of a system file to be created, and that file must not already exist or an error will be generated. Data can be restored using `RESTORE TABLE`.

> :bulb: **Flush Unwritten Data**: First To ensure that all data is written to disk (including any index data) you might need to use a `FLUSH TABLES` statement before performing your backup.

## Perform Database Maintenance

MySQL features a series of statements that can (and should) be used to ensure that databases are correct and functioning properly.

Here are some statements you should be aware of:
  - `ANALYZE TABLE` is used to check that table keys are correct. `ANALYZE TABLE` returns status information.
  - `CHECK TABLE` is used to check tables for a variety of problems. Indexes are also checked on a MyISAM table. `CHECK TABLE` supports a series of modes for use with MyISAM tables. `CHANGED` checks tables that have changed since the last check, `EXTENDED` performs the most thorough check, `FAST` only checks tables that were not closed properly, `MEDIUM` checks all deleted links and performs key verification, and `QUICK` performs a quick scan only. As seen here, `CHECK TABLE` found and repaired a problem:
  - If MyISAM table access produces incorrect and inconsistent results, you might need to repair the table using `REPAIR TABLE`. This statement should not be used frequently, and if regular use is required, there is likely a far bigger problem that needs addressing.
  - If you delete large amounts of data from a table, `OPTIMIZE TABLE` should be used to reclaim previously used space, thus optimizing the performance of the table.

```mysql
ANALYZE TABLE orders;
```

```mysql
CHECK TABLE orders, orderitems;
```

## Diagnose Startup Problems

Server startup problems usually occur when a change has been made to MySQL configuration or the server itself. MySQL reports errors when this occurs, but because most MySQL servers are started automatically as system processes or services, these messages might not be seen.

When troubleshooting system startup problems, try to manually start the server first. The MySQL server itself is started by executing `mysqld` on the command line. Here are several important command `mysqld` line options:
  - `--help` displays help, a list of options.
  - `--safe-mode` loads the server minus some optimizations.
  - `--verbose` displays full text messages (use in conjunction with --help for more detailed help messages).
  - `--version` displays version information and then quits.

Several additional command-line options (pertaining to the use of log files) are listed in the next section.

## Review Log Files

MySQL maintains a series of log files that administrators rely on extensively. The primary log files are:
  - The error log contains details about startup and shutdown problems and any critical errors. The log is usually named `hostname.err` in the `data` directory. This name can be changed using the `--log-error` command-line option.
  - The query log logs all MySQL activity and can be very useful in diagnosing problems. This log file can get very large very quickly, so it should not be used for extended periods of time. The log is usually named `hostname.log` in the `data` directory. This name can be changed using the `--log` command-line option.
  - The binary log logs all statements that updated (or could have updated) data. The log is usually named `hostname-bin` in the `data` directory. This name can be changed using the `--log-bin` command-line option. Note that this log file was added in MySQL 5; the update log is used in earlier versions of MySQL.
  - As its name suggests, the slow query log logs any queries that execute slowly. This log can be useful in determining where database optimizations are needed. The log is usually named `hostname-slow.log` in the `data` directory. This name can be changed using the `--log-slow-queries` command-line option.

When logging is being used, the `FLUSH LOGS` statement can be used to flush and restart all log files.

