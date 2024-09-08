
# Exploration on the relationship between protein and rating of recipes

by Angel Tinoco (atinoco@ucsd.edu)


---

## Introduction

In a world where there is a growing trend towards health and fitness on social media, there’s a 
fascinating question about whether people rate high-protein recipes more favorably due to perceived health benefits. As fitness influencers and health advocates increasingly emphasize the importance of protein for muscle growth and overall wellness, it’s plausible that individuals might rate these recipes higher, driven by the belief that they contribute to a healthier lifestyle. This trend suggests that social media's focus on fitness could shape not just our
eating habits but also our subjective evaluations of what constitutes a "good" recipe, intertwining perceptions of health with culinary preferences. With this in mind, i want to investigate the relationship between rating and the amount of protein present in the recipe. To do so, im am analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com. The original purpose of the datasets is for the recommender system research paper, Generating Personalized Recipes from Historical User Preferences by Majumder et al.

The first dataset, *recipes*,  contains 83782 rows, each row representing a unique recipe, with 12 columns outlined below:


| Column          | Description                                           |
|:-----------------|:-------------------------------------------------------|
| `'name'`          | Recipe name                                           |
| `'id'`            | Recipe ID                                             |
| `'minutes' `      | Minutes to prepare recipe                             |
| `'contributor_id'`| User ID who submitted this recipe                     |
| `'submitted'`     | Date recipe was submitted                             |
| `'tags'  `        | Food.com tags for recipe                              |
| `'nutrition' `    | Nutrition information in the form [calories (#), total fat (PDV),sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”  |
| `'n_steps'`       | Number of steps in recipe                             |
| `'steps'`        | Text for recipe steps, in order                       |
| `'description'`   | User-provided description                             |
| `'ingredients'`   | Text for recipe ingredients                           |
| `'n_ingredients'` | Number of ingredients in recipe                       |

the second dataset, *interactions*, contains 731927 rows, each corisponding to a review by a user on a specfic recipe. the five columns are described as follows: 

| Column      | Description            |
|:-------------|:------------------------|
| `'user_id' `  | User ID                |
|` 'recipe_id'` | Recipe ID              |
| `'date'  `    | Date of interaction    |
| `'rating' `   | Rating given           |
| `'review'`    | Review text            |


 **im going to explore whether people rate protein dense recipes and non-protein dense recipes equally**. to do this im going to break down the values of the **nutrition** column into their own respective columns. im also going to calculate the proportion of protein in terms of the calories of a given recipe and store the information in a new column, "**prop_protein**". protein dense in this context will be referring to recipes with a value of **prop_protein** greater than the average value of **prop_protein**. the columns that will be most useful in answering my question will be **calories (#)**, **protein (PDV)**, **prop_protein** and **average_rating** 

Understanding this dynamic can help food brands and health professionals tailor their messaging and product offerings to align with consumer values. It also highlights how social media trends can influence dietary preferences and nutritional choices, potentially guiding more effective public health strategies and marketing campaigns. By uncovering the motivations behind recipe ratings, we gain insight into how health perceptions shape food choices, which can lead to more informed and targeted approaches in promoting healthier eating habits.


---
## Data Cleaning and Exploratory Data Analysis

To make my analysis quicker and more convenient i conducted the following data cleaning steps:

1. Left merge the recipes and interactions datasets together
   - matches recipes to their ratings and reviews

1. Fill all ratings of 0 with np.nan
   - this makes initive sense becuase a score of 0 on a scale of 1 - 5 indicates that its a comment rather than a review and should not be included in the ratings 

1. Find the average rating per recipe

1. Add this Series containing the average rating per recipe back to the recipes dataset

1. Split the 'nutrition' column into separate components

1. Create high protein boolean column

1. Create prop_protein column and handles instances of 0 calorie or protein recipes

1. Update submitted column to be datetimes type

1. create protein dense column based of prop_protein being above the average

The cleaned dataframe ended up with 83782 rows and 23 columns. Here are the first 5 rows of ~unique recipes of the cleaned dataframe for illustration. Since there is a lot of columns in the dataframe, i selected the columns that are most relevant to our questions for display. Scroll right to view more columns.


| name                               |   id   |  minutes  |      submitted      | average_rating | calories (#) | protein (PDV) | prop_protein | is_protein_dense | 
|:-----------------------------------|-------:|----------:|:--------------------|---------------:|-------------:|--------------:|-------------:|:-----------------|
| 1 brownies in the world best ever  | 333281 |        40 | 2008-10-27 00:00:00 |            4.0 |        138.4 |           3.0 |         0.04 | False            |   
| 1 in canada chocolate chip cookies | 453467 |        45 | 2011-04-11 00:00:00 |            5.0 |        595.1 |          13.0 |         0.04 | False            | 
| 412 broccoli casserole             | 306168 |        40 | 2008-05-30 00:00:00 |            5.0 |        194.8 |          22.0 |         0.23 | True             | 
| millionaire pound cake             | 286009 |       120 | 2008-02-12 00:00:00 |            5.0 |        878.3 |          20.0 |         0.05 | False            |   
| 2000 meatloaf                      | 475785 |        90 | 2012-03-06 00:00:00 |            5.0 |        267.0 |          29.0 |         0.22 | True             |   


### Univariate analyses: distribution of protein among all recipes in dataframe 

<iframe
  src="assets/Univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### bivariate analyses: distribution of rating conditional on protein density in dataframe 


<iframe
  src="assets/bivariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Interesting Aggregates (proportion of protein by number of ingredients)


<iframe
  src="assets/Interesting Aggregates.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## Assessment of Missingness

columns with missing values are `'name'`, `'discription'`, `'average_ratings'`, and `'prop_protein'`

### NMAR Analysis

`'average_ratings'` is NMAR this makes intuitive sense, the average rating of any given recipe can be missing if there are no ratings to average. given that the original recipe is only a subset of data from 2008 and still has 83782 unique recipes it make perfect sense to for a subset of those recipes to have no ratings and therefore be missing a value for average rating. 


### Missingness Dependency



<iframe
  src="assets/prop_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We ran a permutation test by shuffling the missingness of rating for 1000 times to collect 1000 simulating mean differences in the two distributions as described in the test statistic.


<iframe
  src="assets/permutation1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

the observed statisitc of .010 is indicated by the red vertical dotted line. since the pvalue we found (0.0) is < than .05 we reject the null hyptohesis. meaning that the missingness for average rating is being effected by the column prop_protein 



<iframe
  src="assets/mins_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We ran another permutation test by shuffling the missingness of rating for 1000 times to collect 1000 simulating mean differences in the two distributions as described in the test statistic.


<iframe
  src="assets/permutation2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

the observed statisitc of 117.34 is indicated by the red vertical dotted line. since the pvalue we found (.031) is < than .05 we reject the null hyptohesis. meaning that the missingness for average rating is being effected by the column minutes 

---

## Hypothesis Testing

im going to explore whether people rate protein dense recipes and non-protein dense recipes equally


<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



#### conclusion of permutation test 

since the pvalue we found (0.0) is less than the the significance level of .05 we reject the null hypothesis meaning that people are not rating protein dense recipes the same as non protein dense recipes. a possibe reason for this is that outcome is that this trend of health consciuous recipes is a very recent trend and has not been adpoted on a mass scale. 

---


## Framing a Prediction Problem

**predict average_ratings of recipes**


i plan to predict average rating of a recipe which would be a classification problem since we can treat rating as a ordinal categorical variable if we round the average rating so that we only have [1, 2, 3, 4, 5] as possible values.

i choose average rating of recipe as a response varible becuase it does a good job of representing the overall rating of the recipe

i will be using the f1 score instead of accuracy since in our dataset, the rating distrubtion is heavily skewed towards the high end with the main concentration being around ratings of 4 and 5 

the infomation we have available prior to making our predictions is all the columns of the rating datasets which are listed in the introduction section. since all those columns are features, we would have acess to them regardless if they have been rated. 

---


## Baseline Model



---


## Final Model



---


## Fairness Analysis



---

