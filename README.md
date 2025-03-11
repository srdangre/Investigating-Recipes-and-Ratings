# Investigating Sodium Levels in Recipes
by Saloni Dangre
DSC80 Final Project

## Introduction:

Maintaining a healthy diet is an essential part of increasing longevity, improving quality of life, and decreasing the risk of developing chronic illnesses such as heart disease, diabetes and cancer.

This is, however, becoming increasingly difficult to achieve due to the prevalence and accessibility of ultraprocessed foods. Ultraprocessed foods are foods that have undergone extensive industrial processing and contain main added ingredients, artificial colors, and stabilizers. While appealing and easy to indulge in, exposure to ultraprocessed foods is ultimately linked with the chronic illnesses listed above. 

A good way to avoid introducing excessive amounts of ultraprocessed food into one's diet is to prepare meals at home. Cooking meals, however can be time consuming and complex. To look further into the relationship between sodium and the complexity of recipes, the main question I aimed to explore was: **Do more simple recipes contain less sodium?**. If recipes with a fewer number of steps contained lower sodium levels, cooking simple meals could be proposed as a more time-effective, realistic replacement for ultraprocessed foods. 



The dataset I choose to explore was the Recipes and Ratings dataset, which contains recipes and ratings scrapped by Majumder et al. from https://www.food.com/. The final dataset is a merged version of two individal datasets: recipes and ratings which are each explored in more detail below:

##### RECIPES
Number of Rows: 83782
Columns (relevant columns have an astrik)
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
Columns
| Column      | Description          |
|------------|----------------------|
| `user_id`  | User ID              |
| `recipe_id` | Recipe ID           |
| `date`     | Date of interaction  |
| `rating`   | Rating given         |
| `review`   | Review text          |

## Data Cleaning and Exploratory Data Analysis
### Univariate Plots
- N_steps histogram
- Sodium level histogram
### Bivariate Plots
- High sodium vs low sodium n_steps, boxplot
### Interesting Aggregates
- Since Americans statistically consume too much sodium, I wanted to see if high sodium recipies were more highly rated

|   rating |   High |    Low |
|---------:|-------:|-------:|
|        1 |   1117 |   1753 |
|        2 |    967 |   1401 |
|        3 |   2887 |   4285 |
|        4 |  15159 |  22148 |
|        5 |  67712 | 101964 |

- There were more ratings for low sodium recipies to begin with. To get a better sense of how ratings were distributed among each category, the counts were divded by the total number of ratings per column

|   rating |   High_prop |   Low_prop |
|---------:|------------:|-----------:|
|        1 |   0.012716  |  0.0133256 |
|        2 |   0.0110084 |  0.0106499 |
|        3 |   0.0328658 |  0.0325729 |
|        4 |   0.172571  |  0.168361  |
|        5 |   0.770839  |  0.775091  |

- Looking at the proportions, high and low sodium levels have a similar distribution of ratings.
## Assessment of Missingness

## Hypothesis Testing
- Will most likely be a prediction problem

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
