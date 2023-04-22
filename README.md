# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
The goal of this project was to pull data from three separate APIs, link them together into a single dataset, perform EDA and cleaning on the dataset and then to run some simple regression on the variables.

## Process
Before anything involving APIs I did have to land on a city, the first few cities I pulled had no bike information (0 bikes available at all stops) so I settled on Chicago as a relatively large city with plenty of bike data)

The first step was learning how to pull data from each API. This involved making a sample pull and then working out where and how each variable of interest was stored. From there I could build a loop that would go through and pull results based on criteria (e.g. location of each stop). 

I then pulled data for each stop and created a dataframe from those separately (one for yelp and one for foursquare).

Next, I had to combine those two dataframes into a single dataframe. That took a bit of cleaning to ensure all variables were named similarly. At this stage I compared the two location APIs to determine what their strengths and weaknesses were by comparing how many pulls they made per location and what sort of info they provided.

Using this dataframe I was able to create a new dataframe grouped by bikestop with counts for POI and averages for ratings. This allowed me to run a regression on it. However, before doing that I took a look at some charts to get a feel for the data and make some predictions of how the regression might come out.

In order to prepare for any future analysis I also created a SQL database of the dataframes (bikestops & bikestop info, locations & location info, locations per bikestop & ratings) so that if I needed any information in the future it was all there. This also involved backchecking to ensure everything was pulled in correctly.

My last step was to run my analysis. I performed a backwards stepwise regression arriving at a final model detailed below. Posthoc testing did reveal that it did not meet assumptions of a regression. I did attempt a log transformation of freebikes, but this was unsuccessful in meeting assumptions (detailed in model_building.ipynb)

Each of these steps is detailed further in the notebooks contained in the notebooks folder:
1. citibikes outlines how I pulled the bike data
2. yelp_foursquare_EDA outlines how I pulled from those APIs and also combined / performed EDA on the output
3. joining_data details how I joined the data into a single dataframe and also created a sql database
4. model_building details how I built my model and tested it



## Results
In my area Yelp seemed to have better coverage than Foursquare. As I did not offset the data nor do multiple pulls per area its possible this isn't the case, but the foursquare data had many more holes in it than did Yelp, so I'd lean towards that even with more data.

The model produced a small r^2 and did not meet requirements for homoskedasticity or normal distribution of the residuals. It did pass a VIF test however.

All in all the model isn't super useful, but can be interpreted as follows:

All below interpreted only with all other variables being held equal: longitude: Each degree of longitude adds about 4 bikes available indicating that the further west you are the less likely you are to find a bike available review count: Each review for a location had a very small negative impact on bike availability indicating the more well traveled an area (or more reviewed at least) the less likely to find a bike Bar & POI had a slightly larger impact each on number of bikes. The more locations there were the more likely to find a bike Restaurants had a smaller impact per restaurant, but there were more restaurants overall so in the same bucket as Bar & POI

Overall, some work to be done to make this a worthwhile model. Next steps would be to grab bike data every 4-5 hours over a two week period and then take an average number of bikes available (or even availability at different times). This model is also built off Chicago data pulled in April and it's quite cold there so not as many people using bikes likely.

Another issue with the model is it's based off only ~100 POIs per location and many locations likely have many more places near them than that. It would probably be possible to sample multiple times from the same API and make a composite, but this would push me over the my monthly requests so I opted not to.

<img src="\images\test.png" alt="Test Stats" />

Results for tests can be found in notebooks/model_building.ipynb

## Challenges 
One of the main challenges in this was making sure to set everything up correctly so that when it came time to combine it everything was named similarly and fit together nicely. There's a lot of thought that goes in on the front end to ensure you don't mess it up later. 

Another challenge was making sure you were pulling in the right data. At first I didn't think Foursquare had ratings, but it simply isn't a default output of the API so you have to specify it. An API will only give you what you ask for.

Lastly, there were some challenges with getting SQL to work in Python as I haven't done that much before, but in the end it was a good learning experience.

## Future Goals
IF I had more time I would work on getting an offset to work to be able to pull more poi data for each stop. I would also set up a pull every 4-5 hours over a two-three week period for every bike stop to get more complete data instead of relying on a single point in time. This would hopefully result in better distribution of the number of bikes.

It would also be good to pull data in the summer. It's quite cold in Chicago right now so bike use is probably off.
