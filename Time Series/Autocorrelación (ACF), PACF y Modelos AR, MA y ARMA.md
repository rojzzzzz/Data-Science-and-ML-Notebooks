

> [!abstract]
> La mayoría de las Series Temporales poseen **memoria**: el pasado influye sobre el futuro. Este cuaderno estudia cómo medir esa dependencia mediante la **Autocorrelación (ACF)** y la **Autocorrelación Parcial (PACF)**, y cómo estos conceptos dieron origen a los modelos **Autorregresivos (AR)**, **Media Móvil (MA)** y **ARMA**, que constituyen la base de los modelos ARIMA.

---

# La idea fundamental

Imagina que deseas predecir las ventas de mañana.

¿Qué información utilizarías?

Probablemente responderías:

- Las ventas de hoy.
- Las ventas de ayer.
- Las ventas de la semana pasada.

Es decir, utilizarías el **pasado** para explicar el **futuro**.

Eso precisamente hacen los modelos clásicos de Series Temporales.

---

# ¿Qué significa que una serie tenga memoria?

Supongamos las temperaturas máximas.

|Día|Temperatura|
|---|----------:|
|Lunes|30°C|
|Martes|31°C|
|Miércoles|30°C|
|Jueves|29°C|

Si hoy hacen 30°C,

es bastante probable que mañana la temperatura sea similar.

Existe dependencia temporal.

Ahora pensemos en los resultados de una lotería.

El número ganador de hoy no depende del de ayer.

No existe memoria.

---

# ¿Qué es un Lag?

Un **Lag** representa un desplazamiento temporal hacia el pasado.

Ejemplos

```
Lag 1

Hoy ← Ayer
```

---

```
Lag 2

Hoy ← Hace dos días
```

---

```
Lag 7

Hoy ← Hace una semana
```

---

```
Lag 30

Hoy ← Hace un mes
```

Los modelos estadísticos analizan cuánto aporta cada uno de estos retardos para explicar la serie.

---

# Autocorrelación (ACF)

La **Autocorrelation Function (ACF)** mide cuánto se parece una serie a versiones desplazadas de sí misma.

En otras palabras,

responde preguntas como:

- ¿Qué tan parecidos son hoy y ayer?
- ¿Y hoy respecto a hace una semana?
- ¿Y respecto a hace un mes?

---

# Intuición

Supongamos una serie de temperaturas.

```
30
31
30
29
30
31
30
```

Cada día es parecido al anterior.

La autocorrelación será alta.

---

Ahora observemos una serie aleatoria.

```
15
82
41
7
95
38
64
```

No existe ninguna relación.

La autocorrelación será cercana a cero.

---

# ¿Cómo se calcula?

Conceptualmente, la autocorrelación compara una serie con una copia desplazada.

Ejemplo:

Serie original

```
100
120
110
130
125
```

Serie desplazada un día

```
NaN
100
120
110
130
```

Después calcula la correlación entre ambas.

No es necesario memorizar la fórmula; lo importante es comprender su interpretación.

---

# Gráfico ACF

En Python

```python
from statsmodels.graphics.tsaplots import plot_acf

plot_acf(df["ventas"])
```

Obtendremos un gráfico similar a este:

```
1.0 ┤█

0.8 ┤█

0.6 ┤█

0.4 ┤█

0.2 ┤▌

0.0 └────────────────────────

      1 2 3 4 5 6 7 Lags
```

Cada barra representa la autocorrelación para un lag específico.

---

# ¿Cómo interpretar el ACF?

## Barras altas

Indican que el pasado sigue influyendo.

La serie posee memoria.

---

## Barras pequeñas

La influencia desaparece rápidamente.

La serie tiene poca memoria.

---

## Picos repetitivos

Suelen indicar estacionalidad.

Ejemplo:

```
Lag

7

14

21

28
```

Podría existir un patrón semanal.

---

# ¿Qué es PACF?

La **Partial Autocorrelation Function (PACF)** responde una pregunta ligeramente diferente.

En lugar de preguntar:

> ¿Qué relación existe entre hoy y hace tres días?

pregunta:

> ¿Qué relación directa existe entre hoy y hace tres días, eliminando el efecto de los días intermedios?

---

# Una analogía

Supongamos una cadena de personas.

```
Ana

↓

Luis

↓

María

↓

Carlos
```

Existe una relación entre Ana y Carlos.

Pero realmente ocurre porque:

Ana influye sobre Luis,

Luis sobre María,

María sobre Carlos.

PACF elimina esos efectos intermedios y mide únicamente la relación directa.

---

# ACF vs PACF

|ACF|PACF|
|---|----|
|Mide la relación total.|Mide únicamente la relación directa.|
|Incluye efectos indirectos.|Elimina efectos indirectos.|
|Útil para modelos MA.|Útil para modelos AR.|

---

# Gráfico PACF

