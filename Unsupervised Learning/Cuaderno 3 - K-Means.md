
# Parte 1 - Comprendiendo el Algoritmo desde Cero

> **Objetivo del cuaderno**
>
> Hasta este momento ya sabemos que cada observación puede representarse como un punto dentro de un espacio de características y que la similitud entre observaciones puede medirse mediante distancias.
>
> Ahora estudiaremos el algoritmo de clustering más famoso del mundo:
>
> **K-Means.**
>
> En este cuaderno no utilizaremos todavía Scikit-Learn. Primero entenderemos completamente la lógica matemática del algoritmo.

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Comprender qué problema intenta resolver K-Means.
- Entender el concepto de cluster.
- Comprender qué es un centroide.
- Seguir paso a paso el algoritmo.
- Comprender por qué K-Means converge.
- Interpretar la función objetivo del algoritmo.
- Comprender las limitaciones del método.

---

# 1. ¿Qué es un Cluster?

La palabra **cluster** significa literalmente

> Grupo

Sin embargo, en Machine Learning posee un significado mucho más específico.

## Definición

Un **cluster** es un conjunto de observaciones que son más similares entre sí que respecto a las observaciones pertenecientes a otros grupos.

La palabra importante aquí es

**similares**.

No significa iguales.

No significa idénticas.

Significa que están relativamente cerca dentro del espacio de características.

---

# 2. Un ejemplo intuitivo

Supongamos el siguiente conjunto de clientes.

```
Ingreso

^

|

|                             ● ● ●

|

|

|      ● ● ●

|

|                 ● ●

+---------------------------------------->

                  Edad
```

Sin realizar ningún cálculo probablemente responderías:

> "Existen tres grupos."

Nuestro cerebro identifica agrupaciones de manera casi instantánea.

Ahora imaginemos que tenemos un millón de clientes.

Resulta imposible hacer esa clasificación manualmente.

Necesitamos un algoritmo.

---

# 3. ¿Qué intenta hacer K-Means?

La idea detrás de K-Means es sorprendentemente sencilla.

En lugar de preguntar

> ¿Cuál es la clase correcta?

pregunta

> ¿Cómo puedo dividir estos puntos en grupos de manera que los puntos de un mismo grupo estén lo más cerca posible entre sí?

Observa que aquí no aparece ninguna etiqueta.

Únicamente aparecen distancias.

---

# 4. Una analogía

Imagina una ciudad.

Debemos construir tres hospitales.

Nuestro objetivo consiste en que cada persona tenga un hospital relativamente cercano.

Una estrategia razonable sería:

1. Elegir tres ubicaciones.
2. Asignar cada ciudadano al hospital más cercano.
3. Reubicar cada hospital hacia el centro de las personas que atiende.
4. Repetir el proceso.

Eso hace exactamente K-Means.

Solo que en lugar de hospitales utiliza **centroides**.

---

# 5. El parámetro K

La letra **K** representa el número de grupos que deseamos construir.

Por ejemplo,

```
K = 2
```

dos grupos.

```
K = 3
```

tres grupos.

```
K = 6
```

seis grupos.

Aquí aparece una de las preguntas más importantes del algoritmo.

> ¿Cómo sabemos cuál es el valor correcto de K?

Más adelante estudiaremos métodos como:

- Elbow Method
- Silhouette Score

Por ahora asumiremos que K ya fue definido.

---

# 6. ¿Qué es un Centroide?

Esta palabra aparece constantemente cuando estudiamos K-Means.

## Definición

Un **centroide** es el punto que representa el centro geométrico de todas las observaciones pertenecientes a un cluster.

No necesariamente corresponde a una observación real.

Puede encontrarse en cualquier posición del espacio.

Por ejemplo.

```
●

      ●

   X

         ●

●
```

La X representa el centroide.

Observa que no coincide con ninguno de los puntos.

Simplemente representa el promedio.

---

# 7. ¿Cómo se calcula un Centroide?

Supongamos cuatro observaciones.

|X|Y|
|---|---|
|2|4|
|3|5|
|4|4|
|3|3|

El centroide se obtiene calculando el promedio de cada variable.

Para la coordenada X,

\[
\bar{x}
=
\frac{2+3+4+3}{4}
=
3
\]

Para la coordenada Y,

