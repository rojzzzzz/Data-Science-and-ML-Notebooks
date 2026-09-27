# Data Preparation con Pandas
## Parte 1 - Fundamentos, Inspección y Limpieza Inicial

> [!important]
> La preparación de datos (**Data Preparation**) suele consumir entre el **60 % y el 80 % del tiempo** de un proyecto de Machine Learning.
>
> Un algoritmo sofisticado jamás compensará datos de mala calidad.
>
> La calidad de un modelo depende directamente de la calidad de los datos con los que fue entrenado.

---

# Objetivos

Al finalizar este documento serás capaz de:

- Comprender la importancia de la preparación de datos.
- Inspeccionar correctamente un DataFrame.
- Comprender el significado de las variables antes de modificarlas.
- Detectar valores faltantes.
- Seleccionar el método adecuado para tratar datos faltantes.
- Detectar y eliminar registros duplicados.
- Corregir tipos de datos.
- Limpiar variables de texto.
- Comprender buenas prácticas antes de entrenar cualquier modelo.

---

# ¿Qué es Data Preparation?

La preparación de datos es el proceso de transformar datos crudos en un conjunto de datos limpio, consistente y adecuado para el análisis o el entrenamiento de modelos de Machine Learning.

Incluye actividades como:

- Inspección del dataset.
- Limpieza de errores.
- Tratamiento de valores faltantes.
- Corrección de tipos de datos.
- Eliminación de duplicados.
- Limpieza de texto.
- Tratamiento de valores atípicos.
- Codificación de variables categóricas.
- Escalamiento de variables.
- Verificación final del dataset.

Aunque muchas personas utilizan el término **Data Cleaning**, este representa únicamente una parte del proceso de preparación de datos.

---

# ¿Por qué es tan importante?

Un modelo de Machine Learning aprende únicamente a partir de los datos que recibe.

Si los datos contienen errores, inconsistencias o información incorrecta, el modelo aprenderá exactamente esos errores.

Por ejemplo,

supongamos una columna de edades.

| Edad |
|------|
|25|
|31|
|999|
|27|

Si el valor **999** corresponde a un error de captura y no es corregido, muchos algoritmos asumirán que existe una persona de 999 años.

El algoritmo no sabe que ese dato es incorrecto.

Simplemente aprende a partir de él.

---

# Filosofía del Data Preparation

> [!quote]
>
> La calidad de un modelo nunca será superior a la calidad de sus datos.
>
> Antes de pensar en cambiar de algoritmo, mejora tus datos.
>
> En la práctica, una buena preparación de datos suele producir mejoras mayores que utilizar un modelo más complejo.

---

# Flujo General del Data Preparation

```text
Cargar datos
      │
      ▼
Comprender las variables
      │
      ▼
Inspección inicial
      │
      ▼
Valores faltantes
      │
      ▼
Tipos de datos
      │
      ▼
Texto
      │
      ▼
Duplicados
      │
      ▼
Outliers
      │
      ▼
Encoding
      │
      ▼
Escalamiento
      │
      ▼
Verificación final
```

No siempre será necesario ejecutar todos los pasos, pero este constituye un excelente flujo de trabajo para la mayoría de proyectos.

---

# 1. Comprender las Variables

Antes de modificar cualquier dato debes comprender perfectamente qué representa cada columna.

Preguntas recomendadas:

- ¿Qué representa esta variable?
- ¿Qué unidad utiliza?
- ¿Cómo fue recolectada?
- ¿Puede contener errores?
- ¿Qué valores son válidos?
- ¿Es una variable predictora?
- ¿Es la variable objetivo?

Por ejemplo,

| Variable | Significado |
|-----------|-------------|
|Age|Edad del cliente|
|Income|Ingreso anual en dólares|
|Balance|Saldo promedio de la cuenta|

Puede parecer obvio.

Sin embargo,

muchos errores de preparación ocurren precisamente porque el analista modifica variables cuyo significado desconoce.

> [!warning]
> Nunca limpies datos que no entiendas.

