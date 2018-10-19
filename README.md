# Predicting violent crime in Chicago, Illinois

## Overview

For this project, I wanted to explore the relationship between weather and crime in Chicago, Illinois. Specifically, I was interested in finding a relationship between weather features and incidents of violent crime. I wanted to focus on violent crime because these types of crimes are heavily motivated by mood. Mood and weather are known to have a correlational relationship, so it was important to evaluate this relationship. Confirming a positive relationship between weather and violent crime would allow for policing and budget allocation recommendations for the city of Chicago. Past efforts have been met with positive results, but in recent years, crime rates have been seen to increase, urging me to build a model that can predict violent crime occurrences. Using these predictions, we can take necessary measures to prevent future crime instances.

## Outline
- [Weather Data EDA](https://github.com/blisspaik/Capstone/blob/master/Code/01_Chicago_Weather_EDA.ipynb)
- [Crime Data EDA](https://github.com/blisspaik/Capstone/blob/master/Code/02_Chicago_Crime_EDA.ipynb)
- [Creating Negative Class](https://github.com/blisspaik/Capstone/blob/master/Code/03_Creating_Negative_Class.ipynb)
- [Feature Engineering](https://github.com/blisspaik/Capstone/blob/master/Code/04_Feature_Engineering.ipynb)
- [Logistic Regression](https://github.com/blisspaik/Capstone/blob/master/Code/05_Logistic_Regression.ipynb)
- [Random Forest](https://github.com/blisspaik/Capstone/blob/master/Code/06_Random_Forest.ipynb)
- [Neural Network](https://github.com/blisspaik/Capstone/blob/master/Code/07_Neural_Network.ipynb)
- [Model Evaluation](https://github.com/blisspaik/Capstone/blob/master/Code/08_Model_Evaluations.ipynb)

## Problem statement

How does weather affect the incident rate of violent crimes? Or, as weather changes, what happens to the rate of violent crimes? If a positive relationship between crime and weather can be found, then we can evaluate which weather features are most indicative of predicting crime. The goal of this project is to leverage time series analysis and machine learning techniques to create yearly predictions for future crime cases.

The python libraries I used were:

- pandas
- numpy
- seaborn
- matplotlib
- datetime
- timedelta
- keras
- warnings
- imblearn
- pickle
- sklearn

Other applications I utilized:

- Tableau
- Google Big Query

## Date Collection

Google Big Query has an API for obtaining metadata from public datasets hosted by this data warehouse. It contains a standardized query structure that allows you to query data directly from the API. You can then download the data as a JSON or csv for easy use. I used this API to query crime and weather data directly into my notebooks. For more information, visit this [page](https://cloud.google.com/bigquery/public-data/).

I collected weather data from years 2014 to 2017 in order to plot trends in weather during these years. I then narrowed my focus to only 2016 and 2017 weather data for modeling. I also acquired crime data for years 2016 and 2017, and specified which types were considered violent using the online crime encyclopedia [Legal Match](https://www.legalmatch.com/law-library/article/what-are-the-different-types-of-crimes.html). The shapes of each dataset can be seen below.

## Size of Data

- 2016 Chicago crime data: 868,071 x 22 (44.3 MB)
- 2017 Chicago crime data: 866,995 x 22 (44.3 MB)
- 2016 Chicago weather data: 366 x 43 (17.7 KB)
- 2017 Chicago weather data: 365 x 43 (17.7 KB)

## Exploratory Data Analysis

### Exploring Data Features

#### [Crime Data EDA](https://github.com/blisspaik/Capstone/blob/master/Code/01_Chicago_Weather_EDA.ipynb)

The crime data contained both locational and time series features. Location features such as latitude, longitude, police district area, police beat area, and block were all highly correlated. I drop all location columns except beat because it is my geospatial feature of interest. By having beat as the indicator of location, I am focusing my predictions on a small police geographical area that has a dedicated police beat car. A Tableau map of beat areas can be see [here](https://github.com/blisspaik/Capstone/blob/master/Visuals/Chicago_beat_areas.png). After dropping all appropriate features and null values, the clean data contained date, type of violent crime (battery, assault, robbery, etc.), whether an arrest was made, and beat label. The date corresponded with the occurrence of crime, so I set it as the index. Using date attributes, I was able to engineer new features for year, month, day, and hour. This was the level of granularity I was aiming for.

#### [Weather Data EDA](https://github.com/blisspaik/Capstone/blob/master/Code/02_Chicago_Crime_EDA.ipynb)

The weather data contained common weather features such as rain, snow, wind, temperature, etc. as well as location features like latitude and longitude. It also had features dealing with time such as year, month, and day. I dropped the weather features that seemed redundant and similar in nature such as wind, windspeed, and gust of wind. In result, I ended up with temperature, wind speed, precipitation, snow, fog, and thunder as my total weather features.

When it came to null values, I did not want to drop any observations because that would mean we are losing valuable daily information. Imputing the nulls with other weather station values seemed appropriate in this case. I used this [map](https://github.com/blisspaik/Capstone/blob/master/Visuals/Chicago_weather_stations.png) containing all weather stations in Illinois as a reference to see which weather station is near Chicago O' Hare International Airport, the origin of my weather data. I found that Northerly Island was the closest station that did not have null values in the days that I needed to impute. After doing so, I had the clean data that I needed to plot.

### Exploring Data Patterns with Visuals

![Histogram](https://github.com/blisspaik/Capstone/blob/master/Visuals/Histograms.png)
These plots display the distributions of temperatures throughout the years of 2016 and 2017. We can see that there are more days with temperatures around 45-50 degrees and 65-70 degrees in 2017, meaning there were generally more colder days in 2017.
![Average rain per month](https://github.com/blisspaik/Capstone/blob/master/Visuals/Average_rain.png)
![Average snow per month](https://github.com/blisspaik/Capstone/blob/master/Visuals/Average_Snow.png)
The first plot demonstrates the average rain per month in 2016 and 2017. There seems to be a seasonal trend for both years. In 2017, however, there was a dramatic increase in precipitation in April and November. Because 2016 had fewer wet days, it could be the fact that precipitation is not a strong predictor of crime occurences in 2017. Nonetheless, we will see whether it truly has any weight when we model.

The second plot displays the average snow per month for both years. We can see that there was generally less snow in 2017. It is interesting to see that there was more rain in 2017, but less snow, specifically ice pellets. Because ice pellets form directly from precipitation, this can mean that 2017 was generally a hotter year. 

![Incidents 2016](https://github.com/blisspaik/Capstone/blob/master/Visuals/Incidents_2016.png)
![Incidents 2017](https://github.com/blisspaik/Capstone/blob/master/Visuals/Incidents_2017.png)
These count plots show counts of crime incidents per month in 2016 and 2017. There seems to be an increase in volent crime incidents during the hotter months, indicating some relationship between weather and crime.

![Count of arrests](https://github.com/blisspaik/Capstone/blob/master/Visuals/Arrest_countplot.png)
In 2016, we can see that there were dramatically low arrest rates for crimes of assault and robbery. Based on this, one goal could be that there is stronger policing for assault and robbery for future years. Other features that should also be crimes of interest for policing is sexual crimes, offenses involving children, and battery. These, however, may be harder to investigate due to the intimate nature of these crimes.

## Preprocessing

#### [Creating Negative Class](https://github.com/blisspaik/Capstone/blob/master/Code/03_Creating_Negative_Class.ipynb)

The goal for modeling is to use 2016 crime and weather data as the training set, and 2017 crime and weather data as the testing set. Before assigning X and y, however, the datasets must be merged so that we could have two datasets that represent both weather and crime features. In order to do so, the level of granularity between crime and weather data had to be matched (weather data does not have hour). Because there was no date column in the original weather data, I had to manually make a hours dataframe that contained every hour for 1 year using timedelta. I was then able to merge weather data with the hours data on year, month, and day so that hour became a permanent feature. Additionally, I wanted to add beat label to the weather data for the purpose of a smoother merge. I was able to do this on the csv level, and directly add to the raw data a beat column. I then read in the altered csv, and was able to merge the crime and weather data on the similar columns. After merging, I was able to see which observations had no crime occurrences based on the NaN's. I replaced these NaN's with 0's, making a negative class.

The size of the merged dataframes were:

- Train: 2,410,013 x 11
- Test: 2,403,351 x 11

#### [Feature Engineering](https://github.com/blisspaik/Capstone/blob/master/Code/04_Feature_Engineering.ipynb)

For feature engineering, I added daylight and nighttime as features using sunrise and sunset. I obtained this data by scraping [TimeandDate](https://www.timeanddate.com/sun/usa/chicago?month=12&year=2017), and read in the data as a csv. I calculated the rolling day averages and sums for relevant weather features to evaluate whether previous weather knowledge will have an effect on predicting crime. Lastly, I bootstrapped using the module SMOTEEN in order to balance my classes. I was then able to assign X_train and y_train using the train data, and X_test and y_test using the test data.

The resulting sizes of the data were:

- Train: 3,468,735 x 25
- Test: 2,403,344 x 25

## Modeling

Based on crime and weather data from 2016, I was able to evaluate which weather features were most informative when predicting crime for 2017. I produced three classification models.

#### [Logistic Regression](https://github.com/blisspaik/Capstone/blob/master/Code/05_Logistic_Regression.ipynb)

Utilizing an easily interpretable model like logistic regression for binary outcomes allows us to make inferences for how a feature impacts the classification probabilities. Additionally, the beta coefficients are easy to interpret compared to other models. I produced a logistic regression model that predicts crime occurrences in Chicago in 2017 based on weather features. I utilized pipeline and grid search in order to optimize my hyperparameters and boost my accuracy score.

#### [Random Forest](https://github.com/blisspaik/Capstone/blob/master/Code/06_Random_Forest.ipynb)

Random Forest controls for the overfitting by fitting multiple trees using a random subset of the feature space. Because it utilizes multiple decision trees, it contains a wide diversity that generally results in a better model. My assumptions were that this would be my best performing model. Additionally, like logistic regression, I implemented pipeline and gridsearch to the modeling process.

#### [Neural Network](https://github.com/blisspaik/Capstone/blob/master/Code/07_Neural_Network.ipynb)

Rather than making assumptions before fitting like other models do, neural networks have an adaptive learning feature that learn how to do tasks based on the data at the initial start. In general, they should have a high accuracy score due to their backpropagation ability that is the powerhouse of learning for this model. I used the neural network Python library Keras when producing this model, and implemented the regularization method Early Stopping to optimize my accuracy score.

## Results

I found that my best model, logistic regression, had 58% accuracy in predicting crime in 2016, and 54% accuracy in predicting crime in 2017. I chose this as my production model due to the low amount of overfitting, and good overall score.

|             	| precision 	| recall 	| f1-score 	| support 	|
|-------------	|-----------	|--------	|----------	|---------	|
| No Crime  	  | 0.97      	| 0.54   	| 0.70     	| 2315357  	|
| Crime      	  | 0.05      	| 0.59   	| 0.09     	|  87987  	|
| Weighted avg 	| 0.94      	| 0.54   	| 0.67     	| 2403344  	|

Based on the above classification report, we can see that model has high specificity but low sensitivity, suggesting that our model is more accurate in predicting crimes to have not occurred than to have occurred.

## Future Steps

Each model shows good progress by beating the baseline accuracy of 3%. The baseline model, before bootstrapping, guesses randomly for each record weighted by the class balance. So, for each crime record, it would predict crime only 3% of the time. The fact that the model can dramatically beat the baseline accuracy suggests high predictive ability, and is a good start to producing a complex model that predicts crime. To further improve my models, the next steps I would take are:

**Increasing computing power** - Due to the limitation on computing power and length of time for model fitting, the accuracy scores could not be improved at this time. With a bigger machine, we can assign a considerable amount of layers to the neural network, and can allow the model to fit for days. This will allow for more learning ability, and would hopefully result in a higher accuracy score. For logistic regression and random forest, with more time, I hope to tune the hyperparameters so that my model could be optimized to the highest degree. This would take a lot of processing power and time, so again, having a bigger machine with more computing power would allow me to do this.

**Using multiclassification models** - At this time, beat label is a proxy for true geospatial analysis. I did not produce any geospatial models, so beat label was not fully used as strong predictor variable for crime. It merely hinted at a geospatial element in the models. For future models, I would like to explore multiclassification so that I could specify a specific beat label for where a crime occurred. Furthermore, I would like to predict the type of violent crime, whether it be battery, assault, kidnapping, etc, and the time that the crime occured at. I would like to combine geospatial elements, time series elements, and types of crime so that the next model would be able to predict where a certain crime occurred at a certain time.

## Recommendations

**Narrow focus to crimes with low arrest rates** - Based on the countplot above regarding arrest rates, we can see which crimes with high crime rates do not result in arrest. These crimes are assault, robbery, sexual assault, offenses involving children, and battery. Using these findings, we can recommend that these crimes are the ones to look out for in the future. This will not be an easy task, however, due to the intimate nature of these crimes.

**Use temperature and hour as predictors of violent crime occurrence** - The incident plots suggest that during summer months, there seems to be an increase in violent crime rate. This could be attributed to the fact that more people are out during these months because it is the summer. If this is the case, then I can use my model's predictions to recommend to the city of Chicago where to allocate police budgets during these months. Furthermore, because temperature was an important predictive feature, we can focus police efforts during the months that correspond to the temperature. We can also direct police efforts based on hour because hour was also an important feature in the models.

**Beat label can be effective in predicting policing patterns** - Using beat label as a predictive feature in the models can be risky. In each model, beat label held great importance and weight when predicting the probability of crime occurring. Specifically, in logistic regression, we saw that as beat label increases, the probability of crime decreases. This can be explained by the fact that as beat label increases (from 100's to 2000's), the more north in Chicago they are located. In the [beat map](https://github.com/blisspaik/Capstone/blob/master/Visuals/Chicago_beat_areas.png), we can see that the darker shades of blue are located primarily in northern Chicago. Northern Chicago could have higher socioeconomic status in comparison to southern Chicago, suggesting that there are more policing efforts in these higher socioeconomic areas. This then goes into the ethics of where policing efforts are directed based on race, gender, etc. because features like these are highly correlated with socioeconomic status. Police predictive biases are already a huge problem in society, so it is important to build crime models in an ethical way, considering the fact that focusing on numbers can dehumanize citizens and criminals alike. Further analysis on ethical considerations can be found on this online [article](https://www.dummies.com/programming/big-data/data-science/problems-data-science-crime-analysis/) written by Lillian Pierson. 
