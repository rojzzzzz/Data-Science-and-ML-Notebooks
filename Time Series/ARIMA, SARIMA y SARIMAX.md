

> [!abstract]
> **ARIMA** es uno de los modelos más importantes y utilizados en Series Temporales. Combina los modelos **Autorregresivos (AR)**, **Media Móvil (MA)** y el proceso de **diferenciación (I)** para modelar series no estacionarias. Además, existen extensiones como **SARIMA**, que incorpora estacionalidad, y **SARIMAX**, que permite incluir variables externas.

---

# ¿Por qué nació ARIMA?

En el cuaderno anterior vimos que:

- AR funciona únicamente con series estacionarias.
- MA también requiere estacionariedad.
- ARMA combina ambos, pero sigue teniendo la misma limitación.

Sin embargo, la mayoría de las series reales presentan:

- Tendencias.
- Estacionalidad.
- Cambios en el tiempo.

Necesitábamos un modelo que pudiera trabajar con ellas.

Así nació **ARIMA**.

---

# ¿Qué significa ARIMA?

ARIMA significa:

|Letra|Significado|
|------|-----------|
|AR|AutoRegressive|
|I|Integrated|
|MA|Moving Average|

Cada componente representa una idea diferente.

---

# El componente AR

La parte **Autorregresiva** supone que:

> El presente depende de valores pasados.

Ejemplo

```
Ventas Hoy

↓

Ventas Ayer

↓

Ventas Hace 2 días
```

El parámetro asociado es:

```
p
```

---

# El componente I

La letra **I** significa **Integrated**.

No representa un modelo nuevo.

Representa el número de veces que debemos diferenciar la serie para volverla estacionaria.

Parámetro

```
d
```

---

# El componente MA

La parte **Moving Average Model** supone que el presente depende de errores de predicción anteriores.

Parámetro

```
q
```

---

# Los tres parámetros

Un modelo

```
ARIMA(2,1,3)
```

significa

|Parámetro|Valor|
|----------|----:|
|p|2|
|d|1|
|q|3|

Interpretación

- Utiliza dos retardos.
- Diferencia la serie una vez.
- Utiliza tres errores anteriores.

---

# Flujo de trabajo

```
Serie Original

↓

¿Es estacionaria?

↓

No

↓

Aplicar Diferencias

↓

Serie Estacionaria

↓

Entrenar ARIMA

↓

Pronóstico
```

---

# ¿Cómo elegir p, d y q?

Esta es probablemente la pregunta más importante.

---

## Elegir d

Primero analizamos la estacionariedad.

Utilizamos:

- Inspección visual.
- Prueba Dickey-Fuller.

Si la serie no es estacionaria:

Aplicamos una diferencia.

Generalmente:

```
d = 1
```

es suficiente.

Rara vez se necesita

```
d = 2
```

---

## Elegir p

Observamos el gráfico **PACF**.

Si el PACF presenta un corte claro después del lag 2,

podemos probar

```
p = 2
```

---

## Elegir q

Observamos el gráfico **ACF**.

Si el ACF presenta un corte después del lag 1,

podemos intentar

```
q = 1
```

---

# Regla práctica

|Gráfico|Parámetro|
|--------|----------|
|PACF|p|
|ADF|d|
|ACF|q|

> [!tip]
> Estas reglas son una excelente guía inicial, pero no garantizan encontrar el mejor modelo. Siempre es recomendable comparar varios candidatos utilizando métricas de desempeño.

---

# Entrenar un modelo ARIMA

Con `statsmodels`:

```python
from statsmodels.tsa.arima.model import ARIMA

modelo = ARIMA(
    df["ventas"],
    order=(2,1,1)
)

resultado = modelo.fit()
```

---

# Resumen del modelo

```python
resultado.summary()
```

Obtendremos información como:

- Coeficientes.
- Errores estándar.
- Intervalos de confianza.
- AIC.
- BIC.

---

# Pronósticos

```python
predicciones = resultado.forecast(steps=30)
```

Aquí solicitamos un pronóstico para los próximos 30 períodos.

---

# Visualizar el pronóstico

```python
predicciones.plot()
```

También suele compararse con la serie original para evaluar el ajuste del modelo.

---

# ¿Qué es AIC?

El **Akaike Information Criterion (AIC)** es una métrica para comparar modelos.

No mide directamente la precisión del pronóstico.

Evalúa el equilibrio entre:

- Calidad del ajuste.
- Complejidad del modelo.

Mientras más pequeño sea el AIC,

mejor.

---

# ¿Qué es BIC?

El **Bayesian Information Criterion (BIC)** funciona de manera similar al AIC, pero penaliza con mayor fuerza los modelos demasiado complejos.

