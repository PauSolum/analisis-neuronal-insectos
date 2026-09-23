# Taller 2 · De 16 canales a la figura

**Análisis neuronal en insectos asistido por IA**

Este repositorio contiene el desarrollo del **Taller 2**, enfocado en el procesamiento y análisis de registros electrofisiológicos extracelulares obtenidos con una **sonda de 16 canales**.

El objetivo principal es construir un pipeline capaz de detectar potenciales de acción, agruparlos en posibles unidades neuronales, evaluar la calidad de dichos grupos y determinar cuáles neuronas presentan una respuesta significativa ante diferentes condiciones de estímulo.

## Contenido

El análisis se encuentra en el notebook:

```text
Taller2_Jara_López.ipynb
```

El flujo de trabajo incluye:

1. Exploración de archivos HDF5 y geometría de la sonda.
2. Conversión de la señal a microvoltios.
3. Filtrado pasa-banda entre **300 y 5000 Hz**.
4. Detección de potenciales de acción mediante un umbral basado en la mediana del ruido.
5. Fusión de detecciones simultáneas entre los 16 canales.
6. Extracción de características de cada evento:

   * amplitud,
   * media anchura del valle,
   * centroide de profundidad.
7. Estandarización de características y agrupamiento mediante **K-means**.
8. Evaluación de métricas de calidad de cada grupo.
9. Análisis mediante autocorrelogramas y correlogramas cruzados.
10. Curación y clasificación de las unidades como `good`, `mua`, `non-somatic` o `noise`.
11. Verificación del pipeline utilizando la información oculta de los datos sintéticos.
12. Construcción de raster plots y PSTH.
13. Evaluación estadística de la respuesta al estímulo mediante:

* Z-score,
* t-test pareado,
* bootstrap con 5000 remuestreos e IC del 95 %.

14. Construcción de una figura final que resume los resultados.

## Datos

Se utilizan dos registros sintéticos obtenidos con la misma sonda:

| Archivo              | Descripción                                                                                       |
| -------------------- | ------------------------------------------------------------------------------------------------- |
| `taller2_limpia.h5`  | Grabación de 90 s con pocas unidades bien separadas y bajo nivel de ruido.                        |
| `taller2_ruidosa.h5` | Grabación de 200 s con tres condiciones de estímulo, actividad multiunitaria, ruido y artefactos. |

Los archivos `.h5` **no se incluyen en el repositorio debido a su tamaño**.

El notebook los descarga automáticamente desde Google Drive utilizando `gdown`, junto con el módulo `verificacion.py`.

**Carpeta de datos:**
https://drive.google.com/drive/folders/1L4t2SJW36ntvtpDvW3wQEUDLRLehXVGv

## Librerías utilizadas

El proyecto fue desarrollado en Python y utiliza principalmente:

```text
NumPy
Pandas
Matplotlib
Seaborn
SciPy
scikit-learn
h5py
gdown
```

## Ejecución

Se recomienda ejecutar el notebook en **Google Colab**.

1. Abrir `Taller2_Jara_López.ipynb`.
2. Ejecutar las celdas en orden desde el inicio.
3. La primera parte del notebook descarga automáticamente los datos necesarios.
4. Ejecutar el notebook completo de arriba hacia abajo.

Debido al tamaño de las grabaciones, se recomienda no cargar simultáneamente los dos archivos `.h5` si existen limitaciones de memoria.

## Resultados principales

Para la grabación ruidosa se seleccionó inicialmente **K = 24** para el agrupamiento mediante K-means. Después de la evaluación y fusión de grupos se obtuvieron **18 grupos**, de los cuales:

* **4** fueron clasificados como `good`.
* **9** como `mua`.
* **0** como `non-somatic`.
* **5** como `noise`.

La fracción de eventos provenientes de una unidad identificable aumentó de **0.653 antes de la curación a 0.959 después de la curación**.

Las cuatro unidades `good` identificadas fueron las unidades **0, 5, 9 y 17**. El análisis de su actividad frente a los estímulos mostró:

| Unidad | Condiciones con respuesta |
| ------ | ------------------------- |
| 0      | A, B y C                  |
| 5      | A y C                     |
| 9      | A y C                     |
| 17     | A, B y C                  |

La respuesta de cada unidad fue evaluada comparando su actividad basal con la actividad durante el estímulo mediante **Z-score, t-test pareado e intervalos de confianza bootstrap del 95 %**.

## Figura final

El análisis termina con una figura científica de cuatro paneles que resume:

* **A.** Ubicación de las unidades `good` en la sonda y sus formas de onda promedio.
* **B.** Raster plot de una unidad seleccionada para las condiciones A, B y C.
* **C.** PSTH promedio de las unidades `good` para cada condición.
* **D.** Distribución de las tasas de disparo durante el estímulo mediante swarmplot e intervalos de confianza.

## Autores

**Jara · López**

Taller 2 — Análisis de registros electrofisiológicos.