\[
\bar{y}
=
\frac{4+5+4+3}{4}
=
4
\]

El centroide será

\[
(3,\;4)
\]

Es decir,

simplemente calculamos la media de cada columna.

Por eso el algoritmo recibe el nombre de

> **K-Means**

porque utiliza la **media (mean)** para representar cada grupo.

---

# 8. La intuición del algoritmo

K-Means sigue una filosofía extremadamente sencilla.

```
Si dos puntos están cerca,

deberían pertenecer al mismo grupo.
```

Y también

```
Si dos puntos están muy lejos,

probablemente pertenezcan a grupos diferentes.
```

Todo el algoritmo consiste en aplicar esta idea repetidamente.

---

# 9. El algoritmo paso a paso

Imaginemos el siguiente conjunto de datos.

```
             ● ●

        ●

                       ●

●

                               ● ●

                           ●
```

Queremos formar

```
K = 2
```

grupos.

---

## Paso 1

Seleccionar aleatoriamente dos centroides.

```
             ● ●

      X

                       ●

●

                               X

                           ●
```

En esta etapa los centroides son completamente aleatorios.

No existe ninguna garantía de que estén bien ubicados.

---

## Paso 2

Calcular la distancia desde cada punto hasta ambos centroides.

Cada observación será asignada al centroide más cercano.

```
Cluster 1

●

●

●

X
```

```
Cluster 2

X

●

●

●
```

Ahora cada punto pertenece temporalmente a un grupo.

---

## Paso 3

Calcular nuevamente el centroide de cada grupo.

```
Antes

X
```

```
Después

     X
```

El centroide se mueve.

¿Por qué?

Porque ahora representa el promedio de todos los puntos asignados.

---

## Paso 4

Volver a calcular las distancias.

Como el centroide cambió de posición,

algunos puntos podrían cambiar de grupo.

---

## Paso 5

Calcular nuevamente los centroides.

---

## Paso 6

Repetir.

Una y otra vez.

Hasta que los centroides prácticamente dejen de moverse.

---

# 10. Visualizando el proceso

Podemos representar el algoritmo de esta manera.

```
Centroides aleatorios

↓

Asignar puntos

↓

Calcular nuevos centroides

↓

Asignar nuevamente

↓

Recalcular centroides

↓

...

↓

Convergencia
```

Este ciclo constituye el corazón de K-Means.

---

# 11. ¿Por qué el algoritmo converge?

Una pregunta muy interesante es la siguiente.

¿Por qué K-Means no continúa moviendo centroides para siempre?

La respuesta se encuentra en su función objetivo.

Cada vez que el algoritmo actualiza los centroides,

la distancia total entre los puntos y su centroide disminuye.

Nunca aumenta.

Por lo tanto,

después de suficientes iteraciones,

llega un momento en el cual ya no puede mejorar.

Entonces se detiene.

---

# 12. La función objetivo

El objetivo matemático de K-Means consiste en minimizar

$$\sum_{i=1}^{n} \left| x_i-\mu_k \right|^2$$

Esta expresión puede parecer complicada.

Analicémosla.

---

$$x_i$$

Representa una observación.

Es decir,

un punto del dataset.

---

$$\mu_k$$

Representa el centroide del cluster al cual pertenece ese punto.

---

$$(x_i-\mu_k)$$

Representa la distancia entre el punto y su centroide.

---

$$\left|x_i-\mu_k\right|^2$$

Representa la distancia Euclidiana al cuadrado.

---

## La suma

Finalmente,

sumamos las distancias de todos los puntos.

El algoritmo intenta hacer esa suma tan pequeña como sea posible.

En otras palabras,

quiere que todos los puntos estén lo más cerca posible de su centroide.

---

# 13. Una interpretación intuitiva

Imagina nuevamente un hospital.

Cada paciente debe desplazarse hasta el hospital asignado.

La función objetivo mide

> **la distancia total recorrida por todos los pacientes.**

Mover el hospital hacia el centro de los pacientes reduce esa distancia.

Exactamente eso ocurre con los centroides.

---

# 14. ¿Siempre encuentra la mejor solución?

No.

Y este es uno de los aspectos más importantes del algoritmo.

K-Means encuentra un

> **mínimo local**

pero no necesariamente el mínimo global.

Todo depende de la posición inicial de los centroides.

Observa.

