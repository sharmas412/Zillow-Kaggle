# Zillow's Home Value Prediction Problem - Kaggle

## Problem
Zillow is asking to predict the log-error between their Zestimate and hte actual sale price, given all the features of the home. The log error is defined as:

  *logerror = log(Zestimate) - log(SalePrice)*

## Objective
Build a model to accurately predict the logerror

### Dataset
The dataset provided by Zillow is stored in 'zillow_raw.csv'

The dictionary dataset 'zillow_data_dictionary.xlsx' gives the description of the data features

### Codes
1) 01_DataExploration : does the exploratory data analysis of the features
2) 02_DataPreprocess: imputes missing variables, cleans up data and feature engineering and saves the dataset as 'ModelReadyZillow.csv'
3) 03_Model : predicts the logerror using the RandomForestRegressor and the GradientBoostRegressor and applies GridSearch to fine tune the model
