
# Parte 1 - Fundamentos del Aprendizaje No Supervisado

> **Capítulo basado en la lectura "Unsupervised Learning".**
>
> Este cuaderno profundiza considerablemente los conceptos presentados en el libro. El objetivo no es únicamente aprender a utilizar Scikit-Learn, sino comprender la intuición matemática y geométrica detrás del aprendizaje no supervisado. :contentReference[oaicite:0]{index=0}

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Comprender qué es el aprendizaje no supervisado.
- Diferenciar claramente entre aprendizaje supervisado y no supervisado.
- Entender por qué existen algoritmos que no necesitan etiquetas.
- Comprender el concepto de estructura oculta en los datos.
- Interpretar el papel de la geometría dentro del aprendizaje no supervisado.
- Identificar los principales problemas que resuelve este paradigma.
- Comprender la diferencia entre descubrir conocimiento y realizar predicciones.

---

# 1. Introducción

Hasta este momento, todos los algoritmos que hemos estudiado tenían algo en común.

Existía una respuesta correcta.

Por ejemplo, en una regresión lineal intentábamos responder preguntas como:

> ¿Cuál será el precio de esta casa?

En clasificación respondíamos preguntas como:

> ¿Este correo es spam?

> ¿Este paciente tiene cáncer?

> ¿Este cliente abandonará el banco?

En todos estos casos el algoritmo conocía previamente la respuesta correcta durante el entrenamiento.

Esa respuesta recibe distintos nombres:

- Variable objetivo
- Variable dependiente
- Etiqueta (*label*)
- Target

Toda la información utilizada por el algoritmo puede representarse de la siguiente manera:

\[
(X,y)
\]

donde

- \(X\) representa las variables de entrada (*features*)
- \(y\) representa la respuesta correcta.

El objetivo del algoritmo consiste en aprender una función

\[
f(X)=y
\]

capaz de aproximar la relación entre ambas variables.

---

Ahora imaginemos un escenario completamente distinto.

Una empresa posee información de diez millones de clientes.

Para cada cliente conoce:

- Edad
- Sexo
- Ciudad
- Ingresos
- Número de compras
- Tiempo como cliente
- Productos adquiridos
- Número de visitas
- Frecuencia de compra
- Saldo promedio

Sin embargo, el director de mercadeo realiza la siguiente pregunta:

> **¿Qué tipos de clientes tenemos?**

Observa cuidadosamente esta pregunta.

No está preguntando quién comprará.

No está preguntando quién abandonará la empresa.

No está preguntando cuánto dinero gastará.

Está preguntando algo completamente diferente.

Quiere descubrir conocimiento que actualmente nadie conoce.

Aquí aparece el aprendizaje no supervisado. :contentReference[oaicite:1]{index=1}

---

# 2. ¿Qué significa aprender?

Cuando escuchamos la expresión

> "El algoritmo aprende"

es fácil imaginar algo parecido al cerebro humano.

Sin embargo, desde el punto de vista matemático, aprender significa algo mucho más específico.

## Definición

Aprender consiste en encontrar una estructura matemática capaz de explicar los datos.

Observa que esta definición no menciona inteligencia.

Tampoco menciona conciencia.

Mucho menos razonamiento.

Simplemente habla de encontrar una estructura.

Dependiendo del algoritmo, esa estructura puede ser:

- una recta
- una curva
- un árbol
- un hiperplano
- varios grupos
- una distribución de probabilidades
- nuevas variables
- reglas de asociación

En otras palabras,

> **Machine Learning consiste en descubrir estructuras ocultas dentro de los datos.**

---

# 3. Los tres grandes paradigmas del Machine Learning

La mayoría de los algoritmos de Machine Learning pertenecen a uno de tres grandes grupos.

```
Machine Learning
│
├── Supervised Learning
│
├── Unsupervised Learning
│
└── Reinforcement Learning
```

En este curso nos concentraremos en los dos primeros.

