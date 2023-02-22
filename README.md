# Recipes-and-Reviews-Data-Analysis
### by William Kam and Gina Roberg

This was a research project for a course on the Practice and Application of Data Science at UCSD.  The data used was scraped from food.com.  This is a report of our findings.

## Introduction
The dataset we were provided consisted of recipes and ratings for the provided recipes, originally scraped from food.com.  With this data, our analysis was centered around the question of **"What is the relationship between the cooking time and average rating of recipes?"**  Since there are an extensive list of options when it comes to recipes to make, there are many factors that can come into consideration, like how long it will take to prepare, how much preparation is required, what supplies and ingredients are needed, and more.  If it is a new recipe that you haven't made before reviews and ratings are often helpful in narrowing down our choices and hearing from other people's experiences making the same dish.  However, how much correlation can there be between how much time must be invested to create a certain meal, and how well the meal usually turns out? Will investing more cooking time result in a better tasting meal? Are the ratings of these meals simply influenced by how much time and effort was put in? To answer these questions, we utilized our dataset. 

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

Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).

Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

<iframe src="/Users/ginaroberg/Desktop/DSC 80/03-report/Recipes-and-Reviews-Data-Analysis/n_steps_histogram.html" width=800 height=600 frameBorder=0></iframe>


Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

Embed at least one grouped table or pivot table in your website and explain its significance.

## Assessment of Missingness

State whether you believe there is a column in your dataset that is NMAR. Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR). Make sure to explicitly use the term “NMAR.”

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
Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting 
p
-value, and your conclusion. Justify why these choices are good choices for answering the question you are trying to answer.

Optional: Embed a visualization related to your hypothesis test in your website.

Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.

“Only a Sith deals in absolutes” - Obi-Wan Kenobi