```python
from statsmodels.graphics.tsaplots import plot_pacf

plot_pacf(df["ventas"])
```

Su aspecto es muy parecido al del ACF.

La diferencia está en la interpretación.

---

# ¿Por qué necesitamos ambos?

Porque ayudan a identificar qué modelo describe mejor la serie.

El comportamiento típico es:

|Modelo|ACF|PACF|
|-------|---|----|
|AR|Disminuye gradualmente.|Se corta rápidamente.|
|MA|Se corta rápidamente.|Disminuye gradualmente.|
|ARMA|Ambos disminuyen gradualmente.|

Este patrón es una guía práctica, no una regla absoluta.

---

# Modelo Autorregresivo (AR)

La idea del modelo AR es muy sencilla.

> El presente depende del pasado.

Ejemplo

```
Ventas Hoy

↓

Ventas Ayer

↓

Ventas Hace 2 días
```

El modelo aprende cuánto pesa cada observación anterior.

---

## AR(1)

Solo utiliza un retardo.

```
Hoy

↓

Ayer
```

---

## AR(2)

Utiliza dos retardos.

```
Hoy

↓

Ayer

↓

Hace dos días
```

---

## AR(p)

Generalización.

```
Hoy

↓

Últimos p retardos
```

El parámetro **p** representa el número de retardos utilizados.

---

# ¿Cuándo funciona bien un AR?

Cuando el pasado contiene suficiente información para explicar el futuro.

Ejemplos:

- Temperatura.
- Consumo eléctrico.
- Tráfico vehicular.
- Producción industrial.

---

# Modelo de Media Móvil (MA)

Aunque el nombre pueda confundir, **MA (Moving Average)** no es la media móvil que vimos en Pandas.

> [!warning]
> **MA (Moving Average Model)** y **Moving Average** son conceptos distintos.

La media móvil de Pandas es una herramienta de suavizado.

El modelo **MA** utiliza los **errores de predicción anteriores**.

---

# Intuición

Supongamos que ayer nuestro modelo se equivocó.

Ese error puede contener información útil.

El modelo MA aprende precisamente de esos errores.

En lugar de decir:

```
Hoy depende de ayer.
```

dice:

```
Hoy depende de los errores cometidos anteriormente.
```

---

## MA(1)

Utiliza únicamente el error anterior.

---

## MA(2)

Utiliza los dos errores anteriores.

---

## MA(q)

Utiliza los últimos **q** errores.

---

# Modelo ARMA

Ahora combinamos ambas ideas.

```
Hoy

↓

Valores pasados

+

Errores pasados
```

ARMA integra:

- Información histórica.
- Errores históricos.

Por eso suele modelar mejor muchas Series Temporales estacionarias.

---

# ¿Cuándo utilizar ARMA?

Cuando la serie:

- Es estacionaria.
- No presenta tendencia.
- No presenta estacionalidad importante.

Si la serie no es estacionaria,

ARMA deja de ser suficiente.

Aquí aparece ARIMA.

---

# Limitaciones

Los modelos AR, MA y ARMA presentan varias limitaciones.

- Requieren estacionariedad.
- Manejan mal tendencias fuertes.
- No modelan estacionalidad.
- Suponen relaciones lineales.
- Pueden perder precisión en series muy complejas.

Estas limitaciones motivaron el desarrollo de **ARIMA** y posteriormente **SARIMA**.

---

# Resumen Visual

```
               Serie Temporal

                     │

          ¿Existe memoria temporal?

                     │

                 Sí │ No

                     │

         Analizar ACF y PACF

                     │

      ┌──────────────┼──────────────┐

      │              │              │

     AR             MA            ARMA

 Valores        Errores        Valores +

 Pasados        Pasados        Errores
```

---

# Buenas Prácticas

> [!success]
>
> - Analiza primero la estacionariedad.
> - Observa siempre los gráficos ACF y PACF antes de elegir un modelo.
> - No confundas el modelo MA con la media móvil utilizada para suavizar datos.
> - Utiliza ARMA únicamente con series estacionarias.
> - Recuerda que ACF y PACF sirven como herramientas de diagnóstico, no como reglas absolutas.

---

# Resumen

> [!success]
> Después de este cuaderno deberías comprender:
>
> - Qué significa que una serie tenga memoria.
> - Qué es un lag.
> - Qué mide la Autocorrelación (ACF).
> - Qué mide la Autocorrelación Parcial (PACF).
> - Cómo interpretar ambos gráficos.
> - Qué representa un modelo AR.
> - Qué representa un modelo MA.
> - Cómo funciona un modelo ARMA.
> - Cuándo utilizar cada uno.

---

# Conceptos Relacionados

- [[Estacionariedad]]
- [[ARIMA]]
- [[SARIMA]]
- [[SARIMAX]]
- [[Feature Engineering para Series Temporales]]
- [[Rolling Window]]
- [[Autocorrelación]]