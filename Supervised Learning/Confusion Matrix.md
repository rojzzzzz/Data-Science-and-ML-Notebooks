---
date: 2026-06-11
area: tech
tags: [machine-learning, estadística]
status: evergreen
aliases: [Matriz de Confusión, Accuracy, Precision, Recall, F1 Score]
related: [[CGI - Machine Learning]], [[Regresión Logística]]
source: ""
---

# Evaluación de Modelos de Clasificación

## Definición
La **Confusion Matrix** (Matriz de Confusión) es una herramienta fundamental en Machine Learning para evaluar el desempeño de un modelo de clasificación. Es una tabla que permite visualizar el desempeño de un algoritmo comparando los valores reales con las predicciones del modelo.

## Por qué importa
Permite entender no solamente cuántas predicciones fueron correctas, sino también qué tipo de errores está cometiendo el modelo. Esto es crucial cuando los costos de diferentes tipos de errores (Falsos Positivos vs. Falsos Negativos) son distintos, como en diagnósticos médicos o detección de fraude.

---

## Desarrollo Técnico

### Estructura de la Matriz (Problema Binario)

| | Predicho Negativo (0) | Predicho Positivo (1) |
|---|---|---|
| **Observado Negativo (0)** | **TN** (True Negative) | **FP** (False Positive) |
| **Observado Positivo (1)** | **FN** (False Negative) | **TP** (True Positive) |

> [!ABSTRACT] Componentes
> - **True Negative (TN):** Predicción correcta de la clase negativa.
> - **False Positive (FP):** Error Tipo I. El modelo predijo positivo pero era negativo (Falsa alarma).
> - **False Negative (FN):** Error Tipo II. El modelo predijo negativo pero era positivo (Error grave).
> - **True Positive (TP):** Predicción correcta de la clase positiva.

### Métricas de Evaluación

#### 1. Accuracy (Exactitud)
Porcentaje total de predicciones correctas.
$$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$

> [!WARNING] Limitación
> El Accuracy puede ser engañoso en datasets desbalanceados (ej. 95% negativos, 5% positivos). Un modelo que siempre prediga "negativo" tendrá 95% de accuracy sin detectar ningún positivo.

#### 2. Precision (Precisión)
Responde: *De todos los casos que el modelo predijo como positivos, ¿cuántos realmente eran positivos?*
$$Precision = \frac{TP}{TP + FP}$$
*Importante cuando los Falsos Positivos son costosos (ej. filtros de spam).*

#### 3. Recall (Exhaustividad / Sensibilidad)
Responde: *De todos los positivos reales, ¿cuántos encontró el modelo?*
$$Recall = \frac{TP}{TP + FN}$$
*Importante cuando los Falsos Negativos son costosos (ej. detección de cáncer).*

#### 4. F1 Score
Media armónica entre Precision y Recall. Penaliza los desequilibrios entre ambas métricas.
$$F1 = \frac{2 \times (Precision \times Recall)}{Precision + Recall}$$

---

## Ejemplo Práctico

### Caso: Prueba de Cáncer

| | Predicho Sano | Predicho Enfermo |
|---|---|---|
| **Realmente Sano** | 90 (TN) | 5 (FP) |
| **Realmente Enfermo** | 8 (FN) | 82 (TP) |

**Cálculos:**
- **Accuracy:** $\frac{82+90}{185} = 0.930$ (93.0%)
- **Precision:** $\frac{82}{82+5} = 0.943$ (94.3%)
- **Recall:** $\frac{82}{82+8} = 0.911$ (91.1%)
- **F1 Score:** $\frac{2 \times (0.943 \times 0.911)}{0.943 + 0.911} = 0.927$ (92.7%)

---

## Implementación en Scikit-Learn

```python
from sklearn.metrics import confusion_matrix

# 1. Generar predicciones
y_pred = model.predict(X_test)

# 2. Construir la matriz
# y_test: valores reales, y_pred: valores predichos
m = confusion_matrix(y_test, y_pred)

# 3. Mostrar la matriz
print(m)
# Salida ejemplo:
# [[90  5]
#  [ 8 82]]
```

### Cálculo de métricas desde la matriz `m`
```python
# Precision: TP / (TP + FP)
precision = m[1,1] / m[:,1].sum()

# Recall: TP / (TP + FN)
recall = m[1,1] / m[1,:].sum()

# F1 Score
f1 = 2 * (precision * recall) / (precision + recall)
```

---

## Regla Práctica de Decisión

- **Priorizar Recall:** Cuando perder un positivo es muy costoso (Cáncer, Incendios).
- **Priorizar Precision:** Cuando una falsa alarma es muy costosa (Spam, Bloqueo de tarjetas).
- **Usar F1 Score:** Cuando se busca un equilibrio entre ambas métricas.

---

## Conexiones
- [[Basics of Machine Learning]]
- [[Regresión Logística]]
- [[CGI - Machine Learning]]