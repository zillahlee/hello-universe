---
tags: [Notebooks/HF Excel]
title: '04 - Change Your Point of View: Sort, Zoom, Filter'
created: '2020-03-11T19:40:03.184Z'
modified: '2020-03-13T14:48:24.709Z'
---

# 04 - Change Your Point of View: *Sort, Zoom, Filter*

> The details of your data are tantalizing. But only if you **know how to look** at them. This chapter focus on how to change your perspective on your data.
>> When you are **exploring** your data, **looking for issues to investigate**, the sort, zoom, and filter tools offer surprising versatility to help you get a grip on what your data contains.

## `SORT`
- Sort is especially useful when you’re looking at data for the first time and trying to get a feel for what’s in it. When you look at data for the first time, it’s a good idea to **sort by different columns to look for visible patterns**.
  - *Excel can figure out which columns are in your table… usually. If Excel doesn’t sort all your columns together, it can wreck your database Always save your data first and check it after sorting to make sure you and Excel got it right.*
  > This is a reminder of a very important principle of dealing with data: always keep copies of your original data. Once you’ve done an analysis of the data, it’s always a good idea to check your data against the original to make sure that nothing weird happened.

#### Multi-Level Sort
<kbd>Custom Sort</kbd> -> <kbd>Add Level</kbd> "sort by ..., then by ..."

#### Other Sorting Options
- Sort by color (very often people will highlight cells in their spreadsheet to be different colors)
  > Generally there are better ways to tag data than highlighting cells. You can sort by color, but most formulas can’t read your cells’ formatting. So if you want to tag interesting cells, it’s better to add a column and insert your own text or Boolean functions.
- **Custom List**s enable you to create any arbitrary sorting you want.

#### Sorting with Tables
If you define your data set as a table, then you are being really explicit with Excel about the dimensions of your data.

## `ZOOM`
> Looking at the data is a good thing. It’s a nonobvious but important part of data analysis.
>> Formulas and their results might be illuminating, but they take you away from actually looking at the data.
>>> Sometimes you need to look at the forest,
>>> and sometimes you need to look at the trees.
>>> Zooming will let us do it.

## `FILTER`
> hide data you don’t want to see (without changing them)
>> just as with sorting, when you’re exploring a new data set for the first time, it’s a great idea to run filters to look at various subsets of the data.
- Text filters: groups of the same text value
- Numerical filters: groups based on your threshold(s)

#### Ambiguous
- starts with, ends with
- contains, not contains
- wildcards, e.g., "?", "*"
- using AND/OR to combine filters

## Perspective vs. Formulas
> **Good thinking about data is the substance of data analysis, not writing formulas or any other feature of Excel or any other software.**
- Sorting, zooming, and filtering are great tools to use to get a sense of what is inside data that you are looking at for the first time. Sometimes you just need a better perspective on your data, and the way to get at that perspective is literally to look at the data in a bunch of different ways. Your mileage may vary.
- It may be that your specific problem really needs nothing besides the perspective that these visualization tools give you. Or it may be that you need to create a model that summarizes and manipulates the data once you’ve gotten the perspective you need.
- **Formulas, in their most general sense, take data as arguments and return new data.** If your analytic goals aren’t met by simply changing your point of view on the data, chances are you’ll need to hit the data with some formulas to achieve the manipulation or summary that you need.
