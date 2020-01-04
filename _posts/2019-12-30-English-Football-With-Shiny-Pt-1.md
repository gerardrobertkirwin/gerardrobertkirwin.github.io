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

You may be asking yourself why I would do this as I had just mentioned that these statistics are relatively easy to find online. I think that question is like asking someone why they would bake a cake if you could just buy one at Safeway? I wanted to practice some of my data cleaning skills with some big data. Data cleaning can be a hair-pulling exercise but when it's going well, I enjoy the problem-solving aspect of it. 
<br>

I found the dataset I used when I was trying out Tableau. I could not find where I had originally found the data, even searching my browser history and a deep Google search, I couldn't find a database that matched mine. Ironically, despite mentioning that standings are available numerous places online, finding data files with standings is a little more difficult. Most data science exercises use more individual statistics and even larger databases of individual games. This mystery is unfortunate but I think it's a benefit as this is a practice exercise for something that is often encountered in real life, having to clean up data that you don't know where it came from and how it was put together.
<br>

The dataset is a listing of the number of games won, lost and tied (listed as D for draw), the total goals scored (listed as F for fielded) and allowed for each team. These were divided between home games and away games. Also listed were the number of games played, which tier (or division) the team played in, the position in each tier, the difference between the goals scored and allowed, plus the number of points earned from the wins and ties. The data also included which season it was from and the dataset included data from the 1946-47 season (the first one played after English football was suspended for World War II) through the 2017-18 season.


*Data Cleanup*
--------------

Looking at the data there were several things I needed to do to clean the data. First, I wanted standings with combined wins, losses, ties and goals. The data had separate home and away totals except for the 1992-93 through 2008-09 seasons where the data was combined but split into random columns. I calculated and combined the data, which involved changing the NULL data from 1992-93 through 2008-09 to 0 to enable calculations, then added the home and away columns. Finally, I removed the separate home and away columns. 
<br>


In the next step, I cleaned up some of the team names. Often, in standings teams that are relegated or promoted are listed with a R or a P, respectively, next to their name. I removed these with a simple string replace. The original data had a column which signified some of these places, but it is incomplete. In a future project, I would like to make that column complete for all relegated or promoted teams.
<br>

In addition to the Rs and Ps attached to some team names, some team names in some years are written with abbreviations or shortened names. For example, the team Brighton & Hove Albion is written five different ways including the way shown, which is their official name. Again, I used a string replace to fix all these names which not only unifies all naming conventions for each team but allows aggregate data to be calculated by name correctly. As I am unsure of the origin of this data, I cannot fully explain why some seasons have different abbreviations for Brighton & Hove Albion and the others.
<br>

Finally, I changed the names of the tiers from numbers to their correct names. While this may seem straightforward, the names of some divisions had changed three times since 1992 so I used a case_when wrapped in a str_replace_all to give each division in each year the correct name.
<br>

*Shiny app*
----------

I have been wanting to learn Shiny for a while and thought that I could use this newly clean data to try it out.
While I'm still working on the product you will see in part 2, I wanted to show everyone what I've learned so far. I want to give a big thanks to Coding Club because [their Shiny tutorial](https://ourcodingclub.github.io/2017/03/07/shiny.html) was the best one I saw online and really helped me through this project.
<br>

I decided that I wanted to make a simple standings table with the ability to select year and division. In my ui section, I made a pull-down menu with each season. Since different seasons had different division names, I used a renderUI that would gather the right division names for the right season. I also made it so that multiple divisions could be selected for each year. Finally, I put a footnote at the bottom of the main panel. 
<br>

In the server section, I used my clean data to produce the table based on the input of the season and division. I picked the columns most likely seen in standings for English football (side note: they actually call them tables!) and added the name of the division for clarity when multiple are chosen.
<br>

Here is the finished product (Click [here](https://gerardrobertkirwin.shinyapps.io/EnglishFootballTable/) to open in a new window):
<iframe src="https://gerardrobertkirwin.shinyapps.io/EnglishFootballTable/" style="border:none;width:1150px;height:500px;display:block"></iframe>

You can check out my GitHub repository [here](https://github.com/gerardrobertkirwin/Shiny-English-Football-Table) for the full code.

*To Be Continued*
---------------

As I hinted earlier, this is the first part of a bigger project I have planned with this data. The project got too unwieldly with trying to put a visual and a table on one page. Also, I wanted to focus on the basics of Shiny first which I feel like I accomplished with this project. Shiny is obviously unlike other R packages due to the way it was built to produce websites. That made the process a little harder for me but again being able to look for help and understand basic coding syntax was what got me through this project.
<br>
