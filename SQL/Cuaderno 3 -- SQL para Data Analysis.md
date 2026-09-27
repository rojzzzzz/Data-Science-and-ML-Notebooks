

## Parte 1 -- Patrones de Consulta y Funciones de Ventana

> Este cuaderno deja de enfocarse en aprender sintaxis y comienza a
> enseñar cómo resolver problemas reales utilizando SQL. Los ejemplos
> son independientes del motor de base de datos siempre que sea posible.

------------------------------------------------------------------------

# Tabla de contenido

1.  SQL en proyectos reales
2.  Pensar en conjuntos
3.  Patrones comunes de consultas
4.  Consultas de resumen
5.  Segmentación con CASE
6.  Ranking de registros
7.  Introducción a Window Functions
8.  OVER()
9.  ROW_NUMBER()
10. RANK() y DENSE_RANK()
11. LAG() y LEAD()
12. Running Totals
13. PARTITION BY
14. Buenas prácticas
15. Resumen

------------------------------------------------------------------------

# 1. SQL en proyectos reales

En proyectos profesionales rara vez se escriben consultas tan simples
como:

``` sql
SELECT *
FROM employees;
```

Lo habitual es responder preguntas de negocio:

-   ¿Cuáles fueron los cinco productos más vendidos?
-   ¿Quién es el mejor cliente por país?
-   ¿Qué ventas disminuyeron respecto al mes anterior?
-   ¿Cuál es el promedio móvil de los últimos tres meses?

Para responder estas preguntas es necesario combinar agregaciones,
funciones de ventana y patrones de consulta.

------------------------------------------------------------------------

# 2. Pensar en conjuntos

SQL trabaja con **conjuntos de filas**, no con registros individuales.

En lugar de pensar:

> "Voy a recorrer cada fila."

Debemos pensar:

> "¿Qué conjunto de datos necesito obtener?"

Este cambio de mentalidad es una de las mayores diferencias entre SQL y
lenguajes imperativos.

------------------------------------------------------------------------

# 3. Patrones comunes de consultas

Algunos patrones aparecen constantemente:

-   Top N.
-   Bottom N.
-   Totales por categoría.
-   Ranking.
-   Acumulados.
-   Comparación con el promedio.
-   Primer y último registro.
-   Detección de duplicados.

Aprender estos patrones es más útil que memorizar consultas aisladas.

------------------------------------------------------------------------

# 4. Consultas de resumen

Ejemplo:

``` sql
SELECT
    category,
    COUNT(*) AS products,
    AVG(price) AS avg_price,
    SUM(stock) AS total_stock
FROM products
GROUP BY category;
```

Este tipo de consulta constituye la base de la mayoría de dashboards y
reportes.

------------------------------------------------------------------------

# 5. Segmentación con CASE

Una técnica muy utilizada consiste en transformar valores continuos en
categorías.

``` sql
CASE
    WHEN amount < 100 THEN 'Low'
    WHEN amount < 500 THEN 'Medium'
    ELSE 'High'
END
```

Después puede agruparse:

``` sql
SELECT segment,
       COUNT(*)
FROM (...)
GROUP BY segment;
```

------------------------------------------------------------------------

# 6. Ranking de registros

Frecuentemente necesitamos conocer:

-   Los 10 clientes con mayor compra.
-   Los productos más vendidos.
-   Los empleados con mayor salario.

Antes de las funciones de ventana esto requería subconsultas complejas.

------------------------------------------------------------------------

# 7. Introducción a Window Functions

Las funciones de ventana calculan valores utilizando un conjunto de
filas relacionado con la fila actual **sin perder el detalle de cada
registro**.

A diferencia de `GROUP BY`, no reducen el número de filas.

------------------------------------------------------------------------

# 8. OVER()

Todas las funciones de ventana utilizan `OVER()`.

``` sql
AVG(salary) OVER()
```

Calcula el promedio de toda la tabla y lo muestra en cada fila.

