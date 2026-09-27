## Definition  
  
**k-Nearest Neighbors (kNN)** is a supervised machine learning algorithm used for:  
  
- **Classification** → Predicting categories (Spam/Not Spam, Edible/Poisonous).  
- **Regression** → Predicting numerical values (House Price, Temperature).  
  
It is an **instance-based** and **lazy learning** algorithm because it does not build an explicit model during training. Instead, it stores the training data and makes predictions when new observations arrive.  
  
---  
  
## Core Idea  
  
To predict the label of a new observation:  
  
1. Calculate its distance to all training samples.  
2. Identify the **k nearest neighbors**.  
3. Use those neighbors to make a prediction.  
  
For classification:  
- Use **majority voting**.  
  
For regression:  
- Use the **average** of the neighbors' values.  
  
---  
  
## Distance Measurement  
  
The most common distance metric is **Euclidean Distance**:  
  
$$  
d = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}  
$$  
  
Where:  
- $(x_1,y_1)$ = first point  
- $(x_2,y_2)$ = second point  
  
Smaller distance means greater similarity.  
  
---  
  
## Classification Example  
  
Training Data:  
  
| Weight | Color Score | Fruit |  
|----------|----------|---------|  
| 150 | 0.9 | Apple |  
| 170 | 0.8 | Apple |  
| 300 | 0.2 | Orange |  
| 320 | 0.1 | Orange |  
  
New Observation:  
  
| Weight | Color Score |  
|----------|----------|  
| 180 | 0.7 |  
  
If **k = 3**:  
  
Nearest neighbors:  
  
- Apple  
- Apple  
- Orange  
  
Prediction:  
  
**Apple** (majority vote)  
  
---  
  
## Regression Example  
  
Nearest neighbors have house prices:  
  
- \$100,000  
- \$110,000  
- \$120,000  
  
Prediction:  
  
$$  
\frac{100000 + 110000 + 120000}{3}  
= 110000  
$$  
  
Predicted price = **\$110,000**  
  
---  
  
## The k Parameter  
  
### Small k  
  
Example:  
  
```python  
k = 1  
```  
  
Advantages:  
- Captures local patterns.  
- More flexible.  
  
Disadvantages:  
- Sensitive to noise.  
- Can overfit.  
  
---  
  
### Large k  
  
Example:  
  
```python  
k = 25  
```  
  
Advantages:  
- More stable.  
- Less sensitive to outliers.  
  
Disadvantages:  
- Can underfit.  
- May ignore important local patterns.  
  
---  
  
## Choosing the Best k  
  
A common approach is testing multiple values:  
  
```python  
for k in range(1, 31):  
...  
```  
  
Use:  
  
- Validation Set  
- Cross-Validation  
  
Select the value that produces the best performance.  
  
---  
  
## Why Feature Scaling Matters  
  
kNN is distance-based.  
  
Suppose:  
  
| Feature | Range |  
|-----------|-----------|  
| Age | 0–100 |  
| Income | 0–100000 |  
  
Income dominates the distance calculation.  
  
Therefore, features should usually be scaled.  
  
### StandardScaler Example  
  
```python  
from sklearn.pipeline import Pipeline  
from sklearn.preprocessing import StandardScaler  
from sklearn.neighbors import KNeighborsClassifier  
  
model = Pipeline([  
('scaler', StandardScaler()),  
('knn', KNeighborsClassifier(n_neighbors=5))  
])  
```  
  
---  
  
## Stratified Train/Test Split  
  
When dealing with classification problems, preserve class proportions:  
  
```python  
from sklearn.model_selection import train_test_split  
  
X_train, X_test, y_train, y_test = train_test_split(  
X,  
y,  
test_size=0.2,  
random_state=0,  
stratify=y  
)  
```  
  
`stratify=y` ensures the same class distribution in both training and test sets.  
  
---  
  
## Scikit-Learn Example  
  
```python  
from sklearn.neighbors import KNeighborsClassifier  
  
knn = KNeighborsClassifier(n_neighbors=5)  
  
knn.fit(X_train, y_train)  
  
print("Train Accuracy:", knn.score(X_train, y_train))  
print("Test Accuracy:", knn.score(X_test, y_test))  
```  
  
---  
  
## Advantages  
  
- Simple and intuitive.  
- No training phase.  
- Works well on small datasets.  
- Naturally supports multiclass classification.  
- Effective baseline algorithm.  
  
---  
  
## Disadvantages  
  
- Slow predictions on large datasets.  
- Requires feature scaling.  
- Sensitive to irrelevant features.  
- Requires storing all training data.  
- Performance decreases with many features.  
  
---  
  
## Curse of Dimensionality  
  
As the number of features increases:  
  
- Distances become less meaningful.  
- Points tend to appear equally distant.  
- Prediction quality often decreases.  
  
kNN generally performs best with a moderate number of relevant features.  
  
---  
  
## Key Concepts for Exams  
  
- Supervised Learning  
- Classification and Regression  
- Lazy Learning  
- Instance-Based Learning  
- Euclidean Distance  
- Majority Voting  
- Feature Scaling  
- Hyperparameter k  
- Overfitting (small k)  
- Underfitting (large k)  
- Curse of Dimensionality  
  
---  
  
## Quick Summary  
  
k-Nearest Neighbors (kNN) predicts the class or value of a new observation by examining the **k closest training samples** according to a distance metric. For classification it uses majority voting, and for regression it averages the neighbors' values. Because kNN relies on distances, feature scaling is essential. Small values of k may overfit, while large values may underfit. The algorithm is easy to understand and implement but becomes slower and less effective as the dataset size and number of features increase.