---

# 2. Inspección Inicial del DataFrame

Antes de modificar cualquier dato debemos inspeccionar el conjunto de datos.

El objetivo consiste en responder preguntas como:

- ¿Cuántas filas existen?
- ¿Cuántas columnas existen?
- ¿Qué tipos de datos contiene?
- ¿Existen valores faltantes?
- ¿Cómo lucen los primeros registros?

---

## Ver las primeras filas

```python
df.head()
```

Resultado típico

```text
   Age  Income Gender
0   25    1500      M
1   32    2200      F
2   41    3000      M
3   27    1800      F
4   54    6200      M
```

Generalmente se utilizan las primeras cinco filas.

También puede especificarse otro número.

```python
df.head(10)
```

---

## Ver las últimas filas

```python
df.tail()
```

Muy útil para detectar problemas ocurridos al final del archivo.

---

## Conocer las dimensiones

```python
df.shape
```

Resultado

```python
(10000,20)
```

Interpretación

- 10,000 filas.
- 20 columnas.

---

## Información general

```python
df.info()
```

Ejemplo

```text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10000 entries

Age          9988 non-null float64
Income       9955 non-null float64
Gender      10000 non-null object
```

Esta función permite conocer rápidamente:

- cantidad de filas;
- columnas;
- tipos de datos;
- valores no nulos;
- consumo de memoria.

Es probablemente una de las funciones más utilizadas en Pandas.

---

## Estadísticas descriptivas

```python
df.describe()
```

Obtiene estadísticas para variables numéricas.

Por ejemplo,

- media;
- desviación estándar;
- mínimo;
- máximo;
- cuartiles.

Esto ayuda a detectar rápidamente valores sospechosos.

---

# 3. Identificación de Valores Faltantes

Los valores faltantes constituyen uno de los problemas más frecuentes en Ciencia de Datos.

Generalmente aparecen como

```python
NaN
```

(No a Number)

---

## Contar valores faltantes

```python
df.isnull().sum()
```

Resultado

```text
Age          12
Income       45
Gender        0
State         3
```

Esto indica la cantidad de valores faltantes por columna.

---

## Obtener el porcentaje

Muchas veces el porcentaje resulta más útil.

```python
(df.isnull().sum()/len(df))*100
```

Ejemplo

| Variable | % Faltantes |
|-----------|------------:|
|Age|0.12 %|
|Income|0.45 %|
|Gender|0 %|

---

# ¿Siempre debemos eliminar columnas con valores faltantes?

No.

La decisión depende de varios factores.

| Situación | Recomendación |
|------------|--------------|
|Muy pocos valores faltantes|Imputar|
|Muchísimos valores faltantes|Evaluar eliminar la columna|
|Variable muy importante|Intentar conservarla|
|Variable poco útil|Eliminarla|

No existe una regla universal.

Depende del contexto.

---

# 4. Tratamiento de Valores Faltantes

Existen múltiples estrategias.

La elección correcta depende del tipo de variable y del problema.

---

## Eliminar filas

```python
df.dropna()
```

Útil cuando existen muy pocos registros incompletos.

Desventaja.

Puede eliminar mucha información.

---

## Eliminar columnas

```python
df.dropna(axis=1)
```

Se utiliza únicamente cuando una columna posee demasiados valores faltantes.

---

## Reemplazar por la media

```python
df["Age"] = df["Age"].fillna(
    df["Age"].mean()
)
```

Recomendado cuando

- la variable es numérica;
- la distribución es aproximadamente simétrica;
- no existen muchos outliers.

---

## Reemplazar por la mediana

```python
df["Income"] = df["Income"].fillna(
    df["Income"].median()
)
```

La mediana resulta mucho más robusta frente a valores extremos.

Generalmente es la mejor alternativa para variables económicas.

---

## Reemplazar por la moda

```python
df["Gender"] = df["Gender"].fillna(
    df["Gender"].mode()[0]
)
```

Se utiliza principalmente con variables categóricas.

