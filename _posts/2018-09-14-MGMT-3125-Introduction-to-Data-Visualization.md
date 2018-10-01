---
title: MGMT 3125 - Introduction to Data Visualization
date:   2018-09-14
tags:
  - stockton university
  - tableau
author_profile: true
excerpt: I currently teach this course. Repository for notes, lecture videos, and assignments
toc: true
mathjax: true
published: true
---

## Course Information

### Description:

Introduction to Data Visualization provides an overview of business analytics, including the process of business analytics / business intelligence analysis, big data management, and principles of data visualization and dashboard design. The course uses spreadsheet software (Excel, Google Sheets) and Tableau.

This is the first course with a comprehensive overview of the fundamental concepts and tools of business analytics for improving business decision making and performance. This is a hands-on course that is designed to introduce the principles and techniques for data visualization. Visualizations are graphical depictions of data that can improve comprehension, communication, and decision making. Students will learn visual representation methods and techniques that increase the understanding of complex data and models. Emphasis is placed on the identification of patterns, trends and differences from data sets across categories, space, and time. 

### Prerequisites

* CSIS 1206 (Statistics) with a letter grade of C or better.

* General computer skills and familiarity. An understanding of basic charting and statistical terms/practices will be helpful, but not required.

### Textbook

* Storytelling with Data: A Data Visualization Guide for Business Professionals, by Cole Nussbaumer Knaflic, 1st edition, WILEY. ISBN-13: 978- 1119002253.

### Syllabus

* [Download here](/assets/mgmt_3125/MGMT3125_Fall2018.pdf)


## Week 1 - 9/10

### Chapter 1: The Importance of Context

To consider context is strongly encouraged before building a visualization.

#### Exploratory vs. Explanatory analysis
*	Ex _plor_ atory: understand the data to determine what is noteworthy to show the audience
* Ex _plan_ atory: the specific story about the data to tell the audience

Too often the visualization author shows the exploratory analysis instead of the important explanatory analysis the audience will care about.

#### The Who, What and How of Context

Who: Very important to understand the intended audience and how they perceive you. This will shape how you approach creating the visualization. 

* The Who aspect helps to identify common ground that will facilitate relaying your findings to that audience
* The more specific identification of the audience, the better. Just saying clients/key stakeholders usually isn't specific enough 

What: What do you need your audience to know? 

* Should be clear how you want your audience to react
* Take into account overall tone of visualization and/or presentation
* If you are the one analyzing and communicating the data, you are considered the 'subject matter expert'
* Be confident with your findings! Don't let fear of public speaking decrease your credibility

How: Mechanism of conveying the information
* Live presentations should have a paucity of detail considering you're available to answer questions
* Write ups/static documents demand generous detail to cover all the bases for possible client/stakeholder questions

#### The Big Idea

A single sentence summary of the study background information, issue the study is trying to solve, and the outcome of the study. Three components of The Big Idea:

* The idea must articulate your unique point of view
* The idea must convey what is at stake
* The idea must be a complete sentence

#### Storyboarding

Very powerful way of organizing ideas by creating a visual outline for the content of the visualization.

