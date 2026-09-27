
## 1. Cross-Validation  
  
### ¿Qué es?  
  
Cross-validation (validación cruzada) es una técnica para evaluar qué tan bien generaliza un modelo a datos nuevos.  
  
La idea es dividir el conjunto de entrenamiento en varios grupos (*folds*):  
  
1. Entrenar con algunos folds.  
2. Validar con el fold restante.  
3. Repetir el proceso varias veces.  
4. Calcular el promedio de los resultados.  
  
Esto produce una estimación más confiable del rendimiento que una única división train/test.  
  
---  
  
### Clase utilizada  
  
```python  
from sklearn.model_selection import cross_val_score  
```  
  
---  
  
### Sintaxis básica  
  
```python  
scores = cross_val_score(  
estimator=modelo,  
X=X_train,  
y=y_train,  
cv=5  
)  
```  
  
---  
  
### Argumentos principales  
  
| Argumento | Descripción |  
|------------|------------|  
| `estimator` | Modelo a evaluar |  
| `X` | Variables de entrada |  
| `y` | Variable objetivo |  
| `cv` | Número de folds |  
  
---  
  
### Ejemplo  
  
```python  
from sklearn.linear_model import LogisticRegression  
from sklearn.model_selection import cross_val_score  
  
model = LogisticRegression(max_iter=1000)  
  
scores = cross_val_score(  
model,  
X_train,  
y_train,  
cv=5  
)  
  
print(scores)  
print(scores.mean())  
```  
  
---  
  
## 2. Grid Search  
  
### ¿Qué es?  
  
Grid Search es una técnica para encontrar automáticamente la mejor combinación de hiperparámetros.  
  
En lugar de probar manualmente:  
  
```python  
C = 0.1  
C = 1  
C = 10  
```  
  
Grid Search prueba todas las combinaciones especificadas y selecciona la mejor.  
  
---  
  
### Clase utilizada  
  
```python  
from sklearn.model_selection import GridSearchCV  
```  
  
---  
  
### Sintaxis básica  
  
```python  
grid = GridSearchCV(  
estimator=modelo,  
param_grid=parametros,  
cv=5  
)  
```  
  
---  
  
### Argumentos principales  
  
| Argumento | Descripción |  
|------------|------------|  
| `estimator` | Modelo a optimizar |  
| `param_grid` | Diccionario con hiperparámetros a probar |  
| `cv` | Número de folds para validación cruzada |  
| `scoring` | Métrica opcional (accuracy, precision, recall, f1, etc.) |  
  
---  
  
### Ejemplo  
  
```python  
from sklearn.svm import SVC  
from sklearn.model_selection import GridSearchCV  
  
param_grid = {  
"C": [0.1, 1, 10],  
"gamma": [0.001, 0.01, 0.1]  
}  
  
grid = GridSearchCV(  
estimator=SVC(),  
param_grid=param_grid,  
cv=5  
)  
  
grid.fit(X_train, y_train)  
```  
  
---  
  
## 3. Resultados de Grid Search  
  
### Mejor combinación de hiperparámetros  
  
```python  
grid.best_params_  
```  
  
Ejemplo:  
  
```python  
{'C': 10, 'gamma': 0.01}  
```  
  
---  
  
### Mejor score de cross-validation  
  
```python  
grid.best_score_  
```  
  
Ejemplo:  
  
```python  
0.972  
```  
  
Este valor es el promedio obtenido durante la validación cruzada usando el mejor conjunto de hiperparámetros.  
  
---  
  
### Evaluación final sobre test  
  
```python  
grid.score(X_test, y_test)  
```  
  
Ejemplo:  
  
```python  
0.965  
```  
  
Este score se calcula usando datos que nunca participaron en el entrenamiento ni en la búsqueda de hiperparámetros.  
  
---  
  
## Flujo típico  
  
```text  
Train/Test Split  
↓  
Escalado (si es necesario)  
↓  
GridSearchCV(cv=5)  
↓  
best_params_  
↓  
best_score_  
↓  
score(X_test, y_test)  
```  
  
---  
  
## Resumen  
  
### Cross-Validation  
  
Clase:  
  
```python  
cross_val_score  
```  
  
Objetivo:  
  
> Evaluar la capacidad de generalización de un modelo.  
  
---  
  
### Grid Search  
  
Clase:  
  
```python  
GridSearchCV  
```  
  
Objetivo:  
  
> Encontrar automáticamente los mejores hiperparámetros usando validación cruzada.  
  
---  
  
### Diferencia principal  
  
```text  
cross_val_score  
↓  
Evalúa un modelo  
  
GridSearchCV  
↓  
Busca los mejores hiperparámetros y evalúa cada combinación mediante cross-validation  
```