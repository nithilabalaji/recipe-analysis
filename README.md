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



# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis
