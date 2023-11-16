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
|`'nutrition'`	          |Nutritional information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)] (PDV - “percentage of daily value”)|
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

In this section, we will examine the missingness on the original merged dataframe. 

### NMAR Analysis

There is a column in my merged dataframe that is potentially NMAR. Since the description section could have been optional when a user inputs a recipe, therefore the user may choose not to put a description. However, the missingness in this column may also be due to the recipe being simple as is or if the recipe is self explanatory (usually associated with the difficulty of the recipe) so the user who uploaded the recipe may choose not to include a description for the recipe as they may not feel like there is a need to further elaborate on a already simple recipe. Therefore, if the missingness of the description column may depend on the complexity of the recipe, we can say that the missingness of the column is MAR (Missing at Random).

### Missingness Dependency

In this section, we will examine the missingness of the column 'rating' in the merged dataframe and test the dependency of this column with other columns. It is important to note that the original CSV file filled in these missing variables with 0. When I merged the dataframes, I changed the 0s to NaN as the ratings can only be from 1-5 so if a row has the rating of 0, this usually means that the user who reviewed the recipe did not fill in the rating. 

I have chosen two columns to test the dependency of 'rating' on, these columns will be minutes and n_ingredients.

Null Hypothesis: The missingness of food rating does not depend on minutes 

Alternative Hypothesis: The missingness of food rating does depend on minutes

First I create a new column with values that is True if the row in rating is missing else False. To do simulations, I shuffled the minutes column using the np.random.permutation function. Since we are comparing two quantitative distribuations, we calculate the difference in group means. We then append this difference for each individual simulation (simulated 1000 times).

// display compare_dist1 graph // 

Observed Difference: 51.45

P-value = 0.111

Using a significance level of 0.05, we can conclude that we fail to reject the null hypothesis since our p-value of 0.111 > 0.05. Therefore we say that the missingness in rating is NOT dependent on the minutes column.

// display missing_fig1 graph //

Null Hypothesis: The missingness of food rating does not depend on n_ingredients

Alternative Hypothesis: The missingness of food rating does depend on n_ingredients

// display compare_dist2 graph // 

Observed Difference: 0.16

P-value = 0.0

Using a significance level of 0.05, we can conclude that we reject the null hypothesis since our p-value of 0.0 < 0.05. Therefore we say that the missingness in rating is in fact dependent on the n_ingredients column.

// display missing_fig2 graph //

---

## Hypothesis Testing

The research question for this investigation is, do shorter recipes contain less calories than longer recipes? In this test, we decide short and long recipes by the 60-minutes or less tag, meaning recipes that take 60 minutes or less will be categorized as 'short' otherwise they will be labelled 'long'. In order to test this, we will be running a permutation test. 

Null Hypothesis (H0): Short and long recipes have the same amount of calories.

Alternative Hypothesis (H1): Short recipes have less calories than long recipes.

Since previously, I have decided to filter out the cleaned dataframe by removing outliers using the interquartile range. In order to test this hypothesis, I will be using the without_outliers dataframe.

// show hypothesis_testing df head //

// show observed_means df //

For our test statistic, we will be obtaining the difference in group means. Additionally, we will be using a significance level of 0.05.

Observed Difference in Means: 78.44

// display permutation test graph //

P-value = 0.0

A permutation test is run 1000 times. Using a significance level of 0.05, we can conclude that we reject the null hypothesis since our p-value of 0.0 < 0.05 and accept our alternative hypothesis that short recipes have less calories than long recipes.

This result may seem reasonable since shorter recipes usually require less ingredients which means the recipes may contain less calories while longer recipes usually require more ingredients as there are usually more steps meaning that the recipe may generally contain more calories than shorter recipes. However, since I have filtered out the dataframe by removing outliers, we can not completely say that short recipes contain less calories than long recipes. Furthermore, there were around 10,000 outliers removed as a result of using only values that sit within the interquartile range of both 'time' and 'calories' therefore we are picking data that makes it easier to perform hypothesis testing which may result in the result being biased. That being said, from this permutation test, we can observe that short recipes contain less calories than long recipes.

---