

> [!abstract]
> La **estacionariedad** es uno de los conceptos más importantes de las Series Temporales. La mayoría de los modelos estadísticos clásicos, como **AR**, **MA**, **ARMA** y **ARIMA**, asumen que la serie es estacionaria. Antes de construir cualquiera de estos modelos, debemos comprobar si esta condición se cumple.

---

# ¿Qué significa que una serie sea estacionaria?

Una Serie Temporal es **estacionaria** cuando sus propiedades estadísticas permanecen prácticamente constantes a lo largo del tiempo.

En otras palabras:

- Su promedio no cambia.
- Su varianza permanece aproximadamente constante.
- La relación entre observaciones depende únicamente de la distancia temporal (*lag*) y no del momento en que se midieron.

> [!info]
> Una serie estacionaria puede fluctuar constantemente, pero lo hace alrededor de un comportamiento estable.

---

# Intuición

Supongamos la temperatura promedio diaria de una habitación con aire acondicionado.

```
22°C
23°C
22°C
21°C
22°C
23°C
21°C
```

La temperatura cambia ligeramente, pero siempre oscila alrededor del mismo valor.

Esta serie es aproximadamente estacionaria.

---

Ahora observemos otra serie.

```
100
120
145
180
230
280
340
```

Aquí existe un crecimiento continuo.

Su promedio cambia con el tiempo.

Esta serie **no es estacionaria**.

---

# ¿Por qué es importante?

Los modelos ARIMA intentan aprender patrones observados en el pasado.

Si la distribución cambia constantemente, el pasado deja de ser un buen predictor del futuro.

Por ello, primero buscamos convertir la serie en estacionaria.

---

# Propiedades de una serie estacionaria

## 1. Media constante

```
Media

25
25
25
25
```

No aumenta ni disminuye significativamente.

---

## 2. Varianza constante

Las fluctuaciones mantienen aproximadamente la misma amplitud.

Ejemplo:

```
23
24
25
22
26
24
23
```

No aparecen periodos con mucha mayor dispersión.

---

## 3. Autocorrelación estable

La relación entre dos observaciones depende únicamente del **lag**.

Por ejemplo:

```
Hoy ← Hace 3 días
```

Tiene el mismo comportamiento durante toda la serie.

---

# Ejemplos

## Serie estacionaria

```
102
98
101
99
103
100
97
```

Oscila alrededor del mismo promedio.

---

## Serie no estacionaria por tendencia

```
100
120
145
170
210
250
```

Existe una tendencia creciente.

---

## Serie no estacionaria por estacionalidad

```
100
150
100
150
100
150
```

Aunque el promedio global pueda parecer estable, el patrón periódico rompe la estacionariedad.

---

## Serie no estacionaria por varianza

```
100
101
99
102
...

120
70
180
45
200
```

Las variaciones aumentan con el tiempo.

---

# Tipos de no estacionariedad

Las causas más frecuentes son:

|Causa|Descripción|
|------|-----------|
|Tendencia|El promedio cambia continuamente.|
|Estacionalidad|Existen patrones repetitivos.|
|Varianza creciente|La dispersión aumenta o disminuye.|
|Cambios estructurales|Eventos que modifican el comportamiento de la serie.|

---

# ¿Cómo detectar una serie no estacionaria?

Una primera aproximación consiste en observar el gráfico.

Preguntas útiles:

- ¿Existe una tendencia?
- ¿La amplitud aumenta?
- ¿Se repiten patrones?
- ¿Existen cambios bruscos?

Si la respuesta es sí, probablemente la serie no sea estacionaria.

> [!tip]
> Siempre comienza con una inspección visual antes de aplicar pruebas estadísticas.

---

# Prueba de Dickey-Fuller Aumentada (ADF)

La prueba más utilizada para evaluar estacionariedad es la **Augmented Dickey-Fuller Test (ADF)**.

En Python:

```python
from statsmodels.tsa.stattools import adfuller

resultado = adfuller(df["ventas"])
```

El resultado devuelve varios valores.

```python
resultado
```

---

# El p-value

El valor más importante es:

```python
resultado[1]
```

Corresponde al **p-value**.

---

# Interpretación

Hipótesis nula (**H₀**):

