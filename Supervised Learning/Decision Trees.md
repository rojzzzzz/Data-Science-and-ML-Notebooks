

![[Pasted image 20260608184609.png]]

## Definición  
  
Un **Decision Tree** es un algoritmo de **Machine Learning Supervisado** que puede utilizarse para:  
  
- Clasificación  
- Regresión  
  
Su objetivo es dividir los datos mediante una serie de preguntas hasta obtener grupos cada vez más homogéneos.  
  
---  
  
## Idea General  
  
El árbol realiza preguntas sobre las características de los datos.  
  
Ejemplo:  
  
```text  
¿Estudió más de 5 horas?  
/ \  
Sí No  
| |  
¿Asistió? Reprueba  
/ \  
Sí No  
| |  
Aprueba Reprueba  
```  
  
Cada pregunta genera una división (*split*) de los datos.  
  
---  
  
## Estructura de un Árbol  
  
### Nodo Raíz (Root Node)  
  
Es el nodo inicial.  
  
```text  
x[19] <= 0.5  
```  
  
Contiene todos los datos.  
  
---  
  
### Nodos Internos  
  
Representan preguntas o condiciones.  
  
```text  
x[17] <= 0.5  
```  
  
---  
  
### Ramas  
  
Representan las posibles respuestas.  
  
```text  
True  
False  
```  
  
---  
  
### Hojas (Leaf Nodes)  
  
Representan la decisión final.  
  
```text  
Clase 0  
Clase 1  
```  
  
---  
  
# Entropía  
  
## ¿Qué es?  
  
La entropía mide el nivel de incertidumbre o desorden de un conjunto de datos.  
  
### Fórmula  
  
$$  
H(S)=-\sum_{i=1}^{n} p_i \log_2(p_i)  
$$  
  
Donde:  
  
- $p_i$ = proporción de cada clase  
- $n$ = número de clases  
  
---  
  
## Interpretación  
  
### Entropía Máxima  
  
Cuando las clases están perfectamente mezcladas.  
  
Ejemplo:  
  
| Clase | Cantidad |  
|---------|---------|  
| 0 | 50 |  
| 1 | 50 |  
  
$$  
H(S)=1  
$$  
  
Hay máxima incertidumbre.  
  
---  
  
### Entropía Mínima  
  
Cuando todas las observaciones pertenecen a una sola clase.  
  
| Clase | Cantidad |  
|---------|---------|  
| 0 | 100 |  
| 1 | 0 |  
  
$$  
H(S)=0  
$$  
  
No existe incertidumbre.  
  
---  
  
## Caso Especial: Probabilidad Cero  
  
Si una clase no aparece:  
  
$$  
p=0  
$$  
  
La fórmula parece contener:  
  
$$  
0 \cdot \log_2(0)  
$$  
  
Pero matemáticamente:  
  
$$  
\lim_{p\to0} p\log_2(p)=0  
$$  
  
Por lo tanto, ese término simplemente se ignora.  
  
Ejemplo:  
  
$$  
H(S)=-(1\log_2(1))  
$$  
  
Como:  
  
$$  
\log_2(1)=0  
$$  
  
entonces:  
  
$$  
H(S)=0  
$$  
  
---  
  
# Information Gain  
  
## Definición  
  
Mide cuánto disminuye la entropía después de realizar una división.  
  
### Fórmula  
  
$$  
IG = H_{parent} - H_{after}  
$$  
  
---  
  
## Entropía Después del Split  
  
Se calcula mediante un promedio ponderado:  
  
$$  
H_{after}  
=  
\sum  
\left(  
\frac{|S_i|}{|S|}  
\right)  
H(S_i)  
$$  
  
Donde:  
  
- $S$ = conjunto padre  
- $S_i$ = conjuntos hijos  
  
---  
  
## Ejemplo  
  
Supongamos:  
  
$$  
H_{parent}=0.999  
$$  
  
y después del split:  
  
$$  
H_{after}=0.730  
$$  
  
Entonces:  
  
$$  
IG = 0.999 - 0.730  
$$  
  
$$  
IG = 0.269  
$$  
  
---  
  
## Interpretación  
  
| Information Gain | Calidad del Split |  
|-----------------|------------------|  
| 0 | No aporta nada |  
| Bajo | Poco útil |  
| Alto | Muy útil |  
| Máximo | Separación perfecta |  
  
---  
  
# Cómo Construye el Árbol  
  
El algoritmo:  
  
1. Calcula la entropía del nodo actual.  
2. Evalúa todas las divisiones posibles.  
3. Calcula el Information Gain de cada una.  
4. Selecciona la mejor.  
5. Repite el proceso en los nodos hijos.  
  
---  
  
# Implementación en Scikit-Learn  
  
## Importar Librerías  
  