---

## 3.1 Supervised Learning

Disponemos de datos de entrada y de una respuesta conocida.

```
Edad
Ingreso
Ciudad
Experiencia

↓

¿Compra?

Sí
```

El algoritmo observa miles de ejemplos similares.

Después intenta aprender una función capaz de predecir correctamente nuevas respuestas.

Formalmente,

\[
f(X)=y
\]

Nuestro objetivo consiste en minimizar el error de predicción.

---

## 3.2 Unsupervised Learning

En este caso únicamente disponemos de las variables de entrada.

No existe una respuesta correcta.

No existe una etiqueta.

No existe un profesor.

Matemáticamente,

\[
X
\]

Nada más.

El algoritmo debe descubrir por sí mismo cómo están organizados los datos.

---

## 3.3 Reinforcement Learning

Existe un agente.

Existe un ambiente.

El agente toma decisiones.

Recibe recompensas o castigos.

Aprende mediante prueba y error.

Aunque constituye una rama muy importante de la inteligencia artificial, no será abordada en este curso.

---

# 4. ¿Qué es el Aprendizaje No Supervisado?

El libro lo define como un enfoque de modelado que **no utiliza variables objetivo**, y cuyo propósito consiste en descubrir estructuras presentes en los datos. :contentReference[oaicite:2]{index=2}

Podemos ampliar esa idea con una definición más completa.

> **El aprendizaje no supervisado es el conjunto de algoritmos cuyo objetivo consiste en descubrir patrones, relaciones o estructuras presentes en un conjunto de datos sin disponer de una variable objetivo que indique cuál es la respuesta correcta.**

Hay una palabra extremadamente importante en esta definición.

**Estructura.**

Todo el capítulo gira alrededor de ese concepto.

---

# 5. ¿Qué significa descubrir estructura?

Esta es probablemente la idea más importante de todo el capítulo.

Nuestro cerebro es extraordinariamente bueno detectando patrones.

Por ejemplo, observa los siguientes puntos.

```
● ● ● ●

● ● ●

                     ● ● ●

                  ● ● ●

                               ● ●
```

Casi inmediatamente pensamos

> "Existen grupos."

Nadie nos dijo que existían grupos.

Nuestro cerebro simplemente detectó una estructura.

Ahora observa otra distribución.

```
●

    ●

         ●

             ●

                 ●

                      ●
```

Ahora pensamos

> "Existe una relación lineal."

Otra estructura.

Ahora observa esto.

```
●       ●

     ●

●

           ●

      ●
```

Aquí resulta mucho más difícil encontrar un patrón.

Precisamente eso intenta hacer un algoritmo de aprendizaje no supervisado.

Buscar organización donde aparentemente solo existe una nube de puntos.

---

# 6. La diferencia fundamental con Supervised Learning

Esta diferencia puede resumirse en una sola pregunta.

## Supervised Learning

Pregunta:

> ¿Cómo puedo predecir correctamente una respuesta?

## Unsupervised Learning

Pregunta:

> ¿Cómo están organizados realmente estos datos?

Son problemas completamente distintos.

Uno intenta **predecir**.

El otro intenta **descubrir**.

---

## Comparación

| Supervised Learning | Unsupervised Learning |
|---------------------|----------------------|
| Existe variable objetivo | No existe variable objetivo |
| Hay respuestas correctas | No existen respuestas correctas |
| Aprende funciones predictivas | Aprende estructuras |
| Evalúa errores | Evalúa organización |
| Produce predicciones | Produce conocimiento |
| Se optimiza mediante una función de pérdida | Se optimiza mediante criterios de estructura (distancias, varianza, densidad, etc.) |

---

# 7. ¿Por qué existe el aprendizaje no supervisado?

Podría parecer que el aprendizaje supervisado siempre es mejor.

Después de todo, disponer de la respuesta correcta parece una enorme ventaja.

Sin embargo, en el mundo real ocurre exactamente lo contrario.

