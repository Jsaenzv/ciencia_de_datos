# SECCIÓN 1

## Mapa conceptual completo de la materia

### 1. Fundamentos de ciencia de datos

- qué es un dataset
- tipos de variables
- tipos de problemas:
	- clasificación
	- regresión
	- clustering
- diferencia entre aprendizaje supervisado y no supervisado
- relación entre problema, target, features y métricas

### 2. Análisis exploratorio y preprocesamiento

- análisis univariado y multivariado
- medidas resumen
- frecuencias
- correlación
- visualización
- datos faltantes
- valores atípicos
- feature engineering
- encoding
- normalización
- discretización

### 3. Evaluación de modelos

- train / test split
- cross validation
- matrices de confusión
- accuracy, precision, recall, F1, AUC/ROC
- MAE, MSE, RMSE
- comparación train vs test
- overfitting / underfitting
- sesgo / varianza
- elección de métricas según contexto

### 4. Árboles y modelos basados en árboles

- ID3
- C4.5
- entropía
- ganancia de información
- Gini
- poda
- tratamiento de atributos numéricos
- Random Forest
- importancia de variables

### 5. Modelos de clasificación y regresión

- regresión lineal
- regresión logística
- KNN
- SVM
- árboles
- Random Forest
- XGBoost
- modelos a elección comparados por performance y trade-offs

### 6. Clustering

- K-Means
- tendencia al clustering
- Hopkins
- elbow method
- silhouette
- interpretación de clusters

### 7. Reducción de dimensionalidad

- por qué reducir dimensión
- PCA
- scree plot
- MDS
- t-SNE
- ISOMAP
- qué preserva cada técnica
- relación entre reducción, visualización, ruido, colinealidad y overfitting

### 8. Ensambles

- bagging
- boosting
- stacking
- voting
- cascading
- ensambles homogéneos e híbridos
- trade-off entre performance e interpretabilidad

### 9. Redes neuronales

- perceptrón simple
- limitaciones del perceptrón
- perceptrón multicapa
- backpropagation
- descenso por gradiente
- optimizadores
- regularización
- early stopping
- SOM
- introducción a redes profundas

### 10. NLP aplicado

- bag of words
- clasificación/regresión sobre texto
- Bayes Naïve
- análisis de sentimientos
- lexicones
- extracción de features desde texto
- competencia Kaggle con predicción de story points

### Dependencia conceptual global

El flujo natural de la materia es:

- fundamentos → EDA y preprocesamiento → métricas y evaluación → modelos supervisados básicos → árboles y ensambles → clustering → reducción de dimensionalidad → redes → NLP / problemas integradores tipo TP.

# SECCIÓN 2

## Conceptos fundamentales

Los conceptos verdaderamente fundamentales, es decir, los que sostienen casi todo el resto, son estos:

### A. Representación del problema

El alumno debe dominar:

- qué se predice
- con qué variables
- qué tipo de salida tiene el problema
- qué familia de algoritmos aplica

Sin esto no puede decidir ni modelo ni métrica.

### B. Calidad de datos

La materia exige mucho criterio sobre:

- faltantes
- outliers
- escalado
- encoding
- nuevas variables

Esto aparece en TP, parciales y recuperatorios.

### C. Evaluación correcta

No alcanza con entrenar:

- hay que justificar la métrica
- comparar train vs test
- usar cross validation
- interpretar errores del modelo

Esto aparece de forma muy recurrente.

### D. Interpretación de modelos

La cátedra no solo pide “usar” modelos sino:

- explicar reglas de árboles
- interpretar importancia de atributos
- explicar clusters
- justificar hiperparámetros
- defender el modelo elegido

### E. Relación entre complejidad y generalización

Hay un núcleo teórico repetido:

- bias
- varianza
- overfitting
- underfitting
- regularización
- poda
- early stopping
- ensambles

### F. Saber elegir técnica según objetivo

Ejemplos que aparecen explícitamente:

- PCA si interesa varianza explicada
- ISOMAP si los datos viven sobre una variedad
- t-SNE si interesa preservar clusters
- precision o recall según costo del error

# SECCIÓN 3

## Análisis de parciales y finales

### Qué evalúan realmente

