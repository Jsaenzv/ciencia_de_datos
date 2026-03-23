# Resumen integral de Ciencia de Datos

## 1. MAPA DE LA MATERIA

### 1.1 Los grandes ejes que organizan toda la cursada

La materia, vista como un todo, no trata solamente de “aprender algoritmos”. Su lógica real es mucho más interesante: enseña a pasar de **datos crudos** a **decisiones justificadas**. Si uno mira juntos los contenidos teóricos, los trabajos prácticos y los parciales, aparece un recorrido bastante claro.

#### Eje 1 — Entender qué problema tengo delante
Antes de pensar en modelos, la materia exige reconocer la estructura básica del problema.

- ¿La variable objetivo es categórica? Entonces probablemente estamos ante **clasificación**.
- ¿La salida es numérica continua? Entonces hablamos de **regresión**.
- ¿No hay variable objetivo y queremos encontrar grupos? Entonces entramos en **clustering**.

Este eje parece elemental, pero en realidad sostiene casi todo lo demás. Si uno confunde el tipo de problema, después elige mal la métrica, mal el modelo y hasta mal el preprocesamiento.

#### Eje 2 — Leer el dataset antes de modelar
El análisis exploratorio no es un adorno previo al modelado. En esta materia aparece como un paso de diagnóstico.

Acá se aprende a preguntar:

- qué variables tengo,
- qué significan,
- cómo se distribuyen,
- si hay faltantes,
- si hay outliers,
- si las variables se relacionan,
- si conviene crear features nuevas,
- si hay transformaciones necesarias.

La idea profunda es esta: **un modelo nunca arregla automáticamente un mal entendimiento de los datos**.

#### Eje 3 — Preparar los datos para que el algoritmo pueda aprender
Muchos temas de la materia aparecen porque los datos reales no vienen listos para usar.

Por eso tienen tanto peso:

- imputación de faltantes,
- tratamiento de outliers,
- encoding de variables categóricas,
- escalado o normalización,
- discretización,
- transformación de variables sesgadas,
- feature engineering.

El mensaje de fondo es que modelar no consiste solo en “correr sklearn”, sino en **representar bien el problema**.

#### Eje 4 — Evaluar correctamente y no engañarse con resultados lindos
Este es uno de los núcleos más importantes de toda la materia.

No alcanza con entrenar un modelo y obtener un número alto. La pregunta correcta es: **¿ese modelo generaliza o solo memorizó?**

Por eso aparecen una y otra vez:

- train/test split,
- validación cruzada,
- comparación entre performance de entrenamiento y test,
- matrices de confusión,
- métricas según el tipo de problema,
- sesgo y varianza,
- overfitting y underfitting.

Este eje es casi una filosofía de trabajo: en ciencia de datos no gana el modelo que mejor queda en entrenamiento, sino el que mejor se comporta frente a datos nuevos.

#### Eje 5 — Elegir y defender modelos
La materia recorre varias familias de métodos, pero no con el espíritu de memorizar catálogos. Lo que se espera es entender qué resuelve cada técnica, qué costo paga y cuándo tiene sentido usarla.

Aparecen con más fuerza:

- árboles de decisión,
- Random Forest,
- regresión lineal,
- regresión logística,
- XGBoost,
- K-Means,
- PCA, t-SNE, ISOMAP,
- ensambles,
- perceptrón y redes neuronales,
- modelos sobre texto.

#### Eje 6 — Interpretar resultados
En esta materia no se premia solamente obtener performance; también importa explicar.

Hay que poder interpretar:

- las primeras reglas de un árbol,
- la importancia de variables,
- la diferencia entre precision y recall según contexto,
- qué representa un cluster,
- qué preserva una técnica de reducción de dimensionalidad,
- por qué un ensamble mejora o empeora interpretabilidad,
- por qué cierta arquitectura de red o cierto pipeline textual tiene sentido.

#### Eje 7 — Integración en problemas reales
Los TP muestran la versión aplicada de toda la materia:

- EDA real sobre taxis,
- clasificación binaria sobre clima,
- regresión sobre AirBnB,
- clustering sobre Spotify,
- regresión sobre texto para predecir story points.

Eso revela algo importante: la materia no enseña conceptos aislados, sino **pipelines completos**.

---

### 1.2 Conceptos fundamentales de verdad

Estos son los conceptos que estructuran todo el resto. Si un alumno entiende de verdad estos puntos, después los temas más específicos se acomodan mucho mejor.

#### A. Dataset, observaciones, variables, features y target
La unidad básica de toda la materia es el dataset.

- Una **observación** es un caso: una casa, un viaje, un cliente, una canción.
- Una **variable** es una propiedad medida de ese caso.
- Una **feature** es una variable usada como entrada del modelo.
- El **target** es la variable que queremos predecir.

Esto parece terminología menor, pero en realidad organiza todo el pensamiento posterior.

#### B. Tipo de variable y tipo de problema
Hay una relación central entre la naturaleza de la variable objetivo y el tipo de tarea.

- Target categórico → clasificación.
- Target numérico continuo → regresión.
- Sin target → aprendizaje no supervisado, como clustering.

Además, las variables explicativas también importan porque condicionan el preprocesamiento. No se trata igual una edad que un país, ni un texto que una categoría con 200 niveles.

#### C. Representación
Buena parte de la materia puede leerse como un problema de representación.

Un algoritmo “ve” números, no conceptos humanos. Por eso hay que transformar:

- texto en vectores,
- categorías en codificaciones,
- variables desbalanceadas en escalas más útiles,
- columnas existentes en nuevas features más informativas.

#### D. Generalización
La gran pregunta del curso es: **¿el patrón aprendido sirve fuera de la muestra?**

De ahí salen casi todos los temas de evaluación y regularización.

#### E. Complejidad del modelo
Un modelo demasiado simple puede no captar la estructura del problema. Uno demasiado complejo puede perseguir ruido.

De acá nace la tensión entre:

- bias y varianza,
- underfitting y overfitting,
- interpretabilidad y performance,
- simplicidad y capacidad predictiva.

#### F. Elección de métrica según costo del error
La materia insiste mucho, y con razón, en que no existe una métrica “mejor” en abstracto.

Lo correcto depende de qué error duele más.

- Si me preocupan los falsos positivos, miro más precision.
- Si me preocupan los falsos negativos, miro más recall.
- Si quiero un balance entre ambas, F1.
- Si estoy en regresión, necesito métricas de error como MAE, MSE o RMSE.

#### G. Interpretabilidad
Especialmente con árboles, Random Forest, reducción de dimensionalidad y clustering, la materia pide explicar lo que el modelo está haciendo, no solo reportar un score.

---

### 1.3 Dependencias conceptuales

Estas relaciones son claves porque muestran qué conviene entender primero.

