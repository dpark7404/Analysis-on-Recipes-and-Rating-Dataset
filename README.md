# Analysis on Recipes and Rating Dataset
Project for DSC80 at UCSD

Authors: Daniel Park, Tracy Pham

## Project Overview:

   In this project, we aim to investigate the relationship between the season in which recipes are submitted and the nutritional density, specifically focusing on calorie content. This analysis can provide insights into how dietary preferences and nutritional choices vary with the changing seasons.

### Project Introduction:

With the changing seasons follows a new set of recipes that people like to associate with and cook. People's dietary routines and preferences 

### Research Question: 
What is the relationship between the season in which recipes are submitted and calorie content ? 

## Dataset Explanation:

We used two datasets for this research question. The RAW_recipes and RAW_interactions datsets. We initially merged these two

## Cleaning and EDA:
### Cleaning

We took a couple steps to make our data more readable and relevant to our research question and topic. 

1. First we left merged the recipes and interaction datasets together and filled all rating of 0 with np.nan. This was done in response to possible reviews being submitted with no ratings. These get inputted as zero's into the dataset but they are false zeros. So we remove them and replace with np.nans for a more true resemblance of peoples ratings.

2. We then found the average rating per recipe and added this back to the recipes dataset. 

3. Then we changed all columns with non-list type variables into lists (useful for our specific data needs) A 'month' column was created that contained integer values corresponding to what month the recipe was submitted. We used mapping to determine what season each month would be categorized into. Seasons were categorized as [(Winter: December, January, February), (Spring: March, April, May), (Summer: June, July, August), (Fall: September, October, November)]

4. We also created a 'calories' and 'cal' columns that contained the calorie values taken from the 'nutrition' column as string and float types, respectively. The nutrition column contained a lists that with each number in the lists corresponding to ['calories', etc, etc] in that particular order.

5. We thought there may be certain anomalies in the data so we decided to remove outliers. The numbers we used were .95 for upper quartiles and .05 for lower quartiles. However, we only removed the values that were greater than the upper quartile. The reasoning is because smaller calorie foods are more relevant to our data because there are many foods that can fall within smaller calorie counts, however our data has some anomalies with recipes that have more than 40,000 calories. This throws off our mean and max in following analyses performed so removing only the higher bounds seemed fine.

After taking these steps, the cleaned and more readable dataframe is as shown below:

- insert df embed here - 

### Univariate Analysis

We were most interested in the 'month', 'seasons', and 'calories' columns for this analysis

1. Here is our first analysis as a bar graph that plots 'index' on the x and 'month' on the y. This shows the distribution of recipes that were submitted each month. In this graph we can see that there is a general decrease in the amount of recipes submitted each month.

2. Here is our second analysis as another bar graph that plots 'index' on the x and 'seasons' on the y. This show the distribution of recipes that were submitted each season (categorized above in our cleaning steps). Looking at this graph we can see that Spring and Winter have the most submitted recipes.

3. Our third analysis is a histogram that plots the distribution of calories across all unique recipes. From the data we can state that generally higher and lower calorie recipes have slightly fewer counts of recipes. There is visible variation in the distribution as well. 

### Bivariate Analysis

For our bivariate analyses it should be noted that we created a new column called 'calories_category' that uses bins to categorize all calories in amounts of 300 [(0,300], [300, 600], [600,900], [900, inf)] and a new dataframe called count_season_calories_df that is used in our plot.

1. Our first analysis uses a scatter plot that takes in 'calories_category' and index name 'recipe_count'. From the resulting plot we can see a clear variation among the seasons and calori categories. Fall seems to be the most consistently lowest among the four while winter seems to be consistently the highest. 

2. Our second analysis uses a violin plot that takes in 'season' and 'calories'. The graph shows that Summer seems to be the most uniform in comparison to the other seasons. While Winter has a more erratic structure. Fall and Spring seem to be the closest in terms of seasons vs calories

### Interesting Aggregates

Our visual examples show interesting trends and plots so far, but we want different reliable analyses that can show the relationship between other columns of our data set and our created columns.

1. Here is pivot table one where we show the relationship between calories and season. In this one we analyze the count, mean, median, min, max, and std of calories. From the data we can infer that Fall and Winter are the seasons with the highest mean calories while Spring and Summer are both less than them which corroborates with our above plots. 

2. Here is pivot table two where we show the relationship between avg_rating and month. We analyze count, mean, median, min, max, and std of avg_rating for each month. Looking at the means we can see that they are mostly uniform with a different of less than .1 at max.  

3. Here is pivot table three where we show the relationship between n_ingredients and calories_category. We analyze count, mean, median, min, max, and std of n_ingredients. We can infer that with the increments of calorie_category the mean of n_ingredients goes up as well. Which shows a positive trend in these two categories when analyzed.

## Missingness Assessment
### NMAR Analysis

We believe the row 'review' with missing values to be the NMAR in our dataset. The reason for this is possibly because people who submit reviews may feel uninclined to leave reviews on recipes that they have no sort of connection to. 

### Missingness Dependency

We decided to test rating against two different column names for our permutation test to see if there are observable differences. 

1. Testing 'rating' and 'minutes'

After running the permutation test on these columns we get an observed difference in means of 51.507230759 and a p-value of .121, and can conclude that the missingness of 'rating' is most likely not dependent on 'minutes'

2. Testing 'rating' and 'n_steps'

After running the permutation test on these columns we get an observed difference in means of 1.331260648 and a p-value of 0.0, and can conclude that the missingness of 'rating' is most likely dependent on 'n_steps'

## Hypothesis Testing

We now circle back to our original topic and research question at hand. 

Null Hypothesis (H0): There is no significant difference in the mean calorie content among recipes submitted in different seasons.

Alternative Hypothesis (H1): There is a significant difference in the mean calorie content among recipes submitted in different seasons. 