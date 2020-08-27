---
layout: post
title: Predictive Modeling From Scratch  
subtitle: Predicting used car prices with web scrapped data.
cover-img: https://www.autolist.com/6tuem73u73an/K8NOKYJ2aOYmm8Yc2suQU/f6a9695af3a32ea572880dcb84212c7f/buying-used-car-1166-image.jpg 
tags: [blog,build week 2,predictive]
comments: true
---
Titanic, Boston house prices, and iris flower. These are all data sets an aspiring data scientist is more than familiar with. After completing YouTube tutorials and lectures of these data sets I found myself scrolling through Kaggle or UCI Machine Learning Repository looking for a practice dataset to test my newly gained knowledge of supervised machine learning. As I read description after description I kept coming back to the same question “how are these datasets made?”. After a quick google search I discovered the world of web scraping using python. After reading two articles, a full cup of coffee and a blank jupyter notebook I got to work creating a predictive model from scratch as a proof of concept to myself.

# Part 1: Data Collection

## Step One, Ask the question. 

The first step was to figure out the question I wanted to answer. After some deliberation I settled on trying to predict used car prices based on the cars characteristics. I went to a popular new and used car website to gather my data. I narrowed the search parameters to cars that are used, less than $10,000, and within 100 miles of my home. 

## Step Two, What sort of information do I need?

The used car website breaks the listings into two parts. The first page is 20 of the listings under the parameters I chose (the preview). Then each of those individual listings will take you to the specific listing for that car (the listing). The previews give the basic information about the vehicles make, model, price, mileage, color, the transmission type, and the drivetrain. 

> <a href="https://imgur.com/LQWGTL4"><img src="https://i.imgur.com/LQWGTL4.png" title="source: imgur.com" /></a>

Under the actual listings more information is provided. Such as the fuel type, miles per gallon city and highway, engine type/size, and a more descriptive transmission listing. 

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

## Step Three, Scrape the data.

After a HTML crash course I imported BeautifulSoup and got to work.  The main issue I ran into when trying to develop the web scrapper is that each individual listing has its own unique url, rather than a “page1,page2,page3…” so one listing would be `vehicledetail/detail/813715333/overview/` and the next would be `vehicledetail/detail/817597633/overview/`This posed an interesting problem. I wasn’t able to form a range of the page numbers and  loop through them for example `vehicledetail/detail/{np.arange(1-50)} /overview/`. I first had to loop through all the listing previews and store all the unique url values in a list. Then use the values in the list to loop through all the individual listings and gather the necessary information

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

once those values were stored in a list I was able to loop through them by adding them to the cars listing url template. Once that car listing was requested I can pull the necessary information and store them in their own lists. 

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

A majority of the data wrangling consisted of stripping unwanted characters from the data. There were also numerous rows that were out of order. When scrapping the data some of the elements were missing and added elements to the wrong list. A majority of my time was spent looking for elements that weren’t in the correct format and either cleaning them or deleting the row if there wasn’t enough usable data. Another factor I didn’t consider when scraping the data was that the exterior color has different names for different makes. I took those colors and changed them to a more general label. For example:

```python
		df.loc[df['ext_color'].str.contains('Cherry'), 'ext_color'] = 'Red'
```

I did this for each color I was able to identify. Colors I wasn’t able to identify were changed to “other”. Since the scraper pulled information even if it wasn’t correct it caused no missing or `np.nan` values. The cleaning was mostly finding rows that weren’t in the correct order and handling those.

<a href="https://imgur.com/PUs6hBG"><img src="https://i.imgur.com/PUs6hBG.png" title="source: imgur.com" /></a>
> data after wrangling.
## Exploratory Data Analysis

When exploring the data it’s important to keep the main question in mind. Can we predict the price of a used gasoline car, under $10,000 within 100 miles of my home? With the question in mind we can begin to explore the data in relation to the question. My first step is to make a simple correlation matrix to see the relationships of each feature with one another. I focus most of my attention to the target vector of “price” and its relations with the other features.   

<a href="https://imgur.com/7xqQY88"><img src="https://i.imgur.com/7xqQY88.png" title="source: imgur.com" /></a>

Based on the correlation matrix I was able to identify specific relationships between the target (price) and the other features. The first of which is the relation between price and year (car's model year).

<a href="https://imgur.com/Ewt6wAe"><img src="https://i.imgur.com/Ewt6wAe.png" title="source: imgur.com" /></a>

> Based on this scatter plot we can see that there is a relationship between the price of the used car and the model year. As the year of the car goes up the price goes up, so there is a positive correlation between price and year. This isn’t necessarily a ground breaking discovery but it does help answer the question. Based on domain knowledge of vehicles it’s safe to assume that the newer cars are expected to have higher prices. So we were able to confirm our expectations of the data set. If we found the opposite to be true, year was negatively correlated to price, I would be suspicious of the data set. With our data meeting our expectations of what we “think” the data will look like is just as useful to us as finding something unexpected.  

Mileage is another feature that is strongly correlated with price. Although there is a key difference between the way year and mileage react with price. We saw that there is a positive relationship between year and price, but mileage on the other hand has a negative relationship with price. 
 
<a href="https://imgur.com/OUM6TCV"><img src="https://i.imgur.com/OUM6TCV.png" title="source: imgur.com" /></a>

> This is another situation where the findings aren’t necessarily surprising given the type of data we’re working with but the finding is just as important. There are two things I’m looking for when I conduct exploratory data analysis. The first being information that I presume to be true and the data reflecting those presumptions. Secondly, is there any unexpected connections that I didn’t see coming, and if there were why are they connected? For the case of Mileage vs Price we can see that mileage has a negative impact on price. The higher the mileage, the lower the price. This is something that I can assume to be true without conducting any analysis of the data. The exploration just confirms my expectations. 

With these correlations and our initial question in mind we can explore these features a little more deeply. It’s important for us to understand the distribution of the data we’re working with. We’re going to be looking at 3 different distributions of the data vehicle make, year, and mileage. 

<a href="https://imgur.com/w5RgiVv"><img src="https://i.imgur.com/w5RgiVv.png" title="source: imgur.com" /></a>

> As we can see the 3 main makes being sold at the time of data collection are:
> 1. Nissan
> 2. Ford
> 3. Hyundai

<a href="https://imgur.com/OL1waoz"><img src="https://i.imgur.com/OL1waoz.png" title="source: imgur.com" /></a>

> The distribution of vehicles being sold shows us that a majority of vehicles being sold are between 2012-2017

<a href="https://imgur.com/BvELtJt"><img src="https://i.imgur.com/BvELtJt.png" title="source: imgur.com" /></a>

> The distribution of mileage ended up being surprisingly normalized. I expected there to be more fluctuations between the mileages. The mean mileage for our data set is 79,113 indicated by the red vertical line.

# Part 3: Model Creation and Feature Selection

## Baseline Model
The metric of measurement for model evaluation I’ll be using is the ```mean_absolute_error```. We can use this to get a baseline prediction based on the characteristics of each vehicle. To do that i'll start with the mean price (8041.36) and use that as a baseline prediction of each vehicle and see how far off I would be using the mean absolute error. 

```python
# get base line prediction and mean absolute error score

#. get mean
price_mean = train['price'].mean()

#. get prediction with mean
y_pred = [price_mean] * len(train['price'])

#. get base line score
baseline = mean_absolute_error(y_pred,train['price'])
```

> With this method used we get a mean absolute error of 1429.20. This can be expressed as “if we guess the mean price for the actual price of the vehicle we will be off on average by $1400”. With a little supervised machine learning we can do better!

## A Simple Model
Once we have a baseline prediction we have a target to beat. With supervised machine learning I want to be able to score better than my baseline model. A better score would tell me that I am able to better generalize a prediction than just guessing mean. For my predictive model I’ll be using RandomForestRegressor from the scikit-learn library. I split my data into 3 parts, training data, validation data, and testing data. 

```python
# simple predictive model
pipeline = make_pipeline(
    ce.OrdinalEncoder(),
    RandomForestRegressor( random_state=42,
                          n_jobs=-1)
)

# simple prediction
pipeline.fit(X_train, y_train);
y_pred = pipeline.predict(X_val)
y_pred2 = pipeline.predict(X_train)

Validation Mean Absolute Error: 979.80
```
With this simple RandomForestRegressor model I was able to lower my mean absolute error to 979.80 on my validation set. That’s almost a 500 point reduction! I was able to lower the error by almost a third with a simple predictive model with no hyper parameters tuned, no feature selection, and no cross validation. 

## Feature Selection
Once I was able to beat baseline my goal was to make a simpler model in the sense of the number of features used and still beat my first model. I conducted the feature importance with the eli5 API permutaion importance on the first RandomForestRegressor model to see what features contributed the most to the prediction. 

<a href="https://imgur.com/JuyhEIY"><img src="https://i.imgur.com/JuyhEIY.png" title="source: imgur.com" /></a>

Here we can see the weight the RandomForsetRegressor places on each feature and its standard deviation as well as the feature associated with the weight. As to be expected year and mileage contribute the most to the price prediction of the used vehicle. Another interesting point is the `front_wheel_drive` feature. This feature actually made our model worse by being included. I used 13 as an arbitrary threshold to remove the features that didn’t contribute as much or even negatively to the model. Then constructed a new model with new hyper parameters and the top 8 features. 

```python
# fit model with features > 13 (importance)
pipeline = make_pipeline(
    ce.OrdinalEncoder(),
    RandomForestRegressor(n_estimators=100,
                          random_state=42,
                          max_depth=25,
                          n_jobs=-1)
)
Validation Mean Absolute Error: 960.059
```

With the new model I was able to lower the mean absolute error of the validation set to 960.06. That’s another 20 points of reduction with simply selecting the features, and tuning the model's settings. 

# Part 4: Interpretation
I’m using a RandomForestRegressor model that are commonly referred to as black box models. Black box models are notoriously hard to explain because we don’t exactly know how the determinations are being made. Even though they are hard to interpret, there are tools available that we can use to gain better insight on how they work. In this case we can use partial dependencies and shap values to evaluate and explain our model. 

## Partial Dependency
We can look at the partial dependency or functional relationship between features.The models two most important features are mileage and year. We can use partial dependency to see their inpact on price.

<a href="https://imgur.com/9u08qPJ"><img src="https://i.imgur.com/9u08qPJ.png" title="source: imgur.com" /></a>

> We can see how the model uses the mileage and year of the vehicle to predict the price using the pdp interaction plot. We can see that the cheapest vehicles predicted are those with the highest mileage and earliest year. The most expensive cars are those that are newer with lower mileage. 

## Shap Values
We can use shap values as a form of coefficient to help explain how each feature reacts with our model. The shap values interpret the impact a particular value of a particular feature will have on our model. 

<a href="https://imgur.com/OXj0cdx"><img src="https://i.imgur.com/OXj0cdx.png" title="source: imgur.com" /></a>

> We can see that any values to the left of the 0 baseline negatively affect the value of the prediction and values on the right of the 0 affect the prediction in the positive direction. Then we have to look and see the value of each point and how they are distributed across the 0 baseline. Mileage values that are higher negatively affect the prediction and vice versa for lower values. We can interpret this by "cars with lower mileage are predicted to cost more". Values for car year that are low negatively affect the prediction price and values that are high positively affect the predicted price. We can interpret this as "new cars are predicted to cost more". 

# Part 5: The Final Prediction 
After gathering the data, exploring the data, testing validation sets, and interpreting the models the only thing left to do Is predict with the testing set. Im going to use the feature selected model to predict my testing set, with the same parameters and encoders. This is entirely new information that the model has yet to see and will let us know how well the model generalizes predictions. 

```python
pipeline.fit(X_train_perm,y_train)
y_pred = pipeline.predict(X_test_perm)
mae = mean_absolute_error(y_test,y_pred)
print('Validation Accuracy', mae)

Test Mean Absolute Error: 950.618
```
After we ran our model again with the testing set we ended up with a mean absolute error of 950.618. That’s an even better score than our validation set by about 10 points. I would consider that a win!


# Part 6: The Good, The Bad, The Ugly

Even though I was able to beat my baseline prediction by about 500 points I think with a little more work that can be improved upon. If I were to do this project over there are numerous factors that I would try and improve on. I would try and collect more data. I think with more data and more vehicles I would be able to make a better predictor. I would build a better scrapping method and account for missing values. The main issue that this project runs into is ultimately the lack of data. Having only 2000 different vehicles in my data set makes it difficult to get an accurate predictions.

## Resources
[Github repository](https://github.com/Terrencebosco/Build_week_2)