Los exámenes no parecen centrarse en demostraciones matemáticas largas, sino en una mezcla de:

- teoría conceptual breve pero precisa
- interpretación de situaciones
- elección justificada de técnicas
- cálculo o lectura de métricas
- conexión entre teoría y lo hecho en los TP

### Tipos de ejercicios recurrentes

#### 1. Definiciones comparativas

Muy frecuentes:

- clasificación vs regresión vs clustering
- precision vs recall
- bagging vs boosting
- stacking vs voting
- ensambles homogéneos vs híbridos

#### 2. Preguntas de criterio

Por ejemplo:

- qué métrica usarías y por qué
- qué técnica de reducción elegirías y por qué
- cuándo maximizar precision y cuándo recall
- qué problema trae un one-hot con 200 categorías y qué alternativas hay

#### 3. Preguntas de interpretación de pipeline

- cómo se trataron faltantes
- cómo se trataron outliers
- cómo se hizo la optimización de hiperparámetros
- qué se hizo en TP1/TP2 y por qué

#### 4. Ejercicios de métricas

- matriz de confusión
- cálculo/interpretación de F1
- lectura de precision y recall
- RMSE para regresión

#### 5. Preguntas de árboles

- ID3/C4.5
- entropía
- ganancia de información
- Gini
- poda
- numéricos en árboles

#### 6. Reducción de dimensionalidad

- scree plot
- PCA
- MDS
- t-SNE
- ISOMAP
- ventajas generales de reducir dimensión

#### 7. Redes neuronales

- limitación del perceptrón
- backpropagation
- optimizadores
- early stopping
- SOM
- arquitectura e hiperparámetros de redes usadas en TP2

#### 8. NLP y sentimiento

En finales aparecen:

- Naive Bayes con conteos/probabilidades
- lexicones de sentimiento
- CNN / RNN / deep belief en algunos modelos de final

Esto sugiere que el final puede abrir más el abanico que el parcial.

### Conceptos más evaluados

Los más repetidos en el material de exámenes son:

- métricas
- overfitting / underfitting / bias-varianza
- árboles
- reducción de dimensionalidad
- preprocesamiento
- ensambles
- redes neuronales básicas
- relación con los TP

### Nivel de dificultad esperado

Diría que el nivel es medio-alto en comprensión, pero no extremadamente formal.

La dificultad está en:

- distinguir conceptos parecidos
- justificar elecciones
- no confundir métricas
- poder explicar lo hecho en TP con lenguaje técnico correcto

No parece un examen de “memoria pura”, pero sí castiga mucho la comprensión superficial.

# SECCIÓN 4

## Módulos de aprendizaje

Propongo estos módulos.

### Módulo 1 — Fundamentos de ciencia de datos

- Objetivo: construir el mapa mental base.
- Dominar: dataset, features, target, variable cualitativa/cuantitativa, supervisado/no supervisado, clasificación/regresión/clustering.
- Habilidades: reconocer el tipo de problema y elegir familia de técnicas.
- Ejercicios clave: clasificar problemas y justificar algoritmo + métrica.
- Base documental: contenidos mínimos + parciales.

### Módulo 2 — EDA y visualización

- Objetivo: aprender a “leer” un dataset.
- Dominar: medidas resumen, frecuencias, distribuciones, correlaciones, gráficos.
- Habilidades: formular preguntas de investigación y extraer hallazgos.
- Ejercicios clave: análisis exploratorio de dataset estilo TP1-E1.

### Módulo 3 — Preprocesamiento y calidad de datos

- Objetivo: entender cómo preparar datos sin destruir información.
- Dominar: faltantes, outliers uni/multivariados, normalización, discretización, encoding, feature engineering.
- Habilidades: decidir tratamiento y justificarlo.
- Ejercicios clave: proponer pipelines de limpieza y defenderlos.

### Módulo 4 — Evaluación, métricas y generalización

- Objetivo: aprender a medir bien.
- Dominar: train/test, cross validation, accuracy, precision, recall, F1, ROC, MAE/MSE/RMSE, sesgo, varianza, overfitting, underfitting.
- Habilidades: elegir métricas según contexto y detectar mala generalización.
- Ejercicios clave: interpretación de matrices de confusión y casos de negocio.

