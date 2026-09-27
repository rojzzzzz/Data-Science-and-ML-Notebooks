---
tags:
  - machine-learning
  - ensemble-learning
  - random-forest
  - xgboost
created: {{date}}
---

# Ensemble Learning

> [!abstract]
> **Ensemble Learning** consiste en combinar múltiples modelos para obtener uno más preciso, robusto y con mejor capacidad de generalización que cualquiera de los modelos individuales. La idea es simple: varios modelos "aceptables" suelen producir mejores resultados que un único modelo "muy bueno". :contentReference[oaicite:0]{index=0}

---

# La intuición

Supongamos que preguntas la respuesta de un examen a una sola persona.

```text
Persona A
↓

80% de aciertos
```

Ahora preguntas a diez personas independientes.

```text
Persona 1

Persona 2

...

Persona 10

↓

Votación

↓

Respuesta final
```

Es mucho más difícil que todas se equivoquen en la misma pregunta.

Eso es exactamente lo que hace un Ensemble.

---

# ¿Por qué funcionan?

Cada modelo comete errores distintos.

Cuando combinamos sus predicciones:

- algunos errores se cancelan
- disminuye la varianza
- aumenta la capacidad de generalización

En clasificación normalmente se utiliza:

```
Majority Voting
```

En regresión

```
Average Prediction
```

---

# Tipos de Ensemble

Existen dos familias principales.

```
Ensemble Learning

├── Bagging
│      ├── Random Forest
│      └── BaggingClassifier
│
└── Boosting
       ├── AdaBoost
       ├── Gradient Boosting
       ├── XGBoost
       ├── LightGBM
       └── CatBoost
```

---

# Bagging

**Bagging = Bootstrap Aggregating**

La idea es entrenar muchos modelos **de forma independiente** usando diferentes subconjuntos del dataset. :contentReference[oaicite:1]{index=1}

Proceso

```text
Dataset original

↓

Bootstrap Sample 1

↓

Modelo 1


Dataset original

↓

Bootstrap Sample 2

↓

Modelo 2


Dataset original

↓

Bootstrap Sample 3

↓

Modelo 3


↓

Promedio / Votación
```

Cada modelo ve datos ligeramente diferentes.

---

## Bootstrap Sampling

Bootstrap significa:

> Muestrear **con reemplazo**.

Ejemplo

Dataset

```
A
B
C
D
```

Una muestra bootstrap podría ser

```
A

B

B

D
```

o

```
C

C

A

D
```

Es perfectamente válido repetir observaciones.

---

## Bagging en Scikit-Learn

```python
from sklearn.ensemble import BaggingClassifier
from sklearn.tree import DecisionTreeClassifier

bag = BaggingClassifier(

    estimator=DecisionTreeClassifier(),

    n_estimators=100,

    random_state=42

)

bag.fit(X_train, y_train)
```

---

## Parámetros importantes

```python
BaggingClassifier(

    estimator=DecisionTreeClassifier(),

    n_estimators=100,

    max_samples=0.8,

    max_features=0.8

)
```

| Parámetro | Significado |
|-----------|-------------|
|n_estimators|Número de modelos|
|max_samples|Porcentaje de muestras utilizadas|
|max_features|Porcentaje de variables utilizadas|

---

## ¿Cuándo ayuda?

Especialmente cuando el modelo base tiene **alta varianza**.

Ejemplos

- Decision Trees
- KNN

Ayuda mucho menos en modelos muy estables.

Por ejemplo:

- Logistic Regression

La lecture muestra precisamente este comportamiento: los árboles obtienen una mejora notable con bagging, mientras que modelos lineales apenas cambian. :contentReference[oaicite:2]{index=2}

---

# Random Forest

Random Forest es simplemente:

> Bagging + Decision Trees + selección aleatoria de variables.

Cada árbol:

- recibe un bootstrap distinto
- además considera únicamente un subconjunto aleatorio de features en cada división. :contentReference[oaicite:3]{index=3}

---

## ¿Por qué añadir aleatoriedad?

Si todos los árboles fueran iguales

```
Tree 1

↓

MedInc

↓

...

Tree 2

↓

MedInc

↓

...

Tree 3

↓

MedInc
```

todos cometerían prácticamente los mismos errores.

Random Forest obliga a que cada árbol aprenda cosas diferentes.

---

## Código

```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(

    n_estimators=300,

    random_state=42

)

rf.fit(X_train, y_train)
```

---

## Feature Importance

Una ventaja enorme.

```python
importance = rf.feature_importances_

print(importance)
```

También

```python
import pandas as pd

pd.Series(

    rf.feature_importances_,

    index=X.columns

).sort_values(ascending=False)
```

Permite saber qué variables fueron más útiles durante el entrenamiento.

---

# Boosting

Boosting sigue la filosofía opuesta al Bagging.

Los modelos **no son independientes**.

Cada nuevo modelo intenta corregir los errores del anterior. :contentReference[oaicite:4]{index=4}

---

## Idea

```text
Modelo 1

↓

Errores

↓

Modelo 2 aprende esos errores

↓

Errores restantes

↓

Modelo 3 aprende esos errores

↓

...

↓

Modelo final
```

Cada modelo depende del anterior.

---

# AdaBoost

Uno de los primeros algoritmos de Boosting.

Funcionamiento simplificado

```text
Entrenar árbol pequeño

↓

Detectar ejemplos mal clasificados

↓

Darles mayor peso

↓

Entrenar otro árbol

↓

Repetir
```

