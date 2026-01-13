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
------------------

For this project, we needed data and my preference, as a data engineering project, was to find APIs to pull in data. Baseball has always been a sport strong in statistics, numbers and data. Major League Baseball knows this and has a robust statistical API that is easily accessible. Pulling game data from there was a relatively easy task, but I took care not to hit rate limits. I also performed hydration on the call, in order to get stats and scores together.

For the odds data, I struggled to find a free API for the data and did not wish to worry about what fees I could be paying for pulls, especially for a straight forward, one-time project. So I decided to pull from the (https://github.com/ArnavSaraogi/mlb-odds-scraper)[MLB Odds Scraper]. 



