
# Parte 1 - Fundamentos Matemáticos e Intuición

> **Objetivo del cuaderno**
>
> Hasta este momento hemos estudiado algoritmos cuyo objetivo consiste en **agrupar observaciones similares**.
>
> Ahora cambiaremos completamente de perspectiva.
>
> En lugar de preguntar:
>
> > ¿Qué observaciones son parecidas?
>
> nos preguntaremos:
>
> > **¿Es posible describir un conjunto de datos utilizando menos variables sin perder demasiada información?**
>
> Esa es precisamente la idea detrás del **Principal Component Analysis (PCA)**, uno de los algoritmos más importantes de todo el Machine Learning.

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Comprender el problema de la alta dimensionalidad.
- Entender por qué demasiadas variables pueden dificultar el análisis.
- Comprender qué significa reducir dimensiones.
- Entender intuitivamente qué hace PCA.
- Comprender el concepto de varianza.
- Comprender la covarianza.
- Comprender la matriz de covarianza.
- Entender qué son los componentes principales.
- Comprender por qué PCA encuentra nuevas direcciones en el espacio.

---

# 1. El problema de la alta dimensionalidad

Imaginemos un dataset muy pequeño.

|Edad|Ingreso|
|------|---------|
|23|900|
|28|1100|
|35|1800|

Resulta sencillo visualizarlo.

```
Ingreso

^

|

|

|

+---------------------------->

          Edad
```

Tenemos únicamente dos variables.

Ahora imaginemos otro dataset.

|Edad|
|Ingreso|
|Años de experiencia|
|Número de hijos|
|Saldo bancario|
|Cantidad de compras|
|Tiempo como cliente|
|Nivel educativo|
|...

Supongamos que contiene

100 variables.

¿Cómo podríamos visualizarlo?

La respuesta es sencilla.

No podemos.

---

# 2. ¿Qué significa una dimensión?

En Machine Learning,

cada variable representa una dimensión.

Recordemos el cuaderno anterior.

```
Edad

↓

Una dimensión
```

```
Edad

Ingreso

↓

Dos dimensiones
```

```
Edad

Ingreso

Compras

↓

Tres dimensiones
```

En general,

```
n variables

↓

Espacio de n dimensiones
```

Mientras mayor sea el número de variables,

más complejo será ese espacio.

---

# 3. ¿Por qué muchas dimensiones son un problema?

Podría parecer que

> Más información siempre es mejor.

En realidad,

no necesariamente.

Muchas variables producen varios inconvenientes.

---

## Mayor costo computacional

Más variables significan

- más memoria
- más operaciones
- mayor tiempo de entrenamiento

---

## Más ruido

No todas las variables contienen información útil.

Muchas únicamente agregan ruido.

---

## Variables redundantes

Con frecuencia dos variables contienen prácticamente la misma información.

Por ejemplo,

|Altura (cm)|Altura (m)|
|------------|-----------|
|180|1.80|

Claramente no necesitamos ambas.

Una puede obtenerse perfectamente a partir de la otra.

---

# 4. La redundancia de información

Observemos otro ejemplo.

|Edad|Años trabajados|
|------|----------------|
|25|3|
|35|13|
|45|23|
|55|33|

Estas variables están fuertemente relacionadas.

Si conocemos la edad,

podemos estimar aproximadamente los años trabajados.

Las dos variables contienen información muy parecida.

Entonces surge una pregunta.

> ¿Podemos reemplazar ambas variables por una sola?

PCA intenta responder precisamente esa pregunta.

---

# 5. Una analogía

Imagina una biblioteca.

Posee dos libros.

Libro A

```
Historia Universal
```

Libro B

```
Resumen de Historia Universal
```

El segundo libro contiene casi toda la información del primero,

pero ocupa mucho menos espacio.

PCA hace exactamente eso.

Construye un "resumen matemático" del conjunto de datos.

---

# 6. ¿Qué significa reducir dimensiones?

Supongamos un conjunto de datos con

100 variables.

PCA intentará transformarlo en

20 variables.

O quizá

10 variables.

O incluso

2 variables.

Pero aquí aparece una pregunta muy importante.

> ¿Cómo puede eliminar variables sin perder información?

La respuesta es:

No elimina información al azar.

Busca nuevas variables que concentren la mayor cantidad posible de información.

---

# 7. Una idea completamente nueva

Hasta ahora nuestras variables eran

```
Edad

Ingreso

Compras
```

PCA propone algo radicalmente distinto.

Crear nuevas variables.

Por ejemplo,

```
Componente Principal 1

Componente Principal 2

Componente Principal 3
```

Estas nuevas variables no existían originalmente.

Son construidas matemáticamente.

---

# 8. ¿Cómo crea esas nuevas variables?

Imaginemos el siguiente conjunto de puntos.

```
                    ●

                ●

            ●

        ●

    ●
```

Nuestro cerebro observa inmediatamente una dirección predominante.

Todos los puntos parecen alinearse.

PCA detecta exactamente esa dirección.

La convierte en una nueva variable.

Esa nueva variable recibe el nombre de

**Primer Componente Principal.**

---

# 9. La intuición correcta

Muchos libros dicen

> PCA reduce dimensiones.

Eso es cierto.

Pero incompleto.

Una mejor descripción sería

> PCA encuentra una nueva forma de describir los datos utilizando ejes mejor orientados.

Observa la diferencia.

No elimina información.

Simplemente gira el sistema de coordenadas.

---

# 10. Un ejemplo visual

Supongamos estos datos.

```
Y

^

|

|                 ●

|              ●

|           ●

|        ●

|     ●

+------------------------------>

              X
```

Los ejes originales son

```
X

Y
```

Pero claramente los datos no siguen esos ejes.

Siguen otra dirección.

PCA intenta encontrar ese nuevo eje.

```
          /

        /

      /

    /

  /
```

Ese será el primer componente principal.

---

# 11. ¿Por qué esa dirección?

Porque contiene la mayor cantidad posible de información.

Y aquí aparece un concepto nuevo.

La información será medida mediante

> **la varianza.**

---

# 12. ¿Qué es la varianza?

La varianza mide

> **qué tanto se dispersan los datos respecto a su promedio.**

Observemos dos conjuntos.

Conjunto A.

```
●●●●●
```

Todos los puntos están muy juntos.

La varianza es pequeña.

---

Conjunto B.

```
●         ●

      ●

                ●

   ●
```

Los puntos están muy dispersos.

La varianza es mucho mayor.

---

# 13. ¿Por qué la varianza representa información?

Esta es una de las ideas más importantes de PCA.

Imaginemos una variable.

```
Edad

35

35

35

35

35
```

¿Nos aporta mucha información?

No.

Todos los valores son iguales.

Ahora observemos otra.

```
Edad

18

25

42

67

81
```

Aquí sí existe mucha información.

¿Por qué?

Porque los datos presentan mucha variabilidad.

PCA parte precisamente de esta idea.

> **Una dirección con mayor varianza contiene más información.**

---

# 14. El objetivo de PCA

Ahora podemos expresar el objetivo del algoritmo.

PCA busca

> **la dirección en la cual los datos presentan la mayor varianza posible.**

No busca grupos.

No busca clases.

No busca centroides.

Busca direcciones.

---

# 15. El Primer Componente Principal

El primer componente principal es

> **la dirección donde la varianza de los datos es máxima.**

Visualmente.

```
●

    ●

        ●

            ●

                ●
```

El primer componente coincide aproximadamente con la dirección de esa nube de puntos.

---

# 16. ¿Y el Segundo Componente?

Después de encontrar la primera dirección,

PCA busca una segunda.

Pero existe una condición.

Debe ser perpendicular a la primera.

Es decir,

ambos componentes deben ser ortogonales.

```
          PC2

           |

           |

-----------+-----------

          /

        /

      /

    PC1
```

Esta propiedad garantiza que cada componente aporte información diferente.

---

# 17. ¿Por qué deben ser ortogonales?

Imaginemos dos ejes casi paralelos.

Ambos describirían prácticamente la misma información.

Eso sería redundante.

PCA evita esa redundancia obligando a que los componentes sean perpendiculares.

De esa manera,

cada nuevo componente explica una parte distinta de la variabilidad.

---

# 18. ¿Qué ocurre después?

