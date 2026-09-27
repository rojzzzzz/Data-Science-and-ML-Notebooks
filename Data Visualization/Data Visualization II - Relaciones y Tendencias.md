---
title: Data Visualization II - Relaciones y Tendencias
tags:
  - python
  - data-visualization
  - matplotlib
  - seaborn
  - machine-learning
---

# 📈 Data Visualization II - Relaciones y Tendencias

> [!info]
> Estos gráficos permiten descubrir relaciones entre variables, analizar tendencias y verificar si existe una correlación antes de construir un modelo de Machine Learning.

## Contenido

- Scatter Plot
- Regression Plot
- Line Plot

---

# 📈 Scatter Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo para analizar la **relación entre dos variables numéricas**.

Ideal para:

- Detectar correlaciones.
- Encontrar patrones.
- Detectar clusters.
- Identificar outliers.
- Explorar relaciones antes de entrenar un modelo.

### Ejemplos

- Edad vs Salario
- Temperatura vs Ventas
- Horas estudiadas vs Nota
- Precio vs Demanda

---

## ❌ No utilizar cuando

- La variable X es categórica.
- Solo quieres ver la distribución de una variable.
- Quieres comparar categorías.

---

# 🐍 Python

## Matplotlib

### Función

```python
plt.scatter()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

plt.scatter(
    df["edad"],
    df["salario"]
)

plt.xlabel("Edad")
plt.ylabel("Salario")

plt.show()
```

---

## Seaborn

### Función

```python
sns.scatterplot()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

sns.scatterplot(
    data=df,
    x="edad",
    y="salario"
)

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numérica vs Numérica |
| Número de variables | 2 |
| Matplotlib | `plt.scatter()` |
| Seaborn | `sns.scatterplot()` |

---

# 📉 Regression Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo cuando quieras visualizar la relación entre dos variables y añadir una **línea de regresión**.

Ideal para:

- Ver si existe una tendencia lineal.
- Evaluar visualmente una regresión.
- Explorar relaciones antes de crear un modelo.

### Ejemplos

- Edad vs Salario
- Temperatura vs Ventas
- Publicidad vs Ingresos

---

## ❌ No utilizar cuando

- No deseas ajustar una línea de tendencia.
- Las variables no son numéricas.
- Solo quieres observar la distribución.

---

# 🐍 Python

## Matplotlib

No posee una función específica para generar automáticamente una línea de regresión.

---

## Seaborn

### Función

```python
sns.regplot()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

sns.regplot(
    data=df,
    x="edad",
    y="salario"
)

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numérica vs Numérica |
| Número de variables | 2 |
| Matplotlib | — |
| Seaborn | `sns.regplot()` |

---

# 📈 Line Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo para visualizar la **evolución de una variable a lo largo del tiempo o de una secuencia ordenada**.

Ideal para:

- Series temporales.
- Tendencias.
- Crecimiento o disminución.
- Comparar varias series.

### Ejemplos

- Ventas por mes.
- Temperatura diaria.
- Precio de una acción.
- Número de clientes por día.

---

## ❌ No utilizar cuando

- Los datos no tienen un orden natural.
- Quieres comparar categorías.
- Quieres analizar distribuciones.

---

# 🐍 Python

## Matplotlib

### Función

```python
plt.plot()
```

### Ejemplo

```python
plt.figure(figsize=(10,5))

plt.plot(
    df["fecha"],
    df["ventas"]
)

plt.xlabel("Fecha")
plt.ylabel("Ventas")

plt.show()
```

---

## Seaborn

### Función

```python
sns.lineplot()
```

### Ejemplo

```python
plt.figure(figsize=(10,5))

sns.lineplot(
    data=df,
    x="fecha",
    y="ventas"
)

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Tiempo + Numérica |
| Número de variables | 2 |
| Matplotlib | `plt.plot()` |
| Seaborn | `sns.lineplot()` |

---

# 🚀 Resumen rápido

| Gráfico         | ¿Cuándo utilizarlo?                                 | Función                               |
| --------------- | --------------------------------------------------- | ------------------------------------- |
| Scatter Plot    | Analizar la relación entre dos variables numéricas. | `plt.scatter()` / `sns.scatterplot()` |
| Regression Plot | Visualizar una relación con línea de regresión.     | `sns.regplot()`                       |
| Line Plot       | Mostrar la evolución de una variable en el tiempo.  | `plt.plot()` / `sns.lineplot()`       |