------------------------------------------------------------------------

# 9. ROW_NUMBER()

Asigna un número consecutivo.

``` sql
SELECT
    employee_name,
    ROW_NUMBER() OVER(
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

Usos:

-   Top N.
-   Paginación.
-   Eliminar duplicados.

------------------------------------------------------------------------

# 10. RANK() y DENSE_RANK()

Ambas generan rankings.

`RANK()` deja huecos cuando existen empates.

    100
    100
    90

    Ranking

    1
    1
    3

`DENSE_RANK()` no deja huecos.

    100
    100
    90

    1
    1
    2

------------------------------------------------------------------------

# 11. LAG() y LEAD()

Permiten acceder a registros anteriores o posteriores.

Ejemplo:

``` sql
SELECT
    sale_date,
    amount,
    LAG(amount) OVER(
        ORDER BY sale_date
    ) AS previous_sale
FROM sales;
```

Son muy utilizadas para comparar periodos consecutivos.

------------------------------------------------------------------------

# 12. Running Totals

Acumulados.

``` sql
SUM(amount)
OVER(
ORDER BY sale_date
)
```

Resultado conceptual:

    100
    150
    220
    310
    ...

Es uno de los patrones más utilizados en Business Intelligence.

------------------------------------------------------------------------

# 13. PARTITION BY

Equivale a realizar un GROUP BY **sin perder las filas originales**.

``` sql
AVG(salary)
OVER(
PARTITION BY department
)
```

Cada empleado conserva su registro, pero conoce el promedio de su
departamento.

------------------------------------------------------------------------

# 14. Buenas prácticas

-   Prefiere Window Functions frente a subconsultas complejas cuando sea
    posible.
-   Utiliza nombres descriptivos para las columnas calculadas.
-   Ordena explícitamente las funciones de ventana.
-   Evita repetir expresiones largas; usa CTE.
-   Piensa primero en el resultado esperado y luego en la consulta.

------------------------------------------------------------------------

# 15. Resumen

En este capítulo aprendiste:

-   Cómo abordar problemas de análisis con SQL.
-   Patrones comunes de consulta.
-   Segmentación.
-   Ranking.
-   Introducción a las Window Functions.
-   OVER().
-   ROW_NUMBER().
-   RANK().
-   DENSE_RANK().
-   LAG().
-   LEAD().
-   Running Totals.
-   PARTITION BY.

Las funciones de ventana representan uno de los mayores saltos de
productividad para un Data Analyst y son ampliamente utilizadas en
proyectos de Business Intelligence y Ciencia de Datos.

---
## Parte 2 -- SQL Profesional: Optimización, Patrones y Buenas Prácticas

> Este capítulo reúne técnicas y recomendaciones utilizadas diariamente
> por Data Analysts, Data Engineers y desarrolladores. El objetivo no es
> únicamente escribir consultas que funcionen, sino consultas legibles,
> mantenibles y eficientes.

------------------------------------------------------------------------

# Tabla de contenido

1.  Introducción
2.  Cómo piensa un motor SQL
3.  Legibilidad antes que complejidad
4.  Patrones comunes de consultas
5.  JOIN vs IN vs EXISTS
6.  Detección de duplicados
7.  Consultas para Top N
8.  Consultas para Bottom N
9.  Encontrar registros faltantes
10. Consultas con fechas
11. Funciones de texto
12. Optimización de consultas
13. Índices
14. EXPLAIN y planes de ejecución
15. Errores comunes de rendimiento
16. Checklist para escribir buen SQL
17. SQL en entrevistas técnicas
18. Conclusiones

------------------------------------------------------------------------

# 1. Introducción

A medida que los proyectos crecen, escribir consultas correctas deja de
ser suficiente. Una consulta puede producir el resultado esperado y, sin
embargo, ser lenta, difícil de leer o imposible de mantener.

El objetivo de este capítulo es ayudarte a escribir SQL con un enfoque
profesional.

------------------------------------------------------------------------

# 2. Cómo piensa un motor SQL

Un motor de base de datos intenta encontrar el camino más eficiente para
responder una consulta.

No ejecuta las instrucciones exactamente en el orden en que las
escribimos.

Para cada consulta el optimizador analiza:

-   Tamaño de las tablas.
-   Índices disponibles.
-   Estadísticas.
-   Relaciones entre tablas.
-   Costo estimado de cada estrategia.

Por esta razón, dos consultas con el mismo resultado pueden tener
tiempos de ejecución muy diferentes.

------------------------------------------------------------------------

# 3. Legibilidad antes que complejidad

Una buena consulta debe ser:

-   Correcta.
-   Fácil de leer.
-   Fácil de modificar.
-   Fácil de depurar.

Mal ejemplo:

``` sql
SELECT * FROM A a INNER JOIN B b ON a.id=b.id LEFT JOIN C c ON c.id=b.id WHERE ...
```

Mejor:

``` sql
SELECT
    a.customer_id,
    a.customer_name,
    b.order_date,
    c.country
