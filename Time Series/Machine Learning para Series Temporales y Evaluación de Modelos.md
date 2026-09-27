

> [!abstract]
> Aunque modelos clásicos como **ARIMA** siguen siendo muy importantes, en muchos problemas actuales se utilizan algoritmos de **Machine Learning** para realizar pronósticos. La diferencia principal es que estos modelos **no entienden el tiempo de forma nativa**; debemos convertir la información temporal en variables (*features*) que el algoritmo pueda aprender. Además, la evaluación de estos modelos requiere técnicas especiales para evitar utilizar información del futuro.

---

# Modelos Estadísticos vs Machine Learning

Antes de elegir un modelo, conviene entender sus diferencias.

|Modelos Estadísticos|Machine Learning|
|--------------------|----------------|
|ARIMA, SARIMA|Random Forest, XGBoost, LightGBM|
|Suponen relaciones estadísticas.|Aprenden patrones desde los datos.|
|Muy interpretables.|Generalmente más complejos.|
|Funcionan bien con pocos datos.|Suelen requerir más datos.|
|Capturan relaciones lineales.|Capturan relaciones no lineales.|

> [!tip]
> Ningún enfoque es universalmente mejor. La elección depende del problema, la cantidad de datos y el objetivo del análisis.

---

# El gran reto del Machine Learning

Supongamos este conjunto de datos.

|Fecha|Ventas|
|------|------:|
|01/01|100|
|02/01|120|
|03/01|110|

Para un algoritmo como Random Forest, la fecha es simplemente un dato.

No comprende automáticamente conceptos como:

- Ayer.
- La semana pasada.
- Navidad.
- Fin de semana.
- Estacionalidad.

Debemos convertir esa información en variables numéricas.

Aquí entra en juego el **Feature Engineering**.

---

# Variables que suelen funcionar bien

Las más utilizadas son:

## Variables de calendario

- Año.
- Mes.
- Semana.
- Día.
- Día de la semana.
- Fin de semana.
- Hora.

---

## Variables históricas

```python
ventas_lag_1

ventas_lag_7

ventas_lag_30
```

---

## Estadísticas móviles

```python
rolling_mean_7

rolling_std_7

rolling_max_30
```

---

## Variables cíclicas

```python
month_sin

month_cos

hour_sin

hour_cos
```

---

# Algoritmos más utilizados

## Random Forest

Ventajas

- Fácil de entrenar.
- Poco sensible a valores atípicos.
- Captura relaciones no lineales.

Limitaciones

- No extrapola bien tendencias largas.
- Puede requerir bastante memoria.

---

## XGBoost

Uno de los algoritmos más utilizados en competencias de Data Science.

Ventajas

- Excelente precisión.
- Maneja relaciones complejas.
- Muy robusto.

Limitaciones

- Requiere ajuste de hiperparámetros.
- Mayor tiempo de entrenamiento.

---

## LightGBM

Muy parecido a XGBoost.

Ventajas

- Más rápido.
- Consume menos memoria.
- Excelente para grandes volúmenes de datos.

---

## Prophet

Desarrollado por Meta (Facebook).

Está diseñado específicamente para Series Temporales.

Modela automáticamente:

- Tendencia.
- Estacionalidad.
- Festivos.
- Cambios estructurales.

Ejemplo

```python
from prophet import Prophet

modelo = Prophet()

modelo.fit(df)
```

> [!info]
> Prophet es una excelente opción cuando se necesita construir un modelo rápidamente con poco ajuste manual.

---

# ¿Qué datos usamos para entrenar?

En Machine Learning tradicional solemos dividir los datos aleatoriamente.

```
Train

↓

Test
```

Esto funciona para clasificación y regresión.

Pero **no** para Series Temporales.

---

# ¿Por qué no debemos mezclar las fechas?

Imaginemos esta serie.

```
2018

2019

2020

2021

2022
```

Si entrenamos con datos de 2022 y luego intentamos predecir 2020,

estaríamos utilizando información del futuro.

Esto produce **Data Leakage**.

---

# Data Leakage

> [!warning]
> **Data Leakage** ocurre cuando el modelo tiene acceso, directa o indirectamente, a información que en un escenario real aún no existiría.

En Series Temporales esto suele suceder cuando:

- Se mezclan aleatoriamente los datos.
- Se calculan estadísticas utilizando observaciones futuras.
- Se construyen variables con información posterior al instante que se desea predecir.

El resultado suele ser un modelo con un desempeño aparentemente excelente, pero incapaz de generalizar en producción.

---

# División correcta

Siempre respetamos el orden cronológico.

```
2018

2019

2020

2021

2022

──────────────

Train

──────────────

Test
```

Nunca al revés.

---

# TimeSeriesSplit

Scikit-Learn incluye una validación especial.

