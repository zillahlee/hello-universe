---
tags: [Notebooks/HF Data Analysis]
title: '07 - Subjective Probabilities: Numerical Belief'
created: '2020-03-11T19:24:50.630Z'
modified: '2020-03-21T04:27:01.023Z'
---

# 07 - Subjective Probabilities: *Numerical Belief*

> **Specify** your subjective beliefs, and **update** them carefully.

<center> <b>Bayes' Rule keeps heads cool</b> :smirk: </center>

## Subjective Probability
- Assign a numerical probability to your **degree of belief in something**.
- Especially useful when **predicting single events** that **lack hard data** to describe what happened previously under identical conditions, but you **do have access to experts' beliefs** about the event(s).

## Standard Deviation
- Measures how far typical points are from the average/mean of the data set, e.g., disagreement among experts' beliefs about a event's probability of happening.
- Most of the points in the data set will be within one standard deviation of the mean.

> For whatever data analysis tools, it's always easy to bamboozle people with "hard data" if that (to deceive) is what you want to do.
>> Therefore, even though "subjective probabilities" **may seem like "hard data"**, be sure to let the client know that subjective probabilities are, **after all, subjective**.

## More Generic Bayes' Rule
*Given the new **E**vidence, the probability of the **H**ypothesis still holds true:*
$$P(H|E)=\frac{P(H)P(E|H)}{P(H)P(E|H) + P(\sim H)P(E|\sim H)}$$
or simplified as
$$P(H|E)=\frac{P(H)P(E|H)}{P(E)}$$
*where $P(E|H)$ is the probability of seeing the **E**vidence, if the **H**ypothesis is true*

- Bayes' Rule is **more rigorous than** using **gut instinct** to change beliefs.
  - It **prevents people from overcompensating** their original subjective probabilities.
