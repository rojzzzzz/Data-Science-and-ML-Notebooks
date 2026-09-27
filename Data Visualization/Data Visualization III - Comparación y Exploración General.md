---
title: Data Visualization III - Comparación y Exploración General
tags:
  - python
  - data-visualization
  - matplotlib
  - seaborn
  - machine-learning
---

# 📊 Data Visualization III - Comparación y Exploración General

> [!info]
> Estos gráficos permiten comparar categorías, analizar correlaciones y obtener una visión general del conjunto de datos durante el Análisis Exploratorio de Datos (EDA).

## Contenido

- Bar Plot
- Heatmap
- Pair Plot

---

# 📊 Bar Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo para **comparar valores entre diferentes categorías**.

Ideal para:

- Comparar promedios.
- Comparar cantidades.
- Comparar frecuencias.
- Comparar grupos.

### Ejemplos

- Ventas por país.
- Salario promedio por departamento.
- Clientes por ciudad.
- Productos vendidos por categoría.

---

## ❌ No utilizar cuando

- Quieres analizar una distribución (Histogram).
- Quieres ver una tendencia temporal (Line Plot).
- Quieres estudiar la relación entre variables numéricas (Scatter Plot).

---

# 🐍 Python

## Matplotlib

### Función

```python
plt.bar()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

plt.bar(
    df["departamento"],
    df["ventas"]
)

plt.xlabel("Departamento")
plt.ylabel("Ventas")

plt.show()
```

---

## Seaborn

### Función

```python
sns.barplot()
```

### Ejemplo

```python
plt.figure(figsize=(8,5))

sns.barplot(
    data=df,
    x="departamento",
    y="ventas"
)

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Categórica + Numérica |
| Número de variables | 2 |
| Matplotlib | `plt.bar()` |
| Seaborn | `sns.barplot()` |

---

# 🔥 Heatmap

## 🎯 ¿Cuándo usarlo?

Utilízalo para visualizar la **correlación entre múltiples variables numéricas**.

Ideal para:

- Buscar variables altamente correlacionadas.
- Detectar multicolinealidad.
- Seleccionar variables para modelos.
- Obtener una visión rápida del dataset.

### Ejemplos

- Correlación entre variables financieras.
- Correlación entre variables médicas.
- Selección de variables para Machine Learning.

---

## ❌ No utilizar cuando

- Solo deseas comparar dos variables (Scatter Plot).
- Existen pocas variables.
- Las variables son categóricas.

---

# 🐍 Python

## Matplotlib

No posee una función específica para generar un Heatmap.

---

## Seaborn

### Función

```python
sns.heatmap()
```

### Ejemplo

```python
plt.figure(figsize=(8,6))

corr = df.corr(numeric_only=True)

sns.heatmap(
    corr,
    annot=True,
    cmap="coolwarm"
)

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numéricas |
| Número de variables | Varias |
| Matplotlib | — |
| Seaborn | `sns.heatmap()` |

---

# 🔍 Pair Plot

## 🎯 ¿Cuándo usarlo?

Utilízalo para explorar **todas las relaciones entre varias variables numéricas** al mismo tiempo.

Ideal para:

- Iniciar un EDA.
- Detectar correlaciones.
- Observar distribuciones.
- Identificar patrones.
- Detectar outliers.

### Ejemplos

- Dataset Iris.
- Dataset Wine.
- Dataset Housing.
- Cualquier conjunto de datos pequeño o mediano.

---

## ❌ No utilizar cuando

- El dataset contiene muchas variables (el gráfico será muy grande).
- El dataset tiene miles de registros (puede ser lento).
- Solo deseas comparar dos variables.

---

# 🐍 Python

## Matplotlib

No existe una función equivalente.

---

## Seaborn

### Función

```python
sns.pairplot()
```

### Ejemplo

```python
sns.pairplot(
    df[
        [
            "edad",
            "salario",
            "ventas",
            "gastos"
        ]
    ]
)

plt.show()
```

---

## 📌 Resumen

| Característica | Valor |
|---------------|-------|
| Variable | Numéricas |
| Número de variables | Varias |
| Matplotlib | — |
| Seaborn | `sns.pairplot()` |

---

# 🚀 Resumen rápido

| Gráfico | ¿Cuándo utilizarlo? | Función |
|---------|----------------------|----------|
| Bar Plot | Comparar categorías. | `plt.bar()` / `sns.barplot()` |
| Heatmap | Analizar correlaciones entre variables. | `sns.heatmap()` |
| Pair Plot | Explorar múltiples variables simultáneamente. | `sns.pairplot()` |