---
attachments: [Clipboard_2020-03-21-16-47-05.png, Clipboard_2020-03-21-17-01-34.png, Clipboard_2020-03-21-17-11-48.png]
tags: [Notebooks/HF Statistics]
title: '01 - Visualizing Information: First Impressions'
created: '2020-03-11T17:25:47.820Z'
modified: '2020-03-21T09:18:54.794Z'
---

# 01 - Visualizing Information: *First Impressions*

> Statistics make the complex simple.

> Visualizations make statistics clear.

## Statistics: What & Why

### What is Statistics?

:new: Statistics are numbers that **summarize raw facts** and figures in some meaningful way. They present key ideas that may not be immediately apparent by just looking at the raw data, and by data, we mean facts or figures from which we can draw conclusions.

The study of statistics covers where statistics come from, how to calculate them, and how you can use them effectively.

| Gather Data | Analyze | Draw Conclusions |
| :--- | :--- | :--- |
| At the root of statistics is data. Data can be gathered by looking through existing sources, conducting experiments, or by conducting surveys. | Once you have data, you can analyze it and generate statistics. You can calculate probabilities to see how likely certain events are, test ideas, and indicate how confident you are about your results. | When you’ve analyzed your data, you make decisions and predictions. |

### Why Learn Statistics?

Understanding what’s really going on with statistics empowers you. If you really get statistics, you’ll be able to make **objective decisions**, make **accurate predictions** that seem inspired, and **convey the message** you want in the most **effective** way possible.

**Dark side of Statistics**: Statistics are based on facts, but even so, they can sometimes be **misleading**. They can be used to tell the truth—or to lie. The problem is how do you know when you’re being told the truth, and when you’re being told lies?

Having a good understanding of statistics puts you in a strong position. You’re much better equipped to tell when statistics are inaccurate or misleading. In other words, studying statistics is a good way of making sure you don’t get fooled.

## Visualize for Patterns & Trends

:bulb: Sometimes it’s **difficult to see what’s really going on just by looking at the raw data**. There can be **patterns** and **trends** in the data, but these can be very hard to spot if you’re just looking at a heap of numbers. Charts give you a way of literally seeing patterns in your data. They allow you to visualize your data and see what’s really going on in a quick glance.

> **Information vs. Data**: Data refers to raw facts and figures that have been collected. Information is data that has some sort of added meaning.

### Visualize Correctly: Pie, Bar, Histogram, Line

**Software can't think for you.**
Chart software can save you a lot of time and produce effective charts, but you still need to understand what’s going on. At the end of the day, it’s your data, and it’s up to you to choose the right chart for the job and make sure your data is presented in the most effective way possible and conveys the message you want. Software can translate data into charts, but it’s up to you to make sure the chart is right.

> :new: The number in a particular group is called the **frequency**. Frequency describes how many items there are in a particular group or interval. It’s like a count of how many there are.

:bulb: The chart you use depends on **which key facts you want to emphasize**. Which chart you use really comes down to what message you want to put across, and what key facts you want to minimize.

| type | why |
| :--- | :--- |
| *Pie Chart* | sense of the whole, subgroups added to 100% |
| *Bar Chart* | greater precision for comparison of categories |
| *Histogram* | continuous numeric data grouped into intervals |
| *Line Graph* | numerical trends by time |
|  |  |

### Frequency & Percentage: *a matter of scale*

Understanding scale allows you to create powerful bar charts that pick out the key facts you want to draw attention to. But be careful—scale can also conceal vital facts about your data. The golden rule for designing charts that show percentages is to try and indicate the frequencies, either on the chart or just next to it.

Be very wary if you’re given **percentages with no frequencies**, or a **frequency with no percentage**. Sometimes this is a tactic used to hide key facts about the underlying data, as just based on a chart, you have no way of telling how representative it is of the data.

For frequencies, normally your **scale should start at 0**, but watch out! Not every chart does this, and as you saw earlier on page , using a scale that doesn’t start at 0 can give a different first impression of your data. This is something to watch out for on other people’s charts, as it’s very easy to miss and can give you the wrong impression of the data.

### Comapre Multiple Sets of Data

With bar charts, it’s actually really easy to show more than one set of data on the same chart.

**Split-category bar chart** is useful if you want to compare frequencies, but it’s difficult to see proportions and percentages.

