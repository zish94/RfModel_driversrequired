# RfModel_driversrequired

This model evaluates the number of drivers required to meet the demand of orders.

# Importing Libraries

import pandas as pd
import numpy as np


import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split

import xgboost as xgb

from sklearn.ensemble import RandomForestRegressor


# importing data

data = pd.read_csv('wsseldemandbyhour.csv')

# checking data

data = data.dropna()

# data info
data.describe()

# Renaming Columns
data=data.rename(columns = {'avg(amount)':'avg_amount'})

# replacing negative and zeros with 5 in column 'avg_amount'
maskz = data.avg_amount < 5
column_namez = 'avg_amount'
data.loc[mask, column_namez] = 5

# replacing where there is 0 or very small value with  avg waiting time 
mask = data.avgwaitingmins < 30
column_name = 'avgwaitingmins'
data.loc[mask, column_name] = 30


# Removing unneccessary columns
data = data.drop("dt",  axis = 1)

# Creating Test & Train
y2 = data.drop("drivers", axis = 1)
X_train, X_test, y_train, y_test = train_test_split(y2, data['drivers'], test_size=0.2,
                                                    random_state=0)
                                                    
# Creating Random Forest Regressor Model 

rf2 = RandomForestRegressor(n_estimators=500, oob_score=True, random_state=0)
rf2.fit(X_train, y_train)

# Predicting Values
rf2.predict(y_Test)