## La mayoría de los datos no tienen etiquetas.

Supongamos que Amazon posee información de cien millones de usuarios.

¿Existe una columna llamada

```
Tipo de comprador
```

?

No.

¿Existe una columna llamada

```
Perfil psicológico
```

?

No.

¿Existe una columna llamada

```
Cliente impulsivo
```

?

No.

Esas categorías simplemente no existen.

Y precisamente queremos descubrirlas.

Por eso el aprendizaje no supervisado es tan importante.

---

# 8. El verdadero objetivo

Muchas personas creen que el objetivo del aprendizaje no supervisado consiste en "hacer clusters".

Eso es incorrecto.

Los clusters son únicamente una herramienta.

El verdadero objetivo consiste en generar conocimiento.

Por ejemplo,

- descubrir segmentos de clientes
- encontrar genes similares
- detectar comunidades
- identificar documentos relacionados
- resumir información
- encontrar relaciones entre productos

En otras palabras,

> **transformar datos en conocimiento.**

---

# 9. Los tres grandes problemas del aprendizaje no supervisado

Este capítulo estudia tres familias principales de algoritmos. :contentReference[oaicite:3]{index=3}

## Clustering

Pregunta fundamental:

> ¿Qué observaciones son similares entre sí?

Ejemplos:

- Segmentación de clientes.
- Agrupación de imágenes.
- Agrupación de enfermedades.
- Agrupación de documentos.

---

## Reducción de dimensionalidad

Pregunta fundamental:

> ¿Podemos representar toda esta información utilizando menos variables sin perder demasiada información?

El algoritmo más famoso es:

**Principal Component Analysis (PCA).**

---

## Reglas de asociación

Pregunta fundamental:

> ¿Qué elementos suelen aparecer juntos?

Ejemplos:

- Clientes que compran pan también compran mantequilla.
- Personas que adquieren un teléfono suelen comprar un protector.
- Usuarios que ven cierta película suelen ver otra similar.

---

# 10. Una nueva forma de pensar

En los capítulos anteriores siempre existía una respuesta correcta.

Ahora debemos cambiar completamente nuestra mentalidad.

No preguntaremos

> ¿Cuál es la respuesta?

Ahora preguntaremos

> ¿Qué estructura esconden los datos?

Ese cambio de perspectiva marca el inicio del aprendizaje no supervisado.

En los siguientes apartados comenzaremos a estudiar la herramienta más importante de todo este paradigma:

**la geometría del espacio de características.**

# 11. Cambiando nuestra forma de pensar

Uno de los mayores obstáculos al comenzar a estudiar **Unsupervised Learning** no es aprender nuevos algoritmos.

Es cambiar completamente la forma en que pensamos acerca de los datos.

Durante todo nuestro recorrido por el aprendizaje supervisado, nos acostumbramos a una pregunta muy específica:

> **¿Qué quiero predecir?**

Cuando entrenábamos una regresión lineal queríamos predecir un precio.

Cuando entrenábamos una regresión logística queríamos predecir una clase.

Cuando utilizábamos árboles de decisión queríamos clasificar correctamente nuevas observaciones.

Siempre existía un objetivo perfectamente definido.

En aprendizaje no supervisado esa pregunta desaparece.

En su lugar aparecen preguntas completamente distintas.

Por ejemplo:

- ¿Existen grupos naturales?
- ¿Qué observaciones son similares?
- ¿Cuáles variables contienen información redundante?
- ¿Qué relaciones aparecen frecuentemente?
- ¿Qué estructura posee este conjunto de datos?

Este cambio parece pequeño.

En realidad, cambia absolutamente todo.

---

# 12. El problema de no tener respuestas

Imaginemos el siguiente conjunto de datos.

| Cliente | Edad | Ingreso | Compras |
|----------|------|----------|----------|
| A | 24 | 850 | 15 |
| B | 26 | 900 | 18 |
| C | 27 | 870 | 14 |
| D | 58 | 6200 | 3 |
| E | 61 | 6700 | 4 |
| F | 60 | 6400 | 2 |

