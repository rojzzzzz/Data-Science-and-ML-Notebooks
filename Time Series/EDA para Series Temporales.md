

> [!abstract]
> El **Análisis Exploratorio de Datos (EDA)** para Series Temporales tiene como objetivo comprender cómo evoluciona una variable a lo largo del tiempo antes de construir cualquier modelo predictivo. En esta etapa buscamos identificar tendencias, estacionalidad, ciclos, anomalías, ruido y relaciones temporales que nos permitan elegir el modelo adecuado.

---

# ¿Por qué es diferente el EDA en Series Temporales?

En un dataset tradicional solemos preguntarnos:

- ¿Cuál es la distribución?
- ¿Existen valores atípicos?
- ¿Hay correlaciones?

En una Serie de Tiempo las preguntas cambian.

Ahora queremos saber:

- ¿La variable aumenta o disminuye con el tiempo?
- ¿Existe un patrón repetitivo?
- ¿Qué tan predecible es la serie?
- ¿Existen cambios bruscos?
- ¿Hay eventos anormales?

El tiempo se convierte en el eje principal del análisis.

---

# El gráfico más importante

Antes de calcular cualquier estadística, debemos **graficar la serie**.

```python
plt.figure(figsize=(12,5))

plt.plot(df.index, df["ventas"])

plt.title("Ventas Diarias")
plt.xlabel("Fecha")
plt.ylabel("Ventas")
```

> [!tip]
> En Series Temporales, un gráfico suele revelar más información que varias tablas estadísticas.

---

# ¿Qué debemos observar?

Al visualizar la serie debemos intentar responder:

- ¿Existe una tendencia?
- ¿Se observan patrones repetitivos?
- ¿La variabilidad cambia con el tiempo?
- ¿Hay valores atípicos?
- ¿Existen datos faltantes?
- ¿Hay cambios repentinos?

---

# Tendencia (Trend)

La tendencia representa el comportamiento general de largo plazo.

Ejemplo:

```
Ventas

250 ┤                    ●
230 ┤                 ●
210 ┤              ●
190 ┤           ●
170 ┤        ●
150 ┤     ●
130 ┤  ●
    └────────────────────────── Tiempo
```

Aquí observamos un crecimiento constante.

---

## Ejemplos de tendencia

### Tendencia creciente

- Crecimiento poblacional.
- Ventas de una empresa en expansión.

---

### Tendencia decreciente

- Usuarios de una tecnología obsoleta.
- Producción de un pozo petrolero.

---

### Sin tendencia

La serie oscila alrededor de un promedio constante.

---

# Estacionalidad (Seasonality)

La estacionalidad corresponde a patrones que se repiten periódicamente.

Ejemplo:

```
Ventas

200 ┤     /\      /\      /\
180 ┤    /  \    /  \    /  \
160 ┤___/____\__/____\__/____\____ Tiempo
```

El patrón se repite continuamente.

---

## Ejemplos reales

- Mayor venta de juguetes en diciembre.
- Más consumo eléctrico durante el verano.
- Más turistas en vacaciones.
- Mayor tráfico los lunes por la mañana.

---

# Ciclos

Los ciclos son parecidos a la estacionalidad, pero no tienen una duración fija.

Ejemplo:

```
Economía

      /\

     /  \

___ /    \______

        Tiempo
```

Puede durar varios años.

Ejemplos:

- Ciclos económicos.
- Mercados inmobiliarios.
- Precios internacionales.

---

# Ruido

El ruido representa las variaciones completamente aleatorias.

```
─────────────≈≈≈≈≈≈≈≈≈────────────
```

Proviene de:

- Errores de medición.
- Eventos inesperados.
- Variaciones naturales.

Todo modelo intenta explicar la tendencia y la estacionalidad dejando únicamente el ruido.

---

# Valores Atípicos (Outliers)

Una observación muy diferente del resto puede indicar:

- Error de captura.
- Promoción especial.
- Falla técnica.
- Evento extraordinario.

Ejemplo

```
100
102
98
101
5400   ← Outlier
99
103
```

Los outliers deben analizarse antes de eliminarlos.

---

# Datos faltantes

También debemos verificar si existen fechas ausentes.

Ejemplo

```
01/01

02/01

05/01
```

Faltan

```
03/01

04/01
```

Esto puede afectar muchos modelos.

---

# Variabilidad

No todas las series mantienen la misma dispersión.

