# Recipes-and-Reviews-Data-Analysis
### by William Kam and Gina Roberg

This was a research project for a course on the Practice and Application of Data Science at UCSD.  The data used was scraped from food.com.  This is a report of our findings.

## Introduction
The dataset we were provided consisted of recipes and ratings for the provided recipes, originally scraped from food.com.  With this data, our analysis was centered around the question of **"What is the relationship between the cooking time and average rating of recipes?"**  Since there are an extensive list of options when it comes to recipes to make, there are many factors that can come into consideration, like how long it will take to prepare, how much preparation is required, what supplies and ingredients are needed, and more.  If it is a new recipe that you haven't made before reviews and ratings are often helpful in narrowing down our choices and hearing from other people's experiences making the same dish.  However, how much correlation can there be between how much time must be invested to create a certain meal, and how well the meal usually turns out? Will investing more cooking time result in a better tasting meal? Are the ratings of these meals simply influenced by how much time and effort was put in? To answer these questions, we utilized our dataset. 


## Data cleaning
To clean the data we first started by converting all the string representations of list to an actual list representation in python. These columns that we needed to do this on were the tags and nutrition column. The nutrition column was generated from an image of the nutrition information and only numbers were extracted and combined into a list. Afterwards we then hot encoded the tags column and also encoded the nutrition column into their respective elements ie. calories, proteins, fat. In addition, we also converted the submitted column to a date time object. This data was collected by scraping food.com and most of the columns were collected as objects when they need to be something else ie. dates or lists instead of strings. 

### Recipes Data 
- 83781 rows × 12 columns

#### Relevant columns included: 
id
: unique string of numbers to identify each recipe

minutes
: integer value to convey how long it takes to prepare the recipe in minutes

n_steps
: integer value to convey how many steps are required to prepare the recipe


### Interactions (Ratings) Data
- 731927 rows × 5 columns

#### Relevant columns included:
recipe_id
: unique string of numbers corresponding to a recipe

rating
: values ranging from NaN, integers from 1 to 5 to represent the rating a user gives that recipe


## Cleaning and EDA
### Cleaning the Data 
1. Merged the Recipes and Interactions DataFrames by the recipe id columns
2. Dropped columns that were irrelevant or repeated
    - name, contributer id, submitted, steps, description, ingredients, user_id, recipe_id, review
3. Converted submitted and date column to a date time object
4. Converted tags and nutrition columns to lists
5. Added separate columns for each of the values in the nutrition lists and converted values to floats
6. Hot encoded tags column 
7. Reevaluated the new columns and ultimately only kept columns that appeared to be relevant to our question, which resulted in the following DataFrame

|    |   minutes |   n_steps |   rating |   avg_rating |
|---:|----------:|----------:|---------:|-------------:|
|  0 |        40 |        10 |        4 |            4 |
|  1 |        45 |        12 |        5 |            5 |
|  2 |        40 |         6 |        5 |            5 |
|  3 |        40 |         6 |        5 |            5 |
|  4 |        40 |         6 |        5 |            5 |


### Univariate Visualization 

#### Histogram of N_Steps Column 
The histogram of n_steps shows us that the most common steps for a recipe to have is 7 and that the most common number of steps for a recipe to have is between 3 and 11.

<iframe src="assets/n_steps_histogram.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Visualization 

#### Scatterplot of N_Steps by Minutes
Contrary to intuition, our scatter plot shows us that the number of steps is not necessarily correlated with more time to cook. Matter of fact, recipes with cooking times greater than 1000 minutes are more likely to be between 0-35 steps considering the maximum amount of n_steps is 100. 

<iframe src="assets/n_steps_minutes_scatter.html" width=800 height=600 frameBorder=0></iframe>

Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

### Interesting Aggregates 

#### Pivot Table 
This is a pivot table with the mean minutes and n_steps for recipes of each rating.  This is significant as it shouws the slight variation in averages between the average minutes and n_steps for recipes that fall into each rating category. From this, we can see there may be potential correlation between rating and time it takes in making recipes and can look into this further with testing.

|   rating |   minutes |   n_steps |
|---------:|----------:|----------:|
|        1 |   99.6725 |  10.63    |
|        2 |   98.0215 |  10.6976  |
|        3 |   87.4976 |   9.99205 |
|        4 |   91.585  |   9.57743 |
|        5 |  106.924  |   9.9849  |


## Assessment of Missingness

### NMAR - Description Column

A column that we believe is **NMAR** in this data set is the **description column**. We beleive this as when we look at the other columns there seems to be no correlation or additional information to reason about the missigness of the description column. Some additional information that could explain the missigness would be that if the name of the recipe was self explanatory or if the user who inputted the recipe is known to not add any descriptions when enterring the recipe. Another column we believe to be NMAR is review. For the reason column there seems to be no corrleation with the other columns and it seems as if the missigness of the value depends on itself. Some additional information that could help with this is the number of views per recipe or the popularity as that could be a variable that could be related to missigness of reviews. As some reviews have up to 10 reviews while others have none.

### Missingness of Reviews

#### Reviews and Sugar(NMAR not dependent) - Does sugar without reviews and sugar with  reviews come from the same distribution?

Null Hypothesis: The K-S statistic of sugar from recipes with reviews and mean sugar from recipes without reviews was drawn by random chance alone.

Alternative Hypothesis: The K-S statistic of sugar from recipes with reviews and mean sugar from recipes with reviews was not by random chance.

