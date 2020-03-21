---
tags: [Notebooks/SWD Viz]
title: 02 - Choosing an Effective Visual
created: '2020-03-17T04:33:33.894Z'
modified: '2020-03-19T16:25:05.527Z'
---

# 02 - Choosing an Effective Visual

> There are many different graphs and other types of visual displays of information, but a handful will work for the majority of your needs.

> In many cases, there isn’t a single correct visual display; rather, often there are different types of visuals that could meet a given need. What do you need your audience to know? Then choose a visual display that will enable you to make this clear. The bottomline is to **choose whatever will be the easiest for your audience to read. And test your choice**: Have some friends/colleagues articulate the following as they process the information: where they focus, what they see, what observations they make, what questions they have.

| Type | :bulb: When | Alert |
| :--- | :--- | :--- |
| Simple Text | **just a number or two** | *add a few supporting words* |
| Table | **mixed audience** | *don't used for presentation* |
| Heatmap | **mixed audience + visual cues** | *always include a legend* |
| Scatterplot | **relationship** | *explore whether and what relationship exist between variables (x, y)* |
| Line Graph | **trend by continuous data (time)** | *x-axis (time) should be in consistent intervals* |
| Bar Chart | **categorical comparison** | *always use a zero baseline; pay attention to the choice of horizontal vs. vertical, stacked or not, the number of series, and absolute number vs. percentage* |

## Simple Text

**When you have just a number or two** to share, simple text can be a great way to communicate. Think about **solely using the number**, making it as prominent as possible, **and a few supporting words** to clearly make your point.

When you have more data that you want to show, generally a table or graph is the way to go. One thing to understand is that people interact differently with these two types of visuals. :point_down:

## Tables

Tables interact with our **verbal system**, which means that we **read** them. Tables are great for communicating to a **mixed audience** whose members will each look for their particular row of interest. If you need to communicate **multiple different units of measure**, this is also typically easier with a table than a graph.

> :no_entry: **Using a table in a live presentation is rarely a good idea.** As your audience reads it, you lose their ears and attention to make your point verbally. When you find yourself using a table in a presentation or report, ask yourself: what is the point you are trying to make? Odds are that there will be a better way to pull out and visualize the piece or pieces of interest. In the event that you feel you’re losing too much by doing this, consider whether including the full table in the appendix and a link or reference to it will meet your audience’s needs.

One thing to keep in mind with a table is that you want the **design** to **fade into the background**, letting the **data take center stage**.

**Borders** should be used to improve the legibility of your table.
> :bulb: gray, light borders, or simply white space

### Heatmap: *a table with color saturation cues*

One approach for **mixing the detail** you can include in a table while **also making use of visual cues** is via a heatmap. A heatmap is a way to **visualize data in tabular format**, where in place of (or in addition to) the numbers, you leverage **colored cells** that convey the *relative magnitude of the numbers*.

Heatmaps **reduce the mental processing** as with regular tables. We can use **color saturation** to provide visual cues, helping our eyes and brains more quickly target the potential points of interest. Be sure when you leverage this to **always include a legend** to help the reader interpret the data

> :bulb: Excel conditional formatting

## Graphs

While tables interact with our verbal system, graphs interact with our **visual system**, which is **faster at processing information**. This means that a well-designed graph will typically get the information across more quickly than a well-designed table.

> :bulb: **Charts vs. Graphs**: Typically, “chart” is the broader category, with “graphs” being one of the subtypes (other chart types include maps and diagrams). :open_mouth: I don’t tend to draw this distinction, since nearly all of the charts I deal with on a regular basis are graphs. Throughout this book, I use the words chart and graph *interchangeably*.

### Points - Scatterplot

Scatterplots can be useful for showing the **relationship between two things**, because they allow you to **encode data simultaneously on a horizontal x-axis and vertical y-axis** to see whether and what relationship exists.

### Lines - Regular Line Graph

Line graphs are most commonly used to plot **continuous data**. Because the **points are physically connected via the line**, it implies a connection between the points that **may not make sense for categorical data** (a set of data that is sorted or divided into different categories). **Often, our continuous data is in some unit of time**: days, months, quarters, or years.

Note that when you’re graphing **time on the horizontal x-axis** of a line graph, the data plotted must be **in consistent intervals**.

> :bulb: In some cases, the line in your line graph may **represent a summary statistic**, like the **average**, or the point estimate of a **forecast**. If you also want to **give a sense of the range (or confidence level**, depending on the situation), you can do that directly on the graph by **also visualizing this range**.

> :bulb: Bar charts must have a zero baseline; Line graphs don't have to. With line graphs, since the **focus is on the relative position in space** (rather than the length from the baseline or axis), you can get away with a **nonzero baseline**. Still, you should approach with caution: **make it clear to your audience** that you are using a nonzero baseline and **take context into account so you don’t overzoom and make minor changes or differences appear significant**.


### Lines - Slope Graph

Slopegraphs can be useful when you have **two time periods or points of comparison** and want to quickly **show relative increases and decreases or differences across various categories** between the two data points.

In addition to the absolute values (the points), the lines that connect them give you the **visual increase or decrease in rate of change (via the slope or direction)** without ever having to explain that’s what they are doing, or what exactly a “rate of change” is; rather, it’s **intuitive**.

Whether a slopegraph will work in your specific situation depends on the data itself. If many of the lines are overlapping, a slopegraph may not work, though in some cases you can still emphasize a single series at a time with success (e.g., with a special color).

> kind of hard to set up...?