---

## Reemplazar por un valor fijo

```python
df["State"] = df["State"].fillna(
    "Unknown"
)
```

Muy útil cuando "desconocido" constituye una categoría válida.

---

# ¿Cuál método debo utilizar?

| Método | Cuándo utilizarlo |
|---------|-------------------|
|Media|Variables aproximadamente normales|
|Mediana|Variables con outliers|
|Moda|Variables categóricas|
|Valor fijo|Cuando "desconocido" tiene significado|
|Eliminar filas|Pocos registros afectados|
|Eliminar columna|Gran cantidad de datos faltantes|

---

# Buenas prácticas

✔ Comprender por qué existen los valores faltantes.

✔ No asumir que todos deben eliminarse.

✔ Documentar siempre el método utilizado.

✔ Evaluar el impacto de cada decisión sobre el modelo.

> [!tip]
> Los valores faltantes también contienen información. En algunos problemas, el hecho de que un dato esté ausente puede ser una característica importante.

---

# Resumen

En esta primera parte aprendimos que:

- La preparación de datos constituye una de las etapas más importantes de Machine Learning.
- Antes de limpiar un dataset debemos comprender el significado de sus variables.
- La inspección inicial permite conocer rápidamente la estructura de los datos.
- Los valores faltantes pueden tratarse de distintas maneras según el contexto.
- No existe una única estrategia correcta; la elección depende del tipo de variable y del problema que se desea resolver.

En la **Parte 2** continuaremos con:

- Corrección de tipos de datos.
- Limpieza de texto.
- Duplicados.
- Outliers.
- Data Leakage.
- Label Encoding vs One-Hot Encoding.
- Escalamiento de variables.
- Verificación final.
- Checklist profesional para Data Preparation.

# Data Preparation con Pandas
## Parte 2 - Limpieza Avanzada, Codificación y Verificación Final

> [!important]
> La preparación de datos no termina cuando desaparecen los valores nulos.
>
> Un dataset puede no tener valores faltantes y aun así contener errores que afecten seriamente el rendimiento de un modelo.
>
> En esta segunda parte estudiaremos las etapas finales del proceso de preparación de datos.

---

# Objetivos

Al finalizar esta sección serás capaz de:

- Corregir tipos de datos.
- Limpiar variables de texto.
- Detectar y eliminar duplicados.
- Analizar correctamente los outliers.
- Comprender qué es Data Leakage.
- Diferenciar Label Encoding y One-Hot Encoding.
- Escalar variables correctamente.
- Realizar una verificación profesional antes de entrenar un modelo.

---

# 5. Corrección de Tipos de Datos

Uno de los errores más comunes consiste en importar números como texto.

Por ejemplo,

| Sales |
|--------|
|100|
|250|
|350|

Aunque parecen números, Pandas podría interpretarlos como cadenas (`object`).

---

## Revisar tipos de datos

```python
df.dtypes
```

Resultado

```text
Age          float64
Income       object
Gender       object
```

Observamos que `Income` fue importado como texto.

---

## Convertir a numérico

```python
df["Income"] = pd.to_numeric(
    df["Income"]
)
```

---

## Ignorar errores

Cuando algunos registros contienen valores inválidos.

```python
df["Income"] = pd.to_numeric(
    df["Income"],
    errors="coerce"
)
```

Los valores imposibles se convierten automáticamente en

```python
NaN
```

y posteriormente podrán tratarse como cualquier otro valor faltante.

---

## Convertir a entero

```python
df["Age"] = df["Age"].astype(int)
```

Solo funciona si la columna no contiene valores nulos.

---

## Mantener NaN utilizando Int64

```python
df["Age"] = df["Age"].astype("Int64")
```

Este tipo de dato permite almacenar enteros sin perder los valores faltantes.

---

## Convertir fechas

```python
df["Date"] = pd.to_datetime(
    df["Date"]
)
```

Esto permite posteriormente realizar operaciones como:

