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

### Outline
**1.	Creating a Multiple Linear Regression 
2.	Answering Customer Questions
3.	**Business Insights Summary**

For a full view of the code that went into generating this project, please visit https://github.com/Zuchini1234/King-County-Business-Case-Project-Mod-2/blob/master/student.ipynb. 

## 1 – Creating a Multiple Linear Regression
### What is Linear Regression:

Linear Regression is a method of predictive modelling. It tries to predict an outcome (often referred to as the dependent variable or x) from an input variable (the independent variable or y). This can also be done with multiple input variables and is then called Multiple Linear Regression. This differs from Multivariate Linear Regression which predicts multiple dependent variables. The output of this method is scalar, in that it identifies both direction and magnitude of relationship. In other words, it identifies the expected change in y for every change in x. 

Even though Linear Regression is one of the least accurate prediction models, it is the most interpretable. There is an inverse correlation between the complexity, and by extent accuracy, of a model and its accuracy. Linear Regression is used because despite its lower relative accuracy compared to other Machine Learning models, it is highly interpretable. Linear Regression presents a model that can be scaled to acceptable accuracy by including multiple features while still being very easy to gain insight from and present. There is an interesting article on this topic on this link (https://towardsdatascience.com/model-complexity-accuracy-and-interpretability-59888e69ab3d). 

### Assumptions of Linear Regression
There are five key assumptions that need to be met to perform a linear regression:

1.Linear Relationship – 
The relationship between input and target variables needs to be linear. In other words, for a change in input, there is an assumption that there would be a linear change in the target variable. This can be tested with scatter plots. 

2.Normality – 
The data needs to be normally distributed (i.e. resembling a bell curve) which can be checked with boxplots. 

3. No or little Multicollinearity – 
Multicollinearity occurs when different input variables are too highly correlated with each other. This can be checked by using a Pearson Correlation Matrix. Variables such as sqft_lot_15 and sqft_living_15 were removed because of their high correlation with other variables. 

![PCMatrix]( https://github.com/Zuchini1234/King-County-Business-Case-Project-Mod-2/blob/master/Images/PCMatrix.png)

NB: While writing this, I discovered that there is another method for determining Multicollinearity called Variance Inflation Factor (VIF) with any VIF figures over 10 clearly indicating multicollinearity. I will save this knowledge for use in my next project. Cool explanation: https://www.analyticsvidhya.com/blog/2020/03/what-is-multicollinearity/.

4. No auto-correlation – 
Autocorrelation occurs when independent variables are not independent of other values of themselves. An example would be stock price, the stock price on any given day is based largely on the price of the stock the day before. This can be tested with the Durbin-Watson test present in the OLS summary table of every model we generated. The generally accepted threshold for the DB test to indicate acceptable or no auto-correlation is between 1.5 and 2.5. 

5. Homoscedasticity – 
The residuals need to be equally spread around the regression line. This can be checked by graphing the residuals or using the Goldfield-Quandt Test. Both these techniques in our modelling. 

Additionally, Linear Regression requires at least twenty samples per independent variable. Our dataset had closer to twenty thousand so we were covered on that front. 
### Multiple Linear Regression
To make it easier to generate multiple models we created a function that train/test split the data, generated a model summary, model accuracy (R-Squared for both train and test values), then graphed normality (both with a line and a histogram), and homoscedasticity.
Here is the equation we used.:
```def linear_reg(df):
    
	# Split independent and dependent variables
	X = df.drop('price', axis = 1)
	y = df['price']

	# Split both into 75/25 train/test split
	X_train, X_test, y_train, y_test = train_test_split(
			X, y, train_size = 0.75, random_state = 12)

	# Create a new training set Dataframe w/ features and target
	kc_train = pd.concat([X_train, y_train], axis = 1)
	# Test Dataframe
	kc_test = pd.concat([X_test, y_test], axis = 1)

	linreg = LinearRegression()
	model = linreg.fit(X_train, y_train)

	# Check the score (R-squared)
	train_score = model.score(X_train, y_train)
	test_score = model.score(X_test, y_test)
	print('Training Score:', round(train_score, 2))
	print('Test Score:', round(test_score, 2))

	# Designate input and outcome variables
	outcome = 'price'
	x_cols = [f"Q('{i}')" for i in X.columns]

	# Create a formula and use it to initiate an ols model 
	predictors = '+'.join(x_cols)
	formula = outcome + '~' + predictors
	smmodel = ols(formula = formula, data=kc_train).fit()
	print(smmodel.summary())

	# Test for Normality
	print("\nTest for Normality")
	fig = sm.graphics.qqplot(smmodel.resid, dist=stats.norm, line='45', fit=True)
	plt.show()

	# Remove the Q and parentheses from the x_cols object to allow using it with the dataframe
	x_cols_strip = [i.strip("Q()'") for i in x_cols]

	# plot the models homoscedasticity
	print("Test for Homoscedasticity")
	plt.scatter(smmodel.predict(kc_train[x_cols_strip]), smmodel.resid)
	plt.plot(smmodel.predict(kc_train[x_cols_strip]), [0 for i in range(len(kc_train))])
	plt.show()

	# plot the residuals distribution
	print("Test for Distribution of Residuals")
	sns.distplot(smmodel.resid)
	plt.show()

	return model, smmodel
```

### Our Final Model 
After dealing with the above mentioned assumptions and creating five separate models, trying to improve in each one, this is what we came out with in the end. 
![Model_5_Summary]( https://github.com/Zuchini1234/King-County-Business-Case-Project-Mod-2/blob/master/Images/Model_5_Summary.png)

