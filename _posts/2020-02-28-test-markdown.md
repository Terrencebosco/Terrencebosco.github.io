---
layout: post
title: Predictive modeling from scratch  
subtitle: Predicting used car prices with web scrapped data
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [blog,build week 2]
comments: true
---
Titanic, Boston house prices, iris flower. These are all data sets an aspiring data scientist is more than then familiar with. After completing YouTube tutorials and lectures of these data sets I found myself scrolling through Kaggle or UCI Machine Learning Repository looking for a practice dataset to test my newly gained knowledge of supervised machine learning. As I red description after description I kept coming back to the same question “how are these datasets made?”. After a quick google search I discovered the world of web scraping using python. After reading two articles, a full cup of coffee and a blank jupyter notebook I got to work creating a predictive model from scratch as a proof of concept to myself.

# Part 1: Data collection

## Step one, ask the question 

The first step was to figure out the question I wanted to answer. After some deliberation I settled on trying to predict used car prices based on the cars characteristics. I went to a popular new and used car website so gather my data. I narrowed the search parameters to cars that are used, less than $10,000, and within 100 miles of my home. 

## Step two, what sort of information do I need?

The used car website breaks the listings into two parts. The first page is a 20 of the listings under the parameters I those (the preview). Then each of those individual listings will take you to the specific listing for that car (the listing). The previews give the basic information about the vehicles make, model, price, mileage, color, the transmission type, and the drivetrain. 

> <a href="https://imgur.com/LQWGTL4"><img src="https://i.imgur.com/LQWGTL4.png" title="source: imgur.com" /></a>

Under the actual listings more information is provided. Such as the fuel type, miles per gallon city and highway, engine type and size, and a more descriptive transmission listing. 

> <a href="https://imgur.com/XzDsmrm"><img src="https://i.imgur.com/XzDsmrm.png" title="source: imgur.com" /></a>

The information im looking to gather:
1. vehicles make
2. vehicles model
3. price
4. mileage
5. miles per gallon
6. drivtrain
7. transmission
8. interior color
9. exterior color

## Step three, scrap the data

After a HTML crash course I imported BeautifulSoup and got to work.  The main issue I ran into when trying to develop the web scrapper Is that each individual listing has its own unique url, rather than a “page1,page2,page3…” so one listing would be `vehicledetail/detail/813715333/overview/` and the next would be `vehicledetail/detail/817597633/overview/`This posed an interesting problem because I wasn’t able to form a range of the page numbers loop through vehicledetail/detail/{np.arange(1-50} /overview/ for example. I first had to loop through all the listing previews and store all the unique url values in a list. Then use the values in the list to loop through all the individual listings and gather the necessary information
