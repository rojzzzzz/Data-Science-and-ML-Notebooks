

## Parte 1 -- GROUP BY, HAVING, JOIN y UNION

> Este capítulo marca la transición desde consultas básicas hacia
> consultas utilizadas en análisis de datos y reportes empresariales. El
> objetivo no es memorizar sintaxis, sino comprender cómo SQL trabaja
> con conjuntos de datos.

------------------------------------------------------------------------

# Tabla de contenido

1.  Introducción
2.  El procesamiento de una consulta SQL
3.  GROUP BY
4.  Funciones de agregación
5.  HAVING
6.  WHERE vs HAVING
7.  JOIN
8.  INNER JOIN
9.  LEFT JOIN
10. RIGHT JOIN
11. FULL OUTER JOIN
12. SELF JOIN
13. UNION
14. UNION ALL
15. Buenas prácticas
16. Errores comunes
17. Resumen

------------------------------------------------------------------------

# 1. Introducción

Hasta este punto hemos trabajado con registros individuales. Sin
embargo, la mayoría de los análisis requieren responder preguntas como:

-   ¿Cuántos clientes hay por país?
-   ¿Cuál es la venta promedio por mes?
-   ¿Qué productos nunca se han vendido?
-   ¿Qué empleados no tienen departamento asignado?

Responder este tipo de preguntas requiere dominar `GROUP BY` y `JOIN`.

------------------------------------------------------------------------

# 2. ¿Cómo procesa SQL una consulta?

Aunque escribimos:

``` sql
SELECT ...
FROM ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
```

El motor normalmente ejecuta la consulta en este orden:

``` text
1. FROM
2. JOIN
3. WHERE
4. GROUP BY
5. HAVING
6. SELECT
7. ORDER BY
8. LIMIT / FETCH
```

Comprender este orden evita muchos errores al escribir consultas
complejas.

------------------------------------------------------------------------

# 3. GROUP BY

`GROUP BY` agrupa filas que poseen el mismo valor en una o varias
columnas.

Ejemplo:

``` sql
SELECT department,
       COUNT(*) AS employees
FROM employees
GROUP BY department;
```

Conceptualmente, SQL transforma una tabla como:

``` text
IT
IT
HR
IT
Finance
HR
```

en grupos:

``` text
IT
 ├── registro
 ├── registro
 └── registro

HR
 ├── registro
 └── registro

Finance
 └── registro
```

Luego aplica funciones de agregación sobre cada grupo.

## Regla fundamental

Cuando utilizas `GROUP BY`, en el `SELECT` únicamente pueden aparecer:

-   Las columnas utilizadas en el `GROUP BY`.
-   Funciones de agregación.

Incorrecto:

``` sql
SELECT employee_name,
       department
FROM employees
GROUP BY department;
```

SQL no sabe qué empleado mostrar para cada departamento.

------------------------------------------------------------------------

# 4. Funciones de agregación

Las más utilizadas son:

``` sql
COUNT(*)
SUM()
AVG()
MIN()
MAX()
```

Pueden combinarse libremente.

``` sql
SELECT department,
       COUNT(*) AS employees,
       AVG(salary) AS avg_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary
FROM employees
GROUP BY department;
```

------------------------------------------------------------------------

# 5. HAVING

`HAVING` filtra grupos.

Mientras `WHERE` filtra filas, `HAVING` filtra el resultado de un
`GROUP BY`.

Ejemplo:

``` sql
SELECT department,
       COUNT(*) AS employees
FROM employees
GROUP BY department
HAVING COUNT(*) >= 5;
```

Solo se mostrarán departamentos con cinco o más empleados.

------------------------------------------------------------------------

# 6. WHERE vs HAVING

  -----------------------------------------------------------------------
  WHERE                             HAVING
  --------------------------------- -------------------------------------
  Filtra filas                      Filtra grupos

  Se ejecuta antes del GROUP BY     Se ejecuta después

  No utiliza funciones de           Normalmente utiliza agregaciones
  agregación                        
  -----------------------------------------------------------------------