- diferencias entre fechas;
- extracción de meses;
- extracción de años;
- análisis temporal.

---

## Convertir a categoría

```python
df["State"] = df["State"].astype(
    "category"
)
```

Ventajas

- Reduce memoria.
- Acelera algunas operaciones.
- Representa correctamente variables categóricas.

---

# 6. Limpieza de Texto

Las variables de texto suelen contener errores de escritura.

Ejemplo.

```text
Managua
MANAGUA
managua
 Managua
Managa
```

Para un algoritmo todas ellas representan categorías distintas.

---

## Eliminar espacios

```python
df["Name"] = df["Name"].str.strip()
```

Muy útil para eliminar espacios al inicio o al final.

---

## Convertir a minúsculas

```python
df["Name"] = (
    df["Name"]
      .str.lower()
)
```

---

## Convertir a mayúsculas

```python
df["Name"] = (
    df["Name"]
      .str.upper()
)
```

---

## Reemplazar texto

```python
df["Gender"] = df["Gender"].replace({

    "M":"Male",

    "F":"Female"

})
```

Esto permite unificar categorías.

---

## Detectar errores tipográficos

```python
df["State"].value_counts()
```

Resultado.

```text
Managua      520

MANAGUA       15

managua        8

Managa         2
```

Claramente estas categorías deberían unificarse.

---

# 7. Duplicados

Los registros duplicados pueden producir modelos sesgados.

---

## Contar duplicados

```python
df.duplicated().sum()
```

---

## Mostrar duplicados

```python
df[df.duplicated()]
```

---

## Eliminarlos

```python
df = df.drop_duplicates()
```

---

> [!warning]
> Antes de eliminar duplicados verifica que realmente sean registros repetidos y no observaciones diferentes con valores similares.

---

# 8. Outliers

Los outliers son observaciones extremadamente alejadas del resto de los datos.

Por ejemplo.

```
20

25

28

30

27

26

999
```

El valor

```
999
```

es un posible outlier.

---

## ¿Siempre deben eliminarse?

No.

Un outlier puede representar:

- un error de captura;
- un error del sensor;
- un fraude;
- un cliente excepcional;
- un evento raro pero completamente válido.

Nunca elimines un outlier sin comprender su origen.

---

## Método IQR

Calcular cuartiles.

```python
Q1 = df["Income"].quantile(0.25)

Q3 = df["Income"].quantile(0.75)
```

Calcular IQR.

```python
IQR = Q3 - Q1
```

Límites.

```python
lower = Q1 - 1.5*IQR

upper = Q3 + 1.5*IQR
```

Detectar outliers.

```python
outliers = df[

    (df["Income"]<lower)

    |

    (df["Income"]>upper)

]
```

---

## Eliminarlos

```python
df = df[

(df["Income"]>=lower)

&

(df["Income"]<=upper)

]
```

---

# 9. Data Leakage

Esta es una de las causas más frecuentes de modelos aparentemente excelentes que fracasan completamente en producción.

## ¿Qué es?

Data Leakage ocurre cuando el modelo recibe información que en la realidad nunca tendría disponible al momento de realizar una predicción.

Ejemplos.

❌ Escalar utilizando todo el dataset antes del Train/Test Split.

❌ Imputar valores utilizando información del conjunto de prueba.

❌ Crear variables utilizando información futura.

---

## Flujo correcto

```text
Dataset

↓

Train/Test Split

↓

Ajustar transformaciones SOLO con Training

↓

Aplicarlas al Test Set

↓

Entrenar modelo
```

> [!danger]
> El Data Leakage produce métricas artificialmente altas y una falsa sensación de éxito.

---

# 10. Codificación de Variables Categóricas

Muchos algoritmos solo trabajan con números.

Debemos convertir las categorías.

---

# Label Encoding

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["Gender"] = encoder.fit_transform(
    df["Gender"]
)
```

Resultado.

```text
Female → 0

