# Recipe Minutes and Calorie investigation

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

Here I will be cleaning the data and renaming columns so that it is easier to understand. Before merging, I calculated the average rating for each recipes in the reviews dataframe and then we start by left merging the two dataframes on 'recipe_id'. After merging, we will need to fill in all missing values with 0 in order to avoid errors during the analysis. Additionally, I also corrected certain columns to their correct types. For example, the 'date' column contained values of dates presented in the string representation of 'mm/dd/yyyy' format. More importantly, I split the nutrition column into 7 seperate float columns correspinding tho their respective nutritional value. I have also changed some column names to make them easier to reference. 

In my introduction, I have discussed categorizing the recipes into two categories 'short' and 'long'. First we need to define what counts as a 'short' recipe. From the tags columns, we see a tag that says "60-minutes-or-less". I will use this as a threshold to seperate recipes between short and long. Recipes that take less than or equal to 60 minutes will be categorized as 'short' and other recipes will be categorized as 'long'.

The 'minutes' column was renamed to 'time' and a 'time_category' column was added to the dataframe.  Since we are only look at recipes, we can remove all duplicate recipes that came from merging the two dataframes. The head of the resulting cleaned dataframe is displayed below (only important columns are shown for display).

| name                                 |     id |   time |   n_steps |   n_ingredients |         calories | time_category |
|:-------------------------------------|-------:|-------:|----------:|----------------:|-----------------:| -------------:|
| 1 brownies in the world    best ever | 333281 |     40 |        10 |               9 |            138.4 |         short |
| 1 in canada chocolate chip cookies   | 453467 |     45 |        12 |              11 |            595.1 |         short |
| 412 broccoli casserole               | 306168 |     40 |         6 |               9 |            194.8 |         short |
| millionaire pound cake               | 286009 |    120 |         7 |               7 |            878.3 |          long | 
| 2000 meatloaf                        | 475785 |     90 |        17 |              13 |            267.0 |          long |

### Exploratory Data Analysis

#### Univariate Analysis

Here we first analyse the distribution for the time taken for each recipe ('minutes').

<iframe src="assets/uni_fig1.html" width=400 height=300 frameBorder=0></iframe>

In this distribution, we see that there is a peak when the time taken to cook is between 30-40 (exclusive) minutes. I decided to zoom into the range 0-135 minutes due to extreme outliers. It is also interesting to note that the plot is skewed to the right, with more recipes taking less than 60 minutes to make. In order to take into consideration for outliers, I have decided to only consider values within the interquartile range of 'time'. 

Now we want to take a look at the distribution of calories using the original cleaned dataframe that includes all the outliers. 

<iframe src="assets/uni_fig2.html" width=400 height=300 frameBorder=0></iframe>

This graph is also zoomed in as a result of extreme outliers, but we can observe that the graph of this is also skewed to the right. Again in order to take into consideration for outliers, I have decided to omit outliers that lie outside of the interquartile range of 'calories'.

#### Bivariate Analysis

For the bivariate analysis, I decided to remove all outliers (discussed in the univariate analysis) by only keeping the values that are within the interquartile range of both the 'time' column as well as the 'calorie' column.

First, I will be doing a bivariate analysis between time and calories.

<iframe src="assets/biv_fig1.html" width=400 height=300 frameBorder=0></iframe>

Correlation = 0.23

When looking at this figure where we plot time against calories, we can see very clearly that there is a very weak relationship between the two variables. Calculating the correlation between the two columns, we find a weak positive correlation. 

<iframe src="assets/biv_fig2.html" width=400 height=300 frameBorder=0></iframe>

In the box plot observed above, if we compare the median calories between short and long recipes. We find that the medium number of calories in short recipes is less than the medium number of calories in long recipes. We also observe that both the Q1 and Q3 for short is less than the Q1 and Q3 respectively. This may suggest that there is a possible difference between the calories in short recipes compared to the calories in long recipes.

#### Interesting Aggregates

In the aggregate analysis, we will be using the cleaned dataframe again. For this analysis, we will look at the number of ingredients with number of steps.

I have created a pivot table indexed by n_steps with mean calories as its values and n_ingredients as the columns. The head of the pivot table is displayed below for n_ingredients up to 5.

|     n_steps |       1 |          2 |          3 |          4 |          5 |
|------------:|--------:|-----------:|-----------:|-----------:|-----------:|
|           1 |     0.0 | 213.295082 | 245.167742 | 225.876555 | 324.290960 |
|           2 | 1851.75 | 222.759770 | 238.465493 | 246.966738 | 266.345312 |
|           3 |  201.20 | 266.496460 | 251.255313 | 270.967343 | 266.345312 |
|           4 |  284.80 | 394.714851 | 237.226948 | 385.776799 | 328.932575 |
|           5 |  199.50 | 303.427848 | 358.209797 | 314.203442 | 346.244974 |

Without considering outliers, we can see a slight increase in the average calories whenever the number of ingredients increase and the number of steps increase. Meaning it is possible that the number of calories increase with the number of ingredients used as well as the number of steps in the recipe. Taking a closer look, we want to create a pivot table where the index is 'time_category'.

|     time_category |       1 |          2 |          3 |          4 |          5 |
|------------------:|--------:|-----------:|-----------:|-----------:|-----------:|
|              long |   409.4 | 491.073451 | 458.328671 | 490.813129 | 458.726497 |
|             short |   797.9 | 303.628233 | 284.937500 | 314.079694 | 328.272583 |	

