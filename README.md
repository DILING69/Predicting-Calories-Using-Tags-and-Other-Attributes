<title> Predicting Calories Using Tags and Other Attributes</title>
<body>
## Introduction

In this project, we will attempt to **predeict the caloreis of a recipe** through **regression**. 

The response variable is the **calories** of each recipe. We chose to predict this variable because calories is a very straightforward and important indicator of the healthiness of a recipe and is especially important to people who are trying to loose weight (me). 

At the time of prediction, we will have all of the information available except for `calories` in the `nutrition` column to make prediction. Recipe name, recipe ID, minutes to prepare recipe, user ID who submitted this recipe, date recipe was submitted and other columns are all information that we can have our hands on before measuring the calories of a recipe. In terms of other nutritions, those can also be measured prior to measuring calories. 

In the end, we will use **The Root Mean Squared Error (RMSE)** instead of R^2 as the metric to evaluate our model. The reason we use RMSE is because RMSE tells us the typical distance between the predicted value made by the regression model and the actual value while R^2 tells us how well the predictor variables can explain the variation in the response variable. In our case with predicting calories, we are more curious about how close our predicted value is to the actual value rather than the variation. 

## Baseline Model

</body>