```
Inicialización A

↓

Excelente solución
```

```
Inicialización B

↓

Solución mediocre
```

El mismo conjunto de datos puede producir resultados distintos.

Por esa razón suele ejecutarse K-Means varias veces con diferentes inicializaciones.

Posteriormente se conserva la solución con menor error.

Más adelante veremos que **k-means++** fue desarrollado precisamente para reducir este problema, escogiendo centroides iniciales mejor distribuidos. :contentReference[oaicite:0]{index=0}

---

# 15. Ventajas de K-Means

- Muy fácil de comprender.
- Muy rápido incluso con millones de observaciones.
- Escala bastante bien.
- Produce resultados fáciles de interpretar.
- Constituye la base para comprender otros algoritmos más avanzados.

---

# 16. Limitaciones

K-Means también presenta varias limitaciones importantes.

- Debemos especificar K antes del entrenamiento.
- Es sensible a la inicialización.
- Es sensible a valores atípicos.
- Supone clusters aproximadamente esféricos.
- No funciona bien cuando los grupos poseen densidades muy diferentes.
- Requiere variables estandarizadas.

Durante el resto del capítulo iremos viendo cómo afrontar cada una de estas limitaciones.

---

# 17. Resumen

En este cuaderno estudiamos la lógica completa detrás de K-Means.

Aprendimos que:

- Un cluster es un grupo de observaciones similares.
- Cada cluster está representado por un centroide.
- El centroide corresponde al promedio de todas las observaciones del grupo.
- El algoritmo alterna entre asignar observaciones y recalcular centroides.
- Este proceso continúa hasta que los centroides dejan de cambiar.
- Matemáticamente, K-Means minimiza la suma de las distancias cuadradas entre los puntos y sus centroides.

En la **Parte 2** implementaremos todo este proceso utilizando **Scikit-Learn**, analizando línea por línea el código del libro y comprendiendo qué ocurre internamente en cada instrucción.

---
# Parte 2 - Implementación Completa en Scikit-Learn

> **Objetivo del cuaderno**
>
> En la primera parte comprendimos completamente la lógica matemática de K-Means.
>
> Ahora veremos cómo implementar el algoritmo utilizando **Scikit-Learn**, pero, más importante aún, entenderemos exactamente qué ocurre dentro de cada línea de código.
>
> El objetivo no es memorizar instrucciones, sino comprender qué está haciendo el algoritmo internamente.

---

# Objetivos de aprendizaje

Al finalizar esta sección serás capaz de:

- Crear conjuntos de datos para clustering.
- Comprender el uso de `make_blobs()`.
- Construir un modelo `KMeans`.
- Comprender `fit()`, `predict()` y `fit_predict()`.
- Comprender los atributos más importantes del modelo.
- Visualizar correctamente los clusters.
- Interpretar el código del libro línea por línea.

---

# 18. Generando datos de ejemplo

Antes de utilizar datos reales resulta conveniente practicar con un conjunto de datos sencillo.

Scikit-Learn proporciona una función llamada

```python
make_blobs()
```

que genera automáticamente grupos de puntos distribuidos normalmente alrededor de varios centros. :contentReference[oaicite:0]{index=0}

Podemos imaginarla como un "fabricante de clusters".

---

## ¿Qué hace realmente?

Supongamos que queremos crear tres grupos de clientes ficticios.

```
Grupo A

● ● ● ●

Grupo B

● ● ● ●

Grupo C

● ● ● ●
```

En lugar de escribir cientos de coordenadas manualmente,

`make_blobs()` las genera automáticamente.

Esto resulta extremadamente útil para estudiar algoritmos de clustering.

---

# 19. Importando las librerías

```python
from sklearn.datasets import make_blobs
from sklearn.cluster import KMeans
```

La primera línea permite generar datos.

La segunda contiene la implementación del algoritmo K-Means.

---

# 20. Creando los datos

```python
X, _ = make_blobs(random_state=10)
```

Esta línea parece sencilla, pero contiene varias ideas importantes.

---

## ¿Qué devuelve make_blobs()?

Devuelve dos objetos.

```python
X, y = make_blobs(...)
```

donde

- **X** contiene las coordenadas.
- **y** contiene el grupo verdadero utilizado para generar los datos.

Por ejemplo,