> La serie **no es estacionaria**.

Hipótesis alternativa (**H₁**):

> La serie **es estacionaria**.

---

Regla práctica:

|p-value|Conclusión|
|--------|----------|
|< 0.05|Rechazamos H₀. La serie es estacionaria.|
|≥ 0.05|No podemos rechazar H₀. La serie no es estacionaria.|

---

Ejemplo

```python
p-value = 0.003
```

Conclusión

```
La serie es estacionaria.
```

---

Ejemplo

```python
p-value = 0.48
```

Conclusión

```
La serie no es estacionaria.
```

---

# ¿Qué hacemos si la serie no es estacionaria?

No significa que el modelo no pueda utilizarse.

Simplemente debemos transformar la serie.

Las técnicas más comunes son:

- Diferenciación.
- Eliminación de tendencia.
- Eliminación de estacionalidad.
- Transformaciones matemáticas.

---

# Diferenciación (Differencing)

La técnica más utilizada.

Consiste en calcular la diferencia entre observaciones consecutivas.

En lugar de modelar:

```
Ventas
```

modelamos:

```
Cambio en las ventas
```

---

Ejemplo

Serie original

|Día|Ventas|
|---|------:|
|1|100|
|2|105|
|3|110|
|4|118|

Aplicando una diferencia:

```python
df["ventas_diff"] = df["ventas"].diff()
```

Resultado

|Día|Ventas|Diferencia|
|---|------:|----------:|
|1|100|NaN|
|2|105|5|
|3|110|5|
|4|118|8|

Ahora modelamos las diferencias en lugar de los valores absolutos.

---

# Diferenciación de primer orden

```python
df["ventas"].diff()
```

Calcula:

```
Hoy − Ayer
```

Es la transformación más utilizada en ARIMA.

---

# Diferenciación de segundo orden

Si la primera diferencia no es suficiente:

```python
df["ventas"].diff().diff()
```

o

```python
df["ventas"].diff(2)
```

> [!warning]
> Utilizar demasiadas diferenciaciones puede eliminar información importante y dificultar la interpretación del modelo.

---

# Transformación Logarítmica

Cuando la varianza aumenta con el tiempo, una transformación logarítmica puede estabilizarla.

```python
import numpy as np

df["log_ventas"] = np.log(df["ventas"])
```

Muy utilizada en:

- Finanzas.
- Economía.
- Ventas.
- Demanda.

---

# ¿Cuándo volver a ejecutar el ADF?

Después de transformar la serie debemos repetir la prueba.

Proceso típico:

```
Serie Original

↓

ADF

↓

¿No es estacionaria?

↓

Aplicar diferencias

↓

ADF nuevamente

↓

Construir el modelo
```

---

# Relación con ARIMA

La letra **I** en **ARIMA** significa:

```
Integrated
```

Hace referencia precisamente al número de diferenciaciones necesarias para convertir la serie en estacionaria.

Ese número corresponde al parámetro:

```
d
```

Por ejemplo:

```
ARIMA(2,1,1)
```

Significa:

- p = 2
- d = 1
- q = 1

La serie fue diferenciada una vez antes de entrenar el modelo.

---

# Buenas Prácticas

> [!success]
>
> - Siempre inspecciona visualmente la serie.
> - Aplica la prueba ADF antes de entrenar modelos ARIMA.
> - No diferencies más veces de las necesarias.
> - Verifica nuevamente la estacionariedad después de cada transformación.
> - Considera transformaciones logarítmicas cuando la varianza aumente con el tiempo.

---

# Resumen

> [!success]
> Después de este cuaderno deberías comprender:
>
> - Qué es la estacionariedad.
> - Por qué es necesaria para ARIMA.
> - Cómo reconocer una serie no estacionaria.
> - Cómo interpretar la prueba Dickey-Fuller.
> - Qué significa el p-value.
> - Cómo aplicar diferenciación.
> - Qué representa el parámetro **d** de ARIMA.

---

# Conceptos Relacionados

- [[EDA para Series Temporales]]
- [[Autocorrelación (ACF)]]
- [[Autocorrelación Parcial (PACF)]]
- [[AR]]
- [[MA]]
- [[ARMA]]
- [[ARIMA]]
- [[SARIMA]]