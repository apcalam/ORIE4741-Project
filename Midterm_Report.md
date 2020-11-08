# In the report, you should describe your data set in greater detail. 
	
Our project analyzes the dataset from the US Department of Transportation’s Domestic Airline Consumer Airfare Report from 2019. This dataset contains 213175 rows and 23 columns describing information for airport pair markets. Each row contains an origin airport and city, destination airport and city, year, time of year (quarter), average fare, average fare for the carrier with the largest market share, average fare for the lowest carrier, number of miles, passengers per day, and geocoded information. 

After cleaning the data, our dataset has 201392 rows and 24 columns. Of the 213175 rows, 201392 has complete data without any missing values and we do not have any corrupted values. The 24 columns we use come from adding a ‘when’ column that combines the columns year with the financial quarter via the following formula: when = year + (quarter - 1)/4. 

# Include a few histograms or other descriptive statistics about the data. How many features and examples are present? How much data is missing or corrupted? How can you tell? 

![Figure 1](/images/midterm/figure1.png)
Figure 1:  Histogram of average fares. Each data point is an average fare across all flights from a given city to another, in one quarter of a given year, and the histogram shows the frequency of different average fares. Moreover, the average fare statistic has mean 217.7, median 208.4, and standard deviation 82.6. 

![Figure 3](/images/midterm/figure3.png)
Figure 2: Density of the large_ms feature in the dataset, which for each example represents the “Market share for the carrier with the largest market share” for the given trip in the given year and quarter. This statistic has mean 0.658, median 0.650, and standard deviation 0.225. This shows how a large fraction of trips in the dataset receive a majority of their service from a single airline. We hope to use this information to consider the stability of our price predictions, intuiting that if a single airline controls a particular trip, that trip's price may be more stable over time compared to a trip that is serviced by many different airlines. 

![Figure 3](/images/midterm/figure3.png)
Figure 3: For a fixed source and destination airports (JFK and SMF, respectively), we visualized the average fare flight over time. Doing this analysis for various airports is important in this stage of the project, to get a sense for what type of models we expect to fit our data well. 

# Describe how you plan to avoid over (and under-)fitting

We plan to avoid overfitting by performing k-fold cross validation on our data to insure that our model won’t overfit to the training set. By doing so, we can allow for generalization because we will be testing our model on several different combinations of the full data rather than a single sample. Additionally, we plan on using regularization methods such as ridge, lasso, or non-negative to prevent overfitting. These methods help to either reduce the number of features or reduce their effects down to near zero. 

Unfortunately, we cannot completely generalize our model because our data is only from the years 1993-2019. We are limited in the scope of our data because this doesn’t account for an anomaly year for travel such as 2020, where we experienced an unprecedented number of travel restrictions due to COVID-19. Any predictions our model makes will assume that we are in a normal year for travel, and will likely not be robust enough to outliers like the year 2020. However, we should be able to generalize our model to be able to predict prices in a normal year of travel. 

We plan on avoiding underfitting by building more complex models than linear regression, such as using higher degree polynomials. More complex models reduce generalization because they are less rigid compared to simple models. As of now, we are just doing linear regression, but we will try out other models in the future. 
 
# How you will test the effectiveness of the models you develop

We will test the effectiveness of the models we develop with k-fold cross validation and bootstrapping. Both of these methods of testing will help us create a more generalized model and will help us prevent overfitting. 

# You should also run a few preliminary analyses on the data, including perhaps some regressions or other supervised models, describing how you chose which features (and transformations) to use. 

The first preliminary analysis we ran was a simple linear model to predict fare given a source airport, a destination airport, and when the flight is. The when feature is not directly provided in the data, but is derived from the year and quarter features (which are included) via the following formula: when = year + (quarter - 1)/4. We used the Julia backslash operator to solve the least squares problem to predict the fare given these features and a bias, using 80% of the dataset for training and holding out 20% of the data for validation. 

In this experiment, we observed a MSE of 82.0 on the training set, and 81.1 on the held out validation set. This indicates that this simple linear model is not overfitting, and we can add complexity to our model in the next round of modeling. Further, this is an encouraging result because this error is about the same as the standard deviation of the average fare statistic, as described above. 

# Finally, explain what remains to be done, and how you plan to develop the project over the rest of the semester.

Our current analysis uses a linear model to create predictions. In the future, we will experiment with more complex models, including higher degree polynomial models to increase the flexibility of our predictions. Moreover, we will also employ regularization techniques to mitigate overfitting and make predictions more robust, and try different loss functions to see which is most useful for modeling our data. 

Next, we will improve how our model represents the data: our model currently encodes each airport (a nominal feature) as a numerical value, which could skew the model to draw similar predictions for airports with numerical values that are closer in proximity, when in reality their encoding do not have any relation to each other. In the future, we want to employ methods such as one-hot encoding to make the data nominal and categorically separated. Moreover, we want to try using latitude and longitude data for airports to represent flight end points in a way that relates nearby locations, rather than just using nominal values. Further, we will experiment with additional feature engineering and more robust ways of finding and handling outliers. 