```
X

(2.1,4.5)

(2.3,4.7)

(8.4,1.2)
```

```
y

0

0

1
```

---

## ¿Por qué el libro utiliza "_"?

Observa nuevamente.

```python
X, _ = make_blobs(random_state=10)
```

El símbolo

```python
_
```

significa

> "Estoy ignorando este valor."

Como estamos aprendiendo clustering,

no queremos utilizar las etiquetas verdaderas.

Precisamente esa es la esencia del aprendizaje no supervisado.

Solo utilizaremos

```python
X
```

---

# 21. Visualizando los datos

El libro utiliza

```python
plt.scatter(X[:,0], X[:,1], color="black")
```

Analicemos cuidadosamente esta línea.

---

## ¿Qué representa X?

Recordemos.

Cada fila representa una observación.

Cada columna representa una característica.

```
X

Edad     Ingreso

23        800

25        900

48       6000
```

En realidad,

NumPy almacena

```
[[23,800],
 [25,900],
 [48,6000]]
```

---

## ¿Qué significa X[:,0]?

Esta expresión suele confundir mucho al principio.

La sintaxis general es

```
matriz[fila,columna]
```

El símbolo

```
:
```

significa

> Todas las filas.

Entonces

```python
X[:,0]
```

equivale a decir

> Dame la primera columna completa.

Visualmente,

```
Edad    Ingreso

23      800

25      900

48      6000
```

↓

```
23

25

48
```

---

## ¿Y X[:,1]?

Exactamente igual.

```
Edad    Ingreso

23      800

25      900

48      6000
```

↓

```
800

900

6000
```

---

## Entonces...

```python
plt.scatter(
X[:,0],
X[:,1]
)
```

significa simplemente

```
Eje X

↓

Primera variable
```

```
Eje Y

↓

Segunda variable
```

Por eso obtenemos una nube de puntos. :contentReference[oaicite:1]{index=1}

---

# 22. Construyendo el modelo

El siguiente paso consiste en crear el algoritmo.

```python
kmeans = KMeans(
    init="random",
    n_clusters=3
)
```

Veamos cada parámetro.

---

## n_clusters

Es el valor de

\[
K
\]

Es decir,

el número de grupos que queremos construir.

---

## init

Este parámetro indica cómo inicializar los centroides.

En el libro aparece

```python
init="random"
```

Esto significa que los centroides iniciales serán seleccionados aleatoriamente. :contentReference[oaicite:2]{index=2}

Más adelante veremos que normalmente resulta preferible utilizar

```python
k-means++
```

---

# 23. Entrenando el modelo

Una vez construido el objeto,

ejecutamos

```python
kmeans.fit(X)
```

Esta línea realiza todo el algoritmo.

Internamente ocurre lo siguiente.

```
Centroides aleatorios

↓

Calcular distancias

↓

Asignar puntos

↓

Recalcular centroides

↓

Repetir

↓

Convergencia
```

Todo este proceso ocurre automáticamente.

---

# 24. ¿Qué hace fit()?

Muchas personas creen que

```python
fit()
```

simplemente "entrena".

En realidad hace mucho más.

Durante la ejecución,

Scikit-Learn calcula

- centroides
- asignaciones
- iteraciones
- función objetivo
- convergencia

Cuando termina,

el objeto

```python
kmeans
```

ya contiene toda esa información.

---

# 25. Predicción

Después del entrenamiento podemos preguntar

```python
kmeans.predict(X)
```

La respuesta será algo parecido a

```
0

0

2

1

1

2
```

Estos números representan el cluster asignado a cada observación.

Es importante comprender que

```
Cluster 0
```

no significa

"mejor"

ni

"peor"

Simplemente es un identificador.

Podría haberse llamado

```
Rojo

Azul

Verde
```

y el resultado sería exactamente el mismo.

---

# 26. fit_predict()

El libro utiliza

```python
y_pred = kmeans.fit_predict(X)
```

Esta instrucción combina dos pasos.

```python
fit()

↓

predict()
```

Todo ocurre en una sola llamada. :contentReference[oaicite:3]{index=3}

---

# 27. ¿Por qué existen ambas opciones?

Ambas producen prácticamente el mismo resultado.

Sin embargo,

cuando deseamos reutilizar el modelo posteriormente,

es más común utilizar

```python
fit()

↓

predict()
```

