---
tags: [Notebooks/HF Excel]
title: '06 - Dates and Times: Stay on time'
created: '2020-03-11T19:40:05.454Z'
modified: '2020-03-14T06:39:39.551Z'
---

# 06 - Dates and Times: *Stay on time*

> Dates and times are a really special case of the data types and formatting that you already know.

:bulb: dates/times = number *(what Excel uses)* + formatting *(what u see)*

## Dates As Integers
- `VALUE()` returns a number on dates stored as text
- In Excel, a date is just an integer. Excel for Windows defines the integer 0 as January 1, 1900,* so the integer 1000 represents 1,000 days after January 1, 1900.

| Excel | Human    |
| ----: | -------: |
| 0     | 1/1/1990 |
| 40334 | 6/5/2010 |
  > This is how Excel deals with dates: by converting them to integers, even though Excel applies formatting to the dates so that you can read them.

- Subtracting one date from another tells you the number of days between the two dates
  - e.g., the first 10k race - today
  - `=DATEVALUE("6/12/10")-DATEVALUE("6/5/10")`
  - When subtracting dates, watch your formatting
    *Excel based the format of its return value on theformat of the cells that went into the arguments of the formula. Just reformat your formulas to **General**.*

> Most people who need to do date computations are going to need more power than counting days through simple arithmetic provides. It makes sense that Excel would have more versatile formulas....

## Date Differences
`=DATEDIF(start date, end date, interval)`
- With `DATEDIF()`, you specify a start date, an end date, and then a text constant that represents the **unit** you want to use.
  - `"d"` for the number of days between the dates
  - `"m"` for the number of **whole months** between the dates
  - `"y"` for the number of **whole years** between the dates
  - `"md"` for the number of days between the dates
    - *ignoring months and years*
  - `"yd"` for the number of days between the dates
    - *ignoring the years*
  - `"ym"` for the number of months between the dates
    - *ignoring days and years*

## Time As Decimal Numbers
- When you type a time into your spreadsheet, Excel displays that time as a value like what you see on the left. But what you’re really looking at is a decimal number between from 0 to 1 that’s formatted to look like a time.

| Excel     | Human       |
| ----:     | ----:       |
| 0         | 12:00 AM    |
| 0.1       | 2:24 AM     |
| 0.551     | 1:13:26 PM  |
| 0.75      | 18:00       |
| 0.9999843 | 11:59:59 PM |
*if you are doing really heavy time computations, you can have Excel’s decimal numbers go all the way to thousands of a second*

## Date & Time Combined
> Dates are numbers to the left of the decimal point, and times are numbers to the right of the decimal point, so what about values with numbers on both sides of the decimal point?
- Just change the format! e.g., `mm/dd/yyyy hh:mm AM/PM`
- 40336.20833 -> 06/07/2010 05:00 AM
