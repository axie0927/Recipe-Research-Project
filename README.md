# Recipe Time and Calorie investigation

This is Project 3 for DSC80

By Ammie Xie

---

## Introduction

With the increase in reliance on the internet in recent years, it has become increasingly easy to learn skills based on advice obtained online. In particular, the internet has made food recipes extremely accessible, and one can learn the culinary skills of different cultures and types of food without even having to go to culinary school. In this project, I aim to investigate a dataset containing multiple recipes as well as the nutritional statistics in the recipe and try to uncover hidden trends with the information obtained in this dataset. 

The datasets I will be using are obtained from food.com containing different recipes as well as reviews by users on the website reviewing the recipes collected from 2008. The first dataset contains 83782 rows, and 12 columns. The columns of the dataframe is presented below as well as a description of the columns. 

|Column	                 |Description|
|---                     |---        |
|`'name'	`            |Recipe name|
|`'id'`	                 |Recipe ID|
|`'minutes'`	         |Time taken to prepare the recipe (Minutes)|
|`'contributor_id'`	     |User ID who submitted this recipe|
|`'submitted'`	            | Date recipe was submitted|
|`'tags'`	              |Tags used to describe the recipe|
|`'nutrition'`	          |Nutritional information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”|
|`'n_steps'`	          |Number of steps in recipe|
|`'steps'`	              |Step descriptions in step order|
|`'description'`	     | User-provided description|
|`'ingredients'`	     | Ingredients used in recipe|
|`'n_ingredients'`	     | Number of ingredients used in recipe|


For our second dataset, we used a dataset containing individual user reviews for recipes. This dataframe contains 731927 rows and 5 columns. The columns of the dataframe is presented below as well as a description of the columns.

|Column|Description|
|---|---|
|`'user_id'`	|User ID|
|`'recipe_id'`	|Recipe ID|
|`'date'`	|Date that the review was posted|
|`'rating'`	|Rating given to the recipe reviewed|
|`'review'`	|Review given by user in regards to the recipe|

For this investigation, I mainly looked at the 'minutes' column as well as the 'nutrition' column. Since the nutrition column contains the nutritional information of the recipe in the form of [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)], we would need to split up this column since we only need to look at the calories of the recipe. Additionally, since there is a wide range of minutes, for this investigation I have decided to split minutes into two categorical definitions 'short' and 'long' recipes. This will be explained in further detail later on. 

---

## Cleaning and EDA

### Data Cleaning

Here I will be cleaning the data and renaming columns so that it is easier to understand. First we start by left merging the two dataframes on 'recipe_id'. After merging, we will need to fill in all missing values with 0 in order to avoid errors during the analysis. I decided to



---

## Assessment of Missingness

---

## Hypothesis Testing

---