

> [!abstract]
> Pandas ofrece un conjunto de herramientas especializadas para trabajar con fechas y horas. Gracias al tipo de dato **`datetime64[ns]`** y al **`DatetimeIndex`**, podemos filtrar, agrupar, re-muestrear y extraer componentes temporales de forma muy eficiente. Estas funcionalidades constituyen la base del análisis de Series de Tiempo.

---

# ¿Por qué Pandas tiene herramientas especiales para fechas?

Aunque Python dispone del módulo `datetime`, trabajar con miles o millones de fechas utilizando únicamente objetos `datetime` sería poco eficiente.

Pandas implementa un tipo de dato optimizado llamado:

```text
datetime64[ns]
```

Este formato permite realizar operaciones vectorizadas sobre columnas completas, haciéndolas mucho más rápidas que un ciclo `for`.

---

# Convertir una columna a tipo datetime

La mayoría de los archivos CSV o Excel almacenan las fechas como texto.

Ejemplo:

| fecha |
|--------|
|2026-07-17|
|2026-07-18|
|2026-07-19|

Si verificamos el tipo de dato:

```python
df.dtypes
```

Obtendremos:

```text
fecha    object
```

Esto significa que Pandas la interpreta como un **string**.

Para convertirla:

```python
df["fecha"] = pd.to_datetime(df["fecha"])
```

Ahora:

```python
df.dtypes
```

Resultado:

```text
fecha    datetime64[ns]
```

> [!tip]
> Convierte las fechas a `datetime` inmediatamente después de leer un archivo. Evitarás muchos errores posteriores.

---

# Especificar el formato

Cuando las fechas no están en formato ISO, conviene indicar el formato explícitamente.

Ejemplo:

```python
df["fecha"] = pd.to_datetime(
    df["fecha"],
    format="%d/%m/%Y"
)
```

Esto hace que la conversión sea:

- Más rápida.
- Más segura.
- Menos propensa a errores.

---

# Manejo de errores

A veces encontramos fechas inválidas.

Ejemplo:

```text
31/02/2026
```

Podemos indicarle a Pandas qué hacer.

```python
pd.to_datetime(
    df["fecha"],
    errors="coerce"
)
```

Las fechas inválidas se convertirán en:

```text
NaT
```

(**Not a Time**)

Equivalente temporal de un `NaN`.

---

# El accesor `.dt`

Una vez que la columna es `datetime`, aparece un nuevo accesor:

```python
.dt
```

Gracias a él podemos acceder a todas las partes de la fecha.

---

## Año

```python
df["fecha"].dt.year
```

---

## Mes

```python
df["fecha"].dt.month
```

---

## Día

```python
df["fecha"].dt.day
```

---

## Hora

```python
df["fecha"].dt.hour
```

---

## Minutos

```python
df["fecha"].dt.minute
```

---

## Segundo

```python
df["fecha"].dt.second
```

---

## Día de la semana

```python
df["fecha"].dt.dayofweek
```

Resultado

|Valor|Día|
|------|---|
|0|Lunes|
|1|Martes|
|2|Miércoles|
|3|Jueves|
|4|Viernes|
|5|Sábado|
|6|Domingo|

---

## Nombre del día

```python
df["fecha"].dt.day_name()
```

Resultado

```text
Friday
```

---

## Nombre del mes

```python
df["fecha"].dt.month_name()
```

Resultado

```text
July
```

---

## Trimestre

```python
df["fecha"].dt.quarter
```

Resultado

```text
3
```

---

## Día del año

```python
df["fecha"].dt.dayofyear
```

---

## Semana del año

```python
df["fecha"].dt.isocalendar().week
```

---

# Formatear fechas

Podemos utilizar `strftime()` también en Pandas.

```python
df["fecha"].dt.strftime("%d/%m/%Y")
```

Resultado

```text
17/07/2026
```

---

# Filtrar por fecha

```python
df[df["fecha"] >= "2026-07-01"]
```

Pandas convierte automáticamente el string a una fecha para realizar la comparación.

---

También podemos usar objetos `datetime`.

```python
from datetime import datetime

df[df["fecha"] > datetime(2026,7,1)]
```

---

# Ordenar por fecha

```python
df.sort_values("fecha")
```

Muy recomendable antes de analizar Series de Tiempo.

> [!warning]
> Muchos algoritmos asumen que las observaciones ya están ordenadas cronológicamente.

---

# DatetimeIndex

