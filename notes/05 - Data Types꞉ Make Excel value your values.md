---
tags: [Notebooks/HF Excel]
title: '05 - Data Types: Make Excel value your values'
created: '2020-03-11T19:40:04.835Z'
modified: '2020-03-13T16:54:00.115Z'
---

# 05 - Data Types: *Make Excel value your values*

> Excel generally does a good job at figuring out the data types when you input the data manually in Excel.
>> But you’re most likely to experience data-type changing issues when you load data into Excel that has been exported from another system, like a relational database.

## Type Casting in Excel
- `VALUE()` converts number-stored-as-text to numeric values
- `TEXT()`
- `TYPE()`
- `ISTEXT()`, `ISNONTEXT()`, `ISBLANK()`, `ISREF()`

## Errors

#### Some Common Errors
- `#VALUE!` when the formula is looking for value (number) but received text
- `#REF!` when the references in formula point outside of the spreadsheet
- `#NAME?` when you call a formula whose name doesn't exist
- `#DIV/0!` when you try to divide something by 0, or by a cell that contains no values (numbers)
- `#N/A!` when you forget to enter a required argument into a function

#### Errors are a Special Data Type
> Errors are a big deal in Excel. Understanding how they work is critical to developing tight, functional spreadsheets.
>> By giving errors their own data type, it's made possible to create a number of formulas that handle errors specifically.

- `ISERR()` tells you whether the argument is an error
- `IFERROR()` returns different values, depending on whether the argument is an error
- `ERROR.TYPE()` returns a number that specifies what kind of error is passed as an argument
