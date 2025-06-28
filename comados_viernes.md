
---

## 📁 Dataset: `decesos_personajes.csv`

### 🔍 Objetivo del análisis:

Contar cuántos personajes aparecen en cada uno de los libros de la saga:

* `GoT` (Game of Thrones)
* `CoK` (Clash of Kings)
* `SoS` (Storm of Swords)
* `FfC` (Feast for Crows)
* `DwD` (Dance with Dragons)

---

## 🧪 Código con comentarios a nivel ingeniería:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 🧠 Cargar el dataset
decesos = pd.read_csv('decesos_personajes.csv')

# 🧠 Lista de columnas que indican aparición en cada libro
libros = ['GoT', 'CoK', 'SoS', 'FfC', 'DwD']

# ✅ Sumamos los valores por columna para contar cuántos personajes aparecen en cada libro
apariciones_por_libro = decesos[libros].sum()

# 📊 Configuramos estilo del gráfico
sns.set_theme(style='whitegrid', palette='muted')

# 📈 Creamos el gráfico de barras
plt.figure(figsize=(8, 5))
sns.barplot(x=apariciones_por_libro.index, y=apariciones_por_libro.values)

# 🎨 Personalizamos
plt.title('Cantidad de personajes que aparecen en cada libro', fontsize=14)
plt.xlabel('Libro')
plt.ylabel('Cantidad de personajes')
plt.tight_layout()

# 👁️ Mostramos el gráfico
plt.show()
```

---

## 🧠 Explicación técnica:

* Cada columna `GoT`, `CoK`, etc., tiene **1 si el personaje aparece** en ese libro, **0 si no**.
* `.sum()` sobre esas columnas devuelve el **total de personajes que aparecen en cada libro**.
* Seaborn (`sns.barplot`) y Matplotlib (`plt`) se combinan para **visualizar los conteos** con buen diseño.

---

¿Querés que te lo prepare en un `.md` para agregarlo a tu repositorio? También puedo ayudarte a conectar esta lógica con otras ideas, como filtrar por casas o géneros.

¡Perfecto, Tony! Vamos a desarrollar el análisis **"Cantidad de capítulos antes de morir"**, con enfoque de ingeniería y visualización clara. Ideal para tu documentación o como base para modelos futuros.

---

## 📁 Dataset: `decesos_personajes.csv`

### 🔍 Objetivo:

Calcular **cuántos capítulos pasan entre la primera aparición del personaje y su muerte** dentro del mismo libro. Esto nos da una idea del **"tiempo narrativo de vida"** de cada personaje que murió.

---

## 🧪 Código explicado paso a paso:

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 🧠 Cargamos el dataset
decesos = pd.read_csv('decesos_personajes.csv')

# 🧹 Filtramos personajes que tengan tanto capítulo de introducción como capítulo de muerte
fallecidos = decesos[
    decesos['death_chapter'].notnull() & 
    decesos['book_intro_chapter'].notnull()
].copy()

# 🧮 Calculamos la "duración narrativa" en capítulos
fallecidos['capitulos_antes_de_morir'] = (
    fallecidos['death_chapter'] - fallecidos['book_intro_chapter']
).astype(int)

# 📊 Estilo general
sns.set_theme(style='whitegrid', palette='pastel')

# 📈 Visualización 1: Histograma
plt.figure(figsize=(8, 5))
sns.histplot(fallecidos['capitulos_antes_de_morir'], bins=20, kde=True)
plt.title('Distribución de capítulos entre introducción y muerte')
plt.xlabel('Capítulos antes de morir')
plt.ylabel('Cantidad de personajes')
plt.tight_layout()
plt.show()

# 📈 Visualización 2: Boxplot para ver mediana y outliers
plt.figure(figsize=(7, 2))
sns.boxplot(x=fallecidos['capitulos_antes_de_morir'], color='skyblue')
plt.title('Boxplot: capítulos antes de morir')
plt.xlabel('Capítulos antes de morir')
plt.tight_layout()
plt.show()
```

---

## 🧠 Explicación técnica:

* `death_chapter` y `book_intro_chapter` indican en qué capítulo muere y en cuál aparece por primera vez el personaje.
* Al restarlos, obtenemos cuántos capítulos vivió **dentro del mismo libro**.
* Se filtran nulos para evitar errores en la resta.
* El histograma muestra la **distribución general** de esa duración.
* El boxplot nos permite detectar **valores atípicos**, como personajes que vivieron muchos capítulos.

---

### ✅ Posibles hallazgos:

* ¿Muchos personajes mueren rápidamente?
* ¿Existen personajes que vivieron casi todo el libro antes de morir?
* ¿Hay outliers que puedan representar personajes importantes?

---


### 🔍 Objetivo:

Identificar qué **casas** (facción o lealtad de los personajes) tienen **más personajes activos en los libros 4 y 5**, que son los más avanzados cronológicamente.

Esto nos da una idea de:

* Qué casas mantienen relevancia en la narrativa a lo largo del tiempo.
* Posible protagonismo o supervivencia de ciertas casas.

---

## 🧪 Código completo con explicaciones:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 🧠 Cargamos el dataset
decesos = pd.read_csv('decesos_personajes.csv')

# 🧹 Eliminamos personajes sin casa definida para evitar 'nan' o nulos
decesos_filtrados = decesos[decesos['Allegiances'].notnull()].copy()

# 🧮 Agrupamos por casa y sumamos cuántos personajes aparecen en libros 4 (FfC) y 5 (DwD)
apariciones_posteriores = decesos_filtrados.groupby('Allegiances')[['FfC', 'DwD']].sum()

# 🧹 Quitamos casas con muy pocas apariciones totales para evitar ruido visual
apariciones_filtradas = apariciones_posteriores[apariciones_posteriores.sum(axis=1) > 1]

# 🎨 Configuramos el estilo visual
sns.set_theme(style='white', palette='pastel')

# 📊 Visualizamos como heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(apariciones_filtradas, annot=True, fmt='d', cmap='YlGnBu', linewidths=0.5)

# ✨ Personalizamos
plt.title('Apariciones por casa en libros 4 y 5 (FfC, DwD)')
plt.xlabel('Libro')
plt.ylabel('Casa')
plt.tight_layout()

# 👁️ Mostramos el gráfico
plt.show()
```

---

## 🧠 Explicación técnica:

* `groupby('Allegiances')`: agrupa por casa.
* `[['FfC', 'DwD']].sum()`: suma cuántos personajes de cada casa aparecen en libros 4 y 5.
* `sns.heatmap()`: representa la tabla como una matriz de calor donde los valores se codifican por color.
* `annot=True` y `fmt='d'`: muestra los valores numéricos enteros en cada celda del gráfico.
* Se filtran casas con baja aparición para evitar que el gráfico se vuelva ilegible.

---

### ✅ Posibles hallazgos:

* Casas como Stark, Lannister o Targaryen probablemente mantengan apariciones altas.
* Casas menores podrían desaparecer o no tener presencia en los libros finales.
* Este análisis es útil para correlacionar **persistencia narrativa con poder político**.

---

¿Querés que lo empaquete en `.md` o avanzo con el siguiente análisis sugerido? También podemos adaptar el heatmap para ver presencia en **todos los libros**, si lo querés aún más completo.


Perfecto, Tony. Vamos a desarrollar el análisis **"Relación entre género y aparición prolongada"**, con enfoque técnico y visual, ideal para documentar cómo varía la longevidad narrativa entre personajes masculinos y femeninos en *Juego de Tronos*.

---

## 📁 Dataset: `decesos_personajes.csv`

### 🔍 Objetivo:

Analizar si existe una diferencia significativa entre **géneros** en cuanto al **número de libros en los que aparece un personaje**.

Esto puede revelar:

* ¿Qué género tiene mayor "presencia narrativa"?
* ¿Hay sesgo o balance en la duración de participación?

---

## 🧪 Código detallado con explicación nivel ingeniería:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 🧠 Cargamos el dataset
decesos = pd.read_csv('decesos_personajes.csv')

# 🧮 Sumamos la aparición en libros por personaje (GoT a DwD = columnas binarias con 1 o 0)
libros = ['GoT', 'CoK', 'SoS', 'FfC', 'DwD']
decesos['total_libros'] = decesos[libros].sum(axis=1)

# 🧹 Filtramos filas con valores válidos en la columna 'Gender'
# En este dataset, usualmente 0 = mujer, 1 = hombre
decesos_validos = decesos[decesos['Gender'].isin([0, 1])].copy()

# 🎨 Estilo del gráfico
sns.set_theme(style='whitegrid', palette='pastel')

# 📈 Gráfico violinplot
plt.figure(figsize=(7, 5))
sns.violinplot(x='Gender', y='total_libros', data=decesos_validos)

# ✨ Personalización
plt.title('Distribución de libros en los que aparece un personaje según género')
plt.xlabel('Género (0 = mujer, 1 = hombre)')
plt.ylabel('Cantidad de libros en los que aparece')
plt.tight_layout()

# 👁️ Mostrar
plt.show()
```

