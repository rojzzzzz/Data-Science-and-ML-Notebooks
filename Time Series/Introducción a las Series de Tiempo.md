

> [!abstract]
> Una **Serie de Tiempo (Time Series)** es una secuencia de observaciones registradas a lo largo del tiempo, donde **el orden cronológico de los datos es fundamental**. A diferencia de un dataset tradicional, el tiempo introduce dependencia entre las observaciones, permitiendo estudiar patrones y realizar predicciones futuras.

---

# ¿Qué es una Serie de Tiempo?

Una **Serie de Tiempo** es un conjunto de observaciones medidas en distintos instantes de tiempo.

Cada observación está asociada a una fecha o una hora, y **el orden en que ocurren los datos contiene información importante**.

Por ejemplo:

| Fecha | Ventas |
|--------|--------:|
| 2026-01-01 | 125 |
| 2026-01-02 | 130 |
| 2026-01-03 | 128 |
| 2026-01-04 | 145 |

En este caso, no solamente importan los valores de las ventas, sino también **el momento en que ocurrieron**.

---

# ¿Por qué el tiempo es tan importante?

En la mayoría de los problemas de Machine Learning tradicional, las observaciones son independientes entre sí.

Por ejemplo:

| Edad | Ingreso | Compró |
|------:|---------:|:-------:|
|25|3000|Sí|
|40|7000|No|
|32|5000|Sí|

Podemos reorganizar estas filas de cualquier forma y el dataset seguirá representando exactamente la misma información.

Por ejemplo:

| Edad | Ingreso | Compró |
|------:|---------:|:-------:|
|32|5000|Sí|
|25|3000|Sí|
|40|7000|No|

El modelo obtendrá exactamente el mismo conocimiento.

En una Serie de Tiempo esto **no es cierto**.

Supongamos la siguiente serie:

| Fecha | Temperatura |
|--------|------------:|
|08:00|18°C|
|09:00|20°C|
|10:00|23°C|

Si cambiamos el orden:

| Fecha | Temperatura |
|--------|------------:|
|10:00|23°C|
|08:00|18°C|
|09:00|20°C|

La información pierde completamente su significado.

> [!warning]
> En una Serie de Tiempo **nunca debe alterarse el orden cronológico de las observaciones**, ya que el tiempo forma parte de la información del problema.

---

# ¿Por qué son importantes las Series de Tiempo?

Las Series de Tiempo aparecen prácticamente en cualquier industria.

Algunos ejemplos son:

- 📈 Precio de acciones.
- 💰 Ventas diarias.
- 🌡 Temperatura.
- ⚡ Consumo eléctrico.
- 🏥 Número de pacientes.
- 🌐 Tráfico de un sitio web.
- 🚚 Pedidos de una empresa.
- 📡 Sensores IoT.
- 💳 Transacciones bancarias.

Siempre que exista una variable **Fecha** o **Hora**, es posible que estemos frente a un problema de Series de Tiempo.

> [!tip]
> Una buena regla práctica es preguntarse:
>
> **¿El momento en que ocurrió cada observación afecta el análisis?**
>
> Si la respuesta es sí, probablemente se trate de una Serie de Tiempo.

---

# Objetivos del análisis de Series de Tiempo

Dependiendo del problema, el análisis de Series Temporales busca responder preguntas como:

- ¿La variable está creciendo o disminuyendo?
- ¿Existen patrones repetitivos?
- ¿Qué ocurrirá en el futuro?
- ¿Hubo algún comportamiento anormal?
- ¿Cómo ha cambiado la variable con el tiempo?

Por ejemplo:

| Pregunta | Ejemplo |
|----------|----------|
| Forecasting | ¿Cuántas ventas habrá el próximo mes? |
| Tendencia | ¿Las ventas están aumentando? |
| Estacionalidad | ¿Siempre se vende más en diciembre? |
| Detección de anomalías | ¿Hubo un día con ventas inusualmente altas? |

---

# Componentes de una Serie de Tiempo

La mayoría de las Series de Tiempo pueden entenderse como la combinación de cuatro componentes.

## 1. Tendencia (Trend)

La **tendencia** representa el comportamiento de largo plazo de la variable.

Puede ser:

