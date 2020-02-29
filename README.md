# Zillow's Home Value Prediction Problem - Kaggle

## Introduction
This project aims to predict the log error of Zillow’s estimate and the actual sale prices for California for years 2016-2017.  According to the National Association of Realtors, more than 5 million units of existing homes were sold in the US in 2020. Home purchase signifies a significant expense for any individual, therefore, a lot of research goes into buying a home. Home price estimates would give the seller and the buyer a reference point, which would thus reduce the time and effort of the people. As a result, a good home price estimate would reduce a lot of unnecessary cost, and would help both the buyers and sellers.

In this project, I use the dataset provided by Zillow for Kaggle, and apply linear regression and gradient boosting to predict the log(error) of Zillow’s estimate. I use the Mean Squared Error to evaluate the model as it is mathematically easy to define and we can figure out the difference in the price error of the estimate. I find that using the Gradient Boosting model gives a better prediction than other models.

## Data
The data was provided by Zillow. The dataset had 90,275  observations and 60 features that described the location, year built, square footage, number of bedrooms and bathrooms, tax amount, among others. The model aims to predict the log(error), which is described as:

  *logerror = log(Zestimate) - log(SalePrice)*

## Data Analysis
First, log(error) had a normal distribution with the mean centered around 0. The number of transactions decreases towards the end of the year and the log(error) increases compared to the summer months, as shown in the figure below. This could imply that fewer buyers are in the market during winter months, which could incentivize sellers to either not sell it (resulting in fewer transactions) or sell it below the market price, thus increasing the error of the price prediction during these months.

![TargetVar](https://github.com/sharmas412/Zillow-Kaggle/blob/master/images/TargetVarExploration.png)

### Missing Variables
The graph below shows the percentage of missing variables for each feature. 

![Missing](https://github.com/sharmas412/Zillow-Kaggle/blob/master/images/MissingVar.png)

Based on the data dictionary, missing variables for pools and fireplaces are replaced with 0. I take similar approach to patio, deck, garage and basement. 

For other missing variables, I replace them with either the mean or mode depending on the feature. For instance, if the number of pools is greater than zero, then I replace the poolsize with the mean of the variable. Similar approach is taken for features such as ‘lotsizesquarefoot’ and ‘taxvalue’. 

However, for missing variables in the number of stories, replacing it with the mode of the variable makes more sense than the mean. I use mode to replace missing variables for other features such as  year built, building quality type, and property zoning descriptions.

I also checked for multicollinearity of the features as shown in the figure below.

![Collinearity](https://github.com/sharmas412/Zillow-Kaggle/blob/master/images/Multicollinearity.png)

### Feature Selection and Engineering
After imputing for missing variables, there were still some features that had a lot of missing observations such that imputing would be difficult; therefore, they were dropped. Further, there are some redundant features that do not add new information, so I drop these features. For example, bathroom count encompasses the information that other features such as ‘full bathroom count’ and ‘three quarter bathroom’ would give. I also checked for multicollinearity of the features as shown in the figure below.

I also created features such as ‘tax percentage’ based on ‘tax amount’ and ‘tax value’, thus keeping the information but reducing the number of features in the model. I also encoded categorical variables such as ‘zoning desc’ and ‘land use code’ and created dummy variable for ‘weather’.

## Modeling

### Random Forest
I first used Random Forest as it decreases correlation and reduce model variance by randomly selecting a subset of predictors at each node of the decision tree. This is limit overfitting the model. The model returned a MSE of 0.075

### Gradient Bossting
The Gradient Boosting method builds trees sequentially using the residuals from previous trees. The model returned a MSE of 0.070, which is lower than that of the random forest model.

### Cross Validation and Hyperparameter tuning
In order to prevent overfitting, I perform cross-validation and tuned hyperparameters over a tuning grid. Based on that, gradient boosting resulted in the lowest MSE of 0.067. 

## Further Strategies
The next step would be to simplify the model by using only the most important features. Furthermore, we can create new features by combining the important features. 

### Appendix

### Dataset
The dataset provided by Zillow is stored in 'zillow_raw.csv'

The dictionary dataset 'zillow_data_dictionary.xlsx' gives the description of the data features

### Codes
1) 01_DataExploration : does the exploratory data analysis of the features
2) 02_DataPreprocess: imputes missing variables, cleans up data and feature engineering and saves the dataset as 'ModelReadyZillow.csv'
3) 03_Model : predicts the logerror using the RandomForestRegressor and the GradientBoostRegressor and applies GridSearch to fine tune the model
