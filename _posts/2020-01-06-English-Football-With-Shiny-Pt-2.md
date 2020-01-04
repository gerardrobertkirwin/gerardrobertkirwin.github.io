---
layout: post
title: "English Football with Shiny Pt. 2"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
maps: true
image: prem_shiny_2.jpg
---

<p style="font-size: 0.9rem;font-style: italic;"><img style="display: block;" src="https://live.staticflickr.com/6063/6074270007_1044a309eb_b.jpg" alt="Cowie Celebrates"><a href="https://www.flickr.com/photos/37195744@N03/6074270007">"Cowie Celebrates"</a><span> by <a 
href="https://www.flickr.com/photos/37195744@N03">joncandy</a></span> is licensed under <a href="https://creativecommons.org/licenses/by-sa/2.0/?ref=ccsearch&atype=html" style="margin-right: 5px;">CC BY-SA 2.0</a></p>

On the [last episode](https://www.gerardrobertkirwin.com/blog/2019/12/30/english-football-with-shiny-pt-1)...
I made a simple Shiny app that allowed users to view English football standings tables by year and division.
<br>

In this blog, I will add to my app and add a plotly graph.
<br>

*Data Cleanup and Calculations*
----------

Next, I wanted to calculate each team's "total position" for each year. 
English football operates on a system of [relegation and promotion](https://en.wikipedia.org/wiki/Promotion_and_relegation) between their multiple divisions which are tiered. 
Usually the bottom 2-4 teams in each division are relegated to a lower division and the bottom 2-4 teams in each division are promoted to the next division. 
I wanted to calculate where every team is on the system of tiers each year and be able to graph that which would be a good visualization of how teams have progressed through the tiers. 
The top team in the top tier is 1 and the bottom team in the bottom tier is the highest number (for example 92 in 2017-18).
<br>

To figure out each team's "total position", I needed to have a counting sequence through the tiers, and have it start over every year. 
In my dataset, the tiers were listed as text such as "First Tier", which is unreadable for a sequencing function. So, I changed each tier to a number, 1 through 4, which the function could sequence through. 
Then I used R's within function to sequence through the tiers and then through the years. This worked a charm showing me 1 for the top team in the top division in each year and a number in the 80s or 90s (depending on the size of English League football in a particular year) for the worst team in the lowest division.
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

