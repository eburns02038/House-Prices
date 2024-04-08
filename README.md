# House-Prices
House Price Prediction Model

Project goals: 
1) Build a model using local data for recently sold properties in Orlando, FL
2) Build a model using regional data for recently sold properties in Central Florida
3) Compare models to examine performance of regional vs local models on predicting property prices


Initial modeling: 
- Basic model that is the simplest possible version of the model that can be used as a baseline to compare future models to
- - Simple linear regression using just sqft of property to predict lastSoldPrice
    
Completed local and regional models. 

# Local vs Regional Model Comparison
The mean absolute error can be interpreted as the the mean difference in dollars between the predicted prices and the actual prices.

For the local model, the basic version that we made using just linear regression and the most highly correlated variable had an MAe score of 72,478.34, meaning that the estimated prices would be on average 72,400 dollars away from the actual price. The best model we were able to make for the local model had an MAE score of 60,305.76, which is about 12,000 lower than the basic model.

For the regional model, the basic version had an MAE value of 110,364.58, and the best version of the model had an MAE score of 101,159.58.

Comparing the two model's the local model was able to predict house prices with an average error of about 60,300 dollars, while the best version of the regional model was able to predict house prices with an average error of 101,100 dollars. The local model has a lower average error by about 40,000 dollars.

# Results and Discussion
Based on the above comparison we can see that the local model was much more accurate than the regional model, which makes sense as houses in the same area are likely more similar in features and price than houses in different areas. This is why it is important for house prediction models to use more local data to make predictions, compared to wider more general regional data.

Looking at the local model, the best version had an average error of about 60,300 dollars, which doesn't sound great when you consider the average house in the area of interest being worth about 400,000 dollars. That doesn't mean that this model is bad, just that the user needs to understand that the price estimates are a starting point, and knowing the average sbsolute error gives the user a range to looks at around the estimated price. If the estimated price of a house based on the model is 350,000 dollars, then depending on other features, such as recent rennovations or condition of interior finishes, the estimated range for the price of the house would be between 290,000 dollars (for a house that is in poorer condition or has dated interior that will require rennovations) and 410,000 dollars (for a newly rennovated house with quality interior finishes).

Some limitations of the model include that it can only be used for certain types of properties. Since the listings for 'lot' type properties would have thrown off the model they were removed, so this model would only work for already built or ready-to-build properties.

Another limitation of the model is that it would likely only work well for listings below 1 million dollars. During data cleaning I removed many of the higher end outliers that were over 1 million dollars (as the grand majority of properties were listed below that line), and I could see on some of the graphs generated that the difference between the estimated price and true price seemed more spread out compared to the lower priced properties. This makes sense, as more and more expensive properties have very specific features (such as materials used, or custom interior designs) that would affect the price, so this would not be a good model for those properties anyway.

Some future directions that I would like to explore with this model include building separate models for different property types to explore how the models would be different (As I imagine that the features used to predict condo prices would be different than the features used for predicting the price of a farm). Another area that I would like to explore more is how specific a model I can build before it starts to underperform. Would a model for each zip code work better than the local model? Or even a model for specific neighborhoods or developments?