- Para entender **clasificación, regresión y clustering**, primero hay que entender **tipos de variable y objetivo del problema**.
- Para entender **métricas**, primero hay que entender **qué significa equivocarse en cada tarea**.
- Para entender **precision y recall**, antes hay que entender **matriz de confusión**.
- Para entender **overfitting y underfitting**, antes hay que entender **generalización** y la diferencia entre entrenamiento y test.
- Para entender **bias-varianza**, antes hay que entender **complejidad del modelo**.
- Para entender **árboles**, antes hay que entender **cómo se decide una partición útil**.
- Para entender **poda**, antes hay que entender **por qué un árbol puede sobreajustar**.
- Para entender **Random Forest**, antes hay que entender **árboles individuales**, bootstrap y reducción de varianza.
- Para entender **bagging y boosting**, antes hay que entender **por qué combinar modelos puede mejorar resultados**.
- Para entender **XGBoost**, antes hay que entender **boosting**.
- Para entender **K-Means**, antes hay que entender **distancia y similitud entre observaciones**.
- Para entender **silhouette** y **elbow**, antes hay que entender **qué significa que un clustering sea “bueno”**.
- Para entender **PCA**, antes hay que entender **varianza, colinealidad y alta dimensión**.
- Para entender **ISOMAP** y **t-SNE**, antes hay que entender que distintas técnicas preservan distintas propiedades geométricas.
- Para entender **perceptrón multicapa**, antes hay que entender la limitación del **perceptrón simple**.
- Para entender **backpropagation**, antes hay que entender que una red aprende ajustando pesos para reducir una función de pérdida.
- Para entender **NLP aplicado**, antes hay que entender que el texto debe pasar de lenguaje natural a representación numérica.

---

### 1.4 Temas estructurantes vs temas más aislados

#### Temas estructurantes
Son los que reaparecen en teoría, TP y examen, y además sirven para conectar muchos otros temas.

- tipo de problema,
- EDA,
- preprocesamiento,
- métricas,
- train/test y validación cruzada,
- overfitting / underfitting / bias-varianza,
- árboles y Random Forest,
- reducción de dimensionalidad,
- ensambles,
- interpretación de resultados.

#### Temas importantes pero más locales
Son relevantes, pero dependen más de un bloque puntual o del TP específico.

- perceptrón simple,
- SOM,
- detalles de funciones de activación,
- Kaggle como contexto de evaluación,
- algunas herramientas puntuales de Pandas o WEKA.

#### Lagunas o desprolijidades detectadas entre materiales
No todos los materiales tienen la misma profundidad.

- Los parciales privilegian preguntas conceptuales cortas, comparativas y aplicadas.
- Los TP piden más justificación de pipeline, más detalle operativo y más integración.
- Algunas respuestas de resolución están simplificadas, así que conviene tomarlas como orientación de foco evaluativo y no como desarrollo teórico exhaustivo.
- Algunos temas aparecen más abiertos en finales o parciales recientes que en la guía tradicional, especialmente ensambles, redes y problemas sobre texto.

La decisión editorial de este resumen será priorizar lo que está mejor sostenido por la combinación entre contenidos generales, TP y exámenes.

---

## 2. TEMARIO REALMENTE IMPORTANTE PARA APROBAR

### 2.1 Ranking de temas por importancia real

#### Núcleo de examen
Son los temas que más conviene dominar con seguridad alta.

1. **Métricas de evaluación**
   - precision, recall, accuracy, F1, matriz de confusión;
   - MAE, MSE, RMSE;
   - elección de métricas según contexto.

2. **Overfitting, underfitting, bias y varianza**
   - detección,
   - causas,
   - formas de mitigación.

3. **Preprocesamiento e ingeniería de características**
   - faltantes,
   - outliers,
   - encoding,
   - escalado,
   - transformaciones,
   - creación de variables,
   - ejemplos concretos de TP.

4. **Árboles de decisión y Random Forest**
   - entropía,
   - ganancia de información,
   - Gini,
   - poda,
   - atributos numéricos,
   - importancia de variables,
   - comparación árbol vs RF.

5. **Diferencia entre clasificación, regresión y clustering**
   - con ejemplos,
   - con algoritmos posibles,
   - con métrica adecuada para cada caso.

6. **Reducción de dimensionalidad**
   - para qué sirve,
   - scree plot,
   - PCA,
   - ISOMAP,
   - t-SNE,
   - criterio de elección.

7. **Ensambles**
   - bagging vs boosting,
   - stacking vs voting,
   - homogéneos vs híbridos,
   - conexión con Random Forest y XGBoost.

#### Importante pero secundario
Conviene saberlo bien, aunque suele aparecer con menor densidad o menor detalle.

- K-Means,
- Hopkins,
- elbow,
- silhouette,
- interpretación de clusters,
- regresión lineal y logística a nivel conceptual,
- one-hot con alta cardinalidad y alternativas,
- activaciones en redes,
- optimizadores,
- early stopping,
- SOM.

#### Complementario
Puede entrar, sobre todo en trabajos prácticos, finales o preguntas abiertas, pero no parece ser el corazón recurrente del parcial.

- detalles operativos de Pandas,
- WEKA,
- SentiWordNet,
- pormenores de herramientas de competencia Kaggle,
- arquitecturas más avanzadas de deep learning si no fueron trabajadas directamente por el grupo.

---

### 2.2 Qué forma tienen las preguntas de examen

#### Patrón 1 — Comparaciones entre conceptos parecidos
Esto aparece mucho porque revela comprensión real.

Ejemplos típicos:

- clasificación vs regresión vs clustering,
- precision vs recall,
- bagging vs boosting,
- stacking vs voting,
- homogéneo vs híbrido,
- overfitting vs underfitting,
- outlier univariado vs multivariado,
- PCA vs t-SNE vs ISOMAP.

Acá no alcanza con una definición suelta. Hay que marcar:

- qué tienen en común,
- en qué se distinguen,
- qué problema resuelve cada uno,
- cuándo usar uno y no otro.

#### Patrón 2 — Preguntas de criterio
Son muy importantes porque obligan a justificar.

Ejemplos:

- qué métrica usarías y por qué,
- qué técnica de reducción elegirías y por qué,
- qué harías con una variable categórica de 200 categorías,
- qué técnica usarías para evitar overfitting,
- cómo tratarías faltantes u outliers en un caso real.

#### Patrón 3 — Preguntas conectadas con TP
No preguntan el TP como relato administrativo, sino como evidencia de que el alumno sabe explicar decisiones metodológicas.

Por eso conviene poder contar:

- qué se hizo,
- por qué se hizo,
- qué alternativas había,
- cómo impactó en la evaluación,
- qué resultados produjo.

#### Patrón 4 — Cálculo e interpretación de métricas
Aparecen matrices de confusión y pedidos de cálculo o interpretación.

Acá suelen evaluar dos cosas a la vez:

- manejo formal de las fórmulas,
- comprensión del significado de cada número.

#### Patrón 5 — Respuestas breves pero técnicamente precisas
No suelen ser desarrollos matemáticos largos. El desafío está más en responder con exactitud conceptual y ejemplos bien elegidos.

---

### 2.3 Errores frecuentes y trampas conceptuales

#### Error 1 — Elegir métrica por costumbre
Muchos alumnos contestan accuracy porque es la más conocida. Eso suele ser superficial.

Si las clases están desbalanceadas, accuracy puede ser engañosa. La materia claramente espera que el alumno piense el costo del error.

#### Error 2 — Confundir precision con recall
La forma más segura de no confundirse es esta:

- **precision** mira los positivos predichos por el modelo y pregunta cuántos eran correctos;
- **recall** mira los positivos reales y pregunta cuántos logró capturar el modelo.

