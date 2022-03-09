# Traffic Policing And Its Relationship With Income

## Introduction

Policing is a rather mixed affair hinging on a number of different factors with some encounters being relatively short and simple while others are more tense and hostile. There are many different reasons for why police make the decisions that they make, either rightfully or wrongfully, many of which aren't directly observable or easily determined within police data. Some of these factors may include suspicion of drivers doing illegal activity, misdemeanors, or having unconscious bias against individuals that appear to fit their mental description of what a criminal may look like. While racial prejudice is one of the more striking factors to point at when considering how or why encounters between individuals and police officers go differently, decisions seemly motivated by racial bias may potentially be actually due to social class or perceived social class of the individual rather than their race. 

For police traffic stops and searches, the assumption here is that a police officer is much less likely to ticket or search a person they consider to be an upstanding citizen. They would be much more likely to search a lower class person who they wouldn’t regard as highly. In other words police officers would be less likely to suspect a person of drug or other contraband crimes if they perceive the person to be middle or upper class rather than lower class.

## Analysis Based on Service Area Income

The dataset we are using is the San Diego Police Stops Data from the Stanford Open Policing Dataset. This dataset gives us data from 2015 to 2019 for all police traffic stops in San Diego county. The county is split geographically into a number of smaller service areas where officers may be stationed to. For our analysis we will be focusing on the search rate of drivers in order to determine if police are biased in their searches based off the driver's percieved income level. 

The first methods we used to to judge a driver's percived income is using the average income of the service area the police stopped them in. This might not be entirely accurate for say drivers who aren't from a service area they get stopped in, but what is important here is the police's perception of the driver's income level. It should give us a general idea of how police might behave differently in different service areas as well.

In order to get average income of service areas, we used census data of San Diego County which contained information on average income for each geographical census block. Then we joined the correct census blocks that fit or were a part of a service area and took the average from those census blocks for each service area. This is still a rough estimation given how large service areas can be compared to individual census blocks, but overall should be close to the actual average.

![image tooltip here](/images/Searched_By_Service_Area.png)

Here we have each Service Area represented as a bubble and we can see that police searched lower income service areas at a higher rate and that the higher a service area's income the less likely that drivers will be searched. One potential reason is that police treat lower income service areas differently and believe that they have a higher chance of having contraband and thus search drivers there more often. Or maybe the police don't expect higher income areas to have drivers with contraband.

There may be two outliers below the curve at around 70k, but looking at it geospacially it can be more easily explained later.

![image tooltip here](/images/Geospacial_Income_and_Search.png)

In these geospacial plots, we can see the service areas with the lowest average income near the bottom right of San Diego county. And we see the 5 areas with the highest number of searches also in that area clustered together. 

The two outliers from before are the ones at the border of San Diego county, where different types of policing may take place for drivers crossing the border.

This could also point to those 5 clusered Service Areas being more heavily profiled.

![image tooltip here](/images/Arrests_After_Search.png)

The graph above is the percent of searched drivers that were arrested by service area income. You see that the percent of searched people arrested actually goes up at higher income, so they are searched more and aren't arrested as often afterwards. This gives more evidence the lower income people are getting searched much more than they should be.

## Veil of Darkness by Service Area Income

Using service area income like in the last section, a technique known as the veil of darkness will be employed.

This technique limits the range of searches to all searches from an hour before twilight to an hour after twilight. After twiight, police cannot as easily racially profile and stop minority drivers since they would not be able to see the color of the driver's skin. By doing this, we set up a treatment and control group with the treatment being the level of visibility where one group of police officers could racially profile drivers and the other group wouldn't be as able to. By limiting it to an hour before twilight to an hour after twilight, we are trying to hold other factors constant such as what drivers may be out on the road.

By adding the income of the service areas, we may be able to see a pattern emerge in where this racial profiling is taking place. Different income service areas may have larger populations of African American and Hispanic residents, so racial profiling may be easier for police despite the constraint so we may see less of a difference in those service areas. For this analysis, it made sense to divide the data into three income brackets, and it was settled to be under 80k, 80k to 110k and over 110k. This gets us a low, middle and high grouping where each has a similar amount of service areas.

