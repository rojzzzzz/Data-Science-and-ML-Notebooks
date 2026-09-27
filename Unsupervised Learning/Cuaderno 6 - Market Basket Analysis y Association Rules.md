# 
# Parte 1 - Descubriendo Relaciones Ocultas entre Productos

> **Objetivo del cuaderno**
>
> A lo largo de este capítulo hemos estudiado dos grandes familias del aprendizaje no supervisado.
>
> Primero aprendimos a **agrupar observaciones similares** mediante Clustering.
>
> Después aprendimos a **reducir dimensiones** utilizando PCA.
>
> Ahora estudiaremos un tercer problema completamente diferente.
>
> Ya no intentaremos agrupar observaciones.
>
> Ya no intentaremos reducir variables.
>
> Ahora intentaremos responder preguntas como:
>
> - ¿Qué productos suelen comprarse juntos?
> - ¿Qué patrones de compra existen?
> - ¿Qué recomendaciones pueden hacerse automáticamente?
>
> Bienvenido al mundo del **Market Basket Analysis**.

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Comprender qué problema resuelve Market Basket Analysis.
- Entender qué es una transacción.
- Comprender qué es una regla de asociación.
- Calcular manualmente Support.
- Calcular manualmente Confidence.
- Interpretar correctamente Lift.
- Comprender el algoritmo Apriori.
- Implementar reglas de asociación utilizando Python.
- Interpretar correctamente los resultados.
- Comprender las aplicaciones reales de esta técnica.

---

# 1. Un problema completamente diferente

Hasta ahora todas nuestras observaciones eran personas.

Por ejemplo,

|Edad|Ingreso|
|------|---------|
|25|900|
|32|1800|
|58|6500|

Cada fila representaba un cliente.

Ahora cambiaremos completamente de escenario.

Supongamos un supermercado.

Cada compra realizada genera un ticket.

```
Factura 001

Pan

Leche

Huevos
```

```
Factura 002

Pan

Mantequilla
```

```
Factura 003

Leche

Queso

Huevos
```

Observa algo interesante.

Ya no nos interesa describir al cliente.

Nos interesa estudiar la relación entre los productos.

---

# 2. ¿Por qué nació Market Basket Analysis?

Imaginemos un supermercado con millones de ventas.

Cada día se generan miles de tickets.

A simple vista resulta imposible responder preguntas como:

- ¿Qué productos suelen comprarse juntos?
- ¿Qué artículos podrían colocarse cerca?
- ¿Qué promociones serían efectivas?
- ¿Qué productos conviene recomendar?

El análisis tradicional resulta insuficiente.

Necesitamos un algoritmo que descubra automáticamente esas relaciones.

---

# 3. ¿Qué significa "Market Basket"?

El nombre proviene de una idea muy sencilla.

Cuando un cliente realiza una compra,

coloca productos dentro de un carrito.

Ese carrito representa una transacción.

Por ejemplo.

```
Canasta

↓

Pan

Leche

Huevos

Queso
```

Otra persona compra

```
Pan

Mantequilla

Café
```

Cada carrito constituye una observación.

---

# 4. ¿Qué es una transacción?

En este tipo de análisis,

cada fila del dataset representa una compra.

No representa una persona.

No representa un producto.

Representa una transacción.

Por ejemplo.

|Factura|Productos|
|---------|------------------------------|
|1|Pan, Leche, Huevos|
|2|Pan, Café|
|3|Leche, Queso|
|4|Pan, Leche|

Cada factura constituye una observación independiente.

---

# 5. Diferencia con Clustering

Es importante notar la diferencia.

En Clustering,

cada fila representaba una persona.

```
Cliente

↓

Edad

Ingreso

Compras
```

Aquí,

cada fila representa una compra.

```
Compra

↓

Pan

Leche

Huevos
```

Estamos estudiando un problema completamente distinto.

---

# 6. Diferencia con PCA

PCA buscaba responder

> ¿Cómo reducir el número de variables?

Market Basket Analysis responde otra pregunta.

> ¿Qué elementos aparecen juntos con frecuencia?

No intenta reducir información.

No intenta agrupar personas.

Intenta descubrir relaciones.

---

# 7. ¿Qué es una regla de asociación?

