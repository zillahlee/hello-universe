---
tags: [Notebooks/HF Data Analysis]
title: '10 - Regression: Prediction'
created: '2020-03-11T19:28:10.314Z'
modified: '2020-03-21T07:20:40.763Z'
---

# 10 - Regression: *Prediction*

> :bulb: $Data Analysis = Hypothesis Testing + Prediction$ *(generally speaking)*

## Prediction

| input | algorithm | output |
| :--- | :--- | :--- |
| Independent Variable(s) | $Regression Equation$ | Dependent Variables |

- **Things to Predict**:
  people's actions, events, experimental results, etc.
- **Questions to Ask**:
  enough data available? qualitative/quantitative data? effectiveness of prediction? limits of prediction? how to use your prediction algorithm?

:bulb: In order to **compare meaningfully**, we need to compare **two/multiple things of the same kind**.
i.e., the subsets of data we are comparing should be **describing the same variable** of two/multiple subsets of the subjects, *not* just using the same metric.

## Scatterplot

Scatterplots show rich **patterns** quickly.
Consider use them whenever you have **two variables describing the same underlying subject**.

By itself, scatterplots show **association** only.
*Strong qualitative theory* is needed to **explain** the association.
i.e., Scatterplots can **demonstrate** causality, bubt they do ***not* prove** causality.

Within a scatterplot, we can make a **Graph of Averages**, which shows the predicted y-axis value for each strip on x-axis.

## Regression

**Regression Line** is the line that best fits the points on that graph of averages.

Linear regression line is **useful** if the data shows a **linear correlation** (usefullness measured by $Corr(X,Y)$).

**Linear Equation** describes the linear regression line **algebraically**.

> the term "regression" is more a historical artifact than something analytically illuminating.

## Sense Making

:bulb: Always do a **reality check**: Does the "prediction" make sense?
It's always up to you to make sure that it **makes sense to try predicting one variable based on another variable**.

$Useful Regression = Solid Association Between Variables + Make Sense$

Depending on the **problem domain**, your regression might need to be **updated** or **completely changed** some day.

## Statistics Equations

*for samples, not population 均为样本估计*

| what it is | how it's calculated |
| :--- | :--- |
| **Covariance** 协方差 [+, -, 0] | $Cov(X,Y)=\frac{\sum_{i=1}^n(X_i-\bar X)(Y_i-\bar Y)}{n-1}$ |
| **Variance** 方差 | $Var(X)=\frac{\sum_{i=1}^n(X_i-\bar X)^2}{n-1}$ |
| **Standard Deviation** 标准差 | $\sigma_x=\sqrt{Var(X)}$ |
| **Correlation Coefficient** 相关系数 [-1, 1] | $Corr(X,Y)=\frac{Cov(X,Y)}{\sqrt{Var(X)}\sqrt{Var(Y)}}=\frac{Cov(X,Y)}{\sigma_x\sigma_y}$ |
| **Linear Regression Line** 线性回归线 | $y=a+bx$ |
| **Linear Regression Coefficients** 线性回归系数 | $a$: y-axis intercept; $b=\frac{Corr(X,Y)\sigma_y}{\sigma_x}$ |

- **Covariance** 协方差: 本身只用于判断$+$/$-$/$0$相关，其绝对值无意义，且受scale影响很大
- **Variance** 方差: 协方差的一种special case (when $x=y$)，两变量相同时，即自身比较
- **Standard Deviation** 标准差: 描述数据组的离散程度
- **Correlation Coefficient** 相关系数: 描述变量直接的相关程度，范围$[-1, 1]$，$0$表示完全不相关
- **Linear Regression Line** 线性回归线: 回归线的代数表达
- **Linear Regression Coefficients** 线性回归系数: 回归线的常量，截距 $a$，坡度(slope) $b$

