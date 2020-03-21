---
tags: [Notebooks/MySQL CC]
title: 09 - Searching with RegEx
created: '2020-03-12T04:14:17.125Z'
modified: '2020-03-17T14:27:13.653Z'
---

# 09 - Searching with RegEx

> for **greater control over data filtering**

## Understanding Regular Expressions
As the complexity of filtering conditions grows, so does the complexity of the `WHERE` clauses themselves.

And this is where regular expressions become useful. Regular expressions are special strings (sets of characters) that are used to match text.
- If you needed to extract phone numbers from a text file, you might use a regular expression.
- If you needed to locate all files with digits in the middle of their names, you might use a regular expression.
- If you wanted to find all repeated words in a block of text, you might use a regular expression.
- And if you wanted to replace all URLs in a page with actual HTML links to those same URLs, yes, you might use a regular expression (or two, for this last example).

Regular expressions are supported in all sorts of programming languages, text editors, operating systems, and more. And savvy programmers and network managers have long regarded regular expressions as a vital component of their technical toolboxes.

Regular expressions are created using the **regular expression language**, a specialized language designed to do everything that was just discussed and much more. Like any language, regular expressions have a special syntax and instructions that you must learn.

## Use MySQL Regular Expressions

> All regular expressions do is match text, comparing a pattern (the regular expression) with a string of text.
>> MySQL provides rudimentary support for regular expressions with `WHERE` clauses, allowing you to specify regular expressions that are used to filter data retrieved using `SELECT`.

> :exclamation: MySQL only supports a small subset of what is supported in most regular expression implementations, and this chapter covers most of what is supported.

### Basic Character Matching
```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000'
ORDER BY prod_name;
```
similar to `WHERE prod_name LIKE '%1000%'` :question:
*retrieves all rows where column prod_name **contains** the text '1000'*

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '.000'
ORDER BY prod_name;
```
similar to `WHERE prod_name LIKE '%000%'` :question:
*`.` is a special character in the regular expression language. It means match **any single character**, and so both 1000 and 2000 matched and were returned.*

### `OR` Matches
- `|` is the regular expression `OR` operator. It means match one or the other. Using `|` is functionally similar to using `OR` statements in `SELECT` statements, with **multiple `OR` conditions being consolidated into a single regular expression**.
- **More than two `OR` conditions** may be specified. For example, `'1000|2000|3000'` would match 1000 or 2000 or 3000.

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000|2000'
ORDER BY prod_name;
```

