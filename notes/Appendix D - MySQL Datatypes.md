---
tags: [Notebooks/MySQL CC]
title: Appendix D - MySQL Datatypes
created: '2020-03-12T06:43:41.000Z'
modified: '2020-03-12T07:11:48.822Z'
---

# Appendix D - MySQL Datatypes

> As explained in Chapter 1, "Understanding SQL," datatypes are basically rules that define what data may be stored in a column and how that data is actually stored.

Datatypes are used for several reasons:
- Datatypes enable you to restrict the type of data that can be stored in a column. For example, a numeric datatype column only accepts numeric values.
- Datatypes allow for more efficient storage, internally. Numbers and date time values can be stored in a more condensed format than text strings.
- Datatypes allow for alternate sorting orders. If everything is treated as strings, 1 comes before 10, which comes before 2. (Strings are sorted in dictionary sequence, one character at a time starting from the left.) As numeric datatypes, the numbers would be sorted correctly.

*When designing tables, pay careful attention to the datatypes being used. Using the wrong datatype can seriously impact your application. Changing the datatypes of existing populated columns is not a trivial task. (In addition, doing so can result in data loss.)*

*Although this appendix is by no means a complete tutorial on datatypes and how they are to be used, it explains the major MySQL datatype types, and what they are used for.*

## String Datatypes

- **CHAR**
  - Fixed-length string from 1 to 255 chars long. Its size must be specified at create time, or MySQL assumes CHAR(1).
- **ENUM**
  - Accepts one of a predefined set of up to 64K strings.
- **LONGTEXT**
  - Same as TEXT, but with a maximum size of 4GB.
- **MEDIUMTEXT**
  - Same as TEXT, but with a maximum size of 16K.
- **SET**
  - Accepts zero or more of a predefined set of up to 64 strings.
- **TEXT**
  - Variable-length text with a maximum size of 64K.
- **TINYTEXT**
  - Same as TEXT, but with a maximum size of 255 bytes.
- **VARCHAR**
  - Same as CHAR, but stores just the text. The size is a maximum, not a minimum.

> **Using Quotes** *Regardless of the form of string datatype being used, string values must always be surrounded by quotes (single quotes are often preferred).*

> **When Numeric Values Are Not Numeric Values** *You might think that phone numbers and ZIP Codes should be stored in numeric fields (after all, they only store numeric data), but doing so would not be advisable. If you store the ZIP Code 01234 in a numeric field, the number 1234 would be saved. You'd actually lose a digit. The basic rule to follow is this: If the number is a number used in calculations (sums, averages, and so on), it belongs in a numeric datatype column. If it is used as a literal string (that happens to contain only digits), it belongs in a string datatype column.*


## Numeric Datatypes

- **BIT**
  - A bit-field, from 1 to 64 bits wide. (Prior to MySQL 5 BIT was functionally equivalent to TINYINT.)
- **BIGINT**
  - Integer value, supports numbers from -9223372036854775808 to 9223372036854775807 (or 0 to 18446744073709551615 if UNSIGNED).
- **BOOLEAN (BOOL)**
  - Boolean flag, either 0 or 1, used primarily for on/off flags.
- **DECIMAL (DEC)**
  - Floating point values with varying levels of precision.
- **DOUBLE**
  - Double-precision floating point values.
- **FLOAT**
  - Single-precision floating point values.
- **INTEGER (INT)**
  - Integer value, supports numbers from -2147483648 to 2147483647 (or 0 to 4294967295 if UNSIGNED).
- **MEDIUMINT**
  - Integer value, supports numbers from -8388608 to 8388607 (or 0 to 16777215 if UNSIGNED).
- **REAL**
  - 4-byte floating point values.
- **SMALLINT**
  - Integer value, supports numbers from -32768 to 32767 (or 0 to 65535 if UNSIGNED)
- **TINYINT**
  - Integer value, supports numbers from -128 to 127 (or 0 to 255 if UNSIGNED).

> **Signed Or UNSIGNED?**: *All numeric datatypes (with the exception of BIT and BOOLEAN) can be signed or unsigned. Signed numeric columns can store both positive and negative numbers, unsigned numeric columns store only positive numbers. Signed is the default, but if you know that you'll not need to store negative values you can use the UNSIGNED keyword, doing so will allow you to store values twice as large.*

> **Storing Currency**: *There is no special MySQL datatype for currency values, use DECIMAL(8,2) instead.*

## Date & Time Datatypes

- **DATE**
  - Date from 1000-01-01 to 9999-12-31 in the format YYYY-MM-DD.
- **DATETIME**
  - A combination of DATE and TIME.
- **TIMESTAMP**
  - Functionally equivalent to DATETIME (but with a smaller range).
- **TIME**
  - Time in the format HH:MM:SS.
- **YEAR**
  - A 2 or 4 digit year, 2 digit years support a range of 70 (1970) to 69 (2069), 4 digit years support a range of 1901 to 2155.

## Binary Datatypes

- **BLOB**
  - Blob with a maximum length of 64K. 
- **MEDIUMBLOB**
  - Blob with a maximum length of 16MB. 
- **LONGBLOB**
  - Blob with a maximum length of 4GB. 
- **TINYBLOB**
  - Blob with a maximum length of 255 bytes. 