Muchas consultas utilizan ambos.

------------------------------------------------------------------------

# 7. JOIN

Un JOIN combina información de dos o más tablas mediante una relación
lógica.

Generalmente dicha relación se establece entre una Primary Key y una
Foreign Key.

Ejemplo:

``` text
Employees                Departments

department_id ---------> id
```

Sin JOIN sería necesario duplicar información en todas las tablas.

------------------------------------------------------------------------

# 8. INNER JOIN

Devuelve únicamente los registros que existen en ambas tablas.

``` sql
SELECT e.name,
       d.department
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;
```

Visualmente:

``` text
Tabla A      Tabla B

 ○────●────○

Solo la intersección.
```

Es el JOIN más utilizado junto con LEFT JOIN.

------------------------------------------------------------------------

# 9. LEFT JOIN

Devuelve todos los registros de la tabla izquierda.

Si no existe coincidencia en la tabla derecha, SQL completa las columnas
con NULL.

``` sql
SELECT *
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;
```

Visualmente:

``` text
██████●●●
```

Todos los registros de la izquierda permanecen.

Es especialmente útil para encontrar elementos sin relación.

Ejemplo:

``` sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.department_id=d.id
WHERE d.id IS NULL;
```

Obtiene empleados sin departamento.

------------------------------------------------------------------------

# 10. RIGHT JOIN

Es el equivalente al LEFT JOIN intercambiando las tablas.

Muchos equipos prefieren reescribir la consulta utilizando LEFT JOIN por
cuestiones de legibilidad.

------------------------------------------------------------------------

# 11. FULL OUTER JOIN

Devuelve:

-   Todos los registros de la tabla izquierda.
-   Todos los registros de la tabla derecha.

Cuando no existe coincidencia aparecen valores NULL.

No todos los motores lo soportan de forma nativa.

------------------------------------------------------------------------

# 12. SELF JOIN

Una tabla puede unirse consigo misma.

Ejemplo clásico:

``` text
Employees

id
manager_id
```

Consulta:

``` sql
SELECT e.name,
       m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id;
```

Permite representar jerarquías.

------------------------------------------------------------------------

# 13. UNION

Une resultados de varias consultas.

``` sql
SELECT city
FROM customers

UNION

SELECT city
FROM suppliers;
```

Elimina duplicados automáticamente.

Las consultas deben devolver:

-   Igual número de columnas.
-   Tipos compatibles.

------------------------------------------------------------------------

# 14. UNION ALL

Funciona igual que UNION.

La diferencia es que conserva los registros repetidos.

``` sql
SELECT city
FROM customers

UNION ALL

SELECT city
FROM suppliers;
```

Es más rápido porque no necesita eliminar duplicados.

------------------------------------------------------------------------

# 15. Buenas prácticas

-   Utiliza siempre alias para tablas.
-   Especifica claramente la condición del JOIN.
-   Evita CROSS JOIN si no es intencional.
-   Utiliza LEFT JOIN cuando necesites conservar todos los registros de
    una tabla.
-   Utiliza UNION ALL cuando no sea necesario eliminar duplicados.
-   Prefiere nombres descriptivos para los alias.

------------------------------------------------------------------------

# 16. Errores comunes

## Usar WHERE en lugar de HAVING

Incorrecto:

``` sql
WHERE COUNT(*) > 5
```

Correcto:

``` sql
HAVING COUNT(*) > 5
```

------------------------------------------------------------------------

## Seleccionar columnas fuera del GROUP BY

Solo deben seleccionarse columnas agrupadas o funciones de agregación.

------------------------------------------------------------------------

## Olvidar la condición ON

Un JOIN sin ON suele generar un producto cartesiano enorme.

------------------------------------------------------------------------