#### Error 3 — Decir “overfitting es cuando anda mal”
Eso es insuficiente. Hay que decir que el modelo ajusta demasiado el entrenamiento, pierde generalización y suele mostrar brecha entre entrenamiento y test.

#### Error 4 — Tratar el preprocesamiento como receta automática
No hay una receta universal. El valor del curso está en justificar decisiones según el problema.

#### Error 5 — Pensar que reducir dimensión siempre es para “que haya menos columnas”
Eso es parcialmente cierto, pero pobre. También sirve para:

- reducir ruido,
- mitigar colinealidad,
- visualizar,
- facilitar aprendizaje,
- evitar sobreajuste.

#### Error 6 — Memorizar nombres de técnicas sin entender qué preservan
Esto es especialmente peligroso con PCA, ISOMAP y t-SNE.

- PCA prioriza varianza explicada.
- ISOMAP busca respetar la geometría de una variedad.
- t-SNE favorece preservar vecindades y separación visual de clusters.

#### Error 7 — Describir Random Forest como “un árbol mejor”
No es un árbol un poco más grande, sino un ensamble de muchos árboles con bootstrap y aleatoriedad en features para reducir varianza.

#### Error 8 — Suponer que más complejidad siempre significa mejor modelo
La materia insiste bastante en que más complejidad puede llevar a peor generalización.

#### Error 9 — En texto, olvidar que el algoritmo no entiende palabras directamente
Antes de usar Bayes, Random Forest o redes sobre texto, hay que convertir documentos a vectores.

---

## 3. ESTRUCTURA PROPUESTA DEL RESUMEN

### 3.1 Índice ideal

1. Cómo pensar un problema de ciencia de datos.
2. Cómo leer un dataset antes de modelar.
3. Cómo preparar los datos para que el modelo aprenda bien.
4. Cómo evaluar modelos sin engañarse.
5. Modelos supervisados: ideas centrales y criterios de elección.
6. Árboles y ensambles: el bloque más preguntable.
7. Aprendizaje no supervisado: clustering.
8. Reducción de dimensionalidad: qué preserva cada técnica.
9. Redes neuronales: intuición, límites y regularización.
10. Texto como dato: NLP aplicado en la materia.
11. Estrategia práctica para rendir y para defender TPs.

---

### 3.2 Por qué este orden y no el orden histórico de clase

El mejor orden pedagógico no es “tema 1, tema 2, tema 3” según el calendario, sino el orden en que una mente principiante puede construir comprensión sólida.

Primero conviene responder:

- qué problema tengo,
- qué datos tengo,
- cómo los miro,
- cómo los limpio,
- cómo sé si el modelo funciona.

Recién después tiene sentido entrar en familias de algoritmos.

Esto evita un error muy común: aprender nombres de modelos sin haber entendido todavía qué significa evaluar, generalizar o representar bien un dataset.

---

### 3.3 Estrategia didáctica de cada sección

#### Sección 1 — Cómo pensar un problema
- **Problema que introduce:** el alumno ve datasets pero no sabe qué está tratando de resolver.
- **Intuición inicial:** toda tarea de ML depende de qué quiero predecir y qué información tengo.
- **Formalización posterior:** clasificación, regresión, clustering, supervisado/no supervisado.
- **Ejemplo útil:** spam, precio de vivienda, segmentación de canciones.
- **Confusión a evitar:** confundir variable objetivo con variables de entrada.

#### Sección 2 — Leer el dataset
- **Problema:** modelar sin conocer los datos.
- **Intuición:** primero hay que mirar el terreno antes de construir encima.
- **Formalización:** análisis univariado, multivariado, distribuciones, frecuencias, correlaciones.
- **Ejemplo:** taxis, Titanic, ventas.
- **Confusión a evitar:** creer que EDA es solo hacer gráficos lindos.

#### Sección 3 — Preprocesamiento
- **Problema:** los datos reales vienen incompletos, ruidosos o en formatos inapropiados.
- **Intuición:** el modelo hereda la calidad de la representación.
- **Formalización:** imputación, outliers, encoding, normalización, transformaciones, feature engineering.
- **Ejemplo:** variable con 200 categorías, precios sesgados, texto a vectores.
- **Confusión a evitar:** aplicar técnicas por costumbre sin justificar.

#### Sección 4 — Evaluación
- **Problema:** creer que un score aislado alcanza.
- **Intuición:** entrenar bien no es lo mismo que generalizar bien.
- **Formalización:** split, CV, matriz de confusión, métricas, bias-varianza.
- **Ejemplo:** enfermedad, fraude, precio de alquiler.
- **Confusión a evitar:** usar accuracy siempre.

#### Sección 5 — Modelos supervisados
- **Problema:** elegir modelo sin criterio.
- **Intuición:** cada algoritmo mira el problema de una manera distinta.
- **Formalización:** regresión lineal, logística, árboles, KNN, SVM, XGBoost.
- **Ejemplo:** comparar interpretabilidad y performance.
- **Confusión a evitar:** pensar que existe un modelo universalmente mejor.

#### Sección 6 — Árboles y ensambles
- **Problema:** entender el bloque más evaluado.
- **Intuición:** un árbol decide haciendo preguntas sucesivas; un ensamble combina muchos modelos para mejorar estabilidad o potencia.
- **Formalización:** entropía, Gini, poda, bagging, boosting, stacking, voting.
- **Ejemplo:** lluvia en Australia, Random Forest, XGBoost.
- **Confusión a evitar:** mezclar bagging con boosting o pensar que Random Forest y XGBoost son casi lo mismo.

#### Sección 7 — Clustering
- **Problema:** agrupar sin etiquetas.
- **Intuición:** encontrar parecidos útiles cuando nadie nos dijo la respuesta correcta.
- **Formalización:** K-Means, Hopkins, elbow, silhouette.
- **Ejemplo:** canciones de Spotify.
- **Confusión a evitar:** creer que un cluster “descubre verdades naturales” sin interpretación humana.

#### Sección 8 — Reducción de dimensionalidad
- **Problema:** demasiadas variables, ruido o dificultad para visualizar.
- **Intuición:** resumir sin destruir la estructura importante.
- **Formalización:** PCA, scree plot, t-SNE, ISOMAP.
- **Ejemplo:** elegir técnica según objetivo.
- **Confusión a evitar:** usarlas como sinónimos.

#### Sección 9 — Redes neuronales
- **Problema:** entender cómo una red supera la limitación lineal del perceptrón.
- **Intuición:** una red aprende transformaciones sucesivas de los datos.
- **Formalización:** perceptrón, MLP, backpropagation, optimizadores, regularización.
- **Ejemplo:** XOR, clasificación, regresión sobre texto.
- **Confusión a evitar:** usar palabras como backprop u optimizador sin explicar su función real.

#### Sección 10 — NLP aplicado
- **Problema:** el texto no es numérico.
- **Intuición:** antes de predecir con texto, hay que convertir lenguaje en representación matemática.
- **Formalización:** bag of words, TF-IDF, Bayes Naïve, regresión sobre texto, ensambles, redes.
- **Ejemplo:** sentimiento y story points.
- **Confusión a evitar:** creer que el modelo “lee” comprensión semántica humana.

---

## 4. RESUMEN FINAL COMPLETO

### 4.1 Cómo pensar correctamente un problema de ciencia de datos