Una vez encontrados los nuevos ejes,

cada observación es proyectada sobre ellos.

Ya no describimos una persona mediante

```
Edad

Ingreso

Compras
```

Ahora la describimos mediante

```
PC1

PC2

PC3
```

Es decir,

hemos cambiado completamente el sistema de coordenadas.

---

# 19. Una analogía muy útil

Imagina una fotografía de una carretera.

```
**********************
**********************
**********************
```

La carretera es muy larga.

Pero bastante estrecha.

¿Necesitamos dos dimensiones para describirla?

No realmente.

Podríamos recorrer casi toda la carretera utilizando únicamente una línea central.

Eso hace PCA.

Busca esa línea central.

Y utiliza esa dirección para describir la mayor parte de la información.

---

# 20. Lo que aprenderemos después

Hasta ahora todo ha sido intuición.

En la segunda parte responderemos preguntas como:

- ¿Cómo encuentra PCA esa dirección?
- ¿Qué es una matriz de covarianza?
- ¿Qué son los eigenvectores?
- ¿Qué representan los eigenvalores?
- ¿Qué hace realmente `fit()`?
- ¿Qué significa `components_`?
- ¿Cómo interpretar `explained_variance_`?
- ¿Cómo decidir cuántos componentes conservar?

Allí entraremos en la implementación completa utilizando Scikit-Learn y analizaremos línea por línea el código presentado en el libro.

# Cuaderno 5 - Principal Component Analysis (PCA)
# Parte 2 - Implementación Completa en Scikit-Learn e Interpretación Matemática

> **Objetivo del cuaderno**
>
> En la primera parte comprendimos la intuición detrás de PCA.
>
> Sabemos que PCA busca nuevas direcciones donde los datos presentan la mayor variabilidad posible.
>
> En esta segunda parte veremos cómo implementar PCA utilizando Scikit-Learn y, más importante aún, comprenderemos qué significa realmente cada uno de sus atributos.
>
> El objetivo no será memorizar código, sino interpretar correctamente los resultados.

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Implementar PCA utilizando Scikit-Learn.
- Comprender el papel de la estandarización.
- Entender qué calcula `fit()`.
- Comprender qué representan los eigenvectores.
- Interpretar `components_`.
- Interpretar `explained_variance_`.
- Interpretar `explained_variance_ratio_`.
- Comprender la transformación de datos mediante `transform()`.
- Interpretar correctamente una proyección PCA.
- Comprender cuándo utilizar PCA.

---

# 21. ¿Por qué debemos estandarizar?

Antes de aplicar PCA, prácticamente siempre realizamos una estandarización.

El libro utiliza:

```python
from sklearn.preprocessing import StandardScaler

sc = StandardScaler()

X_std = sc.fit_transform(X)
```

antes de ejecutar PCA. :contentReference[oaicite:0]{index=0}

La razón es exactamente la misma que estudiamos con K-Means.

Supongamos dos variables.

|Edad|Ingreso|
|------|---------|
|25|900|
|35|2500|
|60|9000|

La variable ingreso posee valores mucho mayores.

Si no estandarizamos,

la dirección principal encontrada por PCA estará dominada casi exclusivamente por el ingreso.

No porque sea más importante.

Sino porque sus números son mayores.

---

# 22. ¿Qué hace StandardScaler?

Recordemos.

Después de aplicar

```python
StandardScaler()
```

cada variable tendrá

Media

\[
0
\]

Varianza

\[
1
\]

Eso significa que todas las variables comienzan el análisis en igualdad de condiciones.

---

# 23. Construyendo el modelo PCA

La implementación resulta muy sencilla.

```python
from sklearn.decomposition import PCA

pca = PCA(
    n_components=2
)
```

Observemos el parámetro más importante.

---

## n_components

Indica cuántos componentes principales deseamos conservar.

Por ejemplo,

```python
PCA(n_components=2)
```

significa

> Quiero representar todo el conjunto de datos utilizando únicamente dos nuevas variables.

No significa seleccionar dos variables originales.

Significa construir dos variables completamente nuevas.

El libro utiliza este mismo parámetro al crear el objeto `PCA`. :contentReference[oaicite:1]{index=1}

---

