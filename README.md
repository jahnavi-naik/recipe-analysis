# Predicting Recipe Rating based on Recipe Length
Author: Jahnavi Naik

## Overview
This is my final project for DSC80 at UCSD, and focuses on a dataset of recipes and reviews from food.com. The main focus of this project is to learn more about the relationship between the length of a recipe and its rating, as well as creating a prediction model to predict the rating of a recipe based on different features.


## Introduction
Food is an important aspect of everyone's life, especially mine. Food is not just a necessity, but cooking and baking is a hobby and profession for many. Naturally, food.com becomes a prominent website for finding recipes for a variety of dishes, and even allows you to leave reviews and ratings, helping others determine what recipes to try. When looking for a recipe to make, many home cooks value the time it takes to actually make a recipe, to fit it in their busy schedules. Because of this, I think it is important to understand the relationship between the length of the recipe, and its rating, so I decided to focus on the question, **How does the length of the recipe/ ingredients affect the ratings of the recipe?** 
For this project, obtained 2 datasets, originally taken from food.com. The first dataset, called recipes, contains 12 columns, and 83782 rows. The second dataset, called reviews, contains 5 columns and 731927 rows. 

The columns in the recipe dataframe that I use are:
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

The columns of the reviews dataframe that I use are:
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

To start my exploration of the data I was working with, I created a histogram to visualize the distribution of the number of steps in the recipe. The number of steps in this dataframe ranges from 1 to 100 steps. The graph shows that they data is skewed to the right, and that most recipes have between 5-9 steps.

<iframe
  src="assets/univariate-n_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


