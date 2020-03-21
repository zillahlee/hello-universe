---
tags: [Notebooks/HF Excel]
title: '14 - Segmentation: Slice & Dice'
created: '2020-03-11T19:40:13.062Z'
modified: '2020-03-15T17:05:27.254Z'
---

# 14 - Segmentation: *Slice & Dice*


1. :bulb: **Define what pieces of data you need** to achieve the objective.
2. :bulb: **See if you have each piece of data directly available**.
3. :bulb: For the pieces not readily available, see **how you can transform** the current pieces into new pieces (add a column, use some calculations and functions, link pieces through functions, etc.). Are the pieces of data in need **categorical or numerical**? Once you know, you will find the **proper transformation tools**.

> You’ve developed a formidable knowledge of Excel in the past 13 chapters, and by now you know (or know how to find) most of the tools that fit your data problems. But **what if your problems don’t fit those tools**? What if you don’t even have the data you need all in one place, or your data is divided into categories that don’t fit your analytical objectives? In this final chapter, you’ll use **lookup functions** along with some of the tools you already know to **slice new segments out of your data** and ***get really creative with Excel’s tools***.

## Transform Your Data
- Data can be close to what you want without ever quite getting there. But that doesn’t mean that you can’t do your analysis. You can just transform the data you have into the data you need to have.

| Data Inputs | Data Outputs |
| :--- | ---: |
| Categorical Data |  New Categories |
| Categorical Data |  Categorical Summaries Using Numbers |
| Numeric Data |  Numeric Calculations |
| Numeric Data |  Categorical Summaries Using Numbers |

:smile: Practice more with Tables & Pivot Tables!

## `VLOOKUP()`: *cross-references two data sources*
`=VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup])`
- "V" stands for vertical. It look up a reference value in a vertical list and then return the value from another column that matches the position of the value in the vertical list.

#### `LOOKUP()`, `HLOOKUP()`, and more

* see Excel documentation for more information

## On Your Own :heartpulse:

> You've learned enough about the features of Excel that all the stuff you don't know consists of either super-advanced topics or subtle variations on the themes you've already picked up. At this point, your goal should be to play with the functions and think creatively about how to make them work for your specific problems.

> **Just as a book on Microsoft Word won't show you how to write the Great American Novel, a book on Excel can't teach you to create a brilliant spreadsheet.**

> The best way to get good with Excel once you have a strong base of knowledge is just to learn as many functions as you can and experiment with making them work together.