La primera habilidad que esta materia quiere construir no es programar, ni graficar, ni usar un algoritmo famoso. Es algo más básico y más importante: **aprender a mirar una situación y reconocer qué tipo de problema es**.

Eso importa porque en ciencia de datos casi todas las decisiones buenas nacen de una buena formulación inicial. Si formulás mal el problema, después todo lo demás se encadena mal: el modelo, la métrica, el preprocesamiento y la interpretación.

#### El punto de partida: de qué están hechos los problemas
Un dataset puede pensarse como una tabla donde cada fila representa un caso y cada columna una propiedad de ese caso.

Ejemplos:

- en AirBnB, una fila puede ser un alojamiento;
- en el TP de clima, una fila puede ser un día en una estación meteorológica;
- en Spotify, una fila puede ser una canción;
- en story points, una fila puede ser una user story.

Ahora bien: entre todas las columnas, hay una que a veces queremos predecir. Esa columna es el **target**. Las demás suelen funcionar como **features**.

#### Clasificación, regresión y clustering: la gran tríada
La pregunta clave es: **¿qué forma tiene la respuesta que busco?**

##### Clasificación
Hay clasificación cuando el objetivo es elegir entre categorías definidas de antemano.

Ejemplos:

- lloverá / no lloverá,
- spam / no spam,
- enfermo / sano,
- sentimiento positivo / negativo.

La intuición correcta es imaginar que el modelo tiene que decidir “en qué casillero cae este caso”.

##### Regresión
Hay regresión cuando el objetivo es predecir un valor numérico continuo.

Ejemplos:

- precio de alquiler,
- story points,
- duración de un viaje,
- consumo esperado.

Acá el modelo no tiene que elegir una etiqueta, sino estimar un número.

##### Clustering
Hay clustering cuando no hay una respuesta correcta dada de antemano y lo que se busca es encontrar agrupamientos por similitud.

Ejemplo:

- agrupar canciones según características acústicas.

La diferencia profunda con clasificación es que en clasificación los grupos ya están definidos desde el principio; en clustering, los grupos se construyen a partir de la estructura de los datos.

#### Por qué esta distinción es tan importante
Porque cambia todo.

- Cambia la familia de algoritmos razonables.
- Cambia la forma de evaluar.
- Cambia el tipo de errores que importan.
- Cambia incluso cómo comunicar el resultado.

Un error muy común en examen es contestar esto de manera demasiado corta. No alcanza con decir “clasificación predice clases y regresión números”. Conviene agregar siempre:

- un ejemplo,
- una métrica típica,
- y una consecuencia práctica.

#### Supervisado y no supervisado
La clasificación y la regresión son problemas de **aprendizaje supervisado** porque durante el entrenamiento el modelo ve ejemplos con la respuesta correcta.

El clustering, en cambio, es **no supervisado**, porque el dataset no trae una etiqueta objetivo que diga cuál es la respuesta esperada.

Esta distinción es importante porque explica por qué en clustering la evaluación es distinta y la interpretación humana tiene más peso.

**Cómo podría aparecer en examen:** comparación entre clasificación, regresión y agrupamiento con ejemplo y algoritmo posible.

**Error típico a evitar:** mezclar “variable cualitativa” con “modelo de clasificación” como si fueran exactamente lo mismo. La variable objetivo suele ser categórica en clasificación, pero las features pueden ser mixtas.

---

### 4.2 Leer un dataset: por qué el análisis exploratorio no es un trámite

Muchos estudiantes quieren llegar rápido al modelo. La materia, en cambio, enseña algo más maduro: antes de modelar, hay que **entender el material con el que se trabaja**.

Pensalo así: entrenar un modelo sin mirar el dataset es como querer diagnosticar a un paciente sin revisar síntomas, estudios ni antecedentes.

#### Qué busca realmente el EDA
El análisis exploratorio de datos busca responder preguntas como:

- ¿qué variables hay?
- ¿qué representa cada una?
- ¿son cuantitativas o cualitativas?
- ¿cómo se distribuyen?
- ¿hay valores imposibles o sospechosos?
- ¿hay correlaciones relevantes?
- ¿qué patrones llaman la atención?

#### Análisis univariado: entender una variable por vez
Este es el primer paso porque construye intuición básica.

Para variables cuantitativas suelen mirarse:

- media,
- mediana,
- moda,
- dispersión,
- histogramas,
- boxplots.

La idea no es coleccionar estadísticas, sino aprender a “leer el carácter” de la variable.

Por ejemplo, si una distribución de precios tiene una cola muy larga hacia la derecha, uno entiende que hay pocas observaciones muy caras que estiran la escala. Eso ya anticipa posibles transformaciones, como logaritmos.

Para variables cualitativas suelen mirarse:

- categorías posibles,
- frecuencias,
- proporciones,
- gráficos de barras.

Eso sirve para detectar categorías dominantes, rarezas o clases desbalanceadas.

#### Análisis multivariado: cuando el sentido aparece en la relación entre variables
Muchas veces el dato interesante no está en una columna aislada, sino en cómo se combinan varias.

Acá aparecen:

- correlaciones,
- scatter plots,
- tablas cruzadas,
- segmentaciones por grupos,
- comparaciones condicionadas.

Por ejemplo, no solo interesa saber cuánto dura un viaje, sino cómo cambia según horario, día o zona. No solo interesa el precio de un alojamiento, sino cómo se relaciona con ubicación, tamaño o tipo de propiedad.

#### Correlación: qué dice y qué no dice
La correlación lineal de Pearson aparece en materiales y recuperatorios porque es útil, pero también porque puede confundir si se usa superficialmente.

La intuición es esta:

- correlación positiva fuerte: cuando una variable sube, la otra tiende a subir;
- correlación negativa fuerte: cuando una sube, la otra tiende a bajar;
- correlación baja: no hay evidencia de relación lineal fuerte.

Lo importante es remarcar “lineal”. Puede existir relación no lineal aunque Pearson sea bajo.

#### Visualizar bien también es pensar bien
La guía práctica insiste bastante en los gráficos, y eso no es decorativo.

Un buen gráfico ayuda a ver:

- distribuciones,
- asimetrías,
- anomalías,
- relaciones,
- segmentos,
- resultados del preprocesamiento.

En TP y examen conviene recordar que los gráficos deben poder leerse y justificar una idea, no ser solo imágenes insertadas.

**Cómo podría aparecer en examen:** interpretación de correlaciones, análisis de distribuciones, relación entre tipo de variable y análisis apropiado.

**Error típico a evitar:** creer que EDA es una lista de gráficos estándar. El valor está en la lectura que se hace de ellos.

---

### 4.3 Preprocesamiento: cómo convertir datos reales en datos modelables

Esta es una de las partes más formativas de la materia porque obliga a salir del mundo ideal. Los datos reales rara vez vienen limpios, completos y cómodos.

La idea profunda del preprocesamiento es que **un algoritmo aprende sobre la representación que le damos del problema**. Si la representación es mala, el aprendizaje también lo será.

#### Datos faltantes: no son solo huecos, son decisiones
Cuando faltan valores, el primer impulso suele ser “rellenarlos” o “borrar filas”. Pero la pregunta correcta es otra: **¿por qué faltan y qué costo tiene cada decisión?**

