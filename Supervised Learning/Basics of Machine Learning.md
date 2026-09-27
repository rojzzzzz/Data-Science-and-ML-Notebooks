---
date: 2026-06-01
area: tech
tags: [machine-learning, fundamentos]
status: budding
aliases: [Fundamentos de ML, Introducción al Aprendizaje Automático]
related: [CGI - Machine Learning, Regresión Lineal, Regresión Logística]
source: ""
---

## Definición
El **Machine Learning** (Aprendizaje Automático) es una rama de la inteligencia artificial que busca descubrir patrones en los datos para realizar predicciones o decisiones sin programación explícita.

> [!ABSTRACT] Análisis Exploratorio (EDA)
> En la práctica, antes de aplicar cualquier modelo, es fundamental realizar un análisis descriptivo y visual (como *scatter plots*) para captar las tendencias y el panorama general de los datos.

## Conceptos Clave

### La Función del Modelo
Matemáticamente, representamos un modelo como:
$$y = f(x)$$

> [!INFO] Terminología
> - **Variable Objetivo ($y$):** También conocida como *target variable*, *ground truth*, o variable dependiente. Es lo que queremos predecir.
> - **Variables Explicativas ($x$):** Conocidas como *features*, características o variables independientes. Son los datos que usamos para explicar a $y$.

### Tipos de Aprendizaje Supervisado
Dependiendo de la naturaleza de la variable objetivo ($y$):
- **Regresión:** Cuando $y$ toma valores numéricos continuos (ej. precio de una casa). Ver [[Regresión Lineal]].
- **Clasificación:** Cuando $y$ es categórica (ej. "Compra" o "No Compra"). Ver [[Regresión Logística]].

![[assets/Pasted image 20260602203341.png]]

## Técnicas de Machine Learning

### 1. Aprendizaje Supervisado
Método para construir modelos que predicen la salida correcta basándose en datos de entrenamiento etiquetados.
- **Enfoque:** "Data mining orientado a objetivos".

### 2. Aprendizaje No Supervisado
Se centra en los datos de entrada en sí mismos para descubrir patrones o estructuras ocultas sin una variable objetivo.
- **Enfoque:** "Data mining exploratorio".

### 3. Aprendizaje por Refuerzo
Un agente aprende a tomar acciones para maximizar una recompensa en un entorno dinámico.

---

## Flujo de Trabajo (ML Workflow)

### 1. Preparación de Datos
Para que un modelo pueda procesar los datos, estos deben ser numéricos (`int`, `float`, `bool`). Los textos u objetos deben ser convertidos.
- **Importante:** Siempre elimina la variable objetivo ($y$) de la matriz de características ($X$) para evitar el "cheating".

### 2. División Entrenamiento/Prueba (Train/Test Split)
Evaluamos la capacidad de generalización dividiendo los datos:
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.5, random_state=0)
```
- `test_size=0.5`: 50% para entrenar, 50% para evaluar.
- `random_state`: Asegura la **reproducibilidad**. El mismo valor generará la misma división siempre.

### 3. Estandarización (Scaling)
Crucial cuando las variables tienen escalas muy diferentes (ej. edad vs. ingresos).
$$z = \frac{x - \mu}{\sigma}$$
- **Regla de oro:** Hacer `fit()` solo en los datos de entrenamiento para evitar el **Data Leakage**. Luego, `transform()` en ambos.

```python
scaler = StandardScaler()  
  
scaler.fit(X_train)  
  
X_train = scaler.transform(X_train)  
X_test = scaler.transform(X_test)
```

### 4. Entrenamiento y Evaluación
1. **Fit:** `model.fit(X_train, y_train)`
2. **Score:** `model.score(X_test, y_test)`
   - Si el rendimiento en *Train* es similar al de *Test*, el modelo generaliza bien.

## Conexiones
- [[CGI - Machine Learning]] — Mapa de contenido sobre ML.
- [[Regresión Lineal]] — Detalles sobre modelos de regresión.
- [[Regresión Logística]] — Detalles sobre modelos de clasificación.
- [[Plots]] — Visualización para el análisis exploratorio.
