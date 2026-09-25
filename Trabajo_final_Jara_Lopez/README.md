# Análisis neuronal en VISp con registros Neuropixels

## Descripción

Este proyecto analiza la actividad neuronal de la corteza visual primaria del ratón (**VISp / V1**) a partir de registros extracelulares obtenidos con sondas **Neuropixels**.

Los datos provienen del **Allen Brain Observatory – Visual Coding Neuropixels** y corresponden a ratones despiertos expuestos a diferentes estímulos visuales. Las unidades neuronales ya se encuentran identificadas y filtradas por criterios de calidad.

Para este proyecto se analizaron:

- **382 neuronas**
- **5 ratones**
- Registros de la región **VISp**
- Diferentes tipos de estímulos visuales

La comparación principal asignada fue:

> **Rejillas en movimiento vs. rejillas estáticas**

Además, se calculó el eje común del curso:

> **Fracción de neuronas que responden a rejillas en movimiento frente a actividad espontánea.**

---

## Objetivo

Caracterizar la respuesta neuronal de VISp frente a diferentes estímulos visuales, con énfasis en la comparación entre **rejillas en movimiento** y **rejillas estáticas**.

El análisis incluye:

- cálculo de tasas de disparo;
- caracterización de actividad basal;
- selección de la condición preferida de cada neurona;
- cálculo de Z-scores;
- clasificación de neuronas respondedoras;
- PSTH individuales y poblacionales;
- mapas de calor;
- comparación entre condiciones;
- análisis de variabilidad entre animales;
- estadística mediante bootstrap jerárquico.

---

## Datos

El archivo `.npz` contiene tres grupos principales de variables:

### Variables neuronales `u_`

Incluyen información de cada unidad neuronal, como:

- identificador;
- ratón;
- área cerebral;
- canal;
- profundidad;
- métricas de calidad;
- tasa de disparo;
- forma de onda promedio.

### Variables de ensayos `e_`

Describen cada presentación de estímulo:

- clase de estímulo;
- ratón;
- tiempo de inicio;
- duración;
- orientación;
- frecuencia temporal;
- frecuencia espacial;
- contraste;
- color;
- imagen.

### Variables de potenciales de acción `s_`

Cada entrada representa una espiga y contiene:

- neurona;
- ensayo;
- tiempo de la espiga respecto al inicio del estímulo.

---

## Estímulos analizados

Los registros contienen las siguientes clases de estímulos:

- `drifting_gratings`: rejillas en movimiento;
- `static_gratings`: rejillas estáticas;
- `natural_scenes`: escenas naturales;
- `natural_movie_one`: película natural;
- `flashes`: destellos;
- `spontaneous`: actividad espontánea con pantalla gris.

---

## Pipeline de análisis

### 1. Exploración de los datos

Se verificó el número de neuronas, animales, ensayos y potenciales de acción contenidos en el archivo.

Para VISp se encontraron:

- **382 neuronas**
- **5 ratones**

---

### 2. Cálculo de tasas de disparo

Para cada neurona se calcularon matrices de tasa de disparo durante:

- rejillas en movimiento;
- actividad espontánea.

Las combinaciones neurona-ensayo pertenecientes a animales diferentes fueron excluidas utilizando valores `NaN`.

---

### 3. Actividad basal

Para cada neurona se calculó:

- media de la tasa espontánea;
- desviación estándar de la actividad espontánea.

Estos valores se utilizaron como referencia para calcular los Z-scores.

---

### 4. Condición preferida y Z-score

Las rejillas en movimiento contienen combinaciones de:

- dirección;
- frecuencia temporal;
- frecuencia espacial.

Para cada neurona se calculó la respuesta a cada combinación y se seleccionó aquella que produjo el mayor Z-score.

El Z-score se calculó como:

\[
Z =
\frac{
FR_{\mathrm{evocada}} - FR_{\mathrm{basal}}
}{
\sigma_{\mathrm{basal}}
}
\]

Se utilizó como criterio de respuesta:

\[
Z \geq 2.5
\]

Con este criterio se encontraron:

- **187 neuronas respondedoras**
- **382 neuronas totales**
- **≈ 49 % de neuronas respondedoras a rejillas en movimiento**

---

## Figuras

### Figura 1 – Diseño experimental y neurona ejemplo

**Figura 1A.** Esquema experimental con ratón, pantalla, sonda Neuropixels y región VISp.

**Figura 1B.** Esquema de los estímulos visuales.

**Figura 1C.** Forma de onda promedio de una neurona ejemplo en siete canales.

**Figura 1D.** Raster y PSTH de la misma neurona ejemplo.

---

### Figura 2 – Análisis poblacional

**Figura 2A.** PSTH poblacional durante rejillas en movimiento.

**Figura 2B.** PSTH poblacionales separados por clase de estímulo y duración.

**Figura 2C.** Mapa de calor de Z-scores por neurona y estímulo.

**Figura 2D.** Distribución de Z-scores para rejillas en movimiento y rejillas estáticas.

**Figura 2E.** Fracción de neuronas respondedoras por ratón y condición.

---

## Comparación principal

La comparación asignada para VISp fue:

\[
\text{Rejillas en movimiento}
\quad \text{vs.} \quad
\text{Rejillas estáticas}
\]

Para cada ratón se calculó la fracción de neuronas respondedoras en ambas condiciones y se visualizaron mediante puntos conectados.

Esto permite conservar la estructura experimental y observar la variabilidad entre animales.

---

## Bootstrap jerárquico

Las neuronas de un mismo animal no pueden considerarse observaciones completamente independientes.

Por esta razón, el intervalo de confianza se obtiene mediante un **bootstrap jerárquico de tres niveles**:

1. remuestreo de ratones;
2. remuestreo de neuronas dentro de cada ratón;
3. remuestreo de ensayos dentro de cada neurona.

El procedimiento se repite **10 000 veces**.

El intervalo de confianza del 95 % se obtiene utilizando los percentiles:

\[
2.5\% \quad \text{y} \quad 97.5\%
\]

de la distribución bootstrap.

---

## Requisitos

El análisis fue realizado en Python utilizando principalmente:

```text
numpy
pandas
matplotlib
gdown
