---
layout: post
title: "English Football with Shiny Pt. 1"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
maps: true
image: prem_shiny_1.jpg
---

<p style="font-size: 0.9rem;font-style: italic;">
<a href="https://www.flickr.com/photos/28671532@N04/5139485295">"Calm Before the Storm"</a><span> by <a href="https://www.flickr.com/photos/28671532@N04">jwillier2</a></span> is licensed under <a href="https://creativecommons.org/licenses/by-nd/2.0/?ref=ccsearch&atype=html" style="margin-right: 5px;">CC BY-ND 2.0</a>
<a href="https://creativecommons.org/licenses/by-nd/2.0/?ref=ccsearch&atype=html" target="_blank" rel="noopener noreferrer" style="display: inline-block;white-space: none;margin-top: 2px;margin-left: 3px;height: 22px !important;">
</a></p>

I have been wanting to work with sports data for almost as long as I have been working with data and learning R. I've enjoyed sports since I was young and one of the big reasons was the statistics, the numbers behind the plays and games. 
<br>

Probably my favorite type of sports statistic, in team sports, is standings (called a table in some countries). For non-sports fans out there, standings are a ranking of teams usually based on how many games they won. In some sports, such as soccer or hockey, standings are based on "points" where a win is worth 2 or 3 points and a tie is worth 1 point. I always enjoy standings because of the value they have. Often standings are used to pick teams that compete for championships and are thus very exciting to watch.
<br>

There are numerous places to find team standings online, even ones from previous years. I wanted to use some of my R knowledge to play around with this data. In today's blog, I clean up a dataset and produce a simple Shiny app where you can get standings from English Football.

*Data* 
----------

You may be asking yourself why I would do this as I had just mentioned that these statistics are relatively easy to find online. I think that question is like asking someone why they would bake a cake if you could just buy one at Safeway? I wanted to practice some of my data cleaning skills with some big data. Data cleaning can be a hair-pulling exercise but when it's going well, I enjoy the problem solving aspect of it. 
<br>

Ironically, despite mentioning that standings are available numerous places online, finding data files with standings is a little more difficult. Most data science exercises use more individual statistics and even larger databases of individual games. This exercise will be looking at year-to-year data so even trying to manipulate and calculate standings based on that data would be time consuming and a diversion from the goal.
<br>

I found the dataset I used when I was trying out Tableau. I could not find where I had originally found the data, even searching my browser history and a deep Google search, I couldn't find a database that matched mine. This mystery is unfortunate but I think it's a benefit as this is a practice exercise for something that is often encountered in real life, having to clean up data that you don't know where it came from and how it was put together.
<br>

Looking at the data there were a number of things I needed to do to clean the data. First, I wanted standings with combined home wins, losses, ties and goals and away wins, losses, ties and goals. The data had seperate ones except for the 1992-93 through 2008-09 seasons where the data was combined but split into random columns. I calculated and combined the data, which involved changing the NULL data from 1992-93 through 2008-09 to 0 to enable calculations, then added the home and away columns. Finally, I removed the seperate home and away columns. 
<br>
Next, I wanted to calculate each team's "total position"
