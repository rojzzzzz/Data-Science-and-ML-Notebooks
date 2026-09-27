
## Parte 1 -- Bases de Datos y Manipulación de Datos

> **Objetivo:** Este documento constituye un manual de referencia de
> SQL. No está basado en un motor específico (DuckDB, PostgreSQL, MySQL,
> SQL Server u Oracle), sino en SQL estándar, indicando cuando existan
> diferencias importantes.

------------------------------------------------------------------------

# Tabla de contenido

1.  Introducción
2.  ¿Qué es una Base de Datos?
3.  ¿Qué es SQL?
4.  SQL vs DBMS
5.  Bases de Datos Relacionales
6.  Tablas, Filas y Columnas
7.  Claves Primarias
8.  Tipos de Datos
9.  Restricciones
10. CREATE TABLE
11. INSERT
12. SELECT
13. Alias
14. UPDATE
15. DELETE
16. ALTER TABLE
17. Buenas Prácticas
18. Comparación con Python y Pandas
19. Resumen
20. Ejercicios

------------------------------------------------------------------------

# 1. Introducción

SQL (Structured Query Language) es el lenguaje estándar utilizado para
almacenar, consultar y manipular datos dentro de una base de datos
relacional.

Actualmente es uno de los conocimientos más importantes para:

-   Data Analyst
-   Data Scientist
-   Data Engineer
-   Business Intelligence
-   Machine Learning
-   Desarrollo Web
-   Ingeniería de Software

Aprender SQL no consiste únicamente en memorizar comandos. El objetivo
es aprender a **pensar en conjuntos de datos**, una habilidad que
posteriormente utilizarás con Pandas, Spark y herramientas de análisis.

------------------------------------------------------------------------

# 2. ¿Qué es una Base de Datos?

Una base de datos es un sistema diseñado para almacenar información de
forma organizada y permitir su consulta de manera rápida y segura.

Ejemplos de información almacenada:

-   Clientes
-   Productos
-   Ventas
-   Inventarios
-   Empleados
-   Donaciones
-   Pacientes
-   Estudiantes

A diferencia de una hoja de cálculo, una base de datos permite trabajar
con millones de registros manteniendo integridad y consistencia.

> **Nota:** Excel es excelente para análisis manuales. SQL es la
> herramienta adecuada cuando los datos crecen y múltiples usuarios
> trabajan sobre ellos.

------------------------------------------------------------------------

# 3. ¿Qué es SQL?

SQL significa **Structured Query Language**.

Es un lenguaje declarativo.

En un lenguaje declarativo indicamos **qué queremos obtener**, no
**cómo** debe obtenerse.

Ejemplo:

``` sql
SELECT *
FROM employees;
```

No indicamos cómo recorrer la tabla; el motor decide la estrategia más
eficiente.

------------------------------------------------------------------------

# 4. SQL vs DBMS

Es importante diferenciar estos conceptos.

  SQL                       DBMS
  ------------------------- ------------------------------------
  Lenguaje                  Software
  Permite consultar datos   Ejecuta las consultas
  Es un estándar            Tiene implementaciones específicas

Ejemplos de DBMS:

-   PostgreSQL
-   MySQL
-   SQL Server
-   Oracle
-   SQLite
-   DuckDB

La mayor parte de la sintaxis es común entre ellos.

------------------------------------------------------------------------

# 5. Bases de Datos Relacionales

Una base de datos relacional organiza la información en tablas
relacionadas mediante claves.

Ejemplo conceptual:

    Clientes
    ---------
    id
    nombre

    Pedidos
    --------
    id
    cliente_id
    fecha

La columna `cliente_id` permite relacionar ambas tablas.

Este diseño evita duplicar información y facilita el mantenimiento de
los datos.

------------------------------------------------------------------------

# 6. Tablas, Filas y Columnas

Una tabla representa una entidad del mundo real.

Ejemplo:

  id   nombre   edad
  ---- -------- ------
  1    Ana      25
  2    Luis     31

-   Cada **fila** representa un registro.
-   Cada **columna** representa un atributo.

Piensa en una tabla SQL como un **DataFrame** de Pandas almacenado
permanentemente.

------------------------------------------------------------------------

# 7. Clave Primaria (PRIMARY KEY)

Una Primary Key identifica de forma única cada registro.

Características:

-   No puede repetirse.
-   No puede ser NULL.
-   Identifica un registro de forma única.

Ejemplo:

``` sql
CREATE TABLE employees(
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

## Buenas prácticas

-   Utilizar un identificador artificial (`id`) cuando sea posible.
-   Evitar usar nombres, correos electrónicos o documentos nacionales
    como llave primaria si pueden cambiar.

------------------------------------------------------------------------

# 8. Tipos de Datos

Elegir correctamente el tipo de dato mejora el rendimiento y reduce
errores.

  Tipo         Uso
  ------------ ---------------------
  INT          Enteros
  BIGINT       Enteros grandes
  DECIMAL      Valores monetarios
  FLOAT        Valores aproximados
  VARCHAR(n)   Texto corto
  TEXT         Texto largo
  DATE         Fecha
  TIMESTAMP    Fecha y hora
  BOOLEAN      Verdadero/Falso

> **Consejo:** Para dinero, utiliza `DECIMAL` en lugar de `FLOAT` para
> evitar errores de precisión.

------------------------------------------------------------------------

# 9. Restricciones

Las restricciones ayudan a garantizar la calidad de los datos.

Las más utilizadas son:

-   PRIMARY KEY
-   NOT NULL
-   UNIQUE
-   DEFAULT
-   CHECK

Ejemplo:

``` sql
CREATE TABLE products(
    id INT PRIMARY KEY,
    price DECIMAL(10,2) CHECK(price >= 0),
    stock INT DEFAULT 0
);
```

------------------------------------------------------------------------

# 10. CREATE TABLE

Sintaxis general:

``` sql
CREATE TABLE table_name(
    column datatype constraint,
    ...
);
```

Ejemplo:

``` sql
CREATE TABLE employees(
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT
);
```

------------------------------------------------------------------------

# 11. INSERT

Insertar una fila:

``` sql
INSERT INTO employees(id,name,age)
VALUES(1,'Ana',25);
```

Insertar varias filas:

``` sql
INSERT INTO employees(id,name,age)
VALUES
(2,'Luis',31),
(3,'María',28);
```

Siempre que sea posible especifica las columnas.

------------------------------------------------------------------------

# 12. SELECT

Consultar todas las columnas:

``` sql
SELECT *
FROM employees;
```

Consultar columnas específicas:

``` sql
SELECT
    name,
    age
FROM employees;
```

Evita `SELECT *` en sistemas de producción cuando solo necesites unas
pocas columnas.

------------------------------------------------------------------------

# 13. Alias

Los alias mejoran la legibilidad.

``` sql
SELECT
    name AS Employee,
    age AS Age