Since the two distributions of sugar, with and without reviews is quantitative (numerical), and based on the plotted histograms, they are centered in similar locations but have different shapes, we are using the Kologorov-Smirnov statistic.

#### Distribution of Sugar when Review is Missing 

<iframe src="assets/na_rev_sugar.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Minutes when Rating is Not Missing

<iframe src="assets/nona_rev_sugar.html" width=800 height=600 frameBorder=0></iframe>

#### Permutation Test - Review and Sugar 

Observed KS = 0.12425191323333018

p_val = 0.276

<iframe src="assets/emp_ks.html" width=800 height=600 frameBorder=0></iframe>

#### Conclusion
With 5% significance level, we reject the null hypothesis. The K-S statistic of minutes from recipes with ratings and recipes without ratings was not by random chance then the ratings column is MAR dependent on minutes.



#### Reviews and saturated fat (MAR is dependent)- Do saturated fat for recipes without reviews and saturated fat or recipes with  reviews come from the same distribution?

Null Hypothesis: The difference in the distribution of saturated fat  for recipes with reviews and the distribution of saturated fat  for recipes without reviews was drawn by random chance alone.

Alternative Hypothesis: The difference in the distribution of saturated fat  for recipes with reviews and the distribution of saturated fat  for reviews without ratings was not drawn by random chance alone.

Since the two distributions of saturated fat, with and without reviews is quantitative (numerical), and based on the plotted histograms, they have similar basic shapes but are centered at different locations with different means, we are using difference in group means as our test statistic.

#### Distribution of Saturated Fat when Review is Missing 

<iframe src="assets/na_rev_sat_fat.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Saturated Fat when Review is Not Missing

<iframe src="assets/nona_ratings_hist.html" width=800 height=600 frameBorder=0></iframe>

#### Permutation Test - Review and Saturated Fat

Observed Diff in Means = 33.9134865422171

p_val = 0.01

<iframe src="assets/rev_sat_fat.html" width=800 height=600 frameBorder=0></iframe>

#### Conclusion
With 5% significance level, we reject the null hypothesis. The K-S statistic of minutes from recipes with ratings and recipes without ratings was not by random chance then the ratings column is MAR dependent on minutes.



## Hypothesis Testing

### Question - What is the relationship between the cooking time and the average rating of recipes?

#### Null Hypothesis: Cooking time and average rating of recipes are not related
the ratings of the recipes with the greater cooking time were drawn uniformly at random from the population distribution

#### Alternative Hypothesis: Cooking time and average rating of recipes are related
the ratings of the recipes with the greater cooking time were not drawn at random from the population distribution

#### Plan
1. Create Table containg ranges of cooking times (minute_groups = ['0-19','20-39','40-59','60-79','80+']
2. Look at table and pick the cooking time range that contians the highest mean rating, whichever cooking range this is will be our sample that we are comparing to the population distribution 
3. Run simulation and repeatedly sample the number of recipes from the cooking time range we selected, without replacement, to create an empirical distribution, using mean rating as our test statistic
4. Calculate P-value
5. Draw a conclusion on whether we reject the null hypothesis or fail to reject the null hypothesis with a 5% significance level

#### 1. Filtered the dataframe to the necessary columns, which were minutes and ratings, and then added a new column to assign them to minute groups

|    |   minutes |   rating | range of cooking time   |
|---:|----------:|---------:|:------------------------|
|  0 |        40 |        4 | 40-59                   |
|  1 |        45 |        5 | 40-59                   |
|  2 |        40 |        5 | 40-59                   |
|  3 |        40 |        5 | 40-59                   |
|  4 |        40 |        5 | 40-59                   |
|  5 |        40 |        5 | 40-59                   |
|  6 |       120 |        5 | 80+                     |
|  7 |        90 |        5 | 80-100                  |
|  8 |        90 |        5 | 80-100                  |
|  9 |        20 |        4 | 20-39                   |

#### 2. Created a grouped table to analyze the means for each cooking time group
From this table we noticed that for the cooking time range of 0-19 minutes, this group had the highest average rating of 4.72.  We chose this group as our sample.

| range of cooking time   |   minutes |   rating |   # of recipes |
|:------------------------|----------:|---------:|---------------:|
| 0-19                    |   9.61295 |  4.71693 |          49546 |
| 20-39                   |  27.3994  |  4.67881 |          67483 |
| 40-59                   |  46.2631  |  4.66724 |          41102 |
| 60-79                   |  66.4603  |  4.6764  |          25470 |
| 80+                     |  410.2530 |  4.64749 |          35790 |

#### 3. We simulated under the null hypothesis to create an empirical distribution, by sampling 49546 recipes, without replacement, from the population, and calculated the average rating of each sample.  Here are our first 10 samples:
 - array([4.67892464, 4.68360715, 4.68209341, 4.67726961, 4.6839099 , 4.67422194, 4.67757236, 4.67617971, 4.68162919, 4.67688613])

#### 4. With our simulated test statistics, we compared them to our observed test statistic of and average rating of 4.71693, and calculated the p-value, which ended up being 0.0

<iframe src="assets/httest-emp-hist.html" width=800 height=600 frameBorder=0></iframe>

#### 5. At the 5% significance level, we reject the null hypothesis and conclude that there could possibly be a correlation between cooking time and rating of recipes.


