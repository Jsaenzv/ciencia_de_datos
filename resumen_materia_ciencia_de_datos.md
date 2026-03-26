<<<<<<< ours
<<<<<<< ours
<<<<<<< ours
# Tratado de Ciencia de Datos: fundamentos, métodos y criterio experto

## Prólogo: qué significa realmente estudiar ciencia de datos

La ciencia de datos no es una colección de algoritmos sueltos ni una carrera por obtener el mejor *score* en una plataforma. Es una disciplina de razonamiento empírico: parte de fenómenos del mundo real, construye representaciones matemáticas de esos fenómenos, formula hipótesis sobre patrones, estima modelos y, finalmente, toma decisiones bajo incertidumbre. Si se estudia de manera superficial, se vuelve un recetario. Si se estudia con profundidad, se transforma en una forma de pensar.

Este texto asume que la persona lectora puede comenzar con conocimientos mínimos y, al mismo tiempo, aspira a dominar la materia con nivel alto. Por eso cada tema se desarrolla en capas: primero la intuición, luego el problema que obliga a precisarla, después la formalización, y finalmente las consecuencias prácticas, los límites y los errores frecuentes. La meta no es memorizar términos; es adquirir criterio técnico.

En todo el tratado aparecerá una idea central: **todo modelo es una simplificación**. Un modelo útil no “copia” la realidad, sino que captura una estructura relevante para un objetivo definido. De allí surge una responsabilidad metodológica: evaluar permanentemente qué información se pierde al simplificar, qué supuestos se introducen y qué consecuencias tienen esas decisiones.

---

## Capítulo 1. Planteamiento del problema: antes del algoritmo, la pregunta correcta

### 1.1 Del problema real al problema de datos

Una organización no pide “entrenar XGBoost”; pide reducir fraude, predecir demanda, priorizar pacientes, optimizar logística o comprender segmentos de usuarios. La primera tarea científica consiste en traducir ese objetivo en una pregunta operativa y evaluable.

Formalmente, cuando el objetivo es predictivo supervisado, buscamos una función

\[
f: \mathcal{X} \to \mathcal{Y}
\]

que aproxime la relación entre un espacio de entradas \(\mathcal{X}\) (features) y un espacio de salidas \(\mathcal{Y}\) (target). Esta expresión, aparentemente simple, obliga a tomar decisiones difíciles: qué es una observación, qué información está disponible al momento de predecir, qué horizonte temporal se considera y qué definición exacta tiene la variable objetivo.

Si estas definiciones quedan vagas, todo lo posterior se vuelve frágil. Por ejemplo, “predecir abandono de clientes” exige definir abandono (¿30 días sin actividad? ¿cancelación explícita?) y fijar ventana temporal de observación.

### 1.2 Tipos de aprendizaje y naturaleza del target

En aprendizaje supervisado la variable objetivo está observada en entrenamiento. Dos familias dominan:

- **Clasificación**: \(y\) pertenece a un conjunto discreto de clases.
- **Regresión**: \(y\) es continua o cuasi continua.

En aprendizaje no supervisado no hay \(y\) observada: se buscan estructuras latentes (agrupamientos, factores de variación, compresión de información).

La distinción no es taxonómica: determina métricas, pérdidas, validación y riesgos de error. Un error en clasificación puede tener costo asimétrico (falso negativo más grave que falso positivo); en regresión suele importar magnitud del desvío.

### 1.3 Causalidad versus predicción

Un error clásico de estudiantes avanzados: creer que un buen predictor demuestra causalidad. No necesariamente.

- **Predicción**: interesa \(P(Y\mid X)\), aunque el mecanismo causal no esté identificado.
- **Causalidad**: interesa el efecto de intervenir \(do(X=x)\) sobre \(Y\).

Un modelo puede predecir muy bien “ventas” usando “monto de publicidad” y, aun así, no identificar el efecto causal de aumentar presupuesto publicitario si hay variables de confusión.

### 1.4 Función de pérdida y noción de riesgo

Aprender un modelo implica minimizar pérdida esperada:

\[
R(f)=\mathbb{E}_{(X,Y)\sim \mathcal{D}}\left[L\big(Y,f(X)\big)\right]
\]

donde:

- \(\mathcal{D}\): distribución real (desconocida) de datos,
- \(L\): pérdida (0-1, cuadrática, absoluta, log-loss, etc.),
- \(R(f)\): riesgo poblacional.

Como \(\mathcal{D}\) es desconocida, minimizamos riesgo empírico sobre muestra:

\[
\hat R_n(f)=\frac{1}{n}\sum_{i=1}^{n}L\big(y_i,f(x_i)\big)
\]

La distancia entre \(\hat R_n\) y \(R\) explica por qué existe la generalización como problema central del curso.

---

## Capítulo 2. Estructura de datos, calidad y lectura crítica del dataset

### 2.1 Unidad de análisis y granularidad

Antes de cualquier transformación hay que fijar la unidad de observación: cliente, transacción, sesión, día, documento, píxel, etc. Muchos errores nacen de mezclar granularidades incompatibles. Si combinamos datos diarios de clima con ventas mensuales sin agregación coherente, generamos ruido estructural.

### 2.2 Tipos de variables y consecuencias metodológicas

- Numéricas continuas/discretas.
- Categóricas nominales/ordinales.
- Temporales.
- Texto, imagen, audio.

Cada tipo impone operaciones válidas y no válidas. Promediar códigos postales no tiene significado semántico; ordenar categorías nominales induce una estructura inexistente.

### 2.3 Calidad de datos: completitud, consistencia, validez

Un diagnóstico serio exige al menos:

1. **Completitud**: proporción y patrón de faltantes.
2. **Consistencia**: reglas internas (edad negativa, fechas invertidas, duplicados).
3. **Validez**: coherencia con dominio (rango plausible, unidades, definiciones).
4. **Trazabilidad**: origen y transformación de columnas.

### 2.4 Mecanismos de datos faltantes

No todos los faltantes son iguales:

- **MCAR** (*Missing Completely At Random*): ausencia independiente de variables observadas y no observadas.
- **MAR** (*Missing At Random*): ausencia depende de variables observadas.
- **MNAR** (*Missing Not At Random*): ausencia depende del propio valor faltante u otras no observadas.

La estrategia de imputación debe considerar este mecanismo. Tratar MNAR como MCAR puede sesgar inferencias y degradar predicción.

### 2.5 Análisis exploratorio (EDA) como etapa inferencial preliminar

El EDA no es decoración gráfica. Cumple funciones concretas:

- detectar asimetrías y colas pesadas,
- identificar outliers estructurales y errores de carga,
- examinar desbalance de clases,
- revelar relaciones no lineales,
- orientar ingeniería de variables y elección de métricas.

En práctica experta, cada gráfico responde una pregunta explícita, no un ritual de notebook.

---

## Capítulo 3. Preprocesamiento y representación: construir un espacio donde aprender sea posible

### 3.1 Principio rector

Los modelos aprenden en el espacio de representación que les entregamos. Una representación pobre limita incluso al mejor algoritmo.

### 3.2 Imputación con criterio

Estrategias básicas:

- media/mediana/moda,
- imputación por grupo (condicionada a segmento),
- kNN imputation,
- modelos de imputación multivariada.

Regla fundamental: ajustar imputadores **solo en train** y aplicar a validación/test. Ajustar con toda la base produce *data leakage*.

### 3.3 Outliers: error, rareza o señal

Un valor extremo puede ser:

1. error de medición,
2. evento raro legítimo,
3. subpoblación distinta.

Acciones posibles: corrección, recorte, transformación logarítmica, métodos robustos, modelado separado por régimen. Eliminar sin justificación suele destruir información de alto valor.

### 3.4 Codificación de categóricas

- **One-hot encoding**: evita orden artificial; puede explotar dimensionalidad.
- **Ordinal encoding**: válido solo si existe orden semántico real.
- **Target/mean encoding**: potente en alta cardinalidad, pero de alto riesgo de fuga si no se aplica con validación interna.

### 3.5 Escalado y normalización

Sea una variable \(x\):

