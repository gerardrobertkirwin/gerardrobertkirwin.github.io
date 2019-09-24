---
layout: post
title: "Ladies, Get In Formation!"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: stacy-plaskett-and-cheri-bustos.jpg
---

<p style="font-size: 0.9rem;font-style: italic;">
<a href="https://www.flickr.com/photos/158099690@N02/42922519984">"Congresswomen
Stacey Plaskett and Cheri Bustos"</a><span> by
<a href="https://www.flickr.com/photos/158099690@N02">HouseAgDems</a></span>
is licensed under
<a href="https://creativecommons.org/licenses/by-nc-nd/2.0/?ref=ccsearch&atype=html" style="margin-right: 5px;">CC
BY-NC-ND
2.0</a></p>

Teaching myself R has been surprisingly easy for me. I say this not to brag or boast. I've been working on [Data Camp's Data Analyst Track](https://www.datacamp.com/profile/gerardrobertkirwin) for a few months now and I've worked my way through their guided exercises. So the next step in my data science learning journey is to work on some projects on my own. As much as I have learned from the exercises, nothing is better than doing it yourself. The blogs on this site will follow these projects.

Data
----------
I stumbled upon the Women In Parliament data set on [Twitter](https://twitter.com/ilustat/status/1154401034183352321) and was interested in doing a project that, as the tweet says, isn't the "car mpg" or "iris" projects that are the textbook projects.

Included with the tweet was some instructions to follow through and clean the data. I went through the directions, which cleaned the data and produced graphs. I decided to redo the project, using the United States instead of Portugal.

I have also added two more items to clean up the dataset. First, since I am focusing on the United States, it would be useful to compare it to other North American countries. Unfortunately, the data set puts North and South American countries together as "The Americas".
I put together the following code to create a "North America" and "South America" region for better analysis.

    summary(cars)

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

The second issue I had with the dataset was the odd values in 1990. Every plot from the exercise made 1990 seem like an outlier, it skewed each graph in a way that didn't seem correct. Looking closely as well, you can see that the 1990 data is from lfkjdsflsjfls while the 1997 and later data is from the World Bank. The inconsistent data should be removed.

Including Plots
---------------

You can also embed plots, for example:

![](Untitled_files/figure-markdown_strict/pressure-1.png)

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.