* Our minds do not think linearly; our minds scatter our thoughts. Time and effort must be put forth to organize them properly.
* A few pieces of paper and pen just fine for this task. Alternatively, if you have a Mac or iOS software, I recommend [MindNode](https://mindnode.com)

## Week 2 - 9/17

### Chapter 2: Choosing an Effective Visualization

Choosing the right visualization is typically selected from 8 traditional methods. Knaflic elaborates on the scenarios that fit each visualization best.

#### The 8 common visualizations

1. Simple text
* Works great for just a number or two.
* Refrain from taking up excessive space with a bar graph when simple text will suffice to compare two values.
* Be creative - make the font of the number you're trying to convey extremely large; also use color to your advantage by highlighting poignant parts of the study.

2. Table
* Useful to look at the raw data in the build phase, especially if the client/stakeholder requests it.
* Use light or minimal table borders so the observer can concentrate on the data
* Do not use tables for presentations; a well designed graph will convey insights much faster than a table with solely numerical values

3. Heatmap (also known as a treemap)
* Method of visualizing table data
* Use color saturation to create visual cues, shortening the amount of time it takes to get from initial observation to data insight

4. Scatterplot
* Effective for showing a relationship between two fields
* Include baseline statistics such as the median or average values so the observer can compare the plotted values with these summary statistics

5. Line graph
* Optimal for visualizing time series data and also more than one series of data (more than one line).
* I like visualizing summary statistics within a line graph, such as the average representing the average of the time series data, then the max and min values as a range of values that reach above and below the average.
* Line graphs do not need a zero baseline, because the graph is comparing data relative to each data point. Nonetheless, a good practice is to let the audience know you're baseline is non-zero.

6. Slopegraph
* Great for visualizing two time periods or points of comparison. Shows increases or decreases between two data points.
* Use a slope graph if you want to show increasing or decreasing values without showing what occurred in the middle of a particular timeframe.
*  Employ color to emphasize a trend that will be the focal point of your storytelling discussion.

7. Vertical bar graph
* Go-to visualization for plotting categorical data (the information is organized into groups).
* Viewers are so familiar with bar charts that this is the quickest chart to convey valuable data insights to people that matter. 
* Always needs to have a zero baseline, or else there will be misconceptions about the graph.
* Common decision is to preserve the axis labels or eliminate them entirely to label the data points. If you're focusing on big picture trends, then leave the axis labels. If you're focused on the specific numerical values, then label the data points directly. 

8. Horizontal bar graph


#### Graphs to be Avoided

Area Graphs
* Avoid area graphs, as human eyes are optimized for attributing quantitative value to two-dimensional space.
* Same principle applies with pie charts; replace pie charts with horizontal bar charts. 

Secondary y-axis: generally not a good idea
* Avoid the use of a secondary or right hand y-axis
* Don't show the second y-axis. Instead, label the data points that belong on this axis directory
* Pull the graphics apart vertically and have a separate y-axis for each (both along the left) but leverage the same x axis across both

There isn't a single correct visual display. Rather, there are different types of visuals that could meet a given need. The most important aspect of visual build/design is _What do you need your audience to know?_ Choose a visual display to make this abundantly clear.

### Assignment 1 - Excel Graphs

* [Download Directions](/assets/mgmt_3125/Assignment 1 - Excel Graphs_.pdf)
* [Download IT tickets dataset](/assets/mgmt_3125/IT Tickets by Month.xlsx



## Week 3 - 9/24

### Chapter 3: Clutter is Your Enemy!

#### Cognitive Load & Clutter

* Cognitive load: Every single element added to a page takes up cognitive load on the part of the audience.
* Identify anything that doesn't add informative value. Remove these elements.
* Maximize the data ink ratio or signal to noise ratio to relieve cognitive load.

* Clutter: Visual elements that take up space but don't increase understanding.
* Why reduce clutter? Clutter makes visualizations more complicated than necessary

Employ the Gestalt Principles of Visual Perception to identify communicative elements and noisy elements. 

#### Gestalt Principles of Visual Perception

* Proximity: Tendency to think of physically close objects as belonging to a group.
* Similarity: Objects that are of similar color, shape, size, or orientation are perceived as related or belonging to a group.
* Enclosure: Objects that are physically enclosed together as belonging to a group.
* Closure: When parts of a whole are missing, our eyes fill in the gap; This principle renders chart borders and background shading unnecessary.
* Continuity: When looking at objects, our eyes seek the smoothest path and naturally create continuity in what we see even where it may not explicitly exist. 
* Connection: Connective property typically has a strong associate value than similar color, size, or shape.

Without obvious visual cues, the audience will typically start at the top left of a visualization, then move their eyes in a "Z" shape across the page or screen as they take in information

* Because of this, upper left justify text (title, axis, legends, etc).
* Generally, diagonal elements such as lines connecting text to visual attributes should be avoided.
* White space is akin to pauses in sentences; use white space strategically to draw attention to the parts of the page that are not white space. 
* Contrast: The more things we make different, the lesser the degree to which any of them stand out.

#### Decluttering Strategies
* Remove chart border
* Remove gridlines
* Remove data markers
* Clean up axis labels
* Label data directly
* Leverage consistent color

Anytime you put information in front of an audience, you are creating cognitive load and asking them to use their brain power to process that information. Visual clutter creates excessive cognitive load that can hinder the transmission of your message. The Gestalt principles helps identify and remove unnecessary visual elements. Leverage alignment of elements and maintain white space to help make the interpretation of visuals a comfortable experience for the audience.

### Assignment 2 - Introduction to Tableau

<figure>
  <img src="/assets/mgmt_3125/Life Expectancy Dashboard.jpg" width = "1015" height = "725">
</figure>


## Week 4 - 10/01

### Chapter 4: Focus your Audience's Attention

### Assignment 3 - Tableau Line Graphs