Una de las características más poderosas de Pandas consiste en utilizar las fechas como índice.

```python
df = df.set_index("fecha")
```

Ahora el índice del DataFrame será:

```text
DatetimeIndex
```

En lugar de:

```text
RangeIndex
```

---

# Ventajas del DatetimeIndex

Permite realizar consultas muy naturales.

Por ejemplo:

```python
df.loc["2026"]
```

Devuelve todo el año.

---

```python
df.loc["2026-07"]
```

Devuelve únicamente julio.

---

```python
df.loc["2026-07-17"]
```

Devuelve únicamente ese día.

---

También podemos consultar intervalos.

```python
df.loc["2026-07":"2026-09"]
```

---

# Indexación parcial

Una característica exclusiva del `DatetimeIndex`.

```python
df.loc["2026"]
```

No necesitamos escribir:

```
2026-01-01
```

ni

```
2026-12-31
```

Pandas entiende automáticamente que queremos todo el año.

---

# Valores faltantes en Series Temporales

Cuando faltan fechas aparecen problemas.

Ejemplo:

```text
2026-07-01
2026-07-02
2026-07-05
```

Faltan:

```
03
04
```

Más adelante veremos técnicas como:

- Resampling
- Forward Fill
- Backward Fill
- Interpolación

para solucionar estos problemas.

---

# Buenas Prácticas

> [!success]
>
> - Convierte las fechas usando `pd.to_datetime()`.
> - Utiliza `DatetimeIndex` cuando trabajes con Series Temporales.
> - Ordena siempre las observaciones por fecha.
> - Usa `.dt` para extraer componentes temporales.
> - Evita almacenar fechas como texto.

---

# Resumen

> [!success]
> Después de este cuaderno deberías saber:
>
> - Convertir texto a fechas.
> - Utilizar `datetime64[ns]`.
> - Extraer año, mes, día, hora y trimestre.
> - Filtrar por fechas.
> - Formatear fechas.
> - Crear un `DatetimeIndex`.
> - Realizar indexación temporal eficiente.

---

# Conceptos Relacionados

- [[Datetime en Python]]
- [[Introducción a las Series de Tiempo]]
- [[Feature Engineering para Series Temporales]]
- [[Resampling]]
- [[Rolling Window]]
- [[Moving Average]]
- [[EDA para Series Temporales]]

---

# shift(): Desplazar observaciones en el tiempo

> [!abstract]
> `shift()` desplaza los valores de una serie hacia adelante o hacia atrás sin modificar el índice temporal. Es una de las funciones más utilizadas para crear variables históricas (*lag features*) en Machine Learning y Series de Tiempo.

---

# ¿Para qué sirve?

Muchas veces queremos responder preguntas como:

- ¿Cuánto se vendió ayer?
- ¿Cuál fue la temperatura de hace una semana?
- ¿Cómo cambió el precio respecto al día anterior?

Para ello utilizamos **`shift()`**.

Supongamos el siguiente DataFrame.

| Fecha | Ventas |
|--------|--------:|
|01/01|100|
|02/01|120|
|03/01|115|
|04/01|130|

Si ejecutamos:

```python
df["ventas_ayer"] = df["ventas"].shift(1)
```

Obtendremos:

| Fecha | Ventas | ventas_ayer |
|--------|--------:|------------:|
|01/01|100|NaN|
|02/01|120|100|
|03/01|115|120|
|04/01|130|115|

Cada fila contiene ahora el valor del día anterior.

---

# Desplazamiento positivo

```python
df["ventas"].shift(1)
```

Desplaza los datos **hacia abajo**.

Es decir:

```
Hoy ← Ayer
```

---

# Desplazamiento negativo

```python
df["ventas"].shift(-1)
```

Resultado

| Fecha | Ventas | Mañana |
|--------|--------:|--------:|
|01/01|100|120|
|02/01|120|115|
|03/01|115|130|
|04/01|130|NaN|

Ahora observamos el valor del día siguiente.

---

# Crear variables Lag

Una de las aplicaciones más importantes.

```python
df["lag_1"] = df["ventas"].shift(1)
df["lag_7"] = df["ventas"].shift(7)
df["lag_30"] = df["ventas"].shift(30)
```

Estas columnas son conocidas como:

- Lag Features
- Variables rezagadas

Son ampliamente utilizadas en modelos de predicción.

---

# Diferencias entre observaciones

```python
df["cambio"] = df["ventas"] - df["ventas"].shift(1)
```