En aprendizaje supervisado normalmente agregaríamos otra columna.

| Cliente | Edad | Ingreso | Compras | ¿Comprará? |
|----------|------|----------|----------|-------------|
| A | 24 | 850 | 15 | Sí |
| B | 26 | 900 | 18 | Sí |
| C | 27 | 870 | 14 | No |
| D | 58 | 6200 | 3 | No |

Ahora existe una respuesta correcta.

Pero supongamos que esa última columna nunca fue registrada.

La empresa únicamente conoce las primeras tres variables.

¿Significa eso que los datos no sirven?

En absoluto.

Aunque no podamos construir un modelo predictivo, todavía podemos responder preguntas extremadamente valiosas.

Por ejemplo,

- ¿Cuántos tipos de clientes existen?
- ¿Quiénes son similares?
- ¿Qué perfiles aparecen?

Eso es precisamente lo que intenta descubrir el aprendizaje no supervisado.

---

# 13. ¿Realmente existen grupos?

Aquí aparece una pregunta muy interesante.

Supongamos la siguiente distribución.

```
● ● ●

● ●

                ● ● ●

             ● ●

                          ● ● ●
```

Nuestro cerebro dice inmediatamente:

> "Hay tres grupos."

Pero...

¿Quién decidió que eran tres?

Nadie.

Es una interpretación.

Ahora observa otra distribución.

```
● ● ● ● ● ● ● ● ●
```

Aquí probablemente responderíamos:

> "Solo existe un grupo."

Ahora observa esta otra.

```
● ● ●

     ● ● ●

          ● ● ●

               ● ● ●
```

¿Cuántos grupos existen aquí?

Dos personas podrían responder de forma diferente.

Y ambas podrían tener razón.

---

## Una idea importante

En aprendizaje no supervisado muchas veces **no existe una única respuesta correcta**.

Esto representa una enorme diferencia con el aprendizaje supervisado.

En clasificación existe una etiqueta verdadera.

Aquí no.

Muchas veces diferentes algoritmos producirán agrupaciones distintas.

La pregunta deja de ser

> ¿Cuál es la respuesta correcta?

y pasa a ser

> ¿Qué respuesta resulta más útil para nuestro problema?

---

# 14. El aprendizaje no supervisado como exploración

Existe una palabra que aparece constantemente en ciencia de datos:

> **Exploración**

Antes de construir modelos predictivos, normalmente necesitamos entender los datos.

Por ejemplo:

- detectar errores
- descubrir valores atípicos
- encontrar grupos
- comprender relaciones
- identificar redundancias

Todo esto pertenece al análisis exploratorio.

Por esa razón el aprendizaje no supervisado suele utilizarse como una de las primeras etapas de un proyecto de Machine Learning. :contentReference[oaicite:0]{index=0}

Podemos pensar en el proceso de la siguiente manera.

```
Datos

↓

Exploración

↓

Comprensión

↓

Modelado

↓

Predicción
```

Muchas empresas pasan semanas explorando los datos antes de entrenar el primer modelo supervisado.

---

# 15. Descubrir conocimiento

Imaginemos nuevamente una empresa.

Después de ejecutar un algoritmo obtiene los siguientes grupos.

```
Grupo A

• Jóvenes
• Muchas compras
• Bajo ingreso
```

```
Grupo B

• Adultos
• Alto ingreso
• Pocas compras
```

```
Grupo C

• Jubilados
• Compras frecuentes
• Alto ahorro
```

Observa que el algoritmo no predijo absolutamente nada.

Sin embargo, produjo algo extremadamente valioso.

**Conocimiento.**

Ese conocimiento puede utilizarse para

- campañas de marketing
- promociones
- diseño de productos
- segmentación
- personalización

El aprendizaje no supervisado produce información que antes nadie conocía.

