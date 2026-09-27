---
date: 2026-06-11
area: tech
tags: [machine-learning, estadística]
status: evergreen
aliases: [Curva ROC, AUC, Area Under the Curve, Receiver Operating Characteristic]
related: [[Confusion Matrix]], [[Regresión Logística]]
source: ""
---

# ROC Curve y AUC (Area Under the Curve)

## Definición
La **ROC Curve** (Receiver Operating Characteristic) es una representación gráfica que ilustra la capacidad de diagnóstico de un sistema de clasificación binaria a medida que se varía su umbral de discriminación (*threshold*). El **AUC** (Area Under the Curve) es el valor numérico que resume el desempeño mostrado por la curva ROC.

## Por qué importa
A diferencia de métricas como Accuracy o F1 Score que dependen de un umbral fijo (usualmente 0.5), la curva ROC permite evaluar el desempeño del modelo considerando **todos los posibles thresholds**. Esto es vital para comparar modelos de forma global y para manejar datasets desbalanceados.

---

## Desarrollo Técnico

### Ejes de la ROC Curve
La gráfica muestra la relación entre:

1. **Eje Y: True Positive Rate (TPR)**
   También conocido como **Recall** o **Sensibilidad**. Mide qué porcentaje de positivos reales detectó el modelo.
   $$TPR = \frac{TP}{TP + FN}$$

2. **Eje X: False Positive Rate (FPR)**
   Mide qué porcentaje de negativos reales fueron clasificados incorrectamente como positivos.
   $$FPR = \frac{FP}{FP + TN}$$

> [!TIP] Interpretación Intuitiva
> - **TPR alto:** El modelo detecta casi todos los casos positivos.
> - **FPR alto:** El modelo genera muchas falsas alarmas.
> La ROC Curve estudia el equilibrio entre ambas situaciones al variar el umbral.

---

### AUC (Area Under the Curve)
Es el área total bajo la curva ROC. Proporciona una medida agregada del desempeño en todos los umbrales posibles.

| AUC | Interpretación |
|:---:|---|
| **1.0** | **Perfecto:** Clasificación perfecta sin errores. |
| **0.9 - 0.99** | **Excelente:** Muy alta capacidad de discriminación. |
| **0.7 - 0.8** | **Aceptable:** Desempeño razonable. |
| **0.5** | **Aleatorio:** Equivalente a lanzar una moneda. |

> [!ABSTRACT] Interpretación Probabilística
> El AUC representa la probabilidad de que, ante un par aleatorio de instancias (una positiva y una negativa), el modelo asigne una puntuación (probabilidad) más alta a la instancia positiva.

---

## Ejemplo e Implementación

### Obtención de Probabilidades
Para construir la curva, necesitamos las probabilidades continuas, no las etiquetas finales:

```python
# Obtiene probabilidades para cada clase
probs = model.predict_proba(X_test)

# Generalmente nos interesa la probabilidad de la clase positiva (clase 1)
y_score = probs[:, 1]
```

### Construcción Conceptual (Manual)
Si variamos el umbral ($\tau$) de $0$ a $1$:
1. Para cada $\tau$, convertimos probabilidades en etiquetas: `y_pred = (y_score > threshold)`.
2. Calculamos la matriz de confusión.
3. Obtenemos el par $(FPR, TPR)$.
4. Graficamos todos los pares.

> [!IMPORTANT] ROC Ideal vs Aleatoria
> - **Ideal:** La curva sube verticalmente hasta $(0,1)$ y luego avanza horizontalmente. El AUC es $1$.
> - **Aleatoria:** La curva es una diagonal de $(0,0)$ a $(1,1)$. El AUC es $0.5$.

---

## Ventajas sobre el Accuracy
En datos desbalanceados (ej. 95% No Fraude, 5% Fraude), un modelo que siempre prediga "No Fraude" tendrá un Accuracy del 95% pero un AUC de 0.5 (aleatorio), revelando que el modelo no tiene capacidad real de discriminación.

---

## Conexiones
- [[Confusion Matrix]] — Base para el cálculo de TPR y FPR.
- [[Regresión Logística]] — Algoritmo que genera las probabilidades usadas por ROC.
- [[Basics of Machine Learning]] — Evaluación de modelos.
- [[CGI - Machine Learning]] — MOC de Machine Learning.
