# Analysis on Recipes and Rating Dataset
Project for DSC80 at UCSD

Authors: Daniel Park, Tracy Pham

## Project Introduction:
   Our project delves into a dataset from food.com, spanning recipes and ratings since 2008. Our mission is to uncover key patterns and trends about food and people's consumption of food through our dataset about recipes and their ratings. The dataset, divided into recipes and ratings, provides a comprehensive view of contributors, preparation details, and nutritional information. Our methodical approach involves thorough data cleaning and exploratory analysis, revealing relationships between various columns. We also delve into missingness analysis, specifically focusing on NMAR patterns in reviews as well as MCAR and MAR patterns in ratings. This exploration provides insights into missing data intricacies, showcasing dependencies within critical columns and enhancing our understanding of the dataset's dynamics.
   
   In this  project, we aim to unravel the connection between the seasonality of recipe submissions and their calorie content. As we navigate through the changing seasons, our primary inquiry centers around understanding how the nutritional composition of recipes varies with the seasons. Delving into the interplay between the timing of recipe submissions and calorie content, we investigate the relationship between the season in which recipes are submitted and the nutritional density, more specifically focusing on calorie content. This analysis aims to provide insights into how dietary preferences and nutritional choices vary with the changing seasons. Thus through out this project, we focused on the research question, what is the relationship between the season in which recipes are submitted and calorie content? By exploring this question, we aspire to highlight the seasonal dynamics that influence the nutritional landscape of the recipes people choose to share and indulge in. 

### Dataset Explanation:
The merged dataset, that we created by merging a dataset with 83782 rows of different recipes and a dataset with 731927 rows of reviews on the recipes, contains 731927 rows over removing outliers. The following are columns from the merged dataset that are relevant to our research question and analysis.

|Column	                 |Description|
|---                     |---        |
|`'id'	`                |ID of Recipe|
|`'name'`	             |Name of Recipe|
|`'minutes'`	         |# of minutes it takes to prepare recipe|
|`'n_steps'`	         |# of steps in the recipe|
|`'n_ingredients'`	     |# of ingredients in the recipe|
|`'rating'`	             |Rating recipe recieved|
|`'avg_rating'`	         |Average rating of recipe|
|`'month'`	             |Numeric month in which recipe was submitted|
|`'season'`	             |Season in which recipe was submitted(Winter, Spring, Summer, Fall)|
|`'calories'`	         |# of calories in recipe as string type|
|`'cal'`	             |# of calories in recipe as float type|


## Cleaning and Exploratory Data Analysis
###  Data Cleaning

We took a couple steps to make our data more readable and relevant to our research question and topic. 

1. First we left merged the recipes and interaction datasets together and filled all rating of 0 with np.nan. This was done in response to possible reviews being submitted with no ratings. These get inputted as zero's into the dataset but they are false zeros. So we remove them and replace with np.nans for a more true resemblance of peoples ratings.

2. We then found the average rating per recipe and added this back to the recipes dataset. 

3. Then we changed all columns with non-list type variables into lists (useful for our specific data needs) A `month` column was created that contained integer values corresponding to what month the recipe was submitted. We used mapping to determine what season each month would be categorized into. Seasons were categorized as [(Winter: December, January, February), (Spring: March, April, May), (Summer: June, July, August), (Fall: September, October, November)]

4. We also created a `calories` and `cal` columns that contained the calorie values taken from the 'nutrition' column as string and float types, respectively. We decided to have two columns, one with calories as floats and one with calories as strings, because it was what we found to be the most convienent when trying to create our plots and analysis. The `nutrition` column contained a lists that with each number in the lists corresponding to ['calories', etc, etc] in that particular order.

5. We thought there may be certain anomalies in the data so we decided to remove outliers. The numbers we used were .95 for upper quartiles and .05 for lower quartiles. However, we only removed the values that were greater than the upper quartile. The reasoning is because smaller calorie foods are more relevant to our data because there are many foods that can fall within smaller calorie counts, however our data has some anomalies with recipes that have more than 40,000 calories. This throws off our mean and max in following analyses performed so removing only the higher bounds seemed fine.

After taking these steps, the cleaned and more readable dataframe is as shown below:

|     id | name                                 |   minutes |   n_ingredients |   rating |   avg_rating |   month | season   |   calories |   cal |
|:-------|:-------------------------------------|:----------|:----------------|:---------|:-------------|:--------|:---------|:-----------|:------|
| 333281 | 1 brownies in the world    best ever |        40 |               9 |        4 |            4 |      10 | Fall     |      138.4 | 138.4 |
| 453467 | 1 in canada chocolate chip cookies   |        45 |              11 |        5 |            5 |       4 | Spring   |      595.1 | 595.1 |
| 306168 | 412 broccoli casserole               |        40 |               9 |        5 |            5 |       5 | Spring   |      194.8 | 194.8 |
| 306168 | 412 broccoli casserole               |        40 |               9 |        5 |            5 |       5 | Spring   |      194.8 | 194.8 |
| 306168 | 412 broccoli casserole               |        40 |               9 |        5 |            5 |       5 | Spring   |      194.8 | 194.8 |


