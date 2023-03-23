# Introduction

In this project, we will attempt to **predeict the caloreis of a recipe** through **regression**. 

The response variable is the **calories** of each recipe. We chose to predict this variable because calories is a very straightforward and important indicator of the healthiness of a recipe and is especially important to people who are trying to loose weight (me). 

In the end, we will use **The Root Mean Squared Error (RMSE)** instead of R^2 as the metric to evaluate our model. The reason we use RMSE is because RMSE tells us the typical distance between the predicted value made by the regression model and the actual value while R^2 tells us how well the predictor variables can explain the variation in the response variable. In our case with predicting calories, we are more curious about how close our predicted value is to the actual value rather than the variation. 