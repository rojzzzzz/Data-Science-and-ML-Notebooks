

> [!abstract]
> El módulo **`datetime`** permite crear, manipular, comparar y formatear fechas y horas en Python. Es una de las herramientas más importantes para trabajar con **Series de Tiempo**, análisis de datos, SQL, APIs, automatización y Machine Learning.

---

# ¿Qué es `datetime`?

Python almacena las fechas como objetos, no como texto.

Por ejemplo, la fecha:

```
2026-07-17 14:30:00
```

No es simplemente una cadena de caracteres (`string`), sino un objeto que conoce:

- Año
- Mes
- Día
- Hora
- Minutos
- Segundos
- Microsegundos

Gracias a ello podemos realizar operaciones como:

- Sumar días.
- Restar fechas.
- Comparar fechas.
- Extraer el mes.
- Saber el día de la semana.
- Cambiar el formato de visualización.

---

# Importando datetime

La forma más utilizada es:

```python
from datetime import datetime
```

También es común importar otros objetos:

```python
from datetime import datetime, timedelta
```

o incluso todo el módulo:

```python
import datetime
```

---

# Crear una fecha

Podemos construir una fecha indicando sus componentes.

```python
from datetime import datetime

fecha = datetime(2026, 7, 17)

print(fecha)
```

Salida

```text
2026-07-17 00:00:00
```

También podemos incluir la hora.

```python
fecha = datetime(2026, 7, 17, 14, 35, 20)
```

Resultado

```text
2026-07-17 14:35:20
```

---

# Obtener la fecha y hora actual

## datetime.now()

Devuelve la fecha y hora actuales.

```python
from datetime import datetime

datetime.now()
```

Ejemplo

```text
2026-07-17 14:35:48.123456
```

Incluye microsegundos.

---

## datetime.today()

También devuelve la fecha actual.

```python
datetime.today()
```

En la práctica suele utilizarse más:

```python
datetime.now()
```

---

# Acceder a los componentes de una fecha

Una vez tenemos un objeto `datetime`, podemos acceder fácilmente a cada parte.

```python
fecha = datetime.now()
```

|Código|Resultado|
|-------|---------|
|`fecha.year`|2026|
|`fecha.month`|7|
|`fecha.day`|17|
|`fecha.hour`|14|
|`fecha.minute`|35|
|`fecha.second`|48|
|`fecha.microsecond`|123456|

Ejemplo

```python
print(fecha.year)
print(fecha.month)
print(fecha.day)
```

---

# Convertir texto en una fecha

## `strptime()`

Muchas veces recibimos una fecha como texto.

```python
fecha = "2026-07-17"
```

Pero Python todavía no la reconoce como una fecha.

Para convertirla utilizamos:

```python
datetime.strptime()
```

Ejemplo

```python
from datetime import datetime

fecha = datetime.strptime("2026-07-17", "%Y-%m-%d")

print(fecha)
```

Resultado

```text
2026-07-17 00:00:00
```

Ahora sí podemos trabajar con ella.

> [!tip]
> **`strptime` significa "String Parse Time".**
>
> Convierte un **string → datetime**.

---

# Convertir una fecha en texto

## `strftime()`

Hace exactamente lo contrario.

Convierte

```
datetime
```

en

```
string
```

Ejemplo

```python
fecha = datetime.now()

fecha.strftime("%d/%m/%Y")
```

Resultado

```text
17/07/2026
```

Otro ejemplo

```python
fecha.strftime("%Y-%m")
```

Resultado

```text
2026-07
```

> [!tip]
> **`strftime` significa "String Format Time".**
>
> Convierte un **datetime → string**.

---

# Los formatos más importantes

Los símbolos que utilizan `strptime()` y `strftime()` reciben el nombre de **directivas de formato**.

## Año

|Código|Ejemplo|
|-------|--------|
|`%Y`|2026|
|`%y`|26|

---

## Mes

|Código|Ejemplo|
|-------|--------|
|`%m`|07|
|`%b`|Jul|
|`%B`|July|

---

## Día