### Univariate Analysis

We were most interested in the `month`, `seasons`, and `calories` columns for this analysis

1. Here is our first analysis as a bar graph that plots 'index' on the x and 'month' on the y. This shows the distribution of recipes that were submitted each month. In this graph we can see that there is a general decrease in the amount of recipes submitted each month, with an unusual spike in number of recipes submitted in May. This visual representation provides a valuable snapshot of the overall temporal pattern in recipe submissions.
<iframe src="assets/univariate_fig_month.html" width=800 height=600 frameBorder=0></iframe>

2. Here is our second analysis as another bar graph that plots 'index' on the x and 'seasons' on the y. This show the distribution of recipes that were submitted each season (categorized above in our cleaning steps). Looking at this graph we can see that Spring and Winter have the most submitted recipes. This could potentially be attributed to January (in Winter) and May (in Spring) having the most number of recipes submitted, as seen in the first bar graph, bringing the total for the two respective seasons. 
<iframe src="assets/univariate_fig_season.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

For our bivariate analyses it should be noted that we created a new column called 'calories_category' that uses bins to categorize all calories in amounts of 300 [(0,300], [300, 600], [600,900], [900, inf)] and a new dataframe called count_season_calories_df that is used in our plot.

1. Our first analysis uses a scatter plot that takes in 'calories_category' and index name 'recipe_count'. The resulting plot reveals a distinct variation across different seasons and calorie bins. Notably, in the 0 to 300 calorie bin, Spring has the most number of recipes, suggesting a potential inclination towards submssion of lighter, vegetable-centric recipes coinciding with the harvest season. Meanwhile, Winter has the most recipes in the other three bins with higher calorie categories, suggesting a potential preference for heartier, more calorie dense recipes during the colder months, especially around the holidays. While Fall seems to be the most consistently lowest among the four.
<iframe src="assets/bivariate_fig_season_calories.html" width=800 height=600 frameBorder=0></iframe>

2. Our second analysis uses a violin plot that takes in 'season' and 'calories'. The plot shows a similarity in distribution across all seasons, with each season contributing to a comparable spread of calorie values. However, the plot does show that Summer having the most uniform shape in terms of calorie distribution, compared to the other season. In contrast, Winter exhibits a more erratic structure, hinting at a potentially diverse range of calorie content in recipes during this colder season.  In contrast, Winter exhibits a more erratic structure, hinting at a potentially diverse range of calorie content in recipes during this colder season. Fall and Spring seem to be the closest in terms of seasons vs calories,  showcasing a closer alignment in terms of the relationship between the seasonality of recipe submissions and their associated calorie values.
<iframe src="assets/bivariate_fig_violin_calories.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

Our visual examples show interesting trends and plots so far, but we want different reliable analyses that can show the relationship between other columns of our data set and our created columns.

Here is pivot table one where we show the relationship between calories and season. In this one we analyze the count, mean, median, min, max, and std of calories. From the data we can infer that Fall and Winter are the seasons with the highest mean calories while Spring and Summer are both less than them which corroborates with our above plots. 

| season   |   ('count', 'cal') |   ('mean', 'cal') |   ('median', 'cal') |   ('min', 'cal') |   ('max', 'cal') |   ('std', 'cal') |
|:---------|:-------------------|:------------------|:--------------------|:-----------------|:-----------------|:-----------------|
| Fall     |              46394 |           432.172 |               308.6 |                0 |          17287.5 |          607.138 |
| Spring   |              68479 |           412.099 |               296.2 |                0 |          17551.6 |          547.689 |
| Summer   |              53654 |           410.673 |               296   |                0 |          18656   |          492.435 |
| Winter   |              65888 |           419.837 |               307.8 |                0 |          17280.4 |          526.988 |


Here is pivot table two where we show the relationship between avg_rating and month. We analyze count, mean, median, min, max, and std of avg_rating for each month. Looking at the means we can see that they are mostly uniform with a different of less than .1 at most. This suggests a general stability in average ratings across the months, indicating a consistent trend in how recipes are rated throughout the year.

|   month |   ('count', 'avg_rating') |   ('mean', 'avg_rating') |   ('median', 'avg_rating') |   ('min', 'avg_rating') |   ('max', 'avg_rating') |   ('std', 'avg_rating') |
|:--------|:--------------------------|:-------------------------|:---------------------------|:------------------------|:------------------------|:------------------------|
|       1 |                     30063 |                  4.64981 |                    4.80952 |                       1 |                       5 |                0.503994 |
|       2 |                     22697 |                  4.65876 |                    4.83333 |                       1 |                       5 |                0.503039 |
|       3 |                     23040 |                  4.68869 |                    4.875   |                       1 |                       5 |                0.477796 |
|       4 |                     16905 |                  4.66964 |                    4.88889 |                       1 |                       5 |                0.520768 |
|       5 |                     27826 |                  4.70827 |                    4.86207 |                       1 |                       5 |                0.4409   |


