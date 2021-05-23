---
layout: post
title: "How I Beat A Supercomputer"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
maps: true
image: citychampions.jpeg
---

Photo by <a href="https://www.reuters.com/">Reuters</a> via <a href="https://www.khaleejtimes.com/sport/football/man-city-crowned-premier-league-champions-after-man-united-loss">Khaleej Times</a>

A while back on Instagram, I saw a <a href="https://www.thesun.co.uk/sport/football/13560694/premier-league-supercomputer-arsenal-relegation-chelsea">post</a> that a supercomputer used data from the last 10 years to predict the final English Premier League football table (or soccer standings for American readers). The supercomputer based its predictions on Christmas Day, a symbolic midway point for the season. This piqued my interest because of my interest in data science and soccer but also because I wondered whether a supercomputer would be necessary to predict it.

When I was in school for meteorology, the weather forecasts and climate models that we learned about relied on supercomputers. The atmosphere is vast, the earth is large and there’s a lot of data points such as temperature, wind speed and atmospheric energy to account for. Soccer games produce a lot of data as well but nowhere near on the scale. 

Even if you did simulate the remaining games a large amount of times using a Monte Carlo simulator like FiveThirtyEight, all you would need is a consumer-grade PC. A supercomputer seems unnecessary for this task and I suspect the use of “supercomputer” in the headline was a combination of clickbait and taking advantage of tech illiteracy in the public.

<a href="https://www.football365.com/news/premier-league-table-predicted-averages-mediawatch">Sarah Winterburn from Football365 claims</a> that the “supercomputer” is just running off averages. For example, Arsenal were 15th on Christmas Day and their predicted finish of 16th is an average finish of any team in 15th place on Christmas Day. I suspect this isn’t far off from the truth. I looked on The Sun and Bettingexpert.com and there was no methodology for this anywhere. 

On a whim one afternoon, I decided to type a little machine learning code to see what I would get as a result. I then decided to turn this into a portfolio project and see if my predictions could beat the "supercomputer".


*Data*
----------

The data I needed to run was the table at Christmas and the final table. However, teams may have played in different amounts of games by Christmas Day. Teams may have completed less games on the same day due to <a href="https://en.wikipedia.org/wiki/Glossary_of_association_football_terms#F">fixture congestion</a>, the placement of Christmas Day in the footballing week and in the case of the last year, the effects of COVID-19. I decided instead of basing it exactly on the Christmas table, that I would pick a set number of matches so the number of goals and points from extra or missing games wouldn’t skew the results. I used 17 games as my baseline because that was the most common number of games completed by Christmas Day.

I then set about gathering the data and merging the Christmas Day and Final tables so I had a single data table to train and test. I also gathered the 17 game results from the 2020-2021 season for my final test.

*Modeling*
-----------------

My next step was to see what algorithms would give me the best results from the Christmas Day table. I borrowed a method from <a href="https://machinelearningmastery.com/machine-learning-in-python-step-by-step/">this page</a> to test out some algorithms and which worked best. The code produced a box and whisker plot showing the algorithms side by side:



Since the accuracy was based on predicting the number of points at the end of the season and not the position, the accuracy values are quite low. The points total system (3 points for a win and 1 for a draw or tie) is a calculation, not a value so I wasn’t expecting these algorithms to predict the exact numbers. Remember we are looking to predict the order of the finish, not the points total. So Logistic Regression and LinearDiscriminantAnalysis should give us the best results.

I decided to use all the relevant columns in the Christmas Day table, Wins, Losses, Draws, Goals Fielded, Goals Allowed, Goal Difference and Points. My rationale was that they were all good indicators of points and position, but picking and choosing some would skew the results in one way or another. For example, Team A could win fewer games than Team B but could finish ahead if they had more draws and therefore more points. So for simplicity, I left all these features in.

After testing out Logistic Regression on my test and train data, I was ready to let it loose on the current year’s data. Here were my results:



Once again, the points values are secondary to the order.

I did the same thing for LinearDiscriminantAnalysis, which had the same accuracy score as the Logistic Regression. However, their results were very different:




*Analysis*
----------------


*Next Steps*
--------------------


