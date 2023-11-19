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

-test
|     id | name                                 |   minutes | submitted   | tags                                                                                                                                                                                                                                                                 | nutrition                                                        |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                                              |   n_ingredients | date       |   rating | review                                                                                                                                                                                                                                                                                                                                           |   avg_rating |   month | season   |   calories |   cal |
|-------:|:-------------------------------------|----------:|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------|----------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|:-----------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------:|--------:|:---------|-----------:|------:|
| 333281 | 1 brownies in the world    best ever |        40 | 2008-10-27  | ["'60-minutes-or-less'", " 'time-to-make'", " 'course'", " 'main-ingredient'", " 'preparation'", " 'for-large-groups'", " 'desserts'", " 'lunch'", " 'snacks'", " 'cookies-and-brownies'", " 'chocolate'", " 'bar-cookies'", " 'brownies'", " 'number-of-servings'"] | ['138.4', ' 10.0', ' 50.0', ' 3.0', ' 3.0', ' 19.0', ' 6.0']     |        10 | ["'heat the oven to 350f and arrange the rack in the middle'", " 'line an 8-by-8-inch glass baking dish with aluminum foil'", " 'combine chocolate and butter in a medium saucepan and cook over medium-low heat ", ' stirring frequently ', " until evenly melted'", " 'remove from heat and let cool to room temperature'", " 'combine eggs ", ' sugar ', ' cocoa powder ', ' vanilla extract ', ' espresso ', " and salt in a large bowl and briefly stir until just evenly incorporated'", " 'add cooled chocolate and mix until uniform in color'", " 'add flour and stir until just incorporated'", " 'transfer batter to the prepared baking dish'", " 'bake until a tester inserted in the center of the brownies comes out clean ", " about 25 to 30 minutes'", " 'remove from the oven and cool completely before cutting'"]                                                     | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ["'bittersweet chocolate'", " 'unsalted butter'", " 'eggs'", " 'granulated sugar'", " 'unsweetened cocoa powder'", " 'vanilla extract'", " 'brewed espresso'", " 'kosher salt'", " 'all-purpose flour'"] |               9 | 2008-11-19 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |            4 |      10 | Fall     |      138.4 | 138.4 |
| 453467 | 1 in canada chocolate chip cookies   |        45 | 2011-04-11  | ["'60-minutes-or-less'", " 'time-to-make'", " 'cuisine'", " 'preparation'", " 'north-american'", " 'for-large-groups'", " 'canadian'", " 'british-columbian'", " 'number-of-servings'"]                                                                              | ['595.1', ' 46.0', ' 211.0', ' 22.0', ' 13.0', ' 51.0', ' 26.0'] |        12 | ["'pre-heat oven the 350 degrees f'", " 'in a mixing bowl ", " sift together the flours and baking powder'", " 'set aside'", " 'in another mixing bowl ", ' blend together the sugars ', ' margarine ', " and salt until light and fluffy'", " 'add the eggs ", ' water ', " and vanilla to the margarine / sugar mixture and mix together until well combined'", " 'add in the flour mixture to the wet ingredients and blend until combined'", " 'scrape down the sides of the bowl and add the chocolate chips'", " 'mix until combined'", " 'scrape down the sides to the bowl again'", " 'using an ice cream scoop ", " scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking'", " 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center'", " 'serve hot and enjoy !'"] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ["'white sugar'", " 'brown sugar'", " 'salt'", " 'margarine'", " 'eggs'", " 'vanilla'", " 'water'", " 'all-purpose flour'", " 'whole wheat flour'", " 'baking soda'", " 'chocolate chips'"]              |              11 | 2012-01-26 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |            5 |       4 | Spring   |      595.1 | 595.1 |
| 306168 | 412 broccoli casserole               |        40 | 2008-05-30  | ["'60-minutes-or-less'", " 'time-to-make'", " 'course'", " 'main-ingredient'", " 'preparation'", " 'side-dishes'", " 'vegetables'", " 'easy'", " 'beginner-cook'", " 'broccoli'"]                                                                                    | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']    |         6 | ["'preheat oven to 350 degrees'", " 'spray a 2 quart baking dish with cooking spray ", " set aside'", " 'in a large bowl mix together broccoli ", ' soup ', ' one cup of cheese ', ' garlic powder ', ' pepper ', ' salt ', ' milk ', ' 1 cup of french onions ', " and soy sauce'", " 'pour into baking dish ", " sprinkle remaining cheese over top'", " 'bake for 25 minutes or until cheese is lightly browned'", " 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly ", " about 10 more minutes'"]                                                                                                                                                                                                                                                                                                                                    | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ["'frozen broccoli cuts'", " 'cream of chicken soup'", " 'sharp cheddar cheese'", " 'garlic powder'", " 'ground black pepper'", " 'salt'", " 'milk'", " 'soy sauce'", " 'french-fried onions'"]          |               9 | 2008-12-31 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |            5 |       5 | Spring   |      194.8 | 194.8 |
|        |                                      |           |             |                                                                                                                                                                                                                                                                      |                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                                          |                 |            |          | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |              |         |          |            |       |
|        |                                      |           |             |                                                                                                                                                                                                                                                                      |                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                                          |                 |            |          | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                          |              |         |          |            |       |
| 306168 | 412 broccoli casserole               |        40 | 2008-05-30  | ["'60-minutes-or-less'", " 'time-to-make'", " 'course'", " 'main-ingredient'", " 'preparation'", " 'side-dishes'", " 'vegetables'", " 'easy'", " 'beginner-cook'", " 'broccoli'"]                                                                                    | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']    |         6 | ["'preheat oven to 350 degrees'", " 'spray a 2 quart baking dish with cooking spray ", " set aside'", " 'in a large bowl mix together broccoli ", ' soup ', ' one cup of cheese ', ' garlic powder ', ' pepper ', ' salt ', ' milk ', ' 1 cup of french onions ', " and soy sauce'", " 'pour into baking dish ", " sprinkle remaining cheese over top'", " 'bake for 25 minutes or until cheese is lightly browned'", " 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly ", " about 10 more minutes'"]                                                                                                                                                                                                                                                                                                                                    | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ["'frozen broccoli cuts'", " 'cream of chicken soup'", " 'sharp cheddar cheese'", " 'garlic powder'", " 'ground black pepper'", " 'salt'", " 'milk'", " 'soy sauce'", " 'french-fried onions'"]          |               9 | 2009-04-13 |        5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |            5 |       5 | Spring   |      194.8 | 194.8 |
| 306168 | 412 broccoli casserole               |        40 | 2008-05-30  | ["'60-minutes-or-less'", " 'time-to-make'", " 'course'", " 'main-ingredient'", " 'preparation'", " 'side-dishes'", " 'vegetables'", " 'easy'", " 'beginner-cook'", " 'broccoli'"]                                                                                    | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']    |         6 | ["'preheat oven to 350 degrees'", " 'spray a 2 quart baking dish with cooking spray ", " set aside'", " 'in a large bowl mix together broccoli ", ' soup ', ' one cup of cheese ', ' garlic powder ', ' pepper ', ' salt ', ' milk ', ' 1 cup of french onions ', " and soy sauce'", " 'pour into baking dish ", " sprinkle remaining cheese over top'", " 'bake for 25 minutes or until cheese is lightly browned'", " 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly ", " about 10 more minutes'"]                                                                                                                                                                                                                                                                                                                                    | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ["'frozen broccoli cuts'", " 'cream of chicken soup'", " 'sharp cheddar cheese'", " 'garlic powder'", " 'ground black pepper'", " 'salt'", " 'milk'", " 'soy sauce'", " 'french-fried onions'"]          |               9 | 2013-08-02 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |            5 |       5 | Spring   |      194.8 | 194.8 |

