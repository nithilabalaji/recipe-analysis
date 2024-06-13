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
- id: Unique number assigned to each recipe
- minutes: Minutes to complete recipe
- year: The year recipe was uploaded to the site
- tags: A list of tags associated with the recipe
- n_steps: Number of steps in recipe
- n_ingredients: Number of ingredients required
- average_rating: Average of all ratings on recipe (0-5)
- calories: Reported calories the recipe contains

INSERT DF HEAD

We know analyze some of the trends in our data.

This histogram shows the distribution of cooking times for recipes, using a logarithmic scale on the x-axis to accommodate the wide range of values. We can see that the values are quite varied with many around 1000 minutes, or 16 hours.

INSERT UNIV 1

This histogram displays the distribution of average ratings for the recipes. The ratings are concentrated towards the higher end of the scale, suggesting that users generally rate recipes favorably.

INSERT UNIV 2

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

# Final Model

# Fairness Analysis
