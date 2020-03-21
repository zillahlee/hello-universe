---
tags: [Notebooks/HF Data Analysis]
title: '08 - Heuristics: Analyze Like a Human'
created: '2020-03-11T19:25:21.879Z'
modified: '2020-03-15T12:39:56.264Z'
---

# 08 - Heuristics: *Analyze Like a Human*

> Give people a hard question, they'll answer an easier one.

## Infinite Reality vs. Finite Brain Power
- The real world has **more variables than you can handle**:
  - you might **not have the data or time** to use optimization methods.
- Some problems are too "expensive" (economically and/or cognitively) to answer
  - so a **simplified approach** might be the only option.

## Heuristics
- Heuristics are a **middle ground** between gut instinct and optimization

**Intuition** <-----> **Heuristics** <-----> **Optimization**
| Intuition | **Heuristics** | Optimization |
| :---: | :---: | :---: |
| seeing one option | seeing a few options | seeing all options |
| try to avoid | most of the thinking | an ideal |
| :no_entry: | :accept: | :100: |

*heuristics does not guarantee optimality, but optimization is usually unfeasible*

> ***Define heuristics (n.)***
>> ***[psychology]*** learning with **past experience + reasoning**; rules of thumb.
>
>> ***[computer science]*** using **prior experience** to solve problems, **without proving it is always right**.

#### Fast and Frugal Tree
- a schematic way to describe a heuristic
- Practically, using a heuristic approach means **picking several key variables** for analysis, which are reflected in the fast and frugal tree.

> **Stereotypes** are heuristics.
>> They are easy and fast, but might **predispose** you to make **inadequate judgement / poorly reasoned conclusions**.

## On Rationality :bulb:
- :+1: Data analysis is all about **breaking down problems into manageable pieces** and **fitting mental and statistical models to data** to make better judgements. There's no guarantee that you’ll always get the right answers.

- In **computer science**, heuristic algorithms have an ability to solve problems without people being able to prove that the algorithm will always get the right answer. Many times, heuristic algorithms in computer science can solve problems more quickly and more simply than an algorithm that guarantees the right answer, and often, the only algorithms available for a problem are heuristic.
- **Psychologists** have found in experimental research that people use cognitive heuristics all the time. There is just too much data competing for our attention, so we have to use rules of thumb in order to make decisions. There are a number of classic ones that are part of the hard-wiring of our brain, and on the whole, they work really well.

- People who have a strong sense of **humans as rational creatures** might be upset by the notion that we use quick and dirty rules of thumb rather than think through all our sensory input in a more thorough way.
- If **rationality** is an ability to *process every bit of a huge amount of information at lightning speed*, to *construct perfect models to make sense of that information*, and then to *have a flawless ability to implement whatever recommendations your models suggest*, then yes, ***you’re irrational***.

- Computer programs like Solver live in a cognitive world where you determine the inputs. And **your choice of inputs is subject to all the limitations of your own mind and your access to data**. But within the world of those inputs, Solver acts with perfect rationality.
- The data you choose as inputs might never cover every variable that has a relationship to your model; you just have to pick the important ones?
  - With data analysis, it’s all about the tools. A good data analyst knows **how to use his tools to manipulate the data in the context of solving real problems**. There’s no reason to get all fatalistic about how you aren’t perfectly rational. ***Learn the tools, use them wisely, and you’ll be able to do a lot of great work.***

- There is no way of doing data analysis that guarantees correct answers. 
  - Analyzing **where and how you expect reality to deviate from your analytical models** is a big part of data analysis.