Opciones frecuentes:

- eliminar registros,
- imputar con media, mediana, moda,
- imputar con criterios por grupo,
- crear indicadores de ausencia,
- descartar variables con demasiada pérdida.

La mejor decisión depende del contexto. Si elimino masivamente, quizá pierda información. Si imputo mal, quizá introduzca un patrón artificial.

En examen suma mucho no solo nombrar técnicas, sino explicar el criterio de elección.

#### Outliers: por qué no siempre hay que borrarlos
Un outlier es una observación muy alejada del resto. Pero esa definición, sola, es insuficiente. Lo importante es entender que puede ser:

- un error,
- una rareza legítima,
- una anomalía interesante,
- una señal importante del dominio.

Por eso la materia insiste en inspeccionarlos “cuidadosamente”.

##### Univariados
Se detectan mirando una variable a la vez.

Herramientas típicas:

- IQR,
- z-score,
- z-score modificado,
- boxplots.

##### Multivariados
Aparecen cuando una observación no parece rara en cada variable por separado, pero sí en la combinación.

Herramientas típicas:

- distancia de Mahalanobis,
- LOF,
- Isolation Forest,
- clustering.

La intuición útil es esta: una manzana naranja quizá no parezca rara si mirás solo color o solo forma, pero sí al mirar ambas cosas juntas.

#### Encoding: cuando las categorías deben volverse números
Muchos algoritmos necesitan entradas numéricas. Entonces las variables categóricas deben codificarse.

El caso clásico es **one-hot encoding**, que crea una columna por categoría.

Esto funciona bien cuando hay pocas categorías, pero trae problemas cuando la cardinalidad es muy alta. Los parciales recientes lo remarcan explícitamente con el ejemplo de una variable con 200 categorías.

¿Por qué es problemático?

- explota la dimensionalidad,
- genera mucha sparsity,
- aumenta costo computacional,
- puede empeorar interpretabilidad,
- puede favorecer overfitting.

Alternativas razonables:

- agrupar categorías raras en “otros”,
- usar otros esquemas de encoding,
- embeddings en escenarios más complejos.

#### Escalado y normalización: por qué algunas variables necesitan ponerse “en la misma escala”
No todos los algoritmos son igual de sensibles a la escala. Si una variable está en miles y otra entre 0 y 1, ciertos métodos pueden quedar dominados por la primera.

La intuición es simple: algunas técnicas “miden distancia” o dependen de magnitudes relativas. En esos casos conviene escalar.

#### Transformaciones
Si una variable tiene fuerte asimetría positiva, una transformación logarítmica puede hacerla más estable y manejable. Esto aparece claramente en parciales.

La idea no es “embellecer” la variable, sino facilitar:

- modelado,
- interpretación,
- estabilidad,
- reducción de influencia de extremos.

#### Feature engineering
Este tema es central en TP. Consiste en construir nuevas variables que expresen mejor el problema.

Ejemplos conceptuales:

- extraer día de la semana desde una fecha,
- construir duración desde hora de inicio y fin,
- resumir texto con representación vectorial,
- combinar variables existentes.

Acá la materia premia criterio. Una buena feature puede valer más que probar diez modelos sin sentido.

**Cómo podría aparecer en examen:** elegir una técnica usada en TP y describir cómo se implementó y por qué.

**Error típico a evitar:** convertir el preprocesamiento en una lista de herramientas desconectadas del dominio.

---

### 4.4 Evaluar modelos: cómo no mentirse con la performance

Esta sección es probablemente la más importante para entender la materia en serio. Porque muchas veces entrenar un modelo es relativamente fácil; lo difícil es saber si realmente sirve.

#### Entrenamiento y test: la idea básica de generalización
Un modelo aprende sobre un conjunto de entrenamiento. Pero si lo evaluamos solo ahí, estamos haciéndole preguntas sobre datos que ya vio.

Por eso se separa un conjunto de test. El objetivo es estimar cómo rendirá frente a casos nuevos.

La intuición profunda es esta: **el valor de un modelo no está en recordar el pasado, sino en responder bien en datos no vistos**.

#### Validación cruzada
Cuando quiero ajustar hiperparámetros o tener una evaluación más robusta, uso cross validation.

En lugar de depender de una única partición, entreno y valido varias veces en distintos folds. Eso reduce la arbitrariedad de un split accidentalmente favorable o desfavorable.

En TP esto aparece como parte del proceso serio de optimización, no como detalle técnico menor.

#### Matriz de confusión: la radiografía de la clasificación
La matriz de confusión organiza las predicciones en:

- verdaderos positivos,
- falsos positivos,
- verdaderos negativos,
- falsos negativos.

Su valor no está solo en permitir fórmulas, sino en mostrar **qué tipo de error está cometiendo el modelo**.

Eso es crucial porque no todos los errores cuestan lo mismo.

#### Accuracy: útil, pero no siempre suficiente
Accuracy es la proporción total de aciertos.

Es intuitiva y cómoda, pero puede ser engañosa si las clases están desbalanceadas. Un modelo puede acertar mucho simplemente prediciendo siempre la clase mayoritaria.

#### Precision y recall: dos lentes distintos sobre el error
Estos dos conceptos aparecen reiteradamente porque son fáciles de confundir y muy importantes.

##### Precision
Responde: **de todo lo que el modelo marcó como positivo, cuánto era realmente positivo**.

Sirve cuando el costo de “alarmar de más” es alto.

Ejemplo intuitivo: si un filtro marca muchos correos normales como spam, la precisión es baja.

##### Recall
Responde: **de todos los positivos reales, cuántos detectó el modelo**.

Sirve cuando el costo de “dejar pasar un positivo real” es alto.

Ejemplo intuitivo: en fraude o enfermedad, perder casos reales puede ser muy grave.

##### F1
Es una forma de equilibrar precision y recall cuando ambas importan.

No es magia: simplemente resume un compromiso entre dos necesidades que a menudo compiten.

#### Métricas de regresión
En regresión no tiene sentido usar precision o recall porque no estamos etiquetando positivos y negativos.

Acá lo natural es medir error.

- **MAE**: error absoluto medio. Fácil de interpretar.
- **MSE**: castiga más los errores grandes porque los eleva al cuadrado.
- **RMSE**: vuelve a la escala original tras haber cuadratizado.

La intuición importante es que no todas penalizan igual los errores extremos.

#### Overfitting y underfitting
Estos conceptos son centrales porque conectan modelo, evaluación y complejidad.

##### Overfitting
El modelo aprende demasiado el entrenamiento, incluso ruido o peculiaridades accidentales.

Señales típicas:

- entrenamiento muy bueno,
- test bastante peor,
- alta sensibilidad,
- demasiada complejidad relativa al problema.

Cómo mitigarlo:

- regularización,
- poda,
- validación cruzada,
- reducción de complejidad,
- early stopping,
- más datos en algunos casos,
- ensambles bien regularizados.

##### Underfitting
El modelo es demasiado pobre para capturar la estructura relevante.

Señales típicas:

- errores altos tanto en entrenamiento como en validación,
- insuficiente capacidad expresiva,
- exceso de simplificación.

Cómo mitigarlo:

- modelos más ricos,
- mejores features,
- menos regularización,
- mejor representación del problema.

