---
tags:
  - python
  - pandas
  - cheatsheet
  - data-science
created: {{date}}
---

# Pandas

> [!abstract]
> **Pandas** es la librería estándar para manipulación y análisis de datos en Python. Su objetivo es trabajar con datos tabulares de forma rápida, expresiva y eficiente.

```python
import pandas as pd
```

---

# Estructuras principales

## Series

Equivalente a una columna.

```python
ages = pd.Series([20, 21, 22])

print(ages)
```

```
0    20
1    21
2    22
```

---

## DataFrame

Equivalente a una tabla.

```python
df = pd.DataFrame({
    "name": ["Ana", "Luis", "Juan"],
    "age": [20, 22, 25]
})

df
```

| name | age |
|------|----:|
|Ana|20|
|Luis|22|
|Juan|25|

---

# Leer datos

CSV

```python
df = pd.read_csv("students.csv")
```

Excel

```python
df = pd.read_excel("students.xlsx")
```

JSON

```python
df = pd.read_json("students.json")
```

Guardar

```python
df.to_csv("output.csv", index=False)
```

---

# Inspeccionar datos

Primeras filas

```python
df.head()
```

Últimas

```python
df.tail()
```

Dimensiones

```python
df.shape
```

```
(1000, 15)
```

Información general

```python
df.info()
```

Estadísticas

```python
df.describe()
```

Nombres de columnas

```python
df.columns
```

Tipo de datos

```python
df.dtypes
```

---

# Seleccionar columnas

Una columna

```python
df["age"]
```

Varias

```python
df[["age", "salary"]]
```

---

# Seleccionar filas

Por posición

```python
df.iloc[0]
```

Varias

```python
df.iloc[:5]
```

Fila y columna

```python
df.iloc[0, 2]
```

---

Por etiqueta

```python
df.loc[5]
```

Filas y columnas

```python
df.loc[0:10, ["age", "salary"]]
```

---

# Filtrar datos

Mayores de edad

```python
df[df["age"] >= 18]
```

Dos condiciones

```python
df[
    (df["age"] >= 18) &
    (df["salary"] > 30000)
]
```

Usa siempre paréntesis.

---

Con OR

```python
df[
    (df["city"] == "Madrid") |
    (df["city"] == "Barcelona")
]
```

---

# Ordenar

```python
df.sort_values("salary")
```

Descendente

```python
df.sort_values(
    "salary",
    ascending=False
)
```

Varias columnas

```python
df.sort_values(
    ["city", "salary"]
)
```

---

# Crear columnas

```python
df["salary_k"] = (
    df["salary"] / 1000
)
```

Otra

```python
df["is_adult"] = (
    df["age"] >= 18
)
```

---

# Modificar columnas

```python
df["salary"] *= 1.10
```

---

# Eliminar columnas

```python
df.drop(columns=["salary"])
```

Eliminar varias

```python
df.drop(
    columns=["age", "salary"]
)
```

---

# Renombrar columnas

```python
df.rename(columns={
    "age":"Age",
    "salary":"Salary"
})
```

---

# Valores únicos

```python
df["city"].unique()
```

Número

```python
df["city"].nunique()
```

Frecuencia

```python
df["city"].value_counts()
```

Muy útil en EDA.

---

# Valores nulos

Contarlos

```python
df.isna().sum()
```

Filas con nulos

```python
df[df.isna().any(axis=1)]
```

Eliminar

```python
df.dropna()
```

Rellenar

```python
df.fillna(0)
```

Con la media

```python
df["age"] = df["age"].fillna(
    df["age"].mean()
)
```

Con la mediana

```python
df["age"] = df["age"].fillna(
    df["age"].median()
)
```

Con la moda

```python
df["city"] = df["city"].fillna(
    df["city"].mode()[0]
)
```

---

# Operaciones sobre columnas

Media

```python
df["salary"].mean()
```

Máximo

```python
df["salary"].max()
```

Mínimo

```python
df["salary"].min()
```

Suma

```python
df["salary"].sum()
```

Desviación

```python
df["salary"].std()
```

---

# Apply

Muy útil cuando quieres transformar una columna.

```python
df["name"] = df["name"].apply(str.upper)
```

Otra

```python
df["age_squared"] = (
    df["age"].apply(lambda x: x**2)
)
```

Función propia

```python
def bmi(weight, height):
    return weight / height**2

df["BMI"] = df.apply(
    lambda row: bmi(
        row["weight"],
        row["height"]
    ),
    axis=1
)
```

---