While lines work well to show data over time, :point_down: bars are better for plotting categorical data, where information is organized into groups.


### Bars - General

- **Common** -> less of a learning curve for audience.
- **Easy** for eyes to erad: we compare the end points of bars, quickly compare different bars and the incremental difference between them.
- Always use a **zero baseline**: Because of the way our eyes compare the relative end points of the bars, it’s important to have the context of the entire bar there in order to make an accurate comparison.
- **y-axis lables** should be put to the **left**: so we see how to interpret data before we get to the actual data. *Whether to preserve the axis lables or eliminate the axis (instead, lable the data points directly) depends on the level of specificity needed: focus on big-picture trends or specific numeric values.*
- **Data lables** can be pulled **inside** the bars to reduce clutter.
- **Width of bars** should be wider than the white space between bars but not make the graph too crowded.

Different varieties of bar charts gives you flexibility when facing different data visualization challenges. :point_down:

### Bars - Vertical vs. Horizontal

Both can have **one or more series**. Visual grouping happens as a result of the spacing in multiple series bar charts, which makes the **relatie order** of the categorization important. If there is a natural ordering to your categories, use it if makes sense; If, however, there isn’t a natural ordering in your categories, think about what ordering of your data may make the most sense (e.g., descending or ascending). Being thoughtful here can mean providing a construct for your audience, easing the interpretation process.

**Horizontal** bar chars are easier to read, especially useful **when the category names are long**. Also, because of the way we typically process information, by the time we get to the data, we already know what it represents (instead of the darting back and forth our eyes do between the data and category names with vertical bar charts).

### Bars - Stacked

Stacked bar charts compare **totals** across categories and also show the **subcomponent** pieces within a given category. This can become **visually overwhelming**: it's hard to compare the subcomponents across the various categories once you get beyond the bottom series.

Stacked bar chars can be structured as **absolute numbers or with each column summing to 100%**. When you use the 100% stacked bar, think about whether it makes sense to also include the absolute numbers for each category total (either in an unobtrusive way in the graph directly, or possibly in a footnote), which may aid in the interpretation of the data.

Stacked horizontal bar charts can work well for visualizing portions of a whole on a scale from negative to positive, e.g., **survey data collected along a Likert scale**.

### Bars - Waterfall Chart

The waterfall chart can be used to **pull apart the pieces of a stacked bar chart to focus on one at a time**, or to **show a starting point, increases and decreases, and the resulting ending point**.

> :bulb: Some applications do not have waterfall chart option, you can achieve this by making the first series (the one that appears closest to the x-axis) invisible.

### Area

Humans’ eyes don’t do a great job of attributing quantitative value to two-dimensional space, which can render area graphs **harder to read**. With one **exception**: when you need to visualize numbers of vastly different magnitudes.

### Other Types of Graphs

There are many other types of graphs out there. When it comes to selecting a graph, first and foremost, choose a graph type that will enable you to clearly get your message across to your audience. With less familiar types of visuals, you will likely need to take extra care in making them accessible and understandable.

> :bulb: **Infographics**: This term is frequently misused. Visuals coined infographic run the gamut from fluffy to informative. There are many good examples in the area of data journalism (for example, the *New York Times* and *National Geographic*). Information designers should always first understand the context for storytelling with data before choosing an effective method of display that will best aid the message.

## Generally Avoid

### No Pie or Donut Charts

**The human eye isn’t good at ascribing quantitative value to two-dimensional space.**

**Replace** the pie chart with a horizontal bar chart, organized from greatest to least or vice versa (unless there is some natural ordering to the categories that makes sense to leverage). Because they are aligned at a common baseline, it is easy to assess relative size. This makes it straightforward to see not only which segment is the largest, for example, but also how incrementally larger it is than the other segments.

The unique thing you get with a pie chart is **the concept of there being a whole** and, thus, parts of a whole. But if the visual is difficult to read, is it worth it? If you find yourself using a pie chart, pause and ask yourself: why? If you’re able to answer this question, you’ve probably put enough thought into it to use the pie chart, but it certainly shouldn’t be the first type of graph that you reach for, given some of the difficultie in visual interpretation we’ve discussed here.

With pies, we are asking our audience to compare angles and areas. With a **donut chart**, we are asking our audience to compare one **arc length** to another arc length. Still, human eyes are bad at such comparison.

### No 3D

**3D skews our numbers**, making them difficul or impossible to interpret or compare. Adding 3D to graphs introduces unnecessary chart elements like side and floor panels. Applications do some pretty strange things when it comes to plotting values in 3D.

The **only exception** is if you are **actually plotting a third dimension** (and even then, things get really tricky really quickly, so take care when doing this. And you should never use 3D to plot a single dimension.

### Secondary y-axis? Generally No

Sometimes it's useful to **plot data that is in entirely different units against the same x-axis**. If you add a secondary or right-hand y-axis, it takes some time and reading to understand which data should be read against which axis.

Instead, think about whether one of the following approaches will meet your needs:
  1. Don’t show the second y-axis. Instead, **label the data points** that belong on this axis directly. This puts **more attention to specific numbers**.
  2. **Pull the graphs apart vertically and have a separate y-axis for each** (both along the left) but leverage the same x-axis across both. This puts** more focus on the overarching trends**.

*A third potential option not shown here is to link the axis to the data to be read against it through the use of color. I don’t recommend this approach because color can typically be used more strategically.*

When you display two datasets against the same axis, it can imply **a relationship that may or may not exist**. This is something to be aware of when determining whether this is an appropriate approach in the first place.

