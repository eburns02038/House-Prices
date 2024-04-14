# Overview
For this project I built models for predicting house prices in my city and for greater Central Florida.
The problem that I am attempting to solve is accuracy of house price predictions using more general models. A model built for a particular city may have different features that contribute most to that model compared to another city even if they are in the same state. For example: A model for house prices in New York City would most likely have square footage being of high importance in the model. Contrastingly, a model for a rural area of New York would likely have different features, such as lot size, that most contribute to that model compared to the city. 
Building house price prediction models that are specific to a particular town/city would provide a more accurate prediction for prices in that area than more general models that cover larger regions or entire states. It would also allow for insights into the specific features of a property that would most contribute to property value in that particular area.
My solution would be valuable for real estate professionals as well as individuals. 
Real estate companies could use my model to estimate the sale price of a property in order to better advise clients looking to buy or sell. Having a tool that is able to most accurately estimate sale prices for houses in a specific area would also allow local companies to implement tools similar to those that larger companies use and make them more competitive. 
This model could also provide value to individuals who might be interested in what their current home is worth. For people who may be considering selling their home, this model would allow them to determine the value of their home compared to others in the area. Additionally, this model would provide insight into what the specific features are for that area that most affect the value of a home. This would allow individuals considering making improvements to their home to see which features would most affect their home value, estimate how much their home would be worth following planned renovations, and compare the cost of renovations to the estimated price to determine whether the cost of renovations is worth the anticipated change in value.

