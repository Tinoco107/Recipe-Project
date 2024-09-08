
# Exploration on the relationship between protein and rating of recipes

by Angel Tinoco (atinoco@ucsd.edu)


---

## Introduction

In a world where there is a growing trend towards health and fitness on social media, there’s a fascinating question about whether people rate high-protein recipes more favorably due to perceived health benefits. As fitness influencers and health advocates increasingly emphasize the importance of protein for muscle growth and overall wellness, it’s plausible that individuals might rate these recipes higher, driven by the belief that they contribute to a healthier lifestyle. This trend suggests that social media's focus on fitness could shape not just our eating habits but also our subjective evaluations of what constitutes a "good" recipe, intertwining perceptions of health with culinary preferences. With this in mind, i want to investigate the relationship between rating and the amount of protein present in the recipe. To do so, im am analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com. The original purpose of the datasets is for the recommender system research paper, Generating Personalized Recipes from Historical User Preferences by Majumder et al.

The first dataset, *recipes*,  contains 83782 rows, each row representing a unique recipe, with 12 columns outlined below:

| Column          | Description                                           |
|-----------------|-------------------------------------------------------|
| '**name**'          | Recipe name                                           |
| '**id**'            | Recipe ID                                             |
| '**minutes**'       | Minutes to prepare recipe                             |
| '**contributor_id**'| User ID who submitted this recipe                     |
| '**submitted**'     | Date recipe was submitted                             |
| '**tags**'          | Food.com tags for recipe                              |
| '**nutrition**'     | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”   |
| '**n_steps**'       | Number of steps in recipe                             |
| '**steps**'         | Text for recipe steps, in order                       |
| '**description**'   | User-provided description                             |
| '**ingredients**'   | Text for recipe ingredients                           |
| '**n_ingredients**' | Number of ingredients in recipe                       |


the second dataset, *interactions*, contains 731927 rows, each corisponding to a review by a user on a specfic recipe. the five columns are described as follows: 

| Column      | Description            |
|-------------|------------------------|
| '**user_id**'   | User ID                |
| '**recipe_id**' | Recipe ID              |
| '**date**'      | Date of interaction    |
| '**rating**'    | Rating given           |
| '**review**'    | Review text            |

 **im going to explore whether people rate protein dense recipes and non-protein dense recipes equally**. to do this im going to break down the values of the **nutrition** column into their own respective columns. im also going to calculate the proportion of protein in terms of the calories of a given recipe and store the information in a new column, "**prop_protein**". protein dense in this context will be referring to recipes with a value of **prop_protein** greater than the average value of **prop_protein**. the columns that will be most useful in answering my question will be **calories (#)**, **protein (PDV)**, **prop_protein** and **average_rating** 

---

## Cleaning and EDA

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---