# Map

Ideal para reemplazar categorías.

```python
gender = {
    "M": "Male",
    "F": "Female"
}

df["gender"] = (
    df["gender"].map(gender)
)
```

---

# Replace

```python
df["city"] = df["city"].replace({
    "NY":"New York"
})
```

---

# GroupBy ⭐

Probablemente la función más importante de Pandas.

Media por ciudad

```python
df.groupby("city")["salary"].mean()
```

Varias estadísticas

```python
df.groupby("city")["salary"].agg([
    "mean",
    "max",
    "min",
    "count"
])
```

Varias columnas

```python
df.groupby("city").agg({
    "salary":"mean",
    "age":"median"
})
```

---

# Pivot Table

```python
pd.pivot_table(

    df,

    index="city",

    values="salary",

    aggfunc="mean"

)
```

---

# Merge ⭐

Equivalente a SQL JOIN.

```python
merged = pd.merge(

    customers,

    orders,

    on="customer_id"

)
```

Left Join

```python
pd.merge(

    left,

    right,

    how="left",

    on="id"

)
```

---

# Concatenar

Vertical

```python
pd.concat([df1, df2])
```

Horizontal

```python
pd.concat(
    [df1, df2],
    axis=1
)
```

---

# Eliminar duplicados

```python
df.drop_duplicates()
```

Por columna

```python
df.drop_duplicates(
    subset="email"
)
```

---

# One-Hot Encoding

```python
pd.get_dummies(
    df["city"]
)
```

Con prefijo

```python
pd.get_dummies(
    df,
    columns=["city"]
)
```

---

# Muestreo

```python
df.sample(10)
```

Porcentaje

```python
df.sample(frac=0.2)
```

Semilla

```python
df.sample(
    5,
    random_state=42
)
```

---

# Cadena de operaciones

Muy recomendable.

```python
result = (

    df

    .dropna()

    .query("salary > 50000")

    .sort_values("salary")

    .head(10)

)
```

Mucho más legible que escribir 10 variables temporales.

---
---

# Query

Muy útil para filtrar usando expresiones.

```python
df.query("age >= 18")
```

Con varias condiciones

```python
df.query(
    "age >= 18 and salary > 30000"
)
```

Más legible que muchas máscaras booleanas.

---

# Cut

Divide una variable numérica en intervalos definidos por el usuario.

```python
ages = [18, 25, 32, 41, 58]

pd.cut(
    ages,
    bins=[0,18,30,50,100]
)
```

Muy útil para crear rangos de edad.

---

# qcut

Divide los datos según cuantiles.

```python
pd.qcut(
    df["salary"],
    q=4
)
```

Cuartiles.

Deciles

```python
pd.qcut(
    df["sales"],
    q=10
)
```

Muy utilizado en Marketing Analytics y RFM.

---

# Rank

Calcula rankings.

```python
df["salary_rank"] = (
    df["salary"]
    .rank(ascending=False)
)
```

Ideal para obtener posiciones.

---

# Sort Index

Ordenar por índice.

```python
df.sort_index()
```

Descendente

```python
df.sort_index(
    ascending=False
)
```

---

# Reset Index

Reinicia el índice.

```python
df.reset_index()
```

Eliminar el índice anterior

```python
df.reset_index(
    drop=True
)
```

---

# Set Index

Establecer una columna como índice.

```python
df.set_index("CustomerID")
```

Muy común cuando se trabaja con series temporales.

---

# Melt

Convierte columnas en filas.

Muy útil para reorganizar datos.

```python
pd.melt(
    df,
    id_vars=["Country"]
)
```

---

# Stack y Unstack

Reorganizan índices.

```python
df.stack()
```

```python
df.unstack()
```

Muy útiles después de `groupby()`.

---

# Explode

Convierte listas en múltiples filas.

```python
df.explode("Products")
```

Ejemplo

Antes

| Customer | Products |
|----------|----------------|
| Ana | [A,B,C] |

Después

| Customer | Products |
|----------|----------|
| Ana | A |
| Ana | B |
| Ana | C |

---

# Pipe

Permite construir pipelines más legibles.

```python
(
    df
    .pipe(clean_data)
    .pipe(create_features)
)
```

Muy utilizado en proyectos grandes.

---

# Assign

Crear columnas dentro de un pipeline.

```python
(
    df
    .assign(
        Sales=lambda x:
        x["Quantity"] *
        x["Price"]
    )
)
```

---

# Clip

Limita valores dentro de un rango.

