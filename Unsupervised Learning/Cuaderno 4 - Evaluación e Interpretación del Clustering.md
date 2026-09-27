
# Eligiendo el Número Correcto de Clusters e Interpretando los Resultados

> **Objetivo del cuaderno**
>
> Hasta este momento ya sabemos cómo funciona K-Means y cómo implementarlo utilizando Scikit-Learn.
>
> Sin embargo, todavía queda una pregunta mucho más importante.
>
> **¿Cómo sabemos si el resultado obtenido es bueno?**
>
> En este cuaderno aprenderemos a evaluar un modelo de clustering, seleccionar el número adecuado de clusters e interpretar correctamente los grupos encontrados.

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Comprender por qué elegir el número de clusters es uno de los mayores desafíos del clustering.
- Interpretar correctamente la función objetivo de K-Means.
- Comprender qué representa `inertia_`.
- Aplicar e interpretar el Elbow Method.
- Comprender el Silhouette Score.
- Analizar e interpretar clusters mediante tablas y visualizaciones.
- Comprender cuándo K-Means no es la mejor alternativa.
- Conocer las principales variantes del clustering.

---

# 1. El gran problema de K-Means

Supongamos el siguiente conjunto de datos.

```
                 ● ● ●

             ● ●

                               ● ● ●

                           ● ●

        ● ●

     ●
```

La pregunta parece sencilla.

> ¿Cuántos grupos existen?

La respuesta no siempre lo es.

Un analista podría responder

> Tres.

Otro podría decir

> Cuatro.

Otro incluso podría afirmar

> Dos grandes grupos.

¿Quién tiene razón?

La respuesta es:

**todos podrían tener razón dependiendo del objetivo del análisis.**

---

# 2. ¿Existe un número correcto de clusters?

Esta es probablemente la pregunta más frecuente cuando se estudia K-Means.

La respuesta corta es:

> **No siempre.**

K-Means no descubre automáticamente el número "correcto" de grupos.

Nosotros debemos indicarle el valor de \(K\).

```python
KMeans(n_clusters=4)
```

El algoritmo simplemente obedecerá.

Nunca responderá:

> "Creo que tres grupos serían mejores."

Por eso necesitamos métodos para evaluar distintas alternativas.

---

# 3. ¿Qué intenta minimizar realmente K-Means?

En el cuaderno anterior vimos que K-Means intenta minimizar

\[
\sum_{i=1}^{n}
||x_i-\mu_k||^2
\]

Recordemos qué significa.

Cada punto calcula su distancia hasta el centroide de su grupo.

```
●

      ●

   X

          ●

●
```

Luego todas esas distancias se suman.

El objetivo del algoritmo consiste en que esa suma sea lo más pequeña posible.

Mientras menor sea esa suma,

más compactos serán los clusters.

---

# 4. ¿Qué representa la Inercia?

En Scikit-Learn esa suma recibe el nombre de

```python
inertia_
```

Muchas personas creen que la inercia representa "qué tan bueno es el modelo".

Eso no es completamente cierto.

La inercia representa únicamente

> **La suma de las distancias cuadradas entre cada observación y el centroide de su cluster.**

Es exactamente la función objetivo que K-Means intenta minimizar.

---

## Intuición

Supongamos un único cluster.

```
●

●

   X

●

●
```

Todos los puntos están cerca del centroide.

La suma de las distancias será pequeña.

Ahora observemos este caso.

```
●







                X







                           ●
```

Los puntos están muy dispersos.

La suma de las distancias será mucho mayor.

Por eso,

**una inercia pequeña indica clusters compactos.**

---

# 5. ¿Por qué siempre disminuye la Inercia?

Aquí aparece una observación muy interesante.

Supongamos

```
K = 1
```

Existe un solo centroide.

Toda la información debe agruparse alrededor de un único punto.

La distancia promedio será relativamente grande.

Ahora utilizamos

```
K = 2
```

Cada punto puede acercarse a un centroide diferente.

La distancia disminuye.

Ahora utilizamos

```
K = 3
```

La distancia disminuye nuevamente.

Y así sucesivamente.

Es decir,

```
Mayor K

↓

Más centroides

↓

Menores distancias

↓

Menor inercia
```

Esta propiedad siempre se cumple.

---

# 6. Entonces...

Si la inercia siempre disminuye,

¿por qué no elegir

```
K = número de observaciones
```