## Confundir LEFT JOIN con INNER JOIN

Recuerda:

-   INNER → únicamente coincidencias.
-   LEFT → todas las filas de la izquierda.

------------------------------------------------------------------------

# 17. Resumen

En este capítulo aprendiste:

-   Cómo procesa SQL una consulta.
-   GROUP BY.
-   Funciones de agregación.
-   HAVING.
-   Diferencias entre WHERE y HAVING.
-   INNER JOIN.
-   LEFT JOIN.
-   RIGHT JOIN.
-   FULL OUTER JOIN.
-   SELF JOIN.
-   UNION.
-   UNION ALL.

Estos conceptos constituyen el núcleo del SQL utilizado en análisis de
datos y reportes empresariales.

---

## Parte 2 -- CASE, Subconsultas, VIEW y CTE


> En este capítulo aprenderás a construir consultas reutilizables y
> mucho más expresivas. Estos conceptos aparecen constantemente en
> proyectos reales de análisis de datos.



# Tabla de contenido

1.  CASE
2.  Cuándo utilizar CASE
3.  Funciones relacionadas
4.  Subconsultas
5.  Tipos de subconsultas
6.  Tablas derivadas
7.  EXISTS
8.  VIEW
9.  CTE (WITH)
10. ¿Subconsulta, VIEW o CTE?
11. Buenas prácticas
12. Errores comunes
13. Resumen

------------------------------------------------------------------------

# 1. CASE

`CASE` permite introducir lógica condicional dentro de una consulta SQL.

Sintaxis:

``` sql
CASE
    WHEN condición THEN resultado
    WHEN condición THEN resultado
    ELSE resultado
END
```

Ejemplo:

``` sql
SELECT
    employee_name,
    CASE
        WHEN salary < 2000 THEN 'Junior'
        WHEN salary < 5000 THEN 'Mid'
        ELSE 'Senior'
    END AS level
FROM employees;
```

Piensa en `CASE` como el equivalente al `if / else if / else` de un
lenguaje de programación.

------------------------------------------------------------------------

# 2. ¿Cuándo utilizar CASE?

Los usos más comunes son:

-   Clasificar registros.
-   Crear categorías.
-   Transformar valores.
-   Construir indicadores.
-   Crear columnas calculadas.

Ejemplo:

``` sql
CASE
WHEN age<18 THEN 'Minor'
ELSE 'Adult'
END
```

También puede utilizarse junto con funciones de agregación.

``` sql
SELECT
SUM(
CASE
WHEN status='Completed'
THEN amount
ELSE 0
END
)
FROM orders;
```

------------------------------------------------------------------------

# 3. Funciones relacionadas

En algunos motores existen funciones adicionales.

## COALESCE

Devuelve el primer valor que no sea NULL.

``` sql
SELECT COALESCE(phone,mobile,'No phone')
FROM customers;
```

## NULLIF

Devuelve NULL cuando dos expresiones son iguales.

``` sql
SELECT NULLIF(quantity,0);
```

Estas funciones suelen complementar a CASE.

------------------------------------------------------------------------

# 4. Subconsultas

Una subconsulta es una consulta dentro de otra consulta.

``` sql
SELECT *
FROM(
    SELECT *
    FROM employees
) t;
```

La consulta interna se ejecuta primero.

Su resultado se convierte temporalmente en una tabla.

------------------------------------------------------------------------

# 5. Tipos de subconsultas

## En SELECT

``` sql
SELECT
name,
(SELECT AVG(salary) FROM employees) AS company_average
FROM employees;
```

## En WHERE

``` sql
SELECT *
FROM employees
WHERE department_id IN(
    SELECT id
    FROM departments
);
```

## En FROM

``` sql
SELECT *
FROM(
    SELECT *
    FROM employees
) t;
```

Las subconsultas dentro de FROM reciben el nombre de **tablas
derivadas**.

------------------------------------------------------------------------

