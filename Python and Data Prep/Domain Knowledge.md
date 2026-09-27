---
tags:
  - machine-learning
  - feature-engineering
  - domain-knowledge
created: {{date}}
---

# Domain Knowledge

> [!abstract]
> **Domain Knowledge** consiste en utilizar conocimiento específico del problema para crear variables que un algoritmo difícilmente descubriría por sí solo. Es una de las técnicas de Feature Engineering con mayor impacto en el rendimiento de un modelo. :contentReference[oaicite:0]{index=0}

---

# ¿Qué es Domain Knowledge?

Hasta ahora hemos visto transformaciones "genéricas":

- StandardScaler
- One-Hot Encoding
- Log Transform
- Polynomial Features

Estas funcionan para prácticamente cualquier dataset.

Sin embargo, **las mejores features suelen surgir al entender el problema**.

Un experto en el dominio sabe qué relaciones tienen sentido incluso antes de entrenar el modelo.

```text
Conocimiento del negocio
            │
            ▼
Creación de nuevas variables
            │
            ▼
Modelo más preciso
```

---

# Ejemplo sencillo

Queremos predecir el precio de una vivienda.

Dataset original

| Habitaciones | Superficie |
|--------------|-----------:|
|4|120|
|2|60|
|5|200|

Estas variables son útiles, pero un agente inmobiliario pensaría inmediatamente en:

```
Precio por metro cuadrado

Habitaciones por metro cuadrado

Edad de la vivienda

Distancia al centro
```

No aparecen directamente en los datos.

Hay que crearlas.

---

# Ejemplo en Python

```python
houses["rooms_per_m2"] = (
    houses["rooms"] /
    houses["surface"]
)
```

O

```python
houses["price_per_m2"] = (
    houses["price"] /
    houses["surface"]
)
```

Estas nuevas variables suelen explicar mejor el problema que las originales.

---

# Ejemplo del Titanic

En la lecture se utilizan varios ejemplos del dataset Titanic para demostrar cómo el conocimiento del dominio puede mejorar las features. :contentReference[oaicite:1]{index=1}

---

# Ejemplo 1 — Extraer títulos del nombre

Nombre original

```
Braund, Mr. Owen Harris

Cumings, Mrs. John Bradley

Heikkinen, Miss. Laina
```

Para un ordenador son simplemente cadenas de texto.

Sin embargo, una persona detecta inmediatamente información útil:

- Mr
- Mrs
- Miss
- Master

Estos títulos contienen información sobre:

- sexo
- edad aproximada
- estatus social

---

## Código

```python
def extract_title(name):
    after_comma = name.split(",", 1)[1]
    title = after_comma.split(".", 1)[0]
    return title.strip()

df["Title"] = df["Name"].apply(extract_title)
```

Resultado

| Name | Title |
|------|-------|
|Braund, Mr...|Mr|
|Cumings, Mrs...|Mrs|
|Heikkinen, Miss...|Miss|

Después se puede aplicar One-Hot Encoding.

```python
title_dummies = pd.get_dummies(
    df["Title"],
    prefix="Title"
)

df = pd.concat([df, title_dummies], axis=1)
```

---

# Ejemplo 2 — Combinar variables

La lecture explica que durante el hundimiento del Titanic se siguió aproximadamente la regla:

> "Women and children first"

Además,

- la primera clase estaba más cerca de los botes salvavidas.

Por tanto,

```
Sexo
```

y

```
Clase
```

juntas contienen mucha más información que por separado. :contentReference[oaicite:2]{index=2}

---

## Crear la feature

```python
df["Sex_Pclass"] = (
    df["Sex"] +
    "_" +
    df["Pclass"].astype(str)
)
```

Resultado

| Sex | Pclass | Sex_Pclass |
|-----|--------|------------|
|female|1|female_1|
|female|3|female_3|
|male|2|male_2|

Después

```python
pd.get_dummies(
    df["Sex_Pclass"]
)
```

---

# ¿Por qué funciona?

Supongamos estas tasas de supervivencia.

| Grupo | Supervivencia |
|--------|--------------:|
|Mujeres|74%|
|Hombres|19%|

Parece una diferencia importante.

Pero si añadimos la clase...

| Grupo | Supervivencia |
|--------|--------------:|
|female_1|97%|
|female_2|92%|
|female_3|50%|
|male_1|37%|
|male_2|16%|
|male_3|14%|

