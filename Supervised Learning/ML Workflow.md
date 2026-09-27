---
tags:
  - machine-learning
  - workflow
  - checklist
  - cheatsheet
---
# Flujo de Trabajo General para un Proyecto de Machine Learning

> [!important]
> Este flujo de trabajo representa una guía general para la mayoría de proyectos de Machine Learning. Dependiendo del problema (clasificación, regresión, clustering, series de tiempo, NLP, visión por computadora, etc.), algunos pasos pueden variar, pero la filosofía general permanece prácticamente igual.

---

```text
                         NUEVO PROYECTO
                               │
                               ▼
                 1. Entender el problema
                               │
                               ▼
               2. Definir la métrica de éxito
                               │
                               ▼
                    3. Exploratory Data Analysis
                               │
                               ▼
                    4. Formular hipótesis
                               │
                               ▼
                  5. Preparación de los datos
                               │
                               ▼
                    6. Feature Engineering
                               │
                               ▼
                6. Train / Validation / Test Split
                               │
                               ▼
                     8. Modelo Baseline
                               │
                               ▼
                    9. Cross Validation
                               │
                               ▼
                    10. Entrenar modelos
                               │
                               ▼
                    11. Evaluación inicial
                               │
              ┌────────────────┴────────────────┐
              ▼                                 ▼
      ¿Underfitting?                    ¿Overfitting?
              │                                 │
      Sí ─────┘                                 └───── Sí
              ▼                                 ▼
 Mejorar Features                  Regularización
 Cambiar Modelo                    Simplificar Modelo
 Más Información                   Más Datos
              └────────────────┬─────────────────┘
                               ▼
                    12. Comparar modelos
                               │
                               ▼
                7. Hyperparameter Tuning
                               │
                               ▼
                 8. Interpretar el modelo
                               │
                               ▼
                      15. Modelo Final
                               │
                               ▼
               3. Deployment y Monitoreo
                               │
                               ▼
                    17. Reentrenamiento
```

---

# 1. Entender el problema

Antes de abrir el dataset debes comprender completamente el problema que intentas resolver.

Preguntas importantes:

- ¿Cuál es el objetivo del negocio?
- ¿Qué decisión apoyará este modelo?
- ¿Qué representa cada fila?
- ¿Qué representa cada columna?
- ¿Existe una variable objetivo?
- ¿Es un problema de clasificación, regresión, clustering o series de tiempo?
- ¿Qué restricciones existen (tiempo, costo, interpretabilidad, etc.)?

> [!warning]
> La mayoría de los errores en Machine Learning no provienen del algoritmo, sino de una mala comprensión del problema.

---

# 2. Definir la métrica de éxito

Antes de entrenar cualquier modelo debes responder una pregunta fundamental:

> **¿Cómo sabré si mi modelo es bueno?**

### Clasificación

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

### Regresión

- MAE
- MSE
- RMSE
- R²

### Clustering

- Inertia
- Silhouette Score

La métrica debe alinearse con el objetivo del negocio.

---

# 3. Exploratory Data Analysis (EDA)

El EDA permite comprender el comportamiento de los datos antes de construir cualquier modelo.

Incluye actividades como:

- Comprender cada variable.
- Detectar valores faltantes.
- Detectar valores atípicos.
- Analizar distribuciones.
- Analizar correlaciones.
- Detectar posibles errores.
- Visualizar relaciones entre variables.

> [!tip]
> Un buen EDA suele ahorrar muchas horas de entrenamiento de modelos.

---

# 4. Formular hipótesis

Después del EDA comienza el verdadero trabajo analítico.

Pregúntate:

- ¿Qué variables parecen importantes?
- ¿Existen relaciones no lineales?
- ¿Hay variables redundantes?
- ¿Existen interacciones interesantes?
- ¿Qué transformaciones podrían mejorar el modelo?

Las hipótesis guiarán el Feature Engineering.

---

# 5. Preparación de los datos

En esta etapa se limpian y preparan los datos.

Las tareas más comunes incluyen:

- Eliminar duplicados.
- Corregir tipos de datos.
- Tratar valores faltantes.
- Detectar inconsistencias.
- Codificar variables categóricas.
- Escalar variables numéricas.
- Normalizar cuando sea necesario.

---

# 6. Feature Engineering

Consiste en crear nuevas variables que ayuden al modelo a aprender mejor.