#### Bias y varianza: la idea detrás de todo
Conviene entenderlo intuitivamente antes que recitarlo.

- **Bias alto**: el modelo es demasiado rígido, simplifica demasiado.
- **Varianza alta**: el modelo reacciona demasiado a detalles específicos del entrenamiento.

La tensión entre ambos explica por qué encontrar un buen modelo no es solo “hacerlo más potente”.

**Cómo podría aparecer en examen:** definición comparativa, detección por comportamiento train/test, propuesta de mitigación.

**Error típico a evitar:** describir overfitting y underfitting sin hablar de generalización ni complejidad.

---

### 4.5 Modelos supervisados: qué idea aporta cada familia

La materia menciona varios modelos, pero no para que el alumno memorice listas. Lo importante es entender la intuición de cada uno.

#### Regresión lineal
La idea es modelar una relación aproximadamente lineal entre features y target.

Su valor pedagógico es enorme porque obliga a pensar en interpretabilidad, coeficientes y efecto de las variables.

No siempre será el mejor modelo predictivo, pero sí suele ser un buen punto de comparación.

#### Regresión logística
Aunque tenga “regresión” en el nombre, se usa para clasificación binaria.

La intuición es que estima una probabilidad de pertenecer a una clase, y luego esa probabilidad puede transformarse en decisión.

Es especialmente útil para entender clasificación probabilística y fronteras relativamente simples.

#### KNN y SVM
En los materiales aparecen más como parte del repertorio conceptual que como centro absoluto del examen.

- KNN clasifica mirando vecinos cercanos.
- SVM busca una frontera separadora con buen margen.

Sirven para reforzar la idea de que distintos algoritmos representan el problema de maneras distintas.

#### XGBoost
Es importante no tratarlo como una caja negra de moda. Conceptualmente, importa porque muestra el poder de boosting bien regularizado.

Suele ofrecer muy buena performance, pero a costa de mayor complejidad y menor transparencia que un árbol simple.

#### Criterio general de elección
Cuando te preguntan qué modelo elegirías, conviene pensar en varios ejes:

- tipo de problema,
- tamaño y naturaleza del dataset,
- tipo de variables,
- necesidad de interpretabilidad,
- sensibilidad a escala,
- tolerancia a overfitting,
- costo computacional,
- métrica objetivo.

**Cómo podría aparecer en examen:** justificar un modelo a elección o comparar modelos por trade-offs.

**Error típico a evitar:** responder con “uso XGBoost porque suele dar mejor resultado”. En esta materia se espera criterio, no slogans.

---

### 4.6 Árboles de decisión: aprender a decidir haciendo preguntas sucesivas

Este es uno de los bloques más importantes de toda la materia y probablemente el más rentable para estudiar bien.

#### La intuición del árbol
Un árbol de decisión clasifica o predice partiendo el espacio de datos mediante preguntas sucesivas.

Por ejemplo:

- ¿la humedad es mayor que cierto umbral?
- ¿la presión está por debajo de otro umbral?
- ¿la categoría climática es tal o cual?

Cada pregunta divide los datos y trata de dejar juntos casos cada vez más parecidos respecto del target.

La gran virtud del árbol es que su razonamiento se puede contar casi como reglas “si-entonces”. Eso lo vuelve muy interpretable.

#### Qué significa una buena división
No cualquier corte sirve. Una buena división es la que deja nodos más “puros”, es decir, grupos con mayor homogeneidad respecto de la clase.

#### Entropía y ganancia de información
La entropía mide desorden o mezcla de clases. Si un nodo tiene observaciones muy mezcladas, la entropía es alta. Si casi todas son de la misma clase, la entropía es baja.

La **ganancia de información** evalúa cuánto reduce ese desorden una partición.

Intuición:

- antes de preguntar, el nodo está confuso;
- después de preguntar, idealmente queda más ordenado;
- la ganancia mide cuánto ayudó esa pregunta.

#### Impureza de Gini
La lógica es parecida: medir cuán mezclado está un nodo.

Un nodo puro contiene una sola clase. Un nodo impuro mezcla varias.

En examen conviene poder explicar esto con palabras sencillas, no solo con la fórmula.

#### Atributos numéricos en árboles
Un punto bastante preguntado es cómo maneja el árbol variables continuas.

La intuición es simple: el algoritmo busca un umbral.

Por ejemplo:

- temperatura < 18.5,
- humedad > 70,
- precio < cierto valor.

Es decir, convierte una variable continua en una decisión binaria local porque eso permite particionar el espacio.

#### Poda
Un árbol muy grande puede capturar detalles excesivamente particulares del conjunto de entrenamiento. Ahí aparece la poda.

La poda elimina ramas que aportan complejidad pero no mejoran realmente la generalización.

La idea profunda es elegantísima: **no todo detalle aprendido vale la pena conservarlo**.

#### Limitaciones del árbol simple
Aunque es interpretativo y útil, puede ser inestable. Pequeños cambios en los datos pueden producir árboles distintos. Además, si se lo deja crecer mucho, sobreajusta con facilidad.

Esto prepara el terreno para Random Forest.

**Cómo podría aparecer en examen:** entropía, Gini, poda, reglas iniciales del árbol, manejo de numéricos.

**Error típico a evitar:** definir Gini o entropía sin explicar que buscan particiones más puras.

---

### 4.7 Random Forest y ensambles: por qué combinar modelos puede funcionar mejor

#### La intuición general de un ensamble
Un ensamble parte de una idea muy humana: si varias decisiones imperfectas pero informadas se combinan bien, el resultado puede ser más estable y preciso que cualquiera de ellas por separado.

No siempre mejora todo, pero muchas veces reduce debilidades individuales.

#### Bagging
Bagging significa entrenar varios modelos sobre muestras bootstrap y luego combinarlos.

La intuición más útil es esta: en vez de confiar en un solo árbol, construyo muchos árboles parecidos pero no idénticos, y después los promedio o los hago votar.

Eso reduce varianza. Es decir, el sistema total se vuelve menos sensible a accidentes del conjunto de entrenamiento.

#### Random Forest
Random Forest es el ejemplo más emblemático de bagging en la materia.

Consiste en:

- muchos árboles,
- entrenados sobre muestras bootstrap,
- con selección aleatoria de features en cada split.

¿Por qué también se aleatorizan las features? Porque si siempre se dejara elegir entre todas, muchos árboles terminarían pareciéndose demasiado. La aleatoriedad fuerza diversidad útil.

Ventajas:

- mejor generalización que un árbol aislado en muchos casos,
- menos varianza,
- posibilidad de medir importancia de variables,
- buen rendimiento práctico.

Costo:

- menor interpretabilidad global que un árbol único.

#### Boosting
Si bagging trabaja más bien “en paralelo”, boosting trabaja “en secuencia”.

La idea es que cada nuevo modelo se concentre en corregir errores de los anteriores.

Eso hace que el ensamble vaya refinando progresivamente su capacidad predictiva.

#### XGBoost
XGBoost es una realización muy potente de boosting basado en gradiente.

Conceptualmente importa porque muestra un ensamble que no solo combina modelos, sino que **aprende sobre los residuos del ensamble previo**.

Además incorpora regularización, learning rate, pruning y otros mecanismos para controlar complejidad.

