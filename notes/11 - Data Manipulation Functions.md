---
tags: [Notebooks/MySQL CC]
title: 11 - Data Manipulation Functions
created: '2020-03-12T06:36:00.045Z'
modified: '2020-03-16T14:57:54.240Z'
---

# 11 - Data Manipulation Functions

## Understanding Functions
Like almost any other computer language, SQL supports the use of functions to manipulate data. Functions are operations that are usually performed on data, usually to facilitate conversion and manipulation.

> :exclamation: **Functions Are Less Portable Than SQL** Code that runs on multiple systems is said to be *portable*. Most SQL statements are relatively portable, and when differences between SQL implementations do occur they are usually not that difficult to deal with. Functions, on the other hand, tend to be far less portable. Just about every major Database Management System (DBMS) supports functions that others don't, and sometimes the differences are significant.
>> With code portability in mind, *many SQL programmers opt not to use any implementation-specific features*. Although this is a somewhat noble and idealistic view, *it is not always in the best interests of application performance*. If you opt not to use these functions, you make your application code work harder. It must use other methods to do what the DBMS could have done more efficiently.
>
>> ***If you do decide to use functions, make sure you comment your code well***, so that at a later date you (or another developer) will know exactly to which SQL implementation you were writing.

## Use Functions

Most SQL implementations support the following types of functions
- **Text functions** are used to manipulate strings of text (for example, trimming or padding values and converting values to upper- and lowercase.
- **Numeric functions** are used to perform mathematical operations on numeric data (for example, returning absolute numbers and performing algebraic calculations).
- **Date and time functions** are used to manipulate date and time values and to extract specific components from these values (for example, returning differences between dates and checking date validity).
- **System functions** return information specific to the DBMS being used (for example, returning user login information or checking version specifics).

### Text Manipulation Functions

| Function | Description |
|  :--- | :--- |
| `Length()` | Returns the length of a string |
| `Lower()` | Converts string to lowercase |
| `Upper()` | Converts string to uppercas |
| `LTrim()` | Trims white space from left of string |
| `RTrim()` | Trims white space from right of string |
| `Left()` | Returns characters from left of string |
| `Right()` | Returns characters from right of string |
| `SubString()` | Returns characters from within a string |
| `Locate()` | Finds a substring within a string |
| `Soundex()` | Returns a string's SOUNDEX value |

```sql
SELECT vend_name, UPPER(vend_name) AS vend_name_upcase
FROM vendors
ORDER BY vend_name;
```

> :new: **SOUNDEX** is an algorithm that converts any string of text into an alphanumeric pattern describing the phonetic representation of that text. SOUNDEX takes into account similar sounding characters and syllables, enabling strings to be compared by how they sound rather than how they have been typed. Although SOUNDEX is not a SQL concept, MySQL (like many other DBMSs) offers SOUNDEX support.
```sql
SELECT cust_name, cust_contact
FROM customers
WHERE Soundex(cust_contact) = Soundex('Y Lie');
```

### Date & Time Manipulation Functions

Date and times are stored in tables using special datatypes using special internal formats so they may be sorted or filtered quickly and efficiently, as well as to save physical storage space.

The format used to store dates and times is usually of no use to your applications, and so date and time functions are almost always used to read, expand, and manipulate these values. Because of this, date and time manipulation functions are some of the most important functions in the MySQL language.

| Function | Description |
|  :--- | :--- |
| `Now()` | Returns the current date and time |
| `CurDate()` | Returns the current date |
| `CurTime()` | Returns the current time |
| `Year()` | Returns the year portion of a dat |
| `Month()` | Returns the month portion of a date |
| `Date()` | Returns the date portion of a date time |
| `Day()` | Returns the day portion of a date |
| `DayOfWeek()` | Returns the day of week for a date |
| `Time()` | Returns the time portion of a date time |
| `Hour()` | Returns the hour portion of a time |
| `Minute()` | Returns the minute portion of a time |
| `Second()` | Returns the second portion of a time |
| `DateDiff()` | Calculates the difference between two dates |
| `Date_Add()` | Highly flexible date arithmetic function |
| `AddDate()` | Add to a date (days, weeks, and so on) |
| `AddTime()` | Add to a time (hours, minutes, and so on) |
| `Date_Format()` | Returns a formatted date or time string |

**Filtering by date** requires some extra care, and the use of special MySQL functions.

The first thing to keep in mind is the date format used by MySQL. **Whenever you specify a date, be it inserting or updating table values, or filtering using `WHERE` clauses, the date must be in the format `yyyy-mm-dd`.** So, for September 1st, 2005 specify `2005-09-01`. Although other date formats might be recognized, this is the preferred date format because it **eliminates ambiguity** (after all, is 04/05/06 May 4th 2006, or April 5th 2006, or May 6th 2004, or... you get the idea).

> :exclamation: **Always Use Four-Digit Years**: Two-digit years are supported, and MySQL treats years 00-69 as 2000-2069 and 70-99 as 1970-1999. While these might in fact be the intended years, it is far safer to always use a full four-digit year so MySQL does not have to make any assumptions for you.

```sql
SELECT cust_id, order_num
FROM orders
WHERE order_date = '2005-09-01';
```
*`WHERE order_date = '2005-09-01'` works because the values in our example tables all have times of `00:00:00`. However, if the stored order_date value is `2005-09-01 11:30:05`, the intended record(S) will not be retrieved because the `WHERE` match will fail.*
***to be safe, always extract the specific part, as the one below***

```sql
SELECT cust_id, order_num
FROM orders
WHERE Date(order_date) = '2005-09-01';
```
*The solution is to instruct MySQL to only compare the specified date to the **date portion** of the column instead of using the entire column value. To do this you must use the `Date()` function. `Date(order_date)` instructs MySQL to **extract just the date part** of the column,*


```sql
SELECT cust_id, order_num
FROM orders
WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-09-30';

```
***is the same as***
```sql
SELECT cust_id, order_num
FROM orders
WHERE Year(order_date) = 2005 AND Month(order_date) = 9;
```

> :exclamation: **MySQL Version Differences**: Many of the MySQL date and time functions were added in MySQL 4.1.1. If you are using an earlier version of MySQL, be sure to consult your documentation to determine exactly which of these functions is available to you.

### Numeric Manipulation Functions

These functions tend to be used primarily for algebraic, trigonometric, or geometric calculations and, therefore, are not as frequently used as string or date and time manipulation functions.

The ironic thing is that of all the functions found in the major DBMSs, the numeric functions are the ones that are most uniform and consistent.

| Function | Description |
| :--- | :--- |
| `Abs()` | Returns a number's absolute value |
| `Mod()` | Returns the remainder of a division operation |
| `Exp()` | Returns the exponential value of a specific number |
| `Sqrt()` | Returns the square root of a specified number |
| `Rand()` | Returns a random number |
| `Pi()` | Returns the value of pi |
| `Cos()` | Returns the trigonometric cosine of a specified angle |
| `Sin()` | Returns the trigonometric sine of a specified angle |
| `Tan()` | Returns the trigonometric tangent of a specified angle