?

Supongamos un dataset con

100 clientes.

Elegimos

```
K = 100
```

Cada cliente será su propio cluster.

```
● X

● X

● X

● X
```

La distancia será exactamente cero.

La inercia será mínima.

Pero el modelo será completamente inútil.

No habrá aprendido absolutamente nada.

Aquí aparece un concepto fundamental.

> **El mejor modelo no es el que tiene la menor inercia.**

Es el que logra un equilibrio entre simplicidad y calidad.

---

# 7. El Elbow Method

El método más famoso para elegir K es el

**Elbow Method**.

El procedimiento es muy sencillo.

Entrenamos varios modelos.

```
K = 1

K = 2

K = 3

...

K = 10
```

Para cada uno calculamos

```python
kmeans.inertia_
```

Finalmente dibujamos el gráfico.

```
Inercia

|

|

|*

| *

|  *

|    *

|      *

|        *

|          *

+----------------------------

1 2 3 4 5 6 7 8 9 10
```

---

# 8. ¿Por qué aparece un "codo"?

Imaginemos nuevamente el gráfico.

```
|

|*

| *

|  *

|    *

|      *

|       *

|        *

+---------------------------->
```

Observemos la pendiente.

Al principio,

agregar un nuevo cluster reduce muchísimo la inercia.

Después de cierto punto,

cada nuevo cluster aporta muy poca mejora.

Ese cambio de pendiente forma un "codo".

De ahí proviene el nombre

> **Elbow Method**.

El libro utiliza exactamente este procedimiento calculando la inercia para distintos valores de \(K\) y graficando los resultados. :contentReference[oaicite:0]{index=0}

---

# 9. Un ejemplo intuitivo

Supongamos los siguientes resultados.

|K|Inercia|
|---|-------|
|1|900|
|2|500|
|3|240|
|4|180|
|5|160|
|6|150|

Observemos las diferencias.

```
900

↓

500

Gran mejora
```

```
500

↓

240

Gran mejora
```

```
240

↓

180

Mejora moderada
```

```
180

↓

160

Mejora pequeña
```

```
160

↓

150

Mejora mínima
```

Probablemente elegiríamos

```
K = 3
```

porque después de ese punto las mejoras son cada vez menores.

---

# 10. ¿Siempre aparece un codo?

No.

Y esta es una de las mayores limitaciones del método.

En muchos datasets reales obtenemos algo parecido a esto.

```
|

|*

| *

|  *

|   *

|    *

|      *

|        *

+---------------------------->
```

No existe ningún cambio brusco.

No existe un codo evidente.

En esos casos debemos utilizar otras herramientas.

---

# 11. El Silhouette Score

El libro menciona el **Silhouette Coefficient** como una alternativa cuando el Elbow Method no ofrece una respuesta clara, aunque deja su estudio como una investigación adicional. :contentReference[oaicite:1]{index=1}

Vale la pena comprenderlo.

Mientras la inercia mide únicamente qué tan compactos son los grupos,

el Silhouette Score intenta responder otra pregunta.

> ¿Los clusters están bien separados entre sí?

Es decir,

combina dos ideas.

- Compactación.
- Separación.

---

# 12. Interpretación del Silhouette Score

Su valor siempre se encuentra entre

\[
-1
\]

y

\[
1
\]

Interpretación.

|Valor|Interpretación|
|-------|--------------|
|≈1|Excelente separación|
|≈0|Clusters poco definidos|
|<0|Muchos puntos parecen pertenecer al cluster incorrecto|

En la práctica,

mientras más cercano a uno,

mejor.

---

# 13. El verdadero trabajo comienza ahora

Muchos estudiantes creen que el clustering termina cuando ejecutamos

```python
fit_predict()
```

En realidad,

ahí apenas comienza el análisis.

El algoritmo únicamente asignó números.

```
0

1

2

0

2

1
```

Esos números por sí solos no significan absolutamente nada.

Ahora debemos responder preguntas como:

- ¿Quiénes pertenecen al cluster 0?
- ¿Qué edad tienen?
- ¿Qué ingresos poseen?
- ¿Qué ocupaciones predominan?

Aquí aparece la interpretación del clustering.

---

# 14. Agregando el número de cluster

El libro concatena las etiquetas obtenidas con el dataset original.

```python
bank_with_cluster = pd.concat(
    [bank, labels],
    axis=1
)
```

