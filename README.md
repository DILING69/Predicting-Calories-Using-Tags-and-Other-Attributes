<title> Predicting Calories Using Tags and Other Attributes</title>

Our exploratory data analysis on this dataset can be found at <a href="https://diling69.github.io/General_Study_on_Food_Recipe/">here</a>

## Introduction

In this project, we will attempt to **predeict the caloreis of a recipe** through **regression**. 

The response variable is the **calories** of each recipe. We chose to predict this variable because calories is a very straightforward and important indicator of the healthiness of a recipe and is especially important to people who are trying to loose weight (me). 

At the time of prediction, we will have all of the information available except for `calories` in the `nutrition` column to make prediction. Recipe name, recipe ID, minutes to prepare recipe, user ID who submitted this recipe, date recipe was submitted and other columns are all information that we can have our hands on before measuring the calories of a recipe. In terms of other nutritions, those can also be measured prior to measuring calories. 

In the end, we will use **The Root Mean Squared Error (RMSE)** instead of R^2 as the metric to evaluate our model. The reason we use RMSE is because RMSE tells us the typical distance between the predicted value made by the regression model and the actual value while R^2 tells us how well the predictor variables can explain the variation in the response variable. In our case with predicting calories, we are more curious about how close our predicted value is to the actual value rather than the variation. 


## Baseline Model

For our baseline model, we decided to use the good old **linear regression** model, in which we will use the number of steps (`n_steps`) the recipe takes and the number of ingredients (`n_ingredients`)  that the recipe needs as features to base our predictions on. Both features are quantitative, therefore we could directly use them to make predictions without any encoding. 

By running the model multiple times on different training and testing sets, the RMSE of our model seems to range from 500 to 600 calories. 
`'Best performance of our Baseline model is 519.0098969288084, the worst performance is 574.2065724451372'`
(Best and worst performances on 20 sets of test data)

Based on the results that we see, I won't say that our Baseline model is performing good. 500 calories is a very large distance, it is approximately the calories of a bowl of pan fried noodles, which is the amount of calories that an adult would take in in a full meal. If the prediction is 500 calories higher than the actual, that means the person may eat less and causing them to not consume enough energy, which could lead to serious health problems. On the other hand, if the prediction is 500 calories lower than the actual, the person will eat too much, which destroys the person's plan to loose weight.


## Final Model
After looking at the performance of our baseline model, we decide to modify it to improve the prediction. For our final model, the first change that we planned to improve our features. We decided to add three new features and modify two features to improve the performance of our model. The three features that we added are `tags`, `total fat (PDV)`, and `carbohydrates (PDV)`. 

The reason that we added `tags` is because we believe that the tags of a recipe could tell us more about the recipe. For example, a recipe that has tag such as 'healthy' is very likely to have low calories, on the other hand, a recipe that has tag such as 'junk food' is more likely to have high calories.

For the second and third features that we added, we chose them because we think they have the most to do with calories when compared to other nutritions that are available (for example, the amount of sodium almost doesn't contribute to calories at all). In the end, we were deciding between which 2 of the 4 (total fat (PDV), saturated fat (PDV), carbohydrates (PDV), sugar (PDV)) should we choose. The problem is that total fat and saturated fat are highly correlated and carbohydrates and sugar are highly correlated as well. In the end, we decided to use `total fat (PDV)` and `carbohydrates (PDV)` because although the other two could tell us more about how healthy a recipe is, what we want is just to predict calories, which we think fits better with `total fat (PDV)` and `carbohydrates (PDV)`. 