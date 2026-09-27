# Manual rápido de HTTPX en Python

## 1. ¿Qué es HTTPX?

**HTTPX** es una librería de Python para realizar peticiones HTTP. Su sintaxis es parecida a `requests`, pero añade características modernas como:

- API síncrona y asíncrona.
- HTTP/1.1 y HTTP/2.
- Pool de conexiones.
- Timeouts configurables.
- Cookies y autenticación.
- Subida de archivos.
- Streaming de respuestas.

HTTPX proporciona tanto `Client` como `AsyncClient` para reutilizar conexiones y configuración.

---

## 2. Instalación

```
pip install httpx

```

Después se importa normalmente:

```
import httpx

```

---

## 3. Petición GET

Una petición `GET` se utiliza normalmente para obtener información.

```
import httpx

response = httpx.get("https://jsonplaceholder.typicode.com/posts/1")

print(response.status_code)
print(response.json())

```

Algunas propiedades importantes de `response` son:

```
response.status_code
response.text
response.json()
response.headers
response.url

```

Por ejemplo:

```
if response.status_code == 200:
    data = response.json()
    print(data)

```

---

## 4. Parámetros en una petición GET

Podemos enviar parámetros mediante `params`.

```
import httpx

params = {
    "page": 2,
    "limit": 10
}

response = httpx.get(
    "https://example.com/api/users",
    params=params
)

print(response.url)

```

HTTPX construirá una URL similar a:

```
https://example.com/api/users?page=2&limit=10

```

---

## 5. Petición POST

`POST` se utiliza normalmente para enviar información a un servidor.

```
import httpx

data = {
    "name": "Carlos",
    "email": "carlos@example.com"
}

response = httpx.post(
    "https://example.com/api/users",
    json=data
)

print(response.status_code)
print(response.json())

```

Usar:

```
json=data

```

hace que HTTPX envíe los datos como JSON.

Para datos de formulario se puede utilizar:

```
response = httpx.post(
    "https://example.com/login",
    data={
        "username": "carlos",
        "password": "1234"
    }
)

```

---

## 6. PUT, PATCH y DELETE

HTTPX soporta los principales métodos HTTP.

### PUT

Normalmente reemplaza o actualiza un recurso.

```
response = httpx.put(
    "https://example.com/api/users/10",
    json={"name": "Carlos"}
)

```

### PATCH

Normalmente actualiza parcialmente un recurso.

```
response = httpx.patch(
    "https://example.com/api/users/10",
    json={"email": "nuevo@example.com"}
)

```

### DELETE

Elimina un recurso.

```
response = httpx.delete(
    "https://example.com/api/users/10"
)

```

---

## 7. Enviar headers

Los headers pueden enviarse mediante `headers`.

```
headers = {
    "Authorization": "Bearer MI_TOKEN",
    "Accept": "application/json"
}

response = httpx.get(
    "https://example.com/api/profile",
    headers=headers
)

```

Es especialmente común utilizar headers para tokens de autenticación.

---

## 8. `Client`

Si una aplicación realiza muchas peticiones, es recomendable utilizar `httpx.Client()`.

```
import httpx

with httpx.Client() as client:
    response = client.get("https://example.com")
    print(response.status_code)

```

Una ventaja importante es que el cliente mantiene un **pool de conexiones**, permitiendo reutilizar conexiones HTTP en lugar de crear una nueva para cada petición. También permite compartir configuración entre peticiones.

Por ejemplo:

```
import httpx

headers = {
    "Authorization": "Bearer MI_TOKEN"
}

with httpx.Client(
    base_url="https://example.com/api",
    headers=headers
) as client:

    users = client.get("/users")
    products = client.get("/products")

    print(users.json())
    print(products.json())

```

`base_url` evita repetir la dirección principal de la API.

---

## 9. Manejo de errores

HTTPX permite comprobar códigos HTTP con:

```
response.raise_for_status()

```

Por ejemplo:

```
import httpx

try:
    response = httpx.get("https://example.com/api/users")
    response.raise_for_status()

    print(response.json())

except httpx.HTTPStatusError as error:
    print("Error HTTP:", error)

except httpx.RequestError as error:
    print("Error de conexión:", error)

```

`HTTPStatusError` representa respuestas HTTP de error al utilizar `raise_for_status()`, mientras que `RequestError` cubre errores producidos al realizar la petición, como determinados problemas de red.

Una estructura sencilla muy útil es:

```
try:
    response = httpx.get(url)
    response.raise_for_status()
    data = response.json()

except httpx.HTTPError as error:
    print(f"Error: {error}")

```

---

## 10. Timeouts

HTTPX utiliza timeouts por defecto. Actualmente, su comportamiento estándar es lanzar una excepción después de **5 segundos de inactividad de red**.

Podemos establecer nuestro propio timeout:

```
response = httpx.get(
    "https://example.com",
    timeout=10.0
)

```

También puede configurarse en un cliente:

```
with httpx.Client(timeout=10.0) as client:
    response = client.get("https://example.com")

```

HTTPX incluso permite separar los diferentes tipos de timeout:

```
timeout = httpx.Timeout(
    10.0,
    connect=5.0
)

with httpx.Client(timeout=timeout) as client:
    response = client.get("https://example.com")

```

Existen timeouts relacionados con conexión, lectura, escritura y espera del pool de conexiones.

