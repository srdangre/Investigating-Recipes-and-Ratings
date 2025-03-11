# Investigating-Recipes-and-Ratings
by Saloni Dangre
Final Project; DSC80

## Introduction:
- Diet is an important contributor to overall well being
- Talk about processed foods/introduction/overall negative impact
  >- Processed food tends to have high sodium, low potassium
  >- According to the FDA a food is 'high' in something if it is over 20% of the PDV
- People have less time to cook-> find research-> is this true?
- https://www.fda.gov/food/nutrition-facts-label/lows-and-highs-percent-daily-value-nutrition-facts-label#:~:text=The%20percent%20Daily%20Value%20(%25,or%20low%20in%20a%20nutrient.

- Do more simple recipies have higher ratings?
- Do more simple recipies have lower sodium?

## Data Cleaning and Exploratory Data Analysis
### Univariate Plots
- N_steps histogram
- Sodium level histogram
### Bivariate Plots
- High sodium vs low sodium n_steps, boxplot
### Interesting Aggregates
- Since Americans statistically consume too much sodium, I wanted to see if high sodium recipies were more highly rated

|   High |    Low |
|-------:|-------:|
|   1117 |   1753 |
|    967 |   1401 |
|   2887 |   4285 |
|  15159 |  22148 |
|  67712 | 101964 |

- There were more ratings for low sodium recipies to begin with. To get a better sense of how ratings were distributed among each category, the counts were divded by the total number of ratings per column

|   High_prop |   Low_prop |
|------------:|-----------:|
|   0.012716  |  0.0133256 |
|   0.0110084 |  0.0106499 |
|   0.0328658 |  0.0325729 |
|   0.172571  |  0.168361  |
|   0.770839  |  0.775091  |

- Looking at the proportions, high and low sodium levels have a similar distribution of ratings.
## Assessment of Missingness

## Hypothesis Testing
- Will most likely be a prediction problem

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
