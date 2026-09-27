---
tags:
  - python
  - pandas
  - patterns
  - machine-learning
---

# Pandas Patterns

> [!abstract]
> Esta nota reúne los patrones que aparecen constantemente en proyectos de Machine Learning. Más que memorizar funciones, intenta reconocer **qué problema tienes** y cuál es el patrón adecuado para resolverlo.

---

# Patrón 1 — Separar Features y Target

Este es probablemente el patrón que más usarás.

```python
X = df.drop(columns="target")
y = df["target"]
```

o

```python
X = df.drop(columns=["Survived"])
y = df["Survived"]
```

Nunca hagas

```python
X = df
```

porque estarías dejando la variable objetivo dentro de las features.



---

# Patrón 2 — Seleccionar columnas

Una columna

```python
df["age"]
```

Varias

```python
df[["age", "salary"]]
```

Eliminar columnas

```python
df.drop(columns=["id"])
```

Quedarte solo con algunas

```python
df[["age", "salary", "city"]]
```

---

# Patrón 3 — Filtrar filas

```python
adults = df[
    df["age"] >= 18
]
```

Varias condiciones

```python
df[
    (df["age"] >= 18)
    &
    (df["salary"] > 50000)
]
```

OR

```python
df[
    (df["city"] == "Madrid")
    |
    (df["city"] == "Barcelona")
]
```

---

# Patrón 4 — Crear Features

En ML esto ocurre constantemente.

```python
df["BMI"] = (
    df["weight"] /
    df["height"]**2
)
```

```python
df["income_per_room"] = (
    df["income"] /
    df["rooms"]
)
```

```python
df["is_adult"] = (
    df["age"] >= 18
).astype(int)
```

Piensa siempre:

> ¿Existe alguna variable derivada que sea más útil que las originales?

---

# Patrón 5 — Missing Values

Ver cuántos hay

```python
df.isna().sum()
```

Eliminar

```python
df.dropna()
```

Media

```python
df["age"] = df["age"].fillna(
    df["age"].mean()
)
```

Moda

```python
df["city"] = df["city"].fillna(
    df["city"].mode()[0]
)
```

---

# Patrón 6 — Convertir categorías

Label Encoding

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["gender"] = encoder.fit_transform(
    df["gender"]
)
```

One Hot

```python
pd.get_dummies(
    df,
    columns=["gender"]
)
```

---

# Patrón 7 — GroupBy

Pregunta típica

> ¿Cuál es el salario medio por ciudad?

```python
df.groupby("city")["salary"].mean()
```

Otra

> ¿Cuántas personas hay por ciudad?

```python
df["city"].value_counts()
```

Varias estadísticas

```python
df.groupby("city").agg({

    "salary":"mean",

    "age":"median"

})
```

---

# Patrón 8 — Ordenar

Mayor salario

```python
df.sort_values(
    "salary",
    ascending=False
)
```

---

# Patrón 9 — Merge

Muy parecido a SQL.

Clientes

```text
id
nombre
```

Pedidos

```text
id
precio
```

↓

```python
pd.merge(

    customers,

    orders,

    on="id"

)
```

---

# Patrón 10 — Seleccionar por posición

Primeras cinco filas

```python
df.iloc[:5]
```

Primera fila

```python
df.iloc[0]
```

Primera fila, tercera columna

```python
df.iloc[0, 2]
```

---

# Patrón 11 — Seleccionar por nombre

```python
df.loc[:, ["age", "salary"]]
```

Filas

```python
df.loc[10:20]
```

---

# Patrón 12 — Operaciones vectorizadas ⭐

MAL

```python
for i in range(len(df)):

    df.loc[i, "salary"] *= 1.1
```

BIEN

```python
df["salary"] *= 1.1
```

Siempre intenta pensar en columnas, no en filas.

---

# Patrón 13 — Apply

Cuando una operación no existe de forma vectorizada.

```python
df["name"] = df["name"].apply(
    str.upper
)
```

Con lambda

```python
df["square"] = (
    df["age"]
    .apply(lambda x: x**2)
)
```

---

# Patrón 14 — Apply sobre filas

Solo cuando necesitas varias columnas.

```python
def bmi(row):

    return (

        row["weight"] /

        row["height"]**2

    )

df["BMI"] = df.apply(
    bmi,
    axis=1
)
```

Si puedes evitar `axis=1`, hazlo.

Es bastante lento.

---

# Patrón 15 — Encadenar operaciones

En lugar de

```python
df = df.dropna()

df = df.sort_values("salary")

df = df.head(10)
```

Haz

```python
result = (

    df

    .dropna()

    .sort_values("salary")

    .head(10)

)
```

Mucho más legible.

---

# Patrón 16 — Preparar datos para Scikit-Learn

```python
X = df.drop(columns="target")

y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(

    X,

    y,

    test_size=0.2,

    random_state=42

)
```

Este patrón aparece en prácticamente todos los proyectos.

---

# Patrón 17 — Exploración rápida

Cuando cargas un dataset nuevo.

Siempre hago esto:

```python
df.head()

df.info()

df.describe()

df.shape

df.isna().sum()

df.dtypes

df.nunique()
```

Con esas siete líneas ya tienes una idea bastante buena del dataset.

---

# Mi árbol mental

Cuando dudes, pregúntate:

```
¿Necesito...

↓

Seleccionar?

→ loc
→ iloc

↓

Filtrar?

→ []

↓

Crear columnas?

→ =

↓

Agrupar?

→ groupby()

↓

Unir tablas?

→ merge()

↓

Transformar?

→ apply()
→ map()

↓

Eliminar?

→ drop()

↓

Ordenar?

→ sort_values()
```

No pienses en funciones.

Piensa en el problema.

La función sale sola.

---

# Las 20 funciones que realmente uso

```python
head()

info()

describe()

shape

columns

dtypes

drop()

rename()

fillna()

dropna()

iloc

loc

groupby()

agg()

merge()

concat()

sort_values()

value_counts()

get_dummies()

apply()
```

Si dominas estas veinte funciones, probablemente podrás resolver alrededor del **90% de las tareas de manipulación de datos** que aparecen en cursos, prácticas y proyectos de Machine Learning.