FROM employees;
```

También pueden utilizarse para tablas:

``` sql
SELECT e.name
FROM employees AS e;
```

------------------------------------------------------------------------

# 14. UPDATE

Actualizar registros:

``` sql
UPDATE employees
SET age = 30
WHERE id = 1;
```

> ⚠️ Nunca ejecutes un `UPDATE` sin revisar el `WHERE`.

------------------------------------------------------------------------

# 15. DELETE

Eliminar registros:

``` sql
DELETE
FROM employees
WHERE id = 1;
```

Sin `WHERE`, se eliminarán todas las filas.

------------------------------------------------------------------------

# 16. ALTER TABLE

Modificar la estructura de una tabla.

Agregar columna:

``` sql
ALTER TABLE employees
ADD salary DECIMAL(10,2);
```

La sintaxis para renombrar columnas puede variar según el motor.

------------------------------------------------------------------------

# 17. Buenas Prácticas

-   Utiliza nombres descriptivos.
-   Declara una Primary Key.
-   Especifica las columnas en `INSERT`.
-   Usa `DECIMAL` para dinero.
-   Revisa siempre el `WHERE` antes de ejecutar `UPDATE` o `DELETE`.
-   Evita `SELECT *` cuando no sea necesario.
-   Documenta tus consultas complejas.

------------------------------------------------------------------------

# 18. SQL vs Python/Pandas

  SQL        Pandas
  ---------- -------------------
  Tabla      DataFrame
  SELECT     df\[...\]
  WHERE      Filtrado booleano
  INSERT     Concatenar filas
  UPDATE     loc\[\]
  DELETE     drop()
  GROUP BY   groupby()

Comprender esta equivalencia facilita el paso entre SQL y análisis de
datos con Python.

------------------------------------------------------------------------

# 19. Resumen

En esta primera parte aprendiste:

-   Conceptos fundamentales de bases de datos.
-   Qué es SQL.
-   Diferencia entre SQL y un DBMS.
-   Estructura de una tabla.
-   Tipos de datos.
-   Restricciones.
-   CREATE TABLE.
-   INSERT.
-   SELECT.
-   Alias.
-   UPDATE.
-   DELETE.
-   ALTER TABLE.
-   Buenas prácticas.

Estos conceptos constituyen la base sobre la cual construiremos
consultas más complejas en las siguientes partes.

------------------------------------------------------------------------


## Parte 2 -- Consultas, Filtros y Funciones de Agregación

> Este capítulo desarrolla las consultas básicas de SQL utilizando
> sintaxis estándar. Los ejemplos son independientes de un conjunto de
> datos específico para que puedan aplicarse a cualquier proyecto.

------------------------------------------------------------------------

# Tabla de contenido

1.  La sentencia SELECT
2.  WHERE
3.  Operadores de comparación
4.  Operadores lógicos
5.  ORDER BY
6.  DISTINCT
7.  LIMIT y FETCH
8.  IN
9.  BETWEEN
10. LIKE
11. Valores NULL
12. Funciones de agregación
13. CAST
14. Alias (AS)
15. Orden real de ejecución de una consulta SQL
16. Buenas prácticas
17. Resumen

------------------------------------------------------------------------

# 1. La sentencia SELECT

`SELECT` es la instrucción más utilizada en SQL. Permite recuperar
información de una o varias tablas.

Sintaxis general:

``` sql
SELECT columna1, columna2
FROM tabla;
```

Consultar todas las columnas:

``` sql
SELECT *
FROM employees;
```

Consultar únicamente algunas columnas:

``` sql
SELECT id, name
FROM employees;
```

> **Buena práctica:** evita `SELECT *` en aplicaciones y reportes.
> Solicita únicamente las columnas que realmente necesitas.

------------------------------------------------------------------------

# 2. WHERE

La cláusula `WHERE` filtra filas antes de devolver los resultados.

``` sql
SELECT *
FROM employees
WHERE age >= 18;
```

Piensa en `WHERE` como un filtro que decide qué registros continúan en
la consulta.

------------------------------------------------------------------------

# 3. Operadores de comparación

  Operador    Significado
  ----------- ---------------
  =           Igual
  \<\> o !=   Diferente
  \>          Mayor que
  \<          Menor que
  \>=         Mayor o igual
  \<=         Menor o igual

Ejemplo:

``` sql
SELECT *
FROM products
WHERE price >= 100;
```

------------------------------------------------------------------------

# 4. Operadores lógicos

Permiten combinar múltiples condiciones.

## AND

Todas las condiciones deben cumplirse.

``` sql
SELECT *
FROM employees
WHERE department = 'IT'
AND salary > 50000;
```

## OR

Basta con que una condición sea verdadera.

``` sql
SELECT *
FROM employees
WHERE city = 'Madrid'
OR city = 'Barcelona';
```

## NOT

Invierte una condición.

``` sql
SELECT *
FROM employees
WHERE NOT active;
```

------------------------------------------------------------------------

# 5. ORDER BY

Ordena los resultados.

Ascendente (por defecto):

``` sql
SELECT *
FROM employees
ORDER BY age;
```

Descendente:

``` sql
SELECT *
FROM employees
ORDER BY age DESC;
```

También puede ordenar por varias columnas.

``` sql
SELECT *
FROM employees
ORDER BY department, salary DESC;
```

------------------------------------------------------------------------

# 6. DISTINCT

Elimina duplicados.

``` sql
SELECT DISTINCT department
FROM employees;
```

Es útil para conocer los valores únicos existentes en una columna.

------------------------------------------------------------------------

# 7. LIMIT y FETCH

Muchos motores utilizan:

``` sql
SELECT *
FROM employees
LIMIT 10;
```

En SQL Server se utiliza:

``` sql
SELECT TOP 10 *
FROM employees;
```

En SQL estándar moderno también existe:

``` sql
FETCH FIRST 10 ROWS ONLY;
```

------------------------------------------------------------------------

# 8. IN

Equivale a múltiples comparaciones con OR.

En lugar de:

``` sql
WHERE department='IT'
OR department='HR'
OR department='Finance'
```

podemos escribir:

``` sql
WHERE department IN ('IT','HR','Finance');
```

El código es más corto y fácil de mantener.

------------------------------------------------------------------------

# 9. BETWEEN

Selecciona un rango inclusivo.

``` sql
SELECT *
FROM products
WHERE price BETWEEN 100 AND 500;
```

Es equivalente a:

``` sql
WHERE price >=100
AND price <=500;
```

------------------------------------------------------------------------

# 10. LIKE

Se utiliza para búsquedas parciales sobre texto.

Comodines más comunes:

-   `%` → cualquier cantidad de caracteres.
-   `_` → exactamente un carácter.

Ejemplos:

``` sql
WHERE name LIKE 'A%'
```

Empieza por A.

``` sql
WHERE name LIKE '%son'
```

Termina en "son".

``` sql
WHERE name LIKE '%abc%'
```

Contiene "abc".

------------------------------------------------------------------------

# 11. Valores NULL

`NULL` representa un valor desconocido o inexistente.

Nunca debe compararse con `=`.

Incorrecto:

``` sql
WHERE manager = NULL
```

Correcto:

``` sql
WHERE manager IS NULL;
```

o

``` sql
WHERE manager IS NOT NULL;
```

------------------------------------------------------------------------

# 12. Funciones de agregación

Estas funciones trabajan sobre un conjunto de filas y devuelven un único
resultado.

## COUNT

Cuenta registros.

``` sql
SELECT COUNT(*)
FROM employees;
```

## SUM

Suma valores.

``` sql
SELECT SUM(salary)
FROM employees;
```

## AVG

Calcula el promedio.

``` sql
SELECT AVG(age)
FROM employees;
```

## MIN

Obtiene el valor mínimo.

``` sql
SELECT MIN(price)
FROM products;
```

## MAX

Obtiene el valor máximo.

``` sql
SELECT MAX(price)
FROM products;
```

> Las funciones de agregación ignoran los valores `NULL`, excepto
> `COUNT(*)`, que cuenta todas las filas.

------------------------------------------------------------------------

# 13. CAST

`CAST` convierte un dato de un tipo a otro.

Sintaxis:

``` sql
CAST(expresion AS tipo)
```

Ejemplo:

``` sql
SELECT CAST(price AS DECIMAL(10,2));
```

Un uso frecuente es evitar la división entera:

``` sql
SELECT
CAST(SUM(amount) AS DECIMAL(10,2))
/
COUNT(*) AS average_amount
FROM sales;
```

------------------------------------------------------------------------

# 14. Alias (AS)

Los alias permiten renombrar columnas y tablas temporalmente.

``` sql
SELECT
name AS Employee,
salary AS MonthlySalary
FROM employees;
```

También pueden utilizarse para tablas.

``` sql
SELECT e.name
FROM employees AS e;
```

------------------------------------------------------------------------

# 15. Orden real de ejecución de SQL

Aunque escribimos:

``` text
SELECT
FROM
WHERE
ORDER BY
```

El motor normalmente procesa la consulta en este orden:

``` text
1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY
7. LIMIT / FETCH
```

Comprender este orden facilita el aprendizaje de consultas más
complejas.

------------------------------------------------------------------------

# 16. Buenas prácticas

-   Filtra con `WHERE` lo antes posible.
-   Evita `SELECT *` en producción.
-   Usa alias descriptivos.
-   Especifica siempre el criterio de orden cuando el orden importe.
-   Usa `IN` cuando compares contra varios valores.
-   Utiliza `IS NULL` para comprobar valores nulos.
-   Prefiere `AVG()` frente a `SUM()/COUNT()` cuando únicamente
    necesites un promedio.

------------------------------------------------------------------------

# 17. Resumen

En este capítulo aprendiste:

-   SELECT
-   WHERE
-   Operadores de comparación
-   Operadores lógicos
-   ORDER BY
-   DISTINCT
-   LIMIT
-   IN
-   BETWEEN
-   LIKE
-   NULL
-   Funciones de agregación
-   CAST
-   Alias
-   Orden de ejecución de SQL

Estos conceptos constituyen la base de prácticamente todas las consultas
SQL que utilizarás antes de comenzar con `GROUP BY`, `JOIN` y las
subconsultas.

