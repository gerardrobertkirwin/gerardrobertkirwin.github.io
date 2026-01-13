---
layout: post
title: "Betting on Myself: A Bayesian Betting System"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: seattlebaseball.jpeg
---

*Photo is my own.*

After a year, I have finished my Master's degree in AI for Games Development. Not one to rest on my laurels (and unable to rest on my bank account), I decided to build a project that would highlight my skills in the area of data engineering and data science. I wanted to create a platform, not just a pipeline.

I’ve done projects that highlight these at university and at my previous jobs, but those aren’t the kind of examples I would or could show off in my online portfolio. Also, I love to create my own projects that aren’t the same data science tutorials that endless numbers follow on YouTube. I like to dig deep and find questions, not just plug and play.

Beyond that, I wanted to learn a bit more about gambling. Despite being a huge sports fan my entire life, I’m still a bit flummoxed about certain betting terms and wanted to learn more. I chose baseball because it’s not commonly used for betting data science projects and it’s also one of my favourite sports. Plus it provides a lot of interesting statistics to play with.


*Data Engineering*
—---------------------

For this project, we needed data and my preference, as a data engineering project, was to find APIs to pull in data. Baseball has always been a sport strong in statistics, numbers and data. Major League Baseball knows this and has a robust statistical API that is easily accessible. Pulling game data from there was a relatively easy task, but I took care not to hit rate limits. I also performed hydration on the call, in order to get stats and scores together.

For the odds data, I struggled to find a free API for the data and did not wish to worry about what fees I could be paying for pulls, especially for a straightforward, one-time project. So I decided to pull from the [MLB Odds Scraper](https://github.com/ArnavSaraogi/mlb-odds-scraper). 

After pulling in the baseball and odds data, some cleanup was needed. The MLB data had full team names (Arizona Diamondbacks), while the betting data had abbreviations (ARI). I constructed a conversion map to convert the names to abbreviations. Other minor issues occurred in terms of game type and date, but those were easily solved by simple type conversions and filtering.

The biggest issue with the data was what I like to call “the doubleheader trap”. For those unfamiliar, occasionally baseball teams will play the same opponent twice in one day. Often it is to make up for a previous game cancelled due to weather. Usually the first game will be in the afternoon and the second will be in the evening. Initial joins on the data were done on date, which saw the loss of up to a dozen games on our test season. Changing the join to be on date, team and score proved to fix the issue. The New York Mets could have scored 1 run in each game on July 4th, but it was highly unlikely the Atlanta Braves would score 3 in both games as well.


*Modeling*
-----------