### Módulo 5 — Árboles de decisión y Random Forest

- Objetivo: dominar el bloque más preguntado.
- Dominar: ID3, C4.5, entropía, ganancia, Gini, poda, árboles con numéricos, RF, importancia de atributos.
- Habilidades: interpretar reglas y comparar árboles vs ensambles.
- Ejercicios clave: explicar raíz/splits, métricas y primeras reglas de un árbol.

### Módulo 6 — Regresión y clasificación clásica

- Objetivo: consolidar modelos base.
- Dominar: regresión lineal, logística, KNN, SVM, XGBoost a nivel conceptual.
- Habilidades: saber cuándo usar cada uno y qué necesitan en preprocesamiento.
- Ejercicios clave: comparar modelos para un mismo problema.

### Módulo 7 — Clustering

- Objetivo: dominar aprendizaje no supervisado básico.
- Dominar: K-Means, Hopkins, elbow, silhouette, interpretación de grupos.
- Habilidades: decidir si tiene sentido clusterizar y cómo validar clusters.
- Ejercicios clave: TP1 ejercicio Spotify.

### Módulo 8 — Reducción de dimensionalidad

- Objetivo: entender qué preserva cada técnica.
- Dominar: PCA, scree plot, MDS, t-SNE, ISOMAP.
- Habilidades: elegir técnica según objetivo.
- Ejercicios clave: preguntas tipo parcial de selección de técnica.

### Módulo 9 — Ensambles

- Objetivo: comprender por qué combinarlos mejora performance.
- Dominar: bagging, boosting, stacking, voting, cascading, homogéneo vs híbrido.
- Habilidades: explicar beneficios/costos y relacionarlos con TP.
- Ejercicios clave: comparar esquemas y justificar uno.

### Módulo 10 — Redes neuronales

- Objetivo: construir una base suficiente para parcial/final y TP2.
- Dominar: perceptrón, MLP, backpropagation, descenso por gradiente, optimizadores, regularización, early stopping, SOM.
- Habilidades: explicar funcionamiento y limitaciones.
- Ejercicios clave: preguntas conceptuales cortas con ejemplos.

### Módulo 11 — NLP aplicado y TP2

- Objetivo: integrar texto + modelado + evaluación.
- Dominar: bag of words, Bayes Naïve, regresión sobre texto, RMSE, ensambles y red neuronal sobre texto.
- Habilidades: transformar texto en variables y comparar modelos.
- Ejercicios clave: story points + sentimiento + Naive Bayes básico.

### Módulo 12 — Integración para examen

- Objetivo: consolidar y mezclar temas.
- Dominar: selección de técnicas, justificación, conexión con TP, preguntas cortas.
- Habilidades: responder oral o escrito con precisión.
- Ejercicios clave: simulacros intercalados tipo parcial/final.

# SECCIÓN 5

## Dependencias entre módulos

- Módulo 1 es prerequisito de todos.
- Módulos 2 y 3 dependen de 1.
- Módulo 4 depende de 1, 2 y 3.
- Módulo 5 depende de 3 y 4.
- Módulo 6 depende de 3 y 4.
- Módulo 7 depende de 2, 3 y parcialmente 4.
- Módulo 8 depende de 2, 3 y 4.
- Módulo 9 depende de 4, 5 y 6.
- Módulo 10 depende de 4 y 6.
- Módulo 11 depende de 3, 4, 9 y 10.
- Módulo 12 depende de todos.

La lógica es: primero entender datos, después medir, después modelar, después integrar.

# SECCIÓN 6

## Orden óptimo de estudio

- Fundamentos
- EDA y visualización
- Preprocesamiento y calidad de datos
- Evaluación, métricas y generalización
- Árboles de decisión y Random Forest
- Regresión y clasificación clásica
- Clustering
- Reducción de dimensionalidad
- Ensambles
- Redes neuronales
- NLP aplicado y TP2
- Integración para examen

## Por qué este orden

Porque el examen y los TP exigen que el alumno primero sepa:

- qué problema tiene
- cómo se ve el dataset
- cómo limpiarlo
- cómo evaluar
- y recién después elegir y defender modelos.

Además, árboles, métricas y preprocesamiento aparecen antes y más veces que redes profundas, así que conviene adelantarlos.