```python  
from sklearn.tree import DecisionTreeClassifier  
from sklearn.model_selection import train_test_split  
```  
  
---  
  
## Separar Variables  
  
```python  
X = mushroom_dummy.drop('flg', axis=1)  
y = mushroom_dummy['flg']  
```  
  
### X  
  
Variables predictoras.  
  
```text  
cap_color  
gill_color  
odor  
...  
```  
  
### y  
  
Variable objetivo.  
  
```text  
flg  
```  
  
---  
  
## Dividir Datos  
  
```python  
X_train, X_test, y_train, y_test = train_test_split(  
X,  
y,  
random_state=0  
)  
```  
  
Por defecto:  
  
- 75% entrenamiento  
- 25% prueba  
  
---  
  
## Crear el Modelo  
  
```python  
model = DecisionTreeClassifier(  
criterion='entropy',  
max_depth=5,  
random_state=0  
)  
```  
  
### criterion='entropy'  
  
Utiliza:  
  
- Entropía  
- Information Gain  
  
para seleccionar divisiones.  
  
---  
  
### max_depth=5  
  
Limita la profundidad máxima del árbol.  
  
Ayuda a evitar:  
  
> Overfitting  
  
---  
  
### random_state=0  
  
Permite reproducir resultados.  
  
---  
  
## Entrenar  
  
```python  
model.fit(X_train, y_train)  
```  
  
Durante esta etapa el árbol aprende las reglas.  
  
---  
  
## Evaluar  
  
### Entrenamiento  
  
```python  
model.score(X_train, y_train)  
```  
  
### Prueba  
  
```python  
model.score(X_test, y_test)  
```  
  
---  
  
# Accuracy  
  
## Definición  
  
Porcentaje de predicciones correctas.  
  
### Fórmula  
  
$$  
Accuracy=  
\frac{  
Predicciones\ Correctas  
}{  
Total\ de\ Predicciones  
}  
$$  
  
---  
  
## Interpretación  
  
### Buen Modelo  
  
```text  
Train = 99%  
Test = 98%  
```  
  
Generaliza correctamente.  
  
---  
  
### Overfitting  
  
```text  
Train = 100%  
Test = 75%  
```  
  
Memorizó los datos.  
  
---  
  
### Underfitting  
  
```text  
Train = 70%  
Test = 68%  
```  
  
No aprendió suficientes patrones.  
  
---  
  
# Interpretación de un Árbol de Decisión  
  
Ejemplo:  
  
```text  
x[19] <= 0.5  
entropy = 0.999  
samples = 6093  
value = [3147, 2946]  
```  
  
---  
  
## x[19] <= 0.5  
  
Pregunta que realiza el árbol.  
  
```text  
Sí -> izquierda  
No -> derecha  
```  
  
---  
  
## entropy  
  
Nivel de incertidumbre del nodo.  
  
```text  
entropy = 0  
```  
  
Nodo puro.  
  
```text  
entropy ≈ 1  
```  
  
Nodo muy mezclado.  
  
---  
  
## samples  
  
Cantidad de observaciones que llegaron al nodo.  
  
Ejemplo:  
  
```text  
samples = 100  
```  
  
Significa que 100 registros alcanzaron ese nodo.  
  
---  
  
## value  
  
Distribución de clases.  
  
```text  
value = [82,18]  
```  
  
Significa:  
  
```text  
Clase 0 = 82  
Clase 1 = 18  
```  
  
La predicción será:  
  
```text  
Clase 0  
```  
  
porque es la mayoría.  
  
---  
  
# Ejemplo de Interpretación  
  
Nodo raíz:  
  
```text  
entropy = 0.999  
samples = 6093  
value = [3147,2946]  
```  
  
Las clases están casi equilibradas.  
  
---  
  
Nodo intermedio:  
  
```text  
entropy = 0.654  
samples = 3431  
value = [578,2853]  
```  
  
Predomina la clase 1.  
  
La entropía disminuyó.  
  
---  
  
Nodo hoja:  
  
```text  
entropy = 0  
samples = 2853  
value = [0,2853]  
```  
  
Todos pertenecen a la misma clase.  
  
No es necesario seguir dividiendo.  
  
---  
  
# Resumen para Examen  
  
- Un Decision Tree divide los datos mediante preguntas.  
- Utiliza Entropía para medir incertidumbre.  
- Utiliza Information Gain para seleccionar la mejor división.  
- Entropía alta = datos mezclados.  
- Entropía baja = datos homogéneos.  
- Entropía cero = nodo puro.  
- El árbol busca reducir la entropía en cada división.  
- `criterion='entropy'` utiliza Information Gain.  
- `max_depth` controla la complejidad del árbol.  
- `Accuracy` mide el porcentaje de predicciones correctas.  
- Los nodos hoja contienen la predicción final.