---
tags: [Notebooks/HF Excel]
title: '08 - Formula Auditing: Visualize your formulas'
created: '2020-03-11T19:40:06.017Z'
modified: '2020-03-14T09:54:06.663Z'
---

# 08 - Formula Auditing: *Visualize your formulas*

> a simple but powerful graphical feature of Excel
>> dramatically illustrate the flow of data throughout the models in your spreadsheet

## Models in Excel can get Complicated
- You can define models in a number of ways, depending on what you’re trying to do, but in Excel a “model” is **a network of formulas designed to answer a question**.
- Models can get complicated, and it can be hard to sort them all out. **Unless you can understand the workings of a particular model, don't trust it.**

## Where are your Formula's Arguments Located?
- Formula auditing is an Excel feature that allows you to trace the flow of data through complex formulas.
```excel
=PV(rate,nper)
=FV(rate,nper)
=NPER(rate,pmt,pv)
=RATE(nper,pmt,pv)
=PMT(rate,nper,pv)
```

## Correct Formulas + Reasonable Assumptions
> Model complexity can obscure a host of ills
- It’s easy to create an elaborate spreadsheet that flows data all over the place. It’s really hard to devise a complex model that helps you make good real-world decisions. **Always make sure you understand the models you use, especially the complex ones.**
- For the example of "renting vs. buying" in this chapter, Small changes in **interest and appreciation rates** make all the difference in which strategy is best for you.
  - ***These rates are based on subjective assumptions!***
  - ***So, make sure you plug in reasonable rates to get a reasonable result!***
