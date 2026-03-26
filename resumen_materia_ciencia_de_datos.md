# Tratado de Ciencia de Datos: fundamentos, modelado, evaluación y criterio profesional

## Prólogo

Estudiar ciencia de datos con seriedad exige abandonar dos ilusiones muy extendidas. La primera es que la disciplina consiste, ante todo, en elegir algoritmos. La segunda es que basta con ejecutar un pipeline estándar para obtener conclusiones confiables. Ninguna de las dos resiste un análisis riguroso. La ciencia de datos es, antes que nada, una disciplina de representación, inferencia y decisión bajo incertidumbre. Parte de un fenómeno del mundo real, lo traduce a una estructura de datos siempre imperfecta, propone un modelo que simplifica esa realidad y evalúa si esa simplificación es suficientemente buena para un objetivo concreto.

Esta obra está escrita con una convicción pedagógica fuerte: un estudiante no debería verse obligado a elegir entre claridad y profundidad. Cuando un tema es complejo, no hay que mutilarlo para que "suene fácil"; hay que reconstruirlo por capas. Por eso cada capítulo intenta recorrer un camino completo: qué problema obliga a introducir una idea, cuál es su intuición inicial, en qué puntos falla una comprensión ingenua, cómo se formula con precisión, cómo se interpreta esa formulación, qué ejemplos la vuelven concreta y qué errores frecuentes conviene evitar.

El texto asume que la persona lectora puede comenzar con conocimientos mínimos. Sin embargo, no la trata como si fuera incapaz de pensar formalmente. La matemática aparece cuando es necesaria, no como adorno. Toda fórmula importante va acompañada por interpretación conceptual, porque la notación sin comprensión produce falsa seguridad, y la intuición sin formalización produce vaguedad.

Conviene leer este tratado como se estudia una disciplina madura, no como se repasa una lista de temas. Eso significa volver sobre capítulos anteriores, conectar ideas, preguntarse por los supuestos detrás de cada procedimiento y ensayar explicaciones propias. El objetivo final no es solo aprobar una materia. Es poder razonar con precisión sobre datos, modelos, errores, incertidumbre y decisiones.

## Parte I. Fundamentos: qué problema resuelve realmente la ciencia de datos

## Capítulo 1. La ciencia de datos como disciplina de representación y decisión

### 1.1 Qué es y qué no es la ciencia de datos

La ciencia de datos suele definirse de manera superficial como el conjunto de técnicas para extraer conocimiento de datos. La frase es correcta, pero insuficiente. Falta aclarar qué significa "conocimiento", qué entendemos por "extraer" y, sobre todo, por qué la extracción no es automática. Los datos no hablan por sí solos. Son registros construidos mediante instrumentos, criterios administrativos, decisiones humanas y contextos históricos. Por eso, trabajar con datos no equivale a mirar una realidad transparente, sino a interpretar rastros parciales de esa realidad.

Una formulación más rigurosa diría que la ciencia de datos estudia cómo transformar observaciones disponibles en inferencias útiles para describir, predecir, segmentar, detectar anomalías o apoyar decisiones. Esa transformación combina estadística, modelado matemático, conocimiento del dominio, computación y criterio metodológico.

Una idea central de todo el tratado será la siguiente: un modelo no es la realidad. Es una representación comprimida de ciertas regularidades que creemos relevantes para un objetivo. Su valor no depende de que "copie" el mundo, sino de que permita razonar y decidir mejor que alternativas más simples o más complejas.

### 1.2 Fenómeno, dato, variable y decisión

Conviene distinguir cuatro niveles que los principiantes suelen mezclar.

El primero es el **fenómeno**: abandono de clientes, fraude, crecimiento tumoral, demanda eléctrica, sentimiento en textos, etc. El segundo es el **dato**: la forma concreta en que ese fenómeno queda registrado. El tercero es la **variable**: una propiedad medida o construida sobre cada unidad observada. El cuarto es la **decisión**: aquello que alguien hará a partir del análisis, como conceder un crédito, priorizar una inspección, asignar presupuesto o diseñar una campaña.

Esta distinción no es filosófica en sentido ornamental. Tiene consecuencias técnicas inmediatas. Si el fenómeno es la morosidad futura, pero las variables disponibles describen mal la solvencia actual, el problema no se arregla con un modelo más potente. Si la decisión final exige controlar falsos negativos, pero la métrica elegida premia solo accuracy global, tampoco habrá coherencia metodológica. La cadena fenómeno -> dato -> variable -> decisión debe conservar significado en cada eslabón.

### 1.3 Dataset, población, muestra y unidad de análisis

Un dataset no es "un conjunto de filas y columnas" sin más. Es una muestra finita de observaciones tomada de una población o, al menos, de un proceso generador de datos que imaginamos más amplio que la muestra disponible.

Si denotamos por \(n\) el número de observaciones y por \(p\) el número de variables predictoras, un dataset tabular suele representarse como una matriz

\[
X \in \mathbb{R}^{n \times p}
\]

acompañada, en problemas supervisados, por un vector objetivo

\[
y \in \mathbb{R}^n
\]

o, en clasificación, por un vector de etiquetas en un conjunto discreto.

Esta notación es compacta, pero esconde una pregunta crucial: ¿qué representa cada fila? Esa pregunta define la **unidad de análisis**. Una fila puede ser un cliente, una transacción, una sesión web, un documento, una imagen, un día, una ventana de tiempo o una combinación agregada de eventos. Cambiar la unidad de análisis cambia el problema entero.

Supongamos que una empresa quiere predecir churn. Si cada fila representa un cliente, el modelo aprenderá patrones entre clientes. Si cada fila representa interacciones semanales del cliente, el modelo aprenderá sobre secuencias agregadas en el tiempo. Ambas decisiones pueden ser válidas, pero generan targets, features, dependencia entre filas y estrategias de validación diferentes.

El examen suele castigar con dureza una falla típica: responder como si las filas fueran independientes solo porque el dataset está tabulado. Dos filas del mismo cliente, dos mediciones del mismo paciente o dos registros del mismo equipo industrial no son intercambiables sin más. Allí aparece dependencia intra-unidad y la evaluación debe reconocerla.

### 1.4 Features, target y variable respuesta

En aprendizaje supervisado distinguimos entre variables de entrada y variable de salida. A las primeras se las llama **features** o variables predictoras; a la segunda, **target**, variable respuesta, etiqueta o variable objetivo.

No toda variable medida debe transformarse automáticamente en feature. Algunas son irrelevantes, otras están mal definidas, otras introducen fuga de información y otras pueden resultar redundantes o inestables. La selección de features no es un trámite administrativo: es una hipótesis sobre qué información será útil para aproximar el target.

La variable objetivo merece especial cuidado. Una mala definición del target puede arruinar un proyecto completo aunque el entrenamiento sea impecable. "Cliente en riesgo" no es una variable; es una formulación ambigua. Hay que convertirla en una regla operacional, por ejemplo: "cliente que cancela el servicio dentro de los próximos 60 días sin reactivación posterior". Solo entonces el problema se vuelve modelable y evaluable.

### 1.5 Tipos de problemas: clasificación, regresión, clustering y otras tareas

La primera gran distinción metodológica es la naturaleza de la salida.

En **clasificación**, el target pertenece a un conjunto discreto de clases. Si las clases son dos, hablamos de clasificación binaria; si son varias y mutuamente excluyentes, de clasificación multiclase; si una observación puede pertenecer a varias clases a la vez, de clasificación multilabel.

En **regresión**, el target es numérico y la pregunta no es "qué clase corresponde", sino "qué valor continuo conviene estimar". Precio de viviendas, ingreso mensual, demanda futura o duración esperada de un proceso son ejemplos típicos.

En **clustering**, no existe target observado durante el entrenamiento. El objetivo es identificar agrupamientos o estructura latente bajo una noción de similitud.

También aparecen otras tareas importantes. En **ranking**, el sistema ordena ítems por relevancia. En **detección de anomalías**, intenta distinguir observaciones raras respecto de una noción de normalidad. En **reducción de dimensionalidad**, construye una representación más compacta del dato. En **pronóstico temporal**, predice el futuro respetando el orden temporal.

Lo importante no es memorizar la taxonomía, sino entender que cada tipo de problema arrastra decisiones posteriores: pérdida adecuada, métrica, validación, interpretación y riesgos más frecuentes.

### 1.6 Aprendizaje supervisado y no supervisado

En aprendizaje supervisado se observan pares \((x_i, y_i)\). El objetivo es aprender una función \(f\) que relacione entradas y salidas. En aprendizaje no supervisado se observan solo \(x_i\), y el objetivo cambia: descubrir estructura, reducir ruido, segmentar, visualizar o construir representaciones útiles.

Una forma formal de ver el aprendizaje supervisado consiste en definir una pérdida \(L(y, f(x))\) y buscar una función que minimice el riesgo esperado:

\[
R(f) = \mathbb{E}_{(X,Y)\sim P}[L(Y, f(X))]
\]

donde \(P\) es la distribución real, usualmente desconocida, que genera los datos.

Como esa distribución no se conoce, se trabaja con el riesgo empírico:

\[
\hat R_n(f) = \frac{1}{n} \sum_{i=1}^{n} L(y_i, f(x_i))
\]

La diferencia entre ambos conceptos es decisiva. Un modelo puede obtener un riesgo empírico muy bajo y, sin embargo, generalizar mal. Toda la teoría de evaluación, validación y sobreajuste nace de ese desajuste potencial entre lo observado y lo futuro.

En problemas no supervisados no hay una variable respuesta que permita construir directamente una noción única de error predictivo. Por eso la evaluación requiere más cuidado conceptual. En clustering, por ejemplo, no basta con "obtener grupos"; hay que preguntarse si la agrupación es estable, si tiene sentido en el dominio y si sirve para una acción posterior.

### 1.7 Predicción versus causalidad

Uno de los errores más persistentes consiste en interpretar un buen modelo predictivo como si hubiese descubierto causas. La ciencia de datos aplicada trabaja muchas veces con predicción, no con inferencia causal. Son problemas distintos.

La predicción pregunta por \(P(Y \mid X)\): dado lo que observo en \(X\), ¿qué puedo anticipar sobre \(Y\)? La causalidad pregunta por el efecto de intervenir sobre una variable: ¿qué pasaría con \(Y\) si forzara a \(X\) a tomar cierto valor? Ese segundo problema suele expresarse con el operador \(do(\cdot)\), por ejemplo \(P(Y \mid do(X=x))\).

Un modelo puede predecir muy bien la tasa de abandono usando la variable "cantidad de llamados al call center", pero eso no significa que aumentar llamados cause abandono. Tal vez ocurre lo contrario: clientes que ya están insatisfechos llaman más. Confundir asociación con causalidad es un atajo peligroso, tanto en exámenes como en práctica profesional.

### 1.8 Función de pérdida, riesgo y criterio de decisión

Toda técnica de aprendizaje implícita o explícitamente optimiza algo. Ese "algo" suele ser una función de pérdida. La pérdida cuantifica qué tan malo es errar de determinada manera. No existe una pérdida universalmente correcta; su elección depende del problema.

En clasificación binaria, la pérdida 0-1 castiga por igual cualquier error de clase. En regresión, la pérdida cuadrática penaliza más los errores grandes que los pequeños. En clasificación probabilística, la log-loss penaliza con especial dureza probabilidades muy confiadas que resultan falsas.

Pero incluso la pérdida del entrenamiento no agota el problema. Un sistema real toma decisiones. Si el modelo devuelve una probabilidad \(\hat p(x)\), alguien deberá definir desde qué umbral se decide positivo. Esa decisión depende del costo relativo de falsos positivos y falsos negativos. En detección de fraude, dejar pasar un fraude puede costar mucho más que revisar una transacción legítima. En otros contextos puede ocurrir lo contrario.

Una formación experta exige poder responder con precisión: qué se optimiza, por qué se optimiza eso y cómo se traduce ese criterio a la decisión final.

## Capítulo 2. Cómo formular correctamente un problema de datos

### 2.1 Del problema del mundo real al problema modelable

Una organización no pide "aplicar Random Forest". Pide reducir rotación, estimar ventas, priorizar pacientes, detectar operaciones sospechosas o comprender segmentos de usuarios. El primer trabajo serio de la ciencia de datos consiste en traducir esa necesidad en una formulación operativa.

Traducir no significa solo cambiar palabras por variables. Significa fijar con precisión:

1. cuál es la unidad de análisis;
2. qué evento se quiere anticipar o describir;
3. con qué horizonte temporal;
4. qué información estará disponible al momento de decidir;
5. qué costo tiene cada tipo de error.

Si alguna de esas cinco piezas queda difusa, el resto del pipeline se vuelve frágil.

### 2.2 Unidad de análisis, granularidad y nivel de agregación

La **granularidad** indica el nivel de detalle del dato. No es lo mismo trabajar con transacciones individuales que con agregados mensuales por cliente. Tampoco es lo mismo medir actividad diaria que promedios trimestrales.

La granularidad afecta tanto la cantidad de filas como el significado estadístico de las variables. Una media mensual puede ocultar picos semanales importantes. Una agregación por cliente puede perder secuencias de comportamiento críticas para detectar churn o fraude. A la inversa, una granularidad excesivamente fina puede introducir ruido o dependencia entre observaciones que haga más difícil aprender patrones estables.

Los problemas de joins entre tablas suelen nacer aquí. Si se mezclan tablas con distintas granularidades sin un criterio claro, es fácil duplicar registros o contaminar la señal. Unir un dataset de clientes con un dataset de transacciones sin definir si se quiere una fila por cliente o una fila por transacción genera errores estructurales antes de cualquier modelado.

### 2.3 Horizonte temporal y disponibilidad de información

En problemas predictivos, la pregunta correcta nunca es solo "qué quiero predecir", sino también "cuándo quiero predecirlo". Esa dimensión temporal organiza todo el problema.

