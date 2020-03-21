---
tags: [Notebooks/HF Data Analysis]
title: '11 - Error: Err Well'
created: '2020-03-11T19:28:32.758Z'
modified: '2020-03-21T07:56:10.244Z'
---

# 11 - Error: *Err Well*

> :bulb: Before building any models, if you don't have the population data, make sure the **sample** is **representative** of the population.

> :bulb: Regression **predicts** what the outcome would be "on average", but obviously *not* everything is going to be exactly at the **average**.

## Range Error: *Extrapolation*

*Intrapolation* is what regression designed to do.
*Extrapolation* happens when you predict **outside the range of data**.

People extrapolate all the time;
but if you do, always specify additional **assumptions**.
i.e., **make explicit your ignorance outside the range**.

:bulb: Always **evaluate** any model's **assumptions** carefully :exclamation:


## Chance Error: *Residuals*

*Residuals* is how much **outcomes deviate from prediction**.
When possible, **interpreting residuals correctly** lets you better understand the data and understand the usage of the model.

**Residual Distribution** is the spread of chance error. Calculated as:
$$RootMeanSquare(RMS) = \sigma_y \sqrt{1-Corr(x,y)^2}$$
aka *Residual Standard Error*, which is one way to **measure error**.

## Dealing with Errors

$Predict + IgnoreError = Unrealistic Expectations$
leads to false sense of certainty, plain ignorance

$Predict + SpecifyError = Sensible Expectations$
leads to more knowledge, better decisions

### Segmentation

Segmentation is all about **managing error**.
By **splitting** data into groups and building **multiple predictive models** for **subgroups**, we get less error over all.

### Explain vs. Predict

However, error is *not* "the fewer, the better".
> :smile: You'd risk overfitting the training data, right?

| Explanatory Power | $GoodRegression$ | Predictive Power |
| :--- | :---: | ---: |
| fits every data point, no error | fits most data points | huge residual spread |
| can't predict anything | balance explanation & prediction | not precise enough |

