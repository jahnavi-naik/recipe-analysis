# Predicting Recipe Rating based on Recipe Length
Author: Jahnavi Naikqw

## Introduction
Food is an important aspect of everyone's life, especially mine. Food is not just a necessity, but cooking and baking is a hobby and profession for many. Naturally, food.com becomes a prominent website for finding recipes for a variety of dishes, and even allows you to leave reviews and ratings, helping others determine what recipes to try. When looking for a recipe to make, many home cooks value the time it takes to actually make a recipe, to fit it in their busy schedules. Because of this, I think it is important to understand the relationship between the length of the recipe, and its rating, so I decided to focus on the question, **How does the length of the recipe/ ingredients affect the ratings of the recipe?** 
For this project, obtained 2 datasets, originally taken from food.com. The first dataset, called recipes, contains 12 columns, and 83782 rows. The second dataset, called reviews, contains 5 columns and 731927 rows. 

The columns in the recipe dataframe that are relevant to my focus are:
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Column</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>The recipe ID, which is unique per recipe</td>
    </tr>
    <tr>
      <td>minutes</td>
      <td>The number of minutes it takes to complete a recipe</td>
    </tr>
    <tr>
      <td>nutrition</td>
      <td>A string (that looks like a list) of various nutrition facts including calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)</td>
    </tr>
    <tr>
      <td>n_steps</td>
      <td>The number of steps in the recipe</td>
    </tr>
    <tr>
      <td>ingredients</td>
      <td>A string (that looks like a list) of ingredients used in the recipe</td>
    </tr>
    <tr>
      <td>n_ingredients</td>
      <td>The number of ingredients used in the recipe</td>
    </tr>
  </tbody>
</table>

The columns of the reviews dataframe that are relevant to my focus are:
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Column</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>recipe_id</td>
      <td>the recipe id, matching the id column in the recipes dataframe</td>
    </tr>
    <tr>
      <td>rating</td>
      <td>the rating given by the reviewer on a 1 - 5 scale</td>
    </tr>
  </tbody>
</table>

## Data Cleaning and Exploratory Analysis
I started by merging these two dataframes on the recipe id, in order to get a dataframe that contains all of the reviews of all the recipes. After merging, I found that some of the ratings were missing, and decided to fill them with 0 in order to avoid bias.

Next, I decided to create a new column in the dataset for the average rating. The current dataframe has more than one row for each review, so it is necessary to get the average rating for each recipe to get an understanding of the overall assessment, as it will be needed for future analysis.

I then went on to clean the individual columns of the Dataframe. I first focused on the nutrition column. This column contains a list of many different nutrition facts, and I decided that splitting this column up to have a column for each nutrition fact would be more useful for my analysis. Although this column looks like a list, it is actually a string, so I cleaned it by removing the brackets and then splitting it by the comma, in order to turn it into a list. I was then able to split this column of lists into multiple columns that each represented a nutrition fact.

Lastly, I created a new column called has_sugar. To do this, I applied a function to the ingredients column to check if the word 'sugar' was in the string, returning boolean values in this column. Because sugar is a popular topic when it comes to food, I thought this column would be useful in my later prediction model.