---

## 🧠 Explicación técnica:

* `sum(axis=1)` suma en cada fila cuántos libros contiene un `1`, es decir, cuántos libros tiene presencia ese personaje.
* `Gender` es una variable codificada: 0 = femenino, 1 = masculino.
* `sns.violinplot(...)` muestra una combinación de:

  * Distribución (como un histograma suavizado).
  * Boxplot interno (mediana, cuartiles y outliers).
* Es ideal para comparar la **dispersión y densidad** de una variable numérica entre categorías (en este caso, género).

---

### ✅ Posibles hallazgos:

* ¿Hay un género que tiende a participar más tiempo en la historia?
* ¿Algún grupo tiene más personajes con poca aparición?
* ¿Existen diferencias significativas en mediana o distribución?

---

¿Te lo empaqueto también en `.md` o seguimos con el análisis 5 que es el de predicción para este dataset? También puedo ayudarte a traducir los valores de género a etiquetas `"Femenino"` y `"Masculino"` si querés que sea más legible para tus compañeros.


¡Vamos con todo, Tony! Este será nuestro primer análisis predictivo con el dataset `decesos_personajes.csv`: **predecir si un personaje muere o no** según sus características. Te lo explico paso a paso, con enfoque de ingeniería de datos y machine learning profesional.

---

## 🤖 Predicción: ¿Podemos predecir si un personaje muere?

### 🎯 Objetivo:

Construir un **modelo de clasificación binaria** para predecir si un personaje muere (`1`) o sobrevive (`0`), usando variables como género, nobleza y aparición en libros.

---

## 📁 Dataset: `decesos_personajes.csv`

---

### 📌 Paso 1: Importación de librerías

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix
import seaborn as sns
import matplotlib.pyplot as plt
```

---

### 📌 Paso 2: Carga y preparación de los datos

```python
# Cargamos el dataset
df = pd.read_csv('decesos_personajes.csv')

# Creamos la variable target: 1 si murió, 0 si no murió
df['murio'] = df['anio_muerte'].notnull().astype(int)

# Seleccionamos features relevantes
features = ['Gender', 'Nobility', 'GoT', 'CoK', 'SoS', 'FfC', 'DwD', 'book_intro_chapter']
df = df[features + ['murio']].dropna()  # Quitamos filas con NaN

# X: variables independientes, y: target
X = df[features]
y = df['murio']
```

---

### 📌 Paso 3: División de los datos

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)
```

---

### 📌 Paso 4: Escalado (opcional, más útil para regresión logística)

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

---

### 📌 Paso 5A: Modelo de Regresión Logística

```python
lr = LogisticRegression()
lr.fit(X_train_scaled, y_train)

y_pred_lr = lr.predict(X_test_scaled)

print('🔎 Logistic Regression')
print(confusion_matrix(y_test, y_pred_lr))
print(classification_report(y_test, y_pred_lr))
```

---

### 📌 Paso 5B: Modelo con Random Forest

```python
rf = RandomForestClassifier(n_estimators=100, random_state=42)
rf.fit(X_train, y_train)

y_pred_rf = rf.predict(X_test)

print('🌲 Random Forest')
print(confusion_matrix(y_test, y_pred_rf))
print(classification_report(y_test, y_pred_rf))
```

---

## 🧠 Interpretación de salida

* **Precision**: Qué tan bien predice las muertes verdaderas.
* **Recall**: Qué tanto de las muertes reales fueron detectadas.
* **F1-score**: Media armónica de precision y recall.
* **Confusion matrix**: muestra verdaderos positivos, falsos negativos, etc.

---

### 📊 Visualización opcional: Importancia de variables (solo para Random Forest)

```python
importances = rf.feature_importances_
features_importancia = pd.Series(importances, index=features).sort_values()

plt.figure(figsize=(8, 5))
features_importancia.plot(kind='barh')
plt.title('Importancia de variables en Random Forest')
plt.xlabel('Importancia')
plt.tight_layout()
plt.show()
```

---

### ✅ ¿Qué aprendimos?

* Este modelo predice **quién es más probable que muera** según:

  * Si es noble
  * Su género
  * Qué tan temprano aparece en el libro
  * En qué libros aparece
* Es una buena base para incorporar más features (como casa, capítulos, etc.)
* El modelo puede ampliarse con NLP si procesás texto de notas o descripciones.

---

¿Querés que este análisis te lo exporte también en `.md` bien formateado? ¿O querés ahora que armemos uno similar con el dataset de `batallas.csv`?
