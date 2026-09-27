# Manual rápido de `pathlib` y `os` en Python

## 1. ¿Qué son `pathlib` y `os`?

Python incluye varias herramientas para trabajar con archivos, carpetas y rutas del sistema operativo.

Dos de las más importantes son:

- `pathlib`
- `os`

Ambas forman parte de la **biblioteca estándar de Python**, por lo que no necesitas instalarlas con `pip`.

En términos generales:

- `pathlib` ofrece una forma moderna y orientada a objetos para trabajar con rutas.
- `os` permite interactuar con el sistema operativo y contiene muchas funciones relacionadas con archivos, carpetas, variables de entorno y procesos.

En proyectos modernos, normalmente se recomienda utilizar `pathlib` para manipular rutas y archivos cuando sea posible.

---

## 2. Importación

Para utilizar `pathlib`:

```python
from pathlib import Path
```

Para utilizar `os`:

```python
import os
```

También pueden utilizarse juntos:

```python
from pathlib import Path
import os
```

---

# PARTE I — `pathlib`

## 3. Crear una ruta con `Path`

La clase principal de `pathlib` es:

```python
Path
```

Por ejemplo:

```python
from pathlib import Path

ruta = Path("datos/ventas.csv")

print(ruta)
```

Una ruta puede representar:

- Un archivo.
- Una carpeta.
- Una ruta relativa.
- Una ruta absoluta.

---

## 4. Crear rutas usando `/`

Una de las características más cómodas de `pathlib` es que permite construir rutas utilizando `/`.

```python
from pathlib import Path

ruta = Path("datos") / "ventas" / "ventas.csv"

print(ruta)
```

Esto es preferible a concatenar strings manualmente.

Evita hacer:

```python
ruta = "datos/" + "ventas/" + "ventas.csv"
```

Es mejor:

```python
ruta = Path("datos") / "ventas" / "ventas.csv"
```

---

## 5. Obtener la carpeta actual

Para obtener la carpeta donde se está ejecutando el programa:

```python
from pathlib import Path

ruta_actual = Path.cwd()

print(ruta_actual)
```

`cwd` significa:

```text
Current Working Directory
```

---

## 6. Obtener la carpeta del usuario

También podemos obtener la carpeta principal del usuario:

```python
from pathlib import Path

home = Path.home()

print(home)
```

En Windows podría ser algo parecido a:

```text
C:\Users\Roberto
```

---

## 7. Ruta absoluta

Podemos convertir una ruta relativa en una ruta absoluta:

```python
from pathlib import Path

ruta = Path("datos/ventas.csv")

print(ruta.absolute())
```

También puede utilizarse:

```python
ruta.resolve()
```

Ejemplo:

```python
ruta = Path("datos/ventas.csv")

ruta_completa = ruta.resolve()

print(ruta_completa)
```

---

## 8. Comprobar si una ruta existe

Para verificar si un archivo o carpeta existe:

```python
from pathlib import Path

ruta = Path("datos/ventas.csv")

if ruta.exists():
    print("La ruta existe")
else:
    print("La ruta no existe")
```

---

## 9. Comprobar si es archivo o carpeta

Para comprobar si una ruta representa un archivo:

```python
ruta.is_file()
```

Ejemplo:

```python
from pathlib import Path

ruta = Path("datos/ventas.csv")

if ruta.is_file():
    print("Es un archivo")
```

Para comprobar si es una carpeta:

```python
ruta.is_dir()
```

Ejemplo:

```python
from pathlib import Path

ruta = Path("datos")

if ruta.is_dir():
    print("Es una carpeta")
```

---

## 10. Obtener partes de una ruta

Supongamos esta ruta:

```python
from pathlib import Path

ruta = Path("datos/ventas/ventas_2026.csv")
```

Podemos obtener diferentes partes.

### Nombre completo del archivo

```python
ruta.name
```

Resultado:

```text
ventas_2026.csv
```

### Nombre sin extensión

```python
ruta.stem
```

Resultado:

```text
ventas_2026
```

### Extensión

```python
ruta.suffix
```

Resultado:

```text
.csv
```

### Carpeta padre

```python
ruta.parent
```

Resultado:

```text
datos/ventas
```

---

## 11. Crear una carpeta

Para crear una carpeta:

```python
from pathlib import Path

carpeta = Path("backup")

carpeta.mkdir()
```

Si la carpeta ya existe, puede producirse un error.

Para evitarlo:

```python
carpeta.mkdir(exist_ok=True)
```

