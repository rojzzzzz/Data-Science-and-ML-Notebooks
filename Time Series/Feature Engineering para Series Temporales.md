

> [!abstract]
> El **Feature Engineering** consiste en crear nuevas variables (*features*) a partir de los datos existentes con el objetivo de mejorar el rendimiento de un modelo de Machine Learning. En Series de Tiempo, la información temporal es una fuente muy rica de nuevas variables, ya que muchos fenómenos presentan patrones diarios, semanales, mensuales o estacionales.

---

# ¿Qué es el Feature Engineering?

En Machine Learning, los modelos aprenden únicamente de las variables que reciben como entrada.

Por ejemplo, si queremos predecir las ventas y únicamente proporcionamos la fecha:

| Fecha | Ventas |
|--------|--------:|
|2026-01-01|120|
|2026-01-02|140|

Para un algoritmo, la fecha completa puede ser difícil de interpretar.

En cambio, si extraemos información adicional:

| Fecha | Año | Mes | Día | Día Semana | Ventas |
|--------|----:|----:|----:|------------:|--------:|
|2026-01-01|2026|1|1|3|120|
|2026-01-02|2026|1|2|4|140|

El modelo puede descubrir relaciones mucho más fácilmente.

> [!tip]
> El objetivo del Feature Engineering no es crear muchas variables, sino crear variables que aporten información útil al modelo.

---

# ¿Por qué funciona?

Muchos eventos dependen del tiempo.

Por ejemplo:

- Las ventas aumentan los fines de semana.
- El consumo eléctrico aumenta en verano.
- Los hoteles reciben más turistas durante vacaciones.
- Las llamadas a un call center disminuyen durante la madrugada.

Estas relaciones no están escritas explícitamente en la fecha.

Debemos extraerlas.

---

# Componentes que podemos extraer

Una fecha contiene mucha más información de la que parece.

A partir de una sola columna podemos obtener:

|Variable|Ejemplo|
|---------|--------|
|Año|2026|
|Mes|7|
|Trimestre|3|
|Semana del año|29|
|Día|17|
|Día de la semana|Viernes|
|Fin de semana|Sí/No|
|Hora|15|
|Minuto|35|
|Segundo|42|

Cada una puede convertirse en una nueva variable para el modelo.

---

# Extraer el año

```python
df["year"] = df["fecha"].dt.year
```

Útil cuando existen tendencias de largo plazo.

Ejemplo:

- Inflación.
- Crecimiento poblacional.
- Incremento de ventas anual.

---

# Extraer el mes

```python
df["month"] = df["fecha"].dt.month
```

Permite detectar patrones estacionales.

Ejemplos:

- Más ventas en diciembre.
- Mayor consumo eléctrico en verano.
- Mayor turismo en julio.

---

# Extraer el trimestre

```python
df["quarter"] = df["fecha"].dt.quarter
```

Muy utilizado en:

- Finanzas.
- Contabilidad.
- Indicadores económicos.

---

# Extraer el día del mes

```python
df["day"] = df["fecha"].dt.day
```

Puede ser útil cuando existen comportamientos asociados a fechas específicas.

Ejemplos:

- Pago de salarios.
- Cierre de mes.
- Facturación.

---

# Día de la semana

```python
df["weekday"] = df["fecha"].dt.dayofweek
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

Muchas veces resulta más útil almacenar directamente el nombre.

```python
df["weekday_name"] = df["fecha"].dt.day_name()
```

---

# ¿Es fin de semana?

Una variable extremadamente utilizada.

```python
df["is_weekend"] = df["fecha"].dt.dayofweek >= 5
```

Resultado

|Fecha|is_weekend|
|------|-----------|
|Lunes|False|
|Sábado|True|

Muy útil para:

- Retail
- Restaurantes
- Transporte
- Turismo

---

# Extraer la hora

```python
df["hour"] = df["fecha"].dt.hour
```

Es especialmente útil cuando los datos tienen frecuencia horaria.

Ejemplo:

- Consumo eléctrico.
- Sensores.
- Tráfico web.
- Llamadas telefónicas.

---

# Semana del año

```python
df["week"] = df["fecha"].dt.isocalendar().week
```

Útil para:

- Pronósticos semanales.
- Reportes ejecutivos.
- Planeación logística.

---

# Variables rezagadas (Lag Features)

Las variables históricas también forman parte del Feature Engineering.

```python
df["lag_1"] = df["ventas"].shift(1)

