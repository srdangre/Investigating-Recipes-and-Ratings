# Investigating Sodium Levels and Number of Steps in Recipes
by Saloni Dangre
DSC80 Final Project

## Introduction:

Maintaining a healthy diet is an essential part of increasing longevity, improving quality of life, and decreasing the risk of developing chronic illnesses such as heart disease, diabetes and cancer.

This is, however, becoming increasingly difficult to achieve due to the prevalence and accessibility of ultraprocessed foods. Ultraprocessed foods are foods that have undergone extensive industrial processing and contain main added ingredients, artificial colors, and stabilizers. While appealing and easy to indulge in, exposure to ultraprocessed foods is ultimately linked with the chronic illnesses listed above. 

A good way to avoid introducing excessive amounts of ultraprocessed food into one's diet is to prepare meals at home. Cooking meals, however can be time consuming and complex. To look further into the relationship between sodium and the complexity of recipes, the main question I aimed to explore was: **Do more simple recipes contain less sodium?** If recipes with a fewer number of steps contained lower sodium levels, cooking simple meals could be proposed as a more time-effective, realistic replacement for ultraprocessed foods. 

The dataset I choose to explore was the Recipes and Ratings dataset, which contains recipes and ratings scrapped by Majumder et al. from [food.com](https://www.food.com/). The final dataset is a merged version of two individal datasets: recipes and ratings which are each explored in more detail below:

##### RECIPES
Number of Rows: 83782

(relevant columns have an asterisk)

| Column          | Description |
|----------------|------------|
| `name`         | Recipe name |
| `id`           | Recipe ID |
| `minutes`      | Minutes to prepare recipe |
| `contributor_id` | User ID who submitted this recipe |
| `submitted`    | Date recipe was submitted |
| `tags`         | Food.com tags for recipe |
| `nutrition*`    | Nutrition information in the form `[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]`; PDV stands for “percentage of daily value” |
| `n_steps*`      | Number of steps in recipe |
| `steps`        | Text for recipe steps, in order |
| `description`  | User-provided description |

##### RATINGS
Number of Rows: 731927

| Column      | Description          |
|------------|----------------------|
| `user_id`  | User ID              |
| `recipe_id` | Recipe ID           |
| `date`     | Date of interaction  |
| `rating`   | Rating given         |
| `review`   | Review text          |

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
After downloading both datasets the steps below were followed to create a merged dataset 'food_df' that was used for the rest of the project. 

1. The recipes and reviews datasets were merged on 'id' and 'recipe_id' respectively using left merge
2. Since ratings are typically on a scale of 1 to 5, ratings of 0 were treated as missing ratings and replaced with np.nan values
3. Average rating per recipe was found by grouping the dataframe by recipe_id and calculating the mean value for the rating column. The average ratings were then added back to the merged dataset under a new column 'avg_rating'
4. The 'nutrition' column was converted from a string to a list and seperated into respective columns (representing # for calories and PDV for the rest) as float values. Sodium (PDV) was the most relevant nutritional value to this project. 
5. A new column 'sodium_lvl', containing booleans, was created by filtering the Sodium (PDV) column using the value 20 PDV as the threshold. The [FDA](https://www.fda.gov/food/nutrition-facts-label/lows-and-highs-percent-daily-value-nutrition-facts-label#:~:text=The%20percent%20Daily%20Value%20(%25,or%20low%20in%20a%20nutrient) considers sodium levels above 20 PDV 'high sodium', so in this study, I categorized recipes above 20 PDV as 'High Sodium' and recipes under this threshold 'Low Sodium'. 
*After examining the FDA site more closely, I found that 'Low Sodium' actually refers to food items that are below 5 PDV. For the sake of this study, the 'Low Sodium' category represents all sodium levels that are not high.*
6. Outliers in Sodium (PDV) were removed using the IQR method of outlier detection. 

The cleaned dataframe has the same columns as the merged recipes and ratings dataframe with additional PDV values and a new sodium_lvl column. The dataframe is displayed below with relevant columns: 

| name                                 |   recipe_id |   sodium (PDV) |   rating |   avg_rating |   n_steps | sodium_lvl   |
|:-------------------------------------|------------:|---------------:|---------:|-------------:|----------:|:-------------|
| 1 brownies in the world    best ever |      333281 |              3 |        4 |            4 |        10 | Low          |
| 1 in canada chocolate chip cookies   |      453467 |             22 |        5 |            5 |        12 | High         |
| 412 broccoli casserole               |      306168 |             32 |        5 |            5 |         6 | High         |
| 412 broccoli casserole               |      306168 |             32 |        5 |            5 |         6 | High         |
| 412 broccoli casserole               |      306168 |             32 |        5 |            5 |         6 | High         |


### Univariate Plots
<iframe
  src="assets/n_steps Histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The number of steps is most frequently around 10 steps, and the overall distribution of number of steps is skewed to the right, with some outliers close to 100 steps. 

<iframe
  src="assets/Sodium PDV Histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The sodium PDV histogram (before removing outliers) is heavily skewed to the right, indicating that there are some especially high sodium outliers in this dataset. 

<iframe
  src="assets/(No Outliers) Sodium PDV Histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
After removing the outliers, the sodium PDV histogram is still skewed to the right, but less heavily so. 

### Bivariate Plots
<iframe
  src="assets/bivariate_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The n_steps for low and high sodium recipes looked similar, with high sodium recipies having a slightly larger median n_steps. The two categories also have slightly different patterns of n_step upper outliers. 


### Interesting Aggregates
Since Americans statistically consume too much sodium, I wanted to see if high sodium recipies were more highly rated

|   rating |   High |    Low |
|---------:|-------:|-------:|
|        1 |    787 |   1753 |
|        2 |    705 |   1401 |
|        3 |   2158 |   4285 |
|        4 |  11790 |  22148 |
|        5 |  51342 | 101964 |

There were more ratings for low sodium recipies to begin with. To get a better sense of how ratings were distributed among each category, the counts were divided by the total number of ratings per column

|   rating |   High_prop |   Low_prop |
|---------:|------------:|-----------:|
|        1 |   0.0117846 |  0.0133256 |
|        2 |   0.0105567 |  0.0106499 |
|        3 |   0.0323141 |  0.0325729 |
|        4 |   0.176545  |  0.168361  |
|        5 |   0.7688    |  0.775091  |

Looking at the proportions, high and low sodium levels have a similar distribution of ratings, with there being a slightly higher proportion of 4-star ratings for high sodium recipes and a slightly higher proportion of 5-star ratings for low sodium recipes.

## Assessment of Missingness

### NMR Analysis
The three main columns with missing data in my dataset are descriptions, ratings, and reviews. Avg_reviews also has missing data, but since data from reviews was used to create the average reviews column, the missing data is directly related to the missing data in reviews. The missingness of average reviews will not be discussed in this section.

I believe both ratings and reviews are both NMAR. People tend to leave ratings and reviews when they feel strongly about a product, item, or, in this case, recipe. There are no columns in the dataframe that indicate user emotion attached to recipe other than the columns ratings and reviews themselves. A column indicating reviewer strength of opinion could be added to this dataframe to make these two columns MAR. 

I believe the description column, however, is MAR, and choose to further analyze its missingness dependency.

### Missingness Dependency
Under the assumption that shorter or more simple recipes may not have a description, I first choose to investigate the 'minutes' column

*The outliers in minutes spent cooking initially made it difficult to see the shape of the two distributions so the plot was rescaled*

<iframe
  src="assets/Minutes Spent Cooking by Missingness of Description.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Since the distributions of minutes spent cooking when description is True and minutes spent cooking when description is False have a different shape and since we only aim to find out if the two distributions are different, we will use the KS test statistic for our permutation test.

<iframe
  src="assets/Empirical Distribution of the K-S Statistic.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Visually, the observed KS test statistic is in the emperical distribution of the KS test statistic, indicating that we fail to reject our null hypothesis that, using the 'minutes' column, the missing and not-missing distributions are the same. Missingness doesn't depend on the 'minutes spent' column. 

This was confirmed by my p-value of 0.192, which is clearly above my significance level of 0.05. 


Following the same idea that shorter or more simple recipes may not have a description, I decided to investigate the 'n_steps' column next. 

<iframe
  src="assets/Number of Steps by Missingness of Description.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Since the distributions have different shapes and since we only aim to find out if the two distributions are different, we will use the KS test statistic for our permutation test. 

<iframe
  src="assets/Second_Empirical Distribution of the K-S Statistic.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Visually, the observed KS test statistic is at the very edge of the emperical distribution of the KS test statistic, indicating that we might be able to reject our null hypothesis that, using the 'n_steps' column, the missing and not-missing distributions are the same. Missingness does depend on the 'n_steps' column. 

This was confirmed by my p-value of 0.008, which is clearly below my significance level of 0.05. 

Overall, the missingness of the descriptions value **does not** depend on minutes spent and **does** depend on n_steps. 

## Hypothesis Testing
Next, I used permutation testing to better understand the question: "Do more simple recipes contain less sodium?"

First I created a histogram to look at the distributions of n_steps at high and low sodium levels

<iframe
  src="assets/dist_high_low_sod.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
I used the difference in group means test statistic since the distributions looked like they were similar shapes with slightly different centers. 

Null Hypothesis:
Alternative Hypothesis:

<iframe
  src="assets/perm_testing_emp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
On the empirical distribution of the mean differences with the observed group mean plotted, the observed group mean is far from the emperical distribution of the test statistic, indicating that we will likely reject our null hypothesis. I further confirmed this by calculating a p-value.

I decided to choose a lower significance level to set more strict standards for evidence to reject the null hypothesis

Significance Level: 0.01
p-value: 0.0

Since my p-value is below my significance level, I reject my null hypothesis that there is no difference between the group mean n_steps of the high and low sodium groups. The difference between the two samples is statistically significant.

## Framing a Prediction Problem
I will now shift to more closely examining the n_steps variable for this dataframe. I decided to investigate if I could predict the number of steps in recipes, which I used linear regression to answer.

The reponse variable for this regression problem will be the number of steps, and I will use root mean square error ($RMSE$) to evaluate my model. I decided to use this metric over a metric like $R^2$ because I am more interested in measuring prediction error in my model over evaluating the variance explained by my model. 

**CHECK IF THIS STATEMENT IS SOUND**
Since I am looking to predict the number of steps, I will not train my model on the number of steps data.

## Baseline Model
I chose the features sodium PDV, minutes, and n_ingredients to train my baseline regression model. 

All three are quantitative variables. I used the StandardScalar tool for the minutes and n_ingredients models to standardize the units in each feature. I used the Binarizer tranformer to split the sodium PDV into two categories, using the threshold 20 PDV. After applying this transformation, this feature is now ordinal. 

I choose minutes and n_ingredients since they seemed like variables that could be related to the overall time a recipe took and have a linear relationship with n_steps. I choose to investigate the relationship betwen sodium PDV (now similar to sodium_lvl after transformation) and n_steps further since the permutation test I ran on the n_steps and sodium_lvl came back with statistically significant results. 

The RMSE on the training and testing data are as follows:

|            |    RMSE |
|:-----------|--------:|
| rmse_train | 5.75289 |
| rmse_test  | 5.69737 |


Since rmse_train and rmse_test are similar, it doesn't seem like our model is overfitting to the training data. Additionally, considering that the range of n_steps is 99, the root mean square error for both the training and test data is at about 6% of the overall range of my predicted variable, which means, relative to my model, the RMSE is low. 

## Final Model
I now seek to improve my baseline model by using a more complex regression model and utlizing hyperparameter tuning. I will use GridSearchCV to tune hyperparamters and use a RandomForestRegressor model to predict n_steps. 

## Fairness Analysis