---

## Código

```python
from sklearn.ensemble import AdaBoostClassifier

ada = AdaBoostClassifier(

    n_estimators=100,

    random_state=42

)

ada.fit(X_train, y_train)
```

---

# Gradient Boosting

En lugar de cambiar pesos, cada árbol aprende el **error residual** del modelo anterior.

```
Predicción

↓

Error

↓

Nuevo árbol

↓

Predicción mejorada

↓

Nuevo error

↓

Nuevo árbol
```

Es mucho más potente que AdaBoost.

---

## Código

```python
from sklearn.ensemble import GradientBoostingRegressor

model = GradientBoostingRegressor(

    n_estimators=200,

    learning_rate=0.05,

    random_state=42

)

model.fit(X_train, y_train)
```

---

# XGBoost

XGBoost (**Extreme Gradient Boosting**) es probablemente el algoritmo clásico más importante en Machine Learning tabular.

Añade mejoras sobre Gradient Boosting:

- regularización
- paralelización
- manejo eficiente de memoria
- poda de árboles
- tratamiento de valores faltantes

---

## Instalación

```bash
pip install xgboost
```

---

## Ejemplo

```python
from xgboost import XGBClassifier

model = XGBClassifier(

    n_estimators=300,

    max_depth=6,

    learning_rate=0.05,

    random_state=42

)

model.fit(X_train, y_train)
```

---

# LightGBM

Creado por Microsoft.

Ventajas

- extremadamente rápido
- muy eficiente con datasets grandes
- muy usado en Kaggle

```python
from lightgbm import LGBMClassifier

model = LGBMClassifier()

model.fit(X_train, y_train)
```

---

# CatBoost

Creado por Yandex.

Especialmente bueno cuando existen muchas variables categóricas.

```python
from catboost import CatBoostClassifier

model = CatBoostClassifier(

    verbose=False

)

model.fit(X_train, y_train)
```

---

# Bagging vs Boosting

| Bagging | Boosting |
|----------|----------|
|Modelos independientes|Modelos secuenciales|
|Reduce varianza|Reduce sesgo|
|Más fácil de paralelizar|Más lento|
|Muy robusto|Mayor precisión|
|Difícil sobreajustar|Puede sobreajustar|

---

# ¿Cuál elegir?

| Situación | Algoritmo recomendado |
|------------|----------------------|
|Primer modelo robusto|Random Forest|
|Máxima precisión en datos tabulares|XGBoost|
|Dataset enorme|LightGBM|
|Muchas variables categóricas|CatBoost|
|Modelo muy inestable|Bagging|

---

# Bias vs Variance

Todo modelo tiene dos fuentes principales de error.

## Alto Bias

Modelo demasiado simple.

```text
Underfitting
```

Ejemplos

- Linear Regression en problemas muy complejos

---

## Alta Variance

Modelo demasiado complejo.

```text
Overfitting
```

Ejemplos

- Árboles muy profundos

---

## Relación con los ensembles

```text
Bagging

↓

Reduce Variance

↓

Menos Overfitting
```

```text
Boosting

↓

Reduce Bias

↓

Modelo más potente
```

Aunque en la práctica ambos afectan parcialmente a ambos tipos de error, esta es la intuición más útil para recordar.

---

# Pipeline completo

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier

pipeline = Pipeline([

    ("scaler", StandardScaler()),

    ("model", RandomForestClassifier(
        n_estimators=300,
        random_state=42
    ))

])

pipeline.fit(X_train, y_train)

predictions = pipeline.predict(X_test)
```

> [!note]
> Aunque el `StandardScaler` apenas influye en Random Forest, incluirlo dentro de un `Pipeline` es una buena práctica cuando se experimenta con distintos modelos.

---

# Buenas prácticas

> [!tip]
>
> - Empieza con un modelo sencillo como baseline.
> - Usa Random Forest como primer ensemble.
> - Si buscas la máxima precisión en datos tabulares, prueba XGBoost o LightGBM.
> - Ajusta `n_estimators` antes que hiperparámetros más complejos.
> - Evalúa siempre con validación cruzada.
> - No asumas que un ensemble resolverá problemas de datos mal preparados; unas buenas features siguen siendo fundamentales.

---

# Errores comunes

❌ Usar cientos o miles de árboles sin comprobar si el rendimiento mejora.

❌ No fijar `random_state`, dificultando la reproducibilidad.

❌ Intentar compensar malas features con modelos cada vez más complejos.

❌ Pensar que XGBoost siempre será la mejor opción.

❌ Ignorar el coste computacional del entrenamiento.

---

# Resumen

> [!summary]
>
> - Ensemble Learning combina múltiples modelos para obtener mejores predicciones.
> - Bagging entrena modelos independientes sobre muestras bootstrap y reduce principalmente la varianza.
> - Random Forest es un ejemplo de Bagging aplicado a árboles de decisión con selección aleatoria de variables.
> - Boosting entrena modelos secuencialmente, donde cada uno corrige los errores del anterior.
> - Gradient Boosting, XGBoost, LightGBM y CatBoost son algoritmos de Boosting muy utilizados en problemas tabulares.
> - En la práctica, un buen Feature Engineering y un buen Ensemble suelen ser la combinación que ofrece los mejores resultados.

## Enlaces

- [[Feature Engineering]]
- [[Feature Transformations]]
- [[Domain Knowledge]]