# Analysis-on-Recipes-and-Rating-Dataset
Project for DSC80 at UCSD

Authors: Daniel Park, Tracy Pham

## Project Overview:
   In this research, we aim to investigate the relationship between the season in which recipes are submitted and the nutritional density, specifically focusing on calorie content. This analysis can provide insights into how dietary preferences and nutritional choices vary with the changing seasons


## Project Introduction:
With the changing seasons follows a new set of recipes that people like to associate with and cook. People's dietary routines and preferences 

## Dataset Explanation:
We used two datasets for this research question. The RAW_recipes and RAW_interactions datsets. We initially merged these two

## Cleaning and EDA:

### Cleaning
We took a couple steps to make our data more readable and relevant to our research question and topic. 

1. First we left merged the recipes and interaction datasets together and filled all rating of 0 with np.nan.

2. We then found the average rating per recipe and added this back to the recipes dataset. 

3. Then we changed all columns with non-list type variables into lists (useful for our specific data needs) A 'month' column was created that contained integer values corresponding to what month the recipe was submitted. We used mapping to determine what season each month would be categorized into. Seasons were categorized as [(Winter: December, January, February), (Spring: March, April, May), (Summer: June, July, August), (Fall: September, October, November)]

4. We also created a 'calories' and 'cal' columns that contained the calorie values taken from the 'nutrition' column as string and float types, respectively. The nutrition column contained a lists that with each number in the lists corresponding to ['calories', etc, etc] in that particular order.

### Univariate Analysis
