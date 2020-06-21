---
layout: post
title: When are parking tickets being written?
subtitle: An exploration into new york cities 2017 parking violation data set.
cover-img: https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcSvTwcNZGvPOPu5K4mTKtFjVd6Ge4yA2hDF-O651AVC0RKG0cPj&usqp=CAU
tags: [blog,build week]
---
# Purpose of this post:

This is will be the first of three post in the exploration of New York City’s 2017 parking and traffic violations data set. This data set contains over 3 million instance of a parking or traffic violation for the year of 2017 in the city of New York. This includes the time and date of the violation, the vehicles characteristic, the location of the violation occurred, and finally the reason for ticket to be written. Do to the sheer size and depth of this data set I will break the exploitation into 3 parts, when are violations being written, what vehicles are receiving violations and where are the violations happening.  Before we begin I’d like to preface that because the size of this data set is so large I took a random sample of 10,000 violations to conduct my exploratory data analysis. 


## When are violations being written?


### Violations at a glance

To gain an understanding of how the violations are categorized and how many of each type there is we can look at figure 1

*Figure 1*

<iframe width="700" height="700" frameborder="0" scrolling="no" src="//plotly.com/~Terrence.bosco/1.embed"></iframe>

* As we see in figure 1 there are 64 different types of violations in our sample. Of those 64 violation 5 of them make up more than 50% of all the tickets given and “No Parking” tickets being the most making up 20% of the 10,000 sampled violations.

We can gain even more of an understanding of the data set when we set those top 5 violations to the borough they were filed in.

*figure 2*

![figure2](https://i.imgur.com/MgL0uOR.png)

* Figure 2 shows the breakdown of the top five ticket types and their respected borough. We can see that no parking is the leading ticket type for each borough except Richmond. Also, speeding is the second leading ticket type for each borough except for Manhattan where speeding is the least. Another interesting note is how few tickets were recorded for Richmond in our sample.



### Time Breakdown

Our time variable will be broken down into three part, hour of the day, day of the week, and month of the year.

*figure 3*

<iframe width="700" height="700" frameborder="0" scrolling="no" src="//plotly.com/~Terrence.bosco/19.embed"></iframe>

we can gain a better understanding of the disterbution of tickets verse the 3 time intervals when we look at them individually. 

#### Hour of the Day

We can see in figure 4 below that the majority of the tickets given are between the hours of 8am and 3pm. This is something that doesn’t evidently surprise me. We might assume that more people are out driving and parking at this time than between 3pm and 8am. We might also account for this with the idea that more officers that write parking tickets are working during this time than later at night. 

*figure 4*
![Imgur](https://i.imgur.com/eDOBvXg.png)




