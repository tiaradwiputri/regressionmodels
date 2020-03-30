# Regression Model Quiz

This quiz is part of Algoritma Academy assessment process. Congratulations on completing the Regression Model course! We will conduct an assessment quiz to test practical regression model techniques you have learned on the course. The quiz is expected to be taken in the classroom, please contact our team of instructors if you missed the chance to take it in class.

## Data Exploration

In this quiz, you will be using criminologist dataset (`crime`) stored under the `data_input` folder. If you have been following along in the course you can use the one that has already been loaded in the environment or you can run the following chunk in your RMarkdown to make sure we are using a same column names:

```
crime <- read.csv("data_input/crime.csv") %>% 
   select(-X) 
colnames(crime) <- c("percent_m", "is_south", "mean_education", "police_exp60", "police_exp59", "labour_participation", "m_per1000f", "state_pop", "nonwhites_per1000", "unemploy_m24", "unemploy_m39", "gdp", "inequality", "prob_prison", "time_prison", "crime_rate")
```

To make sure you have loaded the data correctly, do a simple data inspection. Try to peek our data frame by using any of the technique you have learned to get a better sense of the data.

```
# your code here

```

Among all variables within our `crime` dataset, there is a `crime_rate` variable that describes the measure of crime rate of the United States in 1960 from different states. Imagine you’re working as a government analyst and would like to see how social-economic conditions could reflect on the crime rate in a state. To do that, you will need to perform a exploratory analysis among the other variables.
___
1. Which variable might **NOT** be suitable as a predictor variable for our `crime_rate`?
  - [ ] gdp
  - [ ] police_exp59
  - [ ] unemploy_m39
  - [ ] nonwhites_per1000
___

## Simple Linear Regression    

A simple linear regression could help you to identify the quantified correlation between target and predictor variable as linear model coefficient. Based on the insight you have found from the previous step, try to answer the following question!

___
2. Which of the following predictor variable that describes `crime_rate` variable as lineary negative slope and statistically significant (p-value lower than 0.05)? 
  - [ ] police_exp59
  - [ ] nonwhites_per1000
  - [ ] prob_prison
  - [ ] percent_m

3. Among all simple linear regression models created from the variables listed in the second question, which one describes `crime_rate` the best?
  - [ ] police_exp59, based on the highest R-squared value which indicates a good fit
  - [ ] prob_prison, based on the lowest R-squared value which indicates a good fit
  - [ ] nonwhites_per1000, based on the coefficient's highest p-value which indicates a good fit
___

## Multiple Linear Regression

Using a simple linear regression limit model's capability of describing target variables using information from multiple variables. However, in building multiple linear regression, a variable selection process is required in order to pick the variables that adequately describes our target variable without having too much noise.

One of the technique we have learned in the course is using the stepwise regression algorithm to find the best model estimate. Now try to create the model for our `crime_rate` variable using a stepwise algorithm. Recall that you are required to specify `direction` parameter in the function. Between all available algorithm directions, pick the one resulting in the lowest AIC and attempt the following question!

```
# your code here

```
___
4. Based on the model with the lowest AIC, which statement is **incorrect**?
  - [ ] An increase of 1 of police_exp60 causes the value of crime_rate to increase by 10,265
  - [ ] An increase of 1 of unemploy_m24 causes the crime_rate to decrease by 6,087
  - [ ] An increase of 1 of mean_education causes the value of crime_rate to decrease by 18.01
  - [ ] Adjusted R-squared is a better metrics for evaluating our model compared to Multiple R-squared
___

## Assumptions Testing

One of the assumptions for linear regression stated that the error obtained from the model must be distributed normally around the mean of 0. To test the assumption, we can use one of the formal statistics test, Shapiro test which tests the following hypotheses:

$H_0$ : Error is distributed normally  

$H_1$ : Error is not distributed normally  

Pay attention at the following R result of using the  `shapiro.test` function on a given model's residual:

```
shapiro.test(model$residuals)
```
```
#   Shapiro-Wilk normality test
#
# data:  model$residuals
# W = 0.98511, p-value = 0.8051
```
___
5. Based on the Shapiro test result shown in the previous code, what conclusion can be drawn from the result?
  - [ ] Error is distributed normally (P-value higher than 0.05) 
  - [ ] Error is distributed normally (P-value lower than 0.05) 
  - [ ] Error is not distributed normally (P-value higher than 0.05) 
  - [ ] Error is not distributed normally (P-value lower than 0.05) 
___

Another assumption you need to test is whether or not the error of our model is homoscedastic indicating the error is distributed with equal variance over different data ranges. To test the assumption, we can use one of the formal statistics test, Breusch-pagan test which tests the following hypotheses:

$H_0$: Error is homoscedastic  

$H_1$: Error is heteroscedastic  

Pay attention at the following R result of using the `bptest` function from `lmtest` package by passing in the given model:

```
library(lmtest)
bptest(model)
```
```
#   studentized Breusch-Pagan test
#
# data:  model
# BP = 13.51, df = 8, p-value = 0.09546
```
___
6. Based on Breusch-Pagan test result, what conclusion can be drawn?
  - [ ] Heteroscedasticity is not present
  - [ ] Heteroscedasticity is present
  - [ ] The data spreads normally
  - [ ] There is no correlation between residuals and target variable
___

## Multicollinearity

Using VIF value, we can determine whether or not there are multicollinearity between predictor variables in our model. A high VIF value indicates a high correlation between predictor variables. Pay attention at the following R result of using the `vif` function from `car` and passing in the given model:

```
library(car)
vif(model)
```
```
#      percent_m mean_education   police_exp60     m_per1000f   unemploy_m24 
#       2.131963       4.189684       2.560496       1.932367       4.360038 
#   unemploy_m39     inequality    prob_prison 
#       4.508106       3.731074       1.381879 
```
___
7. Based on the VIF values shown on the previous result, which interpretation is correct?
  - [ ] inequality does not significantly affect crime_rate
  - [ ] An increase of 1 value on mean_education causes the value of crime_rate to increase by 4.1
  - [ ] Multicollinearity is not present in our model because the VIF values for all variables are considered to be small 
  - [ ] Variable `unemploy_m39` should be removed from model
  ___

## Evaluating Model Performance

Along this quiz session, you have performed statistical tests to make sure the model passed the assumptions of a linear regression model. Now imagine you were given a new dataset that records the same socio-economic variables from different observations. The data is stored under `crime_test.csv`, please read in the data to your R session. Pay attention to the new dataset provided. Among the variables you should see a `crime_rate` column describing the real crime rate value of the observations. 

```
# Your code here

```

Say you are given a model requirement from an existing project and were asked to perform a model evaluation on the provided test dataset. The project requires you to calculate the **mean squared error** of the following model:

```
model <- lm(formula = crime_rate ~ percent_m + mean_education + police_exp60 + 
    m_per1000f + unemploy_m24 + unemploy_m39 + inequality + prob_prison, 
    data = crime)

# Your code here

```
___
8. What is the MSE value of the crime_test prediction result? (round to two decimal points)    
  - [ ] 55027.7
  - [ ] 46447.42
  - [ ] 45269.15
___
