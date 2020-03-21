---
tags: [Notebooks/MySQL CC]
title: 27 - Globalization & Localization
created: '2020-03-12T06:41:17.114Z'
modified: '2020-03-18T16:00:27.122Z'
---

# 27 - Globalization & Localization

> the basics of how MySQL handles different character sets and languages

## Understanding Character Sets & Collation Sequences

Database tables are used to store and retrieve data. Different languages and character sets need to be stored and retrieved differently. As such, MySQL needs to accommodate different character sets (different alphabets and characters) as well as different ways to sort and retrieve data.

When discussing multiple languages and characters sets, you will run into the following important terms:
  - :new: **Character Sets** are collections of letters and symbols.
  - :new: **Encodings** are the internal representations of the members of a character set.
  - :new: **Collations** are the instructions that dictate how characters are to be compared.

> :bulb: **Why Collations Are Important**: Sorting text in English is easy, right? Well, maybe not. Consider the words APE, apex, and Apple. Are they in the correct sorted order? That would depend on whether you wanted a ***case-sensitive or a not case-sensitive sorting***. The words would be sorted one way using a case-sensitive collation, and another way using a not case-sensitive collation. And this affects more than just sorting (as in data sorted using `ORDER BY`); it ***also affects searches*** (whether or not a `WHERE` clause looking for apple finds APPLE, for example). The situation gets even more complex when characters such as the French à or German ö are used, and even more complex when non-Latin-based character sets are used (Japanese, Hebrew, Russian, and so on).

In MySQL there is not much to worry about during regular database activity (`SELECT`, `INSERT`, and so forth). Rather, **the decision as to which character set and collation to use occurs at the server, database, and table level.**

## Work with Character Sets & Collation Sequences  

MySQL supports a vast number of character sets. To see the complete list of supported character sets:

```mysql
SHOW CHARACTER SET;
```
*This statement displays all available character sets, along with the description and default collation for each.*

To see the complete list of supported collations:

```mysql
SHOW COLLATION;
```
*This statement displays all available collations, along with the character sets to which they apply.You will notice that several character sets have more than one collation. latin1, for example, has several for different European languages, and many appear twice, once case sensitive (designated by _cs) and once not case sensitive (designated by _ci).*

**A default character set and collation are defined (usually by the system administration at installation time).** In addition, **when databases are created, default character sets and collations may be specified, too.** To determine the character sets and collations in use:
```mysql
SHOW VARIABLES LIKE 'character%';
SHOW VARIABLES LIKE 'collation%';
```
**In practice, character sets can *seldom* be server-wide (or even database-wide) settings.** Different tables, and even different columns, may require different character sets, and so both may be specified when a table is created.

To specify a character set and collation for a table, `CREATE TABLE` is used with additional clauses:

```mysql
CREATE TABLE mytable
(
   columnn1   INT,
   columnn2   VARCHAR(10)
) DEFAULT CHARACTER SET hebrew
  COLLATE hebrew_general_ci;
```
*This statement creates a two column table, and specifies both a character set and a collate sequence.*

In this example both `CHARACTER SET` and `COLLATE` were specified, but if only one (or neither) is specified, this is how MySQL determines what to use:
  - If both `CHARACTER SET` and `COLLATE` are specified, those values are used.
  - If only `CHARACTER SET` is specified, it is used along with the default collation for that character set (as specified in the SHOW CHARACTER SET results).
  - If neither `CHARACTER SET` nor `COLLATE` are specified, the database default is used.

In addition to being able to specify character set and collation table wide, **MySQL also allows these to be set per column**:

```mysql
CREATE TABLE mytable
(
   columnn1   INT,
   columnn2   VARCHAR(10),
   column3    VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_general_ci
) DEFAULT CHARACTER SET hebrew
  COLLATE hebrew_general_ci;
```
*Here `CHARACTER SET` and `COLLATE` are specified for the entire table as well as for a specific column.*

As mentioned previously, the collation plays a key role in sorting data that is retrieved with an `ORDER BY` clause. If you need to **sort specific `SELECT` statements using a collation sequence other than the one used at table creation time**, you may do so in the `SELECT` statement itself:

```mysql
SELECT * FROM customers
ORDER BY lastname, firstname COLLATE latin1_general_cs;
```
*This `SELECT` uses `COLLATE` to specify an alternate collation sequence (in this example, a case-sensitive one). This will obviously affect the order in which results are sorted.*

> :bulb: **Occasional Case Sensitivity**: The `SELECT` statement just seen demonstrates a useful technique for performing case-sensitive searches on a table that is usually not case sensitive. And of course, the reverse works just as well.

> :bulb: **Other `SELECT COLLATE` Clauses**: In addition to being used in `ORDER BY` clauses, as seen here, `COLLATE` can be used with `GROUP BY`, `HAVING`, aggregate functions, aliases, and more. :open_mouth:

One final point worth noting is that **strings may be converted between character sets if absolutely needed**. To do this, use the `Cast()` or `Convert()` functions.

