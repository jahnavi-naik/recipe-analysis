# Predicting Recipe Rating based on Recipe Length
Author: Jahnavi Naik

## Overview
This is my final project for DSC80 at UCSD, and focuses on a dataset of recipes and reviews from food.com. The main focus of this project is to learn more about the relationship between the length of a recipe and its rating, as well as creating a prediction model to predict the rating of a recipe based on different features.


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