**Segmented (stacked) bar chart**: If you want to show frequencies and percentages, you can try using a segmented bar chart. For this, you use one bar for each category, but you split the bar proportionally. The overall length of the bar reflects the total frequency. This sort of chart allows you to quickly see the total frequency of each category—in this case, the total number of players for each genre—and the frequency of player satisfaction. You can see proportions at a glance, too.

### Data Types: Categories vs. Numbers

When you’re working with charts, one of the key things you need to figure out is what sort of data you’re dealing with. Once you’ve figured that out, you’ll find it easier to make key decisions about what chart you need to best represent your data.

### Create Histograms

**Histograms** present the data using a **continuous numeric scale**. Instead of using bars to represent a single item, we can use each bar to represent a range of scores.

**Histograms vs. Bar Charts**
Histograms are like bar charts but with two key differences. The first is that the **area** of each bar is **proportional to the frequency**, and the second is that there are **no gaps** between the bars on the chart.

The first step to creating a histogram is to look at each of the intervals and work out how wide each of them needs to be, and what range of values each one needs to cover. If each **interval** has the **same width**, the height of each bar is equal to the frequency. For intervals with **different widths**, we need to **adjust the bar heights** to make sure the area of each bar is proportional to the frequency.

> :bulb: **Single Boundary**: The bars have to meet, and it’s usually at the **midway** point, but it all comes down to how you round your values. When you round values, you normally round them to the nearest whole number. This means that the range of values from -0.5 to 0.5 all round to 0, and so when we show 0 on a histogram, we show it using the range of values from -0.5 to 0.5.
>> **Exception**: If you have to represent the **age range** 18–19 on a histogram, you would normally represent this using an interval that goes from 18 to 20. The reason for this is that we typically classify someone as being 19, for example, up until their 20th birthday. In effect, we round ages down.

1. **Find the Bar Width**
  We find how wide our bars need to be by looking at the range of values they cover.
2. **Find the Bar Heights**
  $$Area_{ofBar} = Frequency_{ofGroup}$$
  $$Frequency = Width_{ofBar} \times Height_{ofBar}$$
  $$Height_{ofBar} (Frequency Density) = \frac {Frequency} {Width_{ofBar}}$$
  The **height** of the bar is used to measure how **concentrated** the frequency is for a particular group. It’s a way of measuring how densely packed the frequency is, a way of saying how thick or thin on the ground the numbers are. The height of the bar is called the **frequency density**.
3. **Draw the Histogram**
  Use frequency density for the vertical axis (height).
  *Legend is optional. It makes clear what the area represents.*

> :new: **Frequency density** refers to the concentration of values in data. It’s related to frequency, but it’s not the same thing. Just as a wider glass means the juice comes to a lower level, a wider bar means a lower frequency density. It gives you a way of comparing different intervals that may be different widths.

### Cumulative Frequency: *running totals*

> :new: **Cumulative Frequency**: The total frequency up to certain value. It’s basically a running total of the frequencies. The cumulative frequency of a value is the sum of the frequencies **up to and including** that value. It tells you the total frequency up to that point.

While histograms are an excellent way to display grouped numeric data, there are still some kinds of this data they’re not ideally suited for presenting—like running totals…

Draw two axes, with the vertical one for the cumulative frequency and the horizontal one for the hours. Once you’ve done that, plot each of the upper limits against its cumulative frequency, and then join the points together with a line.

The **cumulative frequency graph** is **a special type of line chart** that shows the total frequency up to a certain value.

**Line charts** are good at showing **trends** in your data. For each set of data, you plot your points and then join them together with lines. You can easily show multiple sets of data on the same chart without it getting too cluttered. Just make sure it’s clear which line is which. As with other sorts of charts, you have a choice of showing **frequency or percentages** on the vertical axis. The scale you use all depends on what key facts you want to draw out. Line charts are often used to show **time measurements**. Time always goes on the horizontal axis, and frequency on the vertical.

Line charts should be used for **numerical data only**, and not categorical. This is because it makes sense to compare different categories, but not to draw a trend line. Only use a line chart if you’re comparing categories over some numerical unit such as time, and in that case you’d use a separate line for each category.

> :new: **Time Series Charts**: A time series chart is really a line chart that focuses on time intervals, just like the examples we used. A line chart doesn’t have to focus on just time, though.

## Concepts Recap

![](@attachment/Clipboard_2020-03-21-16-47-05.png)
![](@attachment/Clipboard_2020-03-21-17-01-34.png)
![](@attachment/Clipboard_2020-03-21-17-11-48.png)

