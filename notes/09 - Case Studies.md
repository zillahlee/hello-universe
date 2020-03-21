---
tags: [Notebooks/SWD Viz]
title: 09 - Case Studies
created: '2020-03-17T04:36:13.257Z'
modified: '2020-03-20T14:39:28.629Z'
---

# 09 - Case Studies

> The penultimate chapter explores specific strategies for tackling common challenges faced in communicating with data through a number of case studies. Topics covered include color considerations with a dark background, leveraging animation in the visuals you present versus those you circulate, establishing logic in order, strategies for avoiding the spaghetti graph, and alternatives to pie charts.

> When you find yourself in a situation where you are unsure how to proceed, I nearly always recommend the same strategy: **pause to consider your audience**. What do you need them to know or do? What story do you aim to tell them? Often, by answering these questions, a good path for how to present your data will become clear. If one doesn’t, try several views and seek feedback.

## *Case Study 1:* Colors with a Dark Background

When it comes to communicating data, I don’t typically recommend anything other than a white background. Light elements on a dark background can create a stronger contrast but are generally harder to read.

But reality is sometimes outside of the ideal scenario. For example, we might better comply with the company or client’s brand and corresponding standard template.

With a **white background**, the further a color is from white, the more it will stand out (so grey stands out less, whereas black stands out very much). With a **black background**, the same is true, but black becomes the baseline (so grey stands out less, and white stands out very much).

## *Case Study 2:* Animations in Presentation

One conundrum commonly faced when communicating with data is when a single view of the data is used for both presentation and report. When presenting content in a live setting, you want to be able to walk your audience through the story, focusing on just the relevant part of the visual. However, the version that gets circulated to your audience (as a pre-read or takeaway, or for those who weren’t able to attend the meeting) needs to be able to stand on its own without you, the presenter, there to walk the audience through it.

You can leverage animation to walk your audience through your visual as you tell the corresponding points of the story. For example, I could start with a blank graph. This forces the audience to look at the graph details with you, rather than jump straight to the data and start trying to interpret it. You can use this approach to build anticipation within your audience that will help you to retain their attention. From there, I subsequently show or highlight only the data that is relevant to the specific point I am making, forcing the audience’s attention to be exactly where I want it as I am speaking.

For the more detailed version that you circulate as a follow up or for those who missed your (stellar) presentation, you can leverage a version that annotates the salient points of the story on the line graph directly.

If you’re leveraging presentation software, you can set up all of the above on a single slide and use animation for the live presentation, having each image appear and disappear as needed to form the desired progression. Put the final annotated version on top so it’s all that shows on the printed version of the slide. If you do this, you can use the exact same deck for the presentation and the communication that you circulate. Alternatively, you can put each graph on a separate slide and flip through them; in this case, you’d only want to circulate the final annotated version.

## *Case Study 3:* Logic in Orders

> There should be logic in the order in which you display information.

The above statement probably goes without saying. Yet, like so many things that seem logical when we read them or hear them or say them out loud, too often we don’t put them into practice. :smile:

If we want to tell **one of the stories**, we can leverage order, color, position, and words to draw our audience’s atten tion to where we want them to pay it in the data.

If we want to tell **all stories**, however, it isn’t very nice to get your audience familiar with the data only to **completely rearrange** it. Doing so creates a **mental tax** (unnecessary cognitive burden). **Preserve the same order** so our audience only has to familiarize themselves with the detail once: highlighting the different stories one at a time through strategic use of color.

The sparing and strategic use of color lets me direct my audience’s attention to one component of the data at a time. a written document to be shared directly with your audience, you might compress all of these views into a single, comprehensive visual. **Strategic use of color sets the various series apart from one another** while also making it clear where the audience should look for the specific evidence of what is being described in the text.

Again, in some cases there is intrinsic order in the data you want to show (ordinal categories, like age ranges). **Keep the intrinsic order**, then use the other methods of drawing attention (through color, position, callout boxes with text) to direct the audience’s attention to where you want them to pay it.

