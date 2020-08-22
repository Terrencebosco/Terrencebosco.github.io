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

After a HTML crash course I imported BeautifulSoup and got to work.  The main issue I ran into when trying to develop the web scrapper Is that each individual listing has its own unique url, rather than a “page1,page2,page3…” so one listing would be `vehicledetail/detail/813715333/overview/` and the next would be `vehicledetail/detail/817597633/overview/`This posed an interesting problem. I wasn’t able to form a range of the page numbers and  loop through `vehicledetail/detail/{np.arange(1-50} /overview/` for example. I first had to loop through all the listing previews and store all the unique url values in a list. Then use the values in the list to loop through all the individual listings and gather the necessary information

```python
pages = np.arange(1,51)
values = []

for page in pages:
    
    data = requests.get('https://www.___.com/for-sale/searchresults.action/?page='+str(page)+'&perPage=100&prMx=10000&rd=250&searchSource=PAGINATION&sort=relevance&stkTypId=28881&zc=33408')
    soup = BeautifulSoup(data.content, 'html.parser')
    search = soup.findAll('div', class_='listing-row__badges')
    
    sleep(randint(2,3))
    
    for post in search:
        values.append(post['data-submit-badge'])
```

once those values were stored in a list I was able to loop through them by adding them to the cars listing url template. once that car listining was requested I can pull the nessisary information and store them in their own lists. 

```python
car_names = []
car_mileage = []
price = []
ext_color = []
int_color = []
city_mpg = []
high_mpg = []
drivetrain = []
transmission = []
engine = []

for value in values:
    url = f'https://www.___.com/vehicledetail/detail/{value}/overview/'
    data = requests.get(url)
    soup = BeautifulSoup(data.content, 'html.parser')
    sleep(randint(2,5))
```

# Part 2: Cleaning and Exploring Data 

## Data Wrangling 

A majority of the data wrangling consisted of stripping unwanted characters from the data. There was also numerous rows that were out of order. When scrapping the data some of the elements were missing and added elements to the wrong list. A majority of my time was spend looking for elements that weren’t in the correct format and either cleaning them or deleting the row if there wasn’t enough usable data. Another factor I didn’t consider when scraping the data was that the exterior color has different names for different makes. I took those colors and changed them to a more general label. For example 

```python
		df.loc[df['ext_color'].str.contains('Cherry'), 'ext_color'] = 'Red'
```

I did this for each color I was able to identify. Colors I wasn’t able to identify were changed to “other”. Since the scraper pulled information even if it wasn’t correct caused there to be no missing or `np.nan` values. The cleaning was mostly finding rows that weren’t in the correct order and handling those. 

<a href="https://imgur.com/PUs6hBG"><img src="https://i.imgur.com/PUs6hBG.png" title="source: imgur.com" /></a>

## Exploratory Eata Analysis

When exploring the data it’s important to keep the main question in mind. Can we predict the price of a used gasoline car, under $10,000 within 200 miles of my home? With the question in the back of our minds we can begin to explore the data in relation to the question. My first step is to make a simple correlation matrix to see the relationships of each feature with one another. I focus most of my attention to the target vector of “price” and its relations with the other features.   

<a href="https://imgur.com/7xqQY88"><img src="https://i.imgur.com/7xqQY88.png" title="source: imgur.com" /></a>

Based on the correlation matrix I was able to identify specific relationships between the target (price) and the other features. The first of which is the relation between price and year (cars model year).

<a href="https://imgur.com/Ewt6wAe"><img src="https://i.imgur.com/Ewt6wAe.png" title="source: imgur.com" /></a>

> Based on this scatter plot we can see that there is a relationship between the price of the used car and the model year. As the year of the car goes up the price goes up, so there is a positive correlation between price and year. This isn’t necessarily a ground breaking discovery but it does help answer the question. Based on domain knowledge of vehicles it’s safe to assume that the newer cars are expected to have higher prices. So we were able to confirm our expectations of the data set. If we found the opposite to be true, year was negatively correlated to price, I would be suspicious of the data set. With our data meeting our expectations of what we “think” the data will look like is just as useful to us as finding something unexpected.  

Mileage is another feature that is strongly correlated with price. Although there is a key difference between the way year and mileage react with price. We saw that there is a positive relationship between year and price, but mileage on the other hand has a negative relationship with price. 
 
<a href="https://imgur.com/OUM6TCV"><img src="https://i.imgur.com/OUM6TCV.png" title="source: imgur.com" /></a>

> This is another situation where the findings aren’t necessarily surprising given the type of data we’re working with but the finding is just as important. There are two things I’m looking for when I conduct exploratory data analysis. The first being information that I presume to be true and the data reflecting those presumptions. Secondly, is there any unexpected connections that I didn’t see coming, and if there is why are they connected. For the case of Mileage vs Price we can see that mileage has a negative impact on price. The higher the mileage the lower the price. This is something that I can assume to be true without conducting any analysis of the data. The exploration just confirms my expectations. 

With these correlations and our initial question in mind we can explore these features a little more deeply. It’s important for us to understand the distribution of the data we’re working with. We’re going to be looking at 3 different distributions of data, the distribution of vehicle make, year, and mileage. 

<a href="https://imgur.com/w5RgiVv"><img src="https://i.imgur.com/w5RgiVv.png" title="source: imgur.com" /></a>

> As we can see the 3 main makes being sold at the time of data collection are;
> 1. Nissan
> 2. Ford
> 3. Hyundai

<a href="https://imgur.com/OL1waoz"><img src="https://i.imgur.com/OL1waoz.png" title="source: imgur.com" /></a>

> The distribution of vehicles being sold show us that a majority of vehicles being sold are between 2012-2017

<a href="https://imgur.com/BvELtJt"><img src="https://i.imgur.com/BvELtJt.png" title="source: imgur.com" /></a>

> The distribution of mileage ended up being surprisingly normalized. I expected there to be more fluctuations between the mileages. The mean mileage for our data set is 79,113 indicated by the red vertical line.

















