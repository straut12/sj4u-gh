---
layout: page
permalink: /ref/data-analysis/ml/
title: ml
description: Machine Learning
nav: false
toc: true
---

**Machine Learning**   
Notes from udemy ML A-Z  
Will be using scikit-learn which is easier to learn/use vs Tensorflow at the expense of being less powerful/flexible.  

General Steps  
- Data pre-processing
- Build (Regression or Classification)
- Train  
- Make predictions  
- Calculate performance metrics
- Make a verdict

Two types - Regression is when you predict a continuous real value. Classification is predicting a category. Regression models are listed first below and then Classification. 

Some libraries using [scikit](https://en.wikipedia.org/wiki/Scikit-learn)  

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

from sklearn.linear_model import LinearRegression   # Single/Multiple linear regression
from sklearn.preprocessing import PolynomialFeatures # Used for Polynomial linear regression
from sklearn.svm import SVR                 # Support vector regression
from sklearn.tree import DecisionTreeRegressor # Decision tree regression
from sklearn.ensemble import RandomForestRegressor # Random forest

from sklearn.linear_model import LogisticRegression # Logistic regression

```
# Data Pre-Processing
## Import
Features (independent variables) vs Dependent variable vector.  
- Features/independent variables are the columns you use to predict the dependent variable (typically last column).  

```python
df = pd.read_csv('Data.csv')
X = df.iloc[:, :-1].values  # all rows/col except the last col (all feature/independent variables )
y = df.iloc[:, -1].values # dependent variable vector. last col
```

## Scrub  
What to do with bad/missing data
- Can simply leave it out if <1%
- Use imputer module to replace it with average data from that column    

```python
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
imputer.fit(X[:, 1:3])  # fit will look for missing values and calculate ave on X col 1,2
X[:, 1:3] = imputer.transform(X[:, 1:3]) # transform will perform the replacement
```  

## Encode Categorical Data  
OneHotEncoder will take a category col (ie a col with 3 unique tool IDs) and create multiple columns to represent them as 1/0. So a col with 3 unique tool IDs would become 3 col with 1,0,0 and 0,1,0 and 0,0,1  

ColumnTransformer takes 2 arg
1. what kind of transformation and which index to perform it on  
2. remainder or which cols to keep that won't have transformation applied  

Example below using 'encoder' transformation with OneHotEncoder class on column 0 which is string type. 'passthrough' says to keep the columns that were not listed in the transform   

```python
# Remember toupdate the 3rd arg with the column to do OneHotEncoding on
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), [0])], remainder='passthrough')
X = np.array(ct.fit_transform(X))  # does fit and transform at same time. Must keep X as numpy array

# For single vector, dependent variable, use label encoder to convert yes/no to 0/1 (binary)
le = LabelEncoder()
y = le.fit_transform(y)  # does not have to be numpy array
```

## Training and Test Sets  
Separating your data into two sets, training and test, allows you to compare the predicted values from the model to the actual values.  
A starting point is to use 80% for training and the remaining data for testing.    

Will create 4 sets.  
- Pair of matrix features (Xtrain) and dependent variable (Ytrain) for train set.   
- Pair of matrix features (Xtest) and dependent variable (Ytest) for test set.   

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 1)
# 80% observations go to training and 20% to test
# random_state=1 for learning so will return same set as tutorial
# random_state=0 for non tutorial
```

## Feature scaling 
Scaling may be needed to normalize or standardize your data so one variable doesn't over power another (one col ranges from 500-10500 and another ranges from 1-50). Typically not used for a lot of regression models because the coeffecients can compensate for high values of features. But useful on other models.  

Feature scaling is applied after split. Your test split is supposed to represent new data that you don't have until training is done so you don't want to modify it.  

Feature scaling always applied to columns (not row data across columns)  
* Normalize values from 0-1 (Xnorm=(X-Xmin)/Xrange)  # Works for normal distribution  
* Standardize values from -3 to 3 except outliers (Xstand=(X-Xave)/Xstdev)  # Works in most cases   

Standardization is a good choice since it works in most cases. (do not apply to category col) 

```python
sc = StandardScaler()  # will calculate mean/std dev of columns for standardization
# fit will get the mean/sigma
# transform will modify data
# Note - only get fit on the training set
X_train[:, 3:] = sc.fit_transform(X_train[:, 3:]) # fit and transform. only apply to numerical col. Not the category in first 3 col
X_test[:, 3:] = sc.transform(X_test[:, 3:]) # do not create new fit. but apply the scalar transform calculated on training set
```