# Data Analyzed
To collect the data, I used the Realtor.com Scraper through APIFY (https://console.apify.com/actors/GctCXDim0MXeLZegY/console) to scrape recently sold properties in Orlando, FL and for the greater Central Florida region. The data that I collected included the address, zip code, lot size, square footage, number of bedrooms and bathrooms, garage size, property type (such as single family home or condo), year built, and the last sold price. 

# Project Goals
The research questions that I wanted to address are: 
For the city of Orlando,FL, what are the most important features for a model predicting house prices?
Are more specific local models better at predicting prices than broader regional models? If yes, by how much?
I wanted to investigate the first question because, as I mentioned in my overview, different cities have different criteria that most contribute to price, and determining which features most contribute to the price estimation of a home in Orlando would provide valuable information to the model user for that area.
For the second question, while I do anticipate that the local model will perform better, the part of this question that I wanted to look more closely at is how much better a local model would perform as opposed to a broader regional model, because it would provide a starting point for determining how broad or narrow the range of the model can be, and how specific we can get the model before we stop seeing improvement (for example, would a model for a whole city perform significantly better than a model for a specific zip code or neighborhood) or would it only be off by a few thousand dollars.

# Dataset Cleaning 
When I first looked at the data I found that there were a lot of null values in the dataset. After looking into some of the listings further I discovered that a large majority of the null values were just representing values that would be zero. For example a ‘null’ value for ‘garage’ just meant the house did not have a garage. 
The remaining null values were just in a handful of listings so I dropped the rest. 
Next I wanted to take a closer look at the data and evaluate any outliers or errors. The feature with the most outliers was the last sold price, which had properties listed as high as $15 million, when the mean was only $400,000. This was to be expected, as the vast majority of houses are priced within a range the ‘average’ person can afford, and as the price increases we see fewer and fewer properties listed at that amount because there are much fewer of extremely expensive homes. After looking at the range of values I decided to cap the model at listings at or below $1 million. 
Looking at the outliers for lot size, I noticed that many of the high end outliers were condos, which didn’t make much sense. Looking into it further I discovered that the lot size listed was the lot size for the entire condo complex, which wouldn’t make sense for the model, so I removed these listings which took care of all of the large outliers I was seeing.  
Following these, I addressed just a few listings for beds, baths, and garage that were outliers or appeared to be errors, and was left with much cleaner looking data than before.

# Data Exploration and Feature Engineering
For my data exploration I wanted to start by making scatter plots of how each of the features interacted and if I could see any obvious trends. Only a few variables had noticeable trends, but I did notice that the way the ‘land’ type properties were distributed on the scatter plots would likely throw off the model since most of their features are equal to zero. So I decided to remove the ‘land’ type features from the feature list. 
Next I wanted to look at a correlation matrix for the dataset to get an idea for how the features correlated with each other. The feature that was most highly correlated with the price was the square footage at 84% correlation, and then followed by the number of beds, baths, and garage spaces, which were all 64 or 65%. 
For the other features, zip code was very poorly correlated with price, as was lot square footage and property type. Since these features were much lower correlation, I decided to remove them from the feature list.
The year built feature was somewhat correlated with price, though not very highly. I decided to leave this feature in because I anticipated that this could provide additional input to the model that might not be contributed by the other features. For example, for two houses that are very similar in terms of size, bedrooms, bathrooms, and garage spaces, the house that is newer will most probably be worth more. 


# Modeling Process
For my modeling process, I started by building a basic version of the model, using only the most highly correlated feature. This would act as my baseline, being the simplest version of the model I could make, to compare future models to. 
Then I would build concurrent models by choosing a model type, optimizing parameters using all of the features in my final list, and then removing lowly correlated features one at a time to see when the model’s performance started to decrease. 
I measured the model’s performance using Mean Absolute Error (MAE), which represents the average amount that the model was off in its predictions compared to the true values. 

I decided to use Mean Absolute Error (MAE) as opposed to RMSE because  MAE measures the average magnitude of the errors in a set of predictions, without considering their direction, and all individual differences between true and predicted values have equal weight. For RMSE, since the errors are squared before they are averaged, the RMSE gives a higher weight to larger errors. Looking at the scatter plots I had generated previously and the graph generated by the basic model I saw a general trend of more variation as price increased, and I thought that using RMSE would heavily weight the larger errors in the higher end of the model and significantly increase the error for the model as a whole. 


# Local Basic Model
So starting with the basic model, I used the most highly correlated feature, square footage in a linear regression model to predict price. 
We can see that while most of the lower value properties are more tightly clustered around the red line (indicating perfect predictions), the points become more spread out the more the values increase, which we can see looking at the cone shape between the green lines. This tells us that the model is better at predicting the house prices in the lower range,while it is not as good at predicting house prices in the higher range. 
This model had a very high R-squared value, at 0.95, which sounds really good, but the test statistics came back with higher error values. The MAE score was 72,478.34, which means that on average the model’s predictions were $72,478 off from the true values, which is pretty high. 

# Local Models
For building the different models, I used four different modeling types: 
Lasso Regression
Elastic Net Regression
Gradient Boosted Regression
XG Boost Regression


# Local Model Comparison
Looking at the statistics for the various models that I built, the gradient boosted models had the best performing versions, with the first version of the gradient boosted model that used all of the final features performing the best.
A lower MAE score indicates a better fit of the model to the data, and since this version of the model had the lowest score, we can conclude that it is the best fit for the local model.
This model had a mean absolute error (MAE) of 56,767.34 (meaning that the estimated price would be an average of $56,767.34 off from the true price). This is about $16,000 less error than we saw in the most basic version of the model. 
Initially, an error of $56,000 does not sound great, but when you consider that two houses with the same data for all of the features could have a lot of variation in terms of features that cannot be included in the model, such as renovations and quality of interior finishes (for example: a house with original 30 year old finishes will sell for much less than a house that has brand new floors, kitchen, bathrooms, etc).

# Regional Models
In order to show that a more local model would be more accurate than a broader more generalized model, I also built a series of models using data from all of Central Florida. 
For these models I followed the same procedure as I did for the local models, cleaning the data, evaluating outliers and errors, assessing correlation of the features, and then choosing a list of features to keep to use for building the models. 

# Regional Model Clustering
Before I built the models using the broader regional data, I wanted to look at whether there were enough distinct differences between houses in the different areas within the geographic range I was looking at for the regional models to be able to use the location as a factor when predicting the price. 
So I grouped the listings into different counties based on zip code and then did some dimensionality reduction, to reduce the various listings down to a two-dimensional representation of the data while maintaining as much of the information as I could, and then attempted to fine tune to see if I could get the data to show up as distinct clusters. 
After trying multiple techniques and hyperparameter tuning, I was not able to see any distinct clusters at all, and all of the data seemed to be highly overlapping. What this tells us is that there are not enough significant differences in the features or prices of the houses in this dataset to accurately determine which part of the region that listing is in. This is important because if we were able to see distinct differences between the county groups that would tell us that the real estate market was different enough in each area that a model could hypothetically determine prices for each county in a larger regional model.

# Basic Model
Next, like for the local model I built a basic model, using the highest correlated feature (sqft again). Just by looking at the graph we can see that the predictive accuracy is much lower than it was for the local basic model, as instead of seeing the data predominantly following the red line, we see that it curves off to the side, meaning that at a certain point, the model starts predicting every listing as lower than the true price. 

For the other models, I used the same four model types for building the regional models as I did for the local models. 

# Regional Model Comparison
Looking at the statistics for the various models, the gradient boosted models had the best performing versions, with the second version of the gradient boosted model that used all of the final features performing the best. 
This model had a mean absolute error (MAE) of 101,154.03 (meaning that the estimated price would be an average of $101,154.03 dollars off from the true price. This is very poor, considering that even the worst model for the local models had almost $30,000 less error in the predictions. Even taking into consideration the overall condition and interior finishes of a property, it would be difficult to make a final estimate of price when the range is so large outside of the model estimate.

# Comparing Local and Regional Models 
Based on the above comparison we can see that the local model was much more accurate than the regional model, and was consistently more accurate, as none of the versions of the regional model performed better than even the worst version of the local model. 
We can also see that the amount of error in the local model is reasonable and explainable, while the much larger error in the regional model was too large to make accurate predictions. Like I mentioned before, having an error of $56,000 sounds quite large when you are taking about a scale where the average house is $400,000, but this does not immediately mean that this is a bad model, simply that the user needs to understand that the price estimates are a starting point, and knowing the average absolute error gives the user a range to look at around the estimated price. If the estimated price of a house based on the local model is $350,000, then depending on other features, such as recent renovations or condition of interior finishes, the estimated range for the price of the house would be between $294,000 (for a house that is in poorer condition or has dated interior that will require renovations) and $406,000 dollars (for a newly renovated house with quality interior finishes).

# Conclusions
Revisiting the two research questions that I wanted to address: 
For the city of Orlando, what are the most important features for a model predicting house prices?
For the local model, the most important feature in determining price was the square footage of the home, and other notable features included, number of bedrooms, number of bathrooms, garage size, and year built 
Are more specific local models better at predicting prices than more regional models? If yes, how much?
Comparing the various local and regional models I found that the more specific local model was better at predicting prices than the more general regional model. This makes sense, as I would imagine that the exact level of importance of one feature over another would vary by area, and also economic differences between cities might result in the same house being worth very different prices depending on the exact city that it is in, or even the specific neighborhood it is in.
Some limitations of the model include that it can only be used for certain types of properties. Since the listings for 'lot' type properties would have thrown off the model they were removed, so this model would only work for already built or ready-to-build properties.
Another limitation of the model is that it would likely only work well for listings below 1 million dollars. During data cleaning I removed many of the higher end outliers that were over 1 million dollars (as the grand majority of properties were listed below that line), and I could see on some of the graphs generated that the difference between the estimated price and true price seemed more spread out compared to the lower priced properties. This makes sense, as more and more expensive properties tend to have additional features (such as exact materials used, or custom interior designs) that would affect the price, so this would not be a good model for those properties anyway.

# Future Directions
Going forward, maintaining this model would require regularly checking its performance on new data, and adjusting the model when its performance starts to decline. Since the real estate market changes regularly, and sometimes quickly, checking to make sure the model is able to perform well based on the most recent real estate sales data would ensure that it remains up to date.
Some future directions that I would like to explore with this model include building separate models for different property types to explore how the models would be different (As I imagine that the features used to predict condo prices would be different than the features used for predicting the price of a farm). 
Another area that I would like to explore more is how specific a model I can build before it starts to underperform. Would a model for each zip code work better than the local model? Or even a model for specific neighborhoods or developments? Essentially, how specific would the best performing model be, and are there scenarios where using less specific models would be detrimental?