De esta manera,

cada cliente posee una nueva columna.

|Edad|Ingreso|Cluster|
|------|----------|-----------|
|25|900|0|
|60|7000|2|
|38|2500|1|

Ahora podemos analizar las características de cada grupo. :contentReference[oaicite:2]{index=2}

---

# 15. Analizando la edad

Supongamos el siguiente resultado.

|Cluster|Edad promedio|
|---------|---------------|
|0|29|
|1|41|
|2|63|

Ahora los clusters comienzan a tener significado.

Podemos interpretar:

```
Cluster 0

↓

Clientes jóvenes
```

```
Cluster 1

↓

Adultos
```

```
Cluster 2

↓

Adultos mayores
```

Eso transforma números en conocimiento.

---

# 16. Tablas cruzadas

El libro utiliza

```python
groupby()

unstack()

pd.cut()
```

para construir tablas cruzadas de edad y ocupación. :contentReference[oaicite:3]{index=3}

Estas herramientas permiten responder preguntas como:

- ¿Qué porcentaje del cluster tiene entre 30 y 40 años?
- ¿Qué profesiones predominan?
- ¿Qué grupo concentra más jubilados?

Ya no estamos entrenando un modelo.

Estamos interpretando sus resultados.

---

# 17. Heatmaps

Una excelente manera de visualizar estas proporciones consiste en utilizar

```python
sns.heatmap()
```

Los colores más oscuros representan mayores proporciones.

```
Muy oscuro

↓

Muy frecuente
```

```
Muy claro

↓

Poco frecuente
```

Este tipo de gráfico permite detectar patrones casi instantáneamente.

---

# 18. Hard Clustering vs Soft Clustering

Hasta ahora cada observación pertenece únicamente a un cluster.

```
Cliente A

↓

Cluster 2
```

No existen dudas.

Esto recibe el nombre de

**Hard Clustering**.

K-Means pertenece a esta categoría. :contentReference[oaicite:4]{index=4}

---

Ahora imaginemos otro algoritmo.

```
Cliente A

70%

Cluster 1

30%

Cluster 2
```

Esto recibe el nombre de

**Soft Clustering**.

El algoritmo más conocido es

**Gaussian Mixture Models (GMM)**.

---

# 19. Clustering jerárquico

Otra gran familia de algoritmos corresponde al

**Hierarchical Clustering**.

En lugar de mover centroides,

construye un árbol.

```
Todos los datos

│

├───────────┐

│           │

Grupo A   Grupo B

│           │

A1 A2     B1 B2
```

Este árbol recibe el nombre de

**Dendrograma**.

El libro menciona el algoritmo `AgglomerativeClustering` como representante de este enfoque. :contentReference[oaicite:5]{index=5}

---

# 20. ¿Cuándo no utilizar K-Means?

Aunque K-Means es extraordinariamente popular,

no siempre es la mejor opción.

Por ejemplo,

cuando existen grupos alargados.

```
● ● ● ● ● ● ●
```

K-Means intentará dividirlos en varios clusters.

---

Cuando existen densidades muy distintas.

```
●●●●●●

                ●

                   ●
```

También puede producir resultados poco satisfactorios.

---

Cuando existen muchos valores atípicos.

```
● ● ● ●

                     ●
```

Ese único punto puede mover considerablemente el centroide.

---

# Resumen general

En este cuaderno aprendimos que:

- Elegir \(K\) es una decisión del analista.
- La **inercia** mide la compactación de los clusters.
- La inercia siempre disminuye cuando aumenta \(K\).
- El **Elbow Method** busca el punto donde las mejoras comienzan a disminuir.
- El **Silhouette Score** evalúa simultáneamente compactación y separación.
- El verdadero valor del clustering aparece al interpretar las características de cada grupo.
- Existen variantes como el **Soft Clustering** y el **Clustering Jerárquico**.
- K-Means no es adecuado para todos los conjuntos de datos.

Con esto cerramos completamente el estudio de **Clustering**.

A partir del siguiente cuaderno cambiaremos completamente de perspectiva.

Ya no intentaremos **agrupar observaciones**.

Intentaremos responder una pregunta completamente distinta:

> **¿Es posible reducir cientos de variables a unas pocas sin perder demasiada información?**

Esa pregunta nos llevará a uno de los algoritmos más elegantes del Machine Learning:

# **Principal Component Analysis (PCA)**