```python
df["salary"] = (
    df["salary"]
    .clip(
        lower=0,
        upper=100000
    )
)
```

Muy útil para tratar outliers.

---

# Between

Filtrar rangos.

```python
df[
    df["age"].between(18,30)
]
```

Más limpio que escribir dos condiciones.

---

# Isin

Buscar múltiples valores.

```python
df[
    df["Country"].isin(
        ["USA","Canada","Mexico"]
    )
]
```

Muy utilizado para filtros.

---

# Nlargest y Nsmallest

Obtener los mayores valores.

```python
df.nlargest(
    10,
    "Sales"
)
```

Menores

```python
df.nsmallest(
    10,
    "Sales"
)
```

Más rápido que ordenar todo el DataFrame.

---

# Duplicated

Identificar duplicados.

```python
df.duplicated()
```

Duplicados por columna

```python
df.duplicated(
    subset="CustomerID"
)
```

---

# Sample

Muestreo aleatorio.

```python
df.sample(
    n=100,
    random_state=42
)
```

Muy útil para pruebas rápidas.

---

# Resample ⭐

Trabajar con series temporales.

Ventas mensuales

```python
(
    df
    .resample(
        "M",
        on="InvoiceDate"
    )["Sales"]
    .sum()
)
```

Ventas por semana

```python
(
    df
    .resample(
        "W",
        on="InvoiceDate"
    )["Sales"]
    .sum()
)
```

Fundamental en Time Series Analysis.

---

# Rolling

Ventanas móviles.

Media móvil

```python
df["Sales"].rolling(
    7
).mean()
```

Muy utilizado para suavizar series temporales.

---

# Shift

Mover una columna.

```python
df["PreviousSales"] = (
    df["Sales"]
    .shift(1)
)
```

Muy utilizado para crear variables Lag.

---

# Diff

Calcula diferencias.

```python
df["Difference"] = (
    df["Sales"]
    .diff()
)
```

---

# CumSum

Suma acumulada.

```python
df["CumSales"] = (
    df["Sales"]
    .cumsum()
)
```

Muy utilizado para Pareto.

---

# CumCount

Cuenta acumulada dentro de grupos.

```python
df.groupby(
    "CustomerID"
).cumcount()
```

---

# Transform

Transforma respetando el tamaño original.

```python
df["CountryMean"] = (
    df.groupby("Country")
    ["Sales"]
    .transform("mean")
)
```

Muy útil para Feature Engineering.

---

# NGroups

Número de grupos.

```python
df.groupby(
    "Country"
).ngroups
```

---

# Nunique

Valores únicos por grupo.

```python
df.groupby(
    "Country"
)["CustomerID"].nunique()
```

Muy utilizado en EDA.

---

# Métodos que más vas a usar en ML

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

groupby()

agg()

merge()

concat()

value_counts()

unique()

sort_values()

get_dummies()

sample()

apply()

map()

query()

iloc

loc
```

---

# Buenas prácticas

> [!tip]
>
> - Usa nombres de columnas en **snake_case**.
> - Evita modificar el DataFrame original si vas a experimentar (`df.copy()`).
> - Prefiere operaciones vectorizadas frente a `for`.
> - Encadena operaciones cuando mejore la legibilidad.
> - Usa `groupby` antes que bucles para resumir datos.

---

# Errores comunes

❌ Usar `for` para recorrer filas.

```python
for i in range(len(df)):
    ...
```

✔ Mejor

```python
df["salary"] *= 1.10
```

---

❌ Modificar un DataFrame filtrado.

```python
filtered = df[df["age"] > 18]

filtered["adult"] = True
```

Puede generar `SettingWithCopyWarning`.

✔ Mejor

```python
filtered = (
    df[df["age"] > 18]
    .copy()
)

filtered["adult"] = True
```

---

❌ Usar `apply()` para operaciones vectorizadas.

```python
df["salary"].apply(lambda x: x*2)
```

✔ Mejor

```python
df["salary"] * 2
```

Es mucho más rápido.

---

# Mi regla mental

Cuando no sepas qué usar, piensa:

> **¿Estoy seleccionando, transformando, agrupando o uniendo datos?**

- Seleccionar → `loc`, `iloc`, `query`
- Transformar → `apply`, `map`, operaciones vectorizadas
- Agrupar → `groupby`
- Unir → `merge`, `concat`

Con esas cuatro ideas resolverás el 90% de los problemas de Pandas.

## Enlaces

- [[NumPy]]
- [[Feature Engineering]]
- [[Feature Transformations]]