Predecir churn dentro de 30 días no es lo mismo que predecir churn dentro de 12 meses. Predecir demora logística antes de despachar un pedido no es lo mismo que después de que el paquete ya sufrió retrasos. Una definición temporal imprecisa crea targets ambiguos y features ilegítimas.

La regla de oro es simple de enunciar y decisiva de respetar: toda feature debe representar información disponible en el instante en que la decisión real se tomaría. Si una variable se conoce solo después del evento, usarla en entrenamiento produce **data leakage** o fuga de información. El desempeño obtenido será ilusorio.

### 2.4 Ejemplo completo de formulación: predicción de abandono

Tomemos el problema "queremos anticipar qué clientes se irán". Esa frase, sin más, todavía no define un problema modelable. Hay que convertirla en algo como esto:

"Para cada cliente activo al cierre de cada mes, queremos estimar la probabilidad de cancelar el servicio dentro de los próximos 60 días, utilizando exclusivamente información observada hasta el cierre del mes actual. La decisión asociada será activar o no una campaña de retención."

Obsérvese cuántas decisiones ya aparecen allí.

La unidad de análisis es "cliente activo al cierre de mes". El target es "cancelación dentro de 60 días". El horizonte temporal está explícito. Las features válidas son las conocidas hasta ese corte. La decisión final es una intervención comercial, por lo que no basta con una probabilidad: luego habrá que fijar un umbral o un ranking de prioridad.

Sin esta formulación, cualquier comparación de modelos queda conceptualmente en el aire.

### 2.5 Clasificación, regresión o clustering: cómo decidir

Una manera práctica de decidir el tipo de problema es preguntarse cuál es la naturaleza del target.

Si el target puede expresarse como un conjunto finito de categorías, el problema es de clasificación. Si se expresa como un valor numérico continuo o casi continuo, es de regresión. Si no existe target y se busca descubrir estructura, es de clustering o, más ampliamente, de aprendizaje no supervisado.

Sin embargo, hay zonas grises que vale la pena entender. El mismo problema puede formularse de más de una manera. La morosidad puede pensarse como clasificación binaria (moroso/no moroso), clasificación ordinal (bajo, medio, alto riesgo), ranking (ordenar solicitantes por riesgo) o regresión (estimar monto esperado de pérdida). Cada formulación enfatiza un aspecto distinto del negocio y exige métricas diferentes.

Una respuesta madura en examen no se limita a nombrar la categoría correcta; justifica por qué esa formulación es la más coherente con el objetivo real.

### 2.6 Costo del error y diseño de la métrica

Una fuente habitual de respuestas pobres es elegir la métrica por costumbre. Accuracy, AUC, RMSE o F1 no son medallas universales. Tienen sentido solo en relación con el tipo de error que importa.

Si en una tarea médica un falso negativo implica no detectar una patología seria, el recall puede volverse prioritario. Si en un filtro automático de spam un falso positivo oculta correos importantes, la precisión adquiere mayor peso. Si se estima demanda y los errores grandes son especialmente costosos, RMSE puede reflejar mejor la gravedad del problema que MAE.

La idea profunda es esta: la métrica no es un accesorio del modelo. Es la formalización cuantitativa de qué significa equivocarse en el contexto real.

Si el modelo produce una probabilidad \(p = P(Y=1\mid X=x)\) y el costo de un falso positivo es \(C_{FP}\) mientras que el de un falso negativo es \(C_{FN}\), una regla elemental de decisión bajo costo esperado indica que conviene decidir positivo cuando

\[
(1-p)C_{FP} < p C_{FN}
\]

lo que equivale a

\[
p > \frac{C_{FP}}{C_{FP}+C_{FN}}
\]

La fórmula no debe memorizarse como truco. Debe entenderse. Si \(C_{FN}\) es muy grande, el umbral baja: preferimos aceptar más falsos positivos antes que dejar escapar positivos importantes.

### 2.7 Cómo se nota que un problema está mal formulado

Hay señales recurrentes de una formulación defectuosa.

La primera es que el target cambia de sentido a mitad del análisis. La segunda es que no puede saberse con claridad qué variables serían legales al momento de predecir. La tercera es que la métrica elegida no refleja el costo de error. La cuarta es que el conjunto de entrenamiento mezcla períodos o poblaciones incompatibles con el uso futuro. La quinta es que la pregunta de negocio y la variable modelada no coinciden.

En la práctica, buena parte de los fracasos en ciencia de datos no se debe a un "mal algoritmo", sino a estas debilidades iniciales.

## Parte II. Leer el dato antes de modelarlo

## Capítulo 3. Anatomía del dataset y calidad de datos

### 3.1 Tipos de variables y escalas de medición

Tratar correctamente un dataset exige saber qué tipo de objeto es cada variable. La distinción más básica separa variables numéricas de categóricas, pero conviene ir más allá.

Una variable **nominal** distingue categorías sin orden intrínseco: ciudad, marca, color, tipo de contrato. Una variable **ordinal** posee categorías ordenadas: nivel educativo, severidad, satisfacción en escala Likert. Una variable **de intervalo** admite diferencias significativas pero no un cero absoluto natural, como la temperatura en grados Celsius. Una variable **de razón** sí tiene cero significativo, como ingresos, edad o distancia.

Estas escalas importan porque delimitan operaciones válidas. Promediar códigos postales no tiene sentido. Asignar números 1, 2 y 3 a categorías nominales y luego usar una técnica basada en distancias introduce una geometría artificial. Tratar una variable ordinal como si fuera nominal puede perder información de orden. La representación no es neutral.

### 3.2 Calidad de datos: completitud, validez, consistencia y unicidad

La expresión "limpiar datos" suele sonar práctica, pero es demasiado vaga. Conviene hablar de dimensiones de calidad.

La **completitud** pregunta cuánto falta. La **validez** pregunta si los valores caen en rangos y formatos plausibles. La **consistencia** pregunta si distintas columnas o tablas se contradicen. La **unicidad** pregunta si existen duplicados indebidos. La **trazabilidad** pregunta de dónde viene cada dato y qué transformaciones sufrió.

Cada dimensión puede afectar un proyecto de manera distinta. Un dato faltante puede manejarse con imputación; una fecha invertida puede requerir corrección; un duplicado puede inflar artificialmente el peso de ciertos casos; una columna cuyo significado cambió en el tiempo puede arruinar una evaluación temporal.

### 3.3 Duplicados, inconsistencias y errores semánticos

Los duplicados no son un detalle menor. Pueden ser registros idénticos repetidos por error, o pueden representar eventos genuinos que solo parecen repetidos. El trabajo experto consiste en distinguir ambos casos.

Un cliente que figura dos veces con el mismo identificador pero distinta información puede revelar un problema de integración. Un mismo documento replicado con diferente fecha de carga puede ser evidencia de reprocesamiento. Si se duplican observaciones en entrenamiento y prueba, la performance puede verse artificialmente inflada.

También existen errores semánticos más sutiles: monedas mezcladas, unidades incompatibles, categorías escritas de forma distinta, cambios de criterio administrativo entre años. Estos problemas rara vez los detecta un algoritmo por sí solo. Exigen lectura crítica del dataset y diálogo con el dominio.

### 3.4 Mecanismos de datos faltantes

No todos los faltantes son iguales. Esta idea es central y merece una formulación precisa.

Se dice que un faltante es **MCAR** (*Missing Completely At Random*) cuando la probabilidad de ausencia es independiente tanto del valor faltante como de las demás variables observadas. Es el escenario más benigno y menos frecuente en la práctica.

Se dice que es **MAR** (*Missing At Random*) cuando la ausencia depende de variables observadas. Por ejemplo, el ingreso puede faltar más en ciertos segmentos etarios o regiones. Aquí la ausencia no es completamente aleatoria, pero puede modelarse usando información disponible.

Se dice que es **MNAR** (*Missing Not At Random*) cuando la ausencia depende del propio valor faltante o de variables no observadas. Un ejemplo clásico: personas con ingresos muy altos o muy bajos pueden omitir esa información precisamente por su valor. Este caso es delicado porque la omisión contiene señal difícil de recuperar.

La importancia de esta clasificación no es académica en el mal sentido. De ella depende si una imputación simple es razonable o si inducirá sesgos serios. Imputar con la media bajo un mecanismo MNAR puede distorsionar tanto distribuciones como relaciones con el target.

### 3.5 Outliers: error, rareza o parte esencial del fenómeno

Un valor extremo no es sinónimo de dato erróneo. Puede ser una falla de medición, una observación legítima pero rara o incluso el tipo de caso que más interesa detectar.

En riesgo crediticio, un ingreso desmesuradamente alto puede ser un error de carga o un caso excepcional real. En fraude, una transacción muy anómala puede ser precisamente la señal que se desea capturar. En sensores industriales, un pico imposible puede delatar un problema del instrumento más que del proceso físico observado.

Por eso conviene distinguir entre **outlier estadístico** y **anomalía sustantiva**. Un outlier estadístico es raro respecto de la distribución observada. Una anomalía sustantiva es rara respecto de la lógica del dominio. Ambos conceptos se superponen a veces, pero no siempre.

### 3.6 Sesgo de muestreo, representatividad y desbalance de clases

Un dataset puede estar perfectamente limpio y seguir siendo metodológicamente deficiente si representa mal el fenómeno. La representatividad importa.

Si un modelo de crédito se entrena solo con clientes históricos aprobados, la muestra puede no reflejar el universo de solicitantes reales. Si un sistema de visión se entrena con imágenes de una sola región o dispositivo, puede fallar al desplegarse en otros contextos. Si un dataset médico sobrerrepresenta ciertos grupos demográficos, la performance promedio puede ocultar desempeños desiguales.

El **desbalance de clases** es un caso particular de este problema. Cuando la clase positiva es muy rara, accuracy pierde valor como resumen global. Un clasificador que predice siempre la clase mayoritaria puede parecer excelente bajo accuracy y ser inútil en términos operativos.

### 3.7 Qué debe salir de un análisis de calidad

El resultado de una lectura rigurosa del dataset no es una colección de observaciones dispersas, sino un diagnóstico estructurado. Ese diagnóstico debería responder al menos estas preguntas: qué significan las filas, qué significan las columnas, qué problemas de calidad existen, qué mecanismos plausibles de faltante hay, dónde puede haber fuga de información, qué variables parecen estables, cuáles requieren transformación y qué riesgos de sesgo acompañan a la muestra.

Cuando un estudiante domina este capítulo, deja de ver el dataset como materia prima neutra y empieza a verlo como evidencia imperfecta que debe ser interpretada con cuidado.

## Capítulo 4. Análisis exploratorio de datos: leer antes de modelar

### 4.1 El sentido profundo del EDA

El análisis exploratorio de datos, o EDA, no es una ceremonia previa al modelado. Es la primera etapa inferencial del trabajo. Allí se construye una comprensión concreta de distribuciones, relaciones, sesgos, faltantes, outliers, escalas, desbalances y posibles fugas.

Un EDA serio no consiste en generar gráficos por costumbre, sino en formular preguntas y buscar evidencia. ¿La distribución de la variable es muy asimétrica? ¿Existen categorías dominantes? ¿Las clases están desbalanceadas? ¿Hay separabilidad aparente entre grupos? ¿Se observan relaciones lineales, saturaciones o umbrales? ¿Qué variables podrían requerir transformación?

La calidad del EDA condiciona la calidad del preprocesamiento y, con ello, la del modelado posterior.

### 4.2 Estadística descriptiva univariada

El análisis univariado estudia cada variable por separado. Aquí aparecen medidas de tendencia central, dispersión y forma.

La media de una variable \(x_1,\dots,x_n\) es

\[
\bar x = \frac{1}{n}\sum_{i=1}^{n} x_i
\]

y resume el centro de gravedad aritmético de la muestra. La mediana, en cambio, identifica el valor central al ordenar los datos. Cuando hay asimetría fuerte o outliers, la mediana suele representar mejor el centro "típico" que la media.

La varianza muestral

\[
s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar x)^2
\]

mide dispersión en torno a la media. Su raíz cuadrada es el desvío estándar. Sin embargo, ambas medidas son sensibles a valores extremos. Por eso conviene complementar con cuantiles e IQR:

\[
IQR = Q_3 - Q_1
\]

donde \(Q_1\) y \(Q_3\) son el primer y el tercer cuartil.

Una lectura experta de estas medidas no se queda en la definición. Se pregunta qué dicen del fenómeno. Una media muy superior a la mediana sugiere asimetría a derecha. Un IQR pequeño con extremos aislados sugiere concentración central con pocos valores muy alejados. Una variable casi constante puede aportar poco al modelo.

### 4.3 Frecuencias y análisis de variables categóricas

En variables categóricas, el análisis central no es el promedio sino la distribución de frecuencias. Importa saber qué categorías dominan, cuáles son raras y qué cardinalidad presenta la variable.

Una categoría rara no debe eliminarse automáticamente. Puede ser ruido, pero también puede corresponder a un segmento de alta relevancia. Por ejemplo, en fraude o fallas críticas, la rareza es muchas veces precisamente la señal más valiosa.

El EDA categórico también permite detectar problemas de codificación: categorías duplicadas por espacios, mayúsculas, tildes, abreviaturas o cambios históricos de nomenclatura.

### 4.4 Relaciones entre variables: correlación, dependencia y no linealidad

El análisis multivariado intenta entender cómo se relacionan las variables entre sí y con el target.

La covarianza entre dos variables \(X\) e \(Y\) mide cómo varían conjuntamente:

\[
\text{Cov}(X,Y)=\mathbb{E}[(X-\mathbb{E}[X])(Y-\mathbb{E}[Y])]
\]

En la muestra, una versión estimada conduce al coeficiente de correlación de Pearson:

\[
r_{XY} = \frac{\sum_{i=1}^{n}(x_i-\bar x)(y_i-\bar y)}{\sqrt{\sum_{i=1}^{n}(x_i-\bar x)^2}\sqrt{\sum_{i=1}^{n}(y_i-\bar y)^2}}
\]

Este coeficiente toma valores entre \(-1\) y \(1\) y mide asociación lineal. El punto crucial es que **correlación cero no implica ausencia de relación**. Si \(Y = X^2\) y \(X\) está simétricamente distribuida alrededor de cero, la correlación lineal puede ser cercana a cero aunque la dependencia sea total. Este es uno de los ejemplos más útiles para evitar interpretaciones ingenuas.

Por eso el EDA no puede apoyarse solo en tablas de correlación. Necesita gráficos, comparaciones estratificadas, análisis de no linealidad y, sobre todo, criterio.

### 4.5 Visualización con propósito analítico

Las visualizaciones buenas no son las más decorativas, sino las que responden preguntas concretas.

El histograma sirve para inspeccionar forma de distribución. El boxplot ayuda a visualizar dispersión y posibles outliers. El scatterplot permite explorar relaciones entre pares de variables y detectar no linealidades o subpoblaciones. Un mapa de calor puede resumir asociaciones, pero no debe reemplazar el análisis fino de las relaciones más importantes.

Mostrar un gráfico sin extraer una conclusión es una forma elegante de no explicar nada. En un examen, y más aún en un informe profesional, siempre conviene formular qué se ve, por qué importa y qué decisión metodológica sugiere.

### 4.6 EDA respecto del target

En clasificación, interesa comparar cómo se distribuyen las features entre clases. ¿La clase positiva presenta ingresos menores, mayor recencia, distinta frecuencia de uso, patrones de texto diferentes? En regresión, interesa observar si la relación parece lineal, saturada, escalonada, heterocedástica o fuertemente afectada por algunos puntos.

Este análisis orienta decisiones concretas. Si una variable muestra relación claramente no lineal con el target, una transformación o un modelo no lineal puede ser razonable. Si una clase está extremadamente desbalanceada, ya durante EDA debe preverse que accuracy será insuficiente.

### 4.7 El EDA como detector de leakage

Muchas fugas de información pueden sospecharse antes de entrenar nada. Variables con separación casi perfecta respecto del target, timestamps posteriores al evento que se quiere predecir, identificadores administrativamente ligados al resultado o columnas derivadas del proceso de resolución del caso son señales de alarma.

Cuando una feature parece "demasiado buena para ser verdad", el especialista no celebra primero. Desconfía primero.

### 4.8 Qué debería producir un buen EDA

Al terminar un EDA serio, el equipo debería tener:

1. un mapa claro de tipos de variables y calidad de datos;
2. hipótesis sobre transformaciones, imputaciones y encoding;
3. sospechas fundadas sobre leakage, outliers y sesgos;
4. noción de desbalance, escalas y relaciones relevantes;
5. criterios iniciales para elección de métricas y modelos.

Si el EDA no produce decisiones, probablemente fue ornamental.

## Parte III. Preprocesar no es maquillar: representar bien para poder aprender

## Capítulo 5. Preprocesamiento y construcción de representación

### 5.1 El principio rector: los modelos aprenden sobre representaciones

Todo modelo trabaja sobre una representación del fenómeno, no sobre el fenómeno mismo. Incluso en datos tabulares aparentemente "listos", cada columna ya es una decisión de medición o construcción. El preprocesamiento continúa esa tarea: busca transformar la información disponible en un espacio donde el patrón relevante sea más aprendible y el ruido menos dañino.

Dos principios deben gobernar esta etapa. El primero es que toda transformación que aprende parámetros debe ajustarse solo con datos de entrenamiento. El segundo es que ninguna transformación debe alterar el significado del problema de manera inconsistente con el uso real del modelo.

### 5.2 Pipelines y separación entre entrenamiento, validación y prueba

Un error clásico consiste en imputar, escalar o reducir dimensión sobre todo el dataset antes de hacer la partición. Eso introduce información del conjunto de evaluación en la etapa de entrenamiento.

Supongamos una estandarización:

\[
z_{ij} = \frac{x_{ij} - \mu_j}{\sigma_j}
\]

donde \(\mu_j\) y \(\sigma_j\) son media y desvío de la feature \(j\). Si esos parámetros se calculan usando todo el dataset, el conjunto de prueba deja de ser verdaderamente externo. La forma correcta es estimarlos solo con entrenamiento:

\[
z_{ij}^{(test)} = \frac{x_{ij}^{(test)} - \mu_j^{(train)}}{\sigma_j^{(train)}}
\]

Esta lógica vale para imputadores, escaladores, selectores de variables, PCA, target encoding y cualquier otra transformación que "aprenda" algo de los datos.

### 5.3 Imputación: decidir bajo incertidumbre

Imputar no es rellenar huecos mecánicamente. Es tomar una decisión estadística sobre valores no observados. Cada estrategia supone algo sobre el proceso de faltante y sobre la estructura de la variable.

La imputación por media es simple, pero reduce artificialmente la varianza y puede distorsionar relaciones. La imputación por mediana es más robusta a asimetrías y outliers. La imputación por moda puede ser razonable para categóricas de baja cardinalidad. La imputación por grupo usa información contextual, por ejemplo mediana de ingreso por segmento. Métodos como KNN o imputación multivariada intentan preservar estructura entre variables, aunque introducen mayor complejidad.

En muchas situaciones conviene agregar además una variable indicadora de faltante. Si la ausencia misma porta señal, esa columna puede ayudar al modelo. Esto es especialmente valioso bajo MAR o MNAR plausibles.

Un criterio experto no pregunta solo "qué imputación mejora la métrica", sino también "qué historia sobre el dato estoy asumiendo al imputar así".

### 5.4 Tratamiento de outliers

No existe una regla universal para outliers porque no existe una única razón por la cual aparecen. Algunas estrategias comunes son:

1. corregir si hay evidencia de error;
2. eliminar si son registros inválidos y escasos;
3. winsorizar si se desea limitar impacto extremo sin borrar casos;
4. transformar, por ejemplo con logaritmos;
5. usar modelos o métricas robustas;
6. modelar explícitamente el régimen raro si tiene interés propio.

La detección puede apoyarse en z-score, IQR, distancia de Mahalanobis, métodos de densidad o simplemente conocimiento de dominio. Pero la decisión final no debería depender solo del umbral estadístico.

### 5.5 Encoding de variables categóricas

Muchos modelos numéricos no aceptan categorías en texto. Hay que codificarlas, pero no de cualquier manera.

El **one-hot encoding** crea una columna binaria por categoría. Es la opción estándar para variables nominales porque evita imponer un orden inexistente. Su desventaja es el crecimiento dimensional cuando la cardinalidad es alta.

El **ordinal encoding** asigna números enteros a categorías ordenadas. Solo es válido cuando el orden tiene significado real. Aplicarlo a categorías nominales puede inducir relaciones espurias.

El **target encoding** reemplaza cada categoría por algún resumen del target, por ejemplo la tasa media de la clase positiva. Puede ser muy potente con alta cardinalidad, pero es especialmente vulnerable a leakage. Si se aplica, debe estimarse de forma interna a cada fold o con esquemas de regularización apropiados.

### 5.6 Escalado, normalización y sensibilidad del modelo

La necesidad de escalar depende de la familia de modelos.

En métodos basados en distancias, como KNN, o en optimización por gradiente, como SVM con ciertos kernels, regresión regularizada y redes neuronales, la escala importa mucho. Una variable medida en miles puede dominar a otra medida en décimas aunque esta última sea más relevante.

La **estandarización** centra y escala por desvío. La **normalización min-max** lleva la variable a un rango fijo. El **robust scaling** utiliza mediana e IQR y resulta útil cuando hay outliers.

En árboles y ensambles de árboles, en cambio, la escala suele importar mucho menos porque las decisiones se basan en umbrales por variable y no en distancias globales.

### 5.7 Discretización y transformaciones de distribución

Discretizar una variable numérica significa convertirla en intervalos. A veces se hace por interpretabilidad, por robustez ante ruido o porque cierta técnica se beneficia de ello. Pero discretizar también implica pérdida de información. No debería hacerse por reflejo.

Las transformaciones continuas, como \(\log(x+c)\), Box-Cox o Yeo-Johnson, pueden ser muy útiles cuando una variable presenta asimetría fuerte, heterocedasticidad o colas pesadas. La transformación logarítmica, por ejemplo, comprime diferencias relativas grandes y suele volver más lineales ciertas relaciones.

Lo importante es interpretar lo que se transformó. Si modelamos \(\log(y)\) en lugar de \(y\), los coeficientes ya no describen cambios absolutos sino aproximaciones a cambios relativos.

### 5.8 Feature engineering: donde el dominio se vuelve variable

La ingeniería de variables es, muchas veces, la diferencia entre un modelo apenas correcto y un modelo realmente útil. Consiste en construir nuevas features que expresen mejor la estructura relevante del problema.

Algunos ejemplos clásicos son:

1. razones, como ingreso/cuota o gasto/ingreso;
2. interacciones, como \(x_1 x_2\);
3. transformaciones temporales, como lags, rolling means o indicadores de estacionalidad;
4. agregados por usuario, como recencia, frecuencia y monetización;
5. atributos de texto, como longitud, n-gramas o TF-IDF;
6. variables de comportamiento, como secuencias resumidas de eventos.

Una buena feature no es un artificio arbitrario. Es una hipótesis informada sobre qué aspecto del fenómeno importa.

### 5.9 Selección de variables y multicolinealidad

No todas las variables contribuyen del mismo modo. Algunas son casi irrelevantes, otras duplican información y otras aumentan ruido o inestabilidad.

En modelos lineales, la **multicolinealidad** es especialmente importante. Si varias variables están muy correlacionadas, los coeficientes pueden volverse inestables e interpretar cada uno por separado resulta peligroso. Un diagnóstico clásico es el VIF (*Variance Inflation Factor*), que mide cuánto aumenta la varianza de un coeficiente por la colinealidad con otros predictores.

La regularización L2 puede estabilizar estimaciones; L1 puede, además, forzar algunos coeficientes a cero. En árboles, la multicolinealidad afecta menos el ajuste directo, pero puede distorsionar la lectura de importancias.

### 5.10 Qué preprocesamiento necesita cada familia de modelos

Una señal de madurez conceptual es no responder "siempre hay que escalar", "siempre hay que imputar con la media" o "siempre hay que hacer one-hot". La respuesta correcta depende del modelo y del problema.

KNN y SVM suelen exigir escalado. Regresión lineal y logística lo agradecen especialmente cuando hay regularización. Árboles toleran bien escalas heterogéneas y ciertas no linealidades. Naive Bayes para texto necesita una representación distinta que para datos tabulares continuos. PCA requiere centrado adecuado. Redes neuronales suelen beneficiarse de entradas estables y bien escaladas.

El preprocesamiento es parte del modelado. No existe como capítulo separado en la realidad del trabajo.

## Parte IV. Evaluar sin autoengañarse

## Capítulo 6. Generalización, validación y diseño experimental

### 6.1 El problema central: entrenar no es demostrar

Que un modelo funcione bien sobre los datos usados para ajustarlo no demuestra que funcionará bien sobre datos nuevos. Esta frase, obvia en apariencia, es el corazón metodológico de la materia.

Si un modelo es demasiado flexible, puede aprender regularidades reales y también ruido accidental. Si es demasiado rígido, puede ignorar estructura importante. El objetivo no es solo ajustar, sino **generalizar**: rendir bien fuera de la muestra observada.

Matemáticamente, el problema se expresa en la diferencia entre riesgo empírico y riesgo poblacional. Todo el aparato de train/test split, validación cruzada, regularización y análisis de sesgo-varianza existe para controlar esa brecha.

### 6.2 Conjuntos de entrenamiento, validación y prueba

Una división conceptual estándar distingue tres roles:

1. **entrenamiento** para ajustar parámetros del modelo;
2. **validación** para comparar alternativas e hiperparámetros;
3. **prueba** para estimar desempeño final en datos no usados para decidir.

En datasets pequeños, entrenamiento y validación suelen organizarse mediante validación cruzada. Pero la lógica de roles sigue vigente: el conjunto de prueba no debe participar en decisiones de tuning. Si participa, deja de ser prueba.

### 6.3 Hold-out y validación cruzada

El **hold-out** es la partición simple entre entrenamiento y prueba. Es rápido y útil, pero su estimación puede depender demasiado de cómo cayó el azar en una única división.

La **validación cruzada k-fold** divide el conjunto de entrenamiento en \(k\) pliegues. En cada iteración, se entrena con \(k-1\) pliegues y se valida con el restante. Al final, se promedia el resultado. Esta estrategia reduce la dependencia de una sola partición.

Cuando las clases están desbalanceadas, conviene usar validación cruzada **estratificada**, que preserve aproximadamente la proporción de clases en cada fold. Cuando hay múltiples registros por individuo, dispositivo o grupo, conviene usar esquemas por grupo para evitar que información muy parecida quede tanto en entrenamiento como en validación.

### 6.4 Validación temporal y walk-forward

En series temporales y problemas donde el tiempo importa, mezclar observaciones al azar es metodológicamente incorrecto. El futuro no debe usarse para predecir el pasado.

En estos casos se utilizan esquemas de validación temporal, como ventanas deslizantes o crecientes. La idea es entrenar en un bloque histórico y validar en un bloque posterior, repitiendo el proceso de forma que la cronología real se conserve.

Esto no es un capricho técnico. Si se rompe el orden temporal, pueden aparecer patrones espurios imposibles de replicar en producción.

### 6.5 Nested cross-validation y comparación honesta de modelos

Cuando se ajustan hiperparámetros de manera intensiva, incluso la validación cruzada puede volverse optimista si se reutiliza para seleccionar y evaluar a la vez. Allí aparece la **nested cross-validation**: un bucle interno para tuning y un bucle externo para estimación más honesta del desempeño.

No siempre es necesaria en trabajos rutinarios, pero conceptualmente es importante porque muestra un principio general: toda elección basada en datos consume información y puede sesgar hacia el optimismo la evaluación si no se separan adecuadamente los roles.

### 6.6 Overfitting, underfitting y curvas de aprendizaje