FROM customers AS a
INNER JOIN orders AS b
    ON a.customer_id = b.customer_id
LEFT JOIN countries AS c
    ON a.country_id = c.country_id;
```

El segundo ejemplo produce el mismo resultado, pero es mucho más
mantenible.

------------------------------------------------------------------------

# 4. Patrones comunes de consultas

Con el tiempo descubrirás que muchas consultas pertenecen a un pequeño
conjunto de patrones:

-   Resumen por categorías.
-   Top N.
-   Bottom N.
-   Ranking.
-   Comparación con el promedio.
-   Registros sin relación.
-   Duplicados.
-   Acumulados.
-   Variación respecto al periodo anterior.

Reconocer estos patrones acelera enormemente el desarrollo.

------------------------------------------------------------------------

# 5. JOIN vs IN vs EXISTS

## JOIN

Utilízalo cuando necesites recuperar columnas de ambas tablas.

``` sql
SELECT c.name,
       o.order_date
FROM customers c
JOIN orders o
ON c.id = o.customer_id;
```

------------------------------------------------------------------------

## IN

Adecuado para comparar contra un conjunto relativamente pequeño de
valores.

``` sql
SELECT *
FROM customers
WHERE id IN(
    SELECT customer_id
    FROM orders
);
```

------------------------------------------------------------------------

## EXISTS

Ideal cuando únicamente deseas comprobar si existe una relación.

``` sql
SELECT *
FROM customers c
WHERE EXISTS(
    SELECT 1
    FROM orders o
    WHERE o.customer_id=c.id
);
```

En muchos motores `EXISTS` resulta más eficiente que `IN` para
subconsultas grandes.

------------------------------------------------------------------------

# 6. Detección de duplicados

Una consulta clásica consiste en localizar registros repetidos.

``` sql
SELECT
    email,
    COUNT(*) AS total
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

Este patrón aparece frecuentemente durante procesos de limpieza de
datos.

------------------------------------------------------------------------

# 7. Top N

Encontrar los mejores registros.

``` sql
SELECT
    product_name,
    sales
FROM products
ORDER BY sales DESC
LIMIT 10;
```

En SQL Server se utiliza `TOP`, mientras que el estándar moderno ofrece
`FETCH FIRST`.

------------------------------------------------------------------------

# 8. Bottom N

La lógica es idéntica, cambiando el orden.

``` sql
ORDER BY sales ASC
LIMIT 10;
```

------------------------------------------------------------------------

# 9. Encontrar registros faltantes

Una de las consultas más útiles.

``` sql
SELECT c.*
FROM customers c
LEFT JOIN orders o
ON c.id=o.customer_id
WHERE o.customer_id IS NULL;
```

Permite encontrar:

-   Clientes sin pedidos.
-   Productos nunca vendidos.
-   Empleados sin departamento.

