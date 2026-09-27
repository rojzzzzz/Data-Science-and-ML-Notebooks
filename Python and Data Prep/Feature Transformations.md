---
tags:
  - machine-learning
  - preprocessing
  - feature-engineering
  - scikit-learn
created: {{date}}
---

# Feature Transformations

> [!abstract]
> Una vez tenemos nuestras variables, el siguiente paso es transformarlas para que el modelo pueda aprender de ellas de la forma más eficiente posible. Las transformaciones dependen del tipo de variable: **numérica** o **categórica**. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}

---

# Transformaciones numéricas

Las variables numéricas suelen requerir transformaciones cuando:

- tienen escalas muy diferentes
- presentan distribuciones muy sesgadas
- algunos algoritmos son sensibles a la magnitud de los datos

Ejemplo:

| Edad | Salario |
|------:|---------:|
|22|25000|
|45|130000|
|31|45000|

Un algoritmo puede dar mucha más importancia al salario simplemente porque sus números son mayores.

---

# Standardization (Z-Score)

Consiste en transformar una variable para que tenga:

- media = 0
- desviación estándar = 1

La distribución **no cambia**, únicamente cambia la escala. :contentReference[oaicite:2]{index=2}

## Cuándo usarla

✅ Logistic Regression

✅ Linear Regression

✅ Ridge / Lasso

✅ SVM

✅ Redes neuronales

✅ KNN

---

## Ejemplo

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

También puede hacerse sobre columnas concretas.

```python
df[["salary"]] = scaler.fit_transform(df[["salary"]])
```

---

## Ejemplo completo

Antes

| salary |
|--------:|
|25000|
|50000|
|120000|

Después

| salary_std |
|------------:|
|-1.12|
|-0.18|
|1.30|

Ya no importa si la variable estaba medida en dólares, euros o miles de euros.

---

## ¿Por qué mejora algunos modelos?

Muchos algoritmos calculan distancias o utilizan descenso por gradiente.

Ejemplo:

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

model.fit(X_scaled, y)
```

Si una variable tiene valores entre

```
0 y 1
```

y otra entre

```
0 y 500000
```

la segunda dominará completamente el entrenamiento.

---

# Min-Max Scaling (Normalization)

Reescala todos los valores entre

```
0 y 1
```

A diferencia de Standardization, aquí sí conocemos el rango final.

:contentReference[oaicite:3]{index=3}

## Código

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

X_scaled = scaler.fit_transform(X)
```

---

## Cuándo usarlo

Especialmente útil cuando:

- Redes neuronales
- Algoritmos basados en distancia
- Variables con límites naturales

Ejemplo

```text
Temperatura

-30°C
50°C

↓

0
1
```

---

# StandardScaler vs MinMaxScaler

| StandardScaler | MinMaxScaler |
|----------------|--------------|
|Media = 0|Rango 0-1|
|Mantiene outliers|Los comprime|
|Muy usado en ML clásico|Muy usado en Deep Learning|

En la práctica, **StandardScaler suele ser la primera opción**.

---

# Log Transformation

Muchas variables reales tienen distribuciones muy sesgadas.

Ejemplos:

- ingresos
- población
- ventas
- número de seguidores
- precio de viviendas

Normalmente hay muchos valores pequeños y unos pocos extremadamente grandes.

La transformación logarítmica reduce esa asimetría. :contentReference[oaicite:4]{index=4}

---

## Antes

```text
10
15
20
18
25
1000
```

Después del log

```text
2.30
2.71
2.99
2.89
3.22
6.91
```

Los valores extremos dejan de dominar.

---

## Código

```python
import numpy as np

df["income_log"] = np.log(df["income"])
```

---

## ¿Qué ocurre si hay ceros?

`log(0)` no existe.

En ese caso usamos:

```python
df["income_log"] = np.log1p(df["income"])
```

`log1p(x)` significa

```
log(1+x)
```

y evita errores cuando existen valores iguales a cero. :contentReference[oaicite:5]{index=5}

---

## Ejemplo completo

```python
import numpy as np
import pandas as pd

df = pd.DataFrame({
    "income": [0, 1000, 5000, 10000]
})

df["income_log"] = np.log1p(df["income"])

print(df)
```

Resultado

| income | income_log |
|---------|-----------:|
|0|0.00|
|1000|6.91|
|5000|8.52|
|10000|9.21|

---

# ¿Necesitan Scaling los árboles?