Male → 1
```

---

> [!warning]
> `LabelEncoder` está pensado principalmente para codificar la variable objetivo (`y`).
>
> Para variables predictoras suele ser preferible utilizar One-Hot Encoding.

---

# One-Hot Encoding con Pandas

```python
pd.get_dummies(
    df,
    columns=["State"]
)
```

Resultado.

```
State_Managua

State_Leon

State_Masaya
```

Cada categoría se convierte en una nueva columna binaria.

---

# ¿Cuándo utilizar cada uno?

| Método | Cuándo utilizarlo |
|----------|------------------|
|LabelEncoder|Variable objetivo|
|OneHotEncoder|Variables predictoras sin orden|
|OrdinalEncoder|Variables categóricas con orden natural|

---

# 11. Escalamiento

Muchos modelos dependen de las distancias.

Ejemplos.

- K-Means
- KNN
- PCA
- SVM

Si las variables poseen escalas muy distintas,

una dominará completamente a las demás.

---

## StandardScaler

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X = scaler.fit_transform(X)
```

Produce:

- media = 0
- desviación estándar = 1

Es el método más utilizado.

---

## MinMaxScaler

```python
from sklearn.preprocessing import MinMaxScaler
```

Transforma todos los valores al intervalo

```
0

↓

1
```

Muy útil en Redes Neuronales.

---

## RobustScaler

Utiliza la mediana y el IQR.

Es especialmente recomendable cuando existen muchos outliers.

---

## ¿Cuál utilizar?

| Escalador | Cuándo utilizarlo |
|------------|------------------|
|StandardScaler|La mayoría de los casos|
|MinMaxScaler|Redes Neuronales|
|RobustScaler|Muchos outliers|

---

# 12. Verificación Final

Antes de entrenar cualquier modelo verifica nuevamente el dataset.

```python
df.info()
```

```python
df.shape
```

```python
df.isnull().sum()
```

```python
df.describe()
```

Nunca entrenes un modelo sin revisar nuevamente estas funciones.

---

# Checklist Profesional

Antes de entrenar un modelo verifica:

- [ ] Comprendo perfectamente todas las variables.
- [ ] No existen valores faltantes sin tratar.
- [ ] Los tipos de datos son correctos.
- [ ] No existen registros duplicados.
- [ ] Las categorías fueron estandarizadas.
- [ ] Los outliers fueron revisados.
- [ ] No existe Data Leakage.
- [ ] Las variables categóricas fueron codificadas.
- [ ] Las variables fueron escaladas cuando corresponde.
- [ ] El Train/Test Split se realizó antes de ajustar las transformaciones.
- [ ] El dataset está listo para el modelado.

---

# Flujo Profesional de Data Preparation

```text
Carga de datos
      │
      ▼
Comprender variables
      │
      ▼
Inspección inicial
      │
      ▼
Valores faltantes
      │
      ▼
Tipos de datos
      │
      ▼
Texto y categorías
      │
      ▼
Duplicados
      │
      ▼
Outliers
      │
      ▼
Train/Test Split
      │
      ▼
Encoding
      │
      ▼
Escalamiento
      │
      ▼
Verificación final
      │
      ▼
Modelo de Machine Learning
```

---

# Filosofía del Data Preparation

> [!quote]
>
> La mayoría de las mejoras en Machine Learning no provienen de cambiar de algoritmo.
>
> Provienen de comprender mejor los datos.
>
> Un científico de datos dedica mucho más tiempo preparando los datos que entrenando modelos.
>
> Aprender a preparar correctamente un dataset es una de las habilidades más valiosas en Ciencia de Datos.

---

# Regla de Oro

> [!success]
>
> Antes de probar un algoritmo más complejo, pregúntate:
>
> - ¿Mis datos están limpios?
> - ¿Comprendo todas las variables?
> - ¿Existe Data Leakage?
> - ¿Las categorías están correctamente codificadas?
> - ¿Las variables necesitan escalamiento?
>
> En la mayoría de los proyectos reales, **mejorar los datos produce un impacto mucho mayor que cambiar de algoritmo**.