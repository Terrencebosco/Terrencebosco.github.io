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


#### Day of the Week

with figure 5 below we can see that most of the tickets written were during the work week of Monday through Friday with less on Saturday and even less on Sunday. We might assume that more people are driving during the week go to work than the weekends. So there are more opportunities for tickets to be written.

*figre 5*

![Imgur](https://i.imgur.com/4Q9gSia.png)


#### Month of the Year

in figure 6 below we can see that the months of July and December are the two months with the least amount of tickets and September and may having the most tickets given.

*Figure 6*

![Imgur](https://i.imgur.com/wHvCTha.png)


### Hour of the day VS. Day of the week

*figure 7*
<iframe width="700" height="800" frameborder="0" scrolling="no" src="//plotly.com/~Terrence.bosco/15.embed"></iframe>

* Here we can look at the distribution of tickets given in hours vs Day of the week. We can see that the hour of the day and day of the week are consistent with our graphs above. A majority of tickets given are during the hours of 8am and 3pm during the day of Monday – Friday. With the most tickets given on Thursdays between the hours of 9am and 10am


### Day of the week VS. Month

*figure 8*
<iframe width="700" height="800" frameborder="0" scrolling="no" src="//plotly.com/~Terrence.bosco/17.embed"></iframe>

* This heat map shows the distribution of tickets for each month and the day of the week for that month. This shows a consistence with our graphs above that the majority of tickets were given between Monday and Friday. Also, the most tickets given in the year was September Thursdays. This can explain why September has the most of any month. 
