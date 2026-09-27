
# Parte 1 - El Espacio de Características (Feature Space)

> **Objetivo del cuaderno**
>
> Antes de estudiar K-Means, PCA o cualquier otro algoritmo de aprendizaje no supervisado, necesitamos comprender un concepto fundamental:
>
> **todos los algoritmos trabajan sobre un espacio geométrico.**
>
> Este cuaderno construirá la intuición necesaria para entender ese espacio.

---

# Objetivos de aprendizaje

Al finalizar este cuaderno serás capaz de:

- Comprender qué es un Feature Space.
- Representar un conjunto de datos como puntos en un espacio geométrico.
- Entender por qué cada fila de un dataset representa un punto.
- Comprender cómo aumenta la dimensionalidad.
- Interpretar correctamente gráficos bidimensionales y tridimensionales.
- Comprender por qué la geometría constituye el lenguaje del aprendizaje no supervisado.

---

# 1. Una idea completamente diferente

Hasta ahora hemos trabajado principalmente con tablas.

Por ejemplo.

| Edad | Ingreso | Compra |
|------|---------|---------|
|23|800|Sí|
|25|900|No|
|54|6200|Sí|

Nuestro cerebro interpreta inmediatamente esta información como una tabla.

Sin embargo, una computadora no piensa de esa manera.

Para un algoritmo de Machine Learning esta tabla representa algo completamente distinto.

Representa una colección de puntos.

Y este pequeño cambio de perspectiva transforma completamente la forma de analizar los datos.

---

# 2. Las computadoras no ven tablas

Para nosotros una fila representa una persona.

Para un algoritmo una fila únicamente representa números.

Por ejemplo,

| Edad | Ingreso |
|------|---------|
|23|800|

Para nosotros significa

> Roberto tiene 23 años y gana 800 dólares.

Para una computadora significa simplemente

\[
(23,\;800)
\]

Nada más.

No existe el concepto de persona.

No existe el concepto de salario.

No existe el concepto de edad.

Solo existen números.

---

# 3. De una tabla a un plano

Tomemos un conjunto de datos muy pequeño.

| Persona | Edad | Ingreso |
|----------|------|----------|
|A|20|800|
|B|22|900|
|C|25|1000|
|D|45|4500|
|E|48|5000|

En lugar de leerlo como una tabla, podemos dibujarlo.

```
Ingreso

5000 |                         E

4500 |                      D

4000 |

3000 |

2000 |

1000 |          C

 900 |      B

 800 |    A

      ----------------------------------------
        20   25   30   35   40   45   50

                 Edad
```

Ahora cada persona es un punto.

Ya no estamos observando una tabla.

Estamos observando un espacio.

Ese espacio recibe un nombre muy importante.

---

# 4. ¿Qué es el Feature Space?

El **Feature Space** (Espacio de Características) es el espacio geométrico formado por todas las variables de entrada de un conjunto de datos.

Dicho de otra manera,

> Cada variable representa un eje.

Cada observación representa un punto dentro de ese espacio.

Por ejemplo.

Si nuestro dataset tiene

- Edad
- Ingreso

obtenemos un plano.

```
Ingreso

^

|

|

+-------------------------->

            Edad
```

Si agregamos una tercera variable

- Número de compras

obtenemos un espacio tridimensional.

```
             Compras
                 ^
                /
               /
              /
             /
            O------------>

          Edad

          \
           \
            \
             v

          Ingreso
```

Y si agregamos una cuarta variable...

Ya no podemos dibujarlo.

---

# 5. Cada columna es un eje

Esta idea es probablemente la más importante del capítulo.

Supongamos el siguiente dataset.

|Edad|Ingreso|Compras|
|------|---------|---------|
|25|900|12|

Matemáticamente esa observación puede escribirse como

\[
(25,\;900,\;12)
\]

Esto significa que el punto posee tres coordenadas.

```
(x,y,z)

↓

(25,900,12)
```

Generalizando,

si un dataset posee \(n\) variables,

cada observación será un punto dentro de un espacio de dimensión \(n\).

---

# 6. Una dimensión

Comencemos con el caso más sencillo posible.

Supongamos que únicamente medimos la edad.

|Edad|
|-----|
|20|
|25|
|40|
|60|

Ahora únicamente existe un eje.

```
Edad

20      25             40                    60

●-------●--------------●---------------------●
```

Este espacio posee una sola dimensión.

---

# 7. Dos dimensiones

Ahora agregamos el ingreso.

|Edad|Ingreso|
|-----|--------|
|20|800|
|25|1000|
|40|4500|
|60|7000|

Ahora aparecen dos ejes.