|Código|Ejemplo|
|-------|--------|
|`%d`|17|
|`%j`|198 (día del año)|

---

## Día de la semana

|Código|Ejemplo|
|-------|--------|
|`%A`|Friday|
|`%a`|Fri|
|`%w`|5|

---

## Hora

|Código|Ejemplo|
|-------|--------|
|`%H`|14|
|`%I`|02|
|`%p`|PM|

---

## Minutos y segundos

|Código|Ejemplo|
|-------|--------|
|`%M`|35|
|`%S`|48|
|`%f`|123456|

---

# Formatos comunes

## ISO 8601

```python
"%Y-%m-%d"
```

Resultado

```
2026-07-17
```

---

## Formato latinoamericano

```python
"%d/%m/%Y"
```

Resultado

```
17/07/2026
```

---

## Formato estadounidense

```python
"%m/%d/%Y"
```

Resultado

```
07/17/2026
```

---

## Fecha y hora

```python
"%Y-%m-%d %H:%M:%S"
```

Resultado

```
2026-07-17 14:35:48
```

---

# Errores muy comunes

> [!warning]
> Estos errores son extremadamente frecuentes.

### `%m` ≠ `%M`

```python
%m
```

Significa

**Mes**

```text
07
```

Mientras que

```python
%M
```

Significa

**Minutos**

```text
35
```

---

### `%Y` ≠ `%y`

```python
%Y
```

Devuelve

```
2026
```

Mientras que

```python
%y
```

Devuelve

```
26
```

---

### `%H` ≠ `%I`

`%H`

Hora en formato de **24 horas**.

```
14
```

`%I`

Hora en formato de **12 horas**.

```
02
```

---

# Equivalencias con Pandas

Cuando trabajemos con Pandas encontraremos propiedades muy parecidas.

|datetime|Pandas|
|---------|-------|
|`fecha.year`|`df["fecha"].dt.year`|
|`fecha.month`|`df["fecha"].dt.month`|
|`fecha.day`|`df["fecha"].dt.day`|
|`fecha.hour`|`df["fecha"].dt.hour`|

Esto hace que la transición entre Python y Pandas sea muy sencilla.

---

# Equivalencias con SQL

Muchos motores SQL tienen funciones equivalentes.

|Python|SQL|
|-------|----|
|`fecha.year`|`YEAR(fecha)`|
|`fecha.month`|`MONTH(fecha)`|
|`fecha.day`|`DAY(fecha)`|
|`fecha.strftime("%Y-%m")`|`DATE_FORMAT(fecha,'%Y-%m')` *(MySQL)*|
|`datetime.now()`|`CURRENT_TIMESTAMP`|

Aunque la sintaxis cambia entre motores (MySQL, PostgreSQL, SQL Server, Oracle, etc.), los conceptos son prácticamente los mismos.

---

# Buenas Prácticas

> [!success]
>
> - Trabaja con objetos `datetime`, no con strings.
> - Usa el formato ISO (`YYYY-MM-DD`) siempre que sea posible.
> - Evita comparar fechas como texto.
> - Convierte las fechas apenas leas un archivo CSV o Excel.
> - Memoriza los formatos `%Y`, `%m`, `%d`, `%H`, `%M` y `%S`; son los que utilizarás el 90% del tiempo.

---

# Resumen

> [!success]
> - `datetime` representa fechas y horas como objetos.
> - `datetime.now()` obtiene la fecha y hora actuales.
> - `strptime()` convierte **texto → fecha**.
> - `strftime()` convierte **fecha → texto**.
> - Los formatos (`%Y`, `%m`, `%d`, `%H`, etc.) controlan cómo se leen y muestran las fechas.
> - Los conceptos de `datetime` son muy similares a los utilizados en Pandas, SQL y otras herramientas de análisis de datos.

---

# Conceptos Relacionados

- [[Introducción a las Series de Tiempo]]
- [[Manipulación de Fechas con Pandas]]
- [[Feature Engineering para Series Temporales]]
- [[SQL - Funciones de Fecha]]
- [[Pandas DatetimeIndex]]