El **underfitting** ocurre cuando el modelo es demasiado simple para capturar estructura relevante. Se observa mal desempeño tanto en entrenamiento como en validación. El **overfitting** aparece cuando el modelo se ajusta demasiado a peculiaridades del entrenamiento: el error de entrenamiento baja mucho, pero el de validación no acompaña o empeora.

Las curvas de aprendizaje ayudan a diagnosticar ambas situaciones. Si con más datos el error de entrenamiento sube un poco y el de validación baja de forma sostenida, puede haber alta varianza. Si ambos errores permanecen altos, hay sesgo elevado o features pobres. Estas lecturas son mucho más informativas que mirar un único score aislado.

### 6.7 Sesgo y varianza como marco unificador

El error esperado de un modelo, en un problema de regresión con pérdida cuadrática, puede descomponerse conceptualmente como:

\[
\mathbb{E}\left[(Y-\hat f(X))^2\right] = \text{Bias}^2 + \text{Variance} + \text{Noise}
\]

El término de sesgo captura cuánto se desvía, en promedio, la predicción esperada del modelo respecto de la función verdadera. La varianza captura cuánto cambia la predicción si repitiéramos el entrenamiento sobre distintas muestras. El ruido representa lo irreducible del fenómeno.

Modelos muy rígidos suelen tener alto sesgo y baja varianza. Modelos muy flexibles, bajo sesgo y alta varianza. La regularización, el tamaño de muestra, la complejidad del modelo y la calidad de las features afectan este equilibrio.

### 6.8 Significancia estadística y relevancia práctica

Una mejora pequeña en AUC o RMSE puede ser estadísticamente consistente y, sin embargo, operacionalmente irrelevante. También puede ocurrir lo contrario: una mejora modesta en la métrica global puede producir gran impacto económico en el segmento crítico del negocio.

Por eso evaluar no es solo comparar promedios. Es traducir diferencias a consecuencias: cuántos casos importantes se recuperan, cuánto costo se evita, qué estabilidad tiene el resultado entre folds o períodos, cuánto cambia la performance entre subpoblaciones.

## Capítulo 7. Métricas: cómo cuantificar el error sin perder el sentido del problema

### 7.1 Matriz de confusión y lectura estructural del error

En clasificación binaria, la matriz de confusión organiza resultados en cuatro celdas:

1. verdaderos positivos (TP);
2. falsos positivos (FP);
3. verdaderos negativos (TN);
4. falsos negativos (FN).

Más que una tabla contable, la matriz de confusión es un mapa de errores. Permite distinguir qué tipo de equivocación domina y si eso coincide o no con lo que más importa en el problema real.

Supongamos 1000 casos, de los cuales 50 son positivos reales. Un modelo detecta 40 de esos positivos, deja pasar 10 y marca erróneamente 60 negativos como positivos. Entonces:

1. \(TP = 40\)
2. \(FN = 10\)
3. \(FP = 60\)
4. \(TN = 890\)

Ya con estos números puede verse un punto crucial: el modelo recupera gran parte de los positivos, pero genera bastantes falsas alarmas. Dependiendo del contexto, eso puede ser aceptable o desastroso.

### 7.2 Accuracy, precision, recall, especificidad y F1

La **accuracy** se define como

\[
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
\]

y mide la proporción total de aciertos. En el ejemplo anterior:

\[
\text{Accuracy} = \frac{40 + 890}{1000} = 0.93
\]

Ese 93% parece excelente. Sin embargo, el problema tiene solo 5% de positivos. Un clasificador que predijera siempre negativo lograría 95% de accuracy y sería inútil. Este ejemplo basta para mostrar por qué accuracy puede engañar en clases desbalanceadas.

La **precision** es

\[
\text{Precision} = \frac{TP}{TP + FP}
\]

y responde a la pregunta: de todo lo que marqué como positivo, ¿cuánto era realmente positivo? En el ejemplo:

\[
\text{Precision} = \frac{40}{40 + 60} = 0.40
\]

El **recall** o sensibilidad es

\[
\text{Recall} = \frac{TP}{TP + FN}
\]

y responde a otra pregunta: de todos los positivos reales, ¿cuántos encontré? En el ejemplo:

\[
\text{Recall} = \frac{40}{50} = 0.80
\]

La **especificidad** es

\[
\text{Specificity} = \frac{TN}{TN + FP}
\]

y mide la capacidad de reconocer negativos.

El **F1-score** combina precisión y recall mediante su media armónica:

\[
F_1 = 2 \cdot \frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
\]

La media armónica castiga más los desequilibrios que la media aritmética. Si una de las dos métricas es muy baja, F1 también lo será.

### 7.3 Curvas ROC, AUC y curvas Precision-Recall

Muchos clasificadores no devuelven directamente una clase, sino un score o una probabilidad. La clasificación final depende del umbral elegido. Las curvas ROC y PR permiten analizar el comportamiento del modelo a lo largo de todos los umbrales posibles.

La curva **ROC** grafica TPR contra FPR. El AUC-ROC resume la capacidad de discriminar positivos y negativos. Tiene valor, pero puede resultar demasiado optimista cuando la clase positiva es muy rara.

La curva **Precision-Recall** suele ser más informativa en esos contextos porque se concentra en el comportamiento sobre la clase positiva. Si el negocio depende de recuperar positivos escasos, mirar solo AUC-ROC puede ocultar debilidades importantes.

### 7.4 Umbral de decisión y calibración

Elegir un umbral no es un detalle técnico menor. Si el modelo produce probabilidades, convertirlas a decisiones requiere un criterio. A veces se usa 0.5 por costumbre, pero eso rara vez está justificado.

El umbral debería responder al costo relativo de errores, a la capacidad operativa para revisar casos y al objetivo del sistema. En problemas de screening puede convenir un umbral bajo para no dejar pasar casos importantes. En problemas donde revisar falsos positivos es caro, puede preferirse un umbral más alto.

La **calibración** agrega otra dimensión. Un modelo puede ordenar muy bien los casos y, sin embargo, asignar probabilidades mal calibradas. Si entre los casos a los que asigna 0.8 de probabilidad solo ocurre el evento en el 50%, la interpretación probabilística es deficiente aunque el ranking sea útil.

### 7.5 Métricas de regresión

En regresión, el error debe cuantificar distancias entre valores reales y predichos.

El **MAE** se define como

\[
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat y_i|
\]

y tiene la ventaja de ser interpretable en las mismas unidades del target. Si se predicen precios en miles de pesos, el MAE también queda en miles de pesos.

El **MSE** es

\[
MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat y_i)^2
\]

y el **RMSE** es su raíz cuadrada:

\[
RMSE = \sqrt{MSE}
\]

Al elevar al cuadrado, MSE y RMSE castigan mucho más los errores grandes. Esto puede ser deseable o no, según el problema.

El coeficiente de determinación \(R^2\) se define como

\[
R^2 = 1 - \frac{\sum_{i=1}^{n}(y_i - \hat y_i)^2}{\sum_{i=1}^{n}(y_i - \bar y)^2}
\]

y mide cuánto mejora el modelo respecto de predecir siempre la media. Su interpretación requiere cuidado. Un \(R^2\) alto no garantiza errores pequeños en términos absolutos, y un \(R^2\) moderado puede ser aceptable en fenómenos intrínsecamente ruidosos.

### 7.6 Métricas internas y externas en clustering

Como el clustering no tiene target durante el entrenamiento, la evaluación requiere otros criterios.

La **silhouette** compara, para cada punto, cuán cerca está de su cluster respecto de otros clusters. Si \(a(i)\) es la distancia promedio del punto \(i\) a los puntos de su propio cluster y \(b(i)\) es la menor distancia promedio a otro cluster, entonces

\[
s(i)=\frac{b(i)-a(i)}{\max\{a(i),b(i)\}}
\]

Valores cercanos a 1 sugieren buena asignación; valores cercanos a 0 sugieren frontera difusa; valores negativos sugieren posible mala asignación. El índice de Davies-Bouldin resume compactación y separación. Si existen etiquetas externas de referencia, pueden usarse medidas como ARI o NMI, pero hay que recordar que entonces ya no estamos evaluando clustering "puro" sino similitud respecto de una partición conocida.

Ninguna métrica interna reemplaza la interpretación sustantiva. Un clustering puede maximizar silhouette y aun así carecer de utilidad analítica o de sentido de negocio.

### 7.7 Elegir la métrica correcta

Una regla útil es esta: la mejor métrica es la que penaliza los errores que verdaderamente importan.

Si las clases están muy desbalanceadas y lo importante es detectar positivos, accuracy probablemente sea insuficiente. Si los errores extremos son muy costosos, RMSE puede ser más informativo que MAE. Si importa la calidad probabilística, la calibración es central. Si la decisión final depende de revisar un número fijo de casos, tal vez convenga evaluar precisión en top-k o métricas de ranking.

La madurez técnica consiste en justificar esa elección, no en recitar definiciones aisladas.

## Parte V. Modelos supervisados fundamentales

## Capítulo 8. Regresión lineal: una base que sigue siendo esencial

### 8.1 Por qué sigue importando un modelo lineal

La regresión lineal suele presentarse como un modelo "básico", y eso lleva a subestimarla. En realidad, es una pieza central por varias razones. Primero, porque enseña con gran claridad la relación entre hipótesis estructural, pérdida y estimación. Segundo, porque su interpretabilidad es muy alta. Tercero, porque muchos conceptos más avanzados pueden entenderse como extensiones, correcciones o flexibilizaciones de sus supuestos.

El modelo lineal clásico para una variable respuesta continua es

\[
y = \beta_0 + \beta_1 x_1 + \cdots + \beta_p x_p + \varepsilon
\]

donde \(\beta_0\) es la ordenada al origen, \(\beta_j\) son coeficientes asociados a las features y \(\varepsilon\) representa el término de error o ruido no explicado.

Lo importante es comprender qué significa "lineal". No quiere decir necesariamente que la relación entre el fenómeno y cada variable sea simple o trivial. Quiere decir que el modelo es lineal en los parámetros \(\beta\). Podemos incluir términos transformados, como \(x^2\) o \(\log x\), y seguir teniendo un modelo lineal en los coeficientes.

### 8.2 Mínimos cuadrados ordinarios

Si escribimos el modelo en forma matricial:

\[
y = X\beta + \varepsilon
\]

la estimación por mínimos cuadrados ordinarios busca el vector \(\hat \beta\) que minimiza la suma de cuadrados de residuos:

\[
\hat \beta = \arg\min_{\beta} \|y - X\beta\|_2^2
\]

Esta función objetivo expresa una idea sencilla: elegir los coeficientes que hagan lo más pequeña posible la discrepancia cuadrática entre valores observados y predichos.

La derivación es importante porque enseña a leer la fórmula, no solo a usarla. Si definimos

\[
S(\beta) = (y - X\beta)^T(y - X\beta)
\]

y derivamos respecto de \(\beta\), obtenemos las ecuaciones normales:

\[
X^T X \hat \beta = X^T y
\]

Si \(X^T X\) es invertible, entonces:

\[
\hat \beta = (X^T X)^{-1}X^T y
\]

Cada símbolo aquí tiene interpretación. \(X^T X\) resume la geometría y correlación de las features. \(X^T y\) mide su relación lineal con la respuesta. La inversa "desenreda" la combinación lineal de estas relaciones para producir el vector de coeficientes.

### 8.3 Interpretación geométrica

La solución de mínimos cuadrados puede verse geométricamente como la proyección ortogonal del vector \(y\) sobre el subespacio generado por las columnas de \(X\). El modelo lineal busca la mejor aproximación lineal posible dentro de ese subespacio.

Esta lectura geométrica ayuda a entender por qué la colinealidad es problemática. Si las columnas de \(X\) son casi linealmente dependientes, el subespacio queda mal condicionado y la proyección se vuelve numéricamente inestable.

### 8.4 Cómo interpretar los coeficientes

Si el modelo está bien especificado, \(\beta_j\) puede leerse como el cambio esperado en \(y\) ante un aumento de una unidad en \(x_j\), manteniendo constantes las demás variables. Esta cláusula final es crucial. Muchas malas interpretaciones provienen de ignorarla.

Decir "si edad aumenta un año, el ingreso sube \(\beta\)" solo tiene sentido dentro del modelo y controlando las demás features incluidas. Además, no implica causalidad salvo que exista un diseño causal que lo justifique.

En presencia de transformaciones, la interpretación cambia. Si la respuesta está en logaritmos, los coeficientes aproximan cambios porcentuales. Si una variable está estandarizada, el coeficiente describe el efecto de aumentar un desvío estándar, no una unidad original.

### 8.5 Supuestos clásicos y qué significa violarlos

Los supuestos clásicos asociados a la regresión lineal incluyen:

1. linealidad en parámetros;
2. exogeneidad o media condicional del error igual a cero;
3. independencia de errores;
4. homocedasticidad;
5. ausencia de multicolinealidad severa;
6. normalidad de errores para ciertos resultados inferenciales.

No todos estos supuestos tienen el mismo rol. La normalidad, por ejemplo, no es necesaria para calcular predicciones, pero sí facilita inferencia exacta en muestras moderadas. La homocedasticidad afecta especialmente la precisión de errores estándar e intervalos. La colinealidad daña la estabilidad de coeficientes. La dependencia temporal o por grupo puede volver optimistas las evaluaciones si se ignora.

Una respuesta superficial enumera supuestos. Una respuesta sólida explica qué se rompe cuando cada uno falla.

### 8.6 Residuos, diagnóstico y límites del modelo

El residuo \(e_i = y_i - \hat y_i\) condensa el error del modelo en cada observación. Analizar residuos permite detectar no linealidad remanente, heterocedasticidad, outliers influyentes y dependencia no modelada.

Un patrón sistemático en residuos contra valores ajustados sugiere especificación incompleta. Un abanico creciente puede señalar varianza no constante. Unos pocos puntos muy influyentes pueden alterar de forma desproporcionada los coeficientes.

