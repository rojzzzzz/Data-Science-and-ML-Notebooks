

> [!abstract]
> En esta segunda parte aprenderemos a realizar operaciones con fechas, calcular diferencias de tiempo, comparar fechas, trabajar con zonas horarias, timestamps Unix y conocer otros objetos importantes del módulo `datetime`.

---

# timedelta

El objeto `timedelta` representa una **duración** o diferencia entre dos fechas.

No representa una fecha específica, sino un intervalo de tiempo.

Se utiliza para:

- Sumar días.
- Restar días.
- Agregar horas.
- Agregar semanas.
- Calcular diferencias entre fechas.

---

## Crear un timedelta

```python
from datetime import timedelta

delta = timedelta(days=5)

print(delta)
```

Resultado

```text
5 days, 0:00:00
```

También podemos indicar otras unidades.

```python
timedelta(
    weeks=2,
    days=3,
    hours=5,
    minutes=30,
    seconds=10
)
```

Parámetros disponibles:

- weeks
- days
- hours
- minutes
- seconds
- milliseconds
- microseconds

---

# Sumar tiempo

```python
from datetime import datetime, timedelta

fecha = datetime(2026,7,17)

nueva_fecha = fecha + timedelta(days=10)

print(nueva_fecha)
```

Resultado

```text
2026-07-27
```

---

También podemos sumar horas.

```python
fecha + timedelta(hours=8)
```

O semanas.

```python
fecha + timedelta(weeks=4)
```

---

# Restar tiempo

```python
fecha - timedelta(days=30)
```

Resultado

```text
2026-06-17
```

Muy utilizado para:

- Ventanas de tiempo
- Filtros
- Series Temporales

---

# Diferencia entre dos fechas

```python
inicio = datetime(2026,7,1)
fin = datetime(2026,7,17)

diferencia = fin - inicio

print(diferencia)
```

Resultado

```text
16 days, 0:00:00
```

---

También podemos obtener únicamente los días.

```python
diferencia.days
```

Resultado

```python
16
```

---

# Comparar fechas

Los objetos `datetime` pueden compararse igual que números.

```python
fecha1 > fecha2
```

```python
fecha1 < fecha2
```

```python
fecha1 >= fecha2
```

```python
fecha1 <= fecha2
```

```python
fecha1 == fecha2
```

Ejemplo

```python
if fecha1 > fecha2:
    print("La fecha es posterior")
```

---

# Obtener el día de la semana

```python
fecha.weekday()
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

También existe

```python
fecha.isoweekday()
```

Aquí

|Valor|Día|
|------|---|
|1|Lunes|
|7|Domingo|

---

# Obtener el nombre del día

```python
fecha.strftime("%A")
```

Resultado

```text
Friday
```

Abreviado

```python
fecha.strftime("%a")
```

Resultado

```text
Fri
```

---

# Timestamp Unix

Un **Timestamp Unix** representa el número de segundos transcurridos desde

```
1970-01-01 00:00:00 UTC
```

Es uno de los formatos más utilizados para almacenar fechas.

Ejemplo

```python
fecha.timestamp()
```

Resultado

```text
1784304000.0
```

Es especialmente útil cuando:

- Se trabaja con APIs.
- Bases de datos.
- Sistemas distribuidos.
- Logs.

---

# Convertir un Timestamp a datetime

```python
datetime.fromtimestamp(1784304000)
```

Resultado

```text
2026-07-17
```

---

# date vs datetime

Muchas personas los confunden.

## date

Representa únicamente

- Año
- Mes
- Día

```python
from datetime import date

date.today()
```

Resultado

```text
2026-07-17
```

---

## datetime

Representa

- Año
- Mes
- Día
- Hora
- Minutos
- Segundos

```python
datetime.now()
```

Resultado

```text
2026-07-17 14:35:48
```

---

# time

Existe otro objeto.

```python
from datetime import time