#### Stacking y voting
Ambos aparecen como ensambles heterogéneos o híbridos en varios materiales.

- **Voting**: varios modelos producen predicciones y se combinan por voto o promedio.
- **Stacking**: las salidas de varios modelos base alimentan un metamodelo que aprende cómo combinarlas.

La diferencia importante es que stacking aprende una combinación; voting aplica una regla fija.

#### Homogéneos vs híbridos
- **Homogéneo**: todos los modelos son del mismo tipo, como en Random Forest.
- **Híbrido**: combina algoritmos distintos, como árbol + SVM + red.

El híbrido puede capturar perspectivas complementarias, pero también complica implementación e interpretación.

**Cómo podría aparecer en examen:** bagging vs boosting, RF vs XGBoost, stacking vs voting, homogéneo vs híbrido.

**Error típico a evitar:** decir que bagging y boosting “hacen varios modelos y listo”. La diferencia está en cómo los construyen y qué tipo de error buscan reducir.

---

### 4.8 Clustering: agrupar sin que nadie nos diga la respuesta correcta

El clustering tiene un atractivo especial porque intenta descubrir estructura sin etiquetas previas.

#### La idea de fondo
En clasificación, el docente ya te dice cuáles son las clases correctas del pasado. En clustering, nadie te da esa respuesta. El desafío es detectar si los datos parecen formar grupos naturalmente.

#### K-Means
K-Means busca particionar los datos en K grupos, asignando cada observación al centroide más cercano y reajustando centroides iterativamente.

La intuición simple es imaginar varios “centros” que se van moviendo hasta quedar en posiciones representativas de grupos de puntos.

#### Un detalle importante: K no cae del cielo
Uno de los problemas más importantes de K-Means es elegir cuántos grupos usar.

Ahí entran criterios como:

- tendencia al clustering,
- elbow method,
- silhouette,
- interpretación sustantiva.

#### Hopkins
La prueba de Hopkins ayuda a evaluar si el dataset realmente parece tener estructura de clusters o si está más cerca de una distribución aleatoria.

Es importante porque a veces uno quiere clusterizar por obligación metodológica, pero los datos no sostienen esa idea.

#### Elbow method
La lógica del elbow es observar cómo cambia la variabilidad intra-cluster al aumentar K.

Al principio agregar clusters mejora mucho. Luego llega un punto donde la mejora marginal cae. Ese “codo” sugiere una cantidad razonable.

No es una verdad automática; es una guía.

#### Silhouette
Silhouette intenta medir qué tan bien queda cada punto dentro de su cluster en comparación con clusters vecinos.

Intuición:

- bueno si un punto está cerca de los suyos y lejos de los otros;
- malo si queda ambiguo entre grupos.

#### Interpretación de clusters
Esta es una parte especialmente importante en TP. Un cluster no vale por existir matemáticamente; vale si puede interpretarse.

Por eso hay que preguntarse:

- ¿qué rasgos dominan cada grupo?
- ¿cómo se diferencia de los demás?
- ¿tiene sentido en el dominio?

**Cómo podría aparecer en examen:** cantidad de clusters elegida, criterio usado, interpretación de grupos.

**Error típico a evitar:** pensar que clustering descubre categorías “reales” sin intervención interpretativa.

---

### 4.9 Reducción de dimensionalidad: reducir sin perder lo importante

Cuando hay muchas variables, pueden aparecer varios problemas a la vez:

- ruido,
- colinealidad,
- dificultad de visualización,
- complejidad computacional,
- mayor riesgo de sobreajuste.

La reducción de dimensionalidad busca aliviar eso.

#### PCA: la lógica de la varianza explicada
PCA construye nuevas variables, llamadas componentes principales, que resumen la mayor cantidad posible de varianza del conjunto.

La intuición útil es imaginar que los datos viven en una nube inclinada en el espacio. PCA intenta encontrar las direcciones más informativas de esa nube.

No conserva necesariamente el significado original de cada columna, pero gana capacidad de síntesis.

#### Scree plot
Sirve para decidir cuántos componentes conservar.

La idea es mirar cuánta varianza explica cada componente y detectar un punto a partir del cual agregar más componentes aporta poco. Ese punto suele leerse como “codo”.

#### t-SNE
t-SNE es muy usado para visualización porque tiende a preservar vecindades locales y mostrar clusters de forma clara.

Por eso en los materiales aparece asociado a la idea de conservar clusters.

La intuición importante es que no está pensado como resumen lineal de varianza, sino como proyección útil para ver estructura local.

#### ISOMAP
ISOMAP aparece cuando sospechamos que los datos viven sobre una variedad no lineal dentro del espacio de alta dimensión.

La intuición clásica es esta: si los datos están sobre una superficie curva, la distancia euclídea directa puede engañar. ISOMAP intenta respetar mejor la geometría intrínseca usando distancias geodésicas aproximadas.

#### No son sinónimos
Este punto es crucial.

- Si quiero varianza explicada y una técnica clásica, pienso en PCA.
- Si quiero visualizar clusters, t-SNE suele ser más natural.
- Si sospecho estructura de variedad, ISOMAP tiene mejor justificación conceptual.

**Cómo podría aparecer en examen:** para qué sirve reducir dimensión, qué elegir según objetivo, uso del scree plot.

**Error típico a evitar:** decir “todas reducen dimensión así que da lo mismo cuál usar”.

---

### 4.10 Redes neuronales: de la limitación lineal al aprendizaje en capas

En muchos cursos las redes neuronales se explican demasiado rápido y quedan como una colección de palabras impresionantes. Acá conviene construir la intuición con calma.

#### Perceptrón simple
El perceptrón simple es una unidad que combina entradas con pesos, suma, aplica una función y decide una salida.

Su importancia histórica y conceptual está en que muestra una idea básica: **aprender significa ajustar pesos para mejorar decisiones**.

#### Su gran limitación
El perceptrón simple solo puede resolver problemas linealmente separables.

La forma más clara de entender esto es imaginar puntos de dos clases en un plano. Si una recta puede separarlos, el perceptrón sirve. Si no, como en XOR, no alcanza.

Esta limitación es la puerta de entrada natural al perceptrón multicapa.

#### Perceptrón multicapa (MLP)
Al agregar capas ocultas y no linealidades, la red deja de estar limitada a fronteras simples.

La intuición es que cada capa transforma la representación de los datos. Lo que al principio parecía difícil de separar puede volverse separable después de varias transformaciones.

#### Backpropagation
Este término suele sonar más misterioso de lo que realmente es. Conceptualmente, backpropagation es el mecanismo para calcular cómo debe cambiar cada peso si quiero reducir el error final.

Usa la regla de la cadena porque el efecto de un peso temprano en la red se propaga a través de muchas operaciones intermedias.

Intuición:

1. la red produce una salida,
2. se compara con la respuesta correcta,
3. se calcula el error,
4. ese error “vuelve hacia atrás” informando cuánto contribuyó cada peso,
5. los pesos se corrigen.

#### Optimizadores
Un optimizador es la regla concreta con la que se actualizan los pesos usando esos gradientes.

Ejemplos frecuentes:

- SGD,
- RMSprop,
- Adam.

No hace falta memorizar demasiados detalles, pero sí entender que todos buscan una actualización eficiente y estable de parámetros.

