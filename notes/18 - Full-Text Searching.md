---
tags: [Notebooks/MySQL CC]
title: 18 - Full-Text Searching
created: '2020-03-12T06:39:11.506Z'
modified: '2020-03-18T08:36:02.003Z'
---

# 18 - Full-Text Searching

- Why full-text searching is used
- Use the MySQL `Match()` and `Against()` functions to perform these searches
- Query expansion as a way to increase the chances of finding related matches
- Boolean mode for more granular lookup control

> :exclamation: **Not All Engines Support Full-Text Searching**: As will be explained in [[21 - Create & Manipulate Tables]], MySQL supports the use of several underlying database engines. Not all engines support full-text searching as is described in this chapter. The two most commonly used engines are **MyISAM** and **InnoDB**; the former supports full-text searching and the latter does not. This is why, although most of the sample tables used in this book were created to use InnoDB, one (the productnotes table) was created to use MyISAM. If you need full-text searching functionality in your applications, keep this in mind.

## Understanding Full Text Search

In [[08 - Wildcard Filtering]], you were introduced to the LIKE keyword that is used to match text (and partial text) using wildcard operators. Using LIKE it is possible to locate rows that contain specific values or parts of values, regardless of the location of those values within row columns.

In [[09 - Searching with RegEx]], text-based searching was taken one step further with the introduction to using regular expressions to match column values. Using regular expressions, it is possible to write very sophisticated matching patterns to locate the desired rows.

But as useful as these search mechanisms are, they have several very important **limitations**:
  - **Performance**: Wildcard and regular expression matching usually requires that MySQL try and match *each and every row in a table* (and *table indexes are rarely of use in these searches*). As such, these searches can be very time-consuming as the number of rows to be searched grows.
  - **Explicit control**: Using wildcard and regular expression matching, it is *very difficult (and not always possible) to explicitly control what is and what is not matched*. An example of this is a search specifying a word that must be matched, a word that must not be matched, and a word that may or may not be matched but only if the first word is indeed matched.
  - **Intelligent results**: Although wildcard- and regular expression-based searching provide for very flexible searching, neither provide an intelligent way to select results. For example, searching for a specific word would return all rows that contained that word, and *not distinguish between rows that contain a single match and those that contained multiple matches* (*ranking* them as potentially better matches). Similarly, searches for a specific word would not find rows that did not contain that word but did contain other *related words*.

All of these limitations and more are addressed by full-text searching. When full-text searching is used, MySQL does not need to look at each row individually, analyzing and processing each word individually. Rather, an **index of the words** (in specified columns) is created by MySQL, and searches can be made against those words. MySQL can thus quickly and **efficiently determine which words match (which rows contain them), which don't, how often they match, and so on**.

## Use Full Text Search

In order to perform full-text searches, the **columns to be searched must be indexed and constantly re-indexed as data changes**. MySQL handles all indexing and re-indexing automatically after table columns have been appropriately designated.

After indexing, `SELECT` can be used with `Match()` and `Against()` to actually perform the searches.

### Enable Full-Text Search Support

Generally, full-text searching is enabled when a table is created. The `CREATE TABLE` statement (which will be introduced in [[21 - Create & Manipulate Tables]]) accepts a `FULLTEXT` clause, which is **a comma-delimited list of the columns to be indexed**.

```sql
CREATE TABLE productnotes
(
  note_id    int           NOT NULL AUTO_INCREMENT,
  prod_id    char(10)      NOT NULL,
  note_date  datetime      NOT NULL,
  note_text  text          NULL,
  PRIMARY KEY(note_id),
  FULLTEXT(note_text)
) ENGINE=MyISAM;
```
*Here `FULLTEXT` indexes a single column, but multiple columns may be specified if needed.*

Once defined, MySQL automatically maintains the index. When rows are added, updated, or deleted, the index is automatically updated accordingly.

`FULLTEXT` may be specified **at table creation time, or later on** (in which case all existing data would have to be immediately indexed).

> :no_entry: **Don't Use `FULLTEXT` When Importing Data**: Updating indexes takes time: not a lot of time, but time nonetheless. If you are importing data into a new table, you ***should not enable `FULLTEXT` indexing at that time***. Rather, ***first import all of the data, and then modify the table to define `FULLTEXT`***. This makes for a much faster data import (and the total time needed to index all data will be less than the sum of the time needed to index each row individually).