------------------------------------------------------------------------

# 10. Consultas con fechas

Las fechas son uno de los tipos de datos más utilizados.

Operaciones comunes:

-   Año.
-   Mes.
-   Día.
-   Diferencia entre fechas.
-   Primer día del mes.
-   Último día del mes.

La sintaxis cambia entre motores, por lo que conviene consultar la
documentación específica.

------------------------------------------------------------------------

# 11. Funciones de texto

Funciones habituales:

-   UPPER()
-   LOWER()
-   TRIM()
-   LENGTH()
-   CONCAT()
-   SUBSTRING()

Ejemplo:

``` sql
SELECT
UPPER(name)
FROM customers;
```

------------------------------------------------------------------------

# 12. Optimización de consultas

Antes de optimizar una consulta pregúntate:

-   ¿Necesito todas las columnas?
-   ¿Estoy filtrando lo antes posible?
-   ¿Existe un índice adecuado?
-   ¿Estoy repitiendo cálculos innecesarios?

En muchas ocasiones una pequeña modificación reduce el tiempo de
ejecución de minutos a segundos.

------------------------------------------------------------------------

# 13. Índices

Un índice funciona de manera similar al índice de un libro.

Sin índice:

    Libro

    Página 1
    Página 2
    Página 3
    ...

Con índice:

    Índice

    Tema -> Página

Los índices aceleran las búsquedas, pero también incrementan el costo de
las inserciones y actualizaciones.

No todas las columnas deben indexarse.

------------------------------------------------------------------------

# 14. EXPLAIN

La mayoría de motores permiten visualizar el plan de ejecución.

Ejemplos:

``` sql
EXPLAIN
SELECT ...
```

o

``` sql
EXPLAIN ANALYZE
SELECT ...
```

Estos comandos muestran:

-   Orden de ejecución.
-   Índices utilizados.
-   Costos estimados.
-   Número esperado de filas.

Aprender a leer un plan de ejecución es una habilidad muy valiosa.

------------------------------------------------------------------------

# 15. Errores comunes de rendimiento

-   Utilizar `SELECT *` innecesariamente.
-   No filtrar con `WHERE`.
-   Realizar JOIN sobre columnas no indexadas.
-   Utilizar funciones sobre columnas filtradas.
-   Abusar de subconsultas cuando un CTE mejora la legibilidad.
-   Crear índices en exceso.

------------------------------------------------------------------------

# 16. Checklist para escribir buen SQL

Antes de dar por terminada una consulta verifica:

-   ¿El resultado es correcto?
-   ¿Los nombres son descriptivos?
-   ¿La consulta puede simplificarse?
-   ¿Existe un JOIN innecesario?
-   ¿Se utilizan únicamente las columnas requeridas?
-   ¿Es fácil de entender por otra persona?

------------------------------------------------------------------------

# 17. SQL en entrevistas técnicas

Las entrevistas para Data Analyst suelen incluir ejercicios sobre:

-   GROUP BY.
-   JOIN.
-   Window Functions.
-   Duplicados.
-   Top N.
-   Ranking.
-   Fechas.
-   Subconsultas.
-   CTE.

Más importante que memorizar soluciones es comprender los patrones que
resuelven estos problemas.

------------------------------------------------------------------------

# 18. Conclusiones

Has recorrido desde los conceptos básicos hasta técnicas utilizadas
diariamente en proyectos profesionales.

Los siguientes pasos recomendados son:

-   Practicar con bases de datos reales.
-   Resolver ejercicios en plataformas como HackerRank o LeetCode.
-   Aprender las funciones específicas del motor SQL que utilices.
-   Integrar SQL con Python, Pandas, Power BI o herramientas de
    visualización.

SQL continúa siendo una de las habilidades más demandadas en Ciencia de
Datos y Business Intelligence. Dominarlo te permitirá acceder,
transformar y analizar información de forma eficiente en prácticamente
cualquier organización.