No.

Esta es probablemente una de las preguntas más frecuentes en entrevistas.

Los árboles (Decision Tree, Random Forest, XGBoost...) **dividen los datos por comparaciones**, no por distancias.

Por ejemplo:

```
Edad > 30
```

sigue siendo exactamente igual después de hacer StandardScaler.

Por eso el rendimiento apenas cambia. :contentReference[oaicite:6]{index=6}

---

## Modelos que suelen necesitar Scaling

| Modelo | ¿Scaling? |
|----------|-----------|
|Linear Regression|✅|
|Logistic Regression|✅|
|SVM|✅|
|KNN|✅|
|Neural Networks|✅|
|Decision Tree|❌|
|Random Forest|❌|
|XGBoost|❌|
|LightGBM|❌|

---

# Transformaciones categóricas

Los algoritmos no entienden texto.

Esto:

```text
Rojo

Azul

Verde
```

debe convertirse en números. :contentReference[oaicite:7]{index=7}

---

# Label Encoding

Asigna un número entero a cada categoría.

```text
Rojo → 0

Azul → 1

Verde → 2
```

## Código

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["color"] = encoder.fit_transform(df["color"])
```

---

## ¿Cuándo usarlo?

✅ Variables ordinales

```
Bajo

Medio

Alto
```

o

✅ Árboles de decisión.

---

## Problema

En modelos lineales introduce un orden artificial.

```
Rojo = 0

Azul = 1

Verde = 2
```

El modelo puede interpretar

```
Verde > Azul > Rojo
```

aunque no tenga sentido.

---

# One-Hot Encoding

Crea una columna por categoría.

Antes

| Color |
|--------|
|Rojo|
|Azul|
|Rojo|

Después

| Rojo | Azul | Verde |
|------:|------:|------:|
|1|0|0|
|0|1|0|
|1|0|0|

---

## Código con pandas

```python
pd.get_dummies(df["color"])
```

---

## Código con Scikit-Learn

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder()

X = encoder.fit_transform(df[["color"]])
```

---

## ¿Cuándo usarlo?

✅ Logistic Regression

✅ Redes neuronales

✅ SVM

✅ Modelos lineales

---

## ¿Cuándo evitarlo?

Variables con miles de categorías.

Ejemplo

```
ProductID

123456

123457

123458

...
```

Generaría miles de columnas.

---

# Label Encoding vs One-Hot

| Label | One-Hot |
|---------|----------|
|Una columna|Muchas columnas|
|Muy compacto|Más memoria|
|Introduce orden|No introduce orden|
|Ideal para árboles|Ideal para modelos lineales|

---

# Cross Features

A veces la relación importante **no está en una variable**, sino en la combinación de dos.

Ejemplo

```
Habitaciones

Distancia al centro
```

Por separado aportan información.

Pero

```
Habitaciones × Distancia
```

puede explicar mucho mejor el precio de una vivienda. :contentReference[oaicite:8]{index=8}

---

## Crear una Cross Feature manualmente

```python
df["income_per_room"] = (
    df["income"] / df["rooms"]
)
```

Otro ejemplo

```python
df["bmi"] = (
    df["weight"] / (df["height"] ** 2)
)
```

---

## Polynomial Features

Scikit-Learn puede generar automáticamente interacciones.

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(
    degree=2,
    interaction_only=True,
    include_bias=False
)

X_poly = poly.fit_transform(X)
```

Si tenemos

```
A
B
C
```

obtendremos

```
A
B
C
A×B
A×C
B×C
```

---

# Pipeline completo

Un flujo típico de preprocesamiento.

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression())
])

pipeline.fit(X_train, y_train)

predictions = pipeline.predict(X_test)
```

Este enfoque evita errores y facilita desplegar el modelo.

---

# Resumen

> [!summary]
>
> - StandardScaler es la transformación más utilizada para variables numéricas.
> - MinMaxScaler reescala al rango [0,1].
> - Log Transform reduce distribuciones muy sesgadas.
> - Los árboles prácticamente no necesitan scaling.
> - Label Encoding es adecuado para variables ordinales o árboles.
> - One-Hot Encoding suele ser la mejor opción para modelos lineales.
> - Las Cross Features permiten capturar relaciones entre variables que un modelo simple no aprendería por sí solo.

## Enlaces

- [[Feature Engineering]]
- [[Domain Knowledge]]
- [[Ensemble Learning]]