La regresión lineal no fracasa por ser "simple", sino por ser inapropiada cuando la estructura dominante del problema no puede expresarse bien en su forma.

### 8.7 Regularización: Ridge, Lasso y Elastic Net

Cuando hay muchas variables, colinealidad o riesgo de sobreajuste, se agregan penalizaciones a la función de mínimos cuadrados.

En **Ridge** se minimiza:

\[
\|y - X\beta\|_2^2 + \lambda \|\beta\|_2^2
\]

El segundo término penaliza coeficientes grandes y tiende a contraerlos de manera suave.

En **Lasso** se minimiza:

\[
\|y - X\beta\|_2^2 + \lambda \|\beta\|_1
\]

donde \(\|\beta\|_1 = \sum_j |\beta_j|\). Esta penalización puede llevar algunos coeficientes exactamente a cero, produciendo selección implícita de variables.

**Elastic Net** combina ambas:

\[
\|y - X\beta\|_2^2 + \lambda_1 \|\beta\|_1 + \lambda_2 \|\beta\|_2^2
\]

La intuición general es clara: aceptamos algo de sesgo para reducir varianza y mejorar generalización.

### 8.8 Cuándo conviene usar regresión lineal

Conviene cuando la interpretabilidad importa, cuando la relación es aproximadamente lineal o puede linearizarse con transformaciones, cuando se necesita un baseline fuerte y cuando el número de observaciones no justifica modelos excesivamente complejos.

En examen suelen pedir comparar regresión lineal con modelos más flexibles. Una respuesta madura no dice solo que uno "es más simple y el otro más complejo". Explica que la lineal ofrece interpretabilidad, estabilidad y facilidad de comunicación, pero puede quedarse corta frente a interacciones y no linealidades pronunciadas si no se enriquecen las features.

## Capítulo 9. Regresión logística: probabilidad para decisiones binarias

### 9.1 Por qué no alcanza la regresión lineal para clasificar

Si el target es binario, podría tentarnos usar una regresión lineal y luego umbralizar la salida. El problema es que las predicciones de un modelo lineal no están restringidas al intervalo \([0,1]\), y además la varianza condicional y la forma de la relación probabilística no son las adecuadas para una variable Bernoulli.

La regresión logística resuelve esto modelando la probabilidad de la clase positiva mediante la función sigmoide:

\[
\sigma(z) = \frac{1}{1 + e^{-z}}
\]

con

\[
z = \beta_0 + \beta_1 x_1 + \cdots + \beta_p x_p
\]

Así, la probabilidad estimada es

\[
P(Y=1\mid X=x) = \sigma(\beta_0 + \beta^T x)
\]

### 9.2 Odds y logit

La regresión logística se entiende mejor si se introduce la noción de **odds**:

\[
\text{odds} = \frac{p}{1-p}
\]

donde \(p\) es la probabilidad del evento.

Tomando logaritmo obtenemos el **logit**:

\[
\log\left(\frac{p}{1-p}\right) = \beta_0 + \beta^T x
\]

Esta formulación es poderosa porque linealiza los log-odds, no la probabilidad. Allí aparece una fuente frecuente de errores conceptuales: los coeficientes logísticos no describen cambios lineales directos en probabilidad.

### 9.3 Estimación por máxima verosimilitud

Si \(Y_i\) sigue una distribución Bernoulli con parámetro \(p_i\), la probabilidad conjunta de observar la muestra, dada \(\beta\), es

\[
\mathcal{L}(\beta) = \prod_{i=1}^{n} p_i^{y_i}(1-p_i)^{1-y_i}
\]

Trabajar con el logaritmo de esa verosimilitud simplifica el problema:

\[
\ell(\beta) = \sum_{i=1}^{n}\left[y_i \log p_i + (1-y_i)\log(1-p_i)\right]
\]

Maximizar esta expresión equivale a minimizar la log-loss. Lo importante pedagógicamente es entender la lógica: se eligen los parámetros que hacen más probable la muestra observada bajo el modelo propuesto.

### 9.4 Cómo interpretar un coeficiente logístico

Si \(\beta_j\) aumenta en una unidad, los log-odds cambian en \(\beta_j\). Equivalente y más interpretable aún:

\[
e^{\beta_j}
\]

es el factor multiplicativo por el que cambian los odds al aumentar una unidad en \(x_j\), manteniendo todo lo demás constante.

Por ejemplo, si \(\beta_j = 0.405\), entonces

\[
e^{0.405} \approx 1.5
\]

Eso significa que los odds aumentan en un 50%, no que la probabilidad aumente 50 puntos porcentuales. La diferencia es fundamental. Cuando la probabilidad base es baja, un aumento de odds puede cambiar poco la probabilidad. Cuando la probabilidad base está cerca de 0.5, el efecto en probabilidad puede ser mayor.

### 9.5 Umbral, desbalance y decisión

La regresión logística produce probabilidades estimadas. Convertirlas en clases requiere elegir un umbral. En problemas con clases desbalanceadas, usar 0.5 sin discusión es una señal de poca madurez metodológica.

Además, la logística suele funcionar muy bien como baseline por varias razones: es interpretable, rápida, probabilística y relativamente estable. En muchos problemas tabulares bien trabajados, un baseline logístico regularizado ofrece un punto de comparación muy difícil de superar honestamente.

### 9.6 Regularización y calibración

Como en regresión lineal, pueden incorporarse penalizaciones L1 y L2 para controlar complejidad. Esto ayuda frente a colinealidad, alta dimensionalidad y sobreajuste.

También es importante recordar que buena discriminación no implica buena calibración. Dos modelos pueden tener AUC parecida y, sin embargo, uno generar probabilidades mucho más confiables. En decisiones de riesgo, esa diferencia es crítica.

### 9.7 Cuándo falla la intuición lineal

Muchos estudiantes interpretan la logística como "una lineal para clasificar". Es una intuición útil a medias. La parte lineal vive en el espacio del logit, no en la probabilidad. Eso significa que el efecto marginal de una feature sobre la probabilidad depende del punto del espacio donde se evalúe.

Comprender este matiz es una de las diferencias entre usar la logística como receta y entenderla de verdad.

## Capítulo 10. KNN, Naive Bayes y SVM: tres lógicas distintas de aprendizaje

### 10.1 K-Nearest Neighbors: aprender por vecindad

KNN es conceptualmente simple: para predecir un caso nuevo, se buscan los \(k\) ejemplos más cercanos en el espacio de features y se decide según ellos. En clasificación, suele usarse voto mayoritario; en regresión, promedio de las respuestas vecinas.

La noción de cercanía suele comenzar con la distancia euclídea:

\[
d(x,x') = \sqrt{\sum_{j=1}^{p}(x_j-x'_j)^2}
\]

pero no es la única posible. En algunos problemas conviene distancia Manhattan, métricas de Minkowski de otro orden o similitudes específicas para texto, secuencias o variables mixtas. Elegir distancia es parte del modelo, no un detalle administrativo.

La idea intuitiva es poderosa: casos parecidos deberían comportarse de manera parecida. Sin embargo, esa idea depende enteramente de cómo se define "parecido". Si las variables están mal escaladas o contienen ruido irrelevante, la cercanía geométrica pierde sentido.

El valor de \(k\) controla un compromiso clásico. Con \(k\) pequeño, el modelo tiene baja rigidez y puede adaptarse a estructuras finas, pero también a ruido local. Con \(k\) grande, el modelo se suaviza, reduce varianza y aumenta sesgo.

El gran límite de KNN es la **maldición de la dimensionalidad**. A medida que el número de dimensiones crece, las distancias tienden a concentrarse y la noción de vecino se vuelve menos informativa.

### 10.2 Naive Bayes: independencia condicional como aproximación útil

Naive Bayes se apoya en el teorema de Bayes:

\[
P(Y\mid X) = \frac{P(X\mid Y)P(Y)}{P(X)}
\]

La versión "naive" supone que, condicionadas a la clase \(Y\), las features son independientes entre sí:

\[
P(X_1,\dots,X_p \mid Y) = \prod_{j=1}^{p} P(X_j\mid Y)
\]

Entonces,

\[
P(Y\mid X_1,\dots,X_p) \propto P(Y)\prod_{j=1}^{p} P(X_j\mid Y)
\]

El supuesto es fuerte y rara vez cierto de manera exacta. Sin embargo, el modelo puede funcionar sorprendentemente bien, especialmente en clasificación de texto, donde la alta dimensionalidad y la dispersión hacen atractiva su simplicidad.

La enseñanza importante aquí es epistemológica: un supuesto obviamente falso puede, aun así, producir un modelo útil si captura suficiente estructura relevante y reduce la complejidad del problema.

### 10.3 SVM: margen máximo y robustez geométrica

Las máquinas de soporte vectorial buscan separar clases mediante un hiperplano que maximice el margen entre los puntos de clases opuestas más cercanos.

Si los datos son linealmente separables, el problema duro consiste en encontrar \(w\) y \(b\) tales que

\[
y_i(w^T x_i + b) \ge 1
\]

para todos los puntos, minimizando al mismo tiempo

\[
\frac{1}{2}\|w\|^2
\]

Minimizar la norma de \(w\) equivale a maximizar el margen. La intuición es que fronteras con margen amplio tienden a generalizar mejor.

Cuando los datos no son perfectamente separables, se introducen variables de holgura \(\xi_i\) y un parámetro \(C\) que controla cuánto penalizamos violaciones del margen:

\[
\min_{w,b,\xi} \frac{1}{2}\|w\|^2 + C\sum_{i=1}^{n}\xi_i
\]

### 10.4 Kernel trick y no linealidad

La potencia de SVM crece con los kernels. Un kernel permite computar productos internos en un espacio transformado de alta dimensión sin construir explícitamente esa transformación. En términos prácticos, eso permite fronteras no lineales con una formulación todavía controlada.

El kernel RBF es muy usado. Allí el parámetro \(\gamma\) controla el alcance local de la influencia de cada punto. Un \(\gamma\) alto produce fronteras muy onduladas y puede sobreajustar. Un \(C\) muy alto fuerza a clasificar correctamente más puntos de entrenamiento y también puede aumentar varianza.

### 10.5 Comparación conceptual entre KNN, Naive Bayes y SVM

Estos tres modelos enseñan tres filosofías de aprendizaje distintas.

KNN delega el conocimiento en los propios datos de entrenamiento y decide por vecindad local. Naive Bayes introduce una simplificación probabilística fuerte para obtener un clasificador eficiente y sorprendentemente robusto en ciertos contextos. SVM define una frontera geométrica de máximo margen y, con kernels, puede capturar no linealidades complejas.

Saber compararlos con rigor es mucho más valioso que memorizar listas de ventajas y desventajas. Hay que entender qué sesgo inductivo trae cada uno y cuándo ese sesgo puede ser una virtud o una limitación.

## Parte VI. Árboles y ensambles: potencia, no linealidad y control de varianza

## Capítulo 11. Árboles de decisión: reglas interpretables con riesgo de inestabilidad

### 11.1 La idea básica de particionar recursivamente

Un árbol de decisión divide el espacio de features mediante preguntas del tipo "¿\(x_j < t\)?". Cada división intenta producir nodos más homogéneos respecto del target. Repetida recursivamente, esta lógica construye una estructura de reglas if-then altamente interpretable.

La gran virtud de los árboles es que capturan no linealidades e interacciones sin requerir transformaciones explícitas complicadas. La gran debilidad es su inestabilidad: pequeñas variaciones en la muestra pueden cambiar bastante la estructura del árbol.

### 11.2 Entropía, información y algoritmo ID3

Una forma clásica de medir impureza en clasificación es la **entropía**:

\[
H(S) = -\sum_{k=1}^{K} p_k \log_2 p_k
\]

donde \(p_k\) es la proporción de la clase \(k\) en el nodo \(S\).

Si un nodo contiene solo una clase, la entropía es cero. Si las clases están muy mezcladas, la entropía es mayor. El algoritmo **ID3** elige la partición que maximiza la **ganancia de información**, definida como la reducción esperada de entropía tras el split.

Veamos una intuición numérica. Supongamos un nodo con 14 observaciones: 9 positivas y 5 negativas. Su entropía es

\[
H(S) = -\frac{9}{14}\log_2\left(\frac{9}{14}\right) - \frac{5}{14}\log_2\left(\frac{5}{14}\right)
\]

que vale aproximadamente 0.94. Si una división genera nodos hijos mucho más puros, la entropía promedio posterior baja y la ganancia de información es alta.

La enseñanza profunda es que el árbol no "elige reglas intuitivas"; elige reglas que maximizan una reducción cuantificada de incertidumbre respecto del target.

### 11.3 Gini, CART y C4.5

Otra medida muy usada de impureza es el índice de **Gini**:

\[
Gini(S) = 1 - \sum_{k=1}^{K} p_k^2
\]

Su interpretación es similar: cuanto menor es el índice, más puro es el nodo. El algoritmo CART suele apoyarse en Gini para clasificación y en reducción de MSE para regresión.

**C4.5**, sucesor de ID3, introduce mejoras relevantes: maneja atributos continuos, utiliza ganancia normalizada mediante **gain ratio** y contempla mecanismos más refinados para lidiar con faltantes y poda.

La idea de gain ratio es importante porque la ganancia de información pura tiende a favorecer atributos con muchos valores posibles. C4.5 corrige parcialmente ese sesgo dividiendo por una medida de dispersión de la partición inducida. En forma esquemática:

\[
\text{GainRatio}(S,A) = \frac{\text{InfoGain}(S,A)}{\text{SplitInfo}(S,A)}
\]

donde \(\text{SplitInfo}(S,A)\) penaliza particiones excesivamente fragmentadas. La lectura conceptual es clara: no alcanza con reducir entropía; también importa no hacerlo mediante divisiones artificialmente "demasiado específicas".

En problemas con variables numéricas, el árbol debe buscar no solo qué variable usar, sino también qué umbral \(t\) produce la mejor separación. Esa búsqueda puede hacerse recorriendo puntos candidatos entre valores ordenados.