![image tooltip here](/images/Stops_by_Race_Low_Income.png) ![image tooltip here](/images/Stops_by_Race_Med_Income.png) ![image tooltip here](/images/Stops_by_Race_High_Income.png)

After grouping the service areas into three categories based on income for low, medium and high income, there seems to be no statistically siginificant evidence of racial profiling shown through the veil of darkness technique.

However seeing that there is still a notable change in stop rate before and after twilight, the lower income service areas may be more systematically or indirectly targeted based on the average income of the service area.

## Car Price as a Proxy for Driver Wealth

In order to further our analysis on police stop data, we wanted to explore how police perceive a driver’s wealth, and how that could influence their decisions to stop, search or arrest different drivers. From the Stanford Open Police Project, we found a dataset that includes stop data from San Antonio, Texas, with the make, model and year of the car stopped. I wanted to join this onto a dataset to get the price of the car that was stopped. We found a Kelly Blue Book (KBB) dataset that we could join onto to get car prices. However, several potential problems soon arose: the Texas data abbreviated the make and model of the car. For example, a 2015 Toyota Tundra would be 2015 TOY TUND, whereas KBB uses full names. After some consideration, I elected to go with the KBB data, as it was more complete and included more makes and models of cars. The Texas data includes over 20,000 unique year, make and model cars, and the KBB data provides price data for 17,651 unique year make and model cars. I first tried to use difflib to find closest matches directly between these two datasets, but the code was too inefficient and would take many hours to run. Difflib.get_close_matches() returns a “close enough” match based on the number of similarities it can find between a string and a list of comparison strings.

To work around the large datasets and runtime of get_close_matches, I first made a dictionary that matches the car make as listed in the texas dataset with the car makes in the KBB dataset by hand. The resulting dictionary had 44 matches. I then made an empty dictionary to hold the price of each make and model. Then, for each car in the Kelly Blue Book data, I found the closest match in the texas data to the full car name, (year, make, model), then set that closest match to be a key in the empty dictionary. I then set the value for that key to be the price from the KBB data. I converted that dictionary into a dataset, cleaned it and gave it column names, then merged it onto the texas dataset to get a dataset that has 65217 rows. Overall, the quality of the join is decent, but leaves much to be desired. After cleaning the Texas dataset, it had 873,113 rows with car data in them, but the join reduced that to just 65,217 rows, a reduction of 92%. Even though there is still plenty of data to be analyzed, it shows that this join method is far from optimal, and is something that can be improved upon in the future. A working hypothesis on why so much data could not be matched is due to a mismatch between KBB and Texas datasets, with KBB not having some cars mentioned in the Texas Dataset. It could also be due to the messy nature of the Texas Dataset, with some cars having wildly incorrect ages, making it impossible to find a similar match with KBB data.

After obtaining a joined dataset that includes the police traffic stops and the price of the car stopped, I binned the prices of the cars into three categories to see how search and arrest rates would differ between them. Cheap cars are less than $10,000, medium cars are between $10,000 and $35,000, and expensive cars are anything more expensive than $35,000. What I found was that cheap cars had much higher arrest and search rates when compared to expensive and medium priced cars. Cheap cars had an arrest rate of 0.237%, and a search rate of 0.77%, when compared to expensive and medium priced cars, which had arrest rates between 0.11% and 0.13%, and search rates between 0.26 and 0.276%, a significant decrease in rates that is indicative of some bias in policing towards cheaper cars.
To dive deeper into the correlation between driver wealth and police action, I looked into any potential correlations between the average car price in each district and the search and arrest rates in each district.

![image tooltip here](/images/Price_vs_Search.png) ![image tooltip here](/images/Price_vs_Arrest.png)

After plotting these variables, I found strong negative correlations between them, showing that as car price increases, the search rate decreases.

![image tooltip here](/images/Income_vs_Prices.png)

We can see here in the graph above that as the average price of a car and the average income of a district increases, we see the search rate begin to shrink. To further investigate this correlation, I ran a logistic regression on these variables: car price, car age, income of the area the car was stopped, and the product of the car price and income of the area to predict the search and arrest rates.

![image tooltip here](/images/logistic_search_price_age_income.png) ![image tooltip here](/images/logistic_arrest_price_age_income.png)

We can see that in both models, each coefficient is small and close to zero, showing that of the four variables, age actually has the most impact in the prediction that the model would make.

