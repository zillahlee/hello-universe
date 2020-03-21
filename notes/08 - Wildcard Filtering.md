---
tags: [Notebooks/MySQL CC]
title: 08 - Wildcard Filtering
created: '2020-03-12T04:14:01.328Z'
modified: '2020-03-16T11:41:12.711Z'
---

# 08 - Wildcard Filtering

> **sophiscated filtering** of retrieved data

> All the previous operators we studied filter against known values. Be it matching one or more values, testing for greater-than or less-than known values, or checking a range of values, the common denominator is that the values used in the filtering are known. But filtering data that way does not always work.
>> Using wildcards, you can create search patterns that can be compared against your data.
>> :new: **Wildcards**: Special characters used to match parts of a value.
>> :new: **Search Pattern**: A search condition made up of literal text, wildcard characters, or any combination of the two.

## Use the `LIKE` Operator

- The wildcards themselves are actually **characters that have special meanings within SQL `WHERE` clauses**, and SQL supports several wildcard types.
- **To use wildcards in search clauses, the `LIKE` operator must be used.** `LIKE` instructs MySQL that the following search pattern is to be compared using a wildcard match rather than a straight equality match.
- Wildcards can be used **anywhere** within the search pattern, and **multiple** wildcards can be used as well.

> :new: **Predicates**: When is an operator not an operator? When it is a predicate. ***Technically, `LIKE` is a predicate, not an operator.*** The end result is the same; just be aware of this term in case you run across it in the MySQL documentation.

### Percent Sign `%` Wildcard
- Within a search string, `%` means **match any number of occurrences of any character**.
- In addition to matching one or more characters, `%` **also matches zero characters**. `%` represents zero, one, or more characters at the specified location in the search pattern.
```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE 'jet%';
```
```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE '%anvil%';
```
```sql
SELECT prod_name
FROM products
WHERE prod_name LIKE 's%e';
```

### Underscore `_` Wildcard
- The underscore is used just like `%`, but instead of matching multiple characters, the underscore matches just **a single character**.
- Unlike `%`, which can match zero characters, `_` always matches one character **no more and no less**.
```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE '_ ton anvil';
```

## Tips for Using Wildcards
MySQL's wildcards are extremely powerful. But that **power comes with a price**: Wildcard searches typically take far longer to process than any other search types discussed previously. Here are some tips to keep in mind when using wildcards:
- **Don't overuse** wildcards. If another search operator will do, use it instead.
- When you do use wildcards, **try to not use them at the beginning of the search pattern unless absolutely necessary**. Search patterns that begin with wildcards are the *slowest to process*.
- Pay careful attention to the **placement of the wildcard symbols**. If they are misplaced, you might not return the data you intended.

> :exclamation: **Case-Sensitivity**: Depending on how MySQL is configured, searches might be case-sensitive, in which case 'jet%' would not match 'JetPack 1000'.

> :exclamation: **Watch for Trailing Spaces**: Trailing spaces can interfere with wildcard matching. One simple solution to this problem is to always append a final `%` to the search pattern. A better solution is to **trim the spaces** using functions, as is discussed in [[11 - Data Manipulation Functions.md]].

> :exclamation: **Watch for `NULL`**: Although it might seem that the `%` wildcard matches anything, there is one exception: `NULL`. Not even the clause `WHERE prod_name LIKE '%'` will match a row with the value `NULL` as the product name.
