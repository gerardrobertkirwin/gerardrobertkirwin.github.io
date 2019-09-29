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
I stumbled upon the Women In Parliament data set on [Twitter](https://twitter.com/ilustat/status/1154401034183352321) and was interested in doing a project that, as the tweet says, isn't the "car mpg" or "iris" projects that are the textbook projects. I was also drawn to the project for its real world implications and possiblities for social and political impacts. Not something you can get from the sepal length of an iris.

Analysis
----------
Included with the tweet was some instructions to follow through and clean the data. I went through the directions, which cleaned the data and produced graphs. I decided to redo the project, using the United States instead of Portugal.

I have also added two more items to clean up the dataset. First, since I am focusing on the United States, it would be useful to compare it to other North American countries. Unfortunately, the data set puts North and South American countries together as "The Americas".
I put together the following code to create a "North America" and "South America" region for better analysis.

    WPN <- WP2 %>% 
        mutate(Continent=replace(Continent, 
                         Country %in% c("Antigua and Barbuda", "Bahamas, The", "Barbados", "Belize", "Canada", "Caribbean small states", "Costa Rica", "Cuba", "Dominica", "Dominican Republic", "El Salvador", "Grenada", "Guatemala", "Honduras",   "Haiti", "Jamaica", "Mexico", "Nicaragua", "Panama", "Puerto Rico", "St. Kitts and Nevis", "St. Lucia", "St. Vincent and the Grenadines", "Trinidad and Tobago", "United States"),
                         "North America")) 

    WPN <- WPN %>%   
        mutate(Continent=replace(Continent, 
                         Country %in% c("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador", "Guyana",  "Paraguay", "Peru", "Suriname", "Uruguay", "Venezuela, RB"), 
                         "South America"))

The second issue I had with the dataset was the odd values in 1990. Every plot from the exercise made 1990 seem like an outlier, it skewed each graph in a way that didn't seem correct. From the example in the documentation with European countries, the percent of women in the Romanian parliament took a sharp drop after 1990. Knowing that this was right after the fall of Communism, I wondered if the spirit of revolution had produced progress on gender equity, only for it to receed in the years following.

A little investigation finds that the data was incorrect. The dataset shows Romania with a 34.4% of women in their 1990 Parliament while my research found that it was only 3.62% (14 out of 387) in the lower chamber and 0.84% (1 out of 119) in the upper chamber. While Romania is just one example, it is clear there is something wrong with the data.

From the documentation, the 1990 data is from the World Bank directly while the 1997 and later data is from the Inter-Parliamentary
Union (IPU). In addition, the World Bank data included upper chambers while the IPU data only includes lower chambers. I'm not sure that either of these are the cause of the enormous error we saw in the Romanian example but I know the inconsistent and unreliable data should be removed. 

     WPN <- WPN %>% 
        filter(Year != 1990)


Visualizations
---------------

With the 1990 data removed and The Americas seperated into North and South, I am ready to do some visualizing and analysis. The North American data set has 24 Countries in it and when graphed, it is messy and not nice to read, click if you dare. From this point forward, I will use the ten largest countries in North America.

Here's a basic line graph with our data:

(/img/WiP_Plots/North_America_Final_Line Plot.png)

I used a portion of the code from the documentation but moved the legend to the bottom, added the title, changed to the minimal theme and chose a rainbow color scale. All of these things were done to make the graph easier to read. With this easier-to-read graph, you can see that the United States (in pink) lags behind Cuba, Mexico, Nicaragua, El Salvador, Canada, Dominican Republic and Honduras in women representation in national legislatures in 2018.


I tried a number of other interesting visualizations to try and visualize this data, including a violin graph. The best representation I could find was a ridgeline plot. 

Conclusions
---------------
This data goes through 2018. In 2019, the percentage of women in the United States lower chamber (the House of Representatives) has increased to 24% (based on my research) from 19.8% in 2018. No surprise here, the election of several new women in the House was a major story in the 2018 midterm election. With politics being such a volatile field, it's hard to predict where the number will go from here.

other countries etc

You can check out the project on github here.