Ahora el modelo dispone de mucha más información.

---

# Ejemplo 3 — Family Size

El Titanic contiene dos variables:

```
SibSp
```

Número de hermanos o cónyuges.

```
Parch
```

Número de padres o hijos.

Pero realmente interesa saber

> ¿Con cuántas personas viaja el pasajero?

Por ello se crea una nueva variable. :contentReference[oaicite:3]{index=3}

---

## Código

```python
df["FamilySize"] = (
    df["SibSp"] +
    df["Parch"] +
    1
)
```

El `+1` representa al propio pasajero.

---

## Crear otra feature

```python
df["IsAlone"] = (
    df["FamilySize"] == 1
).astype(int)
```

Resultado

| FamilySize | IsAlone |
|------------|--------:|
|1|1|
|2|0|
|5|0|

---

# Ejemplo práctico

```python
import pandas as pd

df = pd.DataFrame({

    "siblings":[1,0,3],

    "parents":[0,2,1]

})

df["family_size"] = (

    df["siblings"] +

    df["parents"] +

    1

)

df["is_alone"] = (

    df["family_size"] == 1

).astype(int)

print(df)
```

Resultado

|siblings|parents|family_size|is_alone|
|--------|-------|----------:|--------:|
|1|0|2|0|
|0|2|3|0|
|3|1|5|0|

---

# Más ejemplos de Domain Knowledge

## Finanzas

Variables originales

```
Ingresos

Deudas
```

Nueva feature

```python
df["debt_ratio"] = (
    df["debt"] /
    df["income"]
)
```

---

## Salud

Variables

```
Peso

Altura
```

Nueva feature

```python
df["BMI"] = (
    df["weight"] /
    (df["height"]**2)
)
```

---

## E-commerce

Variables

```
Compras

Tiempo registrado
```

Nueva feature

```python
df["purchases_per_month"] = (

    df["orders"] /

    df["months_registered"]

)
```

---

## Deportes

Variables

```
Partidos

Goles
```

Nueva feature

```python
df["goals_per_game"] = (
    df["goals"] /
    df["games"]
)
```

---

# ¿Cómo pensar nuevas features?

Una buena estrategia consiste en preguntarse:

- ¿Qué calcularía un experto humano?
- ¿Qué ratios tienen sentido?
- ¿Qué variables suelen analizarse juntas?
- ¿Qué información falta para explicar el fenómeno?
- ¿Existe alguna fórmula conocida?

Normalmente las mejores features son:

- proporciones
- medias
- tasas
- diferencias
- interacciones
- acumulados

---

# Buenas prácticas

> [!tip]
>
> - Entiende el negocio antes de escribir código.
> - Habla con expertos del dominio.
> - Crea pocas variables, pero con sentido.
> - Evalúa siempre si realmente mejoran el modelo.
> - Documenta el significado de cada nueva feature.

---

# Errores comunes

❌ Crear cientos de features sin una hipótesis.

❌ Usar información que no estará disponible en producción (**data leakage**).

❌ Crear ratios con divisiones por cero.

```python
df["ratio"] = (
    df["a"] /
    (df["b"] + 1e-8)
)
```

❌ Duplicar información ya existente.

---

# Domain Knowledge vs Polynomial Features

| Domain Knowledge | Polynomial Features |
|------------------|---------------------|
|Creado por una persona|Creado automáticamente|
|Muy interpretable|Poco interpretable|
|Pocas variables nuevas|Muchas variables nuevas|
|Generalmente mejora más|Puede provocar overfitting|

Siempre que sea posible, es preferible crear features utilizando conocimiento del problema antes que generar cientos de combinaciones automáticas.

---

# Resumen

> [!summary]
>
> - El Domain Knowledge suele producir las features más valiosas.
> - Consiste en crear variables utilizando conocimiento del problema.
> - Las mejores features suelen ser ratios, combinaciones o agregaciones.
> - El Titanic muestra cómo variables como `Title`, `FamilySize` o `Sex × Pclass` capturan información que las variables originales no expresan.
> - Antes de usar técnicas automáticas, piensa cómo resolvería el problema un experto humano.

## Enlaces

- [[Feature Engineering]]
- [[Feature Transformations]]
- [[Ensemble Learning]]