time(14,30)
```

Resultado

```text
14:30:00
```

Solo almacena la hora.

---

# Combinar date y time

```python
from datetime import datetime

datetime.combine(fecha, hora)
```

Muy útil cuando ambas variables vienen separadas.

---

# Timezones

Una fecha puede tener o no una zona horaria.

Sin zona horaria:

```python
2026-07-17 15:30
```

No sabemos si corresponde a

- Nicaragua
- Miami
- Londres
- Tokio

Con zona horaria:

```
2026-07-17 15:30 UTC-6
```

Ahora sí sabemos exactamente el instante.

---

## UTC

UTC (**Coordinated Universal Time**) es el estándar mundial sobre el cual se calculan todas las zonas horarias.

Ejemplos

|Ciudad|UTC|
|-------|---|
|Managua|UTC-6|
|Miami (invierno)|UTC-5|
|Londres|UTC+0|
|Madrid|UTC+1|

---

# Calendario

Python también incluye el módulo

```python
import calendar
```

Podemos imprimir un calendario.

```python
calendar.month(2026,7)
```

Resultado

```text
     July 2026
Mo Tu We Th Fr Sa Su
...
```

---

# Buenas Prácticas

> [!success]
>
> - Guarda siempre las fechas como objetos `datetime`.
> - Convierte las fechas apenas leas un CSV.
> - Utiliza UTC cuando desarrolles aplicaciones distribuidas.
> - Usa `timedelta` para sumar y restar tiempo.
> - Evita realizar operaciones con fechas usando strings.

---

# Errores Comunes

> [!warning]

### Comparar strings

❌ Incorrecto

```python
"2026-7-1" > "2026-10-5"
```

La comparación puede ser incorrecta dependiendo del formato.

✔ Correcto

```python
datetime1 > datetime2
```

---

### Trabajar con diferentes zonas horarias

Comparar fechas de dos países sin convertirlas previamente a una misma zona horaria puede producir errores muy difíciles de detectar.

---

### No convertir los datos al leer un CSV

Muchas veces un CSV almacena las fechas como texto.

Siempre conviene convertirlas inmediatamente.

En Pandas:

```python
pd.to_datetime(df["fecha"])
```

---

# Resumen General del Módulo datetime

> [!success]
>
> Después de este cuaderno deberías dominar:
>
> - Crear fechas.
> - Obtener la fecha actual.
> - Extraer año, mes y día.
> - Convertir texto ↔ fecha.
> - Formatear fechas.
> - Utilizar `%Y`, `%m`, `%d`, `%H`, `%M`, `%S`.
> - Sumar y restar fechas.
> - Calcular diferencias.
> - Comparar fechas.
> - Trabajar con `timedelta`.
> - Entender timestamps Unix.
> - Conocer `date`, `time` y `datetime`.
> - Comprender el concepto de zonas horarias.

---

# Cheat Sheet

|Tarea|Código|
|------|------|
|Fecha actual|`datetime.now()`|
|Convertir texto → fecha|`datetime.strptime()`|
|Convertir fecha → texto|`datetime.strftime()`|
|Año|`fecha.year`|
|Mes|`fecha.month`|
|Día|`fecha.day`|
|Hora|`fecha.hour`|
|Minuto|`fecha.minute`|
|Segundo|`fecha.second`|
|Sumar días|`fecha + timedelta(days=5)`|
|Restar días|`fecha - timedelta(days=5)`|
|Diferencia entre fechas|`fecha2 - fecha1`|
|Día de la semana|`fecha.weekday()`|
|Timestamp Unix|`fecha.timestamp()`|

---

# Conceptos Relacionados

- [[Introducción a las Series de Tiempo]]
- [[Manipulación de Fechas con Pandas]]
- [[Feature Engineering para Series Temporales]]
- [[Pandas DatetimeIndex]]
- [[Resampling]]
- [[SQL - Funciones de Fecha]]