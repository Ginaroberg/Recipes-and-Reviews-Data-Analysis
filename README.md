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

### NMAR - INSERT COLUMN

State whether you believe there is a column in your dataset that is NMAR. Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR). Make sure to explicitly use the term “NMAR.”

### Missingness of Ratings Column

#### Distribution of Minutes when Rating is Missing 

<iframe src="assets/na_ratings_hist.html" width=800 height=600 frameBorder=0></iframe>


#### Distribution of Minutes when Rating is Not Missing

<iframe src="assets/nona_ratings_hist.html" width=800 height=600 frameBorder=0></iframe>

#### Permutation Test - Ratings and Minutes 

To see if these two samples come from the same distribution, we conducted a permutation test, using signed difference in mean minutes for our test statistic. 

Observed Statistic = 51.46

p-value = 0.122

<iframe src="assets/emp_dist_diff_min.html" width=800 height=600 frameBorder=0></iframe>

#### Permutation Test - Ratings and N_steps

Present and interpret the results of your missingness permutation tests with respect to your data and question. Embed a plotly plot related to your missingness exploration; ideas include:
• The distribution of column 
Y
 when column 
X
 is missing and the distribution of column 
Y
 when column 
X
 is not missing, as was done in Lecture 12.
• The empirical distribution of the test statistic used in one of your permutation tests, along with the observed statistic.

## Hypothesis Testing

### Question - What is the relationship between the cooking time and average rating of recipes?


### Null Hypothesis - Cooking time and average rating of a recipe are related.

### Alternative Hypothesis - Cooking time and average rating of a recipe are not related.

If the alternative hypothesis is "A and B are different", then the test statistic should measure distances and should contain an absolute value.

The null hypothesis states that no matter what cooking time a recipe has, all recipes should have similar average rating of recipes. For this test we are going to use all recipes with a cooking time between 20 and 30 minutes (inclusive). 

That is, if we repeatedly sampled groups of 52227 recipes from the population and computed their mean rating, it would not be uncommon to see an average seen above.

Plan: Repeatedly sample (without replacement) 52227 cooking recipes from the population and compute their average rating, and see where the 20-30 minutes cooking time average rating lies in this distribution.
Average rating is our test statistic. We will be comparing the simulated test statistics with the observed test statistc to calculate the p-value with a 5% significance level to determine whether we reject the null hypothesis or fail to reject the null hypothesis.

#### 1. Filtered the dataframe to the necessary columns, which are minutes and ratings

|    |   minutes |   rating |
|---:|----------:|---------:|
|  9 |        20 |        4 |
| 48 |        30 |        5 |
| 49 |        30 |        5 |
| 50 |        30 |        4 |
| 51 |        30 |        4 |
| 52 |        30 |        5 |
| 56 |        30 |        5 |
| 58 |        25 |        4 |
| 59 |        25 |        5 |
| 72 |        30 |        2 |

#### 2. Simulated the Empirical Distribution following the Null Hypothesis

<iframe src="assets/emp_dist_avg_rating_ss.html" width=800 height=600 frameBorder=0></iframe>









Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting 
p
-value, and your conclusion. Justify why these choices are good choices for answering the question you are trying to answer.

Optional: Embed a visualization related to your hypothesis test in your website.

Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.

“Only a Sith deals in absolutes” - Obi-Wan Kenobi

