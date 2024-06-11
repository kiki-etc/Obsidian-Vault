# Supervised Learning
Supervised learning, also known as supervised machine learning, is a subcategory of machine learning and artificial intelligence. It is defined by its use of labeled data sets to train algorithms to classify data or predict outcomes accurately.

There are two main types:
- **Classification**: used when the output variable is a categorical label. It learns to assign input examples to one of several predefined classes
- **Regression**: Used when the output variable is a continuous value. It learns to predict a numerical value based on input features
## Regression
Machine Learning tasks where the main objective is value estimation can be termed as regression tasks. It is a supervised learning technique used to model the relationship between a dependent variable (target) and one or more independent variables (features). With this knowledge, it can predict output responses for new, unseen data instances similar to classification but with continuous numeric outputs.
<br>The main objective is to minimize errors during training and validating the model so that it generalizes well, does not overfit or get biased only to the training data and performs well in future predictions.
<br>Interpreting a regression model:
- Coefficient Interpretation (relevant for Linear Regression Algorithms)
- Residual Analysis
- Predictive Insights
- Decision-Making

Types of regression include Linear, Logistic, Polynomial, etc.
### Linear Models of Regression
Simple linear regression models try to model relationships on data with one feature or explanatory variable x and a single response variable y where the objective is to predict y. It is used to model (as a straight line) the relationship between an independent and a dependent variable. The result is a line of best fit which is an equation of our model.
<br>Simplest version: $y(x,w) = w_0 + w_1 x_1 + ... + w_D x_D$ 
<br>Two types: Simple Linear Regression (SLR) and Multilinear Regression (MLR)

>[!example] Example of using linear regression in code<br>
>
>```Python
>lm = linear_model.LinearRegression()
>model = lm.fit(X,y)
>predictions = predict(X)
>```

### Multiple Regression
It is also known as multivariable regression. These methods try to model data where there's one response output but multiple features that affect it in the form of a vector X. Based on all the attributes, the model tries to learn the relationship between each feature vector and its corresponding response output so that it can predict them in the future. 
### Polynomial Regression
It is another type of regression analysis. As opposed to linear, the  polynomial creates a best fit curve based on a given set of data values. It is a special case of multiple regression where the response variable is modeled as an *nth* degree polynomial of the input feature x. Basically it is multiple regression, where each feature in the input feature vector is a multiple of x.
It is very prone to overfitting (which occurs when a model equation fits too closely to the training data such that when new data is introduced it fails).

```Python
from sklearn.preprocessing import PolynomialFeatures

poly_features = PolynomialFeatures(degree = 2, include_bias = False)
X_poly = poly_features.fit_transform(X)

X[0]
X_poly[0]
lin_reg = LinearRegression()
lin_reg.fit(X_poly, y)
```
#### Other Regressors
Examples:
```Python
new_dt_model=DecisionTreeRegressor()  
new_dt_model.fit(imputed_X_train,y)  
dt_predictions=new_dt_model.predict(imputed_X_test)  

from sklearn.ensemble import RandomForestRegressor  
forest_model=RandomForestRegressor()  
forest_model.fit(train_X,train_y)  

xgb_model = XGBRegressor()  
xgb_model.fit(imputed_X_train_plus,y)
```
## Optimizing Regressors
Regularization is a technique used in regression models to avoid overfitting by shrinking the coefficient estimated to zero.
One prominent method for optimizing regressors seems to be to make $h(x)$ close to $y$. To understand this more formally, let's try defining a function that determines, for each value of the $θ$’s, how close the $h(x^{(i)})$'s are to the corresponding $y^{(i)}$'s. The function should look like the following:
<br>$J(θ) = \frac{1}{2} \sum_{i=1}^m (h_θ(x^{(i)})-y^{(i)})^2$ 

This is the ***cost function***.
## Gradient Descent Algorithm
The general idea of Gradient Descent is to tweak parameters iteratively in order to minimize a cost function.
<br>The algorithm measures the local gradient of the error function with regards to the parameter vector θ, and it goes in the direction of descending gradient. Once the gradient is zero, you have reached a minimum.
Concretely, you start by filling θ with random values (random initialization), and then you improve it gradually, taking small steps at a time, each step attempting to decrease the cost function (e.g., the MSE), until the algorithm converges to a minimum.

>[!note]- Hyperparameters
>An important parameter is the size of the steps, determined by the learning rate hyperparameter. A small learning rate implies several iterations before convergence. A high learning rate, however, could cause a jump across the minimum, which could cause divergence, with larger and larger values.

#### Types of Gradient Descent
➔ The method that looks at every example in the entire training set on every step is called batch gradient descent.
<br>➔ Alternatively, you repeatedly run through the training set and (each time you encounter a training example) you update the parameters according to the gradient of the error with respect to that single training example only. This algorithm is called stochastic gradient descent (also incremental gradient descent).
# Evaluation of Models
Common metrics for evaluation.
- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- Mean Absolute Percentage Error
<br>➔ MAE is the easiest to understand, because it's the average error.
➔ MSE is more popular than MAE, because MSE "punishes" larger errors, which tends to be useful in the real world.  
➔ RMSE is even more popular than MSE, because RMSE is interpretable in the "y" units.  
➔ All of these are **loss functions**, because we want to minimize them.

```Python
from sklearn import metrics
print('MAE: ', metrics.mean_absolute_error(y_test, predictions))
print('MSE: ', metrics.mean_squared_error(y_test, predictions))
print('RMSE: ', np.sqrt(metrics.mean_squared_error(y_test, predictions)))
```

## Classification and Predictive modelling
Classification is a supervised learning technique where the goal is to predict the categorical class tables of new instances.
<br>The automation process involves collecting and processing data, selecting models and training them, then evaluation and optimizing the model.
### Classification Matrix
#### Confusion Matrix
A confusion matrix is a table used to evaluate the performance of a classification model. It summarizes the outcomes of predictions and provides insight into the types of errors made by the model.
An example:

|       | T   | F   | Total |
| ----- | --- | --- | ----- |
| T     | 18  | 2   | 20    |
| F     | 3   | 15  | 18    |
| Total | 21  | 17  | 38    |
The table can be used to find the number of true negatives, true positives, false negatives and false positives.

|          | Predicted T    | Predicted F    |
| -------- | -------------- | -------------- |
| Actual T | True Positive  | False Negative |
| Actual F | False Positive | True Negative  |
>[!note] Recall vs. Precision
>Recall = how many 'positive' values were correctly identified
>Precision = how many values predicted to be 'positive' were actually positive
>
#### The AUC/ROC (Area Under Curve/Receiver Operating Characteristic)
When evaluating classification matrices, it is best to use F1 score and the AUC.

### Attribute Selection and Decision Tree Algorithms
