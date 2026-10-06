# Segmentación de perfiles ferroviarios con K-Means

**Asignatura:** Inteligencia Artificial\
**Institución:** Corporación Universitaria Iberoamericana (IBERO)\
**Actividad 4:** Métodos de aprendizaje no supervisado\
**Lenguaje:** Python

## Descripción

Proyecto académico de **aprendizaje no supervisado** orientado a descubrir perfiles de comportamiento de los trenes a partir de sus patrones históricos de puntualidad y retraso.

En lugar de agrupar observaciones individuales, el proyecto construye primero un **perfil por tren**. Cada perfil resume el comportamiento histórico mediante indicadores robustos de retraso, puntualidad, severidad y variabilidad. Sobre estos perfiles se aplica un único método de agrupamiento: **K-Means**.

El análisis utiliza el dataset público **Indian Railways - Delay Dataset** de Kaggle y realiza la descarga directamente con `kagglehub`.

## Problema

En un escenario no supervisado no existe una etiqueta que indique previamente a qué grupo pertenece cada tren. El problema consiste en identificar de forma automática trenes con comportamientos similares y separar perfiles de mayor puntualidad, retraso intermedio y retraso severo.

Agrupar directamente cientos de miles de observaciones produciría grupos difíciles de interpretar. Por esta razón, la unidad de análisis es el **tren**, representado por indicadores derivados de su comportamiento histórico.

## Datos utilizados

La fuente corresponde al dataset público de Kaggle:

**Indian Railways - Delay Dataset**  
`rxydenxd/indian-railways-delay-dataset`

Para esta actividad se utiliza la tabla:

```text
combined_delay
```

Sus datos principales son:

| **Campo** | **Descripción** |
| --- | --- |
| `train_no` | Identificador del tren. |
| `station_name` | Estación asociada a la observación. |
| `date` | Fecha del registro. |
| `delay` | Retraso observado en minutos. |

En la ejecución documentada se utilizaron **460.479 registros válidos**. A partir de ellos se construyeron perfiles para los trenes que contaban con al menos 20 observaciones, obteniendo finalmente **222 perfiles ferroviarios**.

Cada tren se resume inicialmente mediante retraso mediano, percentiles de retraso, porcentaje de observaciones a tiempo, porcentaje de retrasos severos, cantidad de observaciones y estaciones diferentes.

Las cinco variables utilizadas finalmente por K-Means son:

- `retraso_mediano`
- `retraso_p90`
- `iqr_retraso`
- `porcentaje_a_tiempo`
- `porcentaje_severo`

Se considera una observación **a tiempo** cuando su retraso es menor o igual a 15 minutos y un **retraso severo** cuando supera 30 minutos.

## Objetivo

Identificar grupos de trenes con patrones similares de puntualidad, severidad y variabilidad de retrasos, utilizando aprendizaje no supervisado y obteniendo clusters que puedan interpretarse mediante sus características operacionales.

## Características

- Descarga automática del dataset público mediante `kagglehub`.
- Limpieza y normalización de los registros de retraso.
- Corrección de la anomalía de rollover identificada en la fuente.
- Construcción de un perfil histórico por tren.
- Uso de medidas robustas como mediana, percentil 90 e intervalo intercuartílico.
- Cálculo del porcentaje de observaciones a tiempo y del porcentaje de retrasos severos.
- Exclusión de perfiles con menos de 20 observaciones.
- Estandarización de las variables antes del agrupamiento.
- Aplicación de un único método no supervisado: **K-Means**.
- Selección del número de clusters mediante **silhouette score**.
- Visualización de clusters en variables originales y de sus centroides.
- Gráfica de tamaño de los grupos, mapa de calor y boxplot de retrasos.
- 10 pruebas automatizadas de validación.

## Cómo funciona

1. **Descarga:** el notebook obtiene `combined_delay` desde Kaggle.
2. **Limpieza:** se normalizan nombres, tipos de datos y valores anómalos.
3. **Perfilado:** las observaciones se agrupan por `train_no` para construir un perfil histórico por tren.
4. **Variables:** se seleccionan cinco indicadores de puntualidad, severidad y dispersión del retraso.
5. **Escalamiento:** las variables se estandarizan con `StandardScaler` para hacerlas comparables.
6. **Selección de K:** se prueban valores entre 2 y 5 y se selecciona el que obtiene el mayor silhouette score.
7. **Clustering:** K-Means asigna cada tren a uno de los grupos encontrados.
8. **Interpretación:** se revisan centroides, tamaños, perfiles promedio y distribución del retraso por cluster.
9. **Validación:** se comprueba que todos los perfiles hayan sido asignados correctamente y que las salidas gráficas hayan sido generadas.