# Modeling (Regression)  

Details on concepts and 5 different methods  
1. All-in - Have prior knowledge or preparing for Backward Elimination
2. Backward Elimination (* Stepwise Regression)
    1. Select a significance level to STAY in the model (0.05)
    2. Fit the full model with all possible predictors
    3. Consider the predictor with hightest P value. If P > SL go to next step, else finish (when all variables P < SL then model is finished).
    4. Remove the predictor
    5. Fit model with this variable. You have to rebuild/refit the model with the fewer number of variables. Will get new coefficients/constants.
    Keep looping steps 3-5. Remove predictor with highest P val. Fit the model again with one less variable. Keep repeating until variable with highest P is less than the SL.
3. Forward Selection. Growing one variable at a time  (* Stepwise Regression)
    1. Select a SL to ENTER the model (SL= 0.05)
    2. Fit all simple regression models. Fit y to all the separate X features. Select the one with the lowest P value
    3. Keep this variable and fit all possible models with one extra predictor added to the one(s) you already have
    4. Consider the predictor with the lowest P-value. If P < SL, go to step 3, else finish and keep the previous model before adding the last var.
4. Bidirectional Elimination - Combine methods 2-3 (** Stepwise Regression)  
    1. Select a SL to ENTER and to STAY in the model (.05)
    2. Perform the next step of Forward Selection (new var must have : P < SLENTER to enter)
    3. Perform ALL steps of Backward Elimination (old variables must have P < SLSTAY to stay)
    4. No new variables can enter and no old variables can exit  
5. Score Comparison - All Possible Models
    1. Select a criterion of goodness of fit
    2. Construct all possible regression models
    3. Select the one with the best criterion  

## Linear Regression
ý=b₀+b₁X₁  
- ý is dependent var
- X₁ is independent var (feature)
- b₀ is y-intercept (constant)
- b₁ is slope coefficient  

Line is derived using ordinary least squares. Minimize error. Sum of squares. 
- residual Ɛᵢ=yᵢ-ýᵢ 
- sum(yᵢ-ýᵢ)² is minimized  

Assumptions  
- Linear relationship between Y and each X
- Equal variance (Homoscedasticity) - Can not show cone shape in variation or showing variation is dependent on X
- Normality of error distribution (Multivariate Normality) - If you look along the line of linear regression would want to see normal distribution.  
- Independence (no autocorrelation) - No patterns in the data. ie stock market where previous prices affect future  
- Lack of Multicollinearity - Predictors are not correlated to each other. See category example below. 
- Outlier check  

Examples of linear regression, deviation to model fit, cubic polynomial fit, and unequal variation.  
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/chart.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/chart2.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/chart3.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/Heteroscedasticity.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>  

