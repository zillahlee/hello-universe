---
favorited: true
tags: [Mine]
title: 03 - Viz Methods
created: '2020-03-11T19:26:38.148Z'
modified: '2020-03-14T10:19:15.347Z'
---

# 03 - Viz Methods

## Distribution
- **histogram**
  ```sns.distplot(a=*[''], label='', kde=False)```
- **KDE plot**
  ```sns.kdeplot(data=*[''], label='', shade=True)```
- **joint plot**
  ```sns.jointplot(x=*[''], y=*[''], kind='kde')```
- density plot?

## Relationships between Variables
- **scatterplot**
  ```sns.scatterplot(x=*[''], y=*[''])```
  ```sns.scatterplot(x=*[''], y=*[''], hue=*[''])``` *hue for categorical variable*
- **regplot**
  ```sns.regplot(x=*[''], y=*[''])```
- **lmplot**
  ```sns.lmplot(x='', y='', hue='', data=*)``` *hue for categorical variable*
#### Compare Categories
- **bar chart**
  ```sns.barplot(x=*.index, y=*[''])```
- **heatmap**
  ```sns.heatmap(data=*, annot=True)```
- **swarmplot**
  ```sns.swarmplot(x=*[''], y=*[''])``` *x is categorical variable*

## Trends (Patterns of Change Over Time)
- **line graph**
  ```sns.lineplot(data=*)```
  ```sns.lineplot(data=*[], label='')```

## Using <mark>seaborn</mark>
### set up
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
print('setup complete')
```

### before plotting
```python
sns.set_style('dark/white/darkgrid/whitegrid/ticks')
plt.figure(figsize=(12, 6))
plt.title('')
```

### after plotting
```python
plt.xlabel('')
plt.ylabel('')
plt.legend()
```