Ejemplos:

- Variables derivadas.
- Interacciones.
- Variables temporales.
- Agrupaciones.
- Transformaciones logarítmicas.
- Variables agregadas.
- Binning.

> [!important]
> En muchos proyectos, un buen Feature Engineering mejora más el rendimiento que cambiar de algoritmo.

---

# 7. Train / Validation / Test Split

Dividir correctamente los datos evita evaluar el modelo con información que ya vio durante el entrenamiento.

Generalmente:

- Training Set
- Validation Set
- Test Set

---

# 8. Modelo Baseline

Antes de construir modelos complejos, crea un modelo sencillo que sirva como referencia.

Ejemplos:

- Regresión Lineal
- Regresión Logística
- Árbol de Decisión simple

El objetivo es responder:

> ¿Realmente necesito un modelo más complejo?

---

# 9. Cross Validation

La validación cruzada permite comprobar si el modelo es estable.

En lugar de evaluar una sola partición del dataset, se realizan múltiples entrenamientos utilizando diferentes subconjuntos.

Beneficios:

- Reduce la dependencia de una única división.
- Produce estimaciones más confiables.
- Detecta modelos inestables.

---

# 10. Entrenar modelos

Ahora se entrenan distintos algoritmos.

Por ejemplo:

- Linear Regression
- Logistic Regression
- Decision Trees
- Random Forest
- Gradient Boosting
- XGBoost
- SVM
- KNN
- Neural Networks

No asumas desde el inicio cuál será el mejor.

---

# 11. Evaluación inicial

Evaluar el rendimiento utilizando la métrica seleccionada.

Además, analiza:

- Matriz de confusión.
- Curvas ROC.
- Curvas Precision-Recall.
- Residuos.
- Errores importantes.

---

# 12. Diagnóstico del modelo

## Underfitting

El modelo no logra aprender correctamente.

Posibles soluciones:

- Variables más informativas.
- Modelos más complejos.
- Reducir regularización.
- Más entrenamiento.

---

## Overfitting

El modelo memoriza el conjunto de entrenamiento.

Posibles soluciones:

- Más datos.
- Regularización.
- Simplificar el modelo.
- Early Stopping.
- Dropout.
- Feature Selection.

---

# 13. Comparar modelos

Compara varios modelos utilizando exactamente la misma métrica.

Considera además:

- Tiempo de entrenamiento.
- Interpretabilidad.
- Consumo de memoria.
- Facilidad de despliegue.
- Robustez.

---

# 14. Hyperparameter Tuning

Una vez seleccionado el mejor algoritmo, optimiza sus hiperparámetros.

Herramientas comunes:

- Grid Search
- Random Search
- Bayesian Optimization

---

# 15. Interpretar el modelo

Un buen modelo no solo debe ser preciso.

También debe ser comprensible.

Herramientas útiles:

- Feature Importance
- Coeficientes
- SHAP Values
- Permutation Importance
- Partial Dependence Plots

---

# 16. Modelo Final

Selecciona el modelo definitivo.

Documenta:

- Variables utilizadas.
- Transformaciones.
- Hiperparámetros.
- Métricas finales.
- Limitaciones.

---

# 17. Deployment y Monitoreo

Una vez en producción, el trabajo continúa.

Actividades comunes:

- Guardar el modelo.
- Exponerlo mediante una API.
- Integrarlo en aplicaciones.
- Monitorear su rendimiento.
- Detectar Data Drift.
- Detectar Concept Drift.
- Reentrenar cuando sea necesario.

---

# Filosofía del Workflow

> [!quote]
>
> Los algoritmos evolucionan.
>
> Las librerías cambian.
>
> Las APIs se actualizan.
>
> Pero un buen proceso de Machine Learning permanece prácticamente igual.
>
> Aprende primero el proceso.
>
> Después aprende los algoritmos.

---

# Regla de Platino

Antes de cambiar de algoritmo, pregúntate:

1. ¿Entendí realmente el problema?
2. ¿El EDA fue suficiente?
3. ¿Existe Data Leakage?
4. ¿Las variables tienen sentido?
5. ¿Mi baseline ya es razonable?

> [!success]
> En la mayoría de los proyectos de Machine Learning, **mejorar los datos produce un impacto mucho mayor que cambiar de algoritmo**.