[By Krishnavedala - Own work, CC BY-SA 3.0, ](https://commons.wikimedia.org/w/index.php?curid=15462765)
[By Skbkekas - Own work, CC BY 3.0, ](https://commons.wikimedia.org/w/index.php?curid=6457163)
[Heteroscedasticity By Q9 at the English-language Wikipedia, CC BY-SA 3.0,](https://commons.wikimedia.org/w/index.php?curid=18064846)   

```python
from sklearn.linear_model import LinearRegression   # Single/Multiple linear regression
# Using LineaerRegression. Will avoid dummy variable issue
regressor = LinearRegression() # Build linear regression model and create object
regressor.fit(X_train, y_train) # Train on the training set using fit method

# Predict method expect 2D array
y_pred = regressor.predict(X_test) # Predict test results using predict method.

# Plot training set
plt.scatter(X_train, y_train, color = 'red')
plt.plot(X_train, regressor.predict(X_train), color = 'blue') # use plot to graph function, the regression line
plt.title('Salary vs Experience (Training set)')
plt.xlabel('Years of Experience')
plt.ylabel('Salary')
plt.show()

# Plot test set
plt.scatter(X_test, y_test, color = 'red')
plt.plot(X_train, regressor.predict(X_train), color = 'blue') # regression line between test and train will be the same. Could plot either
plt.title('Salary vs Experience (Test set)')
plt.xlabel('Years of Experience')
plt.ylabel('Salary')
plt.show()

# Get value
# Predict method expect 2D array
print(regressor.predict([[12]])) # prediction for 12yr experience. 2D array

# Print equation coefficients
print(regressor.coef_)
print(regressor.intercept_)

# salary = 26816 + 9345(year experience)

```   

## Multiple Linear Regression  
ý=b₀+b₁X₁+b₂X₂+..bₙXₙ + b₄D₁
- Do not need to apply feature scaling
- Do not need to test for linear regression assumptions. It will just show a lower score and you will need to pick a different model.
- ý is dependent var
- X₁ is independent var (feature matrix) for each col or X
- b₀ is y-intercept (constant)
- b₁ is slope coefficient
- D₁ is category - need to find how many unique values are in the col and split into multiple Dummy binary Variable columns (0,1). This LinearRegression class will omit one of the dummy variable col. If you have 4 then include 3. If 2 then include 1. Why? If 2 unique values only include 1 dummy col, not both. D2=1-D1 so you have a case of independent variable predicting another is Multicollinearity.

> LinearRegression class will iterate thru multiple features to find highest P value  

There are multiple columns/features and we can not visualize the multidimensional output so use vectors to compare. Compare vector of the predicted vs vector of the test set result.  

```python  
# Using LineaerRegression. 
from sklearn.linear_model import LinearRegression   # Single/Multiple linear regression
# Will also iterate thru multiple features to find highest P value
regressor = LinearRegression() # Build linear regression model and create object
regressor.fit(X_train, y_train) # Train on the training set using fit method

y_pred = regressor.predict(X_test) # Predict test results using predict method.

np.set_printoptions(precision=2)
# numpy concactanate allows to concatenate vectors or arrays either horizontally or vertically. axis=0,1 for vert/horiz
# Will use it to concatanate vertically the predicted and real profits.
# Pass vector of predicted result and real result (y_pred and y_test)
# Use reshape to go from horiz to vert and 1 is for 1 col.
print(np.concatenate((y_pred.reshape(len(y_pred),1), y_test.reshape(len(y_test),1)),1))

# Single prediction
# Predict method expect 2D array
print(regressor.predict([[1, 0, 0, 160000, 130000, 300000]]))
"""
double pair of square brackets.  "predict" method expects 2D array as the format

1,0,0,160000,130000,300000→scalars
[1,0,0,160000,130000,300000]→1D array
[[1,0,0,160000,130000,300000]]→2D array
"""

# Get coefficients
print(regressor.coef_)
print(regressor.intercept_)

"""Profit=86.6×Dummy State 1−873×Dummy State 2+786×Dummy State 3+0.773×R&D Spend+0.0329×Administration+0.0366×Marketing Spend+42467.53
"""
```  

## Polynomial Linear Regression  
Polynomial linear regression is just a type of multiple linear regression.  
It is still a linear model   
ý=b₀+b₁X₁+b₂X₁²+..bₙX₁ⁿ
- X portion is not linear .. but Linear regression applies to the coefficients (b1) part. Trying determine the coefficients so can plug in varying X values and determine the output.  
- The linear/non-linear refers to the coefficients (b) and if the function can be expressed as a linear combination of coefficients.  
- Do not need to apply feature scaling
- Do not need to test for linear regression assumptions. It will just show a lower score and you will need to pick a different model.
- ý is dependent var
- X₁ is independent var (feature matrix) for each col or X
- b₀ is y-intercept (constant)
- b₁ is slope coefficient

```python
from sklearn.linear_model import LinearRegression   # Single/Multiple linear regression
from sklearn.preprocessing import PolynomialFeatures
# Training the Linear Regression model on the whole dataset
lin_reg = LinearRegression()
# X,y is not broken into train/test set because all data is being used to create the model.
# The goal is to determine salary between level 6 and 7 so want all the data for the model.
lin_reg.fit(X, y)

# Training the Polynomial Regression model on the whole dataset
# X=position levels and Y=salaries
poly_reg = PolynomialFeatures(degree = 4) # enter the order. start with 2nd, 3rd, 4th
X_poly = poly_reg.fit_transform(X)
lin_reg_2 = LinearRegression()
lin_reg_2.fit(X_poly, y)  

# Visualize the linear and polynomial results
plt.scatter(X, y, color = 'red')
plt.plot(X, lin_reg.predict(X), color = 'blue')
plt.title('Truth or Bluff (Linear Regression)')
plt.xlabel('Position Level')
plt.ylabel('Salary')
plt.show()

plt.scatter(X, y, color = 'red')
plt.plot(X, lin_reg_2.predict(X_poly), color = 'blue')
plt.title('Truth or Bluff (Polynomial Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()

# Higher resolution and smoother curve
X_grid = np.arange(min(X), max(X), 0.1)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(X, y, color = 'red')
plt.plot(X_grid, lin_reg_2.predict(poly_reg.fit_transform(X_grid)), color = 'blue')
plt.title('Truth or Bluff (Polynomial Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()

# Predict new results with linear regression
# Predict method expect 2D array
lin_reg.predict([[6.5]])

# Predict new results with polynomial regression
# Can not just enter X, but have to enter X, X^2, X^3, X^4
# This is in the X_poly or poly_reg.fit_transform(X)
lin_reg_2.predict(poly_reg.fit_transform([[6.5]]))

```  

## Support Vector Regression  
Apply a buffer margin or tube where the error does not matter. But points outside the tube are support vectors and dictate the tube.  

Example will have feature scaling.  Job class feature was from 1-15 and dependent Y salary was 45k-1M so had to scale the variables.

```python
from sklearn.svm import SVR

y = y.reshape(len(y),1) # used to get array into 2D array with 1 col for standard scalar function

# StandardScaler will calculate mean/std dev of columns for standardization
sc_X = StandardScaler()   
sc_y = StandardScaler()
X = sc_X.fit_transform(X)  # scale the X col feature and Y dependent var separately
y = sc_y.fit_transform(y)

regressor = SVR(kernel = 'rbf')  # Gaussian radial basis function kernel
regressor.fit(X, y)

# Need to reverse the scaling to get original scale
# predict method expect 2D array
sc_y.inverse_transform(regressor.predict(sc_X.transform([[6.5]])).reshape(-1,1))

plt.scatter(sc_X.inverse_transform(X), sc_y.inverse_transform(y), color = 'red')
plt.plot(sc_X.inverse_transform(X), sc_y.inverse_transform(regressor.predict(X).reshape(-1,1)), color = 'blue')
plt.title('Truth or Bluff (SVR)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()

# Higher resolution
X_grid = np.arange(min(sc_X.inverse_transform(X)), max(sc_X.inverse_transform(X)), 0.1)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(sc_X.inverse_transform(X), sc_y.inverse_transform(y), color = 'red')
plt.plot(X_grid, sc_y.inverse_transform(regressor.predict(sc_X.transform(X_grid)).reshape(-1,1)), color = 'blue')
plt.title('Truth or Bluff (SVR)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
```  

## Decision Tree Regression
CART (Classification and Regression Tree)  
Scaling is not required since predictions from decision tree regression are resulting from successive splits of the data through nodes of the tree and not equations like other models.    
Decision tree is not ideal for single feature data set. Best for many data cols.  

```python
from sklearn.tree import DecisionTreeRegressor

regressor = DecisionTreeRegressor(random_state = 0)
regressor.fit(X, y)

regressor.predict([[6.5]])

# High resolution
X_grid = np.arange(min(X), max(X), 0.01)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(X, y, color = 'red')
plt.plot(X_grid, regressor.predict(X_grid), color = 'blue')
plt.title('Truth or Bluff (Decision Tree Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
```  

## Random Forest
Ensemble. Build a lot of decision trees. Instead of just getting one prediction you get a lot of predictions (500+). Average across the predictions.   
1. Pick at random K data points from training set
2. Build the decision tree associated to these K data points
3. Choose the number Ntree of trees you want to build and repeat steps 1-2
4. For a new data point, make each one of Ntree trees predic the valueo of Y for the data point in question and assign the new data point the average across all of the predicted Y values.  

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.ensemble import RandomForestRegressor
regressor = RandomForestRegressor(n_estimators = 10, random_state = 0)
regressor.fit(X, y)

regressor.predict([[6.5]])

X_grid = np.arange(min(X), max(X), 0.01)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(X, y, color = 'red')
plt.plot(X_grid, regressor.predict(X_grid), color = 'blue')
plt.title('Truth or Bluff (Random Forest Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
```  

# Evaluation  
Residual sum of squares 
Minimize error. Sum of squares.  
- residual Ɛᵢ=yᵢ-ýᵢ 
- sum(yᵢ-ýᵢ)² is minimized  
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/coding/r-squared.jpg" class="img-fluid rounded z-depth-1" %}
</div>
[By Orzetto - Own work, CC BY-SA 3.0,](https://commons.wikimedia.org/w/index.php?curid=11398293)  

- R² > 0.9 is good
- R² < 0.7 its marginal
- R² ~ 0.4 is bad

ý=b₀+b₁X₁+b₂X₂+..bₙXₙ  

As you add more features/col (X's) the SStot does not change but the SSres will decrease or stay the same. This is a problem because as you add more columns of data/features your R² will appear to improve even though they may not be relevant or adding value.  

So need to use adjusted R².   
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/coding/adj-r-squared.jpg" class="img-fluid rounded z-depth-1" %}
</div>
n - sample size
p - number of independent variables. as more variables add the R² will actually decrease unless the new variable improves the fit.  

```python
from sklearn.metrics import r2_score
r2_score(y_test, y_pred)
```  

Run your data through all models and compare R²  

# Modeling (Classification)
Classification is a ML method to identify the category of new observations based on training data.  

## Logistic Regression
Logistic Regression will give a probability. Can split into two categories >50% or < 50% and get a binary 0,1 output.  

<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/coding/logistic-regression.jpg" class="img-fluid rounded z-depth-1" %}
</div>
[By Canley - Own work, CC BY-SA 4.0, ](https://commons.wikimedia.org/w/index.php?curid=116449187)
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/coding/logistic-regression2.jpg" class="img-fluid rounded z-depth-1" %}
</div>


```python
from sklearn.preprocessing import StandardScaler

# scaling
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test) # do not create new fit. but apply the scalar transform calculated on training set

from sklearn.linear_model import LogisticRegression
classifier = LogisticRegression(random_state = 1)
classifier.fit(X_train, y_train)

# Predicting
print(classifier.predict(sc.transform([[30,87000]]))) # Have to use transform since model had scaled values used for training

# print out a matrix of the prediction (Y predicted) vs the actual (Y test)
y_pred = classifier.predict(X_test)
print(np.concatenate((y_pred.reshape(len(y_pred),1), y_test.reshape(len(y_test),1)),1))

# Confusion Matrix
from sklearn.metrics import confusion_matrix, accuracy_score
cm = confusion_matrix(y_test, y_pred)
print(cm)
"""
[[65  3]  # There are 65 correct predictions of 0 (3 incorrect)
 [ 8 24]] # There are 24 correct predictions of 1 (8 incorrect)
          # Total of 89 correct predictions
"""
accuracy_score(y_test, y_pred) # accuracy score was 0.89 or 89% correct. There were 
# 100 samples in the test set. 65 + 24 = 89 (89% of 100)

# Visualize training set
from matplotlib.colors import ListedColormap
X_set, y_set = sc.inverse_transform(X_train), y_train
X1, X2 = np.meshgrid(np.arange(start = X_set[:, 0].min() - 10, stop = X_set[:, 0].max() + 10, step = 0.25),
                     np.arange(start = X_set[:, 1].min() - 1000, stop = X_set[:, 1].max() + 1000, step = 0.25))
plt.contourf(X1, X2, classifier.predict(sc.transform(np.array([X1.ravel(), X2.ravel()]).T)).reshape(X1.shape),
             alpha = 0.75, cmap = ListedColormap(('red', 'green')))
plt.xlim(X1.min(), X1.max())
plt.ylim(X2.min(), X2.max())
for i, j in enumerate(np.unique(y_set)):
    plt.scatter(X_set[y_set == j, 0], X_set[y_set == j, 1], c = ListedColormap(('red', 'green'))(i), label = j)
plt.title('Logistic Regression (Training set)')
plt.xlabel('Age')
plt.ylabel('Estimated Salary')
plt.legend()
plt.show()

# Visualize Test set
from matplotlib.colors import ListedColormap
X_set, y_set = sc.inverse_transform(X_test), y_test
X1, X2 = np.meshgrid(np.arange(start = X_set[:, 0].min() - 10, stop = X_set[:, 0].max() + 10, step = 0.25),
                     np.arange(start = X_set[:, 1].min() - 1000, stop = X_set[:, 1].max() + 1000, step = 0.25))
plt.contourf(X1, X2, classifier.predict(sc.transform(np.array([X1.ravel(), X2.ravel()]).T)).reshape(X1.shape),
             alpha = 0.75, cmap = ListedColormap(('red', 'green')))
plt.xlim(X1.min(), X1.max())
plt.ylim(X2.min(), X2.max())
for i, j in enumerate(np.unique(y_set)):
    plt.scatter(X_set[y_set == j, 0], X_set[y_set == j, 1], c = ListedColormap(('red', 'green'))(i), label = j)
plt.title('Logistic Regression (Test set)')
plt.xlabel('Age')
plt.ylabel('Estimated Salary')
plt.legend()
plt.show()
```

K-Nearest Neighbors (K-NN)
Support Vector Machine (SVM)
Kernel SVM
Naive Bayes
Decision Tree Classification
Random Forest Classification





