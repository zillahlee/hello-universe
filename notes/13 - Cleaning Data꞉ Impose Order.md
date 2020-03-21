---
tags: [Notebooks/HF Data Analysis]
title: '13 - Cleaning Data: Impose Order'
created: '2020-03-11T19:29:25.802Z'
modified: '2020-03-13T11:12:41.935Z'
---

# 13 - Cleaning Data: *Impose Order*

> Text manipulation tools help you to whip the messy data into something useful.

## Steps of Data Preparation
- First, **ask the client** what she wants to do with the data once it's cleaned.
  - e.g., **what** pieces of information **is in need**?
- Then, go through an **iterative process**
  1. **Save a Copy** of the original data
  2. **Previsualize** what you want the final dataset to look like
  3. **Identify Repetitive Patterns** in the messy data
  4. **Clean** and **Restructure**
  5. **Use** the finished data

## Tools for Data Preparation
- Splitting with **Delimiters**
  - e.g., Excel <kbd>Text to Columns</kbd> button
- Removing **Junk Characters**
  - e.g., Excel `SUBSTITUTE` formula
  - ***readability**: nested functions vs. breaking function into pieces*
- **Regular Expressions** for complex patterns
- **Duplicate** entries
  - **Sort** them!

*With lots or recods, always consider **sorting** by different fields. Sorting lets you **visualize groupings** to find duplicates and/or other weirdness.*

*Sometimes, repetition results from RDBMS queries, rather than simply poor data quality.*
