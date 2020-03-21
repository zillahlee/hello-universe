---
tags: [Notebooks/HF Excel]
title: '13 - Booleans: TRUE & FALSE'
created: '2020-03-11T19:40:12.182Z'
modified: '2020-03-15T13:52:19.655Z'
---

# 13 - Booleans: *TRUE & FALSE*

> Plug Boolean values into logical formulas to do a variety of tasks, from cleaning up data to making whole new data points.

:bulb:
| Input | Output |
| :---- | -----: |
| Boolean Expression | `TRUE`/`FALSE` |
| `IF()` | value1/value2 |
| `COUNTIF()`, `COUNTIFS()` | # of matches |

## Boolean & `IF()`

#### Boolean Expression
A *Boolean expression* is **a formula** or **argument to a formula** that returns a value of `TRUE` or `FALSE`. It’s **often used to compare two values**.
| Expression | Result |
| :--------- | -----: |
| 1 = 1 | `TRUE` |
| 3.334 > 5 | `FALSE` |
| "Head" = `LEFT("Head First",4)` | `TRUE` |
| `EXACT("Hi","HI")` | `FALSE` |
| `SUM(2,3)` = 1+4 | `TRUE` |

#### `IF()`: *a Logical Function*
**`=IF(boolean expression, value if true, value if false)`**
*nested* `=IF(exp1,value1,IF(exp2,value2,value3))`
*2019 New* `IFS()`: `=IFS([exp1,value1,exp2,value2,exp3,value3)`
- `IF()` gives results based on a Boolean condition.
- If you stick a Boolean expression inside an `IF()` formula, you can have your formula return any value you want instead of returning `TRUE` or `FALSE`.
- The problem is that `IF()` doesn’t evaluate five options in order to return one or two answers. It just **looks at one Boolean expression at a time**. So you need to take the ***complex logic*** of boat ID assignments and ***convert*** it ***into*** a series of ***linear decisions***. That way, you’ll be able to write the `IF()` formula that gives you the right answer.

> Check out other *Logical Functions* in <mark>Excel functions (by category)</mark>

## `COUNTIF()` & `COUNTIFS()`: *Statistical Functions*
- `COUNT()` Counts how many **numbers** are in the list of arguments
- `COUNTA()` Counts how many **values** are in the list of arguments
- `COUNTBLANK()` Counts the number of **blank cells** within a range
- `COUNTIF()` Counts the number of cells within a range that **meet the given criteria**
- `COUNTIFS()` Counts the number of cells within a range that **meet multiple criteria**
  - It also can count based on single criteria, so it has all the functionality of `COUNTIF()` and more.

## Working with Complex Conditions
- :bulb: Break your formula apart into columns
- and combine them with logical functions like `AND()`, `OR()`
