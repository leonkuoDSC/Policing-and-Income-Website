# Traffic Policing And Its Relationship With Income

## Introduction

Policing is a rather mixed affair hinging on a number of different factors with some encounters being relatively short and simple while others are more tense and hostile. What most notably shifts these encounters is how the police either rightfully or wrongfully perceives an individual, suspicion of them doing illegal activity, misdemeanors, or having unconscious bias against individuals that appear to fit their mental description of what a criminal may look like. Racial prejudice is one of the more striking factors to point at when considering how or why encounters between individuals and police officers go differently, but part of that could be due to the social class or perceived social class of the individual which could be a compounding factor.

For police traffic stops and searches, the assumption here is that a police officer is much less likely to ticket or search a person they consider to be an upstanding citizen. They would be much more likely to search a lower class person who they wouldn’t regard as highly. In other words police officers would be less likely to suspect a person of drug or other contraband crimes if they perceive the person to be middle or upper class rather than lower class.

## Analysis Based on Service Area Income

The dataset we are using is the San Diego Police Stops Data from the Stanford Open Policing Dataset. This dataset gives us data from 2015 to 2019 for all police traffic stops in San Diego county. For our analysis we will be focusing on the search rate of drivers in order to determine if police are biased in their searches based off the driver's percieved income level.

The first methods we used to to judge a driver's percived income is using the average income of the service area the police stopped them in. This might not be entirely accurate for say drivers who aren't from a service area they get stopped in, but what is important here is the police's perception of the driver's income level. It should give us a general idea of how police might behave differently in different service areas as well.

In order to get the average income of each service area here we used income data from the 2010 US census. Then we geospacially joined the service areas and the census tracts and got the average of all census tracts within each service area to get the average income of each service area.

(Image of Search By Service Area Graph)

Here we have each Service Area represented as a bubble and we can see an inverse relationship between the average income of the service area and the rate that drivers are searched. 

There may be two outliers below the curve at around 70k, but looking at it geospacially it can be more easily explained.

(geospacial plots)

In these geospacial plots, we can see the service areas with the lowest average income near the bottom right of San Diego county. And we see the 5 areas with the highest number of searches also in that area clustered together. 

The two outliers from before are the ones at the border of San Diego county, where different types of policing may take place for drivers crossing the border.

This could also point to those 5 clusered Service Areas being more heavily profiled.

## Veil of Darkness by Service Area Income

Using service area income like in the last section, a technique known as the veil of darkness will be employed.

This technique limits the range of searches to all searches from an hour before twilight to an hour after twilight. The thought here is that police cannot as easily racially profile and stop minority drivers after twilight since they would not be able to see the color of the driver's skin. 

By adding the income of the service areas, we may be able to see a pattern emerge in where this racial profiling is taking place. Naturally since lower income service areas have larger populations of African American and Hispanic residents, we'd expect them to have higher amounts of profiling. But what we also want to look at is whether or not racial profiling is also notable higher income neighborhoods.

Another thing we can look at here is how different service areas may be treated after dark, where police might deem a car more suspicious for driving after twilight.

(Stop Rate Change Graph)

This graph shows the percent change from the hour before twilight and after twilight. The orange line there is at 0 percent and what would happen if there were 0 change between before and after twilight.

What we see here is that lower income service area get stopped notably more after twilight, while higher income service areas get stopped less often.