# 24. Entrenando PCA

Una vez construido el objeto,

ejecutamos

```python
pca.fit(X_std)
```

¿Qué ocurre internamente?

Aunque la llamada parezca sencilla,

el algoritmo realiza bastante trabajo.

Internamente:

```
Datos

↓

Calcular medias

↓

Calcular matriz de covarianza

↓

Calcular eigenvalores

↓

Calcular eigenvectores

↓

Ordenarlos

↓

Guardar resultados
```

Todo esto ocurre automáticamente.

---

# 25. ¿Qué aprende realmente PCA?

Cuando ejecutamos

```python
fit()
```

PCA aprende únicamente una cosa.

Las nuevas direcciones del espacio.

Es decir,

aprende cómo debemos girar los ejes.

No transforma todavía los datos.

Simplemente aprende la orientación correcta.

---

# 26. components_

Después del entrenamiento,

el atributo más importante es

```python
pca.components_
```

El libro obtiene un resultado similar a

```python
[[ 0.707  0.707]
 [ 0.707 -0.707]]
```

:contentReference[oaicite:2]{index=2}

Muchos estudiantes creen que estos números no significan nada.

En realidad significan muchísimo.

---

# 27. ¿Qué representan estos números?

Cada fila representa un componente principal.

```
Primera fila

↓

PC1
```

```
Segunda fila

↓

PC2
```

Cada columna representa una variable original.

Por ejemplo,

si el dataset posee

```
Edad

Ingreso
```

entonces

```
PC1

↓

0.70 Edad

+

0.70 Ingreso
```

Es decir,

el primer componente principal es una combinación lineal de las variables originales.

---

# 28. ¿Qué es un eigenvector?

Aquí aparece uno de los conceptos más famosos del álgebra lineal.

Un eigenvector representa

> **una dirección especial del espacio.**

PCA busca precisamente esas direcciones especiales.

Los eigenvectores son los nuevos ejes del sistema de coordenadas.

Por eso

```python
components_
```

contiene precisamente los eigenvectores.

---

# 29. explained_variance_

Otro atributo muy importante es

```python
pca.explained_variance_
```

El libro obtiene aproximadamente

```python
[1.889 0.111]
```

:contentReference[oaicite:3]{index=3}

¿Qué significa?

Cada número representa la varianza capturada por cada componente principal.

Recordemos.

Mayor varianza

↓

Mayor información.

Entonces,

el primer componente contiene mucha más información que el segundo.

---

# 30. explained_variance_ratio_

Probablemente sea el atributo más utilizado en la práctica.

```python
pca.explained_variance_ratio_
```

En el ejemplo del libro aparece aproximadamente

```python
[0.945 0.055]
```

:contentReference[oaicite:4]{index=4}

Esto significa

```
PC1

↓

94.5 %

de toda la información
```

```
PC2

↓

5.5 %
```

La suma siempre será

```
100 %
```

si conservamos todos los componentes.

---

# 31. ¿Cómo interpretar estos porcentajes?

Supongamos un dataset con

50 variables.

Aplicamos PCA.

Obtenemos

```python
explained_variance_ratio_

[0.52,
0.18,
0.11,
0.07,
...]
```

Interpretación.

Primer componente

↓

52 %

Segundo componente

↓

18 %

Acumulado

↓

70 %

Eso significa que únicamente dos componentes explican el 70 % de toda la información del dataset.

Es un resultado excelente.

---

# 32. transform()

Hasta ahora PCA solamente aprendió los nuevos ejes.

Ahora debemos proyectar los datos.

Para ello utilizamos

```python
X_pca = pca.transform(X_std)
```

¿Qué hace esta instrucción?

Cada observación cambia de coordenadas.

Antes

```
Edad

Ingreso
```

Después

```
PC1

PC2
```

Los datos siguen siendo los mismos.

Lo único que cambió fue el sistema de referencia.

---

# 33. Una analogía

Imagina un mapa.

Podemos describir una ciudad mediante

```
Latitud

Longitud
```

Pero también podríamos utilizar otro sistema de coordenadas.

La ciudad no cambia.

Solo cambian los ejes utilizados para describirla.

Eso hace exactamente

```python
transform()
```

---