Resultado

|Fecha|Ventas|Cambio|
|------|------:|------:|
|01/01|100|NaN|
|02/01|120|20|
|03/01|115|-5|
|04/01|130|15|

---

# rolling(): Ventanas móviles

> [!abstract]
> `rolling()` crea una ventana deslizante sobre los datos para calcular estadísticas como medias, máximos, mínimos y desviaciones estándar.

---

# ¿Por qué necesitamos rolling()?

Los datos diarios suelen contener mucho ruido.

Ejemplo:

| Día | Ventas |
|------|--------:|
|1|100|
|2|140|
|3|95|
|4|160|
|5|110|

Es difícil identificar la tendencia únicamente observando estos valores.

Una solución consiste en calcular un promedio de varios días consecutivos.

---

# Media móvil

```python
df["media_3"] = df["ventas"].rolling(3).mean()
```

Resultado

|Ventas|Media móvil|
|------:|----------:|
|100|NaN|
|140|NaN|
|95|111.67|
|160|131.67|
|110|121.67|

La ventana contiene siempre los **últimos tres valores**.

---

# Otras estadísticas

Promedio

```python
rolling().mean()
```

Máximo

```python
rolling().max()
```

Mínimo

```python
rolling().min()
```

Suma

```python
rolling().sum()
```

Desviación estándar

```python
rolling().std()
```

Varianza

```python
rolling().var()
```

---

# ¿Qué significa el tamaño de la ventana?

```python
rolling(7)
```

Significa:

"Utiliza las últimas **7 observaciones**."

No necesariamente son siete días.

Si la serie es mensual:

```python
rolling(12)
```

corresponde a los últimos doce meses.

---

# ¿Para qué se utilizan las medias móviles?

Las Moving Averages sirven para:

- Eliminar ruido.
- Detectar tendencias.
- Suavizar señales.
- Crear nuevas variables.
- Análisis financiero.

---

# resample(): Cambiar la frecuencia temporal

> [!abstract]
> `resample()` permite convertir una Serie de Tiempo de una frecuencia a otra.

Ejemplos:

- Diario → Mensual
- Horario → Diario
- Minutos → Horas

---

# ¿Por qué necesitamos resample()?

Supongamos ventas diarias.

|Fecha|Ventas|
|------|------:|
|01/01|120|
|02/01|150|
|03/01|140|

Pero ahora queremos conocer las ventas mensuales.

No necesitamos recorrer el DataFrame manualmente.

Simplemente utilizamos:

```python
df.resample("M").sum()
```

---

# Frecuencias más utilizadas

|Código|Significado|
|-------|-----------|
|`D`|Día|
|`W`|Semana|
|`M`|Mes|
|`Q`|Trimestre|
|`Y`|Año|
|`H`|Hora|
|`min`|Minutos|

---

# Ejemplos

Promedio mensual

```python
df.resample("M").mean()
```

---

Suma mensual

```python
df.resample("M").sum()
```

---

Máximo semanal

```python
df.resample("W").max()
```

---

Cantidad de observaciones por mes

```python
df.resample("M").count()
```

---

> [!warning]
> `resample()` solamente funciona cuando el índice del DataFrame es un **DatetimeIndex**.

Antes de utilizarlo normalmente hacemos:

```python
df["fecha"] = pd.to_datetime(df["fecha"])

df = df.set_index("fecha")
```

---

# Diferencias entre groupby() y resample()

Muchas personas confunden ambas funciones.

## groupby()

Agrupa utilizando una columna.

```python
df.groupby("categoria")
```

---

## resample()

Agrupa utilizando el tiempo.

```python
df.resample("M")
```

Aunque internamente realiza una operación similar a un `groupby`, está especializado para trabajar con fechas.

---

# Resumen

> [!success]
>
> - `shift()` desplaza observaciones en el tiempo.
> - Se utiliza para crear **Lag Features**.
> - `rolling()` calcula estadísticas sobre ventanas móviles.
> - Las Moving Averages ayudan a reducir el ruido.
> - `resample()` cambia la frecuencia temporal de una serie.
> - `resample()` requiere un `DatetimeIndex`.

---

# Conceptos Relacionados

- [[Datetime en Python]]
- [[Introducción a las Series de Tiempo]]
- [[Feature Engineering para Series Temporales]]
- [[Moving Average]]
- [[EDA para Series Temporales]]
- [[ARIMA]]