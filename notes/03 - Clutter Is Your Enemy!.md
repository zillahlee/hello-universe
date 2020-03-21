---
tags: [Notebooks/SWD Viz]
title: 03 - Clutter Is Your Enemy!
created: '2020-03-17T04:35:17.957Z'
modified: '2020-03-19T17:05:26.740Z'
---

# 03 - Clutter Is Your Enemy!

> Picture a blank page or a blank screen: every single element you add to that page or screen takes up cognitive load on the part of your audience—in other words, takes them brain power to process. Therefore, we want to take a discerning look at the visual elements that we allow into our communications. In general, identify anything that isn’t adding informative value—or isn’t adding enough informative value to make up for its presence—and remove those things.

> Any time you put information in front of your audience, you are creating cognitive load and asking them to use their brain power to process that information. Visual clutter creates excessive cognitive load that can hinder the transmission of our message. The Gestalt Principles of Visual Perception can help you understand how your audience sees and allow you to identify and remove unnecessary visual elements. Leverage alignment of elements and maintain white space to help make the interpretation of your visuals a more comfortable experience for your audience. Use contrast strategically. Clutter is your enemy: ban it from your visuals!

## Cognitive Load

We experience cognitive load anytime we take in information. Cognitive load can be thought of as the **mental effort** that’s required to learn new information. Humans’ brains have a finite amount of this mental processing power. As designers of information, we want to be smart about how we use our audience’s brain power. **Extraneous cognitive load** is the processing that takes up mental resources but doesn’t help the audience understand the information.

The goal is to maximize the **data-ink ratio**, maximize the **signal-to-noise ratio**. Signal is the information we want to communicate; noise are those elements that either don’t add to, or in some cases detract from, the message we are trying to impart to our audience.

## Gestalt Priciples of Visual Perception

The Gestalt School of Psychology set out in the early 1900s to understand **how individuals perceive order in the world around them**. What they came away with are the principles of visual perception still accepted today that define **how people interact with and create order out of visual stimuli**.

1. **Proximity**
    - We tend to think of objects that are physically close together as belonging to part of a group.
    - Table design: by ***differentiating the spacing between cells***, we can draw audience's eyes either down the columns or across the rows.
2. **Similarity**
    - Objects that are of similar color, shape, size, or orientation are perceived as related or belonging to part of a group.
    - Use ***consistent colors*** to draw audience's eyes in the direction we want them to focus, eliminating the need for additional elements such as borders to help direct attention.
3. **Enclosure**
    - We think of objects that are physically enclosed together as belonging to part of a group. It doesn’t take a very strong enclosure to do this: light background shading is often enough.
    - We can use enclosure with background shading to draw a ***visual distinction within our data***.
4. **Closure**
    - The closure concept says that people like things to be simple and to fit in the constructs that are already in our heads. Because of this, people tend to perceive a set of individual elements as a single, recognizable shape when they can—when parts of a whole are missing, our eyes fill in the gap.
    - The closure principle tells us that the defaul settings of elements such as chart borders and background shading are ***unnecessary***. We can ***remove*** them and our graph still appears as a cohesive entity. Bonus: when we take away those unnecessary elements, our data stands out more.
5. **Continuity**
    - The principle of continuity is similar to closure: when looking at objects, our eyes seek the smoothest path and naturally create continuity in what we see even where it may not explicitly exist.
    - E.g., removing the y-axis line and you can still see that the bars are lined up at the same point because of the consistent white space (the smoothest path). Stripping away unnecessary elements allows our data to stand out more.
6. **Connection**
    - We tend to think of objects that are physically connected as part of a group. The connective property typically has a stronger associative value than similar color, size, or shape. The connective property isn’t typically stronger than enclosure, but you can impact this relationship through thickness and darkness of lines to create the desired visual hierarchy
    - Use line graphs to help our eyes see order in the data (connecting the points).

The Gestalt principles help us understand how people see, which we can use to **identify unnecessary elements** and ease the processing of our visual communications.

## Visual Clutter

One culprit that can contribute to excessive or extraneous cognitive load is something I refer to simply as clutter. These are **visual elements that take up space but don’t increase understanding**. Clutters makes our visuals appear **more complicated than necessary**.

When our visuals feel complicated, we run the risk of our audience deciding they don’t want to take the time to understand what we’re showing, at which point we’ve lost our ability to communicate with them.

### Lack of Visual Order

When design is thoughtful, it fades into the background so that your audience doesn’t even notice it. When it’s not, however, your audience feels the burden.

### Alignment

Avoid center-aligned text. Create clean lines (both horizontally and vertically) of elements and white space. Turn on the rulers or gridlines, or even use table as a makeshift brute-force mothod for alignment.

*Without other visual cues, your audience will typically start at the top left of the page or screen and will move their eyes in a “z” shape (or multiple “z” shapes, depending on the layout) across the page or screen as they take in information. Because of this, when it comes to tables and graphs, I like to **upper-left-most justify the text** (title, axis titles, legend). This means the audience will hit the details that tell them how to read the table or graph before they get to the data itself.*

Generally, diagonal elements such as lines and text should be avoided. They look messy and, in the case of text, are harder to read than their horizontal counterparts.

### White Space

White space in visual communication is as important as **pauses in public speaking**. The lack of it, like the lack of pauses in a spoken presentation, is simply uncomfortable for our audience. White space should be used strategically to draw attention to the parts of the page that are not white space.

Minimal guidelines: **Margins** should remain free of text and visuals. Appropriately **size** your visuals to their content.

### Non-Strategic Use of Contrast

Clear contrast can be a signal to our audience, helping them understand where to focus their attention. The lack of clear contrast, on the other hand, can be a form of visual clutter. It’s easy to spot a hawk in a sky full of pigeons, but as the variety of birds increases, that hawk becomes harder and harder to pick out. **The more things we make different, the lesser the degree to which any of them stand out.** To explain this another way, if there is something really important we want our audience to know or see (the hawk), we should make that the one thing that is very different from the rest.

> :exclamation: **Some Elements Shouldn't be Considered Clutter**: There are some elements that should always be retained with numbers, including dollar signs, percent signs, and commas in large numbers.

## Decluttering

1. **Remove Chart Border**
    - Chart borders are usually unnecessary. Think about using white space to differentiate the visual from other elements on the page as needed.
2. **Remove Gridlines**
    - If you think it will be helpful for your audience to trace their finge from the data to the axis, or you feel that your data will be more effectively processed, you can leave the gridlines. But make them thin and use a light color like grey. Do not let them compete visually with your data. When you can, get rid of them altogether: this allows for greater contrast, and your data will stand out more.
3. **Remove Data Markers**
    - Remember, every single element adds cognitive load on the part of your audience. Here, we’re adding cognitive load to process data that is already depicted visually with the lines. This isn’t to say that you should never use data markers, but rather use them on purpose and with a purpose, rather than because their inclusion is your graphing application’s default.
4. **Clean Up Axis Lables**
    - Trailing zeros on y-axis labels carry no informative value. Abbreviate the months of the year so that they will fit horizontally on the x-axis, eliminating the diagonal text.
5. **Label Data Directly**
    - Avoid going back and forth between the legend and the data. Try to identify anything that will feel like effort to our audience and take that work upon ourselves as the designers of the information. In this case, we can leverage the Gestalt principle of proximity and put the data labels right next to the data they describe.
6. **Leverage Consistent Color**
    - Leverage the Gestalt principle of similarity and make the data labels the same color as the data they describe. This is another visual cue to our audience that says, “these two pieces of information are related.”

