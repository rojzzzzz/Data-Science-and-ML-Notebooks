# Documentación: Módulos `csv` y `json` en Python

Guía de referencia práctica para trabajar con archivos CSV y JSON usando la librería estándar de Python.

---

## Índice

1. [Módulo `csv`](#módulo-csv)
   - [Introducción](#introducción-csv)
   - [Lectura con `reader`](#lectura-con-reader)
   - [Lectura con `DictReader`](#lectura-con-dictreader)
   - [Escritura con `writer`](#escritura-con-writer)
   - [Escritura con `DictWriter`](#escritura-con-dictwriter)
   - [Dialectos y parámetros comunes](#dialectos-y-parámetros-comunes)
2. [Módulo `json`](#módulo-json)
   - [Introducción](#introducción-json)
   - [Convertir Python a JSON: `dumps` y `dump`](#convertir-python-a-json-dumps-y-dump)
   - [Convertir JSON a Python: `loads` y `load`](#convertir-json-a-python-loads-y-load)
   - [Tabla de equivalencias de tipos](#tabla-de-equivalencias-de-tipos)
   - [Parámetros útiles](#parámetros-útiles-de-json)
   - [Manejo de errores](#manejo-de-errores)
3. [Ejemplo combinado: CSV → JSON](#ejemplo-combinado-csv--json)
4. [Buenas prácticas](#buenas-prácticas)

---

## Módulo `csv`

### Introducción CSV

El módulo `csv` (Comma-Separated Values) permite leer y escribir archivos de valores separados por comas (u otro delimitador) de forma sencilla y segura, evitando errores comunes al dividir líneas manualmente con `split(",")` (como el manejo incorrecto de comas dentro de comillas).

```python
import csv
```

### Lectura con `reader`

`csv.reader` devuelve cada fila como una **lista** de strings.

```python
import csv

with open("datos.csv", newline="", encoding="utf-8") as archivo:
    lector = csv.reader(archivo)
    encabezado = next(lector)  # primera fila = encabezados
    for fila in lector:
        print(fila)
```

**Salida de ejemplo** (para un CSV con columnas `nombre,edad,ciudad`):
```
['Roberto', '17', 'Managua']
['Ana', '25', 'León']
```

> **Nota:** siempre abre el archivo con `newline=""` para evitar problemas con saltos de línea en distintos sistemas operativos.

### Lectura con `DictReader`

`csv.DictReader` devuelve cada fila como un **diccionario**, usando la primera fila como claves automáticamente.

```python
import csv

with open("datos.csv", newline="", encoding="utf-8") as archivo:
    lector = csv.DictReader(archivo)
    for fila in lector:
        print(fila["nombre"], fila["edad"])
```

**Salida:**
```
Roberto 17
Ana 25
```

### Escritura con `writer`

```python
import csv

datos = [
    ["nombre", "edad", "ciudad"],
    ["Roberto", 17, "Managua"],
    ["Ana", 25, "León"],
]

with open("salida.csv", "w", newline="", encoding="utf-8") as archivo:
    escritor = csv.writer(archivo)
    escritor.writerows(datos)   # escribe varias filas de una vez
    # escritor.writerow(datos[0])  # escribe una sola fila
```

### Escritura con `DictWriter`

Útil cuando los datos ya están en forma de diccionarios (por ejemplo, resultados de una API).

```python
import csv

datos = [
    {"nombre": "Roberto", "edad": 17, "ciudad": "Managua"},
    {"nombre": "Ana", "edad": 25, "ciudad": "León"},
]

with open("salida.csv", "w", newline="", encoding="utf-8") as archivo:
    campos = ["nombre", "edad", "ciudad"]
    escritor = csv.DictWriter(archivo, fieldnames=campos)
    escritor.writeheader()      # escribe la fila de encabezados
    escritor.writerows(datos)
```

### Dialectos y parámetros comunes

El módulo `csv` permite personalizar el formato mediante parámetros o "dialectos" predefinidos.

| Parámetro | Descripción | Ejemplo |
|---|---|---|
| `delimiter` | Carácter separador de columnas | `delimiter=";"` |
| `quotechar` | Carácter usado para encerrar campos | `quotechar='"'` |
| `quoting` | Estrategia de comillas | `csv.QUOTE_ALL`, `csv.QUOTE_MINIMAL` |
| `lineterminator` | Carácter de fin de línea | `lineterminator="\n"` |

```python
import csv

with open("datos_tsv.tsv", newline="", encoding="utf-8") as archivo:
    lector = csv.reader(archivo, delimiter="\t")  # tabulaciones en vez de comas
    for fila in lector:
        print(fila)
```

También puedes registrar un dialecto propio:

```python
csv.register_dialect("mi_formato", delimiter=";", quoting=csv.QUOTE_MINIMAL)

with open("datos.csv", newline="", encoding="utf-8") as archivo:
    lector = csv.reader(archivo, dialect="mi_formato")
```

---

## Módulo `json`

### Introducción JSON

El módulo `json` permite serializar (convertir objetos Python a texto JSON) y deserializar (convertir texto JSON a objetos Python). Es el formato estándar para intercambio de datos con APIs web.

```python
import json
```

### Convertir Python a JSON: `dumps` y `dump`

- `json.dumps(obj)` → devuelve un **string** con el JSON.
- `json.dump(obj, archivo)` → escribe el JSON directamente en un **archivo**.

```python
import json

persona = {
    "nombre": "Roberto",
    "edad": 17,
    "cursos": ["Docker", "PostgreSQL", "Python"],
    "activo": True
}

# A string
texto_json = json.dumps(persona, indent=4, ensure_ascii=False)
print(texto_json)

# A archivo
with open("persona.json", "w", encoding="utf-8") as archivo:
    json.dump(persona, archivo, indent=4, ensure_ascii=False)
```

**Salida:**
```json
{
    "nombre": "Roberto",
    "edad": 17,
    "cursos": [
        "Docker",
        "PostgreSQL",
        "Python"
    ],
    "activo": true
}
```

### Convertir JSON a Python: `loads` y `load`

- `json.loads(texto)` → convierte un **string** JSON en objeto Python.
- `json.load(archivo)` → lee un **archivo** JSON y lo convierte en objeto Python.

```python
import json

# Desde string
texto = '{"nombre": "Ana", "edad": 25}'
datos = json.loads(texto)
print(datos["nombre"])  # Ana

# Desde archivo
with open("persona.json", "r", encoding="utf-8") as archivo:
    datos = json.load(archivo)
print(datos)
```

### Tabla de equivalencias de tipos

| Python | JSON |
|---|---|
| `dict` | `object` |
| `list`, `tuple` | `array` |
| `str` | `string` |
| `int`, `float` | `number` |
| `True` / `False` | `true` / `false` |
| `None` | `null` |

### Parámetros útiles de JSON

| Parámetro | Función | Ejemplo |
|---|---|---|
| `indent` | Formatea el JSON con sangría (más legible) | `json.dumps(d, indent=2)` |
| `sort_keys` | Ordena las claves alfabéticamente | `json.dumps(d, sort_keys=True)` |
| `ensure_ascii` | Si es `False`, conserva tildes/ñ sin escapar | `json.dumps(d, ensure_ascii=False)` |
| `separators` | Controla separadores (útil para JSON compacto) | `json.dumps(d, separators=(",", ":"))` |
| `default` | Función para serializar objetos no soportados | `json.dumps(obj, default=str)` |

**Ejemplo con objetos personalizados** (por ejemplo, `datetime`):

```python
import json
from datetime import datetime

datos = {"evento": "clase", "fecha": datetime.now()}

# datetime no es serializable por defecto, así que usamos "default"
texto = json.dumps(datos, default=str, ensure_ascii=False)
print(texto)
```

### Manejo de errores

Al leer JSON, es común encontrarse con archivos mal formados. Se recomienda capturar `json.JSONDecodeError`.

```python
import json

texto_invalido = "{nombre: Roberto}"  # comillas faltantes -> inválido

try:
    datos = json.loads(texto_invalido)
except json.JSONDecodeError as e:
    print(f"Error al decodificar JSON: {e}")
```

---

## Ejemplo combinado: CSV → JSON

Un caso de uso frecuente en pipelines de datos: leer un CSV y convertirlo a JSON.

```python
import csv
import json

# 1. Leer el CSV como lista de diccionarios
with open("datos.csv", newline="", encoding="utf-8") as archivo_csv:
    lector = csv.DictReader(archivo_csv)
    registros = list(lector)

# 2. Escribir el resultado como JSON
with open("datos.json", "w", encoding="utf-8") as archivo_json:
    json.dump(registros, archivo_json, indent=4, ensure_ascii=False)

print(f"Se convirtieron {len(registros)} registros a JSON.")
```

---

## Buenas prácticas

- Usa siempre `with open(...)` para asegurar que los archivos se cierren correctamente.
- Al trabajar con CSV, abre el archivo con `newline=""` para evitar filas vacías o saltos de línea inesperados.
- Especifica `encoding="utf-8"` explícitamente, especialmente si trabajas con acentos o ñ.
- Usa `DictReader`/`DictWriter` cuando trabajes con datos tabulares que tienen nombres de columna claros; es más legible que índices numéricos.
- Usa `indent` en `json.dumps`/`json.dump` solo cuando necesites legibilidad (por ejemplo, para depuración); para producción o almacenamiento compacto, omítelo.
- Valida los datos externos (APIs, archivos de terceros) con manejo de excepciones (`json.JSONDecodeError`, `csv.Error`) antes de procesarlos.
- Para archivos muy grandes, considera procesar por lotes/streaming en lugar de cargar todo en memoria (por ejemplo, iterar el `reader` de CSV en vez de convertirlo a lista completa).

---

*Documentación generada como referencia rápida para el uso de los módulos estándar `csv` y `json` de Python.*
