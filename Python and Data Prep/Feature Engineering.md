---
tags:
  - machine-learning
  - feature-engineering
  - preprocessing
  - scikit-learn
created: {{date}}
---

# Feature Engineering

> [!abstract]
> **Feature Engineering** consiste en transformar, seleccionar o crear variables (*features*) para que un modelo de Machine Learning pueda aprender patrones de forma más eficiente. En muchos problemas, mejorar las features produce un mayor aumento de rendimiento que cambiar de algoritmo. :contentReference[oaicite:0]{index=0}

---

# ¿Por qué importa?

Los algoritmos solo aprenden a partir de las variables que reciben. Si las features no representan correctamente el problema, incluso un modelo muy potente tendrá un rendimiento pobre.

El objetivo del Feature Engineering es hacer que la información relevante sea **más fácil de aprender** para el modelo.

```text
Datos crudos
      │
      ▼
Feature Engineering
      │
      ▼
Datos más representativos
      │
      ▼
Modelo más preciso
```

En la práctica, una buena ingeniería de variables puede aportar mejoras mayores que cambiar entre algoritmos como Random Forest, XGBoost o Redes Neuronales.

---

# ¿Qué hace exactamente?

Existen tres grandes tareas:

| Tipo | Objetivo | Ejemplo |
|-------|----------|----------|
| Transformar | Cambiar la representación de una variable | Standardization, Log Transform |
| Crear | Generar nuevas variables | Edad², Precio/m² |
| Seleccionar | Eliminar variables irrelevantes | Feature Selection |

---

# Underfitting vs Overfitting

El Feature Engineering puede ayudar tanto cuando un modelo aprende poco como cuando aprende demasiado.

## Underfitting

El modelo es demasiado simple y no captura los patrones importantes.

Síntomas:

- baja precisión en entrenamiento
- baja precisión en test

Posibles soluciones:

- crear nuevas variables
- añadir más información
- combinar variables
- utilizar conocimiento del dominio

Ejemplo:

Tenemos únicamente:

```text
Edad
```

Queremos predecir salario.

Probablemente no sea suficiente.

Podemos añadir:

```text
Años de experiencia
Nivel educativo
Sector laboral
```

o incluso crear

```text
Salario esperado por experiencia
```

---

## Overfitting

El modelo memoriza el entrenamiento.

Síntomas:

- entrenamiento muy bueno
- test muy malo

Muchas veces ocurre porque existen demasiadas variables respecto a la cantidad de datos.

Soluciones:

- eliminar variables irrelevantes
- reducir dimensionalidad
- regularización
- conseguir más datos

---

# Feature Selection vs Feature Extraction

Son dos estrategias distintas para reducir dimensionalidad.

## Feature Selection

Mantiene únicamente variables existentes.

```text
Antes

Edad
Peso
Altura
Color favorito
Ingresos

↓

Después

Edad
Peso
Ingresos
```

No crea información nueva.

Ventajas

- muy interpretable
- sencillo de explicar

Ejemplo con Scikit-Learn

```python
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import f_regression

selector = SelectKBest(score_func=f_regression, k=5)

X_selected = selector.fit_transform(X, y)
```

---

## Feature Extraction

Crea nuevas variables a partir de las originales.

Ejemplo clásico:

```
Edad
Peso
Altura

↓

Componente Principal 1
Componente Principal 2
```

La técnica más conocida es **PCA (Principal Component Analysis)**.

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)

X_pca = pca.fit_transform(X)
```

Ventajas

- elimina redundancia
- reduce ruido

Desventajas

- pierde interpretabilidad

---

# Tipos de variables

Antes de transformar datos debemos saber qué tipo de variable tenemos.

## Variables numéricas

Representan cantidades.

Ejemplos

```text
Edad
Altura
Peso
Ingresos
Temperatura
```

Sobre ellas suelen aplicarse:

- Scaling
- Normalization
- Log Transformation

---

## Variables categóricas

Representan categorías.

Ejemplos

```text
Sexo

Ciudad

Color

País
```

Sobre ellas normalmente aplicamos:

- Label Encoding
- One-Hot Encoding

---

# ¿Cuándo hacer Feature Engineering?

Normalmente se realiza después de limpiar los datos.

```text
Dataset

↓

Limpieza

↓

Missing Values

↓

Feature Engineering

↓

Train/Test Split

↓

Entrenamiento
```

En Scikit-Learn normalmente se implementa mediante un **Pipeline**.

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression())
])

pipeline.fit(X_train, y_train)
```

Esto garantiza que todas las transformaciones se apliquen correctamente tanto en entrenamiento como en inferencia.

---

# Ejemplo completo

Supongamos un dataset de viviendas.

```python
import pandas as pd

houses = pd.DataFrame({
    "income": [50000, 80000, 120000],
    "rooms": [3, 5, 6],
    "surface": [80, 140, 180]
})
```

Datos originales

| income | rooms | surface |
|---------|------:|--------:|
|50000|3|80|
|80000|5|140|
|120000|6|180|

Podemos crear nuevas features.

```python
houses["income_per_room"] = (
    houses["income"] / houses["rooms"]
)

houses["rooms_per_m2"] = (
    houses["rooms"] / houses["surface"]
)
```

Resultado

| income | rooms | surface | income_per_room | rooms_per_m2 |
|---------|------:|--------:|----------------:|-------------:|
|50000|3|80|16666|0.037|
|80000|5|140|16000|0.036|
|120000|6|180|20000|0.033|

Estas nuevas variables pueden ser mucho más informativas que las originales.

---

# Buenas prácticas

> [!tip]
>
> - Entiende primero el problema de negocio.
> - Empieza con transformaciones simples.
> - No crees cientos de variables "por probar".
> - Evalúa siempre mediante validación.
> - Usa `Pipeline` para evitar data leakage.
> - Prefiere features interpretables cuando sea posible.

---

# Errores comunes

❌ Escalar variables categóricas.

❌ Aplicar One-Hot a miles de categorías.

❌ Crear demasiadas features y provocar overfitting.

❌ Ajustar transformaciones usando también el conjunto de test.

❌ Pensar que cambiar de modelo solucionará un problema de malas features.

---

# Resumen

> [!summary]
>
> - El Feature Engineering suele ser la parte con mayor impacto en Machine Learning.
> - Puede transformar, crear o seleccionar variables.
> - Ayuda tanto al underfitting como al overfitting.
> - Existen dos grandes tipos de variables: numéricas y categóricas.
> - Las siguientes notas profundizan en las transformaciones específicas para cada tipo de variable.

## Enlaces

- [[Feature Transformations]]
- [[Domain Knowledge]]
- [[Ensemble Learning]]