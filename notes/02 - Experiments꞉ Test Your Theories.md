---
tags: [Notebooks/HF Data Analysis]
title: '02 - Experiments: Test Your Theories'
created: '2020-03-11T17:22:08.466Z'
modified: '2020-03-12T11:59:25.735Z'
---

# 02 - Experiments: *Test Your Theories*

## Observational Studies
- Observational studies are **not** very powerful for drawing **causal conclusions**
  - Observational data, by itself, cannot tell you what will happen in the future
  - *But they can always be helpful; just know the limitations*
- Observationak studies are full of **confounders**
  - i.e., the **differences among subjects** in respect to **a variable that matters**
- So, we need to **manage confounders** by **breaking the data** into more **internally homogeneous chunks**, with much less variation (the variation might skew the results)

## Experiments
- Experiments should be conducted with control groups (**contemporaneous, not historical**); the control group is the **baseline, or status quo**, for other groups to compare with
- Experiment also have confounders
- For a **comparison** to be valid, the **groups should be the same**

## Randomization
- Randomization selects **similar groups**
- When you **randomly assign subjects to groups**, the **factors that might otherwise become confounders** end up getting **equal representation** among the control and experimented groups
- So now, the groups are **comparable**!

## Customer Survey Data (summary)

```python
import numpy as np
import pandas as pd
```

```python
survey = pd.DataFrame({'Aug': [4.7, 4.9, 3.6, 4.3, 3.9, 100],
                       'Sept': [4.6, 4.9, 4.1, 3.9, 4.2, 101],
                       'Oct': [4.7, 4.7, 4.2, 3.7, 3.7, 99],
                       'Nov': [4.2, 4.9, 3.9, 3.5, 4.3, 99],
                       'Dec': [4.8, 4.7, 3.5, 3.0, 4.3, 101],
                       'Jan': [4.2, 4.9, 4.6, 2.1, 3.9, 100]},
                     index=['Location', 'Temperature', 'Employees',
                           'Value', 'Preffered', 'Store#'])
survey
```

- Step 1. Define the Problem
  - 1.1 client view
  - 1.2 analyst view
- Step 2. Disassemble the Problem and the Data
  - 2.1 problem breakdown
  - 2.2 data breakdown
- Step 3. Evaluate by Comparison
What are you trying to demonstrate? Why?
What are your control and experimental groups going to be?
How will you avoid **confounders**?
What will your results look like?
  - 3.1 trend one:
  - 3.2 trend two:
- Step 4. Decisions to Recommend
  - 4.1 recommendation one:
  - 4.2 recommendation two
