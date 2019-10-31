# Data Exploration

In this quiz, you will be using criminologists data (`crime`). The data has been loaded on the chunk environment. Before you perform a linear regression using this dataset, you will need to explore the data to get information about the data. Common data inspection includes:

* Peek in the first or last 6 data from ‘crime’ data! Choose between using `head` or `tail` function.
* Inspect all available columns by inspecting the data using str

```
# your code here

```

Among the variables within our `crime` dataset, we can see a `crime_rate` variable that describes `crime` rate in a country at 1960. Recall how we can try to inspect the correlation for each variables using `cor` or `ggcorr` from `GGally` package. Try it out on your own and see what are the possible predictor variables for our `crime_rate` variables.

```
# your code here

```

Based on the correlation plot above, we should know which variables has the highest or lowest correlation with our crime_rate. Using the correlation value, we can choose the variables as our variable predictor.

## Quiz Section

Based on the correlation information you have acquired, try to answer the following question

1. Which variable has little to no correlation with crime rate variable?
  - [ ] crime_rate
  - [ ] police_exp59
  - [ ] unemploy_m39
  - [ ] nonwhites_per1000

# Build Linear Model    

From the data exploration process and the correlation information of `crime` dataset, we know that not all variables indicates a strong correlation with `crime_rate`. Let’s try to build a simple linear model using one of the highly correlated variable: `police_exp59`. Create a regression model using `lm()` function to predict `crime_rate` using `police_exp59` from crime dataset and assign it to an object named `model_crime`.

```
# your code here

```

## Quiz Section

After building `model_crime`, inspect the attributes of the model using summary function and use the information to answer the questions below.

2. Which of the following best described the slope?**
  - [ ] It's a negative slope, and is statistically insignificant (P-value higher than 0.05)
  - [ ] It's a positive slope, and is statistically significant (P-value lower than 0.05)
  - [ ] It's a positive slope, and is statistically insignificant (P-value higher than 0.05)
  - [ ] It's a negative slope, and is statistically significant (P-value lower than 0.05)

3. What is the most fitting conclusion from the regression model above?
  - [ ] The R-squared does not tell us about the quality of our model fit. That is the job of the p-value.
  - [ ] The R-squared approximates 0.44, indicating a reasonable fit (the closer to 0 the better)
  - [ ] The R-squared approximates 0.44, indicating a poor fit (the closer to 1 the better)

# Feature Selection

## Feature Selection using Stepwise Regression

Basen on the R-square alone, our `model_crime` might not indicate a good fit. The R-square of `model_crime` approximates 0.44, meaning the `model_crime` can only describes 44% from the total variability of our data. One way to improve the model is by adding more predictor variables. To do that, we can start by hypothesize another variables that is considered to be significant to predict `crime_rate`.

We have learned a stepwise regression algorithm that enables us to perform an automated feature selection to predict `crime_rate` using `step()` function. Try to use the step() function with `direction = "backward"` parameter. Once you get the best model estimates, create a new model from it and store it under `model_step` object.

```
# your code here

```

## Quiz Section

After building the model, go ahead and check model_step summary and answer the question below.

4. Based on the summary of our final model, which statement is incorrect?
  - [ ] An increase of 1 of police_exp60 causes the value of crime_rate to increase by 10,265
  - [ ] An increase of 1 of unemploy_m24 causes the crime_rate to decrease by 6,087
  - [ ] An increase of 1 of mean_education causes the value of crime_rate to decrease by 18.01
  - [ ] Adjusted R-squared is a better metrics for evaluating our model compared to R-squared

# Normality Assumption

## Shapiro test for Normality test

One of the assumption of linear regression stated that the error obtained from the model must distributed normally. We expect to have our error value to be normally distributed around the mean of 0.

In previous tutorial, we have stored a linear regression model under `model_step` object. Let’s try to validate our normality assumption `shapiro.test()` function. This function requires us to pass in the residuals of our model.

```
# your code here

```

Shapiro testing, use the following null and alternative hypothesis:

$H_0$ : Error is distributed normally

$H_1$ : Error is not distributed normally

## Quiz Section

There are varying way to check for normality assumption. Based on the information you have gained along the class, try to answer the following question:

5. What are the method(s) we can use to check for normality of the residual from our model?
  - [ ] QQ plot
  - [ ] Shapiro-test
  - [ ] Adjusted R-Square
  - [ ] Correlation

# Heteroskedasticity Assumption

## Breusch-Pagan for Heteroskedasticity Test

To test wether or not our data is homoscedastic, you can use Breusch-Pagan test for a formal statistical test. This test use the following null and alternative hypothesis:

$H_0$: Data is considered Homoscedastic

$H_1$: Data is considered Heteroscedastic

Validate wether or not our fitted data violates the no heteroscedasticiy assumption using `bptest` function from `lmtest` package. Pass in our `model_step` as the parameter to acquire the test result.

```
# your code here

```

## Quiz Section

Use the output from the test above to answer the following question:

6. Based on Breusch-Pagan test we have perform, what conclusion can be drawn from the result?
  - [ ] Heteroscedasticity is not present
  - [ ] Heteroscedasticity is present
  - [ ] The data spreads normally
  - [ ] There is no correlation between residuals and target variable

# No Multicollinearity Assumption

## Variance Inflation Factor

In linear regression, each predictor is assumed to be independent to each other. To test this, you can use the `vif` function of the`car` package to acquire each predictor variable’s `VIF` value. Let’s take our `model_step` object and pass in to the function and see if there any multicollinearity present.

```
# your code here

```

If there are no `vif` value bigger that is significantly larger than most, we can safely assume that there are no multicollinearity between the variables.

## Quiz Section

Based on the VIF value from previous section, try to answer following question.

7. From the multicolinearity test result, which interpretation is correct?
  - [ ] inequality does not significantly affect crime_rate
  - [ ] unemploy_m39 has weak correlation with unemploy_m24
  - [ ] An increase of 1 value on mean_education causes the value of crime_rate to increase by 4.1

# Making Prediction

## Predict Unseen Data

Since we have passed all our assumption test on previous sections, we will try to predict unseen data using `model_step` object. We have provided `crime_test` dataset in your environment for you to use with `predict` function. Store the prediction value from our new data set under `pred_value` object.

```
# your code here

```

## Mean Squared Error

Within our workshop, we have also learned various metrics to measure our model performance. One of the common metrics used is mean squared error (`MSE`). You can calculate the `MSE` of our prediction using `MSE` function from MLMetrics package. Store the `MSE` value under `mse_crime` object and don’t forget to specify `y_true` and `y_pred` parameter from MSE function.

```
# your code here

```

8. What is the MSE value of the crime_test prediction result? (round to two decimal points)    
  - [ ] 35467.31
  - [ ] 188.33
  - [ ] 0.19
