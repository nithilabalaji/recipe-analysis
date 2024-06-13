# Predicting how long a recipe takes to complete
by Nithila Balaji

# Introduction

This project is part of DSC 80 at UCSD, focusing on analyzing data from Food.com, a popular recipe-sharing website. The dataset includes detailed information about recipes and user interactions such as ratings and reviews. The primary objective is to investigate how the cooking time of a recipe influences user ratings.

Understanding the relationship between cooking time and user ratings is valuable for several reasons:
1. For Home Cooks: Knowing which recipes are more likely to be highly rated can help home cooks make better choices based on the time they have available.
2. For Recipe Developers: Insights from this analysis can guide recipe developers in optimizing their recipes to cater to user preferences, potentially increasing their popularity and user satisfaction.
3. For Food Enthusiasts: It sheds light on broader trends in cooking and food preferences, contributing to the general knowledge about how time investment correlates with culinary satisfaction.

The original dataset has 83782 rows and 13 columns. We will go further into the relevant columns in the next section.

# Data Cleaning and Exploratory Data Analysis

Our dataset is a merge of two dataframes, one containing the original recipes, and another with information on how users interacted with a particular recipe. The interactions gives us information like ratings and reviews.

In order to clean our data the following steps were taken,
1. Fillng all missing values in the rating column with Nan. Ratings range from 0-5, so filling values with 0 adds bias to our data.
2. Grouping data based on recipe ID and taking the average of all ratings per recipe.

We know how a dataframe with a unique row for each recipe. We then add two columns, 'year' and 'calories' using information already in the data to help us later on in the project. 

Here are the relevant columns,
- name: The title of the recipe
- minutes: Minutes to complete recipe
- year: The year recipe was uploaded to the site
- tags: A list of tags associated with the recipe
- n_steps: Number of steps in recipe
- n_ingredients: Number of ingredients required
- average_rating: Average of all ratings on recipe (0-5)
- calories: Reported calories the recipe contains

