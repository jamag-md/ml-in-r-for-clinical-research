# Machine learning en R para investigación: un curso autoguiado

Un curso paso a paso para personas **sin experiencia en programación**.
Aprenderás qué hacen la regresión logística, LASSO, random forest, XGBoost y
LightGBM, los aplicarás a **datos clínicos públicos reales** y terminarás con
una **plantilla de R Markdown reutilizable** para tus propios estudios.

Cada lección es un archivo R Markdown (`.Rmd`) que se abre en RStudio. Cada
una incluye explicaciones de los conceptos en lenguaje sencillo, código con
**cada línea comentada**, preguntas para comprobar lo aprendido y ejercicios
breves.

> **Idioma del código:** las explicaciones y los comentarios están en
> español, pero los nombres de variables, funciones y categorías de los datos
> se mantienen en inglés. Así el código es idéntico al de la versión en
> inglés y coincide con la documentación de R.

---

## 1. Antes de empezar (una sola vez, ~30 min)

1. Instala **R**: <https://cran.r-project.org> (probado con R 4.6.1; cualquier versión reciente debería funcionar).
2. Instala **RStudio Desktop**: <https://posit.co/download/rstudio-desktop/>
3. Haz doble clic en **`curso_ML_en_R.Rproj`**. RStudio se abre *dentro* de
   esta carpeta. **Abre siempre el curso de esta manera** para que las rutas
   de los archivos funcionen.
4. Abre `00_configuracion_y_bases_de_R.Rmd` y síguelo. Instala los paquetes.

---

## 2. El mapa del curso

| # | Archivo | Qué aprenderás | Tiempo |
|---|---------|----------------|--------|
| 0 | `00_configuracion_y_bases_de_R.Rmd` | RStudio, R Markdown, las ~10 ideas de R que necesitas | 1–2 h |
| 1 | `01_importar_y_explorar_datos.Rmd` | Descargar datos reales de internet, limpiarlos, Tabla 1, gráficos, **división entrenamiento/prueba** | 2 h |
| 2 | `02_regresion_logistica.Rmd` | Odds ratios frente a predicción, el **flujo de 5 pasos de tidymodels**, validación cruzada, ROC/AUC, umbrales, **LASSO** y ajuste de hiperparámetros | 3 h |
| 3 | `03_arboles_y_random_forest.Rmd` | Árboles de decisión, random forests, ajuste de `mtry`/`min_n`, importancia por permutación | 2 h |
| 4 | `04_xgboost.Rmd` | Gradient boosting, hiperparámetros clave, cuadrículas space-filling, **valores SHAP** | 3 h |
| 5 | `05_lightgbm.Rmd` | LightGBM: boosting por hojas, manejo directo de categorías y datos faltantes | 1–2 h |
| 6 | `06_comparacion_de_modelos.Rmd` | IC 95% por bootstrap, comparación pareada, **calibración**, **análisis de curvas de decisión**, reporte con TRIPOD+AI | 3 h |
| 7 | `07_PLANTILLA_proyecto_de_investigacion.Rmd` | Un análisis completo con estructura de artículo sobre un **segundo** conjunto de datos, listo para copiar con tus propios datos | 2 h + |

**Haz las lecciones en orden.** La Lección 1 crea `data/`, las Lecciones 2–5
guardan resultados en `results/` y la Lección 6 los lee.

### Ritmo sugerido (más o menos una lección por semana)

* **Semana 1:** Lecciones 0–1. **Semana 2:** Lección 2 (la más importante:
  tómate tu tiempo).
* **Semana 3:** Lección 3. **Semana 4:** Lecciones 4–5. **Semana 5:** Lección 6.
* **Semana 6:** Lección 7, y después repítela con un conjunto de datos que
  elijas tú (ver sección 5).

---

## 3. Cómo trabajar cada lección

1. **Lee primero la sección del concepto.** No ejecutes nada todavía.
2. Ejecuta el código **bloque a bloque** con el ▶ verde (`Ctrl+Shift+Enter`).
3. Después de cada bloque, **mira el resultado** y vuelve a leer los
   comentarios.
4. Responde las preguntas de **"Comprueba lo que has entendido"** en voz alta
   o por escrito.
5. Haz los ejercicios de **"Pruébalo tú"**. Cambiar el código y ver qué se
   rompe es como se aprende.
6. Pulsa **Knit** (`Ctrl+Shift+K`) para generar el informe HTML.
7. Escribe tus propias notas directamente en el texto del `.Rmd`. Es tu
   cuaderno de laboratorio.

**Datos usados:**

* Lecciones 1–6: **Cleveland Heart Disease** (303 pacientes; predicción de
  enfermedad arterial coronaria). UCI ML Repository,
  <https://doi.org/10.24432/C52P4X>
* Lección 7: **Breast Cancer Wisconsin Diagnostic** (569 biopsias;
  predicción de malignidad). UCI ML Repository,
  <https://doi.org/10.24432/C5DW2B>

