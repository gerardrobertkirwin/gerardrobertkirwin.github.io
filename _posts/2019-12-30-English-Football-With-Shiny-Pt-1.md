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

I found the dataset I used when I was trying out Tableau. I could not find where I had originally found the data, even searching my browser history and a deep Google search, I couldn't find a database that matched mine. Ironically, despite mentioning that standings are available numerous places online, finding data files with standings is a little more difficult. Most data science exercises use more individual statistics and even larger databases of individual games. This mystery is unfortunate but I think it's a benefit as this is a practice exercise for something that is often encountered in real life, having to clean up data that you don't know where it came from and how it was put together.
<br>

The dataset is a listing of the number of games won, lost and tied (listed as D for draw), the total goals scored (listed as F for fielded) and allowed for each team. These were divided between home games and away games. Also listed were the number of games played, which tier (or division) the team played in, the position in each tier, the difference between the goals scored and allowed, plus the number of points earned from the wins and ties. The data also included which season it was from and the dataset included data from the 1946-47 season (the first one played after English football was suspended for World War II) through the 2017-18 season.


*Data Cleanup*
--------------

Looking at the data there were a number of things I needed to do to clean the data. First, I wanted standings with combined wins, losses, ties and goals. The data had seperate home and away totals except for the 1992-93 through 2008-09 seasons where the data was combined but split into random columns. I calculated and combined the data, which involved changing the NULL data from 1992-93 through 2008-09 to 0 to enable calculations, then added the home and away columns. Finally, I removed the seperate home and away columns. 
<br>

Next, I wanted to calculate each team's "total position" for each year (this will be used in my next blog). English football operates on a system of [relegation and promotion](https://en.wikipedia.org/wiki/Promotion_and_relegation) between their multiple divisions which are tiered. Usually the bottom 2-4 teams in each division are relegated to a lower division and the bottom 2-4 teams in each division are promoted to the next division. I wanted to calculate where every team is on the system of tiers each year. The top team in the top tier is 1 and the bottom team in the bottom tier is the highest number (for example 92 in 2017-18).
<br>

To figure out each team's "total position", I needed to have a counting sequence through the tiers and have it start over every year. In my dataset, the tiers were listed as text such as "First Tier", which is unreadable for a sequencing function. So I changed each tier to a number, 1 through 4, which the function could sequence through. Then I used R's within function to sequence through the tiers and then through the years. This worked a charm showing me 1 for the top team in the top division in each year and a number in the 80s or 90s (depending on the size of English League football in a particular year) for the worst team in the lowest division.
<br>

The code used is shown below:

    table_season_tiers <- table_clean %>% 
      mutate(Tier=str_replace_all(Tier, "First tier", '1')) %>% 
      mutate(Tier=str_replace_all(Tier, "Second tier", '2')) %>% 
      mutate(Tier=str_replace_all(Tier, "Third tier", '3')) %>% 
      mutate(Tier=str_replace_all(Tier, "Fourth tier", '4')) %>% 
      arrange(Season, Tier, Pos) %>% 
      within( {
        'Total Position' <- ave(Season, Season, FUN = seq_along)
      })  

    table_season_tiers <-  table_season_tiers %>% 
      mutate(`Total Position` = as.numeric(table_season_tiers$`Total Position`))
<br>

In the next step, I cleaned up some of the team names. Often, in standings teams that are relegated or promoted are listed with a R or a P, respectively, next to their name. I removed these with a simple string replace. The original data had a column which signified some of these places but it is incomplete. In a future project, I would like to make that column complete for all relegated or promoted teams.
<br>

In addition to the Rs and Ps attached to some team names, some team names in some years are written with abbreviations or shortened names. For example, the team Brighton & Hove Albion is written five different ways including the way shown, which is their official name. Again, I used a string replace to fix all these names which not only unifies all naming conventions for each team but allows aggregate data to be calculated by name correctly. As I am unsure of the origin of this data, I cannot fully explain why some seasons have different abbreviations for Brighton & Hove Albion and the others.
<br>

*Shiny app*
----------

I have been wanting to learn Shiny for a while and thought that I could use this newly clean data to try it out.
While I'm still working on the product you will see in part 2, I wanted to show everyone what I've learned so far. I want to give a big thanks to Coding Club because [their Shiny tutorial](https://ourcodingclub.github.io/2017/03/07/shiny.html) was the best one I saw online and really helped me through this project.
<br>

I decided to go for 