```
Ingreso

7000 |                            ●

6000 |

5000 |

4000 |                 ●

3000 |

2000 |

1000 |      ●

 800 |   ●

     --------------------------------------------

       20     30      40      50      60

                  Edad
```

Ya no hablamos de una línea.

Ahora hablamos de un plano.

---

# 8. Tres dimensiones

Ahora agregamos otra característica.

|Edad|Ingreso|Compras|
|------|----------|----------|
|20|800|15|
|25|1000|12|
|40|4500|4|
|60|7000|2|

Ahora necesitamos tres ejes.

```
                 Compras
                    ^
                   /
                  /
                 /
                O---------->

             Edad

             \
              \
               \
                v

             Ingreso
```

Cada cliente ocupa una posición distinta.

---

# 9. ¿Y si existen cien variables?

Aquí aparece una idea fascinante.

Muchos datasets reales poseen

- 50 variables
- 100 variables
- 500 variables
- 10 000 variables

No podemos visualizar un espacio de 100 dimensiones.

Sin embargo,

las matemáticas no tienen ningún problema con ello.

Un algoritmo puede trabajar perfectamente con espacios de cientos o miles de dimensiones.

Por ejemplo,

el famoso dataset de cáncer de mama utilizado posteriormente en este capítulo contiene 30 variables predictoras. :contentReference[oaicite:0]{index=0}

Eso significa que cada paciente puede representarse como un punto dentro de un espacio de 30 dimensiones.

Aunque resulte imposible imaginarlo.

---

# 10. Una analogía

Imagina una ciudad.

Cada casa posee una dirección.

```
Avenida
Calle
Número
Apartamento
```

Esa combinación identifica una ubicación única.

Lo mismo ocurre en Machine Learning.

Las variables funcionan como coordenadas.

Cada observación posee una ubicación única dentro del espacio de características.

Por ejemplo,

```
Edad = 32

Ingreso = 1800

Compras = 14
```

equivale a decir

```
Latitud = ...

Longitud = ...

Altitud = ...
```

Ambos describen posiciones.

La diferencia es que uno describe posiciones geográficas y el otro posiciones dentro de un espacio matemático.

---

# 11. Una nueva manera de leer un DataFrame

A partir de este momento intentaremos cambiar nuestra forma de interpretar un DataFrame.

En lugar de pensar

> "Tengo una tabla."

pensaremos

> "Tengo una nube de puntos."

Por ejemplo.

```
DataFrame

Edad   Ingreso

23      900

24      950

50      6000

48      5800
```

En realidad representa

```
Ingreso

^

|

|                         ●

|

|

|      ● ●

+---------------------------------->

              Edad
```

Esta idea parece sencilla.

Sin embargo,

es el fundamento absoluto de todo el aprendizaje no supervisado.

---

# 12. La representación vectorial

Existe otra forma de representar una observación.

En lugar de escribir

|Edad|Ingreso|Compras|
|------|---------|---------|
|25|900|12|

podemos escribir

\[
\mathbf{x}=
\begin{bmatrix}
25\\
900\\
12
\end{bmatrix}
\]

Este objeto recibe el nombre de **vector de características** (*Feature Vector*).

Cada observación del dataset es un vector.

Y un dataset completo es simplemente un conjunto de vectores.

Esta forma de representar los datos será utilizada constantemente cuando estudiemos K-Means, PCA y Redes Neuronales.

---

# 13. Una observación importante

Hasta ahora hemos hablado de puntos.

Pero todavía no hemos respondido una pregunta fundamental.

Supongamos dos clientes.

```
Cliente A

Edad = 25

Ingreso = 900
```

```
Cliente B

Edad = 26

Ingreso = 910
```

Intuitivamente parecen muy parecidos.

¿Por qué?

¿Cómo medimos esa similitud?

¿Qué significa realmente que dos observaciones estén "cerca"?

La respuesta nos llevará al siguiente gran concepto del capítulo:

> **las distancias**.

Sin una medida de distancia, un algoritmo nunca podría decidir qué observaciones son similares y cuáles son completamente diferentes.

Ese será precisamente el tema de la segunda parte de este cuaderno.

# Cuaderno 2 - La Geometría del Aprendizaje No Supervisado
# Parte 2 - Distancias, Similitud y la Base Matemática del Clustering

> **Idea central**
>
> Hasta ahora aprendimos que cada observación puede representarse como un punto dentro de un espacio de características.
>
> La siguiente pregunta es inevitable:
>
> **¿Cómo decide una computadora si dos puntos son parecidos?**
>
> Toda la respuesta se encuentra en un concepto fundamental:
>
> **la distancia.**

---

# Objetivos de aprendizaje

Al finalizar esta sección serás capaz de:

- Comprender qué significa que dos observaciones sean similares.
- Entender la importancia de las funciones de distancia.
- Calcular manualmente la distancia Euclidiana.
- Comprender la distancia Manhattan.
- Introducir la distancia Minkowski.
- Comprender la diferencia entre distancia y similitud.
- Entender por qué es indispensable estandarizar los datos antes de aplicar K-Means.

---

# 14. ¿Qué significa que dos observaciones sean parecidas?

Supongamos que tenemos dos clientes.

|Cliente|Edad|Ingreso|
|-------|------|---------|
|A|25|900|
|B|26|910|

Intuitivamente diríamos que ambos clientes son muy parecidos.

Ahora observemos otro cliente.

|Cliente|Edad|Ingreso|
|-------|------|---------|
|C|62|6500|

Claramente C se parece mucho menos a A.

Pero...

¿Cómo llegó nuestro cerebro a esa conclusión?

No hicimos ningún cálculo.

Simplemente observamos que los valores son muy diferentes.

Una computadora necesita una regla matemática para realizar exactamente ese mismo razonamiento.

---

# 15. Distancia = Diferencia

La idea más sencilla consiste en pensar que

> **dos observaciones son similares cuando la distancia entre ellas es pequeña.**

Y son diferentes cuando esa distancia es grande.

Es exactamente la misma idea que utilizamos en un mapa.

```
Casa A ---------------- Casa B

5 metros
```

Las casas están cerca.

```
Casa A --------------------------------------------- Casa C

5 kilómetros
```

Las casas están lejos.

En Machine Learning ocurre exactamente lo mismo.

La diferencia es que las posiciones ya no representan lugares físicos.

Representan posiciones dentro del Feature Space.

---

# 16. La distancia Euclidiana

La distancia más utilizada en Machine Learning es la **distancia Euclidiana**.

Es simplemente la distancia en línea recta entre dos puntos.

Recordemos el famoso Teorema de Pitágoras.

```
        ● B
       /|
      / |
     /  |
    /   |
   /    |
  /_____|
● A
```

La distancia entre ambos puntos es

\[
d=\sqrt{a^2+b^2}
\]

Si los puntos poseen coordenadas

\[
A=(x_1,y_1)
\]

\[
B=(x_2,y_2)
\]

entonces

\[
d(A,B)=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
\]

Esta fórmula probablemente sea una de las más importantes de todo el aprendizaje no supervisado.

---

# 17. Ejemplo paso a paso

Supongamos dos clientes.

|Cliente|Edad|Ingreso|
|-------|------|---------|
|A|25|900|
|B|28|1000|

Podemos escribirlos como

\[
A=(25,900)
\]

\[
B=(28,1000)
\]

La distancia será

\[
d=
\sqrt{
(28-25)^2+
(1000-900)^2
}
\]

Calculando,

\[
=
\sqrt{
3^2+
100^2
}
\]

\[
=
\sqrt{
9+10000
}
\]

\[
=
100.04
\]

Observa algo curioso.

Aunque la edad cambió tres años,

el ingreso cambió cien dólares.

Prácticamente toda la distancia proviene del ingreso.

Más adelante veremos por qué esto representa un enorme problema.

---

# 18. La distancia en tres dimensiones

Supongamos ahora que agregamos otra variable.

|Edad|Ingreso|Compras|
|------|---------|----------|
|25|900|12|

Cada punto posee ahora tres coordenadas.

```
(Edad,Ingreso,Compras)
```

La distancia se convierte en

\[
d=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2+
(z_2-z_1)^2
}
\]

Simplemente agregamos otro término.

---

# 19. Generalizando a n dimensiones

En Machine Learning rara vez trabajamos con dos variables.

Podemos tener

- 10 variables
- 50 variables
- 500 variables

La fórmula simplemente continúa creciendo.

Si un dataset posee \(n\) variables,

la distancia Euclidiana es

\[
d(x,y)=
\sqrt{
\sum_{i=1}^{n}
(x_i-y_i)^2
}
\]

Esta expresión puede parecer intimidante.

En realidad dice exactamente lo mismo.

Simplemente suma el cuadrado de las diferencias de todas las variables.

---

# 20. ¿Por qué elevar al cuadrado?

Muchas personas se preguntan por qué utilizamos cuadrados.

Existen varias razones.

## Primera razón

Evitar números negativos.

Si no eleváramos al cuadrado,

las diferencias positivas y negativas podrían cancelarse.

Ejemplo.

```
+5

-5

Resultado = 0
```

Eso sería incorrecto.

Ambas diferencias representan cambios importantes.

---

## Segunda razón

Penalizar diferencias grandes.

Observemos.

```
Diferencia = 2

2² = 4
```

```
Diferencia = 10

10² = 100
```

Las diferencias grandes reciben mucho más peso.