- Creciente 📈
- Decreciente 📉
- Estable ➡️

Ejemplo:

Las ventas de una empresa aumentan lentamente año tras año debido al crecimiento del negocio.

---

## 2. Estacionalidad (Seasonality)

La **estacionalidad** corresponde a patrones que se repiten periódicamente.

Ejemplos:

- Más ventas en diciembre.
- Mayor consumo eléctrico durante el verano.
- Incremento del turismo en vacaciones.
- Más enfermedades respiratorias en invierno.

La característica principal es que el patrón ocurre aproximadamente cada cierto intervalo fijo de tiempo.

Puede repetirse:

- Cada día.
- Cada semana.
- Cada mes.
- Cada trimestre.
- Cada año.

---

## 3. Ciclos (Cycles)

Los ciclos también representan movimientos de largo plazo, pero **no tienen una periodicidad fija**.

Por ejemplo:

- Crisis económicas.
- Ciclos inmobiliarios.
- Ciclos del mercado financiero.

Un ciclo puede durar tres años, cinco años o diez años.

A diferencia de la estacionalidad, **no existe un período constante**.

---

## 4. Ruido (Noise)

El ruido representa la variación completamente aleatoria de la serie.

Corresponde a eventos inesperados como:

- Desastres naturales.
- Errores de medición.
- Promociones inesperadas.
- Pandemias.
- Fallas técnicas.

El ruido siempre estará presente y ningún modelo puede eliminarlo completamente.

> [!info]
> El objetivo de muchos modelos de Series Temporales consiste en explicar la tendencia y la estacionalidad, dejando únicamente el ruido como parte impredecible.

---

# Frecuencia de una Serie Temporal

La **frecuencia** indica cada cuánto tiempo se registra una observación.

| Frecuencia | Ejemplo |
|------------|----------|
| Segundos | Sensores industriales |
| Minutos | Precio de criptomonedas |
| Horas | Temperatura |
| Diaria | Ventas |
| Semanal | Pedidos |
| Mensual | Facturación |
| Trimestral | Resultados financieros |
| Anual | PIB de un país |

La frecuencia determina qué tipo de patrones podremos detectar.

Por ejemplo, una serie anual nunca podrá mostrar una estacionalidad semanal.

---

# Series Equiespaciadas

Muchos modelos estadísticos clásicos, como **ARIMA**, asumen que las observaciones están separadas por intervalos constantes.

Ejemplo correcto:

```text
01/01
02/01
03/01
04/01
05/01
```

Cada observación está separada exactamente por un día.

Ejemplo irregular:

```text
01/01
04/01
12/01
25/01
```

Aquí los intervalos son diferentes.

Antes de aplicar muchos modelos será necesario:

- Remuestrear (*Resampling*).
- Interpolar datos.
- Agregar observaciones.
- Cambiar la frecuencia temporal.

Estos temas se estudiarán más adelante.

---

# Ideas Clave

> [!success]
> - Una Serie de Tiempo es una secuencia ordenada cronológicamente.
> - El orden temporal contiene información.
> - No se deben mezclar aleatoriamente las observaciones.
> - Las Series de Tiempo suelen componerse de tendencia, estacionalidad, ciclos y ruido.
> - La frecuencia determina el tipo de análisis que puede realizarse.
> - Muchos modelos requieren observaciones equiespaciadas.

---

# Conceptos Relacionados

- [[Datetime en Python]]
- [[Manipulación de Fechas con Pandas]]
- [[Feature Engineering para Series Temporales]]
- [[EDA para Series Temporales]]
- [[Autocorrelación]]
- [[ARIMA]]
- [[SARIMA]]
- [[LSTM]]

---

# Próximo Cuaderno

➡️ [[Datetime en Python]]

En el siguiente cuaderno aprenderemos a trabajar con fechas y horas utilizando el módulo **datetime**, incluyendo:

- Crear fechas.
- Convertir texto a fechas (`strptime`).
- Formatear fechas (`strftime`).
- Operaciones entre fechas.
- `timedelta`.
- Zonas horarias.
- Equivalencias con Pandas y SQL.
- Los formatos más importantes (`%Y`, `%m`, `%d`, `%H`, `%M`, etc.).