### Perform Full-Text Searches

After indexing, full-text searches are performed using two functions: **`Match()` to specify the columns to be searched**, and **`Against()` to specify the search expression to be used**.

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit');
```
*returns two rows containing the word 'rabbit'*
***achieves a similar result as***
```sql
SELECT note_text
FROM productnotes
WHERE note_text LIKE '%rabbit%';
```
Neither of the two `SELECT` statements contained an `ORDER BY` clause. The latter (using `LIKE`) returns data in no particularly useful order. But the former (using full-text searching) returns data ordered by how well the text matched. Both rows contained the word rabbit, but *the row that contained the word rabbit as the third word ranked higher than the row that contained it as the twentieth word*. This is important. An important part of full-text searching is the **ranking of results**. Rows with a higher rank are returned first (as there is a higher degree of likelihood that those are the ones you really wanted).

```sql
SELECT note_text,
       Match(note_text) Against('rabbit') AS result_rank
FROM productnotes;
```
Here `Match()` and `Against()` are used in the `SELECT` instead of the `WHERE` clause. This causes all rows to be returned (as there is no `WHERE` clause). `Match()` and `Against()` are used to create a calculated column (with the alias result_rank) which contains the ranking value calculated by the full-text search. The ranking is calculated by MySQL based on *the number of words in the row*, *the number of unique words*, *the total number of words in the entire index*, and *the number of rows that contain the word*. As you can see, the rows that do not contain the word 'rabbit' have a rank of 0 (and were therefore not selected by the `WHERE` clause in the previous example). The two rows that do contain the word rabbit each have a rank value, and the one with the word earlier in the text has a higher rank value than the one in which the word appeared later.

This helps demonstrate how full-text searching eliminates rows (those with a rank of 0), and how it sorts results (by rank in descending order).

> :bulb: **Ranking Multiple Search Terms**: If multiple search terms are specified, those that contain the most matching words will be ranked higher than those with less (or just a single match).

> :exclamation::question: **Use `Full Match()` Specification**: The value passed to `Match()` must be the same as the one used in the `FULLTEXT()` definition. *If multiple columns are specified, all of them must be listed (and in the correct order).* 

> :exclamation: **Searches Are Not Case Sensitive**: Full-text searches are not case sensitive, ***unless*** `BINARY` mode (not covered in this chapter) is used.

As you can see, full-text searching offers functionality not available with simple `LIKE` searches. And **as data is indexed, full-text searches are considerably faster**, too.

### Query Expansion

Query expansion is used to try to **widen the range of returned full-text search results**. Consider the following scenario. You want to find all notes with references to 'anvils' in them. Only one note contains the word 'anvils', but you also want any other rows that may be related to your search, even if the specific word 'anvils' is not contained within them.

This is a job for query expansion. When query expansion is used, MySQL makes **two passes through the data and indexes** to perform your search:
  1. First, *a basic full-text search* is performed to find all rows that match the search criteria.
  2. Next, MySQL *examines those matched rows and selects all useful words* (we'll explain how MySQL figures out what is useful and what is not shortly).
  3. Then, MySQL performs the *full-text search again, this time using not just the original criteria, but also all of the useful words*.

Using query expansion you can therefore find results that might be relevant, even if they don't contain the exact words for which you were looking.

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('anvils');
```
***differs from***
```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('anvils' WITH QUERY EXPANSION);
```
*The first contains the word 'anvils' and is thus ranked highest. The second row has nothing to do with 'anvils', but as it contains two words that are also in the first row ('customer' and 'recommend') it was retrieved, too. The third row also contains those same two words, but they are further into the text and further apart, and so it was included, but ranked third. And this third row does indeed refer to 'anvils' (by their product name)...*

As you can see, query expansion greatly increases the number of rows returned, but in doing so also increases the number of returns that you might not actually want.

> :bulb: **The More Rows the Better**: The more rows in your table (and the more text within those rows), the better the results returned when using query expansion.

### Boolean Text Searches