# 34. fit_transform()

Como en muchos algoritmos de Scikit-Learn,

existe un atajo.

```python
X_pca = pca.fit_transform(X_std)
```

Equivale a

```python
fit()

↓

transform()
```

en una sola instrucción.

---

# 35. PCA aplicado al dataset de cáncer

El libro utiliza posteriormente el famoso conjunto de datos

```python
load_breast_cancer()
```

Después de estandarizar las 30 variables,

aplica

```python
PCA(n_components=2)
```

y obtiene

```python
Explained variance ratio

[0.443,
0.190]
```

:contentReference[oaicite:5]{index=5}

¿Qué significa?

Los dos primeros componentes contienen aproximadamente

44.3 %

+

19.0 %

=

63.3 %

de toda la información del conjunto de datos.

Con únicamente dos variables,

hemos conservado casi dos tercios de la información original.

---

# 36. Visualizando los datos

Después del

```python
transform()
```

cada paciente queda representado por

```
PC1

PC2
```

Ahora podemos construir un gráfico bidimensional.

El libro muestra que,

aunque originalmente existían 30 variables,

los pacientes pueden visualizarse en dos dimensiones y las clases (benigno y maligno) quedan relativamente separadas. :contentReference[oaicite:6]{index=6}

Este es uno de los usos más importantes de PCA.

La visualización.

---

# 37. ¿Cuántos componentes debemos conservar?

No existe una respuesta universal.

Sin embargo,

existen varias estrategias.

---

## Regla 1

Conservar suficientes componentes para explicar

95 %

de la varianza.

---

## Regla 2

Utilizar un Scree Plot.

Este gráfico muestra

```
Componente

↓

Varianza explicada
```

Buscamos nuevamente un "codo",

muy parecido al Elbow Method estudiado para K-Means.

---

## Regla 3

Utilizar el conocimiento del dominio.

En algunas aplicaciones

80 %

es suficiente.

En otras,

necesitamos

99 %.

Todo depende del problema.

---

# 38. ¿Cuándo utilizar PCA?

PCA resulta especialmente útil cuando:

- existen muchas variables;
- muchas variables están correlacionadas;
- queremos visualizar datos de alta dimensionalidad;
- deseamos reducir ruido;
- queremos acelerar otros algoritmos.

Por ejemplo,

es común aplicar PCA antes de entrenar

- Regresión Logística.
- SVM.
- Redes Neuronales.
- K-Means.

---

# 39. ¿Cuándo NO utilizar PCA?

PCA no siempre resulta conveniente.

Por ejemplo,

cuando necesitamos interpretar las variables originales.

Después de PCA,

las nuevas variables

```
PC1

PC2
```

ya no representan

Edad,

Ingreso

o

Compras.

Representan combinaciones matemáticas de todas ellas.

También debemos recordar que PCA únicamente captura relaciones lineales.

Si la estructura del problema es altamente no lineal,

otros métodos como

Kernel PCA,

t-SNE

o

UMAP

pueden producir mejores resultados.

---

# 40. Resumen general de PCA

En este cuaderno aprendimos que:

- PCA busca nuevas direcciones del espacio donde la varianza sea máxima.
- Antes de aplicar PCA debemos estandarizar las variables.
- `fit()` aprende los nuevos ejes del espacio.
- `components_` contiene los eigenvectores.
- `explained_variance_` indica cuánta varianza captura cada componente.
- `explained_variance_ratio_` indica qué porcentaje de la información conserva cada componente.
- `transform()` proyecta las observaciones sobre los nuevos ejes.
- PCA permite reducir la dimensionalidad preservando la mayor cantidad posible de información.
- Es una herramienta fundamental para exploración, visualización y preprocesamiento de datos.

Con esto concluimos el estudio de **Principal Component Analysis (PCA)**.

En el siguiente cuaderno cambiaremos nuevamente de paradigma.

Ya no analizaremos similitud entre observaciones ni reducción de dimensionalidad.

Estudiaremos cómo descubrir relaciones frecuentes entre elementos, respondiendo preguntas como:

> **¿Qué productos suelen comprarse juntos?**

Ese será el inicio del estudio de **Market Basket Analysis** y las **Association Rules**.