---

## 12. Crear varias carpetas

Supongamos que queremos crear:

```text
datos/raw/2026
```

Podemos utilizar:

```python
from pathlib import Path

ruta = Path("datos/raw/2026")

ruta.mkdir(
    parents=True,
    exist_ok=True
)
```

`parents=True` permite crear las carpetas intermedias.

---

## 13. Listar archivos de una carpeta

Para recorrer los elementos de una carpeta:

```python
from pathlib import Path

carpeta = Path("datos")

for elemento in carpeta.iterdir():
    print(elemento)
```

Esto puede mostrar tanto:

- Archivos.
- Carpetas.

---

## 14. Buscar archivos con `glob`

Podemos buscar archivos utilizando patrones.

Por ejemplo, todos los archivos CSV:

```python
from pathlib import Path

carpeta = Path("datos")

for archivo in carpeta.glob("*.csv"):
    print(archivo)
```

---

## 15. Buscar archivos recursivamente

Si queremos buscar también dentro de subcarpetas:

```python
from pathlib import Path

carpeta = Path("datos")

for archivo in carpeta.rglob("*.csv"):
    print(archivo)
```

`rglob()` realiza una búsqueda recursiva.

---

## 16. Leer un archivo de texto

`Path` permite leer archivos de texto directamente.

```python
from pathlib import Path

ruta = Path("mensaje.txt")

contenido = ruta.read_text(
    encoding="utf-8"
)

print(contenido)
```

---

## 17. Escribir un archivo de texto

También podemos escribir directamente:

```python
from pathlib import Path

ruta = Path("mensaje.txt")

ruta.write_text(
    "Hola desde Python",
    encoding="utf-8"
)
```

Esto crea el archivo si no existe.

Si existe, reemplaza su contenido.

---

## 18. Leer y escribir archivos binarios

Para leer bytes:

```python
datos = ruta.read_bytes()
```

Para escribir bytes:

```python
ruta.write_bytes(datos)
```

Esto puede ser útil para:

- Imágenes.
- PDFs.
- Archivos binarios.

---

## 19. Cambiar el nombre de un archivo

Podemos utilizar:

```python
rename()
```

Ejemplo:

```python
from pathlib import Path

archivo = Path("ventas.csv")

archivo.rename("ventas_2026.csv")
```

---

## 20. Mover un archivo

`rename()` también puede mover archivos.

```python
from pathlib import Path

archivo = Path("ventas.csv")

archivo.rename(
    Path("backup") / "ventas.csv"
)
```

La carpeta destino debe existir.

---

## 21. Eliminar un archivo

Para eliminar un archivo:

```python
from pathlib import Path

archivo = Path("ventas.csv")

archivo.unlink()
```

Para evitar un error si no existe:

```python
archivo.unlink(missing_ok=True)
```

---

## 22. Eliminar una carpeta vacía

Para eliminar una carpeta vacía:

```python
from pathlib import Path

carpeta = Path("backup")

carpeta.rmdir()
```

La carpeta debe estar vacía.

---

## 23. Cambiar extensión de un archivo

Podemos utilizar:

```python
with_suffix()
```

Ejemplo:

```python
from pathlib import Path

archivo = Path("ventas.csv")

nuevo = archivo.with_suffix(".xlsx")

print(nuevo)
```

Resultado:

```text
ventas.xlsx
```

Esto no convierte el archivo.

Solamente genera una nueva ruta con otra extensión.

---

## 24. Cambiar el nombre dentro de una ruta

Podemos utilizar:

```python
with_name()
```

Ejemplo:

```python
from pathlib import Path

archivo = Path("datos/ventas.csv")

nuevo = archivo.with_name("ventas_2026.csv")

print(nuevo)
```

Resultado:

```text
datos/ventas_2026.csv
```

---

## 25. Ejemplo práctico con `pathlib`

Supongamos que recibimos archivos CSV y queremos moverlos a una carpeta de backup.

```python
from pathlib import Path

entrada = Path("datos")
backup = Path("backup")

backup.mkdir(exist_ok=True)

for archivo in entrada.glob("*.csv"):

    destino = backup / archivo.name

    archivo.rename(destino)

    print(
        f"Movido: {archivo.name}"
    )
```

Este tipo de operación es muy común en pipelines ETL.

---

# PARTE II — `os`

## 26. ¿Qué es `os`?

El módulo `os` permite interactuar con el sistema operativo.

Puede utilizarse para:

