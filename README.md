# Anime — Base preliminar limpia para Aprendizaje Automático Inicial

Origen: `archive.zip` compartido en clase (anime.csv y rating.csv); el ZIP original no fue modificado.
Semilla reproducible: 20261104.

## Archivos
- `01_animes_limpios.csv` (11,830 animes): tabla principal con variables interpretables y DOS posibles objetivos (`rating_promedio` y `alta_calificacion`).
- `02_animes_numericos_para_modelos.csv` (11,830 animes): MISMAS filas que 01, variables ya transformadas en numéricas/indicadores para empezar a usar modelos.
- `03_valoraciones_muestra.csv` (36,000 interacciones): muestra con calificaciones de usuarios; relación con 01 mediante `anime_id`.
- `04_diccionario_variables.csv`: significado de las columnas.
- `05_resumen_limpieza.json`: conteos, filtros y detalles de muestreo.

## Requisitos de la tarea
Se cumplen: más de cinco características y usos posibles para aprendizaje supervisado y no supervisado.

## Modelos posibles
**Clasificación (por anime):** predecir `alta_calificacion` (1 si media >=7); nunca usar `rating_promedio` como predictor porque define exactamente el objetivo.
**Regresión (por anime):** predecir `rating_promedio`; nunca usar `alta_calificacion` como predictor.
**Clustering (por anime):** agrupar por variables de 02 SIN `anime_id`, `rating_promedio` ni `alta_calificacion`. Escalar las variables numéricas antes de K-means.
**Clasificación (por valoración):** predecir `le_gusto` usando variables de contenido en 03; no usar `calificacion_usuario` porque define el objetivo. `usuario_id` y `anime_id` son claves, no características numéricas significativas.

## Limpieza y limitaciones
- Se conservaron 11,830 de 12,294 animes con título, género, tipo, episodios positivos, número de miembros positivo y calificación promedio válida.
- Los 464 animes incompletos fueron excluidos de la versión inicial, SIN inventar ni imputar datos; esto puede excluir series en emisión cuyos episodios eran 'Unknown'.
- Los géneros originales (inglés) se mantienen y los títulos se decodificaron de HTML. Se generaron variables `log_miembros`, `log_episodios` y número de géneros.
- Las valoraciones -1 NO significan rechazo: representan ausencia de calificación y se excluyen de los objetivos supervisados.
- Se excluyeron valoraciones vinculadas a animes no admitidos en la versión limpia.
- La muestra de usuarios contiene 600 personas que tenían >=70 calificaciones reales y hasta 60 valoraciones de cada una, elegidas aleatoriamente con semilla 20261104, total 36,000. **No representa necesariamente a todos los usuarios**: favorece a usuarios activos. Se conservaron sus IDs solo para relacionar registros.
- No se ha dividido en entrenamiento/prueba ni escalado el conjunto: ambos procesos deben hacerse cuando se defina el modelo (para evitar fuga de información).
- Las puntuaciones de usuarios y la media del catálogo provienen del mismo ecosistema. Para predecir preferencias individuales, no incluir la media global del anime como predictor sin calcularla únicamente con el conjunto de entrenamiento.
- Son datos históricos de Kaggle, no estadísticas actuales de la plataforma.

## Lectura rápida en Colab
```python
import pandas as pd
catalogo = pd.read_csv('01_animes_limpios.csv')
numericos = pd.read_csv('02_animes_numericos_para_modelos.csv')
valoraciones = pd.read_csv('03_valoraciones_muestra.csv')
print(catalogo.shape, numericos.shape, valoraciones.shape)
```

## Variables que pueden introducirse en un modelo
En 02 utiliza las columnas entre `log_episodios` y los indicadores de géneros (excluye ID y objetivos). Para K-means transforma/estandariza y prueba k. Para clasificación puedes usar `alta_calificacion` y para regresión `rating_promedio` como objetivos independientes.