# 6. Tablas derivadas

Una tabla derivada existe únicamente durante la ejecución de la
consulta.

Ejemplo:

``` sql
SELECT
department,
AVG(salary)
FROM(
    SELECT *
    FROM employees
    WHERE active=TRUE
) e
GROUP BY department;
```

Son útiles para dividir problemas complejos en pasos más sencillos.

------------------------------------------------------------------------

# 7. EXISTS

`EXISTS` comprueba si una subconsulta devuelve al menos un registro.

``` sql
SELECT *
FROM customers c
WHERE EXISTS(
    SELECT 1
    FROM orders o
    WHERE o.customer_id=c.id
);
```

Se utiliza frecuentemente para verificar relaciones.

En muchos escenarios resulta más eficiente que `IN`.

------------------------------------------------------------------------

# 8. VIEW

Una VIEW es una consulta almacenada.

``` sql
CREATE VIEW active_employees AS
SELECT *
FROM employees
WHERE active=TRUE;
```

Después puede utilizarse como si fuera una tabla.

``` sql
SELECT *
FROM active_employees;
```

## Ventajas

-   Reutilización.
-   Consultas más sencillas.
-   Seguridad.
-   Ocultar complejidad.

Una VIEW normalmente **no almacena datos**; almacena la consulta.

------------------------------------------------------------------------

# 9. CTE (WITH)

Las Common Table Expressions son la forma moderna de escribir consultas
complejas.

``` sql
WITH active_employees AS(

SELECT *

FROM employees

WHERE active=TRUE

)

SELECT department,
COUNT(*)
FROM active_employees
GROUP BY department;
```

Ventajas:

-   Más legibles.
-   Fáciles de depurar.
-   Reutilizables dentro de la misma consulta.
-   Admiten recursividad en muchos motores.

Hoy en día muchos desarrolladores prefieren CTE sobre subconsultas
anidadas.

------------------------------------------------------------------------

# 10. ¿Subconsulta, VIEW o CTE?

  Técnica          Cuándo usarla
  ---------------- -------------------------------------
  Subconsulta      Consultas pequeñas y puntuales
  Tabla derivada   Dividir una consulta en pasos
  VIEW             Reutilizar consultas frecuentemente
  CTE              Consultas largas y complejas

No existe una única respuesta correcta; depende del problema.

------------------------------------------------------------------------

# 11. Buenas prácticas

-   Asigna nombres descriptivos a los CTE.
-   Evita anidar demasiadas subconsultas.
-   Prefiere CTE cuando la consulta pierda legibilidad.
-   Utiliza VIEW para lógica reutilizable.
-   Usa CASE únicamente cuando realmente exista una regla de negocio.

------------------------------------------------------------------------

# 12. Errores comunes

## Confundir VIEW con tabla

Una VIEW normalmente no almacena datos.

------------------------------------------------------------------------

## Abusar de CASE

Muchas condiciones pueden indicar que la lógica pertenece a una tabla de
referencia.

------------------------------------------------------------------------

## Subconsultas excesivamente profundas

Consultas difíciles de leer suelen indicar que conviene utilizar un CTE.

------------------------------------------------------------------------

## No asignar alias

Toda tabla derivada debe tener un alias.

Incorrecto:

``` sql
SELECT *
FROM(
SELECT *
FROM employees
);
```

Correcto:

``` sql
SELECT *
FROM(
SELECT *
FROM employees
) e;
```

------------------------------------------------------------------------

# 13. Resumen

En este capítulo aprendiste:

-   CASE.
-   COALESCE.
-   NULLIF.
-   Subconsultas.
-   Tablas derivadas.
-   EXISTS.
-   VIEW.
-   CTE (WITH).
-   Cuándo utilizar cada técnica.

Estos conceptos permiten construir consultas reutilizables, legibles y
escalables, fundamentales en proyectos de análisis de datos y Business
Intelligence.