### 11.4 Árboles para regresión

En regresión, la lógica es análoga pero la impureza se mide mediante varianza o error cuadrático dentro de cada nodo. El árbol busca splits que reduzcan la dispersión de la respuesta en los hijos.

El resultado final asigna a cada hoja una predicción continua, usualmente la media del target dentro de esa hoja.

### 11.5 Crecimiento, poda y sobreajuste

Si se deja crecer un árbol sin restricciones, suele adaptarse excesivamente al entrenamiento. Termina memorizando detalles accidentales que no generalizan.

Existen dos grandes estrategias de control. La **pre-poda** limita profundidad máxima, mínimo de muestras por hoja o mínimo de mejora por split. La **post-poda** deja crecer más y luego recorta subárboles si su complejidad no se justifica por mejora real.

La lección general es clara: la interpretabilidad del árbol simple tiene un costo potencial en varianza. Para mantenerla útil, hay que controlar complejidad.

### 11.6 Importancia de variables y cautelas

Los árboles ofrecen medidas de importancia basadas en reducción acumulada de impureza. Son útiles, pero no inocentes. Tienden a favorecer variables con muchos posibles puntos de corte o con cardinalidad elevada.

Por eso conviene complementar con **permutation importance**, estabilidad entre reentrenamientos y, sobre todo, lectura sustantiva. Una variable importante para el árbol no es automáticamente una variable causal ni necesariamente una variable sobre la que convenga intervenir.

### 11.7 Cuándo usar árboles

Los árboles son especialmente atractivos cuando importa la interpretabilidad, cuando se esperan interacciones y no linealidades, cuando hay mezcla de variables numéricas y categóricas y cuando se desea un modelo con reglas explícitas.

Su principal limitación es la inestabilidad y, en muchos casos, una performance inferior a la de ensambles bien calibrados. Precisamente por eso los ensambles de árboles ocupan un lugar tan importante en la práctica.

## Capítulo 12. Ensambles: combinar modelos para ganar estabilidad o capacidad

### 12.1 La intuición estadística detrás de los ensambles

Si varios modelos cometen errores diferentes, combinarlos puede reducir error total. Esta idea parece simple, pero su alcance es enorme.

Si \(M\) predictores tienen varianza individual \(\sigma^2\) y correlación promedio \(\rho\), la varianza de su promedio puede escribirse, de manera simplificada, como:

\[
\text{Var}(\bar f) = \rho \sigma^2 + \frac{1-\rho}{M}\sigma^2
\]

La fórmula enseña dos cosas. Promediar ayuda más cuanto menor es la correlación entre modelos. Y, aunque aumentemos \(M\), si todos los modelos se parecen demasiado, la reducción de varianza se estanca.

### 12.2 Bagging

El **bagging** (*bootstrap aggregating*) entrena muchos modelos sobre muestras bootstrap del conjunto de entrenamiento y luego agrega sus predicciones. Su objetivo principal es reducir varianza, especialmente cuando el modelo base es inestable, como un árbol.

La muestra bootstrap se genera tomando \(n\) observaciones con reemplazo a partir del conjunto original de tamaño \(n\). Algunas observaciones se repiten y otras quedan fuera. Estas últimas pueden usarse como validación interna aproximada en el esquema **out-of-bag**.

### 12.3 Random Forest

Random Forest es bagging de árboles con una capa adicional de aleatoriedad: en cada split, el algoritmo considera solo un subconjunto aleatorio de features candidatas.

Esta restricción disminuye la correlación entre árboles. Si siempre se permitiera elegir entre todas las features, una variable muy dominante podría aparecer en casi todos los árboles y reducir la diversidad del ensamble.

El resultado suele ser un modelo robusto, de buen desempeño, relativamente poco sensible a tuning fino y muy competitivo en datos tabulares. Sin embargo, pierde parte de la interpretabilidad directa del árbol individual.

### 12.4 Boosting: aprender corrigiendo errores previos

Mientras bagging trabaja en paralelo para reducir varianza, el **boosting** trabaja de forma secuencial para construir un modelo más fuerte a partir de modelos débiles.

En AdaBoost, los ejemplos mal clasificados van recibiendo mayor peso en iteraciones sucesivas. En Gradient Boosting, cada nuevo árbol se ajusta a los residuos o, más precisamente, a una aproximación del gradiente negativo de la pérdida respecto del modelo actual.

La formulación funcional simplificada es:

\[
F_m(x) = F_{m-1}(x) + \eta h_m(x)
\]

donde \(F_m\) es el ensamble en la iteración \(m\), \(h_m\) es el nuevo modelo débil y \(\eta\) es la tasa de aprendizaje.

Esta ecuación expresa una idea profunda: ya no estamos ajustando directamente un solo modelo paramétrico, sino construyendo gradualmente una función en el espacio de funciones.

### 12.5 XGBoost y regularización moderna en árboles

XGBoost es una implementación especialmente eficiente y regularizada de gradient boosting sobre árboles. Su objetivo puede expresarse esquemáticamente como:

\[
\text{Obj} = \sum_{i=1}^{n} l(y_i, \hat y_i) + \sum_{k=1}^{K}\Omega(f_k)
\]

donde \(l\) es la pérdida sobre cada observación y \(\Omega(f_k)\) penaliza la complejidad de cada árbol del ensamble.

Una forma típica de penalización es:

\[
\Omega(f) = \gamma T + \frac{\lambda}{2}\sum_{j=1}^{T} w_j^2
\]

donde \(T\) es el número de hojas, \(w_j\) son pesos asociados a las hojas, \(\gamma\) penaliza árboles demasiado grandes y \(\lambda\) contrae pesos. La idea no es memorizar esta fórmula, sino comprender que el boosting moderno no solo agrega árboles: también controla explícitamente su complejidad.

### 12.6 Voting, stacking, cascading, ensambles homogéneos e híbridos

No todo ensamble es bagging o boosting.

En **voting**, varios modelos emiten una predicción y se combinan por voto mayoritario o por promedio de probabilidades. Es simple y a menudo efectivo cuando los modelos son complementarios.

En **stacking**, las predicciones de modelos base se convierten en inputs de un meta-modelo que aprende cómo combinarlas. Para que funcione honestamente, las predicciones del nivel base deben generarse fuera de fold, evitando leakage.

En **cascading**, las salidas de un modelo alimentan o condicionan etapas posteriores del sistema. Esta lógica aparece en pipelines complejos, detección en varias etapas o sistemas con filtros sucesivos.

Un ensamble es **homogéneo** cuando combina modelos de la misma familia, como muchos árboles. Es **híbrido** cuando mezcla familias distintas, como logística, SVM y Random Forest.

### 12.7 Performance versus interpretabilidad

Los ensambles suelen mejorar desempeño, pero a costa de perder transparencia inmediata. Esta tensión no debe negarse. Debe gestionarse.

Una respuesta de alto nivel en examen puede decir, por ejemplo: "Prefiero Random Forest frente a un árbol único porque reduce varianza y gana robustez; sin embargo, si la prioridad absoluta es explicar reglas exactas de decisión, un árbol podado puede ser preferible aunque su performance sea menor". Ese tipo de respuesta muestra criterio, no solo conocimiento técnico.

## Parte VII. Aprendizaje no supervisado y reducción de dimensión

## Capítulo 13. Clustering: descubrir estructura sin etiquetas

### 13.1 Qué problema intenta resolver el clustering

En clustering no se trata de "adivinar clases ocultas" de manera mágica. Se trata de construir una partición del espacio de datos tal que observaciones del mismo grupo resulten, según una noción elegida de similitud, más parecidas entre sí que a las de otros grupos.

Esto tiene una consecuencia crucial: los clusters no son entidades ontológicas garantizadas por la realidad. Son construcciones dependientes de la representación, la escala, la métrica y el algoritmo.

### 13.2 Distancia, escala y significado de "parecido"

Antes de agrupar, hay que definir una distancia o similitud. En variables numéricas suele aparecer la distancia euclídea, pero no siempre es la adecuada. En datos mixtos o categóricos pueden requerirse otras nociones. Si las variables no están escaladas, una de gran magnitud puede dominar la distancia.

Por eso el clustering obliga a pensar con especial claridad la representación. Agrupar sobre variables mal escaladas o con ruido dominante produce clusters aparentes pero conceptualmente vacíos.

### 13.3 Tendencia al clustering y estadístico de Hopkins

Antes de elegir un algoritmo, puede ser útil preguntarse si el dataset muestra alguna tendencia a formar agrupamientos genuinos o si se parece más a una nube aleatoria.

El **estadístico de Hopkins** compara distancias entre puntos reales y distancias desde puntos artificiales aleatorios hacia la nube real. Intuitivamente, si la estructura de datos contiene agrupamientos, los puntos reales tenderán a estar más cerca entre sí que los puntos aleatorios respecto del dataset. Valores cercanos a 1 sugieren tendencia a clustering; valores alrededor de 0.5 sugieren estructura más aleatoria.

No es un veredicto absoluto, pero sí una advertencia útil contra el impulso de agrupar por inercia.

### 13.4 K-means en profundidad

K-means busca particionar las observaciones en \(K\) grupos minimizando la suma de cuadrados intra-cluster:

\[
\min_{C_1,\dots,C_K} \sum_{k=1}^{K}\sum_{x_i \in C_k}\|x_i - \mu_k\|^2
\]

donde \(\mu_k\) es el centroide del cluster \(C_k\).

El algoritmo alterna dos pasos:

1. asignar cada punto al centroide más cercano;
2. recomputar cada centroide como el promedio de los puntos asignados.

Ese procedimiento repite hasta converger a un mínimo local. Es importante subrayar "local". K-means no garantiza el óptimo global. La inicialización importa, y por eso conviene múltiples reinicios o estrategias como k-means++.

### 13.5 Un ejemplo mínimo para entender K-means

Supongamos puntos unidimensionales: \(1, 2, 3, 10, 11, 12\), y queremos \(K=2\). Si inicializamos centroides en 2 y 11, las asignaciones serán naturalmente \(\{1,2,3\}\) y \(\{10,11,12\}\). Los centroides nuevos serán 2 y 11, y el algoritmo convergerá de inmediato.

Pero si iniciáramos centroides en 3 y 10, también convergería bien. Si los puntos fueran menos separados o hubiera outliers, distintas inicializaciones podrían llevar a soluciones diferentes. Este ejemplo simple enseña el corazón del método: K-means no "descubre" grupos; optimiza una función bajo una geometría concreta.

### 13.6 Elegir \(K\): codo, silhouette y estabilidad

Elegir el número de clusters no es un acto mágico. El método del **codo** observa la inercia intra-cluster al aumentar \(K\) y busca un punto donde la mejora marginal se aplana. El índice **silhouette** evalúa cohesión y separación relativas. El análisis de estabilidad examina cuánto cambian los clusters bajo remuestreo o variaciones leves.

Ninguno de estos criterios sustituye al dominio. Puede haber un \(K\) matemáticamente plausible pero inútil para la acción posterior. También puede haber varios \(K\) defendibles según el nivel de granularidad que interese.

### 13.7 Clustering jerárquico

El clustering jerárquico no fija \(K\) al inicio. Construye una estructura de fusiones o divisiones sucesivas que puede representarse con un dendrograma.

Las variantes aglomerativas parten de clusters unitarios y van fusionando según una regla de enlace: single, complete, average, Ward, entre otras. Cada regla produce geometrías y sensibilidades distintas. Single linkage puede encadenar puntos a través de puentes; complete linkage favorece grupos compactos; Ward tiende a minimizar aumento de varianza intra-cluster.

Su gran fortaleza es la visión multiescala. Su desventaja principal, en datasets grandes, es el costo computacional.

### 13.8 DBSCAN: densidad, ruido y formas arbitrarias

DBSCAN define clusters como regiones densas separadas por regiones menos densas. Requiere dos parámetros: \(\varepsilon\), radio de vecindad, y \(minPts\), cantidad mínima de puntos para considerar que existe densidad suficiente.

Su ventaja es notable: detecta clusters de forma arbitraria y etiqueta ruido explícitamente. No necesita fijar \(K\). Su dificultad aparece cuando la densidad varía mucho entre regiones o cuando elegir \(\varepsilon\) no es evidente.

### 13.9 Interpretar clusters y evitar errores frecuentes

Una vez obtenidos los clusters, comienza la parte más delicada: interpretarlos sin inventar historias arbitrarias. Conviene describir centroides o perfiles, comparar variables distintivas, analizar tamaño relativo de los grupos y preguntarse si esa segmentación habilita acciones diferentes.

Errores frecuentes:

1. creer que todo cluster encontrado es real y estable;
2. comparar clusters construidos sobre variables mal escaladas;
3. elegir \(K\) solo por un índice sin pensar en utilidad;
4. confundir clusters con clases verdaderas del fenómeno.

## Capítulo 14. Reducción de dimensionalidad: comprimir sin perder estructura esencial

### 14.1 Por qué reducir dimensión

La reducción de dimensionalidad no existe solo para "dibujar en 2D". Responde a varios problemas:

1. costos computacionales elevados;
2. ruido distribuido en muchas variables;
3. colinealidad;
4. dificultad de visualización;
5. degradación de métodos basados en distancias;
6. riesgo de sobreajuste cuando la complejidad efectiva del espacio es alta respecto del tamaño muestral.

La reducción puede ser una herramienta de compresión, visualización, desruido o construcción de representación.

### 14.2 Maldición de la dimensionalidad

Cuando la dimensión crece, muchas intuiciones geométricas de baja dimensión fallan. Los puntos tienden a parecer todos lejanos entre sí, los volúmenes crecen de forma explosiva y la noción de vecindad pierde nitidez. Por eso algoritmos como KNN o ciertos procedimientos de clustering sufren especialmente en espacios de alta dimensión.

Reducir dimensión no elimina mágicamente este problema, pero puede mitigarlo si la estructura relevante del dato vive cerca de una subvariedad o de un subespacio de menor dimensión efectiva.