---

## 11. Redirecciones

HTTPX no sigue redirecciones automáticamente en la configuración habitual de `Client`. Se pueden activar utilizando:

```
response = httpx.get(
    "http://github.com",
    follow_redirects=True
)

```

También puede configurarse en el cliente:

```
with httpx.Client(follow_redirects=True) as client:
    response = client.get("http://github.com")

```

Las redirecciones seguidas pueden consultarse mediante:

```
response.history

```

---

## 12. HTTPX asíncrono

Una de las características más interesantes de HTTPX es `AsyncClient`.

```
import asyncio
import httpx


async def main():
    async with httpx.AsyncClient() as client:
        response = await client.get(
            "https://example.com"
        )

        print(response.status_code)


asyncio.run(main())

```

La diferencia principal es el uso de:

```
async with

```

y:

```
await client.get(...)

```

HTTPX recomienda utilizar `AsyncClient` cuando se trabaja en aplicaciones asíncronas.

---

## 13. Varias peticiones concurrentes

La versión asíncrona permite hacer varias peticiones de forma concurrente.

```
import asyncio
import httpx


async def get_user(client, user_id):
    response = await client.get(
        f"https://jsonplaceholder.typicode.com/users/{user_id}"
    )

    return response.json()


async def main():
    async with httpx.AsyncClient() as client:

        tasks = [
            get_user(client, 1),
            get_user(client, 2),
            get_user(client, 3)
        ]

        users = await asyncio.gather(*tasks)

        print(users)


asyncio.run(main())

```

Esto resulta especialmente útil cuando una aplicación necesita consultar muchas APIs o endpoints sin esperar secuencialmente cada respuesta.

---

## 14. Autenticación

### Basic Auth

HTTPX soporta autenticación HTTP Basic directamente:

```
response = httpx.get(
    "https://example.com/private",
    auth=("usuario", "password")
)

```

### Bearer Token

Para APIs con tokens suele utilizarse un header:

```
headers = {
    "Authorization": "Bearer MI_TOKEN"
}

response = httpx.get(
    "https://example.com/api/profile",
    headers=headers
)

```

---

## 15. Subir archivos

También podemos enviar archivos.

```
import httpx

with open("foto.jpg", "rb") as file:

    files = {
        "file": file
    }

    response = httpx.post(
        "https://example.com/upload",
        files=files
    )

print(response.status_code)

```

---

## 16. Ejemplo completo de consumo de una API

```
import httpx


API_URL = "https://jsonplaceholder.typicode.com"


def obtener_usuario(user_id):

    try:

        with httpx.Client(
            base_url=API_URL,
            timeout=10.0
        ) as client:

            response = client.get(
                f"/users/{user_id}"
            )

            response.raise_for_status()

            return response.json()

    except httpx.HTTPStatusError as error:

        print(
            "Error HTTP:",
            error.response.status_code
        )

    except httpx.RequestError as error:

        print(
            "Error de conexión:",
            error
        )


usuario = obtener_usuario(1)

if usuario:
    print(usuario["name"])
    print(usuario["email"])

```

Este patrón combina varias de las prácticas más importantes:

- `Client`.
- `base_url`.
- Timeout.
- `raise_for_status()`.
- Manejo de excepciones.
- Conversión de JSON a objetos de Python.

---

## 17. HTTPX vs Requests

La sintaxis básica es muy parecida.

Con `requests`:

```
import requests

response = requests.get(url)

```

Con HTTPX:

```
import httpx

response = httpx.get(url)

```

Una diferencia importante es que HTTPX ofrece directamente una API síncrona y otra asíncrona, además de soporte para HTTP/2. También aplica timeouts de red de forma predeterminada, mientras que Requests no establece timeout por defecto.

---

## 18. Chuleta rápida

```
import httpx

# GET
httpx.get(url)

# GET con parámetros
httpx.get(url, params={"page": 1})

# POST JSON
httpx.post(url, json={"name": "Carlos"})

# POST formulario
httpx.post(url, data={"user": "Carlos"})

# PUT
httpx.put(url, json=data)

# PATCH
httpx.patch(url, json=data)

# DELETE
httpx.delete(url)

# Headers
httpx.get(
    url,
    headers={"Authorization": "Bearer TOKEN"}
)

# Timeout
httpx.get(url, timeout=10)

# Seguir redirecciones
httpx.get(url, follow_redirects=True)

# Comprobar errores HTTP
response.raise_for_status()

# Convertir JSON
data = response.json()

```

### Cliente

```
with httpx.Client(
    base_url="https://api.example.com"
) as client:

    response = client.get("/users")

```

### Cliente asíncrono

```
async with httpx.AsyncClient() as client:
    response = await client.get(url)

```

---

## Conclusión

HTTPX es una buena opción para trabajar con APIs REST en Python, especialmente cuando se necesita una librería con una interfaz sencilla pero que también permita trabajar con programación asíncrona.

Para proyectos pequeños puedes utilizar directamente:

```
httpx.get(...)
httpx.post(...)

```

Para aplicaciones que realizan muchas peticiones es preferible utilizar:

```
httpx.Client()

```

Y si tu proyecto utiliza `asyncio`, FastAPI u otro entorno asíncrono, normalmente será más apropiado:

```
httpx.AsyncClient()

```
