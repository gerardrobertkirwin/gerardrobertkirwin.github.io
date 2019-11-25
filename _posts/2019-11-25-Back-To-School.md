---
layout: post
title: "Back to School"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
maps: true
image: language%20arts%20class%20with%20help.jpg
---

<p style="font-size: 0.9rem;font-style: italic;">
<a href="https://www.flickr.com/photos/judybaxter/45968272/">"Language Arts Class with Help"</a><span> by 
<a href="https://www.flickr.com/people/judybaxter/">Judy Baxter</a></span> is licensed under 
<a href="https://creativecommons.org/licenses/by-nc-sa/2.0/?ref=ccsearch&atype=html" style="margin-right: 5px;">CC BY-NC-SA 2.0</a></p>
<br>
After my last project, I was looking for something simpler to practice my data analysis skills. 
Don’t get me wrong, you can cut your teeth on building something like the map from my last post but sometimes you need to get product out quickly. 
So I went looking for some data to play around with where I could produce some simple (but not too simple) visualizations.
<br>

Once again, I turned to Tidy Tuesday. This time, in their archives, from September was [data about racial diversity in schools](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-09-24).
Until June of this year, I had worked in education the last 7 years, so this dataset would allow me to bring in my background knowledge. 
This perspective would become useful not only in the way I expected, in looking at and manipulating the data; but also in the way it was visualized.

*Data* 
----------
The School Diversity dataset was relatively clean already from the work done by the Washington Post, who originally collected the data, and the Tidy Tuesday team. Looking at the data was when I formulated what question I wanted my data to answer and what I would need to do to it to get there. 
<br>
The dataset featured data on all 50 states and Washington DC. It featured data from the 1994-1995 and 2016-2017 school years as comparison. Also, the data included information about whether the district was integrated, meaning how likely is a school to have a similar racial makeup as the district as a whole.
<br>
While looking at the raw data from top to bottom ordered by state, I quickly got to my home state, Arizona. Arizona was one of the fastest growing states in the country during the 90s and the 00s. This fact, and my knowledge of the area, led me to want to investigate only the Arizona data. And I quickly settled on what I wanted to investigate. <b>How has Arizona's growth had an impact on the racial makeup of its schools?</b>
<br>
To make the data ready for this investigation, I filtered the state category for Arizona. Many school districts in Arizona are "elementary" only or "high school" only. In order to get an even comparison for cohorts, I filtered these districts out, leaving only "unified" school districts that serve K-12.
<br>
Finally, my last step was to turn the data from a wide format with each row belonging to a school district in a given year to a long format with each row belonging to a racial group at a school district in a given year.
*Visualization*
----------



*Conclusions and Reflection*
-------------