Ejemplo

```
Inicio

100
102
101

Final

100
150
60
180
```

Aquí la variabilidad aumenta con el tiempo.

Este comportamiento puede requerir transformaciones antes del modelado.

---

# Descomposición de Series Temporales

Una de las herramientas más útiles consiste en separar la serie en sus componentes.

Una serie puede representarse como:

```
Serie = Tendencia + Estacionalidad + Residuales
```

o

```
Serie = Tendencia × Estacionalidad × Residuales
```

dependiendo del tipo de modelo.

---

# STL Decomposition

Una técnica muy utilizada es **STL (Seasonal and Trend decomposition using Loess).**

Permite separar automáticamente:

- Tendencia
- Estacionalidad
- Residuales

En Python:

```python
from statsmodels.tsa.seasonal import STL

stl = STL(df["ventas"])

resultado = stl.fit()

resultado.plot()
```

Obtendremos normalmente cuatro gráficos.

---

## 1. Serie Original

Los datos completos.

---

## 2. Tendencia

El comportamiento de largo plazo.

---

## 3. Estacionalidad

El patrón repetitivo.

---

## 4. Residuales

La información que el modelo no pudo explicar.

Idealmente debería parecer ruido aleatorio.

---

# ¿Por qué es tan útil la descomposición?

Nos permite responder preguntas como:

- ¿La tendencia es fuerte?
- ¿Existe realmente estacionalidad?
- ¿El modelo dejó mucho ruido?
- ¿Qué componente domina la serie?

---

# Autocorrelación

Una pregunta muy importante.

> ¿El valor de hoy depende del valor de ayer?

Si la respuesta es sí,

existe **autocorrelación**.

Es uno de los conceptos fundamentales en Series Temporales.

Ejemplo:

Temperatura.

Si hoy hacen 30°C,

es muy probable que mañana haga una temperatura parecida.

---

# Correlación vs Autocorrelación

Correlación

Relaciona dos variables diferentes.

```
Ventas

Publicidad
```

Autocorrelación

Relaciona la misma variable consigo misma en distintos momentos del tiempo.

```
Ventas Hoy

↓

Ventas Ayer
```

---

# Función de Autocorrelación (ACF)

La **Autocorrelation Function (ACF)** mide la correlación entre una serie y versiones desplazadas de sí misma.

Por ejemplo:

- Hoy vs ayer.
- Hoy vs hace dos días.
- Hoy vs hace siete días.

En Python

```python
from statsmodels.graphics.tsaplots import plot_acf

plot_acf(df["ventas"])
```

---

# ¿Cómo interpretar el gráfico ACF?

Si aparecen barras altas en los primeros retardos (*lags*),

significa que la serie posee memoria.

Es decir,

el pasado ayuda a explicar el futuro.

---

# ¿Qué es un Lag?

Un **Lag** es un desplazamiento temporal.

Ejemplo:

Lag 1

```
Hoy ← Ayer
```

Lag 7

```
Hoy ← Hace una semana
```

Lag 30

```
Hoy ← Hace un mes
```

---

# ¿Para qué sirve el ACF?

El ACF ayuda a:

- Detectar dependencia temporal.
- Identificar estacionalidad.
- Construir modelos ARIMA.
- Elegir parámetros del modelo.

---

# Buenas Prácticas

> [!success]
>
> Antes de construir cualquier modelo:
>
> - Grafica la serie.
> - Busca tendencia.
> - Busca estacionalidad.
> - Identifica datos faltantes.
> - Analiza valores atípicos.
> - Calcula la autocorrelación.
> - Realiza una descomposición STL.

---

# Resumen

> [!success]
>
> Después de este cuaderno deberías comprender:
>
> - Cómo explorar una Serie Temporal.
> - Qué es una tendencia.
> - Qué es la estacionalidad.
> - Qué son los ciclos.
> - Qué representa el ruido.
> - Cómo detectar valores atípicos.
> - Qué es una descomposición STL.
> - Qué significa la autocorrelación.
> - Qué es un Lag.
> - Para qué sirve el gráfico ACF.

---

# Conceptos Relacionados

- [[Introducción a las Series de Tiempo]]
- [[Manipulación de Fechas con Pandas]]
- [[Feature Engineering para Series Temporales]]
- [[Autocorrelación Parcial (PACF)]]
- [[Estacionariedad]]
- [[AR]]
- [[MA]]
- [[ARIMA]]