El código descarga ambos **directamente de internet**.

---

## 4. Aplicarlo a tu propio estudio

1. **Escribe primero la pregunta** (antecedentes, objetivos, definición del
   desenlace, predictores) en la plantilla, *antes* de analizar.
2. **Comprueba el tamaño muestral.** El número de *eventos* (desenlace =
   Yes) importa más que el número de pacientes. Con menos de ~100–200
   eventos, prefiere la regresión logística o LASSO y espera IC amplios.
   Consulta Riley et al., *BMJ* 2020;368:m441, y el paquete de R
   `pmsampsize`.
3. Copia `07_PLANTILLA_proyecto_de_investigacion.Rmd` y reemplaza **solo**
   los bloques de importación y limpieza de datos. Todo lo demás se adapta
   automáticamente siempre que crees `study_data` con una columna `outcome`
   (`"Yes"`/`"No"`, con `"Yes"` primero).
4. **Elimina las fugas de datos:** nada de identificadores, fechas, nada
   medido *después* del desenlace ni derivado de él.
5. Reporta según **TRIPOD+AI** (<https://www.tripod-statement.org>):
   discriminación, calibración, utilidad clínica, incertidumbre y código.
6. Recuerda: **validación interna ≠ validación externa**. Un modelo solo
   queda demostrado cuando se prueba en pacientes nuevos (otro hospital u
   otro periodo).

**¿Tu desenlace es un número (p. ej. presión arterial) en vez de sí/no?**
Usa `set_mode("regression")`, `linear_reg()` en lugar de `logistic_reg()` y
métricas como `metric_set(rmse, rsq, mae)`. Todo lo demás es igual.

**¿Tu desenlace es raro (< 10%)?** La exactitud deja de tener sentido.
Céntrate en el AUC, el PR-AUC (`pr_auc`), la calibración y las curvas de
decisión. *No* hagas sobremuestreo o submuestreo de forma automática:
distorsiona la calibración (van den Goorbergh et al., *JAMIA* 2022).
Ajustar el umbral suele ser mejor.

---

## 5. Dónde encontrar datos públicos para practicar e investigar

| Fuente | Qué contiene | Notas |
|--------|--------------|-------|
| **UCI ML Repository**: <https://archive.ics.uci.edu> | Cientos de conjuntos de datos: enfermedad cardíaca, reingresos por diabetes (100 000 episodios, bueno para LightGBM), enfermedad renal crónica, insuficiencia cardíaca | Gratis, descarga directa |
| **NHANES (CDC)**: <https://wwwn.cdc.gov/nchs/nhanes/> | Encuesta nacional de salud y nutrición de EE. UU.: laboratorio, exploración, cuestionarios | Paquete de R `nhanesA`. Los pesos muestrales importan para estimaciones poblacionales |
| **BRFSS (CDC)**: <https://www.cdc.gov/brfss/> | Gran encuesta de factores de riesgo de EE. UU. (más de 400 000 al año) | Buena práctica de "big data" para boosting |
| **PhysioNet**: <https://physionet.org> | Datos de UCI y fisiológicos (p. ej. la demo de MIMIC-IV es abierta) | MIMIC completo requiere una acreditación y formación gratuitas |
| **GEO / ArrayExpress** | Datos de expresión génica (ómicas) | Muchos predictores y pocos pacientes: terreno de LASSO |
| **Kaggle Datasets**: <https://www.kaggle.com/datasets> | Muchos conjuntos de datos de salud | Comprueba la fuente original y la licencia |
| **WHO GHO, Banco Mundial, Our World in Data** | Indicadores por país | Buenos para regresión (desenlaces numéricos) |
| Paquete de R **`medicaldata`** | Conjuntos de datos clínicos seleccionados para docencia | `install.packages("medicaldata")` |

Comprueba siempre la **licencia**, la forma de **citar** y los requisitos
**éticos**, y cita el conjunto de datos en tu artículo.

---

## 6. Glosario

| Término | Significado sencillo |
|---------|----------------------|
| **Sobreajuste** (*overfitting*) | El modelo memoriza los datos de entrenamiento (incluido el ruido) y funciona peor con pacientes nuevos |
| **Conjunto de entrenamiento / de prueba** | Datos para construir el modelo / datos ocultos hasta la evaluación final, única |
| **Validación cruzada (VC)** | Entrenar repetidamente con una parte de los datos de entrenamiento y evaluar con el resto, para estimar el rendimiento sin tocar el conjunto de prueba |
| **Hiperparámetro** | Un ajuste que eliges antes de entrenar (p. ej. número de árboles, tasa de aprendizaje). Se ajusta con VC |
| **Fuga de datos** (*data leakage*) | Información del conjunto de prueba (o del desenlace) que se cuela en el entrenamiento y da resultados demasiado optimistas |
| **Receta** (*recipe*) | La lista de pasos de preprocesamiento (imputación, dummies, escalado), aprendidos solo con los datos de entrenamiento |
| **ROC AUC / estadístico C** | Probabilidad de que el modelo ordene un caso al azar por encima de un no caso al azar (0,5 = azar, 1 = perfecto) |
| **Calibración** | Concordancia entre las probabilidades predichas y las frecuencias observadas |
| **Puntuación de Brier** | Error cuadrático medio de las probabilidades predichas (cuanto más baja, mejor) |
| **Sensibilidad / especificidad** | % de casos detectados correctamente / % de no casos descartados correctamente |
| **Beneficio neto / DCA** | Utilidad clínica de usar un modelo para decidir con un umbral de riesgo dado |
| **Bootstrap** | Remuestreo con reemplazo para estimar la incertidumbre (IC) |
| **Valor SHAP** | La contribución (+ o −) de una variable a la predicción de un paciente |
| **Importancia por permutación** | Caída del rendimiento al barajar al azar una variable |
| **Eventos por variable (EPV)** | Número de eventos dividido por el número de parámetros del modelo. Una comprobación aproximada de si tienes datos suficientes |
| **Validación externa** | Probar el modelo final en un conjunto de datos completamente distinto |

---

## 7. Recursos para aprender (todos gratuitos en línea; la mayoría en inglés)

* **Tidy Modeling with R** (Kuhn y Silge): <https://www.tmwr.org>. La
  referencia del código de tidymodels que se usa aquí.
* **An Introduction to Statistical Learning with R (ISLR2)**:
  <https://www.statlearning.com>. La mejor introducción conceptual al ML.
* **R for Data Science (2.ª ed.)**: <https://r4ds.hadley.nz>. Para manejar
  datos y hacer gráficos. Existe una traducción al español de la 1.ª edición:
  <https://es.r4ds.hadley.nz>.
* Guía de reporte **TRIPOD+AI**: Collins et al., *BMJ* 2024;385:e078378.
* Christodoulou et al., *J Clin Epidemiol* 2019: revisión sistemática que
  compara el ML con la regresión logística en la predicción clínica.

---

## 8. Solución de problemas

| Problema | Solución |
|----------|----------|
| `there is no package called 'xxx'` | Ejecuta `install.packages("xxx")` en la consola |
| `cannot open file 'data/...'` | Abre el curso con `curso_ML_en_R.Rproj` y ejecuta primero la Lección 1 |
| `could not find function "..."` | Ejecuta el bloque con `library(...)` del principio de la lección |
| Los resultados difieren un poco de los de un colega | Versiones de paquetes o procesador distintos. `set.seed()` fija el azar, pero no todos los detalles numéricos |
| Falla la descarga | Revisa la conexión a internet. Tras la Lección 1 hay una copia local en `data/` |
| Knit falla pero los bloques funcionan | Knit empieza desde una sesión *limpia*: falta en el archivo algo que ejecutaste en la consola |
| El ajuste es lento | Usa `v = 5` particiones y un `size`/`grid` más pequeño mientras aprendes |

---

## 9. Probado con

Todas las lecciones se ejecutaron desde cero, en orden, en octubre de 2026,
con:

| Software | Versión |
|----------|---------|
| R | 4.6.1 (Windows) |
| tidymodels | 1.5.0 (parsnip 1.6.1, recipes 1.4.0, tune 2.1.0, yardstick 1.4.0) |
| ranger | 0.18.0 |
| xgboost | 3.2.1.1 |
| lightgbm | 4.7.0 |
| bonsai | 0.4.1 |
| gtsummary | 2.6.1 |
| skimr | 2.2.2 |

Los paquetes de R cambian con el tiempo. Si una lección deja de funcionar
tras una actualización, abre un *issue* describiendo el error e incluye la
salida de `sessionInfo()`. Los resultados (AUC, hiperparámetros elegidos)
pueden variar ligeramente según las versiones de los paquetes y la
computadora.

---

## 10. Cómo se hizo este curso

Los materiales del curso se desarrollaron con la ayuda de un modelo de IA
(Claude, de Anthropic) y después se ejecutaron y probaron de principio a fin.
Contrasta los métodos con las referencias indicadas y busca asesoramiento
estadístico antes de usarlos en un estudio. El curso se ofrece tal cual, con
fines educativos.

---

## 11. Licencia

* **Código** (el código R de los archivos `.Rmd`): [licencia MIT](../LICENSE).
* **Texto y material docente**: [Creative Commons Atribución 4.0
  (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/deed.es).
* Los **conjuntos de datos no** se incluyen en este repositorio. El código
  los descarga del UCI Machine Learning Repository, bajo las licencias
  indicadas en sus páginas de UCI. Cita los conjuntos de datos originales
  (referencias en las Lecciones 1 y 7).
