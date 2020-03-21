---
tags: [Notebooks/HF Excel]
title: '11 - Text Functions: Letters as data'
created: '2020-03-11T19:40:11.736Z'
modified: '2020-03-15T09:25:59.499Z'
---

# 11 - Text Functions: *Letters as data*

> A lot of times, you’ll receive data that isn’t at all in the format you need it to be in—it might come out of a strange database, for example.
>> Text functions shine at letting you pull elements out of messy data so that you can make analytic use of it.

## <mark>Text to Columns</mark>: *Split Up your Data*
- Text to Columns is a great feature that lets you split your data into columns using a ***delimiter***, which is simply a text character that signifies the breaks between the different data points.
  - If your delimiter is, say, a period, Text to Columns will put the data to the left of the period in one column, the data to the right in another, and then it’ll delete the period.
  > CSV is a really popular file format for data. The letters stand for Comma Separated Value. For these files, commas act as the delimiter. The format is so common that when you load a CSV file, Excel automatically splits the data into columns using the comma delimiter.
- If you have **more than one type of delimiter**, you might have to run Text to Columns more than once.
- If your data points are arranged in columns with the data separated by spaces, click ***Fixed width***.

> Text to Columns doesn't work in all cases
>> Only works with data elements seperated by "useless" delimiters or fixed width.

## Text Functions: *See the Workflow*
#### `LEFT()` & `RIGHT()`: *Text Extraction*
- `LEFT()` Grabs the leftmost text in a cell. You tell it how many characters you want.
  - `=LEFT(target cell, # of characters)`
- `RIGHT()` Returns text from the righthand side of the cell.
  - `=RIGHT(target cell, # of characters)`
- `LEN()` Returns the number of characters in its argument.
- `TRIM()` Removes duplicate spaces and spaces on each end of text in a cell.
- `CONCATENATE()` Returns a value equal to two or more text cells mashed together.

#### `FIND()`: *Specify the Position*
- `FIND()` Returns a number that represents the position of a search string in a cell.
  - `=FIND(“x”, “Head First Excel”)`
- You could use it ***in conjunction*** with a `LEFT()` or `RIGHT()` formula to **extract a number of characters that varies from formula to formula**.

#### `PROPER()`: *The Proper Case*
just takes one argument: the original text
- `=PROPER(H2)`

#### `SUBSTITUTE()`: *Replace A with B*
original text, to be replaced, replaced as
- `=SUBSTITUTE(B2,"*","-")`

## :bulb: Text to Columns vs. Writing Formulas
> Cleaning messy data—which all of us have to do at one point or another— is about ***finding the boundary conditions between your individual data points***. And those boundaries are usually delimiters of some sort. If it’s not a comma or a period, it might be spaces. So most of the field of cleaning messy data involves identifying those boundary conditions and making the software split the data using them.

*Whether and when you use Text to Columns versus formulas is really a personal preference, and there is nothing wrong with using it here. But there is one big, fat reason to use formulas primarily.*

- If you have messy data that has a single, simple pattern to it, you probably wouldn’t have to go back and see how you derived your clean data. If every data point is separated by a delimiter, and you run a Text to Columns, you probably won’t have problems with your cleaned data not squaring with your original data.
- But if the original data is complicated, it’s a different story. Because it’s so messy, you’ve had to do a bunch of things to fix it. In creating the big, formula-filled spreadsheet you used to clean the data, you’ve also set up an audit that you can review if your clean data doesn’t match your messy data perfectly later on. You’d always want to use formulas in situations where you think you might want to go back and trace exactly how your clean data was derived from your messy data.
- If you run it over and over, Text to Columns can usually make some pretty complicated breaks. Just remember that you **sacrifice the ability to go back and tweak the formulas you used to get different results**. Once you run Text to Columns on data, it deletes the original data and leaves you with new columns.

#### <mark>Paste Special</mark>
:exclamation: ***Text to Columns Sees your Formulas, NOT their Results***
- Paste Special is a fantastically helpful operation in Excel that lets you copy something and then—rather than paste an exact copy of the original—paste a modification of the original.
- You just ran Paste Special -> Values to make your data ready for the Text to Columns operation.
- **When finishing up**
  - Copy everything to a new sheet with Paste Special > Values.
  - Delete the columns you no longer need from your new sheet.