In order to compare how drivers were being treated based on their cars, we looked at the cumulative distribution function, or CDF, of the prices of cars that were searched and the prices of all cars that were stopped. The cumulative distribution function by car price will let us see what proportion of cars in the data are less than or equal to any given price in the data. So for example if 50 percent of cars were under 15k in price, then that would give us a y-value of .5 and an x value of 15k. So not only will this give us the general price distribution of all cars in the populations, but it also lets us see how well the searched and stopped distributions match. 

![image tooltip here](/images/car_CDF.png)

Looking at the Search and Stopped CDF by difference value, we see that both hispanic and white drivers match up closely with the overall CDF difference, but Black drivers have a sudden dip around 10000 and overall have less of a search and stop CDF difference. Part of this can be explained by the fact that black drivers in this data have cheaper cars overall, or may be due to the fact there aren't that black drivers compared to the other driving populations and we have an unfortunate lack of data.

The general bias towards searching cars that are of a lower price is still mostly prevalent across the groups of drivers.

![image tooltip here](/images/car_diff_CDF.png)

Similar to Veil of Darkness using Service Areas, we can use veil of darkness while grouping cars of different price ranges in order to control for the effect of car price on racial profiling.

Unfortunately, the downside to using the veil of darkness here is that there isn't quite enough data in the dataset to come to any strong conclusions. The data was divided into three groups, under 10k, 10k to 20k, and above 20k.

![image tooltip here](/images/Stops_by_Race_cheap_car.png) ![image tooltip here](/images/Stops_by_Race_mean_car.png) ![image tooltip here](/images/Stops_by_Race_expensive_car.png)

Based on the data we have still, there isn't any statistically significant evidence for racial profiling.

# Feedback Loop Simulation

When we are preparing the Stanford Open Policing Data, we try to rule out columns that we think are not features that a police officer can come up with at the time of the stop if they wish to use our model. We decided to stick with 9 features, all of which are reasonable in that a police officer is able to pull up all the information necessary to input into the model. For example, we have the service area, a police officer should know which area they are currently assigned to, we have race/sex/age that can be pulled up from the license plate (only correct if the person driving is who the car is registered to), and day of the week.

While cleaning and preparing our data, we are also trying to make our label/prediction much better. For example if our initial data had been a search but there was no contraband found then in our newly created label we would have a 0 meaning there should not have been a search in the first place.

![image tooltip here](/images/MODEL.png)

So how our model works is that it takes in the first n months (lets use 3 in this example) and trains the model on those first 3 months of the dataset. Then we take the next 3 months (Months 4-6) and predict on that. When we have those predictions we then replace the actual labels of Months 4-6 with the predicted labels and we refit the model with Months 4-6 and the predicted labels and then we take the 3 months after (Months 7-9) and do the same until we reach the last months of the dataset. We evaluate our model using recall and precision, as well as accuracy. The current Classifier we are using is LDA, but we will keep trying out different models to see which works best. When we get predictions, we do some modification to get better Y’s. If the model predicted 1, and it is actually 0, we change the label to a 0. If the model predicted 1 and it is actually 1, we keep the label of 1. If the model predicts 0 and it's actually 0, we keep the label of 0. Finally, if the model predicts 0 and it is actually 1, then we flip the label to 0.

We see that our model is predicting to search more white people than black people regardless of having race as a feature or not while training the model. We believe that this is happening because only ~19% of the data we have is black drivers. It is also predicting searches mostly in service area 120.

![image tooltip here](/images/Overall_Precision_Recall.png) ![image tooltip here](/images/Precision.png) ![image tooltip here](/images/Recall.png)

The 3 graphs we have here are for when we give the model race as one of the features as well as only have it take in 3 months at a time. Between recall and precision, we see they kind of have a similar pattern, they are 0 for 3 iterations towards the beginning and then they both end up at 0 for both black and white drivers. The precision/recall overall is low toward the early iterations and then it gradually goes up in the later iterations.

If this model were to be deployed, we would expect police officers to use it if they had someone stopped and were trying to figure out if they should search them or not. The inputs to the model would all be available to the officer performing the stop, therefore the model should be able to give a good prediction of whether or not the officer should search or not.