## Resultados principales

El mejor resultado de la ejecución documentada se obtuvo con **3 clusters** y un **silhouette score de 0,5350**.

La distribución fue:

| **Cluster** | **Trenes** | **Interpretación general** |
| --- | ---: | --- |
| 0 | 96 | Perfil de retraso intermedio-alto y baja puntualidad. |
| 1 | 42 | Perfil con los retrasos más severos y mayor variabilidad. |
| 2 | 84 | Perfil con mayor puntualidad y retrasos considerablemente menores. |

Los valores promedio de los perfiles fueron:

| **Cluster** | **Retraso mediano** | **Retraso P90** | **IQR retraso** | **% a tiempo** | **% severo** |
| --- | ---: | ---: | ---: | ---: | ---: |
| 0 | 79,67 | 247,13 | 127,70 | 21,45 | 67,57 |
| 1 | 328,98 | 734,45 | 391,86 | 9,13 | 88,45 |
| 2 | 6,92 | 38,96 | 15,42 | 74,58 | 11,18 |

Estos resultados permiten distinguir tres comportamientos ferroviarios claramente diferentes dentro de los datos analizados.

## Requisitos

- Python 3.10 o superior.
- Acceso a Internet para descargar el dataset desde Kaggle.
- Paquetes: `kagglehub`, `numpy`, `pandas`, `matplotlib` y `scikit-learn`.

## Instalación y ejecución

Descarga o clona el repositorio y abre una terminal en su carpeta. Se recomienda crear un entorno virtual:

```bash
python -m venv .venv
```

Actívalo en Windows (PowerShell):

```powershell
.\.venv\Scripts\Activate.ps1
```

O en macOS/Linux:

```bash
source .venv/bin/activate
```

Instala las dependencias:

```bash
python -m pip install kagglehub numpy pandas matplotlib scikit-learn
```

Para trabajar en Jupyter o Google Colab, abre:

```text
Actividad4_NoSupervisado_Perfiles_Trenes.ipynb
```

y ejecuta las celdas en orden.

Si se utiliza la versión independiente en Python:

```bash
python actividad4_no_supervisado.py
```

## Archivos de salida

Durante la ejecución, el notebook crea la carpeta `salidas_actividad4/` con:

| **Archivo** | **Contenido** |
| --- | --- |
| `eda_retraso_mediano.png` | Distribución del retraso mediano por tren. |
| `eda_puntualidad_p90.png` | Relación entre puntualidad y retraso P90. |
| `silhouette_k.png` | Comparación del silhouette score para seleccionar K. |
| `clusters_perfiles.png` | Visualización de los perfiles encontrados y sus centroides. |
| `tamano_clusters.png` | Cantidad de trenes asignados a cada cluster. |
| `heatmap_perfiles.png` | Perfil relativo de cada cluster. |
| `boxplot_retraso_cluster.png` | Comparación del retraso mediano entre clusters. |

Además, el notebook muestra las tablas de perfiles, el silhouette final y el resultado de las pruebas.

## Pruebas implementadas

| **ID** | **Caso de prueba** |
| --- | --- |
| P1 | Verificar que el dataset tenga registros disponibles. |
| P2 | Comprobar que se hayan construido suficientes perfiles de tren. |
| P3 | Validar que las variables de clustering no contengan valores nulos. |
| P4 | Comprobar que el escalamiento conserve la dimensión de la matriz. |
| P5 | Validar que el número seleccionado de clusters se encuentre dentro del rango evaluado. |
| P6 | Verificar que K-Means genere exactamente el número esperado de clusters. |
| P7 | Comprobar que el silhouette score se encuentre en su rango válido. |
| P8 | Validar que todos los trenes hayan recibido una asignación de cluster. |
| P9 | Verificar la generación de la gráfica de clusters. |
| P10 | Verificar la generación del mapa de calor de perfiles. |

Las pruebas se ejecutan automáticamente al finalizar el notebook. Si alguna condición no se cumple, se genera una excepción para hacer visible el fallo.

---

**Proyecto académico – Inteligencia Artificial, IBERO.**
