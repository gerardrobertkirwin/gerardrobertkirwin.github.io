---
layout: post
title: "On The Way to Work"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: londonbikecommuters.jpg
---

<p style="font-size: 0.9rem;font-style: italic;">
<a href="https://www.flickr.com/photos/24105592@N02/8498034692">"London bike commuters"</a><span> by
<a href="https://www.flickr.com/photos/24105592@N02">kube414</a></span>
is licensed under
<a href="https://creativecommons.org/licenses/by-nc-nd/2.0/?ref=ccsearch&atype=html" style="margin-right: 5px;">CC
BY-NC-ND
2.0</a></p>

I’ve had a keen interest in maps since I was a child. I’m not sure how it started but for as long as I can remember, I’ve been captivated by weather maps, world maps, the 1992 Rand McNally foldable map of the Southwest in my mom’s minivan. I’ve been searching for a data science project in R to give me an opportunity to learn how to make maps in R.

I minored in Geography (with a focus on GIS) in college 10 years ago but the software is so expensive and didn’t work on my Mac, it made it impossible for me to use and practice. I really enjoyed using it and was really excited when I learned that I could do the same things in R!

My first idea was to create a map of places that I have photographed. My plan was to use Google Maps and my memory to try and find approximate latitudes and longitudes and put them on the map. Since my photographs are film photographs, there is obviously no metadata so I would have to produce it myself. I started working on this but it was so time consuming, and work has been busy lately so I put this project aside and looked for another project where I could develop some mapping skills.

Naturally, one of the first places I looked was #TidyTuesday, the Twitter-based community where a new set of data is released each week and people are asked to share what they come up with. I have been wanting to do some to flex my R skills so I waited for one where I could turn a dataset into a map.

*Data*
----------
This week’s [#TidyTuesday](https://twitter.com/thomas_mock/status/1191368456156958720) had a data set of biking and walking commutes in the United States. 
It is a relatively simple dataset. 9 columns and just under 3500 rows representing small, medium and large American cities with both “bike” and “walk”. 
The data came from the American Community Survey run by the United States Census. The data from the Census broke it down by “city size”. I couldn’t find another database where those categories were broken down, so I decided to use the Tidy Tuesday dataset from [GitHub](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-11-05) instead.

*Analysis* 
----------
My first task (outside of downloading the data and uploading the needed packages) was to clean up the data. The “city” column was messy. 
It included the city name and the designation of the city, for example, a township or borough. 
This information is not necessary for my purposes so I got rid of it.

Code xxxx

Then I created a new column of the city and state combined to allow for the searching of the geographic coordinates. My original plan was to use the ggmap package. However, because the package uses the Google API, it required registering. I don’t know how often I plan to use it so I decided to look around for alternatives. 

It was through this I was able to learn about another R mapping package, leaflet. This package uses the OpenStreetMap you are likely familiar from craigslist. This meant I needed to do another step, as ggmap has a built-in coordinate fetcher and leaflet does not.


I found an answer on StackOverflow and I rewrote the code for my purposes which gave me geographic coordinates for all the cities in my dataset. From here, I did a little more tidying with the place names and had to change the coordinates to the correct type.

Codexxxx

*Visualization*
----------
