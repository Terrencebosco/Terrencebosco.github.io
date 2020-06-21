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
-To gain an understanding of how the violations are categorized and how many of each type there is we can look at figure 1

*Figure 1*

<iframe width="700" height="700" frameborder="0" scrolling="no" src="//plotly.com/~Terrence.bosco/1.embed"></iframe>

* As we see in figure 1 there are 64 different types of violations in our sample. Of those 64 violation 5 of them make up more than 50% of all the tickets given and “No Parking” tickets being the most making up 20% of the 10,000 sampled violations.

We can gain even more of an understanding of the data set when we set those top 5 violations to the borough they were filed in.

*figure 2*
[Imgur](https://i.imgur.com/MgL0uOR.png)

* Figure 2 shows the breakdown of the top five ticket types and their respected borough. We can see that no parking is the leading ticket type for each borough except Richmond. Also, speeding is the second leading ticket type for each borough except for Manhattan where speeding is the least. Another interesting note is how few tickets there are for Richmond in our sample.

<iframe width="700" height="700" frameborder="0" scrolling="no" src="//plotly.com/~Terrence.bosco/19.embed"></iframe>