### 14.3 PCA: idea, formulación y geometría

El análisis de componentes principales (PCA) busca nuevas direcciones ortogonales que capturen la mayor varianza posible.

Si \(X\) es la matriz de datos centrada, la primera componente principal es el vector \(w_1\) que resuelve:

\[
\max_{\|w\|=1} \text{Var}(Xw)
\]

Equivalente a maximizar:

\[
w^T \Sigma w
\]

donde \(\Sigma\) es la matriz de covarianza.

Con un multiplicador de Lagrange se obtiene que \(w\) debe ser un autovector de \(\Sigma\). El asociado al mayor autovalor define la primera componente. Las componentes siguientes repiten el problema imponiendo ortogonalidad respecto de las anteriores.

Esta formulación enseña algo profundo: PCA no busca variables "importantes" en sentido predictivo, sino direcciones de máxima varianza global.

### 14.4 Scores, loadings y varianza explicada

Los **loadings** indican cómo se combinan las variables originales en cada componente. Los **scores** son las coordenadas de cada observación proyectada en esas nuevas direcciones.

Si los autovalores son \(\lambda_1,\dots,\lambda_p\), la proporción de varianza explicada por la componente \(j\) es:

\[
\frac{\lambda_j}{\sum_{m=1}^{p}\lambda_m}
\]

El **scree plot** grafica esas proporciones o la varianza acumulada y ayuda a decidir cuántas componentes conservar. Pero no existe un umbral mágico universal. Conservar 95% de varianza puede ser razonable en un contexto y excesivo o insuficiente en otro.

### 14.5 Qué preserva PCA y qué no

PCA preserva, en el mejor sentido posible bajo restricción lineal y ortogonal, varianza global. No preserva necesariamente separabilidad de clases, estructura no lineal ni interpretabilidad sustantiva. Una variable de baja varianza puede ser altamente predictiva y quedar relegada por PCA. Por eso reducir dimensión antes de supervisar requiere criterio.

También es importante recordar que PCA depende de la escala. Si una variable tiene varianza enorme solo por unidades, dominará las primeras componentes si no se estandariza apropiadamente.

### 14.6 Relación entre PCA, ruido, colinealidad y overfitting

Cuando muchas variables están fuertemente correlacionadas, PCA puede compactar esa redundancia en pocas componentes. Eso puede estabilizar modelos posteriores, reducir ruido y atenuar problemas de colinealidad.

Sin embargo, la ganancia en estabilidad puede venir con pérdida de interpretabilidad. Las componentes son combinaciones lineales, no variables originales fácilmente comunicables. Esta tensión entre compresión e interpretación debe ser explicitada.

### 14.7 MDS: preservar distancias

El **multidimensional scaling** (MDS) parte de una matriz de distancias o disimilitudes y busca una configuración en baja dimensión que las reproduzca lo mejor posible. La idea no es maximizar varianza, sino preservar relaciones de distancia.

Esto lo vuelve conceptualmente distinto de PCA. Si lo importante son proximidades entre observaciones más que direcciones de máxima variación, MDS puede ser más adecuado.

En su versión métrica, una formulación frecuente consiste en minimizar una medida de stress, por ejemplo:

\[
\text{Stress} = \sqrt{\frac{\sum_{i<j}(d_{ij}-\delta_{ij})^2}{\sum_{i<j}\delta_{ij}^2}}
\]

donde \(\delta_{ij}\) son las disimilitudes originales y \(d_{ij}\) las distancias en el espacio reducido. La fórmula expresa con nitidez el problema: encontrar una configuración de baja dimensión que distorsione lo menos posible las relaciones originales.

### 14.8 t-SNE: vecindades locales para visualización

t-SNE es una técnica no lineal orientada principalmente a visualización. Intenta preservar vecindades locales: puntos cercanos en alta dimensión deberían permanecer relativamente cercanos en baja dimensión.

Su poder visual es grande, pero también lo son sus riesgos de malinterpretación. Distancias globales entre grupos en el mapa t-SNE no deben leerse como si fueran métricamente fieles. La técnica prioriza estructura local, no geometría global exacta.

Además, hiperparámetros como la perplexity influyen notablemente en el resultado. Esto significa que un gráfico t-SNE puede ser informativo, pero no debe tomarse como prueba autosuficiente de "cuántos clusters hay".

### 14.9 ISOMAP: variedades y distancias geodésicas

ISOMAP parte de la idea de que los datos pueden vivir sobre una variedad no lineal embebida en un espacio de alta dimensión. En lugar de preservar distancias euclídeas directas, intenta preservar distancias geodésicas aproximadas sobre esa variedad.

Cuando esa hipótesis es razonable, ISOMAP puede desenrollar estructuras no lineales que PCA no capta. Su costo es que depende de un grafo de vecinos bien construido y puede ser sensible a ruido o desconexiones.

### 14.10 Cómo comparar PCA, MDS, t-SNE e ISOMAP

PCA prioriza varianza global lineal. MDS prioriza preservación de distancias. t-SNE prioriza vecindades locales para visualización. ISOMAP prioriza geometría de variedad mediante distancias geodésicas.

Confundir estas técnicas como si todas "sirvieran para bajar columnas" es un error conceptual serio. La pregunta correcta siempre es: qué estructura quiero preservar y para qué necesito la representación reducida.

## Parte VIII. Redes neuronales y representación de texto

## Capítulo 15. Redes neuronales: de la linealidad a representaciones jerárquicas

### 15.1 El perceptrón simple

La neurona artificial más básica calcula una combinación lineal de las entradas y luego aplica una función de activación:

\[
a = \phi(w^T x + b)
\]

donde \(w\) es el vector de pesos, \(b\) es el sesgo y \(\phi\) es una no linealidad como sigmoide, tanh o ReLU.

El perceptrón simple puede verse como un clasificador lineal. Aprende un hiperplano que separa clases si esa separación es posible. Históricamente es importante porque muestra cómo un mecanismo de ajuste puede aprender a partir de errores.

Una regla clásica de actualización, en versión simplificada, es:

\[
w \leftarrow w + \eta(y-\hat y)x
\]

donde \(\eta\) es la tasa de aprendizaje. La lectura conceptual es directa: si el ejemplo se clasificó mal, los pesos se corrigen en la dirección que haría más compatible la salida con la etiqueta observada.

### 15.2 La limitación del perceptrón y el problema XOR

Un punto pedagógico imprescindible es entender por qué el perceptrón simple no puede resolver ciertos problemas, como XOR. La razón no es contingente: XOR no es linealmente separable.

Este ejemplo enseña una idea decisiva. Componer transformaciones lineales sin introducir no linealidades sigue produciendo una transformación lineal. Por eso las redes neuronales necesitan funciones de activación no lineales entre capas.

### 15.3 Perceptrón multicapa y composición de funciones

Un MLP apila capas de la forma

\[
h^{(1)} = \phi(W^{(1)}x + b^{(1)}), \quad
h^{(2)} = \phi(W^{(2)}h^{(1)} + b^{(2)}), \quad \dots
\]

hasta llegar a una salida final \(\hat y\).

La intuición profunda es que cada capa construye una representación nueva del dato. Las capas tempranas capturan patrones relativamente simples; las siguientes combinan esos patrones en estructuras más abstractas. Esta idea de representación jerárquica es una de las claves del aprendizaje profundo.

### 15.4 Función de pérdida y descenso por gradiente

Como en otros modelos, entrenar una red significa minimizar una pérdida \(\mathcal{L}(\theta)\), donde \(\theta\) reúne todos los pesos y sesgos.

Una actualización básica por descenso por gradiente es:

\[
\theta_{t+1} = \theta_t - \eta \nabla_{\theta}\mathcal{L}(\theta_t)
\]

donde \(\eta\) es la tasa de aprendizaje.

La tasa de aprendizaje merece interpretación. Si es demasiado grande, el entrenamiento puede oscilar o divergir. Si es demasiado pequeña, el avance puede ser extremadamente lento o quedar atrapado en mesetas.

### 15.5 Backpropagation: por qué funciona

Backpropagation aplica repetidamente la regla de la cadena para calcular gradientes de la pérdida respecto de todos los parámetros de la red de manera eficiente.

No conviene vivirlo como una receta misteriosa. Si la salida depende de una capa oculta, y esa capa depende de otra anterior, entonces el efecto de un peso temprano sobre la pérdida debe propagarse a través de todas esas dependencias. Backprop es, precisamente, la contabilidad organizada de esas derivadas compuestas.

Este es uno de los puntos que más cuesta en examen porque muchos estudiantes recuerdan el nombre del algoritmo pero no pueden explicar qué problema resuelve: evitar recalcular de forma ingenua derivadas locales para cada peso de manera computacionalmente inviable.

### 15.6 Optimizadores: SGD, momentum y Adam

El descenso por gradiente puede implementarse sobre todo el dataset, por lotes pequeños o incluso observación por observación. En la práctica, los mini-batches suelen ofrecer un buen equilibrio.

Los optimizadores modernos modifican la dinámica básica. **Momentum** acumula dirección para suavizar oscilaciones. **RMSProp** y **Adam** adaptan la magnitud de actualización según escalas recientes del gradiente. No cambian el objetivo de fondo, pero sí la manera de recorrer el paisaje de optimización.

### 15.7 Problemas de entrenamiento y regularización

Las redes profundas pueden sufrir gradientes que se desvanecen o explotan, sobreajuste, alta sensibilidad a inicialización y dependencia de hiperparámetros.

Para combatir estos problemas aparecen herramientas como:

1. activaciones ReLU y variantes;
2. inicializaciones cuidadosas;
3. batch normalization;
4. weight decay;
5. dropout;
6. early stopping;
7. data augmentation.

Cada una responde a un problema concreto. Dropout introduce ruido estructurado para evitar coadaptaciones excesivas. Early stopping detiene el entrenamiento cuando la validación deja de mejorar. Weight decay penaliza pesos grandes.

### 15.8 SOM: mapas autoorganizados

Los **Self-Organizing Maps** (SOM) son redes no supervisadas que proyectan datos de alta dimensión sobre una grilla bidimensional preservando, en cierta medida, proximidad topológica. Su interés pedagógico es grande porque muestran otra faceta de las redes: no solo clasificación supervisada, sino organización espacial de estructura latente.

Aunque hoy son menos dominantes que otros métodos en muchas aplicaciones, siguen siendo conceptualmente valiosos para entender representación y topología.

### 15.9 Cuándo usar deep learning y cuándo no

El aprendizaje profundo brilla especialmente con grandes volúmenes de datos no estructurados, como imágenes, audio y lenguaje, y cuando la estructura relevante exige representaciones jerárquicas complejas.

En muchos datasets tabulares medianos, ensambles de árboles pueden competir o superar a redes con menor costo, menor tuning y mayor interpretabilidad. Elegir deep learning por prestigio y no por necesidad es un error frecuente.

## Capítulo 16. NLP aplicado: del texto crudo a una representación útil

### 16.1 Por qué el texto es un dato especial

El texto plantea un desafío singular: llega como secuencia simbólica, no como vector numérico. Además, está lleno de ambigüedad, contexto, ironía, dependencia sintáctica, variación morfológica y ruido.

La primera tarea en NLP es construir una representación que preserve suficiente información para la tarea objetivo. Ese paso es tan importante como el modelo final.

### 16.2 Pipeline clásico de texto

Un pipeline típico incluye tokenización, normalización, eventualmente eliminación de stopwords, stemming o lematización, construcción de n-gramas y vectorización.

Sin embargo, no existe un preprocesamiento universalmente correcto. Eliminar negaciones puede destruir señal en análisis de sentimiento. Un stemming agresivo puede colapsar palabras de sentido distinto. Mantener signos de puntuación puede ser irrelevante en una tarea y crucial en otra.

### 16.3 Bag of Words

En **Bag of Words** cada documento se representa por el conteo de términos del vocabulario. Se pierde el orden, pero se conserva frecuencia.

Esta simplificación parece brutal, y de hecho lo es. Sin embargo, suele funcionar sorprendentemente bien en tareas de clasificación de texto porque muchas señales discriminativas están en la presencia o ausencia de ciertas palabras o expresiones.

La lección es importante: una representación simple puede ser muy eficaz si retiene la estructura verdaderamente relevante para la tarea.

### 16.4 TF-IDF

TF-IDF refina Bag of Words ponderando los términos por frecuencia local y rareza global:

\[
\text{tfidf}(t,d) = \text{tf}(t,d)\cdot \log\left(\frac{N}{df(t)}\right)
\]

donde \(t\) es un término, \(d\) un documento, \(N\) el número total de documentos y \(df(t)\) la cantidad de documentos en los que aparece el término.

La intuición es muy importante. Si una palabra aparece mucho en un documento pero también aparece en casi todos los documentos, aporta poca discriminación. Si aparece mucho en un documento y poco en el resto del corpus, es probablemente informativa.

### 16.5 Naive Bayes multinomial en texto

En clasificación de texto, Naive Bayes multinomial modela conteos o frecuencias de términos por clase. Aunque el supuesto de independencia entre palabras es claramente simplificador, suele rendir bien porque explota muy bien la estructura dispersa de la representación.

Además, ofrece una interpretación pedagógica valiosa: cada palabra contribuye, aproximadamente, como evidencia multiplicativa a favor o en contra de una clase.

### 16.6 Lexicones y análisis de sentimiento

Una aproximación clásica al análisis de sentimiento usa diccionarios o **lexicones** que asignan polaridad o intensidad emocional a palabras. Esta estrategia es simple y puede ser útil como baseline o complemento, pero tiene límites fuertes: no maneja bien contexto, negación, ironía ni polisemia.

Una frase como "no estuvo nada mal" muestra por qué un enfoque puramente lexical puede fallar si no trata adecuadamente la negación.

### 16.7 Texto para clasificación y para regresión

El texto no sirve solo para clasificar. También puede alimentar problemas de regresión. Un ejemplo típico es predecir **story points** a partir de descripciones de tickets o historias de usuario. Allí el desafío no es solo textual, sino también de definición del target: ¿se quiere una regresión sobre puntos reales? ¿una clasificación ordinal por rangos? ¿un ranking de complejidad relativa?