In this second pivot table generated, it is interesting to note that while the average number of calories for long recipes increase gradually as the number of ingredients increase, the number of calories for short recipes fluctuate as the number of ingredients increase. This may be due to extreme outliers affecting our data. Therefore, we will look at this pivot table again with our dataframe that was filtered by interquartile ranges.

|     time_category |            1 |          2 |          3 |          4 |          5 |
|------------------:|-------------:|-----------:|-----------:|-----------:|-----------:|
|              long |   175.100000 | 310.760976 | 250.679464 | 294.205323 | 327.841152 |
|             short |   230.055556 | 196.928333 | 212.929919 | 232.518780 | 248.952842 |	

Although not significantly better, we are able to observe a slight increasing trend for both long and short recipes. 

---

## Assessment of Missingness

In this section, we will examine the missingness on the original merged dataframe. 

### NMAR Analysis

There is a column in my merged dataframe that is potentially NMAR. Since the description section could have been optional when a user inputs a recipe, therefore the user may choose not to put a description. However, the missingness in this column may also be due to the recipe being simple as is or if the recipe is self explanatory (usually associated with the difficulty of the recipe) so the user who uploaded the recipe may choose not to include a description for the recipe as they may not feel like there is a need to further elaborate on a already simple recipe. Therefore, if the missingness of the description column may depend on the complexity of the recipe, we can say that the missingness of the column is MAR (Missing at Random).

### Missingness Dependency

In this section, we will examine the missingness of the column 'rating' in the merged dataframe and test the dependency of this column with other columns. It is important to note that the original CSV file filled in these missing variables with 0. When I merged the dataframes, I changed the 0s to NaN as the ratings can only be from 1-5 so if a row has the rating of 0, this usually means that the user who reviewed the recipe did not fill in the rating. 

I have chosen two columns to test the dependency of 'rating' on, these columns will be minutes and n_ingredients.

#### Missingness dependency of rating with minutes

Null Hypothesis: The missingness of food rating does not depend on minutes 

Alternative Hypothesis: The missingness of food rating does depend on minutes

First I create a new column with values that is True if the row in rating is missing else False. To do simulations, I shuffled the minutes column using the np.random.permutation function. Since we are comparing two quantitative distribuations, we calculate the difference in group means. We then append this difference for each individual simulation (simulated 1000 times).

<iframe src="assets/compare_dist1.html" width=400 height=300 frameBorder=0></iframe>

Observed Difference: 51.45

P-value = 0.125

Using a significance level of 0.05, we can conclude that we fail to reject the null hypothesis since our p-value of 0.111 > 0.05. Therefore we say that the missingness in rating is NOT dependent on the minutes column.

<iframe src="assets/missing_fig1.html" width=400 height=300 frameBorder=0></iframe>

#### Missingness dependency of rating with n_ingredients

Null Hypothesis: The missingness of food rating does not depend on n_ingredients

Alternative Hypothesis: The missingness of food rating does depend on n_ingredients

<iframe src="assets/compare_dist2.html" width=400 height=300 frameBorder=0></iframe>

Observed Difference: 0.16

P-value = 0.0

Using a significance level of 0.05, we can conclude that we reject the null hypothesis since our p-value of 0.0 < 0.05. Therefore we say that the missingness in rating is in fact dependent on the n_ingredients column.

<iframe src="assets/missing_fig2.html" width=400 height=300 frameBorder=0></iframe>

---

## Hypothesis Testing

### Permutation Testing

The research question for this investigation is, do shorter recipes contain less calories than longer recipes? In this test, we decide short and long recipes by the 60-minutes or less tag, meaning recipes that take 60 minutes or less will be categorized as 'short' otherwise they will be labelled 'long'. In order to test this, we will be running a permutation test. 

Null Hypothesis (H0): Short and long recipes have the same amount of calories.

Alternative Hypothesis (H1): Short recipes have less calories than long recipes.

Since previously, I have decided to filter out the cleaned dataframe by removing outliers using the interquartile range. In order to test this hypothesis, I will be using the without_outliers dataframe. The head of the without_outliers dataframe without only the columns 'time_category' and 'calories' is displayed below.

| time_category | calories |
|--------------:|---------:|
|         short |    138.4 |
|         short |    595.1 |
|         short |    194.8 |
|          long |    878.3 |
|          long |    267.0 |

Here are the observed means for each category.

| time_category |   calories |
|--------------:|-----------:|
|          long | 390.776169 |
|         short | 312.339553 |

For our test statistic, we will be obtaining the difference in group means. Additionally, we will be using a significance level of 0.05.

Observed Difference in Means: 78.44

<iframe src="assets/hypothesis_fig.html" width=400 height=300 frameBorder=0></iframe>

P-value = 0.0

A permutation test is run 1000 times. Using a significance level of 0.05, we can conclude that we reject the null hypothesis since our p-value of 0.0 < 0.05 and accept our alternative hypothesis that short recipes have less calories than long recipes.

### Conclusion

This result may seem reasonable since shorter recipes usually require less ingredients which means the recipes may contain less calories while longer recipes usually require more ingredients as there are usually more steps meaning that the recipe may generally contain more calories than shorter recipes. However, since I have filtered out the dataframe by removing outliers, we can not completely say that short recipes contain less calories than long recipes. Furthermore, there were around 10,000 outliers removed as a result of using only values that sit within the interquartile range of both 'time' and 'calories' therefore we are picking data that makes it easier to perform hypothesis testing which may result in the result being biased. That being said, from this permutation test, we can observe that short recipes contain less calories than long recipes.

---