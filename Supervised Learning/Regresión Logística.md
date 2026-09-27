---
date: 2026-06-08
area: tech
tags: [machine-learning, clasificación]
status: budding
aliases: [Logistic Regression]
related: [Basics of Machine Learning, Regresión Lineal]
source: ""
---

## Definición

La **Regresión Logística** es un algoritmo de **clasificación supervisada** utilizado para estimar la probabilidad de que una observación pertenezca a una determinada clase. Aunque contiene la palabra *regresión*, su objetivo principal es realizar **clasificación**.

### Intuición General
La regresión logística realiza dos pasos principales:
1. Calcula una combinación lineal de las variables de entrada.
2. Convierte ese resultado en una probabilidad mediante la **función sigmoide**.

```text
Variables → Combinación Lineal → Sigmoide → Probabilidad → Clase
```

### Modelo Matemático
Primero calcula la combinación lineal $z$:
$$ z = b + w_1x_1 + w_2x_2 + \cdots + w_nx_n $$

Donde:
- $b$ = intercepto
- $w_i$ = coeficientes
- $x_i$ = variables explicativas

Posteriormente aplica la **función sigmoide** para obtener la probabilidad $p$:
$$ p = \frac{1}{1 + e^{-z}} $$

**Propiedades:**
- $p \in [0,1]$
- Puede interpretarse directamente como una probabilidad.

| $z$ | $p$ |
|---|---|
|-10|≈ 0|
|0|0.5|
|10|≈ 1|

---

## Por qué importa

Es uno de los algoritmos fundamentales en Machine Learning para problemas de clasificación binaria (y multiclase vía One-vs-Rest). Permite no solo predecir una categoría, sino también entender la **confianza** de la predicción a través de la probabilidad estimada. 

Además, sus coeficientes son interpretables mediante los **Odds Ratios**, lo que permite entender el impacto de cada variable en la probabilidad del evento.

---

## Desarrollo Técnico

### ¿Qué Aprende el Modelo?
La regresión logística busca encontrar los coeficientes ($w_1, w_2, \dots, w_n, b$) que minimizan el error de clasificación. A diferencia de la regresión lineal, utiliza un proceso iterativo de optimización (como Gradient Descent o L-BFGS).

### Función de Pérdida (Cross-Entropy)
La función objetivo a minimizar es la entropía cruzada:
$$ -\sum_{i=1}^{n} \left[ y_i \log(f(x_i)) + (1-y_i)\log(1-f(x_i)) \right] $$

**Intuición de la pérdida:**
Penaliza levemente los aciertos con alta confianza y severamente los errores cometidos con alta confianza.

| Real | Predicción | Pérdida |
|---|---|---|
|1|0.99|Muy baja|
|1|0.50|Moderada|
|1|0.01|Muy alta|

### Coeficientes, Odds y Log-Odds
La regresión logística modela el logaritmo de los odds (**Log-Odds**):
$$ \log\left(\frac{p}{1-p}\right) = b + w_1x_1 + \cdots + w_nx_n $$

El término $\frac{p}{1-p}$ se denomina **Odds**:
- **Odds = 1**: Misma probabilidad de éxito y fracaso ($p=0.5$).
- **Odds > 1**: Clase positiva más probable.
- **Odds < 1**: Clase negativa más probable.

### Odds Ratio
Para interpretar los coeficientes se usa la exponencial: `np.exp(model.coef_)`.
- **OR > 1**: Aumenta la probabilidad de la clase positiva.
- **OR < 1**: Disminuye la probabilidad de la clase positiva.
- **OR = 1**: Sin efecto.

---

## Ejemplo y Aplicación

### Regla de Decisión
Normalmente se usa un umbral de 0.5:
- $p \geq 0.5 \rightarrow$ **Clase 1**
- $p < 0.5 \rightarrow$ **Clase 0**

**Ejemplos de uso:**
- Spam / No Spam
- Compra / No Compra
- Enfermo / Sano

### Estandarización y Flujo de Trabajo
Es crítico usar `StandardScaler()` cuando las variables tienen escalas muy diferentes para ayudar a la convergencia del optimizador.

```python
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

# Flujo correcto
sc = StandardScaler()
sc.fit(X_train) # Solo en entrenamiento

X_train_std = sc.transform(X_train)
X_test_std = sc.transform(X_test)

model = LogisticRegression(max_iter=1000)
model.fit(X_train_std, y_train)
```

**Métrica Típica (Accuracy):**
$$ Accuracy = \frac{Predicciones\ Correctas}{Total\ de\ Observaciones} $$

---

## Comparativa: Lineal vs Logística

| Característica | Regresión Lineal | Regresión Logística |
|---|---|---|
| **Tipo de problema** | Regresión | Clasificación |
| **Salida** | Número continuo | Probabilidad [0,1] |
| **Rango de salida** | $(-\infty, +\infty)$ | $[0, 1]$ |
| **Función de pérdida** | SSE / MSE | Cross-Entropy |
| **Métrica típica** | $R^2$ | Accuracy |

---

## Conexiones
- [[Basics of Machine Learning]] — Fundamentos de aprendizaje supervisado.
- [[Regresión Lineal]] — Modelo base para la combinación lineal.
- [[CGI - Machine Learning]] — MOC de Machine Learning.
- [[Plots]] — Para visualización de fronteras de decisión.

## Preguntas abiertas
- ¿Cómo afecta el desbalance de clases a la función de pérdida Cross-Entropy?
- ¿En qué casos es preferible usar un umbral distinto a 0.5?