- Obtener rutas.
- Crear carpetas.
- Listar archivos.
- Renombrar archivos.
- Eliminar archivos.
- Trabajar con variables de entorno.
- Consultar información del sistema.

Se importa mediante:

```python
import os
```

---

## 27. Obtener la carpeta actual con `os`

```python
import os

ruta_actual = os.getcwd()

print(ruta_actual)
```

Esto es equivalente aproximadamente a:

```python
Path.cwd()
```

---

## 28. Cambiar la carpeta actual

Podemos modificar el directorio de trabajo:

```python
import os

os.chdir("datos")

print(os.getcwd())
```

Debe utilizarse con cuidado, porque cambia el contexto global de rutas relativas del programa.

---

## 29. Listar archivos y carpetas

Para listar el contenido de una carpeta:

```python
import os

elementos = os.listdir("datos")

for elemento in elementos:
    print(elemento)
```

---

## 30. Comprobar si una ruta existe

```python
import os

if os.path.exists("datos/ventas.csv"):
    print("Existe")
```

---

## 31. Comprobar si es archivo

```python
import os

if os.path.isfile("datos/ventas.csv"):
    print("Es archivo")
```

---

## 32. Comprobar si es carpeta

```python
import os

if os.path.isdir("datos"):
    print("Es carpeta")
```

---

## 33. Construir rutas con `os.path.join`

En lugar de concatenar strings:

```python
ruta = "datos/" + "ventas.csv"
```

podemos utilizar:

```python
import os

ruta = os.path.join(
    "datos",
    "ventas.csv"
)

print(ruta)
```

`os.path.join()` construye rutas adaptadas al sistema operativo.

---

## 34. Obtener nombre de archivo

```python
import os

ruta = "datos/ventas.csv"

nombre = os.path.basename(ruta)

print(nombre)
```

Resultado:

```text
ventas.csv
```

---

## 35. Obtener carpeta padre

```python
import os

ruta = "datos/ventas.csv"

carpeta = os.path.dirname(ruta)

print(carpeta)
```

Resultado:

```text
datos
```

---

## 36. Separar nombre y extensión

Podemos utilizar:

```python
os.path.splitext()
```

Ejemplo:

```python
import os

ruta = "ventas.csv"

nombre, extension = os.path.splitext(ruta)

print(nombre)
print(extension)
```

Resultado:

```text
ventas
.csv
```

---

## 37. Crear una carpeta

```python
import os

os.mkdir("backup")
```

Esto crea una sola carpeta.

Si ya existe, producirá un error.

---

## 38. Crear varias carpetas

Para crear una estructura completa:

```python
import os

os.makedirs(
    "datos/raw/2026",
    exist_ok=True
)
```

`exist_ok=True` evita errores si la carpeta ya existe.

---

## 39. Renombrar o mover archivos

Podemos utilizar:

```python
os.rename()
```

Ejemplo:

```python
import os

os.rename(
    "ventas.csv",
    "ventas_2026.csv"
)
```

También puede utilizarse para mover:

```python
os.rename(
    "ventas.csv",
    "backup/ventas.csv"
)
```

---

## 40. Eliminar un archivo

Podemos utilizar:

```python
os.remove()
```

Ejemplo:

```python
import os

os.remove("ventas.csv")
```

También existe:

```python
os.unlink("ventas.csv")
```

En este contexto realizan esencialmente la misma operación.

---

## 41. Eliminar una carpeta vacía

```python
import os

os.rmdir("backup")
```

La carpeta debe estar vacía.

---

## 42. Recorrer carpetas con `os.walk`

`os.walk()` permite recorrer una estructura de carpetas de forma recursiva.

```python
import os

for carpeta, subcarpetas, archivos in os.walk("datos"):

    print("Carpeta:", carpeta)

    for archivo in archivos:
        print("Archivo:", archivo)
```

Esto es útil cuando necesitamos explorar muchos archivos y subdirectorios.

---

## 43. Variables de entorno

Una característica muy importante de `os` es trabajar con variables de entorno.

Podemos consultar una variable mediante:

```python
import os

valor = os.getenv("DATABASE_URL")

print(valor)
```

También:

```python
valor = os.environ.get("DATABASE_URL")
```

Esto es muy común para guardar:

- Contraseñas.
- Usuarios.
- URLs.
- Tokens.
- Configuración.

Por ejemplo:

```python
import os

db_host = os.getenv("DB_HOST")
db_user = os.getenv("DB_USER")
db_password = os.getenv("DB_PASSWORD")
```