Here is pivot table three where we show the relationship between n_ingredients and calories_category.  By looking at count, mean, median, min, max, and std of n_ingredients, we observe a clear trend: as calorie categories increase, the mean number of ingredients also tend to go up as well. This indicates a positive correlation, suggesting that recipes with more calories tend to have more ingredients. This finding helps provide further insights into the relationship between recipe complexity and nutritional content.

| calories_category   |   ('count', 'n_ingredients') |   ('mean', 'n_ingredients') |   ('median', 'n_ingredients') |   ('min', 'n_ingredients') |   ('max', 'n_ingredients') |   ('std', 'n_ingredients') |
|:--------------------|:-----------------------------|:----------------------------|:------------------------------|:---------------------------|:---------------------------|:---------------------------|
| (0.0, 300.0]        |                       116720 |                     8.14746 |                             8 |                          1 |                         31 |                    3.41626 |
| (300.0, 600.0]      |                        76937 |                     9.79881 |                             9 |                          1 |                         33 |                    3.75761 |
| (600.0, 900.0]      |                        24259 |                    10.4949  |                            10 |                          1 |                         30 |                    4.18617 |
| (900.0, inf]        |                        16397 |                    10.1623  |                            10 |                          1 |                         37 |                    4.55006 |
| nan                 |                          102 |                     3.37255 |                             3 |                          2 |                          6 |                    1.50196 |


## Missingness Assessment
### NMAR Analysis

The missing values from the `review` column is likely to be Not Missing at Random, NMAR. NMAR occurs when the probability of a value being missing depends on reasons unknown to us. This aligns with the idea that  people who could have submitted reviews with rating the recipes because they did not resonate with the recipe or have some personal connection. For example, someone could have left a rating and did not write a review because nothing about the recipe really stuck out to them and made them want to write about. Other examples include people finding it challenging to articulate their experience with the recip in words, so they would leave a quanitative rating. Additional data that could be obtain in order to explain the missingness of the `review` would be adding a yes/no question asking people who use the recipe if they enjoyed the recipe.  This could potentially identify patterns or trends that explain why certain users do not leave reviews, such as, people who did not enjoy the recipe tending to leave less reviews or people who did enjoy the recipe tending to leave less reviews. 

### Missingness Dependency

We decided to test rating against two different column names for our permutation test to see if there are observable differences. 

#### Testing `rating` and `minutes`
Null Hypothesis: There is no association between the missingness of ratings and the minutes of recipe preparation.
Alternative Hypothesis: The missingness of ratings is dependent on the minutes of recipe preparation.

After running the permutation test on these columns we get an observed difference in means of 51.507230759 and a p-value of .121. Using 0.05 as the significant threshold, since the p-value is higher than the threshold, we can conclude that the missingness of `rating` is most likely not dependent on `minutes`, which is Missing Completely at Random, MCAR.

<iframe src="assets/perm_missing_test.html" width=800 height=600 frameBorder=0></iframe>

#### Testing `rating` and `n_steps`
Null Hypothesis: There is no association between the missingness of ratings and the number of steps in the recipes.
Alternative Hypothesis: The missingness of ratings is dependent on the the number of step in the recipes.

After running the permutation test on these columns we get an observed difference in means of 1.331260648 and a p-value of approximately 0.0. Using 0.05 as the significant threshold, since the p-value is lower than the threshold, we can conclude that the missingness of `rating` is most likely dependent on `n_steps`, which is Missing At Random, MAR.
<iframe src="assets/perm_missing_test2.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Testing

We now circle back to our original topic and research question at hand. 

Null Hypothesis (H0): There is no significant difference in the mean calorie content among recipes submitted in different seasons.

Alternative Hypothesis (H1): There is a significant difference in the mean calorie content among recipes submitted in different seasons. 

Columns used: For the permutation test, the columns selected to use in the test were the `cal` and `season` columns because those are the two columns that our hypothesis is testing on.

|   cal | season   |
|:------|:---------|
| 138.4 | Fall     |
| 595.1 | Spring   |
| 194.8 | Spring   |
| 194.8 | Spring   |
| 194.8 | Spring   |

Since the data for `cal` are float types, we used the difference in mean as test statistics. In addition the significance level chosen to compare with p-value generated is 0.05.

### Permutation Test
<iframe src="assets/empirical_hist.html" width=800 height=600 frameBorder=0></iframe>

### Conclusion
After conducting the permutation test, we get an observed difference in means of 9.1641596154437427 and a p-value of approximately 0.0. Using 0.05 as the significant threshold, since the p-value is lower than the threshold, we can reject the null hypothesis. This indicates a statistically significant difference in the mean calorie content across seasons, suggesting that seasonal variations play a role in influencing the nutritional composition of submitted recipes.