### Univariate Analysis

We were most interested in the 'month', 'seasons', and 'calories' columns for this analysis

1. Here is our first analysis as a bar graph that plots 'index' on the x and 'month' on the y. This shows the distribution of recipes that were submitted each month. In this graph we can see that there is a general decrease in the amount of recipes submitted each month.

<iframe src="assets/univariate_fig_month.html" width=800 height=600 frameBorder=0></iframe>

2. Here is our second analysis as another bar graph that plots 'index' on the x and 'seasons' on the y. This show the distribution of recipes that were submitted each season (categorized above in our cleaning steps). Looking at this graph we can see that Spring and Winter have the most submitted recipes.

<iframe src="assets/univariate_fig_season.html" width=800 height=600 frameBorder=0></iframe>

3. Our third analysis is a histogram that plots the distribution of calories across all unique recipes. From the data we can state that generally higher and lower calorie recipes have slightly fewer counts of recipes. There is visible variation in the distribution as well. 

<iframe src="assets/univariate_fig_hist_calories.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

For our bivariate analyses it should be noted that we created a new column called 'calories_category' that uses bins to categorize all calories in amounts of 300 [(0,300], [300, 600], [600,900], [900, inf)] and a new dataframe called count_season_calories_df that is used in our plot.

1. Our first analysis uses a scatter plot that takes in 'calories_category' and index name 'recipe_count'. From the resulting plot we can see a clear variation among the seasons and calori categories. Fall seems to be the most consistently lowest among the four while winter seems to be consistently the highest. 

<iframe src="assets/bivariate_fig_season_calories.html" width=800 height=600 frameBorder=0></iframe>

2. Our second analysis uses a violin plot that takes in 'season' and 'calories'. The graph shows that Summer seems to be the most uniform in comparison to the other seasons. While Winter has a more erratic structure. Fall and Spring seem to be the closest in terms of seasons vs calories

<iframe src="assets/bivariate_fig_violin_calories.html" width=800 height=600 frameBorder=0></iframe>

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

<iframe src="assets/perm_missing_test.html" width=800 height=600 frameBorder=0></iframe>

2. Testing 'rating' and 'n_steps'

After running the permutation test on these columns we get an observed difference in means of 1.331260648 and a p-value of 0.0, and can conclude that the missingness of 'rating' is most likely dependent on 'n_steps'

<iframe src="assets/perm_missing_test2.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Testing

We now circle back to our original topic and research question at hand. 

Null Hypothesis (H0): There is no significant difference in the mean calorie content among recipes submitted in different seasons.

Alternative Hypothesis (H1): There is a significant difference in the mean calorie content among recipes submitted in different seasons. 

<iframe src="assets/hypothesis_testing.html" width=800 height=600 frameBorder=0></iframe>

stuff ???

<iframe src="assets/empirical_hist.html" width=800 height=600 frameBorder=0></iframe>