## *Case Study 4:* Avoiding Spaghetti Graph

If you find yourself facing a spaghetti graph, don’t stop there. Think about what information you want to most convey, **what story you want to tell**, and what changes to the visual could help you accomplish that effectively. Note that in some cases, this may mean **showing less data** altogether. Ask yourself: Do I need all categories? All years? When appropriate, reducing the amount of data shown can make the challenge of graphing data like that shown in this example easier as well.

### Emphasize One Line at a Time

One way to keep the spaghetti graph from becoming visually overwhelming is to use preattentive attributes (color, line thickness, added marks: data label and data marker) to draw attention to a single line at a time.

This can work well in a live presentation, where you explain the details of the graph once (as we’ve seen in the recent case studies), then cycle through the various data series in this manner, highlighting what is interesting or should be paid attention to with each and why. Note that we need either this voiceover or the addition of text to make it clear why we are highlighting the given data and provide the story for our audience.

### Separate Spatially

We can untangle the spaghetti graph by pulling the lines apart either vertically or horizontally.

One of the axis should stay the same across all of the graphs. Create separate graphs for each line but organized them such that they appear to be a single visual. The other axis within each graph isn’t shown; rather, the starting and ending point labels are meant to provide enough context so that the axis is unnecessary. Though they aren’t shown, it is important that the axis minimum and maximum are the same for each graph so the audience can compare the relative position of each line or point within the given space.

Decision on which axis to keep depends on your chosen focus.

### Combined Approach

Another option is to combine the above two approaches. We can separate spatially and emphasize a single line at a time, while leaving the others there for comparison but pushing them to the background. As was the case with the prior approach, we can do this by separating the lines vertically or horizontally.

Having a number of small graphs together is sometimes referred to as **small multiples**. As noted previously, it’s imperative here that the details of each graph (the x- and y-axis minimum and maximum) are the same so that the audience can quickly compare the highlighted series across the various graphs.

The combined approach can work well **if the context of the full dataset is important** but you want to be able to focus on a single line at a time. Because of the denseness of information, this combined approach may work **better for a report** or presentation that will be circulated rather than a live presentation, where it will be more challenging to direct your audience where you want them to look.

## *Case Study 5:* Pie Alternatives

### *Alternative #1:* Show the Numbers Directly

Too often, we think we have to include all of the data and overlook the simplicity and power of communicating with just one or two numbers directly.

### *Alternative #2:* Simple Bar Graph

:bulb: When you want to **compare** two things, you should generally put those two things **as close together as possible** and **align them along a common baseline** to make this comparison easy.

### *Alternative #3:* 100% Stacked Horizontal Bars

When the **part-to-whole concept** is important (something you don’t get with either Alternative #1 or #2), the stacked 100% horizontal bar graph achieves this.

A consistent baseline allows the audience to easily compare both the negative segments at the left and the positive segments at the right across the two bars. Because of this, 100% stacked horizontal bars are a useful way to visualize **survey data in general**.

:bulb: With 100% stacked horizontal bars, **retain the x-axis labels** rather than put data labels on the bars directly, so that you can use the scale at the top to read either from left to right or from right to left.

### *Alternative #4:* Slope Graph

With slopegraphs, you can easily see the **visual percentage change** from Before to After for each category via the slope of the respective line. The slopegraph also provides **clear visual ordering of categories from greatest to least** (via their respective points in space from top to bottom on the left and right sides of the graph).

As was the case with the simple bar chart, you don’t get a clear sense of there being a **whole** and thus pieces-of-a-whole in this view (in the way that you do with the initial pie or with the 100% horizontal stacked bar).

Also, if it is important to have your categories ordered in a certain way, a slopegraph won’t always be ideal since the various categories are placed according to the respective data values. If you need to dictate the **category order**, use the simple bar graph or the 100% stacked bar graph, where you can control this.