```python
from sklearn.model_selection import TimeSeriesSplit

tscv = TimeSeriesSplit(
    n_splits=5
)
```

En cada iteración:

- El conjunto de entrenamiento crece.
- El conjunto de prueba siempre contiene datos posteriores.

---

## Esquema

```
Split 1

Train | Test

------------

Split 2

Train Train | Test

-------------------

Split 3

Train Train Train | Test
```

Esto imita el comportamiento real de un sistema de pronósticos.

---

# Walk-Forward Validation

Una estrategia aún más realista.

Proceso

```
Entrenar

↓

Predecir siguiente período

↓

Agregar dato real

↓

Reentrenar

↓

Predecir nuevamente
```

Es muy utilizada en:

- Finanzas.
- Energía.
- Retail.
- Pronósticos de demanda.

---

# Métricas de evaluación

## MAE

**Mean Absolute Error**

```
Promedio del error absoluto.
```

Ventajas

- Fácil de interpretar.
- Misma unidad que la variable.

Limitación

- No penaliza especialmente errores grandes.

---

## RMSE

**Root Mean Squared Error**

Eleva los errores al cuadrado antes de promediarlos.

Por ello penaliza más los errores grandes.

Muy utilizada cuando un error grande tiene un costo elevado.

---

## MAPE

**Mean Absolute Percentage Error**

Expresa el error en porcentaje.

Ejemplo

```
MAPE = 8%
```

Interpretación

El modelo se equivoca aproximadamente un 8% en promedio.

> [!warning]
> MAPE puede comportarse mal cuando existen valores reales iguales o muy cercanos a cero.

---

## sMAPE

**Symmetric Mean Absolute Percentage Error**

Corrige varias limitaciones del MAPE y suele ser preferible cuando la serie contiene valores pequeños.

---

# ¿Qué métrica elegir?

|Métrica|¿Cuándo utilizarla?|
|---------|-------------------|
|MAE|Cuando todos los errores tienen el mismo peso.|
|RMSE|Cuando los errores grandes son especialmente costosos.|
|MAPE|Cuando interesa comunicar el error como porcentaje.|
|sMAPE|Cuando existen valores pequeños o cercanos a cero.|

---

# Pipeline recomendado

Un flujo de trabajo típico sería:

```
Datos

↓

EDA

↓

Limpieza

↓

Feature Engineering

↓

Train/Test Temporal

↓

Entrenamiento

↓

Evaluación

↓

Optimización

↓

Pronóstico
```

---

# Comparar varios modelos

Nunca debemos asumir que el primer modelo será el mejor.

Una práctica recomendada es entrenar varios candidatos.

Ejemplo

|Modelo|MAE|RMSE|
|--------|---:|---:|
|ARIMA|12.5|18.4|
|Random Forest|11.1|16.7|
|XGBoost|9.8|14.5|
|Prophet|10.7|15.6|

Seleccionamos el modelo considerando:

- Precisión.
- Interpretabilidad.
- Tiempo de entrenamiento.
- Facilidad de mantenimiento.

---

# Buenas Prácticas

> [!success]
>
> - Nunca mezcles observaciones futuras con datos de entrenamiento.
> - Realiza un buen Feature Engineering antes de entrenar modelos de Machine Learning.
> - Compara varios algoritmos; no existe un modelo universalmente superior.
> - Evalúa el modelo con datos futuros que no haya visto durante el entrenamiento.
> - Utiliza una línea base sencilla (por ejemplo, ARIMA) antes de probar modelos más complejos.

---

# Resumen General del Flujo de Trabajo

```text
Serie Temporal

↓

EDA

↓

Estacionariedad

↓

Feature Engineering

↓

Train/Test Temporal

↓

Modelo

↓

Evaluación

↓

Pronóstico
```

---

# Resumen

> [!success]
> Después de este cuaderno deberías comprender:
>
> - La diferencia entre modelos estadísticos y de Machine Learning para Series Temporales.
> - Cómo transformar una serie temporal en un conjunto de variables para Machine Learning.
> - Qué algoritmos se utilizan con mayor frecuencia.
> - Por qué no debemos mezclar datos futuros con datos pasados.
> - Cómo utilizar `TimeSeriesSplit` y Walk-Forward Validation.
> - Cuándo utilizar MAE, RMSE, MAPE y sMAPE.
> - Cómo construir un pipeline completo para un proyecto de pronóstico.

---

# Conceptos Relacionados

- [[Feature Engineering para Series Temporales]]
- [[EDA para Series Temporales]]
- [[Estacionariedad]]
- [[ARIMA]]
- [[SARIMA]]
- [[Random Forest]]
- [[XGBoost]]
- [[LightGBM]]
- [[Prophet]]
- [[Cross Validation]]