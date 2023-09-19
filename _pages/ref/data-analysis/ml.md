---
layout: page
permalink: /ref/data-analysis/ml/
title: ml
description: Machine Learning
nav: false
toc: true
---

Machine Learning 

Some libraries..
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

from sklearn.impute import SimpleImputer        # Scrub data - populate empty cells with ave value of col

from sklearn.compose import ColumnTransformer   # Categorical data tool
from sklearn.preprocessing import OneHotEncoder # Take a category with unique vals and create multi bin values (1,0,0 and 0,1,0 etc)
from sklearn.preprocessing import LabelEncoder  # Encode No/Yes into 0,1 

from sklearn.model_selection import train_test_split # Training and Test Sets

from sklearn.preprocessing import StandardScaler  # Scale the training set so data is in the same range

```
# Data Pre-Processing
## Import
    - Features (independent variables) vs Dependent variable vector. Features/independent variables are the columns you will use to predict the dependent variable (last column). 
```python
df = pd.read_csv('Data.csv')
X = df.iloc[:, :-1].values  # all rows/col except the last col (all feature/independent variables )
y = df.iloc[:, -1].values # dependent variable vector. last col
```

## Scrub  
    - Can leave bad/missing data out if <1% or could replace it with average data  
```python
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
imputer.fit(X[:, 1:3])  # fit will look for missing values and calculate ave on X col 1,2
X[:, 1:3] = imputer.transform(X[:, 1:3]) # transform will perform the replacement
```  

## Encode Categorical Data
```python
# OneHotEncoder will take a category col (ie a col with 3 unique tool IDs) and create multiple columns
# to represent them as 1/0.  So a col with 3 unique tool IDs would become 3 col with 1,0,0 and 0,1,0 and 0,0,1
# ColumnTransformer takes 2 arg
# 1-  what kind of transformation and which index to perform it on 
# 2-  remainder or which cols to keep that won't have transformation applied
# this example using 'encoder' transformation with OneHotEncoder class on column 0 which is string type.
# 'passthrough' says to keep the columns that were not listed in the transform 
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), [0])], remainder='passthrough')
X = np.array(ct.fit_transform(X))  # does fit and transform at same time. Must keep X as numpy array

# For single vector, dependent variable, use label encoder to convert yes/no to 0/1 (binary)
le = LabelEncoder()
y = le.fit_transform(y)  # does not have to be numpy array
```

## Training and Test Sets
Split the data into a training and test sets. Use 80% for training and remaining data for testing. This lets you compare the predicted values from the model to the actual values.  
Will create 4 sets.  
Pair of matrix features and dependent variable for train set.   
  X train or matrix of features of training set  
  Y train or dependent var of training set  
Pair of matrix features and dependent variable for test set.   
  X test or matrix of features of test set  
  Y test or dependent var of test set  

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 1)
# 80% observations go to training and 20% to test
# random_state=1 for learning so will return same set as tutorial
```

## Feature scaling 
Not used for a lot of models.  
Feature scaling applied after split. your test split is supposed to represent new data that you don't have until training is done.  
Needed to normalize or standardize your data so one variable doesn't over power another due to a difference in units. Feature scaling always applied to columns (not row data across columns)  
    - Normalize values from 0-1 (Xnorm=(X-Xmin)/Xrange)  # Works for normal distribution
    - Standardize values from -3 to 3 except outliers (Xstand=(X-Xave)/Xstdev)  # Works in most cases
# Use standardization since it works in all cases
# Apply scaling to numerical col.  Don't apply to category
```python
sc = StandardScaler()  # will calculate mean/std dev of columns for standardization
# fit will get the mean/sigma
# transform will modify data
# Note - only get fit on the training set
X_train[:, 3:] = sc.fit_transform(X_train[:, 3:]) # fit and transform. only apply to numerical col. Not the category in first 3 col
X_test[:, 3:] = sc.transform(X_test[:, 3:]) # do not create new fit. but apply the scalar transform calculated on training set
```

# Modeling
    - Build
    - Train
    - Make predictions
# Evaluation
    - Calculate performance metrics
    - Make a verdict





