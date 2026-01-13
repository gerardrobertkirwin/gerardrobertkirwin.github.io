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


*Modelling*
-----------

With the connection made, the time was to look at what stats I would use to assist my model. Runs (points scored) and wins are simple but should be useful enough for our purpose of determining the likelihood of a winner. These can be fed into the “Pythagorean Theorem of Baseball”, devised by Bill James, the father of modern baseball statistics. A team’s Pythagorean Expectation is a measure of how a team should do over a series of games, based on the runs they score and runs they allow. Teams that win more games than this amount are considered ‘lucky’ while teams that score less are considered ‘unlucky’.

The Pythagorean Expectation can then be fed into the [log5 method](https://sabr.org/journal/article/matchup-probabilities-in-major-league-baseball/#:~:text=Both%20formulas%20have%20been%20observed,triple%2C%20home%20run%2C%20etc.) to determine the probability that a team will beat another team. So even if a team is ‘lucky’ and outperforms their Pythagorean record, the log5 can counteract that and see the team for how they’re truly performing. This is perfect for a simple win/loss model built on simple features.

The centrepiece of the feature engineering was the calculate_rolling_features function. Instead of covering the entire season up to today, I decided that a look at the previous 10 games would be a better way to predict future outcomes. In real time, 10 games is about 2 weeks of games, and on sports websites it is often used as a delineation of a team’s recent play. 

In data terms, we also need to be careful not to include today’s game into the data. The logic equivalent would be “up to” today’s date but not including it. This is done for each feature (like runs scored) using the code below:

```
   for metric in metrics:
        col_name = f'rolling_{window_size}_{metric}'

        df_features[col_name] = (
            df_features.groupby('team')[metric]
            .transform(lambda x: x.shift(1).rolling(window=window_size).mean())
        )
```

Without this simple code, the model would be able to predict today’s score using…today’s score. Not very useful for a predictive model.

A Bayesian Linear Model (using PyMC) was chosen because it's more robust than a standard logistic regression model. It accounts for random effects of baseball (of which there are many) and produces a range of probabilities. It creates a "Margin of Safety" where the bets are placed only when the model's confidence is significantly higher than the market odds. For this project, it is set to 5% above market odds but it is a variable that can be changed if the bettor so chooses.

If you check ESPN.com for example, they will tell you the percentage chance that a team will win the game during the game. I don’t enjoy this very much as a fan, it reminds me too much of Hillary Clinton’s election night in 2016. A statistical moment that ended up being misinterpreted and incorrect. However, I can see its use in betting markets and as a longer term metric. 

*Moving to Production*
—--------------------------


