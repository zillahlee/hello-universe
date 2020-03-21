---
tags: [Notebooks/HF Data Analysis]
title: '01 - Introduction to Data Analysis: Break It Down'
created: '2020-03-11T17:20:43.085Z'
modified: '2020-03-12T11:45:10.348Z'
---

# 01 - Introduction to Data Analysis: *Break It Down*

## Data Analysis vs. Statistics
> Data analysis is **more than** statistics.
> Data analysts **should learn** as much statistics as possible.


## Data Analysis in 4 Steps
- **Step 1: Define**
  - client view
  - analyst view
- **Step 2: Disassemble**
  - problem breakdown
  - data breakdown
- **Step 3: Evaluate**
  - compare subsets of data
  - compare hypothesis with evidence (from data)
- **Step 4: Decide**
  - make a recommendation
  - for the client to make better decisions

## Mental Models
- Statistical models **depend on** mental models.
- An individual **cannot see everything**; mental models decide what is seen and how the gaps are filled.
- Always make mental models (including uncertainty) **explicit**!
- Always treat mental models **carefully**, whoever it may belong to!
- Everything in your mental model should be **empirically testable**.
- Look for more data to solve crucial uncertainties; **new info** is likely to change your mental model.

## The Data (in summary form, not raw)

```python
import numpy as np
import pandas as pd
```

```python
data = pd.DataFrame({'September': [5280000, 5280000, 1056000, 0, 2.00],
                    'October': [5501000, 5500000, 950400, 105600, 2.00],
                    'November': [5469000, 5729000, 739200, 316800, 2.00],
                    'December': [5480000, 5968000, 528000, 528000, 1.90],
                    'January': [5533000, 6217000, 316800, 739200, 1.90],
                    'February': [5554000, 6476000, 316800, 739200, 1.90]},
                   index=['Gross sales', 'Target sales',
                          'Ad costs', 'Social network costs',
                          'Unit prices (per oz.)'])
data
```