por separado.

Esto hace el código más claro y facilita guardar el modelo entrenado.

---

# 28. Visualizando los clusters

Después del entrenamiento queremos observar el resultado.

El libro utiliza Pandas para construir un DataFrame.

```python
merge_data = pd.concat([
    pd.DataFrame(X[:,0]),
    pd.DataFrame(X[:,1]),
    pd.DataFrame(y_pred)
], axis=1)
```

Veamos qué ocurre.

---

## Primer DataFrame

```python
pd.DataFrame(X[:,0])
```

Contiene

```
Edad
```

---

## Segundo DataFrame

```python
pd.DataFrame(X[:,1])
```

Contiene

```
Ingreso
```

---

## Tercer DataFrame

```python
pd.DataFrame(y_pred)
```

Contiene

```
Cluster
```

---

## concat()

Finalmente,

```python
concat(axis=1)
```

los une horizontalmente.

Obtenemos

|Feature1|Feature2|Cluster|
|----------|-----------|-----------|
|2.3|5.4|0|
|2.1|5.2|0|
|7.8|1.5|2|

Ahora cada observación conoce el grupo al cual pertenece. :contentReference[oaicite:4]{index=4}

---

# 29. Dibujando cada grupo

El libro utiliza el siguiente código.

```python
ax = None

colors = [
    "blue",
    "red",
    "green"
]

for i, data in merge_data.groupby("cluster"):

    ax = data.plot.scatter(
        x="feature1",
        y="feature2",
        color=colors[i],
        label=f"cluster{i}",
        ax=ax
    )
```

Este fragmento suele generar muchas dudas.

Analicémoslo paso por paso.

---

# 30. groupby()

Recordemos.

Tenemos

|Feature1|Feature2|Cluster|
|---------|---------|--------|
|2|4|0|
|3|5|0|
|8|1|1|
|9|2|1|

Cuando ejecutamos

```python
groupby("cluster")
```

Pandas crea dos grupos.

Grupo 0

|Feature1|Feature2|
|---------|---------|
|2|4|
|3|5|

Grupo 1

|Feature1|Feature2|
|---------|---------|
|8|1|
|9|2|

---

# 31. El ciclo for

Cada iteración trabaja con un grupo.

Primera iteración.

```
i = 0
```

```
data

↓

Cluster 0
```

Segunda iteración.

```
i = 1
```

```
data

↓

Cluster 1
```

Y así sucesivamente.

---

# 32. ¿Por qué aparece ax = None?

Esta es probablemente la línea más confusa para quienes comienzan.

```
ax = None
```

simplemente significa

> Todavía no existe ningún gráfico.

Cuando se dibuja el primer grupo,

Matplotlib crea un gráfico.

Ese gráfico es almacenado dentro de

```python
ax
```

En la siguiente iteración,

ya no queremos crear otro gráfico.

Queremos dibujar sobre el mismo.

Por eso hacemos

```python
ax=ax
```

Es decir,

> Utiliza el gráfico que ya existe.

Gracias a esto,

todos los clusters aparecen en una única figura.

---

# 33. ¿Qué aprendimos?

Ahora ya entendemos completamente el código del libro.

Cada línea tiene un propósito específico.

Ya no estamos copiando código.

Estamos comprendiendo exactamente qué ocurre detrás de cada instrucción.

---

# Resumen

En esta segunda parte aprendimos que:

- `make_blobs()` genera conjuntos de datos ideales para practicar clustering.
- `KMeans` implementa el algoritmo estudiado en la Parte 1.
- `fit()` ejecuta todo el proceso de optimización.
- `predict()` asigna un cluster a cada observación.
- `fit_predict()` combina ambos pasos.
- `concat()` une las variables y los clusters en un solo DataFrame.
- `groupby()` separa automáticamente las observaciones por cluster.
- `ax=None` y `ax=ax` permiten dibujar todos los grupos sobre una misma figura.

Con esto ya dominamos la implementación básica de **K-Means**. En el siguiente cuaderno estudiaremos uno de los problemas más importantes del algoritmo:

> **¿Cómo elegir correctamente el número de clusters (\(K\))?**

Analizaremos el **Elbow Method**, la **inercia (`inertia_`)**, el **Silhouette Score** y los criterios prácticos para seleccionar el valor más adecuado de \(K\).