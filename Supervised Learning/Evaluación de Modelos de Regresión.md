---
date: 2026-06-11
area: tech
tags: [machine-learning, estadística]
status: evergreen
aliases: [Métricas de Regresión, MSE, RMSE, MAE, R2 Score]
related: [[Regresión Lineal]], [[Basics of Machine Learning]], [[Confusion Matrix]]
source: ""
---

# Evaluación de Modelos de Regresión

## Definición
A diferencia de los modelos de clasificación, donde el objetivo es predecir categorías, los modelos de **regresión** intentan predecir valores numéricos continuos (precios, temperaturas, ventas). Para evaluar su desempeño, utilizamos métricas que cuantifican la "distancia" o error entre los valores reales ($y$) y las predicciones ($\hat y$).

## Por qué importa
No existe una única métrica perfecta. Cada una penaliza de forma distinta los errores (errores grandes vs. pequeños) y es sensible a la escala de los datos o a la presencia de valores atípicos (*outliers*). Elegir la métrica correcta asegura que el modelo se optimice para el problema de negocio real.

---

## Desarrollo Técnico

### El Concepto de Residual (Error)
El error o residual es la diferencia individual entre el valor real y el predicho:
$$Error_i = y_i - \hat y_i$$

### Principales Métricas de Error

#### 1. MSE (Mean Squared Error)
Promedio de los errores elevados al cuadrado.
$$MSE = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat y_i)^2$$
> [!WARNING] Características
> - Penaliza fuertemente errores grandes.
> - Difícil de interpretar directamente (unidades al cuadrado).

#### 2. RMSE (Root Mean Squared Error)
Es la raíz cuadrada del MSE.
$$RMSE = \sqrt{MSE}$$
> [!TIP] Ventaja
> Devuelve el error a la **escala original** de los datos (ej. dólares, grados). Es la métrica más común en ingeniería.

#### 3. MAE (Mean Absolute Error)
Promedio del valor absoluto de los errores.
$$MAE = \frac{1}{N} \sum_{i=1}^{N} |y_i - \hat y_i|$$
- Menos sensible a outliers que el RMSE. Todos los errores pesan proporcionalmente.

#### 4. MedAE (Median Absolute Error)
Mediana de los errores absolutos. Es **extremadamente robusta** ante outliers extremos.

---

### Coeficiente de Determinación ($R^2$)
Indica qué porcentaje de la variabilidad de los datos es explicada por el modelo comparado con predecir siempre el promedio.
$$R^2 = 1 - \frac{SSE}{SST}$$
- **$R^2 = 1$:** Predicción perfecta.
- **$R^2 = 0$:** Igual que predecir el promedio.
- **$R^2 < 0$:** El modelo es peor que usar el promedio.

---

## Implementación en Scikit-Learn

```python
from sklearn.metrics import r2_score, mean_squared_error, mean_absolute_error
import numpy as np

y_pred = model.predict(X_test)

# Cálculo de métricas
r2 = r2_score(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
mae = mean_absolute_error(y_test, y_pred)

print(f"R2: {r2:.4f}")
print(f"RMSE: {rmse:.2f}")
```

> [!IMPORTANT] El método `.score()`
> En regresores (ej. `LinearRegression`), `model.score()` devuelve **$R^2$**, no Accuracy.

---

## Resumen de Selección

| Si quieres... | Usa... |
| :--- | :--- |
| **Castigar mucho errores grandes** | RMSE / MSE |
| **Robustez ante algunos outliers** | MAE |
| **Robustez ante muchos outliers** | MedAE |
| **Entender el % de varianza explicada** | $R^2$ |

---

## Conexiones
- [[Regresión Lineal]] — Algoritmo base de regresión.
- [[Basics of Machine Learning]] — Flujo general de evaluación.
- [[Confusion Matrix]] — Equivalente para clasificación.
- [[CGI - Machine Learning]] — MOC de Machine Learning.