También buscamos minimizar su valor.

---

# Limitaciones de ARIMA

Aunque ARIMA es muy potente, presenta algunas limitaciones.

- Modela relaciones lineales.
- No incorpora estacionalidad por sí solo.
- No utiliza variables externas.
- Puede perder precisión en series muy complejas.

Estas limitaciones motivaron la creación de modelos más avanzados.

---

# SARIMA

Muchísimas series presentan estacionalidad.

Ejemplos:

- Ventas navideñas.
- Consumo eléctrico.
- Turismo.
- Temperatura.

ARIMA no modela este comportamiento directamente.

Para ello utilizamos:

```
SARIMA
```

---

# ¿Qué agrega SARIMA?

Además de

```
(p,d,q)
```

incluye un segundo conjunto de parámetros estacionales.

```
(P,D,Q,s)
```

---

# ¿Qué significa s?

Representa el período de la estacionalidad.

Ejemplos

Datos mensuales

```
s = 12
```

---

Datos diarios con patrón semanal

```
s = 7
```

---

Datos horarios con patrón diario

```
s = 24
```

---

# Ejemplo

```
SARIMA

(2,1,1)

×

(1,1,0,12)
```

Interpretación

- Parte no estacional.
- Parte estacional anual.

---

# Entrenar SARIMA

```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

modelo = SARIMAX(
    df["ventas"],
    order=(2,1,1),
    seasonal_order=(1,1,0,12)
)

resultado = modelo.fit()
```

---

# SARIMAX

Ahora imaginemos que las ventas dependen también de:

- Publicidad.
- Precio.
- Temperatura.
- Tipo de cambio.

ARIMA únicamente observa la serie histórica.

Pero nosotros conocemos esas variables.

Entonces utilizamos

```
SARIMAX
```

La X significa:

```
Variables Exógenas
```

---

# ¿Qué son variables exógenas?

Son variables externas que ayudan a explicar el comportamiento de la serie.

Ejemplos

|Serie|Variables Exógenas|
|------|------------------|
|Ventas|Publicidad|
|Consumo eléctrico|Temperatura|
|Demanda aérea|Precio del combustible|
|Hospitales|Casos de influenza|

---

# Entrenar SARIMAX

```python
modelo = SARIMAX(
    endog=df["ventas"],
    exog=df[["publicidad"]],
    order=(2,1,1),
    seasonal_order=(1,1,0,12)
)

resultado = modelo.fit()
```

---

# ¿Cuál modelo elegir?

|Modelo|¿Cuándo usarlo?|
|--------|----------------|
|AR|Serie estacionaria con dependencia temporal simple.|
|MA|Dependencia basada en errores anteriores.|
|ARMA|Serie estacionaria sin tendencia ni estacionalidad.|
|ARIMA|Serie no estacionaria sin estacionalidad importante.|
|SARIMA|Serie con estacionalidad.|
|SARIMAX|Serie con estacionalidad y variables externas.|

---

# Ventajas

> [!success]
>
> - Muy interpretables.
> - Funcionan bien con conjuntos de datos pequeños.
> - Amplio respaldo estadístico.
> - Son una excelente línea base para comparar modelos más complejos.

---

# Limitaciones

> [!warning]
>
> - Requieren una preparación cuidadosa de la serie.
> - Suponen relaciones lineales.
> - Pueden tener dificultades con múltiples patrones complejos o cambios estructurales.
> - En algunos problemas modernos, modelos basados en Machine Learning o Deep Learning pueden ofrecer un mejor desempeño.

---

# Resumen Visual

```
              Serie Temporal

                     │

      ¿Existe Estacionalidad?

             │             │

            No            Sí

             │             │

          ARIMA        SARIMA

                           │

          ¿Variables Externas?

                     │

                    Sí

                     │

                 SARIMAX
```

---

# Resumen

> [!success]
> Después de este cuaderno deberías comprender:
>
> - Qué significa ARIMA.
> - Qué representan los parámetros **p**, **d** y **q**.
> - Cómo utilizar ACF, PACF y ADF para seleccionar parámetros iniciales.
> - Cómo entrenar un modelo ARIMA.
> - Qué representan AIC y BIC.
> - Cuándo utilizar SARIMA.
> - Qué son las variables exógenas.
> - Cuándo utilizar SARIMAX.

---

# Conceptos Relacionados

- [[Estacionariedad]]
- [[Autocorrelación (ACF)]]
- [[Autocorrelación Parcial (PACF)]]
- [[AR]]
- [[MA]]
- [[ARMA]]
- [[Machine Learning para Series Temporales]]