#### Regularización en redes
Como son modelos potentes, las redes pueden sobreajustar con facilidad.

Por eso aparecen técnicas como:

- regularización L1/L2,
- dropout,
- early stopping.

##### Early stopping
Es conceptualmente muy elegante: se deja de entrenar cuando el rendimiento de validación empieza a empeorar, aunque el entrenamiento siga mejorando. Es una forma de decirle a la red: “hasta acá aprendiste estructura útil; si seguís, empezás a memorizar ruido”.

#### Funciones de activación
En parciales aparecen ejemplos como ReLU y sigmoidea.

Lo importante no es dibujarlas de memoria solamente, sino entender que introducen no linealidad. Sin esa no linealidad, apilar capas lineales no aportaría verdadera riqueza expresiva.

#### SOM
Las Self-Organizing Maps aparecen como redes no supervisadas útiles para visualización y clustering en dos dimensiones.

No suelen ser el núcleo más frecuente del parcial, pero conviene saber para qué sirven y ubicarlas conceptualmente.

**Cómo podría aparecer en examen:** limitación del perceptrón simple, backpropagation, optimizadores, técnicas anti-overfitting, SOM.

**Error típico a evitar:** describir backpropagation como “el algoritmo que entrena redes” sin explicar que calcula gradientes de la pérdida respecto de los pesos.

---

### 4.11 Texto como dato: NLP aplicado en la materia

Trabajar con texto obliga a enfrentar un problema básico pero decisivo: el lenguaje natural no entra directamente a la mayoría de los algoritmos clásicos.

#### El problema de representación
Si yo tengo reseñas de películas o user stories, para un algoritmo esas cadenas no tienen significado operativo inmediato. Antes hay que transformarlas en números.

#### Bag of Words
La idea básica de bolsa de palabras es representar cada documento por el conteo de palabras de un vocabulario.

La intuición es simple: dejo de mirar el texto como secuencia “humana” y lo paso a ver como presencia y frecuencia de términos.

Es un modelo sencillo, pero muy útil pedagógicamente porque muestra cómo un objeto complejo puede volverse vector.

#### TF-IDF
TF-IDF mejora la representación al ponderar palabras según su frecuencia en el documento y su rareza global en el corpus.

La intuición es que no todas las palabras informan igual. Términos muy comunes en todos los documentos aportan poco para discriminar.

#### Bayes Naïve / Multinomial
Estos modelos aparecen en materiales de sentimiento y son importantes porque combinan simplicidad, velocidad y buen rendimiento en muchos problemas de texto.

No hace falta pensar que “entienden semántica”. Funcionan bien porque aprovechan regularidades estadísticas de términos y clases.

#### Lexicones de sentimiento
Con recursos como SentiWordNet se puede puntuar texto usando diccionarios de polaridad.

Es una aproximación distinta de los modelos entrenados: en vez de aprender solo desde ejemplos etiquetados, usa conocimiento léxico preexistente.

#### Story points como problema integrador
El TP2 es muy revelador sobre el espíritu de la materia.

No se pide solo predecir texto con un modelo único. Se pide:

- vectorizar,
- probar varios modelos,
- buscar hiperparámetros,
- comparar,
- ensamblar,
- justificar,
- evaluar con RMSE,
- defender la arquitectura de red elegida.

Es decir: la materia espera que el alumno sepa construir un pipeline real, no solo aplicar una receta mínima.

**Cómo podría aparecer en examen:** bag of words, necesidad de vectorizar, Bayes sobre texto, RMSE en story points.

**Error típico a evitar:** hablar del texto como si el modelo trabajara sobre significado humano directo sin paso de representación.

---

### 4.12 Estrategia inteligente para estudiar y rendir

Después de reconstruir el mapa completo, la mejor estrategia de estudio no es memorizar una lista inmensa de definiciones. Lo más eficiente es dominar un conjunto de ideas madre y usarlas para colgar el resto.

#### Idea madre 1 — Todo empieza por formular bien el problema
Si reconocés bien si es clasificación, regresión o clustering, ya estás ordenando buena parte del examen.

#### Idea madre 2 — No hay modelado serio sin entender los datos
EDA, faltantes, outliers y features no son preámbulo burocrático. Son el corazón del trabajo aplicado.

#### Idea madre 3 — Evaluar bien importa más que entusiasmarse con un algoritmo
Si una respuesta no menciona generalización, métrica apropiada o costo del error, suele quedar floja.

#### Idea madre 4 — La complejidad es una herramienta, no una garantía
Árbol simple, Random Forest, XGBoost o red neuronal no forman una escala donde el último “gana” siempre. Cada uno resuelve algo y paga algo.

#### Idea madre 5 — En examen se premia mucho la justificación
Cuando dudes entre dos respuestas posibles, suele ser mejor la que:

- da criterio,
- contextualiza,
- compara,
- elige con fundamento.

#### Cómo conviene practicar
1. Poder explicar con tus palabras cada tríada clásica:
   - clasificación / regresión / clustering;
   - precision / recall / F1;
   - overfitting / underfitting / bias-varianza;
   - bagging / boosting;
   - PCA / t-SNE / ISOMAP.
2. Resolver matrices de confusión sin mirar apuntes.
3. Practicar respuestas de “qué usarías y por qué”.
4. Poder contar decisiones concretas de los TP como si fueras el autor del pipeline.
5. Estudiar árboles y ensambles con especial prioridad.

---

## 5. REVISIÓN CRÍTICA FINAL

### 5.1 Debilidades detectadas al reconstruir la materia

- Los materiales disponibles no desarrollan todos los temas con la misma profundidad.
- Algunas resoluciones de parcial son deliberadamente breves y sirven más para identificar foco evaluativo que para construir teoría completa.
- La guía práctica mezcla herramientas, ejercicios y contextos de distinta antigüedad, por lo que no conviene usarla como único organizador pedagógico.

### 5.2 Mejoras realizadas en esta reconstrucción

- Reorganicé la materia por problemas intelectuales y no por orden de archivo.
- Puse primero la intuición y después la formalización.
- Priorizé lo más preguntado en examen sin perder conexión con los TP.
- Hice explícitas las dependencias entre conceptos para que el estudio tenga una secuencia lógica.
- Marqué errores típicos para transformar el resumen en una guía de comprensión y no solo de lectura.

### 5.3 Decisiones editoriales importantes

- Priorizé lo que aparece repetidamente en parciales, recuperatorios y trabajos prácticos.
- Traté redes y NLP con profundidad suficiente para comprensión y examen, pero sin darles el mismo peso que métricas, evaluación, preprocesamiento y árboles, porque los materiales no sostienen esa simetría.
- Evité organizar el texto como glosario, porque eso habría empeorado la comprensión global.
- Elegí un tono de mini-clases enlazadas para que cada bloque pueda estudiarse de forma relativamente autónoma.

### 5.4 Criterio adoptado frente a ambigüedades

Cuando los materiales fueron desparejos o resumidos, prioricé:

1. lo que aparece reiterado en los exámenes,
2. lo que los TP obligan a justificar en la práctica,
3. y recién después los detalles accesorios de herramientas o implementaciones.

Ese criterio busca alinearse con el objetivo real del curso: entender, aplicar y defender decisiones de ciencia de datos con fundamento.