---

## 44. Consultar todas las variables de entorno

```python
import os

print(os.environ)
```

`os.environ` se comporta de forma similar a un diccionario.

Por ejemplo:

```python
for clave, valor in os.environ.items():
    print(clave, valor)
```

Debe tenerse cuidado porque algunas variables pueden contener información sensible.

---

## 45. Crear una variable de entorno temporal

Dentro del proceso actual:

```python
import os

os.environ["APP_MODE"] = "development"

print(
    os.getenv("APP_MODE")
)
```

Esta modificación normalmente solo afecta al proceso actual y a los procesos hijos que hereden su entorno.

---

# PARTE III — `pathlib` vs `os`

## 46. Construcción de rutas

Con `os`:

```python
import os

ruta = os.path.join(
    "datos",
    "ventas",
    "ventas.csv"
)
```

Con `pathlib`:

```python
from pathlib import Path

ruta = (
    Path("datos")
    / "ventas"
    / "ventas.csv"
)
```

Para rutas, `pathlib` suele resultar más legible.

---

## 47. Verificar existencia

Con `os`:

```python
os.path.exists("datos/ventas.csv")
```

Con `pathlib`:

```python
Path("datos/ventas.csv").exists()
```

---

## 48. Comprobar archivo

Con `os`:

```python
os.path.isfile("datos/ventas.csv")
```

Con `pathlib`:

```python
Path("datos/ventas.csv").is_file()
```

---

## 49. Crear carpetas

Con `os`:

```python
os.makedirs(
    "datos/raw",
    exist_ok=True
)
```

Con `pathlib`:

```python
Path("datos/raw").mkdir(
    parents=True,
    exist_ok=True
)
```

---

## 50. Listar archivos

Con `os`:

```python
import os

for archivo in os.listdir("datos"):
    print(archivo)
```

Con `pathlib`:

```python
from pathlib import Path

for archivo in Path("datos").iterdir():
    print(archivo)
```

---

## 51. Buscar CSV

Con `pathlib`:

```python
from pathlib import Path

for archivo in Path("datos").glob("*.csv"):
    print(archivo)
```

Con `os`, normalmente hay que combinar varias funciones o utilizar otros módulos.

Por esta razón, para trabajar principalmente con rutas y archivos, `pathlib` suele ser más cómodo.

---

# PARTE IV — Uso conjunto

## 52. `pathlib` y variables de entorno

Una combinación muy común es utilizar:

- `os` para variables de entorno.
- `pathlib` para rutas.

Ejemplo:

```python
from pathlib import Path
import os


base_dir = Path(
    os.getenv(
        "DATA_DIR",
        "datos"
    )
)

archivo = base_dir / "ventas.csv"

print(archivo)
```

Si `DATA_DIR` no existe, se utilizará:

```text
datos
```

---

## 53. Ejemplo para un pipeline ETL

Una estructura típica podría ser:

```text
proyecto/
│
├── data/
│   ├── raw/
│   ├── staging/
│   └── processed/
│
└── main.py
```

Podemos construir estas rutas:

```python
from pathlib import Path


BASE_DIR = Path.cwd()

DATA_DIR = BASE_DIR / "data"

RAW_DIR = DATA_DIR / "raw"
STAGING_DIR = DATA_DIR / "staging"
PROCESSED_DIR = DATA_DIR / "processed"


for carpeta in [
    RAW_DIR,
    STAGING_DIR,
    PROCESSED_DIR
]:
    carpeta.mkdir(
        parents=True,
        exist_ok=True
    )
```

---

## 54. Procesar varios CSV

Por ejemplo:

```python
from pathlib import Path
import pandas as pd


RAW_DIR = Path("data/raw")

for archivo in RAW_DIR.glob("*.csv"):

    df = pd.read_csv(archivo)

    print(
        archivo.name,
        len(df)
    )
```

Aquí `pathlib` funciona muy bien junto con pandas.

---

## 55. Crear un backup después de procesar

```python
from pathlib import Path
import shutil


RAW_DIR = Path("data/raw")
BACKUP_DIR = Path("data/backup")

BACKUP_DIR.mkdir(
    parents=True,
    exist_ok=True
)


for archivo in RAW_DIR.glob("*.csv"):

    destino = (
        BACKUP_DIR
        / archivo.name
    )

    shutil.move(
        archivo,
        destino
    )
```

Aunque `pathlib` maneja rutas, para determinadas operaciones avanzadas de copia y movimiento también es común utilizar el módulo:

```python
shutil
```

---

## 56. ¿Cuándo utilizar `pathlib`?

Utiliza `pathlib` especialmente para:

- Construir rutas.
- Verificar archivos.
- Crear carpetas.
- Buscar archivos.
- Obtener nombres y extensiones.
- Leer y escribir archivos sencillos.
- Recorrer directorios.
- Trabajar con rutas en pipelines de datos.

Por ejemplo:

```python
from pathlib import Path

path = Path("data/raw/orders.csv")
```

---

## 57. ¿Cuándo utilizar `os`?

Utiliza `os` especialmente cuando necesites:

- Variables de entorno.
- Cambiar el directorio de trabajo.
- Información del sistema operativo.
- Operaciones relacionadas con procesos.
- Compatibilidad con código antiguo que usa `os.path`.

Por ejemplo:

```python
import os

db_password = os.getenv(
    "DB_PASSWORD"
)
```

---

## 58. ¿Cuál aprender primero?

Para trabajar con datos, ETL y automatización, una buena prioridad es:

1. Aprender bien `Path`.
2. Aprender las operaciones principales de `pathlib`.
3. Aprender `os.getenv()` y `os.environ`.
4. Reconocer `os.path`.
5. Aprender `os.walk()`.

No es necesario memorizar todas las funciones de `os`.

---

# PARTE V — Chuleta rápida

## 59. `pathlib`

```python
from pathlib import Path


# Ruta
path = Path("data/file.csv")


# Combinar rutas
path = Path("data") / "file.csv"


# Carpeta actual
Path.cwd()


# Home del usuario
Path.home()


# Existe
path.exists()


# Es archivo
path.is_file()


# Es carpeta
path.is_dir()


# Nombre
path.name


# Nombre sin extensión
path.stem


# Extensión
path.suffix


# Carpeta padre
path.parent


# Ruta absoluta
path.resolve()


# Crear carpeta
Path("backup").mkdir(
    exist_ok=True
)


# Crear estructura
Path("data/raw").mkdir(
    parents=True,
    exist_ok=True
)


# Listar elementos
for item in Path("data").iterdir():
    print(item)


# Buscar CSV
for file in Path("data").glob("*.csv"):
    print(file)


# Buscar recursivamente
for file in Path("data").rglob("*.csv"):
    print(file)


# Leer texto
text = path.read_text(
    encoding="utf-8"
)


# Escribir texto
path.write_text(
    "Hola",
    encoding="utf-8"
)


# Renombrar
path.rename("nuevo.csv")


# Eliminar archivo
path.unlink(
    missing_ok=True
)
```

---

## 60. `os`

```python
import os


# Carpeta actual
os.getcwd()


# Cambiar carpeta
os.chdir("data")


# Listar carpeta
os.listdir("data")


# Existe
os.path.exists(
    "data/file.csv"
)


# Es archivo
os.path.isfile(
    "data/file.csv"
)


# Es carpeta
os.path.isdir(
    "data"
)


# Combinar rutas
os.path.join(
    "data",
    "file.csv"
)


# Nombre
os.path.basename(
    "data/file.csv"
)


# Carpeta padre
os.path.dirname(
    "data/file.csv"
)


# Separar extensión
os.path.splitext(
    "file.csv"
)


# Crear carpeta
os.mkdir("backup")


# Crear estructura
os.makedirs(
    "data/raw",
    exist_ok=True
)


# Renombrar o mover
os.rename(
    "file.csv",
    "backup/file.csv"
)


# Eliminar archivo
os.remove(
    "file.csv"
)


# Eliminar carpeta vacía
os.rmdir(
    "backup"
)


# Variable de entorno
os.getenv(
    "DB_PASSWORD"
)


# Variables de entorno
os.environ
```

---

# Conclusión

`pathlib` y `os` son dos herramientas fundamentales para automatización, manipulación de archivos y desarrollo de pipelines en Python.

Para código moderno relacionado con rutas, normalmente es preferible utilizar:

```python
from pathlib import Path
```

Por ejemplo:

```python
path = Path("data") / "raw" / "orders.csv"
```

Para configuración del sistema y variables de entorno, `os` sigue siendo especialmente importante:

```python
import os

password = os.getenv(
    "DB_PASSWORD"
)
```

Una combinación muy común en proyectos reales es:

```python
from pathlib import Path
import os
```

Utilizando:

```python
Path
```

para rutas y archivos, y:

```python
os.getenv()
```

para configuración y variables de entorno.