| name                                 |   minutes |   year | tags                                                                                                                                                                                                                                                                                               |   n_steps |   n_ingredients |   average_rating |   calories |\n|:-------------------------------------|----------:|-------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------------:|-----------------:|-----------:|\n| 1 brownies in the world    best ever |        40 |   2008 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']                                                                        |        10 |               9 |                4 |      138.4 |\n| 1 in canada chocolate chip cookies   |        45 |   2011 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                                                                                                      |        12 |              11 |                5 |      595.1 |\n| 412 broccoli casserole               |        40 |   2008 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                                                                                               |         6 |               9 |                5 |      194.8 |\n| millionaire pound cake               |       120 |   2008 | ['time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'north-american', 'desserts', 'american', 'southern-united-states', 'dinner-party', 'holiday-event', 'cakes', 'dietary', 'christmas', 'thanksgiving', 'low-sodium', 'low-in-something', 'taste-mood', 'sweet', '4-hours-or-less'] |         7 |               7 |                5 |      878.3 |\n| 2000 meatloaf                        |        90 |   2012 | ['time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'potatoes', 'vegetables', '4-hours-or-less', 'meatloaf', 'simply-potatoes2']                                                                                                                                             |        17 |              13 |                5 |      267   |
We know analyze some of the trends in our data.

This histogram shows the distribution of cooking times for recipes, using a logarithmic scale on the x-axis to accommodate the wide range of values. We can see that the values are quite varied with many around 1000 minutes, or 16 hours.

<iframe
  src="uni_1_fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This histogram displays the distribution of average ratings for the recipes. The ratings are concentrated towards the higher end of the scale, suggesting that users generally rate recipes favorably.

<iframe
  src="uni_2_fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The scatter plot illustrates the relationship between cooking time and average rating. The plot shows no clear correlation between cooking time and average rating. The ratings for short recipes vary widely, indicating that factors other than cooking time might play a significant role in how recipes are rated.

INSERT BIVAR 1

The box plot reveals that recipes with shorter cooking times generally receive higher ratings. This trend implies that users might prefer recipes that are quick to prepare, associating shorter cooking times with better overall experiences.

INSERT BIVAR 2

This table presents a summary of the average ratings for recipes grouped by different cooking time intervals. The summary statistics include the mean, median, count, and standard deviation of the ratings within each interval. We can draw some information,
- 0-15 Minutes: Recipes in this interval have the highest average rating (mean = 4.67) and the highest median rating (5.0), with a standard deviation of 0.60. This indicates users generally rate these quick recipes more favorably.
- 120+ Minutes: The longest cooking time interval has the lowest average rating (mean = 4.59) but still a high median rating of 5.0. The standard deviation is 0.68, showing the highest variability among all intervals.

INSERT AGGREGATE TABLE

# Assessment of Missingness

The only missing/null values are found in these columns,
1. name
2. description
3. average_rating
4. cooking_time_interval

### NMAR analysis

If a recipe lacks a description, it might be because the author intentionally left it blank, possibly due to the recipe being straightforward or not requiring additional context. This missingness is inherently related to the value itself (i.e., the presence or absence of descriptive text), which is a classic example of NMAR.

## Missingness Dependency

### Analyzing dependency of average_rating on calories

The objective is to determine if the missingness of average_rating is dependent on the calories of the recipes with a permutation test.

Null Hypothesis: The missingness of average_rating does not depend on the calories of the recipe.

Alternative Hypothesis: The missingness of average_rating does depend on the calories of the recipe.

Test Statistic: The absolute difference in mean calories between recipes with missing average ratings and those without missing average ratings.

We found, 
- Observed Difference of Means: 87.86
- P-value: 0.0

Here is a visualization of our permutations and significance level.

INSERT MISSING 1

Since the p-value is less than the significance level of 0.05, we reject the null hypothesis. This indicates that the missingness of average_rating does depend on the calories of the recipe. There is a significant difference in the average calories between recipes with missing ratings and those with complete ratings.

### Analyzing dependency of average_rating on minutes

The objective is to determine if the missingness of average_rating is dependent on the cooking time (minutes) of the recipes.

Null Hypothesis: The missingness of average_rating does not depend on the cooking time (minutes) of the recipe.

Alternative Hypothesis: The missingness of average_rating does depend on the cooking time (minutes) of the recipe.

We found, 
- Observed Difference of Means: 117.34
- P-value: 0.042

Here is a visualization of our permutations and significance level.

INSERT MISSING 2

Since the p-value is less than the significance level of 0.05, we reject the null hypothesis. This indicates that the missingness of average_rating does depend on the cooking time (minutes) of the recipe. 

# Hypothesis Testing

We now want to investigate the difference in average Rating Between short and long cooking times

Objective: To determine if there is a significant difference in average ratings between recipes with short cooking times (0-30 minutes) and long cooking times (> 30 minutes).

Permutation Test: This non-parametric method does not assume a specific distribution for the data, making it robust for comparing means in our context.

Null Hypothesis: There is no significant difference in average ratings between recipes with short cooking times (0-30 minutes) and long cooking times (> 30 minutes).

Alternative Hypothesis: There is a significant difference in average ratings between recipes with short cooking times (0-30 minutes) and long cooking times (> 30 minutes).

Test Statistic: The difference in mean average ratings between short and long recipes.
- The difference in mean average ratings is a straightforward measure to compare the central tendency of ratings between two groups, making it an appropriate choice for this analysis.

Significance Level: 0.05

INSERT PLOT

Since the p-value is less than the significance level of 0.05, we reject the null hypothesis. This indicates that there is a significant difference in average ratings between recipes with short cooking times (0-30 minutes) and those with long cooking times (> 30 minutes).

# Framing a Prediction Problem

In the previous section, we saw evidence of a relation between cooking time and average rating. We will use this as a base step for out prediction problem. We will try to predict how long a recipe takes to complete using the minutes column, as well as the tags columns. Note that the tags column contains a list of strings, each of which is a unique hashtag. There are over 7000 unique tags in our data.

# Baseline Model

### Understanding the Model

Here is how the model works to forecast the cooking time (minutes) based on the average rating and tags of recipes.

1. The MultiLabelBinarizerWrapper class is a custom transformer designed to handle the tags column, which contains lists of tags for each recipe. The MultiLabelBinarizer from sklearn is used to convert these lists into a binary matrix, where each tag is represented as a separate feature.
2. The data is split into training and testing sets. The features (X) include average_rating and tags, while the target variable (y) is the cooking time in minutes. The train_test_split function allocates 80% of the data for training and 20% for testing, with a random state set for reproducibility.
3. A ColumnTransformer is created to preprocess the data. The average_rating column is passed through as-is, while the tags column is processed using the MultiLabelBinarizerWrapper. The Pipeline integrates this preprocessing step with a LinearRegression model for training.
4. The pipeline is fitted to the training data, which involves preprocessing the features and training the linear regression model on the preprocessed data. Predictions are made on both the training and testing sets to evaluate the model's performance.
5. We finally evaluate the model with mean squared error.

### Analyzing the Model

Here is our result,

Mean Absolute Error on Training Data: 168.75853787827248

Mean Absolute Error on Test Data: 148.85871949708385

The MAE values indicate the average prediction error in minutes. Given that cooking times can vary widely, the MAE values suggest a moderate level of prediction accuracy. The model performs slightly better on the test data than on the training data, which is a positive sign as it suggests that the model is not overfitting.

While the model provides a basic level of predictive capability, the relatively high MAE values suggest that there is room for improvement. Factors such as additional features and more sophisticated modeling techniques (e.g., regularization, ensemble methods) could enhance the model's performance.

# Final Model

We now are able to move onto the final model.

### Improving the Baseline Model

1. The selected features for the model are increased to n_steps (number of steps in the recipe), tags (list of tags associated with the recipe), n_ingredients (number of ingredients), and average_rating. These features are chosen because they provide a comprehensive overview of the recipe's complexity, content, and user feedback, which are likely to influence the cooking time.
2. The data is split, and tags analyzed as before.
3. A RandomForestRegressor is chosen for its ability to handle complex interactions and non-linear relationships. Grid search with cross-validation (GridSearchCV) is used to find the best combination of hyperparameters (n_estimators and max_depth). This method systematically tests different hyperparameter values to optimize model performance.
4. The model is trained using the training data and the best hyperparameters are identified through grid search.
5. We calculate the MAE again to evaluate performance.

Features Added and Their Importance:
- n_steps: Indicates the complexity of the recipe. More steps generally suggest a longer cooking time.
- tags: Provide additional context about the recipe, such as cuisine type or dietary restrictions, which can influence cooking time.
- n_ingredients: Another indicator of complexity. More ingredients can lead to longer preparation and cooking times.
- average_rating: Reflects user satisfaction and potential recipe popularity, which might correlate with cooking time efficiency.

### Evaluating the Model
Here are our findings,

Best Parameters: {'regressor__max_depth': 10, 'regressor__n_estimators': 50}

Mean Absolute Error on Training Data: 71.19

Mean Absolute Error on Test Data: 128.93

The final model shows significant improvement over the baseline model!

# Fairness Analysis

We will now evaluate fairness by comparing its performance on two different groups. 

Group X: Recipes submitted between 2008-2013

Group Y: Recipes submitted between 2014-2018

Null Hypothesis: The model's performance (MAE) for recipes submitted between 2008-2013 is the same as for those submitted between 2014-2018. Any observed difference is due to random chance.

Alternative Hypothesis: The model's performance (MAE) for recipes submitted between 2008-2013 is different from that for those submitted between 2014-2018.

Test Statistic: The difference in MAE between the two groups.

Significance Level: 0.05

We then split the data into the two groups and used our above model on each group. Finally we perform a hypothesis test using the above specifics to compare its results. Here is our output,

MAE for group 1 (2008-2013): 84.48981653540908

MAE for group 2 (2014-2018): 1249.27327608401

Observed difference in MAE: 1164.7834595486008

P-value: 0.4336

Since the p-value is greater than the significance level of 0.05, we fail to reject the null hypothesis. This indicates that the observed difference in MAE between the two groups is not statistically significant and could be due to random chance.

Therefore, based on this analysis, we cannot conclude that the model is unfairly biased against recipes submitted in the more recent time period (2014-2018). However, the large difference in MAE does suggest that there may be underlying issues that are worth further investigation, such as changes in recipe characteristics over time that the model does not account for. After some analysis it was found that the subset of data before 2014 was much larger than the newer recipes. This difference in data size can be one of the many reasons.

