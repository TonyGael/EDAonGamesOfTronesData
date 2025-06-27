---

## 📁 1) Dataset: `decesos_personajes.csv`

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

¿Querés que lo guarde como `.md` también o que avance con el siguiente análisis? Puedo ayudarte también a segmentarlo por casa o género si querés agregar más capas de análisis.