Supongamos que analizamos un millón de compras.

Descubrimos lo siguiente.

```
Pan

↓

Leche
```

Esta expresión recibe el nombre de

**Regla de Asociación**.

Pero...

¿qué significa realmente?

Muchos estudiantes responden

> "Si alguien compra pan, entonces comprará leche."

Eso es incorrecto.

Una regla de asociación no expresa una certeza.

Expresa una tendencia.

Una mejor interpretación sería

> **Las personas que compran pan tienden a comprar leche con mayor frecuencia que el resto de los clientes.**

---

# 8. Correlación NO significa causalidad

Este punto merece especial atención.

Supongamos la regla

```
Helados

↓

Bloqueador solar
```

¿Significa que comprar helados provoca comprar bloqueador?

No.

Ambos aumentan durante el verano.

Existe una causa común.

Otro ejemplo.

```
Paraguas

↓

Botas de lluvia
```

Comprar paraguas no provoca comprar botas.

Ambos aparecen cuando llueve.

Por ello,

las reglas de asociación describen patrones.

No relaciones de causa y efecto.

---

# 9. ¿Cómo sabemos si una regla es importante?

Supongamos dos reglas.

```
Pan

↓

Leche
```

```
Caviar

↓

Champaña
```

¿Cuál es mejor?

Necesitamos métricas.

Las tres más importantes son

- Support
- Confidence
- Lift

---

# 10. Support

El Support responde una pregunta muy sencilla.

> **¿Con qué frecuencia aparece un conjunto de productos?**

Supongamos diez compras.

|Factura|Productos|
|---------|--------------------|
|1|Pan, Leche|
|2|Pan|
|3|Pan, Leche|
|4|Leche|
|5|Pan, Leche|
|6|Huevos|
|7|Pan|
|8|Pan, Leche|
|9|Café|
|10|Pan|

Pan y Leche aparecen juntos

4 veces.

El soporte será

\[
Support=
\frac{4}{10}
=0.40
\]

Es decir,

40 % de todas las compras contienen ambos productos.

---

# 11. Interpretación del Support

Un soporte alto significa

> Esa combinación aparece frecuentemente.

Un soporte bajo significa

> Esa combinación es poco común.

Por sí solo,

el soporte no indica si existe una buena regla.

Simplemente indica frecuencia.

---

# 12. Confidence

Ahora respondemos otra pregunta.

> Si alguien compró Pan,

¿qué probabilidad existe de que también haya comprado Leche?

La fórmula es

\[
Confidence=
\frac{Support(Pan,Leche)}
{Support(Pan)}
\]

Supongamos.

Pan aparece

8 veces.

Pan y Leche aparecen

4 veces.

Entonces

\[
Confidence=
\frac{4}{8}
=
0.50
\]

Interpretación.

La mitad de las personas que compraron pan también compraron leche.

---

# 13. Interpretación correcta

Es muy importante leer correctamente una regla.

```
Pan

↓

Leche
```

NO significa

> La mitad de quienes compraron leche compraron pan.

La dirección importa.

Las reglas

```
Pan

↓

Leche
```

y

```
Leche

↓

Pan
```

generalmente poseen confidencias distintas.

---

# 14. Lift

Ahora aparece probablemente la métrica más importante.

El Lift responde la siguiente pregunta.

> **¿La asociación observada es realmente interesante o simplemente ocurre porque ambos productos son muy populares?**

La fórmula es

\[
Lift=
\frac{Confidence}
{Support(Leche)}
\]

---

# 15. Interpretación del Lift

## Lift = 1

No existe asociación.

Comprar pan no cambia la probabilidad de comprar leche.

---

## Lift > 1

Existe asociación positiva.

Comprar pan aumenta la probabilidad de comprar leche.

---

## Lift < 1

Existe asociación negativa.

Comprar pan reduce la probabilidad de comprar leche.

---

# 16. Un ejemplo

Supongamos.

Support

```
Pan

80 %
```

Support

```
Leche

70 %
```

Confidence

```
Pan

↓

Leche

70 %
```

El Lift será

\[
\frac{0.70}{0.70}=1
\]

No existe ninguna asociación especial.

La leche ya era muy popular.

---

# 17. El algoritmo Apriori