MySQL supports an additional form of full-text searching called ***boolean mode***. In Boolean mode you may provide specifics as to
  - **Words to be matched**
  - **Words to be excluded** (if a row contained this word it would not be returned, even though other specified words were matched)
  - **Ranking hints** (specifying which words are more important than others so they can be ranked higher)
  - **Expression grouping**
  - **And more**

> :bulb: **Usable Even Without a `FULLTEXT` Index**: Boolean mode differs from the full-text search syntax used thus far in that *it may be used even if no `FULLTEXT` index is defined*. However, this would be a ***very slow operation*** (and the performance would degrade further as data volume increased).

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('heavy' IN BOOLEAN MODE);
```
*This full-text search retrieves all rows containing the word 'heavy' (there are two of them). The keywords `IN BOOLEAN MODE` are specified, but **no boolean operators are actually specified** and so the results are just as if boolean mode had not been specified.*

> :exclamation: **`IN BOOLEAN MODE` Behaves Differently**: Although the results in this example are the same as they would be without `IN BOOLEAN MODE`, there is an important difference in behavior (even if it did not manifest itself in this particular example). I'll point these out in the use notes later in this chapter.

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('heavy -rope*' IN BOOLEAN MODE);
```
*matches the rows that contain 'heavy' but not any word beginning with 'rope'*

You have now seen two full-text search boolean operators: `-` excludes a word and `*` is the truncation operator (think of it as a wildcard used at the end of a word). The table below lists all of the supported boolean operators.

| Privilege | Description | 
| :---: | :--- |
| `+` | Include, word must be present. | 
| `-` | Exclude, word must not be present. | 
| `>` | Include, and increase ranking value. | 
| `<` | Include, and decrease ranking value. | 
| `()` | Group words into subexpressions (allowing them to be included, excluded, ranked, and so forth as a group). | 
| `~` | Negate a word's ranking value. | 
| `*` | Wildcard at end of word. | 
| `""` | Defines a phrase (as opposed to a list of individual words, the entire phrase is matched for inclusion or exclusion). |

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('+rabbit +bait"' IN BOOLEAN MODE);
```
*matches rows that contain both the words 'rabbit' and 'bait'*

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit bait' IN BOOLEAN MODE);
```
*matches rows that contain at least one of 'rabbit' or 'bait'*

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('"rabbit bait"' IN BOOLEAN MODE);
```
*matches the phrase 'rabbit bait' instead of the two words 'rabbit' and 'bait'*

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('>rabbit <carrot' IN BOOLEAN MODE);
```
*matches both 'rabbit' and 'carrot', increasing the rank of the former and decreasing the rank of the latter*

```sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('+safe +(<combination)' IN BOOLEAN
MODE);
```
*matches the words 'safe' and 'combination', lowering the ranking of the latter*

> :exclamation: **Ranked, but Not Sorted**: In boolean mode, rows will ***not*** be returned sorted descending by ranking score.

### Full-Text Search Usage Notes

Before finishing this chapter, here are some important notes pertaining to the use of full-text searching:
  - When indexing full-text data, **short words are ignored and are excluded from the index**. Short words are defined as those having **three or fewer characters** (**this number can be changed** if needed).
  - MySQL comes with **a built-in list of stopwords**, words that are always ignored when indexing full-text data. This list **can be overridden** if needed. (Refer to the MySQL documentation to learn how to accomplish this.)
  - Many words **appear so frequently** that searching on them would be **useless** (too many results would be returned). As such, MySQL honors a **50% rule**: if a word appears in 50% or more rows, it is treated as a stopword and is effectively ignored. (**The 50% rule is not used for `IN BOOLEAN MODE`**).
    - Full-text searching **never returns any results if there are fewer than three rows in a table** (because every word is always in at least 50% of the rows).
  - **Single quote characters in words are ignored.** For example, "don't" is indexed as 'dont'.
  - *Languages that don't have word delimiters (including Japanese and Chinese) will not return full-text results properly.*
  - As already noted, full-text searching is only supported in the MyISAM database engine.

> :exclamation::question: **No Proximity Operators**: One feature supported by many full-text search engines is proximity searching, ***the ability to search for words that are near each other*** (in the same sentence, in the same paragraph, or no more than a specific number of words apart, and so on). Proximity operators are not yet supported by MySQL full-text searching, although this is planned for a future release.