df["lag_7"] = df["ventas"].shift(7)
```

El modelo aprende utilizando información del pasado.

---

# Medias móviles

También pueden utilizarse como variables.

```python
df["rolling_7"] = df["ventas"].rolling(7).mean()
```

Ahora el modelo conoce el promedio de la última semana.

---

# Encoding temporal

Una duda muy común es:

> ¿Por qué no utilizar simplemente el número del mes?

Supongamos

```
Enero = 1

Febrero = 2

...

Diciembre = 12
```

Para el modelo:

```
12
```

está muy lejos de

```
1
```

Pero sabemos que diciembre y enero son meses consecutivos.

Aquí aparece uno de los problemas más importantes del Feature Engineering Temporal.

---

# Variables cíclicas

Muchas variables temporales son **cíclicas**.

Ejemplos:

- Hora del día.
- Día de la semana.
- Mes.
- Dirección del viento.
- Ángulo.

Cuando termina un ciclo vuelve a comenzar.

Por ejemplo

```
23:00

↓

00:00
```

La distancia real entre ambas horas es únicamente una hora.

Pero numéricamente:

```
23

↓

0
```

parecen muy alejadas.

---

# ¿Cómo solucionamos este problema?

Utilizando funciones trigonométricas.

En lugar de representar una hora mediante un número, la representamos mediante un punto sobre un círculo.

Se crean dos variables.

```python
sin(...)
```

y

```python
cos(...)
```

---

# Ejemplo

Supongamos las horas

```
0

6

12

18
```

Con Encoding Cíclico obtenemos

|Hora|sin|cos|
|----:|---:|---:|
|0|0|1|
|6|1|0|
|12|0|-1|
|18|-1|0|

Ahora:

- 23:00
- 00:00

quedan muy próximos entre sí.

El modelo entiende correctamente la naturaleza circular del tiempo.

---

# Implementación

Para las horas del día:

```python
import numpy as np

df["hour_sin"] = np.sin(
    2 * np.pi * df["hour"] / 24
)

df["hour_cos"] = np.cos(
    2 * np.pi * df["hour"] / 24
)
```

Para los meses:

```python
df["month_sin"] = np.sin(
    2*np.pi*df["month"]/12
)

df["month_cos"] = np.cos(
    2*np.pi*df["month"]/12
)
```

> [!info]
> Aunque la fórmula pueda parecer compleja, la idea es sencilla: **transformar una variable lineal en una representación circular**, preservando la cercanía entre el inicio y el final del ciclo.

---

# ¿Cuándo utilizar Encoding Cíclico?

Es recomendable cuando la variable representa un ciclo completo.

Ejemplos:

- Hora del día.
- Día de la semana.
- Mes del año.
- Minuto.
- Segundo.
- Ángulos.

No suele ser necesario para variables como:

- Año.
- Edad.
- Número de clientes.

Estas variables no son cíclicas.

---

# Buenas Prácticas

> [!success]
>
> - Extrae únicamente variables que puedan aportar información.
> - Evita crear decenas de variables innecesarias.
> - Aprovecha las variables históricas (`shift`).
> - Utiliza medias móviles cuando exista mucho ruido.
> - Usa Encoding Cíclico para variables temporales periódicas.

---

# Resumen

> [!success]
>
> Después de este cuaderno deberías comprender:
>
> - Qué es el Feature Engineering.
> - Cómo extraer componentes de una fecha.
> - Cómo crear variables históricas.
> - Cómo crear medias móviles.
> - Por qué existen las variables cíclicas.
> - Cuándo utilizar `sin` y `cos`.
> - Cómo mejorar la información temporal disponible para un modelo.

---

# Conceptos Relacionados

- [[Datetime en Python]]
- [[Manipulación de Fechas con Pandas]]
- [[Rolling Window]]
- [[Shift]]
- [[Moving Average]]
- [[EDA para Series Temporales]]
- [[Autocorrelación]]
- [[ARIMA]]