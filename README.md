<title> Further Investigation on The Recipe and Rating Dataset: Predicting Calories/title>

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
After looking at the performance of our baseline model, we decide to modify it to improve the prediction. For our final model, the first change that we planned to improve our features. We decided to add three new features and engineer two features to improve the performance of our model. The three features that we added are `tags`, `total fat (PDV)`, and `carbohydrates (PDV)`. 

The reason that we added `tags` is because we believe that the tags of a recipe could tell us more about the recipe. For example, a recipe that has tag such as 'healthy' is very likely to have low calories, on the other hand, a recipe that has tag such as 'junk food' is more likely to have high calories. We had to engineer this feature because it is categorical. We made each unique tag a column and the corresponding column would be a 1 if the recipe of the row had that tag, otherwise 0.

For the second and third features that we added, we chose them because we think they have the most to do with calories when compared to other nutritions that are available (for example, the amount of sodium almost doesn't contribute to calories at all). In the end, we were deciding between which 2 of the 4 (total fat (PDV), saturated fat (PDV), carbohydrates (PDV), sugar (PDV)) should we choose. The problem is that total fat and saturated fat are highly correlated and carbohydrates and sugar are highly correlated as well. Choosing all of them would most likely cause multicollinearity. In the end, we decided to use `total fat (PDV)` and `carbohydrates (PDV)` because although the other two could tell us more about how healthy a recipe is, what we want is just to predict calories, which we think fits better with `total fat (PDV)` and `carbohydrates (PDV)`. 

In addition, we standardized the `n_ingredients` because we felt its range was quite different from the range of the other features we used. 

Next, we move on to model selection. Although linear regression is not bad, we think that there are better options out there. It came down to deciding between two models, K-Nearest Neighbors Regressor and Decision Tree Regressor (An honorable mention is Random Forest Regressor, it performed quite well but due to computing power limitations, we had to give up on that). The major reason we saved K-Nearest Neighbors Regressor to the last round is because it works well with small datasets and thus less likely to overfit. For Decision Tree Regressor, we saved it till last round because it is more robust to outliers. 

In order to decide between the two, we ran each of them for more than 20 times and looked at the best and worst performances. Best performance of our Knn regressor model is 263.1250550529161 and the worst performance is 289.4620151703712. Best performance of our DecisionTree regressor model is 117.494828027991, the worst performance is 201.34523387269653. As you could see, the KNN regressor model gives more consistant performances. On the other hand, our DecisionTree regressor model gives less consistant performance but performs better overall. Therefore, after serious consideration, we decided to use the DecisionTree regressor model. 

The next step is for us to decide which hyperparameters to use for our DecisionTree regressor model. We performed a series of Grid Search and found that our model performs best when `max_depth` is 18 and `min_samples_split` is 9. 

With all that done, we trained the model on the train data and used it to predict on the test data. After getting the predictions back, we calculated it's RMSE and it came back to be a stunning **52.0570606312737**. We ran it a few more times afterwards and most of the times it came back to be around **50**. In my opinion, our model performed great especially when compared to our baseline model. Our new RMSE is only 1/10 of the RMSE of our baseline model. That means, our predictions on average are only off by around 50 calories. The effect of this is very small and people could use it reliably to predict the calories of recipes based on its tags, carb, fat, and number of ingredients. 


## Fairness Analysis
We tried to test our model on high carbohydrates recipe and low carbohydrates recipe. And since our model is a regressor model, we used RMSE as our evaluation metric. Our null hypothesis is that our model is fair. Its RMSE for high carbohydrates recipes and low carbohydrates recipes is roughly the same, and any differences are due to random chance.Our alternative hypothesis is that our model is  unfair. Its RMSE for high carbohydrates recipes and low carbohydrates recipes is higher toward high carbohydrates recipes. The test statistic is how much high carbohydrates recipes RMSE is larger than the low carbohydrates recipes RMSE. Our significance level is 0.05. The resulting p-value is 0.0, which means none of our RMSE after permutated carbo labels has as equal or higher RMSE than the observed RMSE. In this case, our model likely does not achieve accuracy parity for high carbohydrates recipes and low carbohydrates recipes.