### Matching One of Several Characters
- `[]` is another form of `OR` statement. In fact, the regular expression `[123] Ton` is shorthand for `[1|2|3] Ton`, which also would have worked. But the `[]` characters are needed to define what the `OR` statement is looking for.
- Sets of characters can also be **negated**. That is, they'll match **anything but** the specified characters. To negate a character set, place a `^` at the start of the set. So, whereas `[123]` matches characters 1, 2, or 3, `[^123]` matches anything but those characters.

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[123] Ton'
ORDER BY prod_name;
```
*`[123]` defines a set of characters, and here it means match 1 or 2 or 3, so both 1 ton and 2 ton matched and were returned (there was no 3 ton).*

***differs from***
```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1|2|3 Ton'
ORDER BY prod_name;
```
*MySQL assumed that you meant '1' or '2' or '3 ton'. **The `|` character applies to the entire string unless it is enclosed with a set.***

### Matching Ranges
- Sets can be used to define one or more characters to be matched.
- `[456789]` simplified as `[4-9]` with `-` declaring it's a range
- `[a-z]` will match any alphabetical character
```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[1-5] Ton'
ORDER BY prod_name;
```

### Matching Special Characters *(aka. Escaping)*
- The regular expression language is made up of special characters that have specific meanings. You've already seen `.`, `[]`, `|`, and `-`, and there are others, too.
- If you needed to match those characters, **precede the special characters with `\\`**.
- To match the backslash character itself `\`, you would need to use `\\\`.

```sql
SELECT vend_name
FROM vendors
WHERE vend_name REGEXP '\\.'
ORDER BY vend_name;
```
*vender names contain '.'*

> `\\` is also used to refer to metacharacters (characters that have specific meanings):

| Metacharacter | Description |
| :--- | :--- |
| \\f | Form feed |
| \\n | Line feed |
| \\r | Carriage return |
| \\t | Tab |
| \\v | Vertical Tab |

> :exclamation: **`\` or `\\`?**: Most regular expression implementation use a single backslash to escape special characters to be able to use them as literals. MySQL, however, requires two backslashes (MySQL itself interprets one and the regular expression library interprets the other).

### Matching Character Classes *(aka. Predefined Character Sets)*
There are matches that you'll find yourself using frequently, digits, or all alphabetical characters, or all alphanumerical characters, and so on. To make working with these easier, you may use predefined character sets known as ***character classes***.

| Class | Description |
| :--- | :--- |
| `[:alnum:]` | Any letter or digit (same as `[a-zA-Z0-9]`) |
| `[:alpha:]` | Any letter (same as `[a-zA-Z]`) |
| `[:lower:]` | Any lowercase letter (same as `[a-z]`) |
| `[:upper:]` | Any uppercase letter (same as `[A-Z]`) |
| `[:digit:]` | Any digit (same as `[0-9]`) |
| `[:xdigit:]` | Any hexadecimal digit (same as `[a-fA-F0-9]`) |
| `[:blank:]` | Space or tab (same as `[\\t]`) |
| `[:space:]` | Any whitespace character including the space (same as `[\\f\\n\\r\\t\\v ]`) |
| `[:cntrl:]` | Any ASCII control characters (ASCII 0 through 31 and 127) |
| `[:punct:]` | Any character that is neither in `[:alnum:]` nor `[:cntrl:]` |
| `[:print:]` | Any printable character |
| `[:graph:]` | Sampe as `[:print:]` but excludes space |

> **ASCII**: American Standard Code for Information Interchange

### Matching Multiple Instances *with Repetition Metacharacters*
All of the regular expressions used thus far attempt to match a single occurrence. If there is a match, the row is retrieved and if not, nothing is retrieved. But sometimes you'll require greater control over the number of matches. For example, you might want to locate all numbers regardless of how many digits the number contains, or you might want to locate a word but also be able to accommodate a trailing s if one exists, and so on.

This can be accomplished using the regular expressions ***repetition metacharacters***.

| Metacharacter | Description |
| :--- | :--- |
| `*` | 0 or more matches |
| `+` | 1 or more matches (equivalent to `{1,}`) |
| `?` | 0 or 1 match (equivalent to `{0,1}`) |
| `{n}` | Specific number of matches |
| `{n,}` | No less than a specified number of matches |
| `{n,m}` | Range of matches (`m` not to exceed 255) |

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)'
ORDER BY prod_name;
```
- *`\\(` matches `(`, and `\\)` matches the closing `)`*
- *`[0-9]` matches any digit (1 and 5 in this example)*
- *`sticks?` matches `stick` and `sticks` (the `?` after the `s` makes that `s` optional because `?` matches 0 or 1 occurrence of whatever it follows). **Without `?` it would have been very difficult to match both `stick` and `sticks`.***

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}'
ORDER BY prod_name;
```
- *`[:digit:]` matches any digit, and so `[[:digit:]]` is a set of digits.*
- *`{4}` requires exactly four occurrences of whatever it follows (any digit)*
- *`[[:digit:]]{4}` matches **any four consecutive digits**.*

***is the same as***
```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[0-9][0-9][0-9][0-9]'
ORDER BY prod_name;
```
> When using regular expressions there is almost always more than one way to write a specific expression.

### Anchors Metacharacters
All of the examples thus far have matched text anywhere within a string. To match text at specific locations, you need to use anchors.

| Metacharacter | Description |
| :--- | :--- |
| `^` | Start of text |
| `$` | End of text |
| `[[:<:]]` | Start of word |
| `[[:>:]]` | End of word |

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '^[0-9\\.]'
ORDER BY prod_name;
```
- *finds all products that started with a number (including numbers starting with a decimal point)*
- *`^[0-9\\.]` matches `.` or any digit **only if they are the first characters within a string**. Without the `^`, four other rows would have been retrieved, too (those that have digits in the middle).*

> :bulb: **`^` Dual Purpose**: `^` has two uses. Within a set (defined using `[` and `]`) it is used to *negate* that set. Otherwise it is used to refer to the *start* of a string.

> :bulb: **Making REGEXP Behave Like `LIKE`**: Earlier in this chapter I mentioned that `LIKE` and REGEXP behaved differently in that `LIKE` matched an entire string and REGEXP matched substrings, too. Using anchors, REGEXP can be made to behave just like `LIKE` by simply **starting each expression with `^` and ending it with `$`**.

> :exclamation: **Not Case-Sensitive**: Regular expression matching in MySQL (as of version 3.23.4) are not case-sensitive (either case will be matched). To force case-sensitivity, you can use the `BINARY` keyword, as in `WHERE prod_name REGEXP BINARY 'JetPack .000'`

> :bulb: **Simple Regular Expression Testing**: You can use `SELECT` to **test regular expressions without using database tables**. REGEXP checks always return 0 (not a match) or 1 (match). You can use REGEXP with **literal strings** to test expressions and to experiment with them. The syntax would look like `SELECT 'hello' REGEXP '[0-9]';`


