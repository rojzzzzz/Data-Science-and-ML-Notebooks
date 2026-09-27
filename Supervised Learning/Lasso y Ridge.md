  
## Why Do We Need Regularization?  
  
When a regression model becomes too complex, it may fit not only the underlying pattern in the data but also random noise.  
  
This phenomenon is called **overfitting**.  
  
Symptoms:  
  
- Very high performance on training data.  
- Lower performance on test data.  
- Poor generalization to unseen observations.  
  
Regularization helps reduce overfitting by penalizing large coefficients.  
  
---  
  
# Linear Regression Review  
  
Multiple Linear Regression attempts to find coefficients that minimize the Residual Sum of Squares (RSS):  
  
$$  
RSS = \sum_{i=1}^{n}(y_i - \hat{y}_i)^2  
$$  
  
Model:  
  
$$  
y = b + w_1x_1 + w_2x_2 + \cdots + w_nx_n  
$$  
  
---  
  
# Ridge Regression (L2 Regularization)  
  
## Definition  
  
Ridge Regression adds a penalty proportional to the squared magnitude of the coefficients.  
  
Objective Function:  
  
$$  
RSS + \lambda \sum_{j=1}^{n} w_j^2  
$$  
  
Where:  
  
- RSS = prediction error  
- λ (lambda) = regularization strength  
- w = model coefficients  
  
---  
  
## Effect of Ridge  
  
Ridge:  
  
- Shrinks coefficients toward zero.  
- Rarely makes coefficients exactly zero.  
- Keeps all variables in the model.  
- Reduces model complexity.  
  
Example:  
  
| Variable | Linear | Ridge |  
|-----------|---------|--------|  
| x1 | 10 | 7 |  
| x2 | -8 | -5 |  
| x3 | 4 | 2 |  
  
---  
  
## Advantages  
  
- Reduces overfitting.  
- Handles multicollinearity well.  
- Works well when most variables contain useful information.  
  
---  
  
# Lasso Regression (L1 Regularization)  
  
## Definition  
  
Lasso Regression adds a penalty proportional to the absolute value of the coefficients.  
  
Objective Function:  
  
$$  
RSS + \lambda \sum_{j=1}^{n}|w_j|  
$$  
  
---  
  
## Effect of Lasso  
  
Lasso:  
  
- Shrinks coefficients.  
- Can force coefficients to become exactly zero.  
- Performs automatic feature selection.  
  
Example:  
  
| Variable | Linear | Lasso |  
|-----------|---------|--------|  
| x1 | 10 | 7 |  
| x2 | -8 | 0 |  
| x3 | 4 | 0 |  
  
The variables whose coefficients become zero are effectively removed from the model.  
  
---  
  
## Advantages  
  
- Reduces overfitting.  
- Produces simpler models.  
- Automatically identifies important features.  
  
---  
  
# The Role of λ (Lambda)  
  
Lambda controls the amount of regularization.  
  
### Small λ  
  
```text  
Behaves similarly to Linear Regression  
```  
  
### Large λ  
  
```text  
Strong coefficient shrinkage  
```  
  
### Extremely Large λ  
  
```text  
Underfitting  
```  
  
The model becomes too simple and loses predictive power.  
  
---  
  
# Why Does Ridge Often Perform Better on Test Data?  
  
A common observation:  
  
```text  
Linear Regression  
Train R² = 0.95  
Test R² = 0.82  
  
Ridge Regression  
Train R² = 0.93  
Test R² = 0.89  
```  
  
At first glance this seems strange because the regularization term is only applied during training.  
  
However:  
  
- Regularization changes the coefficients learned during training.  
- Those coefficients are then used for both training and test predictions.  
- The resulting model is less sensitive to noise.  
- Therefore it generalizes better to unseen data.  
  
Important:  
  
> Ridge does not apply regularization during testing. The regularization influences the coefficients learned during training.  
  
---  
  
# Linear Regression vs Ridge vs Lasso  
  
| Characteristic | Linear | Ridge | Lasso |  
|---------------|---------|--------|--------|  
| Regularization | No | L2 | L1 |  
| Shrinks Coefficients | No | Yes | Yes |  
| Eliminates Variables | No | No | Yes |  
| Handles Multicollinearity | Moderate | Excellent | Good |  
| Feature Selection | No | No | Yes |  
| Overfitting Control | Low | High | High |  
  
---  
  
# Scikit-Learn Implementation  
  
## Linear Regression  
  
```python  
from sklearn.linear_model import LinearRegression  
  
model = LinearRegression()  
model.fit(X_train, y_train)  
```  
  
---  
  
## Ridge Regression  
  
```python  
from sklearn.linear_model import Ridge  
  
model = Ridge(alpha=1.0)  
model.fit(X_train, y_train)  
```  
  
---  
  
## Lasso Regression  
  
```python  
from sklearn.linear_model import Lasso  
  
model = Lasso(alpha=1.0)  
model.fit(X_train, y_train)  
```  
  
Note:  
  
```text  
alpha = λ  
```  
  
in Scikit-Learn.  
  
---  
  
# Key Takeaways  
  
- Ridge uses an L2 penalty.  
- Lasso uses an L1 penalty.  
- Both help reduce overfitting.  
- Ridge keeps all features but shrinks coefficients.  
- Lasso can eliminate irrelevant features.  
- Regularization acts during training, but improves test performance through better generalization.  
- The regularization strength is controlled by λ (alpha).  
- Lasso is useful for feature selection.  
- Ridge is useful when most variables are relevant and correlated.