Ahora aparece el algoritmo que hace posible descubrir miles de reglas automáticamente.

El algoritmo Apriori parte de una idea muy elegante.

Supongamos que

```
Pan

Leche

Huevos
```

es una combinación muy frecuente.

Entonces necesariamente

```
Pan

Leche
```

también debe ser frecuente.

Y también

```
Pan

Huevos
```

Y

```
Leche

Huevos
```

Esta propiedad recibe el nombre de

> **Propiedad Apriori**.

---

# 18. ¿Por qué funciona?

Imaginemos que

```
Pan

Leche

Huevos
```

aparece únicamente dos veces.

Entonces

```
Pan

Leche

Huevos

Queso
```

jamás podrá aparecer diez veces.

Sería imposible.

Por ello,

Apriori elimina enormes cantidades de combinaciones antes de analizarlas.

Eso reduce drásticamente el tiempo de cálculo.

---

# 19. Implementación en Python

Actualmente la implementación más utilizada pertenece a la librería

```python
mlxtend
```

El flujo general es

```python
apriori()

↓

Frequent Itemsets

↓

association_rules()

↓

Reglas
```

Aunque el libro presenta un ejemplo práctico, el objetivo sigue siendo el mismo: encontrar conjuntos frecuentes y generar reglas de asociación a partir de ellos. :contentReference[oaicite:0]{index=0}

---

# 20. Aplicaciones reales

Market Basket Analysis no se limita a supermercados.

También se utiliza en

## Amazon

Productos recomendados.

```
Clientes que compraron este libro

↓

También compraron...
```

---

## Netflix

Películas frecuentemente vistas juntas.

---

## Spotify

Canciones que suelen escucharse en la misma sesión.

---

## Bancos

Productos financieros adquiridos conjuntamente.

---

## Hospitales

Medicamentos prescritos en conjunto.

---

# 21. Limitaciones

Aunque es una técnica poderosa,

también presenta limitaciones.

- Correlación no implica causalidad.
- Reglas muy frecuentes pueden no aportar conocimiento.
- Bases de datos muy grandes generan millones de reglas.
- Se requiere filtrar cuidadosamente utilizando Support, Confidence y Lift.

---

# 22. Comparación de todo el capítulo

| Método | Objetivo | Entrada | Resultado |
|---------|----------|----------|------------|
| K-Means | Encontrar grupos | Variables numéricas | Clusters |
| PCA | Reducir dimensiones | Variables numéricas | Componentes principales |
| Association Rules | Descubrir relaciones | Transacciones | Reglas de asociación |

---

# 23. Resumen general del capítulo

Durante este capítulo estudiamos las tres grandes familias del aprendizaje no supervisado.

```
UNSUPERVISED LEARNING

│

├───────────────┐
│               │
│               │
▼               ▼

Clustering      PCA

│               │

Agrupar         Reducir

Observaciones   Dimensiones

│               │

└───────────────┐
                │
                ▼

Market Basket Analysis

↓

Descubrir relaciones
```

Cada técnica responde una pregunta diferente.

**Clustering**

> ¿Qué observaciones son similares?

**PCA**

> ¿Cómo representar los datos con menos variables?

**Association Rules**

> ¿Qué elementos aparecen juntos con frecuencia?

Comprender estas diferencias es fundamental, porque permite seleccionar la herramienta adecuada según el problema que se desea resolver.

---

# Conclusión del capítulo

Con este cuaderno concluimos el estudio del **Aprendizaje No Supervisado**.

A diferencia del aprendizaje supervisado, donde el objetivo consiste en predecir una variable conocida, el aprendizaje no supervisado busca **descubrir conocimiento oculto** dentro de los datos.

A lo largo de estos siete cuadernos aprendimos que los datos pueden estudiarse desde tres perspectivas complementarias:

- **Su estructura**, mediante Clustering.
- **Su representación**, mediante PCA.
- **Sus relaciones**, mediante Market Basket Analysis.

Estas tres herramientas constituyen la base del análisis exploratorio moderno y son utilizadas diariamente en áreas como marketing, finanzas, medicina, biología, comercio electrónico y sistemas de recomendación.

Con esto finalizamos uno de los capítulos más importantes de Machine Learning.