- Estandarización: \(z=(x-\mu)/\sigma\)
- Min-max: \(x'=(x-x_{\min})/(x_{\max}-x_{\min})\)

Modelos basados en distancia/gradiente (KNN, SVM, redes, regresión regularizada) suelen exigir escalas comparables; árboles y ensambles de árboles son menos sensibles.

### 3.6 Ingeniería de variables

Es la etapa donde se incorpora conocimiento de dominio en forma computable:

- ratios (ingreso/gasto),
- interacciones (\(x_1x_2\)),
- transformaciones temporales (lag, rolling mean),
- atributos de texto (longitud, TF-IDF, embeddings),
- señales de comportamiento (recencia, frecuencia, monetización).

Una buena variable puede superar mejoras obtenidas por cambio de algoritmo.

---

## Capítulo 4. Partición, validación y evaluación: medir sin autoengaño

### 4.1 Generalización

El objetivo no es minimizar error histórico, sino error futuro. Por eso distinguimos:

- entrenamiento,
- validación (selección/tuning),
- test final (estimación no sesgada del desempeño final).

### 4.2 Validación cruzada

En *k-fold cross-validation*, se divide train en \(k\) pliegues. Cada corrida entrena con \(k-1\) y valida con el restante. Resultado: media y dispersión del desempeño.

Ventaja: reduce dependencia de una sola partición.
Costo: mayor tiempo computacional.

### 4.3 Métricas de clasificación

Sea matriz de confusión con TP, FP, TN, FN:

\[
\text{Accuracy}=\frac{TP+TN}{TP+TN+FP+FN}
\]

\[
\text{Precision}=\frac{TP}{TP+FP},\qquad
\text{Recall}=\frac{TP}{TP+FN}
\]

\[
F_1=2\frac{\text{Precision}\cdot \text{Recall}}{\text{Precision}+\text{Recall}}
\]

Interpretación:

- precisión alta: pocos falsos positivos,
- recall alto: pocos falsos negativos,
- F1 alto: equilibrio entre ambos.

En clases desbalanceadas, accuracy puede ser engañosa.

### 4.4 Curvas ROC y PR

- ROC: TPR vs FPR para distintos umbrales.
- PR: Precision vs Recall; más informativa con fuerte desbalance.

Elegir umbral es decisión de negocio, no mero detalle técnico.

### 4.5 Métricas de regresión

\[
MAE=\frac{1}{n}\sum|y_i-\hat y_i|
\]

\[
MSE=\frac{1}{n}\sum(y_i-\hat y_i)^2,
\qquad RMSE=\sqrt{MSE}
\]

\(MSE/RMSE\) penalizan más errores grandes; \(MAE\) es más robusta a outliers.

### 4.6 Calibración probabilística

En problemas de decisión, no basta ranking correcto: importa que probabilidades sean confiables. Un modelo bien calibrado que predice 0.8 debería acertar aproximadamente 80% de veces en ese estrato.

---

## Capítulo 5. Modelos supervisados I: regresión lineal y logística con profundidad

### 5.1 Regresión lineal: hipótesis y geometría

Modelo:

\[
\hat y = \beta_0 + \beta_1x_1+\cdots+\beta_px_p = X\beta
\]

Se estima típicamente por mínimos cuadrados ordinarios (OLS):

\[
\hat\beta = \arg\min_{\beta}\|y-X\beta\|_2^2
\]

Solución cerrada (si \(X^TX\) invertible):

\[
\hat\beta=(X^TX)^{-1}X^Ty
\]

Interpretación clave: cada \(\beta_j\) mide cambio esperado en \(y\) por unidad de \(x_j\), manteniendo las demás constantes.

#### Supuestos clásicos

1. Linealidad en parámetros.
2. Independencia de errores.
3. Homocedasticidad.
4. Normalidad de errores (para inferencia clásica).
5. Ausencia de multicolinealidad severa.

Violarlos no siempre invalida predicción, pero sí cambia interpretación e inferencia.

### 5.2 Regularización: sesgo-varianza en acción

Ridge:

\[
\arg\min_\beta \|y-X\beta\|_2^2 + \lambda\|\beta\|_2^2
\]

Lasso:

\[
\arg\min_\beta \|y-X\beta\|_2^2 + \lambda\|\beta\|_1
\]

- Ridge contrae coeficientes de forma suave.
- Lasso puede llevar coeficientes exactamente a cero (selección implícita).

\(\lambda\) controla compromiso ajuste-complejidad.

### 5.3 Regresión logística

Para \(y\in\{0,1\}\):

\[
P(y=1\mid x)=\sigma(z)=\frac{1}{1+e^{-z}},\quad z=\beta_0+\beta^Tx
\]

Linealizamos en *log-odds*:

\[
\log\frac{p}{1-p}=\beta_0+\beta^Tx
\]

Entrenamiento por máxima verosimilitud (equivale a minimizar log-loss).

Interpretación de coeficiente \(\beta_j\): incremento unitario en \(x_j\) cambia log-odds en \(\beta_j\), manteniendo resto fijo.

Error típico: interpretar coeficiente logístico como cambio lineal directo de probabilidad; no es así, depende del punto de operación por no linealidad sigmoide.

---

## Capítulo 6. Modelos supervisados II: árboles, ensambles y boosting

### 6.1 Árboles de decisión

Un árbol divide recursivamente espacio de features con reglas tipo \(x_j < t\). Ventajas: interpretabilidad, manejo natural de no linealidades e interacciones.

Criterios de división:

- Clasificación: Gini, entropía.
- Regresión: reducción de varianza/MSE.

Riesgo: sobreajuste en árboles profundos.

### 6.2 Bagging y Random Forest

**Bagging**: entrenar múltiples modelos en muestras *bootstrap* y promediar/votar.

**Random Forest** añade aleatoriedad en selección de variables por split, reduciendo correlación entre árboles y mejorando varianza del ensamble.

Intuición formal de reducción de varianza: promediar estimadores débiles pero relativamente des-correlacionados disminuye varianza global.

### 6.3 Boosting y XGBoost

Boosting entrena secuencialmente modelos débiles que corrigen errores/residuos del conjunto previo.

En gradiente boosting, se minimiza una función objetivo mediante descenso funcional en espacio de funciones:

\[
F_m(x)=F_{m-1}(x)+\eta h_m(x)
\]

donde \(h_m\) aproxima el gradiente negativo de la pérdida.

XGBoost añade regularización explícita, manejo eficiente de faltantes y optimización de alto rendimiento.

### 6.4 Cuándo usar cada familia

- Lineales: interpretabilidad y baseline sólido.
- Árboles simples: reglas explicables.
- Random Forest: robustez con bajo tuning.
- Boosting: alto desempeño con tuning cuidadoso.

No existe “mejor universal”; depende de tamaño de datos, ruido, necesidad de explicación, latencia, mantenimiento y riesgo de drift.

---

## Capítulo 7. Aprendizaje no supervisado: clustering y reducción de dimensionalidad

### 7.1 Qué problema resuelve el clustering

Clustering busca particionar observaciones en grupos internamente similares y externamente distintos sin etiquetas previas. Es descubrimiento de estructura, no clasificación oculta.

### 7.2 K-means en profundidad

Objetivo:

\[
\min_{C_1,\dots,C_k} \sum_{j=1}^k \sum_{x_i\in C_j}\|x_i-\mu_j\|^2
\]

con \(\mu_j\) centroide del cluster \(C_j\).

Algoritmo iterativo:

1. Inicializar centroides.
2. Asignar cada punto al centroide más cercano.
3. Recalcular centroides.
4. Repetir hasta convergencia local.

Limitaciones:

- asume clusters aproximadamente esféricos,
- sensible a escala y outliers,
- requiere elegir \(k\).

### 7.3 Selección de k e interpretación

Métodos: codo (*elbow*), silueta, estabilidad bajo remuestreo. Ninguno sustituye juicio de dominio. Un \(k\) estadísticamente plausible puede carecer de significado de negocio.

### 7.4 Clustering jerárquico y DBSCAN

- **Jerárquico**: construye dendrograma; útil para explorar estructura multiescala.
- **DBSCAN**: define clusters como regiones densas; detecta ruido y formas arbitrarias, evita fijar \(k\), pero requiere parámetros de densidad adecuados.

### 7.5 PCA: compresión lineal y varianza explicada

PCA busca direcciones ortogonales de máxima varianza. Si \(X\) está centrada, se diagonaliza matriz de covarianza:

\[
\Sigma=\frac{1}{n-1}X^TX
\]

Autovectores \(v_j\) definen componentes principales; autovalores \(\lambda_j\) cuantifican varianza explicada.

Proyección en primeras \(m\) componentes reduce dimensión conservando estructura dominante.

Riesgo didáctico: creer que “menos dimensión siempre mejora”. Puede perder señales discriminantes de baja varianza pero alta relevancia predictiva.

---

## Capítulo 8. Redes neuronales y aprendizaje profundo: de la neurona al entrenamiento estable

### 8.1 Perceptrón y composición de funciones

Una neurona calcula:

=======
# Ciencia de Datos: tratado de fundamentos, modelado y criterio profesional

## Introducción general: qué significa dominar ciencia de datos

La ciencia de datos no es una colección de algoritmos ni una secuencia mecánica de pasos. Es una disciplina de inferencia, representación y decisión. Su objeto real de estudio no son solamente los datos, sino la relación entre un fenómeno del mundo, su registro imperfecto en una base y las decisiones que tomamos a partir de ese registro. Esta afirmación puede parecer filosófica, pero tiene consecuencias metodológicas muy concretas: elegir una variable objetivo, diseñar una métrica, imputar faltantes, ajustar un modelo, interpretar un score y decidir si desplegar o no una solución son partes de un mismo proceso intelectual.

Cuando una persona estudia ciencia de datos de manera superficial, suele creer que el problema principal es “qué algoritmo usar”. En cambio, cuando alcanza un nivel más alto, entiende que la mayor parte de los errores graves aparece antes del algoritmo: formulaciones ambiguas del problema, features mal definidas, leakage, evaluación mal planteada, métricas incoherentes con el costo del error o interpretaciones causales de resultados meramente predictivos. Este texto adopta esa segunda perspectiva: cada técnica se presenta como respuesta a una necesidad, no como una receta aislada.

Para facilitar un aprendizaje profundo, avanzaremos desde la base conceptual hasta temas de modelado más complejos, pasando por las zonas que más cuestan: formalización matemática, interpretación de métricas, sesgo-varianza, diferencia entre ajuste y generalización, significado real de la validación cruzada, diseño de experimentos comparables y lectura crítica de resultados. El objetivo final no es que el lector “reconozca términos”, sino que pueda defender decisiones técnicas con argumentos sólidos, detectar errores comunes y resolver problemas de examen o de práctica profesional con rigor.

## Capítulo 1. Pensar correctamente el problema antes de modelar

Todo proyecto de ciencia de datos comienza con una traducción: pasamos de una pregunta del mundo real a una pregunta modelable. Esa traducción nunca es neutra. Si la formulamos mal, el mejor modelo del mundo optimizará el objetivo equivocado. Por eso este primer capítulo no es introductorio en sentido liviano; es fundacional.

### 1.1 Unidad de análisis, observación y variable

Un dataset tabular puede verse como una matriz \(X\in\mathbb{R}^{n\times p}\), donde \(n\) es el número de observaciones y \(p\) el número de variables predictoras (features). Si además existe una variable objetivo \(y\), estamos en un escenario supervisado. Sin embargo, esta notación compacta oculta una decisión clave: ¿qué representa cada fila?

Si cada fila representa una persona, el modelo aprende patrones entre personas. Si cada fila representa una transacción, aprende patrones entre eventos. Si cada fila representa una ventana temporal, aprende patrones agregados en tiempo. Cambiar la unidad de análisis cambia el significado de toda la tarea. Un error frecuente es mezclar unidades (por ejemplo, usar variables de cliente en un problema por transacción sin controlar dependencia intra-cliente), generando pseudo-replicación y evaluación inflada.

### 1.2 Tipos de tarea: clasificación, regresión, ranking, detección y segmentación

La división clásica entre clasificación, regresión y clustering es correcta, pero conviene ampliarla.

En **clasificación**, el modelo busca una función \(f:X\to\mathcal{C}\), donde \(\mathcal{C}\) es un conjunto finito de clases. Si \(|\mathcal{C}|=2\), hablamos de clasificación binaria; si \(|\mathcal{C}|>2\), multiclase; si una observación puede pertenecer simultáneamente a varias clases, multilabel.

En **regresión**, \(f:X\to\mathbb{R}\) (o \(\mathbb{R}^k\) si hay múltiples objetivos continuos). El error no es “acierto/fallo”, sino distancia entre valor real y predicho.

En **clustering**, no existe \(y\) durante el entrenamiento: buscamos una partición \(\{C_1,\dots,C_K\}\) que maximice similitud intra-cluster y minimice similitud inter-cluster según una métrica elegida.

También aparecen tareas de **ranking** (ordenar items por relevancia), **detección de anomalías** (identificar observaciones raras bajo una noción de normalidad), y **pronóstico temporal** (forecasting), donde la dependencia temporal rompe supuestos de i.i.d. (independencia e idéntica distribución).

Saber nombrar estas tareas no alcanza. Hay que entender qué cambia en cada una: tipo de pérdida, métrica adecuada, validación pertinente, riesgos de leakage y forma de interpretar resultados.

### 1.3 Supervisado vs no supervisado: diferencia epistemológica

En aprendizaje supervisado disponemos de pares \((x_i,y_i)\). El objetivo es estimar una función que minimice una pérdida esperada:

\[
\mathcal{R}(f)=\mathbb{E}_{(X,Y)\sim P}[L(Y,f(X))].
\]

Como \(P\) es desconocida, minimizamos el riesgo empírico sobre la muestra:

\[
\hat{\mathcal{R}}(f)=\frac{1}{n}\sum_{i=1}^n L(y_i,f(x_i)).
\]

En no supervisado no existe \(Y\) explícita: optimizamos criterios estructurales (varianza intra-cluster, reconstrucción, densidad, etc.). Por eso la evaluación es menos directa y más dependiente del objetivo analítico. Esta diferencia es central en examen: en supervisado preguntamos “¿predice bien?”. En no supervisado preguntamos “¿la estructura hallada es estable, interpretable y útil para una decisión?”.

### 1.4 El costo del error y la función de decisión

Un modelo produce salidas; un sistema produce decisiones. Confundir ambas cosas es peligroso. En clasificación binaria, muchos modelos generan \(\hat p(x)=P(Y=1\mid X=x)\). Convertir esa probabilidad en decisión requiere un umbral \(t\): decidir positivo si \(\hat p\ge t\). Ese \(t\) no debe fijarse por costumbre en 0.5. Debe responder al costo relativo de errores:

- falso positivo: costo \(C_{FP}\)
- falso negativo: costo \(C_{FN}\)

Si \(C_{FN}\gg C_{FP}\), conviene umbral bajo (más recall). Si \(C_{FP}\gg C_{FN}\), conviene umbral alto (más precision). La ciencia de datos aplicada exige esta traducción explícita entre métrica y negocio.

### 1.5 Errores conceptuales iniciales más frecuentes

El primer error es plantear una tarea predictiva con variables que no estarán disponibles al momento de inferencia (leakage estructural). El segundo es definir una variable objetivo con ruido de etiquetado no reconocido. El tercero es ignorar el horizonte temporal: entrenar con información futura para predecir el pasado. El cuarto es optimizar una métrica irrelevante para la decisión real.

Un lector que domina este capítulo puede responder con precisión preguntas del tipo: “¿Por qué este problema es clasificación y no regresión?”, “¿Qué cambia si la clase positiva es rara?”, “¿Qué variable objetivo sería metodológicamente válida?”, “¿Qué costo de error es prioritario?”.

## Capítulo 2. EDA (Análisis Exploratorio): leer el dataset como evidencia

Antes de modelar hay que entender qué dicen y qué ocultan los datos. El EDA no es adorno visual: es una investigación preliminar que reduce incertidumbre metodológica.

### 2.1 Estructura inicial y calidad de datos

Las primeras preguntas son concretas: tamaño de muestra, tipos de variables, porcentaje de faltantes, duplicados, columnas constantes, cardinalidad categórica, presencia de valores imposibles y coherencia de unidades.

Un chequeo de rango parece trivial, pero evita errores devastadores: edades negativas, fechas invertidas, montos en monedas mezcladas, tasas fuera de \([0,1]\), códigos de categoría inconsistentes por mayúsculas o acentos. Cada una de estas fallas puede introducir patrones espurios que un algoritmo aprovechará sin “saber” que son inválidos.

### 2.2 Estadística descriptiva con propósito

La media, mediana y desvío estándar deben interpretarse en contexto. Si una variable es asimétrica, la media puede ser poco representativa. Si tiene outliers extremos, el desvío puede inflarse. Por eso conviene combinar medidas robustas (mediana, IQR) con visualizaciones (histograma, boxplot, densidades).

En variables categóricas, la distribución de frecuencias revela desbalance y categorías raras. Una categoría con frecuencia ínfima puede no ser ruido: puede corresponder a un segmento de alto valor (por ejemplo, fraude real). El EDA riguroso evita tanto la eliminación automática como la conservación acrítica.

### 2.3 Relación entre variables y target

En clasificación, conviene analizar separabilidad aproximada: cómo varían las distribuciones de features entre clases. En regresión, interesa relación funcional (lineal, no lineal, saturación, umbrales). La correlación de Pearson mide asociación lineal, no dependencia general. Dos variables pueden tener correlación cercana a cero y relación no lineal fuerte. Este matiz evita errores de descarte prematuro.

### 2.4 Faltantes: tipologías y consecuencias

La teoría distingue tres mecanismos:

- MCAR (Missing Completely At Random): la ausencia es independiente de observados y no observados.
- MAR (Missing At Random): depende de variables observadas.
- MNAR (Missing Not At Random): depende del propio valor no observado.

La diferencia importa porque condiciona sesgo de imputación. Imputar por media bajo MNAR puede distorsionar relaciones de forma severa. En práctica, rara vez sabemos el mecanismo exacto, pero sí podemos investigar patrones de ausencia y usar indicadores de faltante para capturar señal.

### 2.5 Outliers: error, rareza o subpoblación

Un outlier puede ser:

1. error de carga;
2. evento raro legítimo;
3. evidencia de mezcla de poblaciones.

El tratamiento cambia en cada caso. En datasets financieros, un valor extremo puede representar fraude real; eliminarlo puede degradar el modelo justo donde más importa. En sensores industriales, un pico imposible puede ser fallo instrumental y conviene corregir o excluir.

### 2.6 Leakage detectado en EDA

Si una feature es una transformación del target o se registra después del evento a predecir, cualquier desempeño alto será ilusorio. Ejemplo clásico: predecir mora con una variable que se actualiza tras entrar en mora. En examen, identificar leakage suele valer más que elegir algoritmo.

### 2.7 Salida pedagógica del EDA

Al cerrar EDA debe existir un diagnóstico: qué problemas de calidad hay, qué transformaciones se justifican, qué variables son prometedoras, dónde hay riesgo de sesgo y qué hipótesis de modelado son plausibles. Sin ese diagnóstico, el pipeline posterior carece de fundamento.

## Capítulo 3. Preprocesamiento y representación: preparar datos para aprender

Preprocesar no es “limpiar por limpiar”. Es diseñar una representación \(\phi(X)\) que haga aprendible el patrón relevante y reduzca ruido irrelevante.

### 3.1 Pipeline y separación train/test

Todo transformador que aprende parámetros (imputador, escalador, PCA, selector) debe ajustarse con train y aplicarse a valid/test. Formalmente, si un escalador estima \(\mu_j,\sigma_j\), esos parámetros deben calcularse con train:

\[
\tilde x_{ij}=\frac{x_{ij}-\mu_j^{train}}{\sigma_j^{train}}.
\]

Calcularlos con todo el dataset introduce información de test en entrenamiento y sesga evaluación.

### 3.2 Imputación: decisión estadística, no relleno mecánico

Imputar media, mediana, moda, KNN o modelos múltiples implica supuestos distintos. La mediana suele ser robusta a colas; KNN preserva estructura local pero escala mal y puede amplificar ruido. Imputación múltiple refleja incertidumbre mejor que imputación única, aunque con mayor costo operativo.

Una práctica recomendable es comparar al menos dos estrategias dentro de validación cruzada y medir impacto real en métrica objetivo.

### 3.3 Encoding de categóricas

En nominales, one-hot evita orden artificial. En ordinales, puede usarse codificación ordenada si el orden es semántico real. En alta cardinalidad, target encoding puede ser útil, pero debe hacerse con estrategias anti-leakage (por ejemplo, encoding por fold). Si se calcula promedio de target por categoría usando todo train sin cuidado, el modelo puede sobreajustar fuertemente.

### 3.4 Escalado y su dependencia del modelo

KNN, SVM con kernel RBF, regresión regularizada y redes neuronales suelen requerir escalado por sensibilidad a magnitud. Árboles y ensambles de árboles son casi invariantes a transformaciones monótonas de escala. Esta distinción es frecuente en examen: no responder con “siempre escalar”.

### 3.5 Transformaciones de distribución

Aplicar \(\log(x+c)\), Box-Cox o Yeo-Johnson puede estabilizar varianza y reducir asimetría. El término \(c\) evita logaritmo de cero. La interpretación cambia: en una regresión lineal sobre \(\log y\), efectos marginales aproximan cambios porcentuales.

### 3.6 Feature engineering como ventaja competitiva

Crear variables derivadas suele aportar más que cambiar de algoritmo. Ejemplos:

- razón ingreso/cuota en riesgo crediticio;
- antigüedad de cliente en churn;
- estacionalidad (mes, semana, feriado) en series;
- n-gramas y longitud en NLP.

La regla es que cada feature nueva tenga justificación conceptual, no proliferación indiscriminada.

### 3.7 Selección de variables y multicolinealidad

En modelos lineales, alta colinealidad infla varianza de coeficientes. El VIF (Variance Inflation Factor) ayuda a diagnosticarla. Regularización L2 mitiga inestabilidad; L1 puede llevar algunos coeficientes a cero (selección implícita). En árboles, la colinealidad afecta menos el ajuste pero puede distorsionar importancias de variables.

## Capítulo 4. Evaluación rigurosa: generalización, validación y métricas

Evaluar bien significa estimar desempeño futuro, no celebrar desempeño pasado.

### 4.1 Error de entrenamiento vs error de generalización

Si \(\hat f\) minimiza riesgo empírico, no necesariamente minimiza riesgo poblacional. La brecha \(\mathcal{R}(\hat f)-\hat{\mathcal{R}}(\hat f)\) refleja generalización. Modelos muy flexibles pueden reducir \(\hat{\mathcal{R}}\) casi a cero y aun así fallar fuera de muestra.

### 4.2 Hold-out, cross-validation y nested CV

- **Hold-out**: simple, rápido, más varianza en estimación.
- **K-fold CV**: mejor estabilidad, mayor costo.
- **Nested CV**: necesaria cuando se ajustan hiperparámetros y se quiere estimación menos sesgada del desempeño final.

Confundir validación para tuning con evaluación final produce optimismo indebido.

### 4.3 Matriz de confusión y métricas de clasificación

Sea TP, TN, FP, FN:

\[
\text{Accuracy}=\frac{TP+TN}{TP+TN+FP+FN}
\]
\[
\text{Precision}=\frac{TP}{TP+FP},\quad
\text{Recall}=\frac{TP}{TP+FN}
\]
\[
F1=2\cdot\frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
\]

Cada fórmula expresa un compromiso distinto. La clave no es memorizarlas, sino leer qué error ponderan.

### 4.4 ROC, AUC y PR-AUC

ROC evalúa trade-off TPR/FPR en todos los umbrales. AUC-ROC es útil, pero en clases extremadamente desbalanceadas la curva Precision-Recall suele ser más informativa porque se centra en rendimiento sobre positivos.

### 4.5 Métricas de regresión

\[
MAE=\frac{1}{n}\sum|y_i-\hat y_i|,
\quad
MSE=\frac{1}{n}\sum(y_i-\hat y_i)^2,
\quad
RMSE=\sqrt{MSE}
\]

MSE/RMSE penalizan más errores grandes. MAE es más robusta y directamente interpretable en unidades del target. \(R^2\) mide proporción de varianza explicada respecto a predictor constante, pero un \(R^2\) alto no garantiza utilidad operativa si error absoluto sigue siendo costoso.

### 4.6 Calibración de probabilidades

Un clasificador puede discriminar bien pero estar mal calibrado. Si predice 0.8, idealmente cerca del 80% de casos con ese score debería ser positivo. Métodos como Platt scaling o isotonic regression corrigen calibración. En decisiones de riesgo, calibrar puede ser tan importante como clasificar.

### 4.7 Significancia práctica vs estadística

Una mejora de 0.003 en AUC puede ser estadísticamente detectable y operacionalmente irrelevante. El criterio profesional exige traducir métricas a impacto: costo evitado, tiempo ahorrado, casos críticos recuperados.

## Capítulo 5. Modelos supervisados fundamentales

No existe modelo universalmente mejor ("no free lunch"). Cada familia incorpora sesgos inductivos distintos.

### 5.1 Regresión lineal

Modelo:
\[
y=\beta_0+\sum_{j=1}^p\beta_jx_j+\varepsilon
\]

Interpretación: \(\beta_j\) es cambio esperado en \(y\) ante aumento unitario de \(x_j\), manteniendo lo demás constante. Supuestos clásicos: linealidad, independencia, homocedasticidad, normalidad de errores (para inferencia exacta), no colinealidad extrema.

Aunque simple, la regresión lineal es poderosa por interpretabilidad. En examen suelen pedir: significado de coeficientes, impacto de colinealidad, lectura de residuos.

### 5.2 Regresión logística

Para clasificación binaria:
\[
P(Y=1|x)=\sigma(z)=\frac{1}{1+e^{-z}},\quad z=\beta_0+\beta^Tx
\]

El logit linealiza odds:
\[
\log\frac{p}{1-p}=\beta_0+\beta^Tx
\]

\(e^{\beta_j}\) se interpreta como multiplicador de odds por unidad de \(x_j\). Error común: interpretar coeficiente como cambio lineal en probabilidad. No lo es; depende del punto de operación por no linealidad de \(\sigma\).

### 5.3 K-Nearest Neighbors (KNN)

Predice según vecinos más cercanos bajo métrica (usualmente euclídea). Ventajas: simplicidad, no paramétrico. Limitaciones: sensible a escala, costo alto en inferencia, degradación en alta dimensión (curse of dimensionality). Elegir \(k\) controla sesgo-varianza: \(k\) pequeño baja sesgo, sube varianza; \(k\) grande al revés.

### 5.4 Naive Bayes

Basado en Bayes con independencia condicional entre features:
\[
P(y|x)\propto P(y)\prod_j P(x_j|y)
\]

Aunque el supuesto es fuerte y rara vez exacto, puede funcionar muy bien en texto y problemas de alta dimensionalidad dispersa.

### 5.5 SVM

Busca hiperplano con margen máximo. En caso no separable usa margen blando con parámetro \(C\). Con kernel trick puede representar fronteras no lineales sin mapear explícitamente a dimensión alta.

- \(C\) alto: penaliza más errores, frontera más ajustada.
- \(\gamma\) alto (RBF): fronteras más locales/onduladas, mayor riesgo de overfitting.

## Capítulo 6. Árboles de decisión

Los árboles aprenden reglas tipo “si-entonces” mediante particiones recursivas del espacio de features.

### 6.1 Criterios de partición

En clasificación se usan impurezas como Gini o entropía.

\[
Gini=1-\sum_k p_k^2,
\quad
Entropy=-\sum_k p_k\log p_k
\]

La ganancia de información mide reducción de impureza tras split. En regresión se usa reducción de varianza o MSE intra-nodo.

### 6.2 Sobreajuste y poda

Árboles profundos memorizan entrenamiento. Controlar profundidad máxima, mínimo de muestras por hoja y poda de complejidad-coste reduce varianza. Interpretabilidad alta es ventaja fuerte, pero árboles únicos suelen ser inestables: pequeñas variaciones de datos pueden cambiar estructura.

### 6.3 Importancia de variables y cautelas

Importancias basadas en reducción de impureza pueden sesgarse hacia variables con muchas categorías o rangos amplios. Conviene complementar con permutation importance y análisis de estabilidad.

## Capítulo 7. Ensambles: bagging, boosting y stacking

Combinar modelos débiles o inestables puede producir estimadores más robustos.

### 7.1 Bagging y Random Forest

Bagging entrena múltiples modelos sobre muestras bootstrap y promedia/vota. Reduce varianza.

Random Forest agrega aleatoriedad en selección de features por split, disminuyendo correlación entre árboles. Resultado: menor varianza sin aumento fuerte de sesgo.

Hiperparámetros relevantes: número de árboles, profundidad, max_features, min_samples_leaf. Más árboles suele estabilizar rendimiento hasta saturación.

### 7.2 Boosting (AdaBoost, Gradient Boosting, XGBoost)

Boosting construye modelos secuenciales que corrigen errores previos. En Gradient Boosting se ajusta cada nuevo árbol a gradiente negativo de la pérdida.

XGBoost añade regularización explícita, shrinkage (learning rate), subsampling y manejo eficiente de faltantes. Es potente, pero sensible a tuning: learning rate muy alto o árboles muy profundos pueden sobreajustar.

### 7.3 Bias-variance en ensambles

Bagging: reduce varianza.
Boosting: reduce sesgo (y puede también varianza según configuración), pero más propenso a sobreajuste si no se regula.

### 7.4 Stacking

Combina predicciones de modelos base mediante un meta-modelo. Debe entrenarse con predicciones out-of-fold para evitar leakage.

## Capítulo 8. Clustering: estructura sin etiquetas

Clustering no “descubre la verdad” automáticamente. Propone una organización posible del espacio según una métrica y un criterio.

### 8.1 K-means

Objetivo:
\[
\min_{C_1,\dots,C_K}\sum_{k=1}^K\sum_{x_i\in C_k}||x_i-\mu_k||^2
\]

Supone clusters aproximadamente esféricos y similares en tamaño bajo distancia euclídea. Sensible a escala y a inicialización. Conviene ejecutar múltiples inicializaciones (k-means++ ayuda).

Elegir \(K\): método del codo, silhouette, criterio de negocio e interpretabilidad.

### 8.2 Clustering jerárquico

No requiere fijar \(K\) al inicio. Construye dendrograma mediante criterios de enlace (single, complete, average, Ward). Permite analizar estructura multiescala, pero escala peor en datasets grandes.

### 8.3 DBSCAN

Define clusters como regiones densas (\(\varepsilon\), minPts). Ventaja: detecta formas arbitrarias y etiqueta ruido explícitamente. Limitación: sensible a parámetros y dificultad en densidades variables.

### 8.4 Evaluación interna y externa

Interna: silhouette, Davies-Bouldin.
Externa (si hay etiquetas de referencia): ARI, NMI.

Pero ninguna métrica reemplaza interpretación de utilidad. Un clustering “matemáticamente prolijo” puede ser operativamente inútil.

## Capítulo 9. Reducción de dimensionalidad

La alta dimensionalidad aumenta ruido, costo computacional y dificulta visualización.

### 9.1 PCA: fundamento algebraico

Dado \(X\) centrado, PCA encuentra direcciones ortogonales \(w_j\) que maximizan varianza proyectada:
\[
\max_{||w||=1} Var(Xw)
\]

=======
# Ciencia de Datos: tratado de fundamentos, modelado y criterio profesional

## Introducción general: qué significa dominar ciencia de datos

La ciencia de datos no es una colección de algoritmos ni una secuencia mecánica de pasos. Es una disciplina de inferencia, representación y decisión. Su objeto real de estudio no son solamente los datos, sino la relación entre un fenómeno del mundo, su registro imperfecto en una base y las decisiones que tomamos a partir de ese registro. Esta afirmación puede parecer filosófica, pero tiene consecuencias metodológicas muy concretas: elegir una variable objetivo, diseñar una métrica, imputar faltantes, ajustar un modelo, interpretar un score y decidir si desplegar o no una solución son partes de un mismo proceso intelectual.

Cuando una persona estudia ciencia de datos de manera superficial, suele creer que el problema principal es “qué algoritmo usar”. En cambio, cuando alcanza un nivel más alto, entiende que la mayor parte de los errores graves aparece antes del algoritmo: formulaciones ambiguas del problema, features mal definidas, leakage, evaluación mal planteada, métricas incoherentes con el costo del error o interpretaciones causales de resultados meramente predictivos. Este texto adopta esa segunda perspectiva: cada técnica se presenta como respuesta a una necesidad, no como una receta aislada.

Para facilitar un aprendizaje profundo, avanzaremos desde la base conceptual hasta temas de modelado más complejos, pasando por las zonas que más cuestan: formalización matemática, interpretación de métricas, sesgo-varianza, diferencia entre ajuste y generalización, significado real de la validación cruzada, diseño de experimentos comparables y lectura crítica de resultados. El objetivo final no es que el lector “reconozca términos”, sino que pueda defender decisiones técnicas con argumentos sólidos, detectar errores comunes y resolver problemas de examen o de práctica profesional con rigor.

## Capítulo 1. Pensar correctamente el problema antes de modelar

Todo proyecto de ciencia de datos comienza con una traducción: pasamos de una pregunta del mundo real a una pregunta modelable. Esa traducción nunca es neutra. Si la formulamos mal, el mejor modelo del mundo optimizará el objetivo equivocado. Por eso este primer capítulo no es introductorio en sentido liviano; es fundacional.

### 1.1 Unidad de análisis, observación y variable

Un dataset tabular puede verse como una matriz \(X\in\mathbb{R}^{n\times p}\), donde \(n\) es el número de observaciones y \(p\) el número de variables predictoras (features). Si además existe una variable objetivo \(y\), estamos en un escenario supervisado. Sin embargo, esta notación compacta oculta una decisión clave: ¿qué representa cada fila?

Si cada fila representa una persona, el modelo aprende patrones entre personas. Si cada fila representa una transacción, aprende patrones entre eventos. Si cada fila representa una ventana temporal, aprende patrones agregados en tiempo. Cambiar la unidad de análisis cambia el significado de toda la tarea. Un error frecuente es mezclar unidades (por ejemplo, usar variables de cliente en un problema por transacción sin controlar dependencia intra-cliente), generando pseudo-replicación y evaluación inflada.

### 1.2 Tipos de tarea: clasificación, regresión, ranking, detección y segmentación

La división clásica entre clasificación, regresión y clustering es correcta, pero conviene ampliarla.

En **clasificación**, el modelo busca una función \(f:X\to\mathcal{C}\), donde \(\mathcal{C}\) es un conjunto finito de clases. Si \(|\mathcal{C}|=2\), hablamos de clasificación binaria; si \(|\mathcal{C}|>2\), multiclase; si una observación puede pertenecer simultáneamente a varias clases, multilabel.

En **regresión**, \(f:X\to\mathbb{R}\) (o \(\mathbb{R}^k\) si hay múltiples objetivos continuos). El error no es “acierto/fallo”, sino distancia entre valor real y predicho.

En **clustering**, no existe \(y\) durante el entrenamiento: buscamos una partición \(\{C_1,\dots,C_K\}\) que maximice similitud intra-cluster y minimice similitud inter-cluster según una métrica elegida.

También aparecen tareas de **ranking** (ordenar items por relevancia), **detección de anomalías** (identificar observaciones raras bajo una noción de normalidad), y **pronóstico temporal** (forecasting), donde la dependencia temporal rompe supuestos de i.i.d. (independencia e idéntica distribución).

Saber nombrar estas tareas no alcanza. Hay que entender qué cambia en cada una: tipo de pérdida, métrica adecuada, validación pertinente, riesgos de leakage y forma de interpretar resultados.

### 1.3 Supervisado vs no supervisado: diferencia epistemológica

En aprendizaje supervisado disponemos de pares \((x_i,y_i)\). El objetivo es estimar una función que minimice una pérdida esperada:

\[
\mathcal{R}(f)=\mathbb{E}_{(X,Y)\sim P}[L(Y,f(X))].
\]

Como \(P\) es desconocida, minimizamos el riesgo empírico sobre la muestra:

\[
\hat{\mathcal{R}}(f)=\frac{1}{n}\sum_{i=1}^n L(y_i,f(x_i)).
\]

En no supervisado no existe \(Y\) explícita: optimizamos criterios estructurales (varianza intra-cluster, reconstrucción, densidad, etc.). Por eso la evaluación es menos directa y más dependiente del objetivo analítico. Esta diferencia es central en examen: en supervisado preguntamos “¿predice bien?”. En no supervisado preguntamos “¿la estructura hallada es estable, interpretable y útil para una decisión?”.

### 1.4 El costo del error y la función de decisión

Un modelo produce salidas; un sistema produce decisiones. Confundir ambas cosas es peligroso. En clasificación binaria, muchos modelos generan \(\hat p(x)=P(Y=1\mid X=x)\). Convertir esa probabilidad en decisión requiere un umbral \(t\): decidir positivo si \(\hat p\ge t\). Ese \(t\) no debe fijarse por costumbre en 0.5. Debe responder al costo relativo de errores:

- falso positivo: costo \(C_{FP}\)
- falso negativo: costo \(C_{FN}\)

Si \(C_{FN}\gg C_{FP}\), conviene umbral bajo (más recall). Si \(C_{FP}\gg C_{FN}\), conviene umbral alto (más precision). La ciencia de datos aplicada exige esta traducción explícita entre métrica y negocio.

### 1.5 Errores conceptuales iniciales más frecuentes

El primer error es plantear una tarea predictiva con variables que no estarán disponibles al momento de inferencia (leakage estructural). El segundo es definir una variable objetivo con ruido de etiquetado no reconocido. El tercero es ignorar el horizonte temporal: entrenar con información futura para predecir el pasado. El cuarto es optimizar una métrica irrelevante para la decisión real.

Un lector que domina este capítulo puede responder con precisión preguntas del tipo: “¿Por qué este problema es clasificación y no regresión?”, “¿Qué cambia si la clase positiva es rara?”, “¿Qué variable objetivo sería metodológicamente válida?”, “¿Qué costo de error es prioritario?”.

## Capítulo 2. EDA (Análisis Exploratorio): leer el dataset como evidencia

Antes de modelar hay que entender qué dicen y qué ocultan los datos. El EDA no es adorno visual: es una investigación preliminar que reduce incertidumbre metodológica.

### 2.1 Estructura inicial y calidad de datos

Las primeras preguntas son concretas: tamaño de muestra, tipos de variables, porcentaje de faltantes, duplicados, columnas constantes, cardinalidad categórica, presencia de valores imposibles y coherencia de unidades.

Un chequeo de rango parece trivial, pero evita errores devastadores: edades negativas, fechas invertidas, montos en monedas mezcladas, tasas fuera de \([0,1]\), códigos de categoría inconsistentes por mayúsculas o acentos. Cada una de estas fallas puede introducir patrones espurios que un algoritmo aprovechará sin “saber” que son inválidos.

### 2.2 Estadística descriptiva con propósito

La media, mediana y desvío estándar deben interpretarse en contexto. Si una variable es asimétrica, la media puede ser poco representativa. Si tiene outliers extremos, el desvío puede inflarse. Por eso conviene combinar medidas robustas (mediana, IQR) con visualizaciones (histograma, boxplot, densidades).

En variables categóricas, la distribución de frecuencias revela desbalance y categorías raras. Una categoría con frecuencia ínfima puede no ser ruido: puede corresponder a un segmento de alto valor (por ejemplo, fraude real). El EDA riguroso evita tanto la eliminación automática como la conservación acrítica.

### 2.3 Relación entre variables y target

En clasificación, conviene analizar separabilidad aproximada: cómo varían las distribuciones de features entre clases. En regresión, interesa relación funcional (lineal, no lineal, saturación, umbrales). La correlación de Pearson mide asociación lineal, no dependencia general. Dos variables pueden tener correlación cercana a cero y relación no lineal fuerte. Este matiz evita errores de descarte prematuro.

### 2.4 Faltantes: tipologías y consecuencias

La teoría distingue tres mecanismos:

- MCAR (Missing Completely At Random): la ausencia es independiente de observados y no observados.
- MAR (Missing At Random): depende de variables observadas.
- MNAR (Missing Not At Random): depende del propio valor no observado.

La diferencia importa porque condiciona sesgo de imputación. Imputar por media bajo MNAR puede distorsionar relaciones de forma severa. En práctica, rara vez sabemos el mecanismo exacto, pero sí podemos investigar patrones de ausencia y usar indicadores de faltante para capturar señal.

### 2.5 Outliers: error, rareza o subpoblación

Un outlier puede ser:

1. error de carga;
2. evento raro legítimo;
3. evidencia de mezcla de poblaciones.

El tratamiento cambia en cada caso. En datasets financieros, un valor extremo puede representar fraude real; eliminarlo puede degradar el modelo justo donde más importa. En sensores industriales, un pico imposible puede ser fallo instrumental y conviene corregir o excluir.

### 2.6 Leakage detectado en EDA

Si una feature es una transformación del target o se registra después del evento a predecir, cualquier desempeño alto será ilusorio. Ejemplo clásico: predecir mora con una variable que se actualiza tras entrar en mora. En examen, identificar leakage suele valer más que elegir algoritmo.

### 2.7 Salida pedagógica del EDA

Al cerrar EDA debe existir un diagnóstico: qué problemas de calidad hay, qué transformaciones se justifican, qué variables son prometedoras, dónde hay riesgo de sesgo y qué hipótesis de modelado son plausibles. Sin ese diagnóstico, el pipeline posterior carece de fundamento.

## Capítulo 3. Preprocesamiento y representación: preparar datos para aprender

Preprocesar no es “limpiar por limpiar”. Es diseñar una representación \(\phi(X)\) que haga aprendible el patrón relevante y reduzca ruido irrelevante.

### 3.1 Pipeline y separación train/test

Todo transformador que aprende parámetros (imputador, escalador, PCA, selector) debe ajustarse con train y aplicarse a valid/test. Formalmente, si un escalador estima \(\mu_j,\sigma_j\), esos parámetros deben calcularse con train:

\[
\tilde x_{ij}=\frac{x_{ij}-\mu_j^{train}}{\sigma_j^{train}}.
\]

Calcularlos con todo el dataset introduce información de test en entrenamiento y sesga evaluación.

### 3.2 Imputación: decisión estadística, no relleno mecánico

Imputar media, mediana, moda, KNN o modelos múltiples implica supuestos distintos. La mediana suele ser robusta a colas; KNN preserva estructura local pero escala mal y puede amplificar ruido. Imputación múltiple refleja incertidumbre mejor que imputación única, aunque con mayor costo operativo.

Una práctica recomendable es comparar al menos dos estrategias dentro de validación cruzada y medir impacto real en métrica objetivo.

### 3.3 Encoding de categóricas

En nominales, one-hot evita orden artificial. En ordinales, puede usarse codificación ordenada si el orden es semántico real. En alta cardinalidad, target encoding puede ser útil, pero debe hacerse con estrategias anti-leakage (por ejemplo, encoding por fold). Si se calcula promedio de target por categoría usando todo train sin cuidado, el modelo puede sobreajustar fuertemente.

### 3.4 Escalado y su dependencia del modelo

KNN, SVM con kernel RBF, regresión regularizada y redes neuronales suelen requerir escalado por sensibilidad a magnitud. Árboles y ensambles de árboles son casi invariantes a transformaciones monótonas de escala. Esta distinción es frecuente en examen: no responder con “siempre escalar”.

### 3.5 Transformaciones de distribución

Aplicar \(\log(x+c)\), Box-Cox o Yeo-Johnson puede estabilizar varianza y reducir asimetría. El término \(c\) evita logaritmo de cero. La interpretación cambia: en una regresión lineal sobre \(\log y\), efectos marginales aproximan cambios porcentuales.

### 3.6 Feature engineering como ventaja competitiva

Crear variables derivadas suele aportar más que cambiar de algoritmo. Ejemplos:

- razón ingreso/cuota en riesgo crediticio;
- antigüedad de cliente en churn;
- estacionalidad (mes, semana, feriado) en series;
- n-gramas y longitud en NLP.

La regla es que cada feature nueva tenga justificación conceptual, no proliferación indiscriminada.

### 3.7 Selección de variables y multicolinealidad

En modelos lineales, alta colinealidad infla varianza de coeficientes. El VIF (Variance Inflation Factor) ayuda a diagnosticarla. Regularización L2 mitiga inestabilidad; L1 puede llevar algunos coeficientes a cero (selección implícita). En árboles, la colinealidad afecta menos el ajuste pero puede distorsionar importancias de variables.

## Capítulo 4. Evaluación rigurosa: generalización, validación y métricas

Evaluar bien significa estimar desempeño futuro, no celebrar desempeño pasado.

### 4.1 Error de entrenamiento vs error de generalización

Si \(\hat f\) minimiza riesgo empírico, no necesariamente minimiza riesgo poblacional. La brecha \(\mathcal{R}(\hat f)-\hat{\mathcal{R}}(\hat f)\) refleja generalización. Modelos muy flexibles pueden reducir \(\hat{\mathcal{R}}\) casi a cero y aun así fallar fuera de muestra.

### 4.2 Hold-out, cross-validation y nested CV

- **Hold-out**: simple, rápido, más varianza en estimación.
- **K-fold CV**: mejor estabilidad, mayor costo.
- **Nested CV**: necesaria cuando se ajustan hiperparámetros y se quiere estimación menos sesgada del desempeño final.

Confundir validación para tuning con evaluación final produce optimismo indebido.

### 4.3 Matriz de confusión y métricas de clasificación

Sea TP, TN, FP, FN:

\[
\text{Accuracy}=\frac{TP+TN}{TP+TN+FP+FN}
\]
\[
\text{Precision}=\frac{TP}{TP+FP},\quad
\text{Recall}=\frac{TP}{TP+FN}
\]
\[
F1=2\cdot\frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
\]

Cada fórmula expresa un compromiso distinto. La clave no es memorizarlas, sino leer qué error ponderan.

### 4.4 ROC, AUC y PR-AUC

ROC evalúa trade-off TPR/FPR en todos los umbrales. AUC-ROC es útil, pero en clases extremadamente desbalanceadas la curva Precision-Recall suele ser más informativa porque se centra en rendimiento sobre positivos.

### 4.5 Métricas de regresión

\[
MAE=\frac{1}{n}\sum|y_i-\hat y_i|,
\quad
MSE=\frac{1}{n}\sum(y_i-\hat y_i)^2,
\quad
RMSE=\sqrt{MSE}
\]

MSE/RMSE penalizan más errores grandes. MAE es más robusta y directamente interpretable en unidades del target. \(R^2\) mide proporción de varianza explicada respecto a predictor constante, pero un \(R^2\) alto no garantiza utilidad operativa si error absoluto sigue siendo costoso.

### 4.6 Calibración de probabilidades

Un clasificador puede discriminar bien pero estar mal calibrado. Si predice 0.8, idealmente cerca del 80% de casos con ese score debería ser positivo. Métodos como Platt scaling o isotonic regression corrigen calibración. En decisiones de riesgo, calibrar puede ser tan importante como clasificar.

### 4.7 Significancia práctica vs estadística

Una mejora de 0.003 en AUC puede ser estadísticamente detectable y operacionalmente irrelevante. El criterio profesional exige traducir métricas a impacto: costo evitado, tiempo ahorrado, casos críticos recuperados.

## Capítulo 5. Modelos supervisados fundamentales

No existe modelo universalmente mejor ("no free lunch"). Cada familia incorpora sesgos inductivos distintos.

### 5.1 Regresión lineal

Modelo:
\[
y=\beta_0+\sum_{j=1}^p\beta_jx_j+\varepsilon
\]

Interpretación: \(\beta_j\) es cambio esperado en \(y\) ante aumento unitario de \(x_j\), manteniendo lo demás constante. Supuestos clásicos: linealidad, independencia, homocedasticidad, normalidad de errores (para inferencia exacta), no colinealidad extrema.

Aunque simple, la regresión lineal es poderosa por interpretabilidad. En examen suelen pedir: significado de coeficientes, impacto de colinealidad, lectura de residuos.

### 5.2 Regresión logística

Para clasificación binaria:
\[
P(Y=1|x)=\sigma(z)=\frac{1}{1+e^{-z}},\quad z=\beta_0+\beta^Tx
\]

El logit linealiza odds:
\[
\log\frac{p}{1-p}=\beta_0+\beta^Tx
\]

\(e^{\beta_j}\) se interpreta como multiplicador de odds por unidad de \(x_j\). Error común: interpretar coeficiente como cambio lineal en probabilidad. No lo es; depende del punto de operación por no linealidad de \(\sigma\).

### 5.3 K-Nearest Neighbors (KNN)

Predice según vecinos más cercanos bajo métrica (usualmente euclídea). Ventajas: simplicidad, no paramétrico. Limitaciones: sensible a escala, costo alto en inferencia, degradación en alta dimensión (curse of dimensionality). Elegir \(k\) controla sesgo-varianza: \(k\) pequeño baja sesgo, sube varianza; \(k\) grande al revés.

### 5.4 Naive Bayes

Basado en Bayes con independencia condicional entre features:
\[
P(y|x)\propto P(y)\prod_j P(x_j|y)
\]

Aunque el supuesto es fuerte y rara vez exacto, puede funcionar muy bien en texto y problemas de alta dimensionalidad dispersa.

### 5.5 SVM

Busca hiperplano con margen máximo. En caso no separable usa margen blando con parámetro \(C\). Con kernel trick puede representar fronteras no lineales sin mapear explícitamente a dimensión alta.

- \(C\) alto: penaliza más errores, frontera más ajustada.
- \(\gamma\) alto (RBF): fronteras más locales/onduladas, mayor riesgo de overfitting.

## Capítulo 6. Árboles de decisión

Los árboles aprenden reglas tipo “si-entonces” mediante particiones recursivas del espacio de features.

### 6.1 Criterios de partición

En clasificación se usan impurezas como Gini o entropía.

\[
Gini=1-\sum_k p_k^2,
\quad
Entropy=-\sum_k p_k\log p_k
\]

La ganancia de información mide reducción de impureza tras split. En regresión se usa reducción de varianza o MSE intra-nodo.

### 6.2 Sobreajuste y poda

Árboles profundos memorizan entrenamiento. Controlar profundidad máxima, mínimo de muestras por hoja y poda de complejidad-coste reduce varianza. Interpretabilidad alta es ventaja fuerte, pero árboles únicos suelen ser inestables: pequeñas variaciones de datos pueden cambiar estructura.

### 6.3 Importancia de variables y cautelas

Importancias basadas en reducción de impureza pueden sesgarse hacia variables con muchas categorías o rangos amplios. Conviene complementar con permutation importance y análisis de estabilidad.

## Capítulo 7. Ensambles: bagging, boosting y stacking

Combinar modelos débiles o inestables puede producir estimadores más robustos.

### 7.1 Bagging y Random Forest

Bagging entrena múltiples modelos sobre muestras bootstrap y promedia/vota. Reduce varianza.

Random Forest agrega aleatoriedad en selección de features por split, disminuyendo correlación entre árboles. Resultado: menor varianza sin aumento fuerte de sesgo.

Hiperparámetros relevantes: número de árboles, profundidad, max_features, min_samples_leaf. Más árboles suele estabilizar rendimiento hasta saturación.

### 7.2 Boosting (AdaBoost, Gradient Boosting, XGBoost)

Boosting construye modelos secuenciales que corrigen errores previos. En Gradient Boosting se ajusta cada nuevo árbol a gradiente negativo de la pérdida.

XGBoost añade regularización explícita, shrinkage (learning rate), subsampling y manejo eficiente de faltantes. Es potente, pero sensible a tuning: learning rate muy alto o árboles muy profundos pueden sobreajustar.

### 7.3 Bias-variance en ensambles

Bagging: reduce varianza.
Boosting: reduce sesgo (y puede también varianza según configuración), pero más propenso a sobreajuste si no se regula.

### 7.4 Stacking

Combina predicciones de modelos base mediante un meta-modelo. Debe entrenarse con predicciones out-of-fold para evitar leakage.

## Capítulo 8. Clustering: estructura sin etiquetas

Clustering no “descubre la verdad” automáticamente. Propone una organización posible del espacio según una métrica y un criterio.

### 8.1 K-means

Objetivo:
\[
\min_{C_1,\dots,C_K}\sum_{k=1}^K\sum_{x_i\in C_k}||x_i-\mu_k||^2
\]

Supone clusters aproximadamente esféricos y similares en tamaño bajo distancia euclídea. Sensible a escala y a inicialización. Conviene ejecutar múltiples inicializaciones (k-means++ ayuda).

Elegir \(K\): método del codo, silhouette, criterio de negocio e interpretabilidad.

### 8.2 Clustering jerárquico

No requiere fijar \(K\) al inicio. Construye dendrograma mediante criterios de enlace (single, complete, average, Ward). Permite analizar estructura multiescala, pero escala peor en datasets grandes.

### 8.3 DBSCAN

Define clusters como regiones densas (\(\varepsilon\), minPts). Ventaja: detecta formas arbitrarias y etiqueta ruido explícitamente. Limitación: sensible a parámetros y dificultad en densidades variables.

### 8.4 Evaluación interna y externa

Interna: silhouette, Davies-Bouldin.
Externa (si hay etiquetas de referencia): ARI, NMI.

Pero ninguna métrica reemplaza interpretación de utilidad. Un clustering “matemáticamente prolijo” puede ser operativamente inútil.

## Capítulo 9. Reducción de dimensionalidad

La alta dimensionalidad aumenta ruido, costo computacional y dificulta visualización.

### 9.1 PCA: fundamento algebraico

Dado \(X\) centrado, PCA encuentra direcciones ortogonales \(w_j\) que maximizan varianza proyectada:
\[
\max_{||w||=1} Var(Xw)
\]

>>>>>>> theirs
=======
# Ciencia de Datos: tratado de fundamentos, modelado y criterio profesional

## Introducción general: qué significa dominar ciencia de datos

La ciencia de datos no es una colección de algoritmos ni una secuencia mecánica de pasos. Es una disciplina de inferencia, representación y decisión. Su objeto real de estudio no son solamente los datos, sino la relación entre un fenómeno del mundo, su registro imperfecto en una base y las decisiones que tomamos a partir de ese registro. Esta afirmación puede parecer filosófica, pero tiene consecuencias metodológicas muy concretas: elegir una variable objetivo, diseñar una métrica, imputar faltantes, ajustar un modelo, interpretar un score y decidir si desplegar o no una solución son partes de un mismo proceso intelectual.

Cuando una persona estudia ciencia de datos de manera superficial, suele creer que el problema principal es “qué algoritmo usar”. En cambio, cuando alcanza un nivel más alto, entiende que la mayor parte de los errores graves aparece antes del algoritmo: formulaciones ambiguas del problema, features mal definidas, leakage, evaluación mal planteada, métricas incoherentes con el costo del error o interpretaciones causales de resultados meramente predictivos. Este texto adopta esa segunda perspectiva: cada técnica se presenta como respuesta a una necesidad, no como una receta aislada.

Para facilitar un aprendizaje profundo, avanzaremos desde la base conceptual hasta temas de modelado más complejos, pasando por las zonas que más cuestan: formalización matemática, interpretación de métricas, sesgo-varianza, diferencia entre ajuste y generalización, significado real de la validación cruzada, diseño de experimentos comparables y lectura crítica de resultados. El objetivo final no es que el lector “reconozca términos”, sino que pueda defender decisiones técnicas con argumentos sólidos, detectar errores comunes y resolver problemas de examen o de práctica profesional con rigor.

## Capítulo 1. Pensar correctamente el problema antes de modelar

Todo proyecto de ciencia de datos comienza con una traducción: pasamos de una pregunta del mundo real a una pregunta modelable. Esa traducción nunca es neutra. Si la formulamos mal, el mejor modelo del mundo optimizará el objetivo equivocado. Por eso este primer capítulo no es introductorio en sentido liviano; es fundacional.

### 1.1 Unidad de análisis, observación y variable

Un dataset tabular puede verse como una matriz \(X\in\mathbb{R}^{n\times p}\), donde \(n\) es el número de observaciones y \(p\) el número de variables predictoras (features). Si además existe una variable objetivo \(y\), estamos en un escenario supervisado. Sin embargo, esta notación compacta oculta una decisión clave: ¿qué representa cada fila?

Si cada fila representa una persona, el modelo aprende patrones entre personas. Si cada fila representa una transacción, aprende patrones entre eventos. Si cada fila representa una ventana temporal, aprende patrones agregados en tiempo. Cambiar la unidad de análisis cambia el significado de toda la tarea. Un error frecuente es mezclar unidades (por ejemplo, usar variables de cliente en un problema por transacción sin controlar dependencia intra-cliente), generando pseudo-replicación y evaluación inflada.

### 1.2 Tipos de tarea: clasificación, regresión, ranking, detección y segmentación

La división clásica entre clasificación, regresión y clustering es correcta, pero conviene ampliarla.

En **clasificación**, el modelo busca una función \(f:X\to\mathcal{C}\), donde \(\mathcal{C}\) es un conjunto finito de clases. Si \(|\mathcal{C}|=2\), hablamos de clasificación binaria; si \(|\mathcal{C}|>2\), multiclase; si una observación puede pertenecer simultáneamente a varias clases, multilabel.

En **regresión**, \(f:X\to\mathbb{R}\) (o \(\mathbb{R}^k\) si hay múltiples objetivos continuos). El error no es “acierto/fallo”, sino distancia entre valor real y predicho.

En **clustering**, no existe \(y\) durante el entrenamiento: buscamos una partición \(\{C_1,\dots,C_K\}\) que maximice similitud intra-cluster y minimice similitud inter-cluster según una métrica elegida.

También aparecen tareas de **ranking** (ordenar items por relevancia), **detección de anomalías** (identificar observaciones raras bajo una noción de normalidad), y **pronóstico temporal** (forecasting), donde la dependencia temporal rompe supuestos de i.i.d. (independencia e idéntica distribución).

Saber nombrar estas tareas no alcanza. Hay que entender qué cambia en cada una: tipo de pérdida, métrica adecuada, validación pertinente, riesgos de leakage y forma de interpretar resultados.

### 1.3 Supervisado vs no supervisado: diferencia epistemológica

En aprendizaje supervisado disponemos de pares \((x_i,y_i)\). El objetivo es estimar una función que minimice una pérdida esperada:

\[
\mathcal{R}(f)=\mathbb{E}_{(X,Y)\sim P}[L(Y,f(X))].
\]

Como \(P\) es desconocida, minimizamos el riesgo empírico sobre la muestra:

\[
\hat{\mathcal{R}}(f)=\frac{1}{n}\sum_{i=1}^n L(y_i,f(x_i)).
\]

En no supervisado no existe \(Y\) explícita: optimizamos criterios estructurales (varianza intra-cluster, reconstrucción, densidad, etc.). Por eso la evaluación es menos directa y más dependiente del objetivo analítico. Esta diferencia es central en examen: en supervisado preguntamos “¿predice bien?”. En no supervisado preguntamos “¿la estructura hallada es estable, interpretable y útil para una decisión?”.

### 1.4 El costo del error y la función de decisión

Un modelo produce salidas; un sistema produce decisiones. Confundir ambas cosas es peligroso. En clasificación binaria, muchos modelos generan \(\hat p(x)=P(Y=1\mid X=x)\). Convertir esa probabilidad en decisión requiere un umbral \(t\): decidir positivo si \(\hat p\ge t\). Ese \(t\) no debe fijarse por costumbre en 0.5. Debe responder al costo relativo de errores:

- falso positivo: costo \(C_{FP}\)
- falso negativo: costo \(C_{FN}\)

Si \(C_{FN}\gg C_{FP}\), conviene umbral bajo (más recall). Si \(C_{FP}\gg C_{FN}\), conviene umbral alto (más precision). La ciencia de datos aplicada exige esta traducción explícita entre métrica y negocio.

### 1.5 Errores conceptuales iniciales más frecuentes

El primer error es plantear una tarea predictiva con variables que no estarán disponibles al momento de inferencia (leakage estructural). El segundo es definir una variable objetivo con ruido de etiquetado no reconocido. El tercero es ignorar el horizonte temporal: entrenar con información futura para predecir el pasado. El cuarto es optimizar una métrica irrelevante para la decisión real.

Un lector que domina este capítulo puede responder con precisión preguntas del tipo: “¿Por qué este problema es clasificación y no regresión?”, “¿Qué cambia si la clase positiva es rara?”, “¿Qué variable objetivo sería metodológicamente válida?”, “¿Qué costo de error es prioritario?”.

## Capítulo 2. EDA (Análisis Exploratorio): leer el dataset como evidencia

Antes de modelar hay que entender qué dicen y qué ocultan los datos. El EDA no es adorno visual: es una investigación preliminar que reduce incertidumbre metodológica.

### 2.1 Estructura inicial y calidad de datos

Las primeras preguntas son concretas: tamaño de muestra, tipos de variables, porcentaje de faltantes, duplicados, columnas constantes, cardinalidad categórica, presencia de valores imposibles y coherencia de unidades.

Un chequeo de rango parece trivial, pero evita errores devastadores: edades negativas, fechas invertidas, montos en monedas mezcladas, tasas fuera de \([0,1]\), códigos de categoría inconsistentes por mayúsculas o acentos. Cada una de estas fallas puede introducir patrones espurios que un algoritmo aprovechará sin “saber” que son inválidos.

### 2.2 Estadística descriptiva con propósito

La media, mediana y desvío estándar deben interpretarse en contexto. Si una variable es asimétrica, la media puede ser poco representativa. Si tiene outliers extremos, el desvío puede inflarse. Por eso conviene combinar medidas robustas (mediana, IQR) con visualizaciones (histograma, boxplot, densidades).

En variables categóricas, la distribución de frecuencias revela desbalance y categorías raras. Una categoría con frecuencia ínfima puede no ser ruido: puede corresponder a un segmento de alto valor (por ejemplo, fraude real). El EDA riguroso evita tanto la eliminación automática como la conservación acrítica.

### 2.3 Relación entre variables y target

En clasificación, conviene analizar separabilidad aproximada: cómo varían las distribuciones de features entre clases. En regresión, interesa relación funcional (lineal, no lineal, saturación, umbrales). La correlación de Pearson mide asociación lineal, no dependencia general. Dos variables pueden tener correlación cercana a cero y relación no lineal fuerte. Este matiz evita errores de descarte prematuro.

### 2.4 Faltantes: tipologías y consecuencias

La teoría distingue tres mecanismos:

- MCAR (Missing Completely At Random): la ausencia es independiente de observados y no observados.
- MAR (Missing At Random): depende de variables observadas.
- MNAR (Missing Not At Random): depende del propio valor no observado.

La diferencia importa porque condiciona sesgo de imputación. Imputar por media bajo MNAR puede distorsionar relaciones de forma severa. En práctica, rara vez sabemos el mecanismo exacto, pero sí podemos investigar patrones de ausencia y usar indicadores de faltante para capturar señal.

### 2.5 Outliers: error, rareza o subpoblación

Un outlier puede ser:

1. error de carga;
2. evento raro legítimo;
3. evidencia de mezcla de poblaciones.

El tratamiento cambia en cada caso. En datasets financieros, un valor extremo puede representar fraude real; eliminarlo puede degradar el modelo justo donde más importa. En sensores industriales, un pico imposible puede ser fallo instrumental y conviene corregir o excluir.

### 2.6 Leakage detectado en EDA

Si una feature es una transformación del target o se registra después del evento a predecir, cualquier desempeño alto será ilusorio. Ejemplo clásico: predecir mora con una variable que se actualiza tras entrar en mora. En examen, identificar leakage suele valer más que elegir algoritmo.

### 2.7 Salida pedagógica del EDA

Al cerrar EDA debe existir un diagnóstico: qué problemas de calidad hay, qué transformaciones se justifican, qué variables son prometedoras, dónde hay riesgo de sesgo y qué hipótesis de modelado son plausibles. Sin ese diagnóstico, el pipeline posterior carece de fundamento.

## Capítulo 3. Preprocesamiento y representación: preparar datos para aprender

Preprocesar no es “limpiar por limpiar”. Es diseñar una representación \(\phi(X)\) que haga aprendible el patrón relevante y reduzca ruido irrelevante.

### 3.1 Pipeline y separación train/test

Todo transformador que aprende parámetros (imputador, escalador, PCA, selector) debe ajustarse con train y aplicarse a valid/test. Formalmente, si un escalador estima \(\mu_j,\sigma_j\), esos parámetros deben calcularse con train:

\[
\tilde x_{ij}=\frac{x_{ij}-\mu_j^{train}}{\sigma_j^{train}}.
\]

Calcularlos con todo el dataset introduce información de test en entrenamiento y sesga evaluación.

### 3.2 Imputación: decisión estadística, no relleno mecánico

Imputar media, mediana, moda, KNN o modelos múltiples implica supuestos distintos. La mediana suele ser robusta a colas; KNN preserva estructura local pero escala mal y puede amplificar ruido. Imputación múltiple refleja incertidumbre mejor que imputación única, aunque con mayor costo operativo.

Una práctica recomendable es comparar al menos dos estrategias dentro de validación cruzada y medir impacto real en métrica objetivo.

### 3.3 Encoding de categóricas

En nominales, one-hot evita orden artificial. En ordinales, puede usarse codificación ordenada si el orden es semántico real. En alta cardinalidad, target encoding puede ser útil, pero debe hacerse con estrategias anti-leakage (por ejemplo, encoding por fold). Si se calcula promedio de target por categoría usando todo train sin cuidado, el modelo puede sobreajustar fuertemente.

### 3.4 Escalado y su dependencia del modelo

KNN, SVM con kernel RBF, regresión regularizada y redes neuronales suelen requerir escalado por sensibilidad a magnitud. Árboles y ensambles de árboles son casi invariantes a transformaciones monótonas de escala. Esta distinción es frecuente en examen: no responder con “siempre escalar”.

### 3.5 Transformaciones de distribución

Aplicar \(\log(x+c)\), Box-Cox o Yeo-Johnson puede estabilizar varianza y reducir asimetría. El término \(c\) evita logaritmo de cero. La interpretación cambia: en una regresión lineal sobre \(\log y\), efectos marginales aproximan cambios porcentuales.

### 3.6 Feature engineering como ventaja competitiva

Crear variables derivadas suele aportar más que cambiar de algoritmo. Ejemplos:

- razón ingreso/cuota en riesgo crediticio;
- antigüedad de cliente en churn;
- estacionalidad (mes, semana, feriado) en series;
- n-gramas y longitud en NLP.

La regla es que cada feature nueva tenga justificación conceptual, no proliferación indiscriminada.

### 3.7 Selección de variables y multicolinealidad

En modelos lineales, alta colinealidad infla varianza de coeficientes. El VIF (Variance Inflation Factor) ayuda a diagnosticarla. Regularización L2 mitiga inestabilidad; L1 puede llevar algunos coeficientes a cero (selección implícita). En árboles, la colinealidad afecta menos el ajuste pero puede distorsionar importancias de variables.

## Capítulo 4. Evaluación rigurosa: generalización, validación y métricas

Evaluar bien significa estimar desempeño futuro, no celebrar desempeño pasado.

### 4.1 Error de entrenamiento vs error de generalización

Si \(\hat f\) minimiza riesgo empírico, no necesariamente minimiza riesgo poblacional. La brecha \(\mathcal{R}(\hat f)-\hat{\mathcal{R}}(\hat f)\) refleja generalización. Modelos muy flexibles pueden reducir \(\hat{\mathcal{R}}\) casi a cero y aun así fallar fuera de muestra.

### 4.2 Hold-out, cross-validation y nested CV

- **Hold-out**: simple, rápido, más varianza en estimación.
- **K-fold CV**: mejor estabilidad, mayor costo.
- **Nested CV**: necesaria cuando se ajustan hiperparámetros y se quiere estimación menos sesgada del desempeño final.

Confundir validación para tuning con evaluación final produce optimismo indebido.

### 4.3 Matriz de confusión y métricas de clasificación

Sea TP, TN, FP, FN:

\[
\text{Accuracy}=\frac{TP+TN}{TP+TN+FP+FN}
\]
\[
\text{Precision}=\frac{TP}{TP+FP},\quad
\text{Recall}=\frac{TP}{TP+FN}
\]
\[
F1=2\cdot\frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
\]

Cada fórmula expresa un compromiso distinto. La clave no es memorizarlas, sino leer qué error ponderan.

### 4.4 ROC, AUC y PR-AUC

ROC evalúa trade-off TPR/FPR en todos los umbrales. AUC-ROC es útil, pero en clases extremadamente desbalanceadas la curva Precision-Recall suele ser más informativa porque se centra en rendimiento sobre positivos.

### 4.5 Métricas de regresión

\[
MAE=\frac{1}{n}\sum|y_i-\hat y_i|,
\quad
MSE=\frac{1}{n}\sum(y_i-\hat y_i)^2,
\quad
RMSE=\sqrt{MSE}
\]

MSE/RMSE penalizan más errores grandes. MAE es más robusta y directamente interpretable en unidades del target. \(R^2\) mide proporción de varianza explicada respecto a predictor constante, pero un \(R^2\) alto no garantiza utilidad operativa si error absoluto sigue siendo costoso.

### 4.6 Calibración de probabilidades

Un clasificador puede discriminar bien pero estar mal calibrado. Si predice 0.8, idealmente cerca del 80% de casos con ese score debería ser positivo. Métodos como Platt scaling o isotonic regression corrigen calibración. En decisiones de riesgo, calibrar puede ser tan importante como clasificar.

### 4.7 Significancia práctica vs estadística

Una mejora de 0.003 en AUC puede ser estadísticamente detectable y operacionalmente irrelevante. El criterio profesional exige traducir métricas a impacto: costo evitado, tiempo ahorrado, casos críticos recuperados.

## Capítulo 5. Modelos supervisados fundamentales

No existe modelo universalmente mejor ("no free lunch"). Cada familia incorpora sesgos inductivos distintos.

### 5.1 Regresión lineal

Modelo:
\[
y=\beta_0+\sum_{j=1}^p\beta_jx_j+\varepsilon
\]

Interpretación: \(\beta_j\) es cambio esperado en \(y\) ante aumento unitario de \(x_j\), manteniendo lo demás constante. Supuestos clásicos: linealidad, independencia, homocedasticidad, normalidad de errores (para inferencia exacta), no colinealidad extrema.

Aunque simple, la regresión lineal es poderosa por interpretabilidad. En examen suelen pedir: significado de coeficientes, impacto de colinealidad, lectura de residuos.

### 5.2 Regresión logística

Para clasificación binaria:
\[
P(Y=1|x)=\sigma(z)=\frac{1}{1+e^{-z}},\quad z=\beta_0+\beta^Tx
\]

El logit linealiza odds:
\[
\log\frac{p}{1-p}=\beta_0+\beta^Tx
\]

\(e^{\beta_j}\) se interpreta como multiplicador de odds por unidad de \(x_j\). Error común: interpretar coeficiente como cambio lineal en probabilidad. No lo es; depende del punto de operación por no linealidad de \(\sigma\).

### 5.3 K-Nearest Neighbors (KNN)

Predice según vecinos más cercanos bajo métrica (usualmente euclídea). Ventajas: simplicidad, no paramétrico. Limitaciones: sensible a escala, costo alto en inferencia, degradación en alta dimensión (curse of dimensionality). Elegir \(k\) controla sesgo-varianza: \(k\) pequeño baja sesgo, sube varianza; \(k\) grande al revés.

### 5.4 Naive Bayes

Basado en Bayes con independencia condicional entre features:
\[
P(y|x)\propto P(y)\prod_j P(x_j|y)
\]

Aunque el supuesto es fuerte y rara vez exacto, puede funcionar muy bien en texto y problemas de alta dimensionalidad dispersa.

### 5.5 SVM

Busca hiperplano con margen máximo. En caso no separable usa margen blando con parámetro \(C\). Con kernel trick puede representar fronteras no lineales sin mapear explícitamente a dimensión alta.

- \(C\) alto: penaliza más errores, frontera más ajustada.
- \(\gamma\) alto (RBF): fronteras más locales/onduladas, mayor riesgo de overfitting.

## Capítulo 6. Árboles de decisión

Los árboles aprenden reglas tipo “si-entonces” mediante particiones recursivas del espacio de features.

### 6.1 Criterios de partición

En clasificación se usan impurezas como Gini o entropía.

\[
Gini=1-\sum_k p_k^2,
\quad
Entropy=-\sum_k p_k\log p_k
\]

La ganancia de información mide reducción de impureza tras split. En regresión se usa reducción de varianza o MSE intra-nodo.

### 6.2 Sobreajuste y poda

Árboles profundos memorizan entrenamiento. Controlar profundidad máxima, mínimo de muestras por hoja y poda de complejidad-coste reduce varianza. Interpretabilidad alta es ventaja fuerte, pero árboles únicos suelen ser inestables: pequeñas variaciones de datos pueden cambiar estructura.

### 6.3 Importancia de variables y cautelas

Importancias basadas en reducción de impureza pueden sesgarse hacia variables con muchas categorías o rangos amplios. Conviene complementar con permutation importance y análisis de estabilidad.

## Capítulo 7. Ensambles: bagging, boosting y stacking

Combinar modelos débiles o inestables puede producir estimadores más robustos.

### 7.1 Bagging y Random Forest

Bagging entrena múltiples modelos sobre muestras bootstrap y promedia/vota. Reduce varianza.

Random Forest agrega aleatoriedad en selección de features por split, disminuyendo correlación entre árboles. Resultado: menor varianza sin aumento fuerte de sesgo.

Hiperparámetros relevantes: número de árboles, profundidad, max_features, min_samples_leaf. Más árboles suele estabilizar rendimiento hasta saturación.

### 7.2 Boosting (AdaBoost, Gradient Boosting, XGBoost)

Boosting construye modelos secuenciales que corrigen errores previos. En Gradient Boosting se ajusta cada nuevo árbol a gradiente negativo de la pérdida.

XGBoost añade regularización explícita, shrinkage (learning rate), subsampling y manejo eficiente de faltantes. Es potente, pero sensible a tuning: learning rate muy alto o árboles muy profundos pueden sobreajustar.

### 7.3 Bias-variance en ensambles

Bagging: reduce varianza.
Boosting: reduce sesgo (y puede también varianza según configuración), pero más propenso a sobreajuste si no se regula.

### 7.4 Stacking

Combina predicciones de modelos base mediante un meta-modelo. Debe entrenarse con predicciones out-of-fold para evitar leakage.

## Capítulo 8. Clustering: estructura sin etiquetas

Clustering no “descubre la verdad” automáticamente. Propone una organización posible del espacio según una métrica y un criterio.

### 8.1 K-means

Objetivo:
\[
\min_{C_1,\dots,C_K}\sum_{k=1}^K\sum_{x_i\in C_k}||x_i-\mu_k||^2
\]

Supone clusters aproximadamente esféricos y similares en tamaño bajo distancia euclídea. Sensible a escala y a inicialización. Conviene ejecutar múltiples inicializaciones (k-means++ ayuda).

Elegir \(K\): método del codo, silhouette, criterio de negocio e interpretabilidad.

### 8.2 Clustering jerárquico

No requiere fijar \(K\) al inicio. Construye dendrograma mediante criterios de enlace (single, complete, average, Ward). Permite analizar estructura multiescala, pero escala peor en datasets grandes.

### 8.3 DBSCAN

Define clusters como regiones densas (\(\varepsilon\), minPts). Ventaja: detecta formas arbitrarias y etiqueta ruido explícitamente. Limitación: sensible a parámetros y dificultad en densidades variables.

### 8.4 Evaluación interna y externa

Interna: silhouette, Davies-Bouldin.
Externa (si hay etiquetas de referencia): ARI, NMI.

Pero ninguna métrica reemplaza interpretación de utilidad. Un clustering “matemáticamente prolijo” puede ser operativamente inútil.

## Capítulo 9. Reducción de dimensionalidad

La alta dimensionalidad aumenta ruido, costo computacional y dificulta visualización.

### 9.1 PCA: fundamento algebraico

Dado \(X\) centrado, PCA encuentra direcciones ortogonales \(w_j\) que maximizan varianza proyectada:
\[
\max_{||w||=1} Var(Xw)
\]

>>>>>>> theirs
Equivale a autovectores de matriz de covarianza \(\Sigma\). Los autovalores indican varianza explicada por componente.

Interpretación: PCA busca nuevas coordenadas lineales que concentren información de variabilidad. No usa target, por lo que puede descartar información predictiva si varianza no coincide con relevancia para \(y\).

### 9.2 Selección de número de componentes

Se usan curvas de varianza acumulada y criterio de codo, pero la decisión final debe balancear compresión, desempeño predictivo e interpretabilidad.

### 9.3 Métodos no lineales

t-SNE y UMAP son útiles para visualización de estructura local/global, pero no deben interpretarse como proyecciones métricamente fieles para inferencia cuantitativa estricta.

## Capítulo 10. Redes neuronales y aprendizaje profundo

### 10.1 Perceptrón y neurona artificial

Una neurona computa:
<<<<<<< ours
<<<<<<< ours
>>>>>>> theirs
=======
>>>>>>> theirs
=======
>>>>>>> theirs
\[
a=\phi(w^Tx+b)
\]

<<<<<<< ours
<<<<<<< ours
<<<<<<< ours
aplicando activación no lineal \(\phi\). Al apilar capas, la red compone transformaciones y aprende representaciones jerárquicas.

### 8.2 Propagación hacia adelante y función de pérdida

Para entrada \(x\), la red produce salida \(\hat y\). Se computa pérdida \(L(y,\hat y)\) y se actualizan parámetros para minimizarla.

### 8.3 Backpropagation y descenso por gradiente

Backprop aplica regla de la cadena para obtener gradientes de todos los parámetros eficientemente.

Actualización simple:

\[
\theta \leftarrow \theta - \eta \nabla_\theta L
\]

con \(\eta\) tasa de aprendizaje.

### 8.4 Problemas de entrenamiento y soluciones

- **Vanishing/exploding gradients**: mitigación con inicialización adecuada, normalización, activaciones ReLU/variantes.
- **Sobreajuste**: regularización L2, dropout, *early stopping*, aumento de datos.
- **Inestabilidad**: optimizadores adaptativos (Adam), *batch normalization*.

### 8.5 Cuándo usar deep learning

Funciona especialmente bien con datos no estructurados (imagen, audio, lenguaje) y gran escala. En tabular pequeño/mediano, ensambles de árboles suelen competir o superar con menor costo operativo.

---

## Capítulo 9. Procesamiento de lenguaje natural (NLP): de texto crudo a representación semántica

### 9.1 El desafío central

El texto no llega como vector numérico; llega como secuencia simbólica ambigua, contextual y ruidosa. La tarea es mapear texto a representaciones útiles para inferencia.

### 9.2 Pipeline clásico

1. limpieza básica,
2. tokenización,
3. normalización (minúsculas, lematización/ stemming según objetivo),
4. vectorización.

### 9.3 BoW y TF-IDF

En bolsa de palabras, cada dimensión representa término.

TF-IDF pondera por frecuencia local e inversa de frecuencia documental:

\[
\text{tfidf}(t,d)=\text{tf}(t,d)\cdot \log\frac{N}{df(t)}
\]

Intuición: términos frecuentes en un documento y raros globalmente son más informativos.

### 9.4 Embeddings

Word embeddings densos (Word2Vec, GloVe) capturan similitud semántica distribuida. Modelos contextuales (BERT y variantes) generan representaciones dependientes del contexto de aparición.

### 9.5 Limitaciones y sesgos

Representaciones de lenguaje pueden codificar sesgos presentes en corpus. Evaluar equidad y riesgo reputacional es parte del trabajo profesional, no un agregado opcional.

---

## Capítulo 10. Series temporales: dependencia temporal, estación y pronóstico

### 10.1 Por qué no vale mezclar todo al azar

En series temporales, el orden importa. Hacer *shuffle* indiscriminado destruye dependencia temporal y produce evaluación ilusoria.

### 10.2 Componentes conceptuales

Una serie \(y_t\) puede descomponerse (según modelo) en:

- tendencia,
- estacionalidad,
- ciclo,
- ruido.

### 10.3 Features temporales y fugas

Se usan lags \(y_{t-1}, y_{t-2},\dots\), medias móviles y variables calendarias. Regla crítica: toda feature debe construirse solo con información disponible hasta tiempo \(t\). Usar datos futuros produce fuga temporal.

### 10.4 Evaluación temporal

Se emplea *walk-forward validation*: entrenar en ventana histórica y validar en bloque futuro, repitiendo con ventana expandida o deslizante.

---

## Capítulo 11. Sesgo-varianza, complejidad y regularización: marco unificador

### 11.1 Descomposición conceptual

Error esperado puede pensarse como combinación de:

- sesgo (error sistemático por modelo demasiado rígido),
- varianza (sensibilidad excesiva a fluctuaciones de muestra),
- ruido irreducible.

Modelos simples: alto sesgo, baja varianza.
Modelos complejos: bajo sesgo, alta varianza.

### 11.2 Herramientas prácticas

- reducir complejidad del modelo,
- regularizar,
- aumentar datos,
- ensambles,
- selección de variables,
- validación rigurosa.

La maestría técnica consiste en navegar este compromiso según objetivo y costo del error.

---

## Capítulo 12. Interpretabilidad y explicabilidad: entender por qué el modelo decide

### 12.1 Distinción útil

- **Interpretabilidad intrínseca**: modelo transparente por diseño (lineales, árboles pequeños).
- **Explicabilidad post hoc**: técnicas sobre modelos complejos (SHAP, LIME, importancia por permutación).

### 12.2 Importancia global y local

- Global: qué variables dominan comportamiento general.
- Local: por qué una predicción puntual fue alta/baja.

Error frecuente: usar explicaciones locales como leyes universales.

### 12.3 Riesgos de malinterpretación

- correlación explicada como causalidad,
- importancia alta confundida con utilidad de intervención,
- explicaciones inestables por colinealidad fuerte.

Un sistema profesional combina explicación técnica con conocimiento de dominio y revisión crítica.

---

## Capítulo 13. Diseño experimental, selección de modelos y tuning de hiperparámetros

### 13.1 Hiperparámetros versus parámetros

- Parámetros: aprendidos del dato (pesos, coeficientes).
- Hiperparámetros: configuran el aprendizaje (profundidad de árbol, \(C\), \(\lambda\), learning rate).

### 13.2 Búsqueda

- Grid Search: exhaustiva en grilla fija.
- Random Search: más eficiente en espacios amplios.
- Bayesian optimization: explota estructura del problema de búsqueda.

### 13.3 Regla de oro

El conjunto de test no participa en selección de hiperparámetros. Se reserva para estimación final.

### 13.4 Comparación estadística

Diferencias pequeñas de métrica pueden no ser robustas. Conviene observar dispersión por folds y estabilidad en distintos cortes temporales/poblacionales.

---

## Capítulo 14. MLOps y ciclo de vida: del notebook al sistema confiable

### 14.1 Reproducibilidad

Debe poder reconstruirse:

- datos usados,
- versión de código,
- hiperparámetros,
- métricas,
- artefactos de modelo.

Sin reproducibilidad no hay auditoría técnica.

### 14.2 Despliegue y monitoreo

Luego de producción, el problema cambia: ahora importa detectar degradación.

Tipos de drift:

- **Data drift**: cambia distribución de \(X\).
- **Concept drift**: cambia \(P(Y\mid X)\).

Se monitorean métricas de negocio y de modelo, con umbrales y políticas de reentrenamiento.

### 14.3 Latencia, costo y robustez

Un modelo excelente offline puede ser inviable online por latencia o costo. Ingeniería de sistemas y modelado deben co-diseñarse.

---

## Capítulo 15. Ética, sesgo algorítmico y gobernanza

### 15.1 Por qué este capítulo es técnico y no ornamental

Las decisiones automáticas impactan personas. La calidad de un modelo no se reduce a precisión; incluye justicia, seguridad, transparencia y posibilidad de apelación.

### 15.2 Fuentes de sesgo

- sesgo de muestreo,
- sesgo de etiqueta,
- sesgo histórico,
- sesgo de despliegue (uso fuera del dominio de entrenamiento).

### 15.3 Métricas de equidad (visión introductoria)

Diferentes nociones pueden ser incompatibles entre sí (paridad demográfica, igualdad de oportunidades, etc.). No existe criterio único universal; se elige según contexto normativo y riesgo social.

### 15.4 Gobernanza mínima responsable

- documentación de datos/modelo,
- evaluación por subgrupos,
- trazabilidad de decisiones,
- protocolos de auditoría y revisión humana.

---

## Capítulo 16. Ejemplo integrador completo: de problema crudo a solución evaluada

Supongamos objetivo: predecir morosidad en créditos de consumo.

1. **Definición de target**: mora > 90 días dentro de 12 meses.
2. **Unidad de análisis**: solicitud de crédito.
3. **Partición temporal**: train (2022-2024), validación (2025H1), test (2025H2).
4. **EDA**: desbalance 8% positivos, faltantes MNAR en ingreso declarado.
5. **Preprocesamiento**: imputación por segmento + indicador de faltante; one-hot para categóricas moderadas; target encoding con validación interna para alta cardinalidad.
6. **Modelos**: baseline logístico regularizado, Random Forest, XGBoost.
7. **Métrica principal**: AUC-PR y recall en umbral de costo definido por riesgo.
8. **Calibración**: isotonic/Platt según desempeño.
9. **Interpretabilidad**: SHAP global/local + análisis por segmento.
10. **Monitoreo en producción**: drift de ingreso, empleo y comportamiento de pago.

Este flujo muestra el principio rector del tratado: no hay paso aislado; cada decisión depende de la anterior y condiciona la siguiente.

---

## Capítulo 17. Errores típicos en examen y en práctica profesional

1. Elegir algoritmo antes de definir problema y métrica.
2. Usar accuracy con fuerte desbalance sin análisis adicional.
3. Hacer preprocesamiento antes del split (fuga).
4. Confundir correlación con causalidad.
5. Evaluar con test durante tuning.
6. Ignorar costo diferencial de errores.
7. Celebrar métricas sin revisar calibración ni estabilidad temporal.
8. Suponer que explicación post hoc equivale a prueba causal.

Dominar la materia implica detectar y prevenir estos errores de forma sistemática.

---

## Capítulo 18. Guía de estudio para dominio experto

Para consolidar un nivel alto, cada tema debe poder responderse en tres registros:

1. **Intuitivo**: explicar en lenguaje claro qué problema resuelve.
2. **Formal**: escribir la formulación matemática esencial y definir símbolos.
3. **Aplicado**: justificar decisiones metodológicas en un caso real.

Si falta uno de los tres, el aprendizaje está incompleto.

Un buen entrenamiento para examen oral o escrito es tomar cada técnica y responder:

- cuándo usarla,
- cuándo no usarla,
- qué asume,
- cómo se evalúa,
- qué errores comete un principiante,
- cómo defender su uso frente a alternativas.

---

## Cierre: arquitectura conceptual de la materia

La ciencia de datos se vuelve coherente cuando se entiende su arquitectura:

1. plantear bien el problema,
2. construir representación útil de datos,
3. entrenar con criterio estadístico,
4. evaluar para generalizar,
5. interpretar y decidir,
6. operar y auditar en producción.

Cada módulo del curso ocupa una pieza de esta arquitectura. Aprender “en serio” no es acumular técnicas, sino integrar estas piezas en un proceso disciplinar robusto.

Cuando este proceso está internalizado, el estudiante deja de preguntar “¿qué modelo uso?” como primera reacción. Empieza a preguntar, con precisión profesional: **¿qué decisión quiero sostener, con qué evidencia, bajo qué supuestos y con qué riesgos?** En ese punto, la materia deja de ser un conjunto de temas y se convierte en competencia experta.
=======
=======
>>>>>>> theirs
=======
>>>>>>> theirs
\(w\): pesos, \(b\): sesgo, \(\phi\): activación (ReLU, sigmoid, tanh). Una capa lineal sin activación no agrega no linealidad; múltiples capas con activaciones permiten aproximar funciones complejas.

### 10.2 Función de pérdida y backpropagation

Entrenar red implica minimizar pérdida \(\mathcal{L}(\theta)\) con descenso por gradiente (SGD/Adam):
\[
\theta_{t+1}=\theta_t-\eta\nabla_\theta \mathcal{L}(\theta_t)
\]

Backpropagation aplica regla de la cadena para propagar gradientes desde salida hacia capas internas.

### 10.3 Riesgos y regularización

Redes profundas tienen alta capacidad: pueden sobreajustar si datos son pocos o ruidosos. Herramientas: dropout, weight decay, early stopping, data augmentation, batch normalization.

### 10.4 Cuándo usar deep learning

Es ventajoso con datos masivos no estructurados (imagen, audio, texto) y patrones complejos. En tablas pequeñas/medianas, modelos de árboles frecuentemente compiten o superan con menor costo y mayor interpretabilidad.

## Capítulo 11. Procesamiento de lenguaje natural (NLP)

El texto debe transformarse en representación numérica.

### 11.1 Pipeline básico

Normalización, tokenización, manejo de stopwords, stemming/lemmatización según objetivo. No existe preprocesamiento universal: eliminar negaciones puede destruir señal en análisis de sentimiento.

### 11.2 Bag of Words y TF-IDF

En BoW, cada documento se representa por conteos de términos. TF-IDF pondera términos frecuentes en documento pero raros en corpus:
\[
TFIDF(t,d)=TF(t,d)\cdot\log\frac{N}{df(t)}
\]

Ventaja: simplicidad e interpretabilidad. Limitación: ignora orden y contexto semántico profundo.

### 11.3 Embeddings

Word2Vec/GloVe asignan vectores densos que capturan proximidad semántica. Modelos contextualizados (BERT y derivados) producen embeddings dependientes del contexto. Esto resuelve ambigüedad léxica parcial y mejora tareas complejas.

### 11.4 Modelado y evaluación en NLP

Clasificación de texto usa frecuentemente logística, SVM, árboles o transformers. Métricas: accuracy/F1, pero con clases desbalanceadas conviene macro-F1. En recuperación y ranking, métricas como MAP o NDCG cobran relevancia.

## Capítulo 12. Integración metodológica: arquitectura completa de un proyecto

Una visión experta entiende la materia como sistema integrado:

1. Formular problema y criterio de éxito.
2. Auditar datos y riesgos (calidad, sesgo, leakage).
3. Diseñar representación y pipeline reproducible.
4. Entrenar baselines simples.
5. Comparar modelos con validación rigurosa.
6. Ajustar hiperparámetros con control de sobreajuste.
7. Interpretar resultados y calibrar decisión.
8. Validar robustez y equidad.
9. Preparar despliegue, monitoreo y retraining.

### 12.1 Reproducibilidad

Semillas aleatorias, versionado de datos y código, trazabilidad de experimentos. Sin reproducibilidad no hay ciencia ni auditoría técnica seria.

### 12.2 Sesgo, equidad y deriva

Un modelo puede ser globalmente preciso y perjudicar subgrupos específicos. Hay que evaluar métricas segmentadas. Tras despliegue, distribución de datos cambia (data drift) y relación \(X\to y\) puede degradarse (concept drift). Monitorear deriva es obligatorio en sistemas vivos.

### 12.3 Interpretabilidad y explicabilidad

Interpretabilidad intrínseca (modelos simples, coeficientes, reglas) vs explicabilidad post-hoc (SHAP, LIME, PDP). Explicar no es justificar causalmente; es describir comportamiento del modelo bajo supuestos locales o globales.

## Capítulo 13. Estrategia para examen y dominio experto

Responder bien en examen requiere más que definiciones memorizadas. Se evalúa capacidad de argumentar decisiones.

### 13.1 Qué suelen exigir

- Diferenciar tareas (clasificación/regresión/clustering) con implicancias métricas.
- Justificar pipeline de preprocesamiento.
- Detectar leakage y errores de validación.
- Comparar modelos por sesgo-varianza, interpretabilidad y costo.
- Interpretar métricas según contexto de error.

### 13.2 Errores que penalizan

- Elegir accuracy en clases muy desbalanceadas sin discusión.
- Escalar o imputar con todo el dataset antes del split.
- Afirmar causalidad desde modelos predictivos.
- Describir técnicas sin conectar con el problema planteado.

### 13.3 Cómo construir respuestas de alto nivel

Una buena respuesta suele seguir esta secuencia: definir el problema, formalizarlo, proponer método, justificar supuestos, discutir limitaciones, interpretar resultados y señalar alternativas. Mostrar conciencia de límites vale tanto como exponer fortalezas.

## Conclusión: de ejecutar algoritmos a razonar científicamente

Dominar ciencia de datos es pasar de la ejecución instrumental al razonamiento científico aplicado. Un profesional sólido no se define por conocer más librerías, sino por poder explicar por qué una representación es válida, por qué una métrica es coherente con el costo del error, por qué un experimento está bien diseñado y por qué un resultado merece confianza.

Esta obra buscó construir ese recorrido completo: desde la formulación del problema hasta la evaluación rigurosa, desde los modelos clásicos hasta enfoques más complejos, desde la técnica matemática hasta la responsabilidad interpretativa. Si el lector puede ahora plantear problemas con precisión, diseñar pipelines sin leakage, comparar modelos con criterio, interpretar métricas en contexto y reconocer límites de inferencia, entonces alcanzó el objetivo central de la materia: un dominio profundo, transferible y profesional de la ciencia de datos.
<<<<<<< ours
<<<<<<< ours
>>>>>>> theirs
=======
>>>>>>> theirs
=======
>>>>>>> theirs
