---
layout: post
title:      "Module 2 Project - King County Business Case"
date:       2020-12-22 08:26:06 -0500
permalink:  module_2_project_-_king_county_business_case
---


## Introduction
We are back, with another Module Project, this time focused on using Statistics and Linear Regression in Python. For this project we will be working with a dataset containing data on property transactions from 2014-15 in King County, Washington.  This dataset contains features of a property such as square footage, rooms, floors, price and several others. The dataset used was modified for the purposes of this project (basically to force data cleaning techniques – good practice!), but the original dataset can be found on Kaggle on the following link: https://www.kaggle.com/harlfoxem/housesalesprediction.
For this project we decided to fill the fictional shoes of a real estate consultancy agency in the area called KCC. As a real estate consultancy, we deal with various clients and problems and our primary obligation is to understand the housing market. We cover the entirety of King County, Washington and aim to provide a comprehensive resolution to whatever requests our clients might make.
![Seattle Skyline](https://github.com/Zuchini1234/King-County-Business-Case-Project-Mod-2/blob/master/Images/Seattle_Skyline.jpeg)

## What is Linear Regression:

Linear Regression is a method of predictive modelling. It tries to predict an outcome (often referred to as the dependent variable or x) from an input variable (the independent variable or y). This can also be done with multiple input variables and is then called Multiple Linear Regression. This differs from Multivariate Linear Regression which predicts multiple dependent variables. The output of this method is scalar, in that it identifies both direction and magnitude of relationship. In other words, it identifies the expected change in y for every change in x. 

Even though Linear Regression is one of the least accurate prediction models, it is the most interpretable. There is an inverse correlation between the complexity, and by extent accuracy, of a model and its accuracy. Linear Regression is used because despite its lower relative accuracy compared to other Machine Learning models, it is highly interpretable. Linear Regression presents a model that can be scaled to acceptable accuracy by including multiple features while still being very easy to gain insight from and present. There is an interesting article on this topic on this link (https://towardsdatascience.com/model-complexity-accuracy-and-interpretability-59888e69ab3d). 

### Outline
1.	**Creating a Multiple Linear Regression **
2.	**Answering Customer Questions**
3.	**Business Insights Summary**
For a full view of the code that went into generating this project, please visit https://github.com/Zuchini1234/King-County-Business-Case-Project-Mod-2/blob/master/student.ipynb. 

## 1 – Creating a Multiple Linear Regression
### What is Linear Regression:

Linear Regression is a method of predictive modelling. It tries to predict an outcome (often referred to as the dependent variable or x) from an input variable (the independent variable or y). This can also be done with multiple input variables and is then called Multiple Linear Regression. This differs from Multivariate Linear Regression which predicts multiple dependent variables. The output of this method is scalar, in that it identifies both direction and magnitude of relationship. In other words, it identifies the expected change in y for every change in x. 

Even though Linear Regression is one of the least accurate prediction models, it is the most interpretable. There is an inverse correlation between the complexity, and by extent accuracy, of a model and its accuracy. Linear Regression is used because despite its lower relative accuracy compared to other Machine Learning models, it is highly interpretable. Linear Regression presents a model that can be scaled to acceptable accuracy by including multiple features while still being very easy to gain insight from and present. There is an interesting article on this topic on this link (https://towardsdatascience.com/model-complexity-accuracy-and-interpretability-59888e69ab3d). 

### Assumptions of Linear Regression
There are five key assumptions that need to be met to perform a linear regression:
*Linear Relationship – The relationship between input and target variables needs to be linear. In other words, for a change in input, there is an assumption that there would be a linear change in the target variable. This can be tested with scatter plots. 
*Normality – The data needs to be normally distributed (i.e. resembling a bell curve) which can be checked with boxplots. 
*No or little Multicollinearity – Multicollinearity occurs when different input variables are too highly correlated with each other. This can be checked by using a Pearson Correlation Matrix. Variables such as sqft_lot_15 and sqft_living_15 were removed because of their high correlation with other variables. 
![PCMatrix]( https://github.com/Zuchini1234/King-County-Business-Case-Project-Mod-2/blob/master/Images/PCMatrix.png)
NB: While writing this, I discovered that there is another method for determining Multicollinearity called Variance Inflation Factor (VIF) with any VIF figures over 10 clearly indicating multicollinearity. I will save this knowledge for use in my next project. Cool explanation: https://www.analyticsvidhya.com/blog/2020/03/what-is-multicollinearity/.
*No auto-correlation – 
Autocorrelation occurs when independent variables are not independent of other values of themselves. An example would be stock price, the stock price on any given day is based largely on the price of the stock the day before. This can be tested with the Durbin-Watson test present in the OLS summary table of every model we generated. The generally accepted threshold for the DB test to indicate acceptable or no auto-correlation is between 1.5 and 2.5. 
*Homoscedasticity - The residuals are equally spread around the regression line, will be checked for each model
Additionally, Linear Regression requires at least twenty samples per independent variable which is easily met here.