---

# 16. ¿Cómo descubre patrones un algoritmo?

Hasta ahora hemos hablado mucho acerca de "descubrir patrones".

Pero...

¿Cómo lo hace realmente una computadora?

Aquí aparece una idea fundamental.

Las computadoras no entienden conceptos como

- cliente
- automóvil
- fotografía
- enfermedad

Lo único que entienden son números.

Por ejemplo.

| Cliente | Edad | Salario |
|----------|----------|----------|
| A | 25 | 800 |
| B | 27 | 850 |
| C | 61 | 6100 |

Para nosotros esas filas representan personas.

Para la computadora únicamente representan puntos dentro de un espacio matemático.

Más adelante veremos que esta idea recibe el nombre de **espacio de características (Feature Space)**.

Todo el aprendizaje no supervisado ocurre dentro de ese espacio.

---

# 17. La intuición correcta

Cuando comenzamos a estudiar K-Means, muchos estudiantes piensan que el algoritmo intenta responder

> ¿Quién pertenece al grupo A?

En realidad no.

La pregunta correcta es

> ¿Qué observaciones se parecen entre sí?

La palabra importante es

**parecerse**.

Y eso inmediatamente nos conduce a otro concepto.

¿Cómo medimos el parecido?

La respuesta será:

**mediante distancias.**

Y allí comienza realmente la geometría del aprendizaje no supervisado.

---

# 18. La importancia de la geometría

Hasta ahora probablemente hayas notado algo.

Este capítulo se siente mucho más matemático que los anteriores.

Y esa sensación es completamente correcta.

¿Por qué?

Porque en aprendizaje supervisado las etiquetas guían el aprendizaje.

Mientras exista una columna objetivo, el algoritmo puede utilizarla para corregir sus errores.

En aprendizaje no supervisado esa ayuda desaparece.

El algoritmo únicamente dispone de la posición de las observaciones.

Por ello necesita responder preguntas como:

- ¿Qué puntos están cerca?
- ¿Qué puntos están lejos?
- ¿Qué dirección presenta mayor variabilidad?
- ¿Dónde aparecen regiones densas?
- ¿Qué observaciones parecen similares?

Todas estas preguntas pertenecen a la geometría.

Por esa razón los algoritmos de aprendizaje no supervisado utilizan constantemente conceptos como:

- distancia
- norma
- ángulo
- proyección
- centroide
- densidad
- covarianza
- varianza
- eigenvectores
- eigenvalores

En otras palabras,

> **el aprendizaje no supervisado estudia la forma de los datos.**

---

# 19. Una analogía útil

Imagina una habitación completamente oscura.

No puedes ver nada.

Sin embargo, comienzas a caminar.

Poco a poco descubres:

- dónde están las paredes
- dónde están los muebles
- qué zonas están libres
- qué objetos aparecen agrupados

Nunca alguien te dijo cómo era la habitación.

La descubriste explorándola.

Eso hace exactamente un algoritmo de aprendizaje no supervisado.

Explora.

Observa.

Busca regularidades.

Descubre estructura.

---

# 20. Resumen

En esta segunda parte aprendimos varias ideas fundamentales.

- El aprendizaje no supervisado cambia completamente nuestra forma de pensar.
- Ya no buscamos predecir respuestas.
- Buscamos descubrir conocimiento.
- Muchas veces no existe una única solución correcta.
- Los algoritmos trabajan explorando la estructura de los datos.
- La geometría será el lenguaje principal durante todo este capítulo.
- Conceptos como distancia, similitud y espacio de características serán la base para comprender K-Means y PCA.

En el siguiente apartado comenzaremos a estudiar el concepto más importante de todo el capítulo:

> **El Espacio de Características (Feature Space)**.

A partir de ese momento veremos que prácticamente todos los algoritmos de aprendizaje no supervisado pueden entenderse como diferentes maneras de analizar la geometría de ese espacio.