Este tipo de problemas es pedagógicamente valioso porque obliga a integrar representación textual, elección del target, métrica adecuada y validación honesta.

### 16.8 Embeddings y representaciones distribuidas

Los embeddings densos, como Word2Vec o GloVe, representan palabras en espacios continuos donde la proximidad geométrica captura cierta similitud semántica. Los modelos contextualizados, como BERT y derivados, van más lejos: la representación de una palabra depende del contexto en que aparece.

Aunque el foco clásico de la materia puede estar en BoW, TF-IDF y Naive Bayes, conviene entender que la historia general del NLP es una historia de representaciones cada vez más expresivas.

### 16.9 Riesgos y sesgos en NLP

El lenguaje arrastra sesgos sociales, culturales e históricos. Un modelo entrenado sobre corpus sesgados puede reproducir o amplificar estereotipos. Además, textos breves, ironía, jergas o cambios de dominio pueden degradar gravemente el desempeño.

Por eso el NLP serio no termina en una métrica promedio. Requiere análisis de errores, revisión cualitativa de ejemplos y, cuando corresponde, evaluación por subgrupos o dominios específicos.

## Parte IX. Extensiones metodológicas y criterio profesional

## Capítulo 17. Series temporales: cuando el tiempo no puede ignorarse

### 17.1 Qué cambia cuando hay dependencia temporal

En una serie temporal, las observaciones están ordenadas y suelen depender unas de otras. Esto rompe la comodidad del supuesto i.i.d. que subyace a muchos métodos estándar.

Si se mezclan temporalmente los datos al azar, puede filtrarse información del futuro al pasado. La evaluación resultante será engañosa porque el sistema real nunca dispondrá de ese futuro al momento de predecir.

### 17.2 Tendencia, estacionalidad, ciclo y ruido

Una serie suele pensarse como combinación de varios componentes:

1. tendencia de largo plazo;
2. estacionalidad periódica;
3. ciclos de frecuencia menos rígida;
4. ruido o variación no explicada.

Esta descomposición no es solo descriptiva. Ayuda a decidir qué features temporales construir y qué tipo de modelo puede ser razonable.

También conviene recordar el valor de los baselines ingenuos en series temporales. Predecir \(y_t\) usando \(y_{t-1}\), repetir el último valor observado o usar el promedio estacional histórico puede parecer rudimentario, pero ofrece un punto de comparación indispensable. Si un modelo sofisticado no supera con claridad a un baseline temporal razonable, la complejidad extra probablemente no está justificada.

### 17.3 Lags, rolling windows y features temporales

Una feature temporal típica es un **lag**, como \(y_{t-1}\) o \(y_{t-7}\). También se usan medias móviles, máximos recientes, diferencias, indicadores de día de semana, mes, feriado y otras señales de calendario.

La regla crítica es que toda feature construida en el tiempo \(t\) debe depender solo de información disponible hasta \(t\). Esto parece obvio, pero muchas fugas temporales nacen de no respetar esta causalidad operativa mínima.

### 17.4 Evaluación temporal

La validación temporal debe imitar el uso real. Los esquemas walk-forward entrenan sobre una ventana histórica y validan en un bloque futuro, repitiendo el proceso con ventanas crecientes o deslizantes.

Un modelo temporal puede rendir muy bien dentro de un período y degradarse luego por cambios en comportamiento, estacionalidad o contexto económico. Por eso la estabilidad temporal importa tanto como el score promedio.

## Capítulo 18. Interpretabilidad, MLOps, sesgo y gobernanza

### 18.1 Interpretabilidad intrínseca y explicabilidad post hoc

No todos los modelos son igual de transparentes. En modelos lineales y árboles pequeños, la estructura misma del modelo ofrece interpretabilidad relativamente directa. En ensambles complejos y redes profundas, esa transparencia se pierde y aparecen técnicas **post hoc** de explicabilidad.

La distinción es importante. Un modelo intrínsecamente interpretable permite leer directamente por qué decide. Una explicación post hoc aproxima o resume comportamientos del modelo, pero no lo vuelve mágicamente simple.

### 18.2 Importancia global y explicación local

La **importancia global** intenta responder qué variables pesan más en el comportamiento general del modelo. Las explicaciones **locales** intentan responder por qué un caso particular recibió cierta predicción.

Ambas son útiles, pero deben leerse con cautela. Una explicación local no se generaliza automáticamente. Una importancia alta no prueba causalidad. En presencia de colinealidad, distintas variables pueden turnarse como "explicadoras" sin que ello implique diferencias sustantivas reales.

Herramientas como PDP, permutation importance, LIME o SHAP pueden ser muy valiosas si se entienden sus supuestos. PDP promedia efectos parciales y puede volverse engañoso cuando las variables están fuertemente correlacionadas. LIME construye aproximaciones locales simples alrededor de una observación. SHAP reparte contribuciones bajo una lógica inspirada en valores de Shapley. Ninguna de estas herramientas transforma una explicación en prueba causal; todas describen, bajo distintos supuestos, cómo parece comportarse el modelo.

### 18.3 Comunicar resultados con honestidad técnica

Un informe serio no dice solo "el modelo obtuvo 0.89 de AUC". Debe explicar sobre qué datos se evaluó, cómo se separaron entrenamiento y prueba, qué métricas secundarias se observaron, qué costo de error se privilegió, qué limitaciones persisten y dónde el modelo falla.

La comunicación es parte de la competencia técnica. Un modelo bien construido pero mal comunicado puede inducir decisiones peores que un modelo más simple y mejor explicado.

### 18.4 Reproducibilidad y ciclo de vida del modelo

Una solución de ciencia de datos no termina en un notebook. Debe poder reproducirse, auditarse y mantenerse.

Reproducibilidad significa, entre otras cosas, poder reconstruir:

1. la versión de datos utilizada;
2. el código exacto;
3. los hiperparámetros;
4. las métricas obtenidas;
5. el artefacto final del modelo.

Sin esto, comparar experimentos o investigar fallas se vuelve muy difícil.

### 18.5 Drift: cuando el mundo cambia

Tras el despliegue, el problema sigue moviéndose. Puede cambiar la distribución de las features (**data drift**) o la relación entre features y target (**concept drift**).

Ambos fenómenos degradan modelos. Por eso el monitoreo no es un lujo. Es una condición para que un sistema predictivo siga siendo confiable.

### 18.6 Fuentes de sesgo algorítmico

El sesgo puede entrar por múltiples vías: muestreo, etiquetado, variables proxy, historia social previa, definición del target o uso del modelo fuera del dominio de entrenamiento.

Un modelo puede tener excelente performance promedio y dañar sistemáticamente a subgrupos específicos. La evaluación responsable exige mirar métricas segmentadas, estabilidad por población y consecuencias reales de los errores.

Algunas nociones introductorias de equidad son la **paridad demográfica**, que compara tasas globales de decisión positiva entre grupos, y la **igualdad de oportunidad**, que compara recall entre grupos en la clase positiva. El punto importante no es memorizar etiquetas, sino entender que distintas nociones de equidad pueden entrar en tensión y que no existe una definición universalmente correcta fuera del contexto normativo y operativo concreto.

### 18.7 Gobernanza mínima responsable

La gobernanza de modelos incluye documentación, trazabilidad, criterios de aprobación, revisión humana cuando corresponde, monitoreo continuo y canales de auditoría.

No es un apéndice moral desvinculado de la técnica. Es parte de garantizar que los modelos se usen dentro de sus condiciones de validez.

## Capítulo 19. Ejemplo integrador y estrategia de estudio para dominio experto

### 19.1 Un ejemplo completo: predicción de morosidad

Consideremos un problema integral: predecir morosidad en créditos de consumo.

La formulación rigurosa podría ser: "Para cada solicitud aprobada en el momento de originación, estimar la probabilidad de incurrir en mora superior a 90 días dentro de los próximos 12 meses, usando exclusivamente información disponible al momento de otorgamiento". Esta frase ya fija unidad de análisis, target, horizonte temporal y restricción de información.

El EDA debería revisar calidad de variables económicas, faltantes en ingreso, estabilidad temporal de fuentes laborales, desbalance del target y posibles fugas, por ejemplo variables creadas tras el otorgamiento.

El preprocesamiento podría incluir imputación segmentada de ingreso, indicador de faltante, one-hot para categóricas moderadas, target encoding cuidadosamente validado para alta cardinalidad y escalado para modelos sensibles.

La evaluación probablemente no debería descansar solo en accuracy. Si la mora es rara, AUC-PR, recall en ciertos umbrales y calibración pueden ser más relevantes. El umbral de decisión, además, debe alinearse con la política de riesgo.

Podría compararse una logística regularizada como baseline, un Random Forest y un XGBoost. La logística ofrecería interpretabilidad y calibración razonable; Random Forest, robustez; XGBoost, potencia predictiva con tuning cuidadoso. La decisión final no sería solo "quién gana en una métrica", sino qué compromiso ofrecen entre desempeño, estabilidad, explicabilidad y costo operativo.

Este ejemplo resume la arquitectura completa de la materia: formular, leer datos, representar, validar, modelar, interpretar y decidir.

### 19.2 Qué debería poder hacer un estudiante al terminar

Un dominio serio de la materia implica poder:

1. formular con precisión un problema de clasificación, regresión o clustering;
2. detectar leakage antes de modelar;
3. justificar una estrategia de preprocesamiento según el tipo de variable y el modelo;
4. elegir métricas coherentes con el costo del error;
5. explicar la lógica profunda de modelos lineales, árboles, ensambles, clustering, PCA y redes;
6. distinguir predicción de causalidad;
7. argumentar ventajas, límites y trade-offs de distintas técnicas;
8. leer críticamente resultados en lugar de celebrar scores sin contexto.

### 19.3 Cómo suelen evaluar los exámenes

Los exámenes de esta materia suelen premiar respuestas comparativas, precisas y justificadas. No alcanza con definir precision o bagging de memoria. Hay que explicar cuándo importan, con qué problema se conectan, qué error resuelven y qué límites tienen.

Preguntas frecuentes, explícitas o implícitas, son:

1. por qué este problema es clasificación y no regresión;
2. qué métrica conviene y por qué;
3. por qué hubo o no leakage;
4. qué diferencia conceptual existe entre bagging y boosting;
5. qué preserva PCA y por qué no es lo mismo que t-SNE;
6. por qué accuracy puede engañar en datos desbalanceados;
7. cómo interpretar un coeficiente logístico;
8. por qué un árbol profundo sobreajusta;
9. qué exige una validación temporal correcta.

Responder bien exige estructura argumentativa. Una respuesta excelente suele hacer cuatro cosas: definir con precisión, motivar el concepto, explicarlo formalmente cuando corresponde e ilustrarlo con un ejemplo o contraejemplo.

### 19.4 Errores que más suelen costar

Los errores más costosos, académica y profesionalmente, son bastante estables:

1. elegir algoritmo antes de formular el problema;
2. usar métricas por costumbre y no por coherencia con el costo de error;
3. preprocesar antes del split e introducir leakage;
4. confundir correlación con causalidad;
5. interpretar clusters como verdades naturales;
6. usar PCA, t-SNE e ISOMAP como si preservaran la misma estructura;
7. creer que más complejidad siempre implica mejor generalización;
8. reportar una única métrica sin análisis de estabilidad, calibración o subgrupos.

### 19.5 Cómo estudiar esta materia de verdad

La mejor estrategia de estudio combina tres capas.

La primera es la **capa intuitiva**: poder explicar cada concepto en lenguaje claro. Si no se puede explicar sin notación, todavía no se entiende del todo.

La segunda es la **capa formal**: poder escribir las fórmulas esenciales, definir cada símbolo y explicar qué problema resuelve esa formulación.

La tercera es la **capa aplicada**: poder decidir cuándo usar una técnica, cuándo no, qué supuestos la sostienen, cómo se evalúa y qué errores comete un principiante.

Un entrenamiento eficaz consiste en tomar cada tema y contestar, por escrito o en voz alta:

1. qué problema obliga a introducirlo;
2. qué intuición inicial lo justifica;
3. cómo se formula con precisión;
4. cómo se interpreta;
5. qué ejemplos lo aclaran;
6. en qué falla una comprensión ingenua;
7. con qué temas anteriores y posteriores se conecta.

Cuando un estudiante puede hacer eso de manera sostenida, ya no está repasando. Está dominando la materia.

## Cierre

La ciencia de datos se vuelve verdaderamente comprensible cuando deja de verse como un catálogo de algoritmos y empieza a verse como una arquitectura intelectual coherente. Primero se formula bien el problema. Luego se examina críticamente el dato. Después se construye una representación adecuada, se elige un modelo compatible con esa representación, se valida sin autoengaño, se interpreta lo obtenido y se decide con responsabilidad.

Cada módulo de esta materia ocupa un lugar en esa arquitectura. Los fundamentos definen el lenguaje del problema. El EDA y el preprocesamiento construyen el espacio donde el aprendizaje será posible o imposible. La evaluación protege contra conclusiones ilusorias. Los modelos lineales enseñan forma, interpretación y regularización. Árboles y ensambles introducen no linealidad, estabilidad y trade-offs. Clustering y reducción de dimensionalidad muestran cómo buscar estructura sin etiquetas o comprimirla con criterio. Las redes y el NLP enseñan que representar bien puede ser más importante que escoger un algoritmo de moda. La interpretabilidad, el MLOps y la gobernanza recuerdan que un modelo útil no termina en un score.

Si al finalizar la lectura el estudiante puede tomar un problema nuevo y preguntarse, con rigor, qué se quiere predecir o comprender, con qué datos, bajo qué supuestos, con qué riesgos de error, con qué métrica, con qué validación y con qué límites, entonces habrá alcanzado la meta central de la materia. No solo habrá aprendido técnicas. Habrá aprendido a pensar científicamente con datos.
