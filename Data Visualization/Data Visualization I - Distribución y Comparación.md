---
title: Data Visualization I - Distribución y Comparación
tags:
  - python
  - data-visualization
  - matplotlib
  - seaborn
  - machine-learning
---

# 📊 Data Visualization I - Distribución y Comparación

> [!info]
> Estos gráficos permiten comprender cómo se distribuyen los datos y comparar grupos antes de entrenar un modelo de Machine Learning.

## Contenido

- Histogram
- Box Plot
- Violin Plot

---

# 📊 Histogram (Histograma)

## 🎯 ¿Cuándo usarlo?

Utilízalo para analizar la **distribución de una variable numérica**.

Ideal para:

- Conocer la distribución de los datos.
- Detectar sesgos.
- Identificar concentraciones de valores.
- Detectar múltiples distribuciones.
- Analizar variables antes de entrenar un modelo.

### Ejemplos

- Edad
- Salario
- Precio
- Temperatura
- Ventas

---

## ❌ No utilizar cuando

- La variable es categórica.
- Quieres comparar categorías.
- Quieres estudiar la relación entre variables.

---

# 🐍 Python

## Matplotlib

### Función

```python
plt.hist()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

plt.hist(
    df["cnt"],
    bins=20,
    color="steelblue",
    edgecolor="black"
)

plt.title("Distribución de cnt")
plt.xlabel("cnt")
plt.ylabel("Frecuencia")

plt.show()
```

---

## Seaborn

### Función

```python
sns.histplot()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

sns.histplot(
    data=df,
    x="cnt",
    bins=20,
    kde=True
)

plt.title("Distribución de cnt")

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numérica |
| Número de variables | 1 |
| Matplotlib | `plt.hist()` |
| Seaborn | `sns.histplot()` |

---

# 📦 Box Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo para comparar la **distribución de una variable numérica entre diferentes categorías** y detectar valores atípicos (*outliers*).

Ideal para:

- Detectar outliers.
- Comparar grupos.
- Analizar dispersión.
- Comparar medianas.

### Ejemplos

- Salario por departamento.
- Ventas por región.
- Edad por género.
- Rentas por estación.

---

## ❌ No utilizar cuando

- Solo quieres conocer la distribución general de una variable.
- Quieres estudiar relaciones entre variables numéricas.

---

# 🐍 Python

## Matplotlib

### Función

```python
plt.boxplot()
```

### Ejemplo

```python
plt.figure(figsize=(7,5))

plt.boxplot(df["cnt"])

plt.title("Box Plot de cnt")

plt.show()
```

---

## Seaborn

### Función

```python
sns.boxplot()
```

### Ejemplo

```python
plt.figure(figsize=(9,5))

sns.boxplot(
    data=df,
    x="season",
    y="cnt"
)

plt.title("Rentas por estación")

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numérica + Categórica (opcional) |
| Número de variables | 1 o 2 |
| Matplotlib | `plt.boxplot()` |
| Seaborn | `sns.boxplot()` |

---

# 🎻 Violin Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo cuando quieras comparar distribuciones entre categorías y, además, visualizar la forma de la distribución.

Es una combinación entre un **Box Plot** y un **Histograma**.

Ideal para:

- Comparar distribuciones.
- Comparar densidades.
- Analizar grupos con muchos datos.

### Ejemplos

- Salario por departamento.
- Edad por género.
- Ventas por sucursal.

---

## ❌ No utilizar cuando

- Solo deseas detectar outliers rápidamente (usa Box Plot).
- Solo quieres ver la distribución de una variable (usa Histogram).

---

# 🐍 Python

## Matplotlib

No existe una función específica equivalente. Se recomienda utilizar Seaborn.

---

## Seaborn

### Función

```python
sns.violinplot()
```

### Ejemplo

```python
plt.figure(figsize=(9,5))

sns.violinplot(
    data=df,
    x="season",
    y="cnt"
)

plt.title("Distribución de rentas por estación")

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numérica + Categórica |
| Número de variables | 2 |
| Matplotlib | — |
| Seaborn | `sns.violinplot()` |

---

# 🚀 Resumen rápido

| Gráfico | ¿Cuándo utilizarlo? | Función |
|---------|----------------------|----------|
| Histogram | Ver la distribución de una variable numérica. | `plt.hist()` / `sns.histplot()` |
| Box Plot | Detectar outliers y comparar grupos. | `plt.boxplot()` / `sns.boxplot()` |
| Violin Plot | Comparar distribuciones completas entre categorías. | `sns.violinplot()` |