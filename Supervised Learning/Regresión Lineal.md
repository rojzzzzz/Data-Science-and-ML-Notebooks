---
date: 2026-06-01
area: tech
tags: [machine-learning, regresión]
status: budding
aliases: [Multiple Linear Regression, Regresión Lineal Múltiple]
related: [Basics of Machine Learning]
---

## Definición

La **Regresión Lineal** es un algoritmo de **aprendizaje supervisado** utilizado para predecir una variable numérica continua a partir de una o más variables explicativas.

Ejemplos:

- Precio de una casa
- Precio de un automóvil
- Temperatura
- Ventas mensuales
- Consumo eléctrico

---

# Intuición General

La regresión lineal busca encontrar la recta (o plano) que mejor describe la relación entre las variables de entrada y la variable objetivo.

```
Variables → Ecuación Lineal → Predicción Numérica
```

---

# Regresión Lineal Simple

Utiliza una única variable explicativa.

Modelo:

$$  
y = b + wx  
$$

Donde:

- $y$ = valor predicho
- $x$ = variable explicativa
- $w$ = coeficiente o pendiente
- $b$ = intercepto

Ejemplo:

$$  
Precio = 10000 + 150(Horsepower)  
$$

Interpretación:

- Cada caballo de fuerza adicional incrementa el precio estimado en $150.
- Cuando $Horsepower=0$, el precio estimado sería $10,000 (intercepto).

---

# Regresión Lineal Múltiple

Utiliza varias variables explicativas.

Modelo:

$$  
y = b + w_1x_1 + w_2x_2 + \cdots + w_nx_n  
$$

Ejemplo:

$$  
Precio = 10000 + 81.65(Horsepower) + 229.51(Height) + 1829.17(Width)  
$$

Interpretación:

- Cada coeficiente representa el cambio esperado en la variable objetivo cuando esa variable aumenta una unidad, manteniendo las demás constantes.

---

# Variables del Modelo

## Variable Objetivo (Target)

Es la variable que queremos predecir.

Ejemplo:

```
y = auto['price']
```

---

## Variables Explicativas (Features)

Son las variables utilizadas para realizar la predicción.

Ejemplo:

```
X = auto.drop('price', axis=1)
```

o

```
X = auto.drop(columns='price')
```

---

# Entrenamiento del Modelo

```
model = LinearRegression()

model.fit(X_train, y_train)
```

Durante el entrenamiento el algoritmo calcula:

$$  
w_1,w_2,\ldots,w_n,b  
$$

que producen el menor error posible.

---

# Función Objetivo

La regresión lineal minimiza la **Suma de Errores Cuadráticos (SSE)**:

$$  
\sum_{i=1}^{n}(y_i-f(x_i))^2  
$$

Donde:

- $y_i$ = valor real
- $f(x_i)$ = valor predicho

---

# ¿Por Qué Se Eleva al Cuadrado?

Sin elevar al cuadrado:

```
+5 + (-5) = 0
```

Los errores se cancelan.

Con el cuadrado:

```
25 + 25 = 50
```

Todos los errores contribuyen positivamente.

Además:

- Penaliza más los errores grandes.
- Facilita la optimización matemática.

---

# Interpretación de los Coeficientes

Ejemplo:

```
horsepower      81.65
height         229.51
width         1829.17
```

Interpretación:

### Horsepower

```
+1 unidad de horsepower
```

↓

```
+81.65 dólares en el precio esperado
```

---

### Width

```
+1 unidad de width
```

↓

```
+1829.17 dólares en el precio esperado
```

---

# Intercepto

Ejemplo:

```
Intercept = -128409
```

Representa la predicción cuando todas las variables explicativas son cero.

Generalmente no tiene interpretación física directa.

Su función principal es mejorar el ajuste matemático del modelo.

---

# Coeficiente de Determinación (R²)

Mide qué porcentaje de la variabilidad de la variable objetivo es explicado por el modelo.

$$  
R^2  
$$

Valores típicos:

|R²|Interpretación|
|---|---|
|0|No explica nada|
|0.5|Explica 50%|
|0.8|Explica 80%|
|1|Ajuste perfecto|

---

## Ejemplo

```
Train R² = 0.733
Test R² = 0.737
```

Interpretación:

```
El modelo explica aproximadamente el 73% de la variación del precio.
```

---

# Generalización

El objetivo del Machine Learning no es memorizar los datos.

El objetivo es funcionar correctamente con datos nuevos.

Por eso dividimos los datos en:

```
Training Set
```

y

```
Test Set
```

---

# Train/Test Split

```
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.5,
    random_state=0
)
```

---

## test_size

```
test_size=0.5
```

Significa:

```
50% entrenamiento
50% prueba
```

---

## random_state

Controla la semilla del generador pseudoaleatorio.

Ejemplo:

```
random_state=0
```

o

```
random_state=42
```

No son mejores ni peores.

Simplemente generan divisiones distintas pero reproducibles.

Utilizando:

- el mismo dataset
- el mismo código
- el mismo random_state

se obtendrán exactamente los mismos resultados incluso en otra computadora.

---

# Sobreajuste (Overfitting)

Ocurre cuando el modelo aprende demasiado bien los datos de entrenamiento.

Síntoma típico:

```
Train R² = 0.99
Test R² = 0.50
```

El modelo memoriza en lugar de generalizar.

---

## Buen Ajuste

```
Train R² = 0.77
Test R² = 0.76
```

Interpretación:

```
El modelo generaliza correctamente.
```

---

# Multicolinealidad

Ocurre cuando dos o más variables explicativas están altamente correlacionadas.

Ejemplo:

```
Width
Horsepower
```

pueden contener información similar.

---

## Consecuencias

- Coeficientes inestables.
- Interpretación difícil.
- Alta variabilidad en los coeficientes.

---

## Importante

La multicolinealidad:

❌ No necesariamente empeora las predicciones.

✅ Principalmente afecta la interpretación de los coeficientes.

---

# Tipos de Datos

Para entrenar el modelo las variables deben ser numéricas.

Permitidos:

```
int
float
bool
```

No permitidos directamente:

```
string
object
text
```

Ejemplo:

```
Toyota
Honda
Ford
```

debe convertirse a representación numérica antes de entrenar.

---

# Flujo Típico de Trabajo

```
X = auto.drop(columns='price')
y = auto['price']

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.5,
    random_state=0
)

model = LinearRegression()

model.fit(X_train, y_train)

print(model.score(X_train, y_train))
print(model.score(X_test, y_test))
```

---

# Resumen Rápido

```
Variables Explicativas (X)
↓
Ecuación Lineal
↓
Predicción Numérica (y)
```

La regresión lineal busca los coeficientes que minimizan la suma de errores cuadrados y utiliza R² para evaluar qué tan bien explica la variabilidad de la variable objetivo.