The final, cleaned, dataframe looks something like this:
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>name</th>
      <th>id</th>
      <th>minutes</th>
      <th>n_steps</th>
      <th>n_ingredients</th>
      <th>calories (#)</th>
      <th>has_sugar</th>
      <th>avg_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1 brownies in the world    best ever</td>
      <td>333281</td>
      <td>40</td>
      <td>10</td>
      <td>9</td>
      <td>138.4</td>
      <td>True</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>1 in canada chocolate chip cookies</td>
      <td>453467</td>
      <td>45</td>
      <td>12</td>
      <td>11</td>
      <td>595.1</td>
      <td>True</td>
      <td>5.0</td>
    </tr>
    <tr>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>6</td>
      <td>9</td>
      <td>194.8</td>
      <td>False</td>
      <td>5.0</td>
    </tr>
    <tr>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>6</td>
      <td>9</td>
      <td>194.8</td>
      <td>False</td>
      <td>5.0</td>
    </tr>
    <tr>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>6</td>
      <td>9</td>
      <td>194.8</td>
      <td>False</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>

### Univariate Analysis

To start my exploration of the data I was working with, I created a histogram to visualize the distribution of the number of steps in the recipe. The number of steps in this dataframe ranges from 1 to 100 steps. The graph shows that they data is skewed to the right, and that most recipes have between 5-9 steps.

<iframe
  src="assets/univariate-n_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I also decided to create a histogram for the distribution of the minutes it takes to do the recipes in the dataset. The distribution is skewed to the right with a very long tail, making the graph unreadable. To combat this, I decided to remove the outliers that I determined using the outlier test. THe average minutes it takes for a recipe in this dataset is 36 minutes.

<iframe
  src="assets/univariate-minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Anaylsis

I then decided to look at the relationship between pairs of variables, specifically the relationship between the average rating and the number of steps in the recipe. The scatter plot below shows that there seems to be a somewhat positive relationship between these variabes, meaning as the recipes with a higher average rating tend to have more steps.

<iframe
  src="assets/n_steps-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Aggregates

The pivot table below shows the mean, median, and mode of the number of calories per the number of ingredients in the recipe. Outliers were removed using the IQR rule. THe pivot table shows that as the number of ingredients increases, the number of calories of the recipe also increases, show a positive relationship between the two variables.

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>mean</th>
      <th>median</th>
      <th>std</th>
    </tr>
    <tr>
      <th></th>
      <th>calories (#)</th>
      <th>calories (#)</th>
      <th>calories (#)</th>
    </tr>
    <tr>
      <th>n_ingredients</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>157.229630</td>
      <td>144.20</td>
      <td>113.179463</td>
    </tr>
    <tr>
      <th>2</th>
      <td>212.604051</td>
      <td>144.70</td>
      <td>205.470241</td>
    </tr>
    <tr>
      <th>3</th>
      <td>215.911121</td>
      <td>163.60</td>
      <td>191.614204</td>
    </tr>
    <tr>
      <th>4</th>
      <td>237.279823</td>
      <td>183.60</td>
      <td>193.219294</td>
    </tr>
    <tr>
      <th>5</th>
      <td>262.716944</td>
      <td>219.00</td>
      <td>191.958962</td>
    </tr>
    <tr>
      <th>6</th>
      <td>285.390661</td>
      <td>239.90</td>
      <td>207.755204</td>
    </tr>
    <tr>
      <th>7</th>
      <td>302.350313</td>
      <td>252.20</td>
      <td>205.388476</td>
    </tr>
    <tr>
      <th>8</th>
      <td>318.771192</td>
      <td>275.20</td>
      <td>204.539190</td>
    </tr>
    <tr>
      <th>9</th>
      <td>336.953357</td>
      <td>294.90</td>
      <td>207.894473</td>
    </tr>
    <tr>
      <th>10</th>
      <td>341.251078</td>
      <td>304.10</td>
      <td>201.831227</td>
    </tr>
    <tr>
      <th>11</th>
      <td>363.584400</td>
      <td>324.80</td>
      <td>207.621016</td>
    </tr>
    <tr>
      <th>12</th>
      <td>364.438348</td>
      <td>326.40</td>
      <td>202.957711</td>
    </tr>
    <tr>
      <th>13</th>
      <td>384.200156</td>
      <td>359.50</td>
      <td>205.592255</td>
    </tr>
    <tr>
      <th>14</th>
      <td>402.556815</td>
      <td>374.30</td>
      <td>211.273798</td>
    </tr>
    <tr>
      <th>15</th>
      <td>422.722760</td>
      <td>392.50</td>
      <td>215.983254</td>
    </tr>
    <tr>
      <th>16</th>
      <td>433.215455</td>
      <td>416.70</td>
      <td>210.721340</td>
    </tr>
    <tr>
      <th>17</th>
      <td>458.555769</td>
      <td>419.70</td>
      <td>216.732349</td>
    </tr>
    <tr>
      <th>18</th>
      <td>469.239942</td>
      <td>440.20</td>
      <td>215.431545</td>
    </tr>
    <tr>
      <th>19</th>
      <td>469.590974</td>
      <td>439.10</td>
      <td>224.048137</td>
    </tr>
    <tr>
      <th>20</th>
      <td>498.537207</td>
      <td>452.70</td>
      <td>237.817307</td>
    </tr>
    <tr>
      <th>21</th>
      <td>452.178998</td>
      <td>416.80</td>
      <td>218.114314</td>
    </tr>
    <tr>
      <th>22</th>
      <td>572.306952</td>
      <td>620.50</td>
      <td>242.775594</td>
    </tr>
    <tr>
      <th>23</th>
      <td>476.350549</td>
      <td>471.10</td>
      <td>211.118148</td>
    </tr>
    <tr>
      <th>24</th>
      <td>505.260976</td>
      <td>454.50</td>
      <td>214.630693</td>
    </tr>
    <tr>
      <th>25</th>
      <td>540.551163</td>
      <td>606.40</td>
      <td>271.928627</td>
    </tr>
    <tr>
      <th>26</th>
      <td>544.987273</td>
      <td>572.70</td>
      <td>178.086653</td>
    </tr>
    <tr>
      <th>27</th>
      <td>583.072222</td>
      <td>589.85</td>
      <td>214.616784</td>
    </tr>
    <tr>
      <th>28</th>
      <td>559.124324</td>
      <td>491.70</td>
      <td>257.784868</td>
    </tr>
    <tr>
      <th>29</th>
      <td>442.145000</td>
      <td>336.20</td>
      <td>169.228984</td>
    </tr>
    <tr>
      <th>30</th>
      <td>580.293103</td>
      <td>594.30</td>
      <td>165.961585</td>
    </tr>
    <tr>
      <th>31</th>
      <td>348.242857</td>
      <td>219.60</td>
      <td>172.555632</td>
    </tr>
    <tr>
      <th>32</th>
      <td>363.100000</td>
      <td>363.10</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>33</th>
      <td>338.200000</td>
      <td>338.20</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

## Assessment of Missingness

The review column in the dataset has missing data, and I believe the missingness is NMAR, not missing at random. I believe this because most of the time, people only take the time to write out and post a review if they have strong feelings about what they are reviewing and want to express their feelings. Because of this, the missingness of the column depends on how they feel which is the value itself, so the missingness is classified as NMAR.

To determine the missingness for the ratings column, I ran a permutation test to assess if the missingness of the rating column was due to the minutes column. 

Null hypothesis: Missingness of rating does not depend on the number of minutes.
Alternative hypothesis: the missingness of the rating depends on the number of minutes

After running the test, I obtained a p-value of 0.036, which I failed to reject at the 0.01 significance level, and therefore determined that the missingness of ratings was not dependant on the minutes column.

<iframe
  src="assets/missing_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I then ran a permutation test to assess if the missingness for the ratings column was due to the number of calories in the recipe.

Null hypothesis: Missingness of rating does not depend on the number of calories in the recipe
Alternative hypothesis: Missingness of the rating depends on the number of calories in the recipe

After running the permutation test, I obtained a p-value of 0.0, and therefore rejected the null hypothesis at the 0.01 significance level, and determined that the missingness of the rating column was dependant on the calories column, and therefore the missingness of the rating column is MAR.

## Hypothesis Testing

Next, I ran a permutation test to determine if there is a relationship between the length of a recipe and its average rating. 

Null Hypothesis: There is no relationship between time it takes for a recipe and the average rating of a recipe.
Alternative Hypothesis: Recipes that take over 37 minutes have a lower average rating than ones that take 37 or less minutes.

In order to avoid bias, I removed the outliers from the dataframe using the IQR rule. I then created a new column with boolean values that determined if the minutes of the recipe was less than or equal to 37 minutes. My test statistic was the difference in means between recipes under 37 minutes and over 37 minutes. The significance level i chose was 0.01, and the p-value I found was 0.0, so I therefore reject the null hypothesis, and conclude that there is a relationship between the length fo a recipe and its average rating. This test allows me to determine that that recipe length and ratinge are not independent, and allows me to get closer to understanding thier relationship.

<iframe
  src="assets/hyp_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

The prediction problem I will focus on is predicting the rating of a recipe, and it is a multiclass classification, as the rating is on a 1-5 scale, where the rating can be treated as a ordinal categorical variable. I chose the response variable to be the average rating of the recipe, as I think it would be interesting to see how the different variable can play a part in the decision of rating a recipe. It could also help to understand the thought process that goes into rating a recipe. The metric I am using to evaluate the model is the F-1 score, I this metric takes into account the imbalances of the dataset, which will a allow for a more balanced evaluation of the model that other metrics dont have. The things we would know at the time of the prediction are the variables we obtain from the recipes from the wensite, including things like the  ingredients, the steps, how many minutes it will take, etc.

## Baseline Model

My baseline model contains only 2 features, the number of steps and the number of ingredients that the recipe states. Both of these variables are numerical variables. I encoded both of these variables using the standard scaler transformation, in order to measure them on equal scales and make sure that large ranges dont cause bias. I used F1-score to evaluate the model, and got a score of 0.638. I dont think this model is very good as it only uses 2 features that I dont give a complete picture of the recipe, so I think a few other features are needed to make the model more complex and perform better. Also, the F1-score is a little low, so I think adding more features that are important to a recipe will help increase this score.

## Final Model

The features I used in the final model are minutes, n_steps, n_ingredients, calories(#), and has_sugar. I chose to include the minutes and calories features, as these features showed to have a relationship with the ratings column and n_ingredients column, meaning that it would add complexity to the model. I applied the standard scaler transformation to these columns to measure them on the same scale and avoid bias. I also chose to include the has_sugar feature, which I believed would help my model, as sugar is something people pay attention to when choosing a recipe. As this is a categorical feature, I one hot encoded it in order to use it in my model.

I used a random forest classifier for this model, as it would use predictions from many different trees, which would help to reduce overfitting. In order to choose my hyperparameters, I used GridSearchCV from sklearn along with 5-fold validation in order to determine the best value to use for max_depth and n_estimators. After running this, I found that the best parameters were a max depth of None and 200 for n_estimators. The F1-score that this model recieved was 0.914, which is a 0.276 increase. I think that this model is better suited for the data than the baseline model, as it takes into account more variables related to the dataset, giving the model a good overview in order to make a properly educated prediction. 

## Fairness Analysis

To evaluate the fairness of the model, I decided to determine if the precision of the model was roughly the same between shorter recipes (35 minutes or less) and longer recipes (over 35 minutes).

Null Hypothesis: The model is fair. The precision for shorter recipes is roughly the same as longer recipes
Alternative Hypothesis: The model is unfair. The precision for shorter recipes is lower than longer recipes

My test statistic was the difference between the precision of the model on shorter recipes and longer recipes (precision of shorter - precision of longer). I chose a significanve level of 0.01, and obtained a p-value of 0.88, and therefore fail to reject the null hypothesis. I conclucde that the model is fair between shorter and longer recipes.

<iframe
  src="assets/fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>








