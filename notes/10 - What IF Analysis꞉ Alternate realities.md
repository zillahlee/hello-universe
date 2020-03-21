---
tags: [Notebooks/HF Excel]
title: '10 - What IF Analysis: Alternate realities'
created: '2020-03-11T19:40:06.443Z'
modified: '2020-03-15T07:46:12.489Z'
---

# 10 - What IF Analysis: *Alternate realities*

> There are all sorts of **quantitative factors** that can affect how your business will work, how your finances will fare, how your schedule will manage, and so forth. Excel excels at helping you **model and manage all your projections**, **evaluating how changes in those factors will affect the variables you care about most**.

## Scenarios
#### Keep Track of Different Inputs to the Same Model 
- Having the model is one thing, and getting the inputs correct is another. 
- Scenarios is a feature in Excel that helps you keep track of all your different sets of model inputs. It saves different configurations of the elements that change.

#### Scenario Manager
<mark>Data</mark> Tab -> <mark>Forecast</mark> -> <mark>What If Analysis</mark>
- In the dialog box, you can name each of your scenarios and specify which cells change and what the values are for those cells in each scenario.

## Goal Seek
#### Optimizes a Value by Trying a bunch of Different Candidate Values
<mark>Data</mark> Tab -> <mark>Forecast</mark> -> <mark>What If Analysis</mark>
- Goal Seek operates by trying a whole bunch of different values in one cell in order to get a formula in another cell to be equal to the value you want. In the case of finding breakeven points (return on ad = 0), you need Goal Seek to try a bunch of different values in your New Customers cell to figure out which one makes your return equal to zero.
- Goal Seek + Scenario Manager = recording the breakeven scenarios :smile:

#### Model Complexity
:bulb: There are many other details to reality that are not incorporated into the model. If you think you should make your model more complex, you need to distinguish between the issues that affect your goals and those that do not. When you create your own models, you’ll need to be really careful to make sure that you :one: ***incorporate all the relevant variables***, that :two: ***those variables are all linked by the right formulas***, and that :three: ***the values you have for those variables are reasonable***.

> Goal Seek is really all about **setting a single formula to a single value by modifying a single cell**. It ***cannot*** handle:
>> :x: Change the values of **more than one variable**.
>> :x: One of the variables is subject to **constraints**.

## Solver
#### Optimization Problem
- In an optimization problem, you have a target cell you want to maximize, minimize, or set to a value by changing other cells that may be subject to constraints.

#### Solver: an Optional Add-on Utility
<mark>Data</mark> Tab -> <mark>Analyze</mark>
- Solver + Scenario Manager = recording the optimal scenarios :smile:

#### :exclamation: Do a *Sanity Check* on your Solver Model
- Your models somehow needs to recognize that other variables may be changed by a change in the price of baguettes.
- Solver will give you optimal answers, provided that your model is correct. But it doesn’t know whether your model is based in reality.
- You always need to check your formulas to **make sure your model corresponds to reality correctly**.