Esto hace que puntos muy alejados influyan considerablemente en la distancia total.

---

# 21. Distancia Manhattan

No siempre queremos medir la distancia en línea recta.

Supongamos una ciudad organizada como Manhattan.

```
■──■──■──■

│  │  │  │

■──■──■──■

│  │  │  │

■──■──■──■
```

Los automóviles no pueden atravesar edificios.

Solo pueden recorrer calles.

La distancia deja de ser diagonal.

Ahora consiste en sumar desplazamientos horizontales y verticales.

La fórmula es

\[
d=
|x_2-x_1|
+
|y_2-y_1|
\]

A esta medida se le conoce como **Distancia Manhattan** o **L1**.

---

# 22. Comparación visual

Supongamos estos dos puntos.

```
B ●

│

│

│

●────────────── A
```

Distancia Euclidiana.

```
A ----------- B
```

La línea recta.

Distancia Manhattan.

```
Arriba

Luego

Derecha
```

Son dos formas distintas de medir la separación entre los mismos puntos.

---

# 23. Distancia Minkowski

Existe una familia completa de distancias.

Todas ellas pueden escribirse mediante una sola ecuación.

\[
d=
\left(
\sum
|x_i-y_i|^p
\right)^{1/p}
\]

Dependiendo del valor de \(p\),

obtenemos diferentes medidas.

|Valor de \(p\)|Distancia|
|--------------|-----------|
|1|Manhattan|
|2|Euclidiana|
|\(\infty\)|Chebyshev|

Por esta razón suele decirse que la distancia Euclidiana y la Manhattan son casos particulares de la distancia de Minkowski.

---

# 24. Distancia no significa similitud

Este es un detalle muy importante.

Una distancia mide separación.

Una similitud mide parecido.

Son conceptos relacionados, pero no iguales.

Por ejemplo,

```
Distancia pequeña

↓

Gran similitud
```

```
Distancia grande

↓

Poca similitud
```

Muchos algoritmos trabajan con distancias.

Otros trabajan directamente con medidas de similitud.

---

# 25. El gran problema de las escalas

Volvamos al ejemplo anterior.

|Edad|Ingreso|
|------|----------|
|25|900|

|Edad|Ingreso|
|------|----------|
|28|1000|

La distancia fue aproximadamente

100.

Sin embargo,

¿realmente tres años de edad son cien veces menos importantes que cien dólares?

No necesariamente.

El problema es que ambas variables están medidas en escalas distintas.

```
Edad

20

30

40
```

```
Ingreso

500

5000

20000
```

Los ingresos poseen valores muchísimo mayores.

Como consecuencia,

dominan completamente la distancia.

---

# 26. ¿Por qué debemos estandarizar?

Supongamos dos variables.

```
Edad

18

25

40

65
```

```
Salario

500

15000

80000

300000
```

Cuando calculamos la distancia,

el salario prácticamente decide todo.

La edad casi no influye.

Eso significa que K-Means terminaría agrupando clientes casi exclusivamente según el salario.

Y probablemente eso no sea lo que queremos.

Por esa razón,

antes de aplicar la mayoría de algoritmos de clustering,

se realiza una **estandarización** utilizando herramientas como `StandardScaler`.

En el capítulo del libro, antes de aplicar K-Means al conjunto de datos bancario, se estandarizan las variables seleccionadas precisamente para evitar que aquellas con escalas mayores dominen el proceso de agrupamiento. :contentReference[oaicite:0]{index=0}

---

# 27. Una intuición importante

Podemos resumir todo este cuaderno en una sola frase.

```
Datos

↓

Puntos

↓

Distancias

↓

Similitudes

↓

Grupos
```

Ese será exactamente el razonamiento que seguirá K-Means.

No "entiende" clientes.

No "entiende" fotografías.

No "entiende" enfermedades.

Únicamente calcula distancias entre puntos.

Y, a partir de ellas,

descubre grupos.

---

# 28. Resumen

En este cuaderno construimos la base matemática del aprendizaje no supervisado.

Aprendimos que:

- Cada fila de un DataFrame representa un punto.
- Todas las variables forman un espacio geométrico.
- La similitud entre observaciones se mide mediante distancias.
- La distancia Euclidiana es la medida más utilizada.
- La distancia Manhattan es otra alternativa importante.
- Ambas pertenecen a la familia de distancias de Minkowski.
- Las variables deben encontrarse en escalas comparables antes de calcular distancias.
- La estandarización evita que una variable domine a las demás.

Con estas ideas ya estamos preparados para estudiar el primer algoritmo de clustering.

En el siguiente cuaderno comenzaremos con **K-Means**, comprendiendo primero la intuición detrás de los centroides y posteriormente el algoritmo completo paso a paso.