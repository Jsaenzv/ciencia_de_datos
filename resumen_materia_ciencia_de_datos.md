# Tratado de Ciencia de Datos: fundamentos, modelado, evaluación y criterio profesional

## Prólogo

La ciencia de datos suele aprenderse mal por dos razones opuestas. A veces se la reduce a un catálogo de algoritmos, como si dominar la materia consistiera en memorizar cuándo usar regresión lineal, Random Forest o redes neuronales. Otras veces se la vuelve una práctica puramente instrumental: limpiar columnas, correr un pipeline, mirar una métrica y pasar al siguiente caso. En ambos extremos se pierde lo esencial. La disciplina no empieza en el algoritmo ni termina en el score. Empieza en una pregunta sobre un fenómeno real, atraviesa una serie de decisiones de representación y culmina en una inferencia que alguien utilizará para actuar bajo incertidumbre.

Esa secuencia exige mucho más que familiaridad con librerías. Entre el fenómeno y el dato hay instrumentos de medición, definiciones operativas, procesos administrativos, sesgos históricos y recortes temporales. Entre el dato y el modelo hay decisiones de granularidad, de variables, de imputación, de transformación y de criterio de error. Entre el modelo y la decisión final hay umbrales, costos, restricciones operativas, límites de interpretación y riesgos éticos. Quien no vea esa arquitectura confundirá facilidad de uso con comprensión.

Este tratado parte de una convicción pedagógica precisa: un tema difícil no debe simplificarse hasta volverse trivial, sino reconstruirse hasta volverse inteligible. Por eso cada capítulo avanza del problema a la intuición, de la intuición a la formulación, y de la formulación a la interpretación. La matemática aparece cuando hace falta, no como ornamento. Cada fórmula relevante va acompañada por una lectura conceptual, porque la notación sin comprensión produce una ilusión de rigor, y la intuición sin formalización produce vaguedad.

La meta no es solo aprobar una materia. Es adquirir una forma de pensar. Quien estudie bien estas páginas debería poder tomar un problema nuevo, traducirlo a un lenguaje modelable, leer críticamente los datos disponibles, elegir una estrategia de representación y validación coherente, interpretar los resultados con honestidad y reconocer con claridad qué sabe, qué no sabe y qué no puede afirmar. Ese es, en sentido fuerte, el oficio de la ciencia de datos.

## Parte I. Fundamentos: qué problema resuelve realmente la ciencia de datos

Antes de hablar de modelos conviene fijar el terreno. La ciencia de datos no trabaja directamente sobre la realidad, sino sobre rastros parciales de esa realidad. Por eso esta primera parte no se ocupa de algoritmos todavía, sino del lenguaje básico de la disciplina: qué es un dato, qué es una variable, qué significa formular bien un problema y por qué una buena evaluación empieza mucho antes del entrenamiento.

## Capítulo 1. La ciencia de datos como disciplina de representación y decisión

### 1.1 Qué es y qué no es la ciencia de datos

La definición más repetida dice que la ciencia de datos extrae conocimiento de los datos. La frase es correcta, pero demasiado pobre. Sugiere que el conocimiento ya estuviera intacto dentro del dataset y que bastara aplicar una técnica para revelarlo. En realidad, los datos no hablan por sí solos. Son registros producidos por sensores, formularios, sistemas transaccionales, decisiones humanas, convenciones administrativas y contextos históricos. Trabajar con datos no es mirar una realidad transparente, sino interpretar evidencias parciales, ruidosas y muchas veces sesgadas.

Una definición más rigurosa diría que la ciencia de datos estudia cómo transformar observaciones disponibles en descripciones, predicciones, segmentaciones o señales útiles para decidir. Esa transformación combina estadística, modelado, computación, conocimiento del dominio y criterio metodológico. Ninguno de esos elementos alcanza por separado. Un muy buen algoritmo sobre variables mal definidas produce conclusiones pobres. Un dataset enorme con una pregunta mal formulada conduce a errores bien maquillados.

También conviene decir qué no es la disciplina. No es sinónimo de inteligencia artificial, aunque se superponga con ella. No es solo estadística computacional, aunque la estadística sea uno de sus cimientos. No es simplemente programar modelos. Y, sobre todo, no es un procedimiento automático que convierta datos en verdad. Su objeto real es mucho más modesto y mucho más serio: construir representaciones suficientemente buenas de un fenómeno para razonar y decidir mejor que antes.

### 1.2 Fenómeno, dato, variable y decisión

Buena parte de los errores iniciales nace de mezclar niveles que conviene mantener separados. El primero es el **fenómeno**: abandono de clientes, mora, fraude, propagación de una enfermedad, sentimiento en textos, consumo eléctrico o desempeño académico. El segundo es el **dato**: la forma concreta en que ese fenómeno queda registrado. El tercero es la **variable**: la propiedad que medimos o construimos sobre cada unidad observada. El cuarto es la **decisión**: aquello que una persona o una organización hará a partir del análisis.

La distinción no es filosófica en sentido decorativo. Tiene consecuencias técnicas inmediatas. Si el fenómeno es la morosidad futura, pero los datos disponibles describen mal la solvencia presente, el problema no se arregla con un modelo más sofisticado. Si la decisión exige detectar casi todos los casos positivos, pero la métrica elegida premia solo la accuracy, tampoco habrá coherencia metodológica. Entre fenómeno, dato, variable y decisión debe conservarse una continuidad de sentido. Cuando esa cadena se rompe, el proyecto entra en contradicción mucho antes del entrenamiento.

### 1.3 Dataset, población, muestra y unidad de análisis

Un dataset no es meramente una tabla. Es una muestra finita extraída de una población o, al menos, de un proceso generador de datos que imaginamos más amplio que lo observado. Cuando trabajamos con un dataset tabular solemos representarlo como una matriz

\[
X \in \mathbb{R}^{n \times p}
\]

donde \(n\) es el número de observaciones y \(p\) el número de variables predictoras. En problemas supervisados, esa matriz suele ir acompañada por una variable objetivo

\[
y \in \mathbb{R}^n
\]

o por un vector de etiquetas en un conjunto discreto. La notación es compacta, pero deja escondida la pregunta más importante: ¿qué representa cada fila?

Esa pregunta define la **unidad de análisis**. Una fila puede ser un cliente, una transacción, un documento, una imagen, una consulta médica, un día o una ventana temporal. Cambiar la unidad de análisis cambia el problema completo. Si en un proyecto de churn cada fila representa un cliente, el modelo aprenderá diferencias entre clientes. Si cada fila representa semanas de actividad por cliente, el problema pasa a involucrar secuencias agregadas, dependencia temporal y otro criterio de validación. La tabla puede verse parecida; el problema ya no lo es.

Por eso una respuesta madura nunca supone independencia solo porque el dataset tiene filas y columnas. Dos registros del mismo paciente, del mismo dispositivo o del mismo usuario no son observaciones intercambiables sin más. El supuesto i.i.d. no viene garantizado por el formato tabular. Hay que justificarlo.

### 1.4 Features, target y variable respuesta

En aprendizaje supervisado distinguimos entre variables de entrada y variable de salida. A las primeras se las llama **features**, variables predictoras o covariables. A la segunda se la llama **target**, variable objetivo, etiqueta o variable respuesta. La distinción parece simple, pero en la práctica es una de las zonas donde más decisiones metodológicas se concentran.

No toda columna disponible merece convertirse en feature. Algunas no contienen información útil, otras son demasiado inestables, otras duplican lo que ya expresan otras variables y otras introducen fuga de información. Elegir features no es un trámite administrativo: es una hipótesis sobre qué aspectos del mundo observado contienen señal relevante para aproximar el target.

La variable objetivo requiere todavía más cuidado. Expresiones como "cliente en riesgo", "caso complicado" o "usuario valioso" no son targets; son etiquetas vagas. Hay que convertirlas en reglas observables. Por ejemplo: "cliente que cancela el servicio dentro de los próximos 60 días y no se reactiva durante ese período". Solo cuando el target queda operacionalizado el problema se vuelve entrenable, evaluable y discutible con rigor.

### 1.5 Tipos de problemas: clasificación, regresión, clustering y otras tareas

La primera gran división metodológica depende de la naturaleza de la salida. En **clasificación** el objetivo pertenece a un conjunto discreto de clases. Puede tratarse de un problema binario, multiclase o multilabel. En **regresión**, la salida es numérica y continua o casi continua: precio, demanda, duración, temperatura, ingreso esperado. En **clustering** no hay etiquetas observadas durante el entrenamiento y lo que se busca es detectar estructura o agrupamientos bajo una noción de similitud.

La taxonomía, sin embargo, no termina ahí. También existen problemas de ranking, recomendación, detección de anomalías, reducción de dimensionalidad, pronóstico temporal y estimación de supervivencia, entre otros. Lo importante no es memorizar una lista, sino entender que cada familia de problemas arrastra decisiones posteriores distintas: qué pérdida tiene sentido, cómo se evalúa, qué significa cometer un error y qué se puede interpretar del resultado.

Una misma necesidad real puede formularse de más de una manera. El riesgo crediticio, por ejemplo, puede modelarse como clasificación binaria, como regresión de pérdida esperada o como ranking de solicitantes por prioridad. La mejor formulación no es la más elegante en abstracto, sino la que mejor se acopla a la decisión que deberá tomarse.

### 1.6 Aprendizaje supervisado y no supervisado

En aprendizaje supervisado se observan pares \((x_i, y_i)\) y se busca aprender una relación entre entradas y salidas. En aprendizaje no supervisado se observan solo \(x_i\), y el objetivo ya no es predecir una etiqueta conocida, sino descubrir estructura, segmentar, reducir ruido o construir representaciones más útiles.

Una manera formal de escribir el aprendizaje supervisado es mediante una función de pérdida \(L\) y un riesgo esperado:

\[
R(f) = \mathbb{E}_{(X,Y)\sim P}[L(Y, f(X))]
\]

Aquí \(P\) representa la distribución real, generalmente desconocida, que genera los datos, y \(f\) es la regla de predicción. Como \(P\) no se conoce, en la práctica se minimiza el riesgo empírico:

\[
\hat R_n(f) = \frac{1}{n}\sum_{i=1}^{n} L(y_i, f(x_i))
\]

Toda la teoría de generalización nace de la diferencia entre estas dos expresiones. Un modelo puede obtener un riesgo empírico muy bajo y, sin embargo, comportarse mal fuera de la muestra. En no supervisado el problema es aún más delicado, porque no existe una variable respuesta que permita definir una noción única de error predictivo. Por eso la evaluación requiere más interpretación y menos automatismo.

### 1.7 Predicción versus causalidad

Una de las confusiones más persistentes consiste en tomar un buen modelo predictivo como si fuera un descubrimiento causal. Son problemas distintos. La predicción pregunta por \(P(Y \mid X)\): dado lo que observo, ¿qué puedo anticipar? La causalidad pregunta qué ocurriría si interviniera sobre una variable. Ese segundo problema se expresa de manera natural con el operador \(do(\cdot)\), por ejemplo \(P(Y \mid do(X=x))\).

Que una variable mejore mucho la predicción no implica que modificarla produzca el efecto esperado. Un modelo puede encontrar que la cantidad de reclamos al call center es un gran predictor de churn. De ahí no se sigue que aumentar reclamos cause abandono. Tal vez lo que ocurre es exactamente lo contrario: quienes ya están insatisfechos reclaman más. La ciencia de datos aplicada trabaja muchas veces con asociación predictiva; la inferencia causal exige diseños, supuestos y herramientas específicas. Confundir ambos planos lleva a errores conceptuales graves y a malas decisiones.

### 1.8 Función de pérdida, riesgo y criterio de decisión

Todo modelo optimiza algo, aunque ese "algo" no siempre se explicite. Ese objeto suele ser una **función de pérdida**, es decir, una manera de cuantificar cuán costoso es equivocarse. En clasificación la pérdida 0-1 castiga todo error por igual. En regresión, la pérdida cuadrática penaliza con especial dureza los errores grandes. En clasificación probabilística, la log-loss castiga mucho las predicciones falsas emitidas con exceso de confianza.

Pero la pérdida de entrenamiento no agota el problema. Un sistema real no solo predice: decide. Si un modelo devuelve una probabilidad \(\hat p(x)\), alguien deberá traducirla a una acción. Esa traducción depende del costo relativo de falsos positivos y falsos negativos. Si el costo de un falso positivo es \(C_{FP}\) y el de un falso negativo es \(C_{FN}\), una regla elemental de decisión bajo costo esperado sugiere clasificar como positivo cuando

\[
(1-p)C_{FP} < pC_{FN}
\]

lo que equivale a

\[
p > \frac{C_{FP}}{C_{FP} + C_{FN}}
\]

La fórmula importa menos por su aspecto algebraico que por su significado. Si dejar pasar un caso positivo es muy costoso, el umbral debe bajar. Una formación sólida exige poder responder siempre tres preguntas: qué se optimiza, por qué se optimiza eso y cómo esa optimización se traduce en una decisión concreta.

## Capítulo 2. Cómo formular correctamente un problema de datos

### 2.1 Del problema del mundo real al problema modelable

Las organizaciones no piden "aplicar XGBoost". Piden reducir rotación, mejorar una campaña, anticipar demanda, detectar fraude o segmentar usuarios. El primer trabajo serio de la ciencia de datos consiste en traducir esa necesidad a una formulación operativa. Traducir no significa solo cambiar palabras por columnas. Significa fijar con precisión cuál es la unidad de análisis, qué evento se quiere anticipar o describir, con qué horizonte temporal, usando qué información disponible al momento de decidir y con qué costo para cada tipo de error.

Si alguna de esas piezas queda difusa, el proyecto parece avanzar pero en realidad se apoya sobre una pregunta mal hecha. Ese tipo de error es especialmente peligroso porque suele descubrirse tarde: cuando ya se modeló, ya se evaluó y alguien empieza a notar que la respuesta no sirve para tomar la decisión que motivó el trabajo.

### 2.2 Unidad de análisis, granularidad y nivel de agregación

La **granularidad** indica el nivel de detalle con el que observamos el fenómeno. No es lo mismo trabajar con transacciones individuales que con agregados mensuales por cliente, ni con registros horarios que con promedios semanales. Cada nivel de agregación resuelve algunos problemas y crea otros.

Una granularidad muy fina puede conservar secuencias importantes, pero también introducir dependencia entre filas, ruido y explosión del número de observaciones. Una granularidad muy gruesa simplifica el modelado, aunque a costa de ocultar variación relevante. Esta decisión afecta la forma del target, la naturaleza de las features, la estrategia de validación y hasta el sentido de los joins entre tablas. Muchas bases "mal integradas" no están mal integradas por un error técnico, sino porque combinan fuentes con granularidades incompatibles sin haber definido primero qué representa una fila.

### 2.3 Horizonte temporal y disponibilidad de información

En problemas predictivos no alcanza con preguntar qué se quiere predecir. También hay que precisar **cuándo** se quiere predecir. Ese punto organiza todo el problema. Predecir mora a 12 meses al momento de otorgar un crédito no es lo mismo que predecir atraso a 30 días con información del comportamiento reciente. Cambia el target, cambian las variables legales y cambia el criterio de utilidad del modelo.

La regla central es simple y decisiva: toda feature debe representar información que estaría disponible en el instante real de decisión. Si una variable solo se conoce después del evento, usarla en entrenamiento introduce **data leakage**. El modelo parecerá mejor de lo que realmente es porque estará aprendiendo con pistas del futuro. En ciencia de datos, una formulación temporal vaga casi siempre termina en una validación optimista.

### 2.4 Ejemplo completo de formulación: predicción de abandono

La frase "queremos anticipar qué clientes se van a ir" todavía no define un problema modelable. Hace falta una reformulación más precisa, por ejemplo: "Para cada cliente activo al cierre de cada mes, estimar la probabilidad de cancelar el servicio dentro de los próximos 60 días utilizando exclusivamente información disponible hasta ese cierre. La decisión asociada será activar o no una campaña de retención".

Esa redacción ya contiene decisiones estructurales. La unidad de análisis es "cliente activo al cierre de mes". El target es "cancelación dentro de 60 días". El horizonte temporal está fijado. Las features válidas quedan delimitadas por el instante de corte. Y la salida del modelo no es todavía la acción, sino un insumo para una acción posterior. Formular así el problema permite discutir de verdad si conviene clasificar, rankear o calibrar probabilidades; sin esa formulación, cualquier comparación de modelos flota en el aire.

### 2.5 Clasificación, regresión o clustering: cómo decidir

Una heurística útil consiste en mirar la naturaleza del target. Si la salida pertenece a un conjunto finito de categorías, estamos frente a clasificación. Si es una magnitud numérica continua, el problema es de regresión. Si no existe target y se busca estructura latente, el terreno es el del aprendizaje no supervisado.

Sin embargo, las decisiones importantes aparecen en las zonas grises. El mismo problema puede admitir varias formulaciones válidas. La morosidad puede verse como clasificación binaria, como regresión de pérdida esperada o como ranking de prioridad. La elección correcta depende de qué decisión seguirá al modelo. Si hay que asignar un cupo fijo de revisiones, quizá el ranking sea más natural que la clasificación. Si hay que estimar pérdidas monetarias, la regresión puede capturar mejor el problema. Elegir bien la tarea es ya parte del modelado.

### 2.6 Costo del error y diseño de la métrica

Las métricas no son medallas universales. Son formalizaciones cuantitativas de qué significa equivocarse en un contexto concreto. Accuracy, F1, AUC, MAE o RMSE solo cobran sentido cuando se los relaciona con el costo real del error. En medicina puede ser preferible tolerar muchos falsos positivos con tal de no dejar pasar casos graves. En un filtro de spam puede ocurrir lo contrario: ocultar un correo importante puede ser más costoso que dejar pasar algunos mensajes basura.

Una buena formulación obliga a pensar la métrica antes del entrenamiento. Si la clase positiva es rara y operativamente crucial, accuracy será insuficiente. Si los errores grandes son mucho peores que los errores moderados, RMSE puede reflejar mejor el problema que MAE. Elegir una métrica por costumbre es una señal de inmadurez conceptual; elegirla porque traduce el costo del error es el comienzo de un análisis serio.

### 2.7 Cómo se nota que un problema está mal formulado

Los problemas mal formulados tienen síntomas bastante estables. El target cambia de significado a mitad del análisis. No queda claro qué variables serían legales al momento de predecir. La métrica elegida no representa el costo real del error. El conjunto de entrenamiento mezcla poblaciones o períodos incompatibles con el uso futuro. Y, sobre todo, la pregunta de negocio no coincide con la variable que efectivamente se está modelando.

Cuando ocurre eso, el pipeline puede verse prolijo y aun así producir algo inútil. Una parte importante del trabajo profesional consiste precisamente en detectar esas inconsistencias antes de que se disfracen de sofisticación técnica.

## Parte II. Leer el dato antes de modelarlo

Formular bien el problema no alcanza. El siguiente paso es aprender a leer los datos como evidencia y no como materia prima neutra. Esta parte se concentra en dos tareas previas al modelado que suelen hacerse mal cuando se las trata como rutina: el análisis de calidad y el análisis exploratorio. Su función no es "preparar" el dataset en sentido superficial, sino comprender qué tipo de objeto tenemos entre manos, qué sesgos arrastra y qué transformaciones serán legítimas.

## Capítulo 3. Anatomía del dataset y calidad de datos

### 3.1 Tipos de variables y escalas de medición

No basta con separar variables numéricas de variables categóricas. Esa distinción es demasiado gruesa para decidir cómo tratarlas. Una variable **nominal** distingue categorías sin orden intrínseco, como ciudad o marca. Una variable **ordinal** introduce un orden, como nivel educativo o severidad clínica. Las variables **de intervalo** permiten comparar diferencias, pero no poseen un cero absoluto natural, como la temperatura en Celsius. Las variables **de razón** sí tienen un cero interpretable, como ingresos, peso o distancia.

La escala de medición no es un detalle teórico. Delimita qué operaciones tienen sentido. Promediar códigos postales no tiene interpretación. Tratar como numérica una categoría meramente nominal impone una geometría artificial. Ignorar el orden de una variable ordinal puede hacer perder información relevante. En ciencia de datos, representar mal una variable equivale a deformar el fenómeno antes de empezar a aprender sobre él.

### 3.2 Calidad de datos: completitud, validez, consistencia y unicidad

La expresión "limpieza de datos" suele ocultar demasiadas cosas distintas bajo un mismo rótulo. Conviene separar dimensiones. La **completitud** pregunta cuánto falta. La **validez** pregunta si los valores observados respetan rangos, formatos y reglas plausibles. La **consistencia** examina si distintas columnas o tablas se contradicen. La **unicidad** verifica si hay duplicados indebidos. A esto se suma una dimensión menos visible pero igual de importante: la **trazabilidad**, es decir, saber de dónde viene cada dato y qué transformaciones sufrió.

Cada problema de calidad afecta de un modo diferente el análisis. Un faltante puede requerir imputación o exclusión. Una inconsistencia entre columnas puede delatar una regla de negocio mal aplicada. Un duplicado puede inflar artificialmente el peso de ciertos casos. Una variable cuyo significado cambió en el tiempo puede contaminar evaluaciones temporales. La calidad no es un casillero a marcar; es parte de la interpretación del dato.

### 3.3 Duplicados, inconsistencias y errores semánticos

Los duplicados rara vez son un asunto trivial. A veces son registros repetidos por error; otras veces son eventos distintos que solo parecen iguales. La tarea no consiste en eliminarlos por reflejo, sino en comprender qué representan. Dos filas idénticas pueden indicar una falla de ingesta, pero también un reprocesamiento legítimo. Dos clientes con el mismo identificador y atributos contradictorios pueden revelar un problema más profundo de integración.

Los errores semánticos son todavía más insidiosos. Unidades mezcladas, monedas distintas, categorías escritas de formas incompatibles, cambios de nomenclatura entre años o columnas cuyo significado administrativo mutó con el tiempo. Estos problemas no suelen aparecer como valores nulos ni como tipos mal inferidos. Exigen lectura crítica, control de contexto y diálogo con quienes conocen el origen de la base. Limpiar sin comprender el significado del dato es apenas mover la suciedad de lugar.

### 3.4 Mecanismos de datos faltantes

No todo faltante significa lo mismo. La distinción clásica entre MCAR, MAR y MNAR sigue siendo útil porque orienta la interpretación y la estrategia de imputación. Un faltante es **MCAR** (*Missing Completely At Random*) cuando su ausencia es independiente tanto del valor faltante como de las demás variables observadas. Es el caso más benigno y, en la práctica, el menos frecuente.

Hablamos de **MAR** (*Missing At Random*) cuando la ausencia depende de variables observadas. Por ejemplo, el ingreso falta más en ciertas regiones o grupos etarios. En ese caso la ausencia no es completamente aleatoria, pero puede explicarse con información disponible. En **MNAR** (*Missing Not At Random*), en cambio, la probabilidad de ausencia depende del propio valor faltante o de factores no observados. Allí la omisión misma contiene señal difícil de recuperar. La importancia de esta clasificación es práctica: imputar con un método simple bajo un mecanismo MNAR puede deformar la distribución y las relaciones con el target de manera seria.

### 3.5 Outliers: error, rareza o parte esencial del fenómeno

Un valor extremo no es necesariamente un error. Puede ser una medición fallida, una observación válida pero rara o, en algunos dominios, precisamente el tipo de caso que interesa detectar. En fraude, una transacción anómala puede ser la señal más valiosa. En un sensor industrial, un pico imposible puede indicar una avería del instrumento. En ingresos declarados, un valor desmesuradamente alto puede ser un error de carga o un caso genuino.

Por eso conviene separar **outlier estadístico** de **anomalía sustantiva**. El primero es raro respecto de la distribución observada; la segunda es rara respecto de la lógica del dominio. A veces coinciden. Otras no. Tratar todo valor extremo como basura elimina información útil; conservarlo todo sin criterio también es un error. La pregunta correcta no es solo cuán lejos está un punto, sino qué historia plausible explica esa distancia.

### 3.6 Sesgo de muestreo, representatividad y desbalance de clases

Un dataset puede estar impecablemente limpio y, aun así, representar mal el fenómeno. La calidad técnica de la tabla no compensa una mala calidad del muestreo. Si un modelo de crédito se entrena solo con clientes aprobados, lo que aprende puede no generalizar a solicitantes futuros. Si un sistema médico se entrena en un hospital de alta complejidad, quizá rinda peor en atención primaria. Si un corpus de texto proviene de un único canal, la generalización a otros registros discursivos puede ser débil.

El desbalance de clases es una forma particular y muy frecuente de este problema. Cuando la clase positiva es rara, la accuracy se vuelve engañosa y las métricas deben elegirse con mayor cuidado. Pero el punto de fondo es más amplio: la muestra no es solo un contenedor de ejemplos, sino una versión concreta del mundo sobre la cual el modelo aprenderá qué es "normal", qué es "raro" y qué merece ser priorizado.

### 3.7 Qué debe salir de un análisis de calidad

Una lectura rigurosa del dataset no debería producir una colección dispersa de observaciones, sino un diagnóstico estructurado. Ese diagnóstico tiene que dejar claro qué representa cada fila, qué significa cada columna, qué problemas de calidad están confirmados o son plausibles, qué mecanismos de faltante parecen actuar, qué variables pueden inducir fuga de información, qué sesgos de muestreo aparecen y qué restricciones impondrá todo eso sobre el modelado posterior.

Cuando un estudiante domina este punto deja de mirar la base como un conjunto neutro de columnas y empieza a verla como evidencia imperfecta. Esa transición intelectual es decisiva. Modelar bien depende, en gran medida, de haber aprendido primero a desconfiar con criterio.

## Capítulo 4. Análisis exploratorio de datos: leer antes de modelar

### 4.1 El sentido profundo del EDA

El análisis exploratorio de datos, o EDA, no es una ceremonia previa al modelado ni una colección de gráficos de compromiso. Es la primera etapa inferencial del trabajo. Allí se estudian distribuciones, escalas, faltantes, outliers, relaciones, desbalances y posibles fugas. Un buen EDA no intenta impresionar: intenta responder preguntas.

Por eso la calidad del EDA se mide por las decisiones que habilita. Si después de explorarlo no sabemos qué variables transformar, dónde sospechar leakage, qué familias de modelos podrían ser razonables o qué métricas deberían priorizarse, entonces el EDA fue ornamental. Mirar mucho no equivale a comprender.

### 4.2 Estadística descriptiva univariada

El análisis univariado estudia cada variable por separado. La media

\[
\bar x = \frac{1}{n}\sum_{i=1}^{n} x_i
\]

resume el centro aritmético de la muestra. La mediana, en cambio, señala el valor que deja la mitad de las observaciones a cada lado. Cuando la distribución es asimétrica o contiene outliers, la mediana suele describir mejor el centro típico que la media.

La dispersión puede medirse con la varianza muestral

\[
s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar x)^2
\]

y con su raíz cuadrada, el desvío estándar. Pero como ambas dependen fuertemente de valores extremos, conviene complementarlas con cuantiles e intervalo intercuartílico:

\[
IQR = Q_3 - Q_1
\]

Lo importante no es recitar definiciones, sino interpretarlas. Si la media supera mucho a la mediana, hay asimetría a derecha. Si el IQR es pequeño y aparecen extremos muy alejados, la variable está concentrada en el centro con pocos casos raros. Una variable casi constante probablemente aporte poco a muchos modelos.

### 4.3 Frecuencias y análisis de variables categóricas

En variables categóricas, el resumen central no es el promedio sino la distribución de frecuencias. Interesa saber qué categorías dominan, cuáles son raras, si la cardinalidad es alta y si existen codificaciones inconsistentes. Una categoría poco frecuente no debe eliminarse por inercia: puede ser ruido, pero también puede representar un segmento pequeño y relevante.

El análisis categórico también ayuda a detectar problemas silenciosos. Espacios extra, tildes inconsistentes, abreviaturas mezcladas, cambios históricos de nomenclatura o categorías que en realidad expresan datos faltantes con otra etiqueta. Muchos problemas de modelado empiezan como pequeños descuidos semánticos en variables categóricas.

### 4.4 Relaciones entre variables: correlación, dependencia y no linealidad

El análisis multivariado busca entender cómo se relacionan las variables entre sí y con el target. Una herramienta clásica es la correlación de Pearson:

\[
r_{XY} = \frac{\sum_{i=1}^{n}(x_i-\bar x)(y_i-\bar y)}
{\sqrt{\sum_{i=1}^{n}(x_i-\bar x)^2}\sqrt{\sum_{i=1}^{n}(y_i-\bar y)^2}}
\]

Esta medida resume asociación lineal y toma valores entre \(-1\) y \(1\). Es útil, pero tiene límites muy claros. Correlación cercana a cero no significa ausencia de relación. Si \(Y = X^2\) y \(X\) se distribuye simétricamente alrededor de cero, la relación es total y la correlación lineal puede ser casi nula.

Por eso el EDA serio no se agota en una matriz de correlación. Necesita mirar no linealidades, interacciones, saturaciones, umbrales y subpoblaciones. En otras palabras, necesita criterio geométrico y no solo resumen numérico.

### 4.5 Visualización con propósito analítico

Las visualizaciones valiosas no son las más vistosas, sino las que responden una pregunta. Un histograma muestra forma de distribución. Un boxplot resume dispersión y sugiere posibles extremos. Un scatterplot permite ver asociaciones, curvaturas, heterogeneidad y subgrupos. Un mapa de calor puede ser útil para una lectura panorámica, pero no reemplaza el examen fino de relaciones importantes.

En un examen y en un informe profesional vale la misma regla: un gráfico sin interpretación no explica nada. Cada visualización debería responder al menos tres preguntas. Qué muestra, por qué importa y qué decisión metodológica sugiere.

### 4.6 EDA respecto del target

Explorar respecto del target significa preguntar cómo cambian las features entre clases o a lo largo de la respuesta continua. En clasificación interesa saber si la clase positiva presenta distribuciones distintas, categorías más frecuentes, patrones temporales particulares o señales textuales específicas. En regresión interesa observar si la relación parece lineal, escalonada, saturada, heterocedástica o dominada por pocos puntos influyentes.

Ese análisis orienta el modelado. Si una relación con el target es fuertemente no lineal, quizá convenga transformar la variable o usar un modelo capaz de capturar curvaturas. Si la clase está extremadamente desbalanceada, ya desde el EDA debe quedar claro que la accuracy será insuficiente. Explorar sin mirar el target es conocer la forma del dato; explorar respecto del target es empezar a entender la dificultad real del problema.

### 4.7 El EDA como detector de leakage

Muchas fugas de información pueden sospecharse antes de entrenar un solo modelo. Variables con separación casi perfecta respecto del target, timestamps posteriores al evento que se quiere predecir, identificadores administrativos ligados al resultado o columnas derivadas del proceso de resolución del caso son señales de alarma.

Hay una regla empírica útil: cuando una variable parece demasiado buena para ser verdad, primero hay que desconfiar. Los grandes scores inesperados suelen deberse más a información ilegítima que a genialidad algorítmica.

### 4.8 Qué debería producir un buen EDA

Al terminar un EDA serio debería existir una imagen bastante precisa del dataset. Qué variables parecen estables y cuáles problemáticas. Qué transformaciones son plausibles. Qué desbalances, outliers y relaciones dominan el escenario. Dónde puede haber leakage. Qué familias de modelos tienen sentido como baseline y cuáles requerirán más trabajo de representación.

Si el EDA no cambia decisiones, probablemente fue una actividad cosmética. Su función no es adornar el informe, sino mejorar la calidad del razonamiento posterior.

## Parte III. Preprocesar no es maquillar: representar bien para poder aprender

Una vez entendido el dato aparece un problema más profundo que el de "dejarlo prolijo": cómo construir una representación sobre la que un modelo pueda aprender sin engañarse. Preprocesar no es embellecer la tabla. Es decidir cómo traducir un fenómeno imperfectamente observado a un espacio donde ciertas regularidades sean detectables sin destruir el sentido original del problema.

## Capítulo 5. Preprocesamiento y construcción de representación

### 5.1 El principio rector: los modelos aprenden sobre representaciones

Ningún modelo aprende directamente sobre el fenómeno. Aprende sobre la forma en que ese fenómeno quedó representado. Incluso cuando la base parece "lista", cada columna ya encapsula una decisión de medición, agregación o codificación. El preprocesamiento continúa esa tarea. Su objetivo no es solo corregir imperfecciones, sino hacer más legible la estructura relevante y menos dañino el ruido.

Dos principios gobiernan esta etapa. Primero: toda transformación que ajuste parámetros debe aprenderse solo con los datos de entrenamiento. Segundo: ninguna transformación debe alterar el significado del problema de una forma incompatible con el uso real del modelo. Preprocesar bien es, en el fondo, preservar sentido mientras se mejora aprendibilidad.

### 5.2 Pipelines y separación entre entrenamiento, validación y prueba

Un error clásico consiste en imputar, escalar o reducir dimensión sobre toda la base antes de partirla en entrenamiento y prueba. Eso filtra información del conjunto de evaluación hacia el entrenamiento. Si estandarizamos una variable como

\[
z_{ij} = \frac{x_{ij} - \mu_j}{\sigma_j}
\]

los parámetros \(\mu_j\) y \(\sigma_j\) deben calcularse con el conjunto de entrenamiento, y luego aplicarse al resto:

\[
z_{ij}^{(test)} = \frac{x_{ij}^{(test)} - \mu_j^{(train)}}{\sigma_j^{(train)}}
\]

La lógica se extiende a imputadores, selectores de variables, PCA, target encoding y cualquier otra transformación que "aprenda" algo de los datos. Los pipelines existen precisamente para encapsular esa disciplina y evitar leakage accidental.

### 5.3 Imputación: decidir bajo incertidumbre

Imputar no es rellenar agujeros de manera mecánica. Es tomar una decisión estadística sobre valores no observados. Imputar con la media es simple, pero reduce artificialmente la varianza y puede aplanar relaciones importantes. La mediana suele ser más robusta frente a asimetrías y outliers. La moda puede ser razonable en categóricas de baja cardinalidad. La imputación por grupos aprovecha contexto. Métodos multivariados o basados en vecinos intentan preservar mejor la estructura conjunta, aunque al precio de más complejidad.

La elección solo tiene sentido si se la vincula con el mecanismo de faltante. Si la ausencia misma contiene información, conviene además agregar una variable indicadora. En muchos problemas, ese simple indicador aporta señal que la imputación por sí sola destruiría. La pregunta correcta no es "qué método suena más sofisticado", sino "qué historia sobre el dato estoy asumiendo cuando completo de esta manera".

### 5.4 Tratamiento de outliers

Con los outliers no existe una receta única porque no existe una única razón por la que aparecen. Si hay evidencia de error, puede corresponder corregir o eliminar. Si son pocos pero válidos y su peso distorsiona demasiado, puede ser razonable winsorizar o transformar. Si expresan un régimen raro pero importante, conviene preservarlos e incluso modelarlos con cuidado especial.

Los umbrales estadísticos, como z-score o reglas basadas en IQR, son útiles como detector inicial, no como juez final. La decisión correcta depende del dominio, del objetivo y del modelo. Un árbol tolera mejor ciertos extremos que una regresión lineal. Un sistema de detección de anomalías, precisamente, vive de los casos raros. Tratar bien los outliers exige pasar de la distancia al significado.

### 5.5 Encoding de variables categóricas

Muchos modelos necesitan entradas numéricas y obligan a traducir categorías a números. Pero codificar no es un acto neutro. El **one-hot encoding** crea una columna binaria por categoría y suele ser la opción natural para variables nominales porque no impone orden. Su costo es el aumento de dimensionalidad cuando la cardinalidad es alta.

El **ordinal encoding** solo tiene sentido cuando las categorías poseen un orden real. Aplicarlo sobre valores puramente nominales introduce distancias ficticias. El **target encoding** puede ser muy potente para cardinalidades elevadas porque resume cada categoría usando información del target, pero justamente por eso es altamente vulnerable a leakage y debe estimarse dentro de cada fold o con regularización cuidadosa. La regla general es simple: toda codificación impone una geometría. Lo importante es que esa geometría se parezca al problema y no a la comodidad de la herramienta.

### 5.6 Escalado, normalización y sensibilidad del modelo

La necesidad de escalar depende fuertemente de la familia de modelos. En métodos basados en distancias, como KNN, o en algoritmos optimizados por gradiente, como regresión regularizada, SVM o redes neuronales, la escala importa mucho. Una variable medida en miles puede dominar otra medida en décimas aunque sea menos relevante.

La **estandarización** centra y escala por desvío estándar. La normalización min-max fuerza un rango determinado. El **robust scaling** usa mediana e IQR y resiste mejor los extremos. En cambio, árboles y ensambles de árboles suelen ser mucho menos sensibles a la escala porque comparan una variable consigo misma a través de umbrales. Responder "siempre hay que escalar" es una señal de aprendizaje mecánico; responder "depende del sesgo inductivo del modelo" es una señal de comprensión.

### 5.7 Discretización y transformaciones de distribución

Discretizar una variable continua significa reemplazarla por intervalos. A veces mejora interpretabilidad o robustez frente a ruido; otras veces destruye información útil. No debería hacerse por costumbre. Lo mismo vale para las transformaciones continuas. Una transformación logarítmica como \(\log(x+c)\) puede comprimir colas largas, reducir asimetría y acercar ciertas relaciones a la linealidad. Otras familias, como Box-Cox o Yeo-Johnson, buscan objetivos parecidos con más flexibilidad.

Lo importante es no olvidar qué se transformó. Si se modela \(\log(y)\) y no \(y\), la interpretación de coeficientes, residuos y errores cambia. Transformar bien exige saber qué problema resuelve la transformación y qué nueva escala conceptual introduce.

### 5.8 Feature engineering: donde el dominio se vuelve variable

La ingeniería de variables es el lugar donde el conocimiento del dominio se traduce en estructura utilizable por un modelo. Construir razones como ingreso/cuota, interacciones como \(x_1x_2\), agregados temporales, indicadores de recencia o medidas resumidas de comportamiento suele ser mucho más valioso que cambiar de algoritmo sin cambiar la representación.

Una buena feature no es un truco arbitrario. Es una hipótesis explícita sobre qué aspecto del fenómeno importa. En problemas temporales, por ejemplo, un lag o una media móvil no son adornos: condensan dependencia temporal. En texto, TF-IDF no es un simple conteo refinado: es una forma de destacar términos discriminativos. Quien entiende el problema suele mejorar primero la representación y solo después discute la familia de modelos.

### 5.9 Selección de variables y multicolinealidad

No todas las variables contribuyen de la misma manera. Algunas son casi irrelevantes, otras son redundantes y otras introducen inestabilidad. En modelos lineales, la **multicolinealidad** vuelve difíciles de interpretar los coeficientes porque pequeñas perturbaciones en los datos pueden alterar mucho su valor individual. El problema no es que el modelo "no pueda predecir", sino que la descomposición del efecto entre variables fuertemente correlacionadas se vuelve frágil.

Herramientas como el VIF ayudan a diagnosticar esta situación, y técnicas como Ridge, Lasso o Elastic Net permiten mitigarla. En árboles el problema cambia de forma: la predicción puede mantenerse estable, pero las importancias de variables pueden volverse engañosas cuando varias columnas expresan casi la misma señal. Seleccionar variables no consiste solo en quedarse con menos, sino en quedarse con un conjunto que sostenga mejor el equilibrio entre información, estabilidad e interpretabilidad.

### 5.10 Qué preprocesamiento necesita cada familia de modelos

El preprocesamiento no existe separado del modelado. KNN y SVM suelen exigir escalado cuidadoso. Regresión lineal y logística lo agradecen especialmente cuando hay regularización. Árboles toleran bien heterogeneidad de escala y cierta no linealidad. PCA requiere centrado y, muchas veces, estandarización previa. Modelos de texto necesitan otra noción de representación. Redes neuronales suelen beneficiarse de entradas bien condicionadas.

Una respuesta experta no repite recetas universales. Explica por qué un modelo es sensible a cierta transformación y otro no. En esa explicación aparece, en realidad, una comprensión más profunda del sesgo inductivo de cada familia de métodos.

## Parte IV. Evaluar sin autoengañarse

El modelado se vuelve científicamente interesante solo cuando la evaluación protege contra la ilusión de haber aprendido lo que en realidad se memorizó. Esta parte se concentra en la cuestión más importante de toda la materia después de la formulación del problema: cómo diseñar un experimento honesto. Entrenar bien no demuestra nada si la validación está mal pensada.

## Capítulo 6. Generalización, validación y diseño experimental

### 6.1 El problema central: entrenar no es demostrar

Que un modelo funcione bien sobre los datos con los que fue ajustado no demuestra que funcionará bien sobre datos nuevos. Esta obviedad aparente es el corazón de la disciplina. Si el modelo es demasiado rígido, puede no capturar estructura relevante. Si es demasiado flexible, puede aprender también el ruido accidental de la muestra.

El objetivo no es minimizar solo el error en entrenamiento, sino aproximar bien el riesgo fuera de muestra. Toda la maquinaria de particiones, validación cruzada, regularización y comparación de modelos existe para controlar la brecha entre desempeño observado y desempeño futuro. La evaluación no es un trámite posterior al modelado; es el marco que le da sentido.

### 6.2 Conjuntos de entrenamiento, validación y prueba

La división clásica distingue tres roles. El conjunto de **entrenamiento** ajusta parámetros. El de **validación** sirve para comparar alternativas e hiperparámetros. El de **prueba** estima el desempeño final en datos no usados para decidir. Aun cuando en la práctica entrenamiento y validación se organicen mediante cross-validation, esos roles conceptuales deben mantenerse separados.

La idea de fondo es simple: si usamos un conjunto para tomar decisiones de modelado, deja de ser una medida externa del rendimiento. Quien evalúa honestamente protege su último conjunto realmente no visto como un recurso escaso, no como un espacio más para probar ideas.

### 6.3 Hold-out y validación cruzada

El **hold-out** consiste en una partición simple entre entrenamiento y prueba. Es rápido, claro y a veces suficiente, pero su estimación puede depender demasiado del azar de una única división. La **validación cruzada k-fold** reduce ese problema al repartir el entrenamiento en \(k\) pliegues, entrenar sobre \(k-1\) y validar sobre el restante, repitiendo el proceso hasta recorrerlos todos.

Cuando las clases están desbalanceadas conviene estratificar para preservar proporciones. Cuando varias observaciones pertenecen a la misma entidad, conviene separar por grupos. La técnica concreta importa menos que el principio: el esquema de validación debe parecerse a la situación real de generalización que queremos evaluar.

### 6.4 Validación temporal y walk-forward

En problemas donde el tiempo importa, mezclar observaciones al azar es conceptualmente incorrecto. El futuro no debe usarse para predecir el pasado. Por eso en series temporales y en contextos con deriva temporal conviene usar esquemas de validación que respeten la cronología, como ventanas crecientes o deslizantes.

La validación **walk-forward** refleja bien esta lógica: se entrena en un bloque histórico y se valida en un bloque posterior, repitiendo el proceso a lo largo del tiempo. Si un modelo solo funciona cuando se rompe el orden temporal, no es que generaliza bien; es que la evaluación está mal diseñada.

### 6.5 Nested cross-validation y comparación honesta de modelos

Cuando el ajuste de hiperparámetros es intenso, incluso la validación cruzada puede volverse optimista si se utiliza al mismo tiempo para seleccionar y para evaluar. La **nested cross-validation** separa esas funciones mediante un bucle interno para tuning y un bucle externo para estimación más honesta del rendimiento.

No siempre es necesaria en la práctica cotidiana, pero su lógica es muy instructiva. Muestra que cada decisión guiada por los datos consume información y puede sesgar la evaluación hacia el optimismo. Cuanto más intensamente exploramos el espacio de modelos, más estricta debe ser nuestra disciplina experimental.

### 6.6 Overfitting, underfitting y curvas de aprendizaje

El **underfitting** aparece cuando el modelo es demasiado simple o la representación demasiado pobre: el error es alto tanto en entrenamiento como en validación. El **overfitting** aparece cuando el modelo se adapta demasiado a peculiaridades del entrenamiento: el error de entrenamiento baja mucho, pero la validación no acompaña o empeora.

Las curvas de aprendizaje ayudan a distinguir ambos escenarios. Si con más datos el error de validación cae y la brecha con entrenamiento disminuye, probablemente haya alta varianza. Si ambos errores permanecen altos, el problema puede ser alto sesgo, features pobres o un target mal formulado. Mirar un único score aislado dice poco; mirar cómo evoluciona el comportamiento con más información dice mucho más.

### 6.7 Sesgo y varianza como marco unificador

En regresión con pérdida cuadrática, el error esperado puede descomponerse conceptualmente como

\[
\mathbb{E}\left[(Y-\hat f(X))^2\right] = \text{Bias}^2 + \text{Variance} + \text{Noise}
\]

El **sesgo** mide cuánto se aparta, en promedio, la predicción esperada del modelo respecto de la función verdadera. La **varianza** mide cuánto cambia la predicción si reentrenáramos el modelo sobre distintas muestras. El **ruido** representa aquello que ni siquiera un modelo ideal puede eliminar porque pertenece a la variabilidad irreducible del fenómeno.

La utilidad pedagógica de esta descomposición es enorme. Permite entender por qué modelos muy rígidos suelen fallar por sesgo y modelos muy flexibles por varianza. También explica por qué regularizar, agregar datos o construir mejores features puede mejorar la generalización por caminos diferentes.

### 6.8 Significancia estadística y relevancia práctica

Una mejora pequeña en una métrica puede ser estadísticamente estable y, sin embargo, irrelevante en la práctica. También puede ocurrir lo contrario: un cambio modesto en el score promedio puede implicar una mejora importante en el segmento donde el negocio realmente se juega.

Por eso evaluar no es solo comparar medias. Es traducir esas diferencias a consecuencias: cuántos casos valiosos se recuperan, cuánto costo se evita, cuánto varía el resultado entre folds o períodos, cómo cambia entre subpoblaciones y qué tan estable es la ventaja del modelo ganador. El número importa; su interpretación importa todavía más.

## Capítulo 7. Métricas: cómo cuantificar el error sin perder el sentido del problema

### 7.1 Matriz de confusión y lectura estructural del error

En clasificación binaria, la matriz de confusión organiza los resultados en cuatro celdas: verdaderos positivos, falsos positivos, verdaderos negativos y falsos negativos. Más que una tabla contable, es un mapa de cómo se distribuye el error. Permite distinguir si el modelo deja pasar demasiados positivos, si produce demasiadas falsas alarmas o si sus aciertos se concentran en la clase mayoritaria.

Supongamos 1000 casos, de los cuales 50 son positivos reales. Si el modelo detecta 40, deja pasar 10 y marca erróneamente 60 negativos como positivos, entonces tiene \(TP=40\), \(FN=10\), \(FP=60\) y \(TN=890\). Esa simple descomposición ya muestra algo importante: el modelo recupera muchos positivos, pero a costa de bastantes falsas alarmas. Si eso es bueno o malo depende del problema, no de la tabla por sí sola.

### 7.2 Accuracy, precision, recall, especificidad y F1

La **accuracy**

\[
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
\]

mide la proporción total de aciertos. En el ejemplo anterior vale \(0.93\). Sin embargo, como la clase positiva representa solo el 5% de los casos, un clasificador que predijera siempre negativo obtendría \(0.95\) y sería inútil. Este contraste basta para entender por qué la accuracy puede ser engañosa.

La **precision**

\[
\text{Precision} = \frac{TP}{TP + FP}
\]

responde a la pregunta: de todo lo que marqué como positivo, ¿cuánto era realmente positivo? El **recall**

\[
\text{Recall} = \frac{TP}{TP + FN}
\]

responde a otra: de todos los positivos reales, ¿cuántos encontré? La **especificidad**

\[
\text{Specificity} = \frac{TN}{TN + FP}
\]

mide la capacidad de reconocer negativos. El **F1-score**

\[
F_1 = 2\cdot \frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
\]

combina precisión y recall de forma exigente. Si una es muy baja, F1 también lo será. Lo valioso no es memorizar fórmulas, sino entender qué pregunta operativa responde cada una.

### 7.3 Curvas ROC, AUC y curvas Precision-Recall

Muchos modelos no devuelven directamente una clase, sino un score o una probabilidad. Al variar el umbral de decisión cambian las tasas de verdaderos y falsos positivos. La curva **ROC** resume ese comportamiento y el AUC-ROC captura la capacidad global de discriminación entre positivos y negativos.

La curva **Precision-Recall** resulta especialmente informativa cuando la clase positiva es rara. En esos contextos puede ocurrir que un modelo tenga un AUC-ROC aceptable y, sin embargo, una precisión muy pobre en los rangos de recall que realmente interesan. Elegir entre ROC y PR no es un detalle gráfico. Es decidir qué aspecto del comportamiento del modelo queremos hacer visible.

### 7.4 Umbral de decisión y calibración

Usar 0.5 como umbral por costumbre es una práctica demasiado extendida y casi nunca está justificada. El umbral correcto depende del costo relativo de los errores, de la capacidad operativa para revisar casos y del objetivo del sistema. En screening médico puede convenir un umbral bajo. En auditorías costosas, uno más alto.

Además de discriminar bien, un modelo puede necesitar estar **calibrado**. Decimos que está bien calibrado si entre los casos a los que asigna probabilidad 0.8 el evento ocurre, aproximadamente, el 80% de las veces. Un modelo puede rankear correctamente y, sin embargo, estar mal calibrado. Cuando la salida se interpreta como probabilidad y no solo como orden, esa diferencia es crucial.

### 7.5 Métricas de regresión

En regresión interesa medir distancias entre valor real y valor predicho. El **MAE**

\[
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat y_i|
\]

tiene la ventaja de expresarse en las mismas unidades del target y de ser menos sensible a errores extremos. El **MSE**

\[
MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i-\hat y_i)^2
\]

y su raíz, el **RMSE**, penalizan más fuertemente los errores grandes. Esa penalización puede ser deseable o no, según el costo real del problema.

El coeficiente

\[
R^2 = 1 - \frac{\sum_{i=1}^{n}(y_i-\hat y_i)^2}{\sum_{i=1}^{n}(y_i-\bar y)^2}
\]

indica cuánto mejora el modelo respecto de predecir siempre la media. Pero no debe leerse como una medida universal de bondad. Un \(R^2\) alto puede convivir con errores absolutos importantes, y un \(R^2\) moderado puede ser razonable en fenómenos intrínsecamente ruidosos. Ninguna métrica de regresión se interpreta al margen de la escala del problema.

### 7.6 Métricas internas y externas en clustering

Como en clustering no existe target observado durante el ajuste, la evaluación exige otro tipo de criterios. El índice **silhouette** compara, para cada observación, qué tan cerca está de su propio cluster respecto del cluster alternativo más próximo:

\[
s(i)=\frac{b(i)-a(i)}{\max\{a(i),b(i)\}}
\]

donde \(a(i)\) es la distancia promedio al propio grupo y \(b(i)\) la mínima distancia promedio a otro grupo. Valores altos sugieren buena asignación; valores cercanos a cero, fronteras difusas.

También pueden usarse índices como Davies-Bouldin o, si existen etiquetas externas, medidas de acuerdo como ARI o NMI. Pero ninguna métrica interna reemplaza la interpretación sustantiva. Un clustering puede optimizar silhouette y seguir siendo irrelevante para la acción o conceptualmente vacío si la representación de partida era mala.

### 7.7 Elegir la métrica correcta

La mejor métrica no es la más famosa, sino la que castiga los errores que verdaderamente importan. Si la clase positiva es rara y valiosa, accuracy no alcanzará. Si importa la calidad probabilística, la calibración debe entrar en escena. Si el sistema opera sobre un cupo fijo de revisión, quizá convenga medir precisión en top-k o recall en un tramo particular del ranking.

La madurez técnica se reconoce aquí con facilidad. No se trata de recitar definiciones, sino de justificar por qué una métrica representa mejor que otra el problema que queremos resolver.

## Parte V. Modelos supervisados fundamentales

Con el problema bien formulado, los datos leídos críticamente, la representación construida y la evaluación diseñada, recién ahora tiene sentido estudiar modelos concretos. Esta parte reúne las familias supervisadas que suelen funcionar como columna vertebral conceptual de la materia. No interesa solo saber cómo operan, sino qué supuestos introducen, qué sesgo inductivo traen y qué tipo de interpretación permiten.

## Capítulo 8. Regresión lineal: una base que sigue siendo esencial

### 8.1 Por qué sigue importando un modelo lineal

La regresión lineal suele presentarse como el modelo "básico", y esa etiqueta la perjudica. En realidad, es una de las herramientas más formativas de toda la disciplina. Permite ver con claridad la relación entre una hipótesis estructural, una función de pérdida, un método de estimación y un criterio de interpretación. Además, sigue siendo muy útil cuando importa la explicabilidad, cuando la relación es aproximadamente lineal o cuando necesitamos un baseline fuerte y honesto.

El modelo clásico para una respuesta continua es

\[
y = \beta_0 + \beta_1x_1 + \cdots + \beta_px_p + \varepsilon
\]

donde \(\beta_0\) es la ordenada al origen, los \(\beta_j\) son coeficientes y \(\varepsilon\) resume aquello que el modelo no explica. La palabra "lineal" debe leerse con cuidado: el modelo es lineal en los parámetros. Podemos incluir \(x^2\), \(\log x\) o interacciones y seguir dentro de la familia lineal mientras la combinación siga siendo lineal en \(\beta\).

### 8.2 Mínimos cuadrados ordinarios

Si escribimos el modelo en forma matricial,

\[
y = X\beta + \varepsilon
\]

la estimación por mínimos cuadrados ordinarios busca

\[
\hat\beta = \arg\min_\beta \|y - X\beta\|_2^2
\]

Es decir, el vector de coeficientes que hace mínima la suma de cuadrados de los residuos. La formulación es importante porque deja ver qué se está optimizando exactamente.

Si desarrollamos

\[
S(\beta) = (y - X\beta)^T(y - X\beta)
\]

y derivamos respecto de \(\beta\), obtenemos las ecuaciones normales:

\[
X^TX\hat\beta = X^Ty
\]

Cuando \(X^TX\) es invertible,

\[
\hat\beta = (X^TX)^{-1}X^Ty
\]

Cada símbolo tiene contenido. \(X^TX\) resume la geometría y correlación de las features. \(X^Ty\) resume su alineación lineal con la respuesta. La solución no es una fórmula para memorizar, sino una lectura compacta de cómo se equilibran esas relaciones.

### 8.3 Interpretación geométrica

La solución de mínimos cuadrados puede leerse como la proyección ortogonal del vector \(y\) sobre el subespacio generado por las columnas de \(X\). El modelo lineal busca, en ese subespacio, la aproximación más cercana posible a la respuesta observada.

Esta lectura geométrica explica por qué la colinealidad es problemática. Si las columnas de \(X\) son casi linealmente dependientes, el subespacio queda mal condicionado y pequeñas variaciones en los datos producen grandes cambios en la solución. La geometría, en este caso, no es una metáfora: es el problema mismo.

### 8.4 Cómo interpretar los coeficientes

Bajo una especificación razonable, \(\beta_j\) se interpreta como el cambio esperado en \(y\) ante un aumento de una unidad en \(x_j\), manteniendo constantes las demás variables. Esa última cláusula es decisiva. El coeficiente nunca describe el efecto bruto de una variable "por sí sola", sino su contribución dentro del modelo y condicionada al resto de las covariables.

Las transformaciones cambian la interpretación. Si el target está en logaritmos, los coeficientes se leen aproximadamente como cambios porcentuales. Si una variable fue estandarizada, el coeficiente corresponde a un cambio de un desvío estándar, no de una unidad original. Y, en todos los casos, la interpretación asociativa no debe confundirse con interpretación causal.

### 8.5 Supuestos clásicos y qué significa violarlos

Los supuestos clásicos del modelo lineal suelen enumerarse de memoria: linealidad en parámetros, exogeneidad, independencia de errores, homocedasticidad, ausencia de multicolinealidad severa y normalidad de errores para ciertos resultados inferenciales. Lo importante no es recitar la lista, sino comprender qué se rompe cuando uno falla.

Si la relación relevante no es aproximadamente lineal y no fue enriquecida con transformaciones, el modelo queda mal especificado. Si la varianza del error cambia sistemáticamente, los errores estándar e intervalos pueden volverse poco confiables. Si hay dependencia temporal o por grupo, el optimismo evaluativo aumenta. Si hay colinealidad severa, los coeficientes se vuelven inestables. Entender el rol de cada supuesto vale más que repetirlo.

### 8.6 Residuos, diagnóstico y límites del modelo

El residuo \(e_i = y_i - \hat y_i\) resume el error del modelo en cada observación. Analizar residuos permite ver si quedó estructura sin modelar. Un patrón curvo en residuos contra ajustados sugiere no linealidad remanente. Un "abanico" puede indicar heterocedasticidad. Algunos puntos con alta influencia pueden alterar desproporcionadamente la estimación.

La regresión lineal no fracasa por ser simple, sino cuando se le exige representar una estructura que no cabe bien en ella. Su gran virtud es justamente que deja visibles sus límites. Por eso sigue siendo una herramienta de pensamiento, no solo un modelo de uso.

### 8.7 Regularización: Ridge, Lasso y Elastic Net

Cuando hay muchas variables, colinealidad o riesgo de sobreajuste, conviene agregar penalizaciones a la función objetivo. En **Ridge** se minimiza

\[
\|y-X\beta\|_2^2 + \lambda\|\beta\|_2^2
\]

lo que contrae suavemente los coeficientes. En **Lasso** se usa

\[
\|y-X\beta\|_2^2 + \lambda\|\beta\|_1
\]

y esa penalización puede llevar algunos coeficientes exactamente a cero. **Elastic Net** combina ambas ideas y suele ser útil cuando hay grupos de variables correlacionadas.

La intuición general es valiosa: aceptamos introducir algo de sesgo para reducir varianza y ganar generalización. La regularización no es un truco numérico, sino una forma explícita de controlar complejidad.

### 8.8 Cuándo conviene usar regresión lineal

La regresión lineal conviene cuando la interpretabilidad importa, cuando la relación es aproximadamente lineal o puede linearizarse con transformaciones razonables, cuando hace falta un baseline sólido y cuando la complejidad del problema no justifica modelos más opacos. También es muy útil como referencia conceptual frente a técnicas más flexibles.

Compararla con modelos más complejos exige criterio. Un modelo lineal puede perder frente a interacciones y no linealidades fuertes, pero gana en estabilidad, transparencia y facilidad de comunicación. La respuesta madura no opone "simple" a "potente"; analiza el tipo de estructura que cada familia puede capturar y el costo de perder interpretabilidad.

## Capítulo 9. Regresión logística: probabilidad para decisiones binarias

### 9.1 Por qué no alcanza la regresión lineal para clasificar

Si el target es binario, podría tentarnos ajustar una recta y luego umbralizar la salida. El problema es doble. Por un lado, la predicción lineal no está restringida al intervalo \([0,1]\). Por otro, la relación entre covariables y probabilidad rara vez es adecuadamente representada como una función lineal en la escala original.

La regresión logística resuelve esto modelando la probabilidad de la clase positiva mediante la sigmoide

\[
\sigma(z)=\frac{1}{1+e^{-z}}
\]

con

\[
z = \beta_0 + \beta^Tx
\]

De este modo,

\[
P(Y=1 \mid X=x)=\sigma(\beta_0+\beta^Tx)
\]

La salida queda naturalmente entre 0 y 1, y la relación lineal se desplaza a una escala más adecuada.

### 9.2 Odds y logit

La logística se entiende mejor a través de los **odds**:

\[
\text{odds}=\frac{p}{1-p}
\]

Si tomamos logaritmos obtenemos el **logit**:

\[
\log\left(\frac{p}{1-p}\right)=\beta_0+\beta^Tx
\]

Esta ecuación encierra la idea central del modelo: lo que es lineal no es la probabilidad, sino el logaritmo de sus odds. Muchas malas interpretaciones nacen de olvidar este punto y leer los coeficientes como si produjeran cambios lineales directos en probabilidad.

### 9.3 Estimación por máxima verosimilitud

Si cada \(Y_i\) sigue una Bernoulli con probabilidad \(p_i\), la verosimilitud del conjunto de observaciones es

\[
\mathcal{L}(\beta) = \prod_{i=1}^{n} p_i^{y_i}(1-p_i)^{1-y_i}
\]

y su logaritmo es

\[
\ell(\beta)=\sum_{i=1}^{n}\left[y_i\log p_i + (1-y_i)\log(1-p_i)\right]
\]

Maximizar esta expresión equivale a minimizar la log-loss. A diferencia de la regresión lineal, no existe una solución cerrada simple como las ecuaciones normales, y por eso se usan métodos iterativos de optimización. Lo importante pedagógicamente es entender la lógica: elegimos los parámetros que vuelven más plausible la muestra observada bajo el modelo.

### 9.4 Cómo interpretar un coeficiente logístico

Si \(x_j\) aumenta en una unidad, los log-odds cambian en \(\beta_j\). Una lectura más intuitiva surge al exponenciar:

\[
e^{\beta_j}
\]

Ese valor es el factor por el cual se multiplican los odds cuando \(x_j\) aumenta una unidad, manteniendo constantes las demás variables. Si \(e^{\beta_j}=1.5\), los odds aumentan un 50%. Eso no significa que la probabilidad aumente 50 puntos porcentuales. El cambio en probabilidad depende del nivel base en el que estemos operando.

Comprender esa diferencia es clave. La logística es interpretable, pero no admite una lectura ingenua lineal sobre la probabilidad.

### 9.5 Umbral, desbalance y decisión

La regresión logística entrega probabilidades. Convertirlas en clases requiere un umbral, y en problemas desbalanceados usar 0.5 por defecto suele ser una mala decisión. El umbral debe discutirse a la luz del costo de error, del volumen de casos que el sistema puede procesar y de la política operativa que seguirá al modelo.

Por esa misma razón, la logística suele ser un excelente baseline. Es rápida, interpretable, probabilística y estable. En muchos problemas tabulares, una logística regularizada y bien calibrada ofrece una referencia sorprendentemente difícil de superar honestamente.

### 9.6 Regularización y calibración

La logística admite penalizaciones L1 y L2 del mismo modo que la regresión lineal, lo que ayuda frente a alta dimensionalidad, colinealidad y sobreajuste. Pero además conviene recordar que buena discriminación no implica buena calibración. Dos modelos pueden ordenar casos de manera parecida y, aun así, diferir mucho en la calidad de las probabilidades que emiten.

En problemas de riesgo esa diferencia es central. A veces el orden basta; otras veces se necesita una probabilidad razonablemente interpretable para fijar políticas, precios o niveles de intervención. La calibración no es un lujo estadístico, sino una propiedad funcional del modelo.

### 9.7 Cuándo falla la intuición lineal

Muchos estudiantes piensan la logística como "una lineal para clasificar". La intuición ayuda al comienzo, pero si se la toma literalmente conduce a errores. La linealidad vive en el espacio del logit, no en la probabilidad. Por eso el efecto marginal de una variable sobre \(P(Y=1)\) depende del punto del espacio en que se evalúe.

Ese matiz marca la diferencia entre usar el modelo como receta y entenderlo de verdad. La logística es simple, sí, pero no trivial.

## Capítulo 10. KNN, Naive Bayes y SVM: tres lógicas distintas de aprendizaje

### 10.1 K-Nearest Neighbors: aprender por vecindad

KNN parte de una idea intuitiva y potente: casos parecidos deberían comportarse de manera parecida. Para predecir un nuevo punto, el algoritmo busca los \(k\) vecinos más cercanos en el espacio de features y decide a partir de ellos. En clasificación suele usar voto mayoritario; en regresión, promedio de las respuestas vecinas.

La simplicidad es engañosa. Todo depende de cómo se defina "cercanía". Una distancia euclídea

\[
d(x,x')=\sqrt{\sum_{j=1}^{p}(x_j-x'_j)^2}
\]

puede ser razonable en algunos problemas y pésima en otros. Si las variables no están escaladas o si abundan dimensiones irrelevantes, la vecindad pierde sentido. El parámetro \(k\) controla además un compromiso clásico: valores bajos adaptan mejor estructuras finas pero aumentan varianza; valores altos suavizan el modelo y aumentan sesgo.

### 10.2 Naive Bayes: independencia condicional como aproximación útil

Naive Bayes se apoya en el teorema de Bayes:

\[
P(Y \mid X)=\frac{P(X \mid Y)P(Y)}{P(X)}
\]

Su versión "naive" asume independencia condicional entre features dada la clase:

\[
P(X_1,\ldots,X_p \mid Y)=\prod_{j=1}^{p}P(X_j \mid Y)
\]

Entonces,

\[
P(Y \mid X_1,\ldots,X_p)\propto P(Y)\prod_{j=1}^{p}P(X_j \mid Y)
\]

El supuesto es fuerte y rara vez exacto. Sin embargo, el modelo puede funcionar muy bien porque convierte un problema complejo en uno tratable y, en muchos dominios, conserva suficiente señal relevante. Su gran lección es epistemológica: un supuesto claramente falso puede seguir siendo útil si organiza bien la parte importante de la realidad.

### 10.3 SVM: margen máximo y robustez geométrica

Las máquinas de soporte vectorial buscan separar clases mediante un hiperplano que maximice el margen respecto de los puntos más cercanos de cada clase, los llamados **support vectors**. Si los datos son separables, se busca \(w\) y \(b\) tales que

\[
y_i(w^Tx_i+b)\ge 1
\]

para todos los puntos, minimizando al mismo tiempo

\[
\frac{1}{2}\|w\|^2
\]

Minimizar la norma de \(w\) equivale a maximizar el margen. La intuición es que fronteras con más margen tienden a generalizar mejor.

Cuando la separabilidad perfecta no existe, se introducen variables de holgura \(\xi_i\) y un parámetro \(C\):

\[
\min_{w,b,\xi}\frac{1}{2}\|w\|^2 + C\sum_{i=1}^{n}\xi_i
\]

Ese parámetro controla cuánto penalizamos violaciones del margen. Allí aparece el equilibrio entre tolerar errores de entrenamiento y evitar fronteras demasiado rígidas.

### 10.4 Kernel trick y no linealidad

La potencia de SVM crece enormemente con los kernels. Un kernel permite calcular productos internos en un espacio transformado de alta dimensión sin construir explícitamente esa transformación. Eso hace posible obtener fronteras no lineales con una formulación todavía controlada.

El kernel RBF es uno de los más usados. Sus hiperparámetros, en particular \(C\) y \(\gamma\), regulan la complejidad efectiva de la frontera. Valores altos de \(\gamma\) vuelven la influencia de cada punto muy local y pueden conducir a sobreajuste. La virtud de SVM no está solo en su rendimiento, sino en la nitidez con la que muestra cómo una idea geométrica puede convertirse en un método poderoso.

### 10.5 Comparación conceptual entre KNN, Naive Bayes y SVM

Estos tres modelos enseñan tres formas muy distintas de aprender. KNN delega el conocimiento en la estructura local de los ejemplos de entrenamiento. Naive Bayes adopta una simplificación probabilística fuerte para volver tratable el problema. SVM construye una frontera geométrica que privilegia el margen.

Saber compararlos no consiste en enumerar ventajas y desventajas, sino en entender qué sesgo inductivo trae cada uno. KNN supone que la cercanía geométrica es informativa. Naive Bayes supone independencia condicional aproximada. SVM supone que una buena frontera, quizá en un espacio transformado, puede separar con margen. Comprender ese sesgo es mucho más valioso que memorizar una tabla.

## Parte VI. Árboles y ensambles: potencia, no linealidad y control de varianza

Las familias lineales y de margen máximo no agotan la práctica moderna. En datos tabulares, los árboles y sus ensambles ocupan un lugar central porque capturan interacciones y no linealidades con gran eficacia. Pero su verdadera importancia pedagógica está en otra parte: obligan a pensar cómo se controla la complejidad de modelos muy flexibles y cómo se gana estabilidad sin resignar demasiado poder predictivo.

## Capítulo 11. Árboles de decisión: reglas interpretables con riesgo de inestabilidad

### 11.1 La idea básica de particionar recursivamente

Un árbol de decisión divide el espacio de features mediante preguntas del tipo "¿\(x_j < t\)?". Cada división intenta producir subconjuntos cada vez más homogéneos respecto del target. Repetida recursivamente, esta lógica construye una colección de reglas if-then que puede ser muy interpretable.

La gran ventaja de los árboles es que capturan interacciones y no linealidades sin pedir demasiadas transformaciones previas. Su gran debilidad es la inestabilidad: pequeños cambios en la muestra pueden alterar mucho la estructura aprendida. Esa combinación entre claridad local y fragilidad global define buena parte de su comportamiento.

### 11.2 Entropía, información y algoritmo ID3

Una manera clásica de medir impureza en clasificación es la entropía:

\[
H(S) = -\sum_{k=1}^{K} p_k \log_2 p_k
\]

donde \(p_k\) es la proporción de la clase \(k\) en el nodo \(S\). Si el nodo es puro, la entropía vale cero. Si las clases están mezcladas, la entropía aumenta. El algoritmo **ID3** elige la partición que maximiza la reducción esperada de entropía, llamada ganancia de información.

La idea profunda es que el árbol no busca reglas "bonitas" ni "intuitivas", sino divisiones que reduzcan incertidumbre respecto del target. La lectura correcta no es solo algorítmica, sino informacional.

### 11.3 Gini, CART y C4.5

Otra medida muy utilizada es el índice de **Gini**:

\[
Gini(S) = 1 - \sum_{k=1}^{K} p_k^2
\]

Su interpretación es parecida: nodos más puros tienen menor impureza. El algoritmo **CART** suele usar Gini en clasificación y reducción del error cuadrático en regresión. **C4.5**, por su parte, mejora sobre ID3 al manejar variables continuas, faltantes y una medida de partición más equilibrada llamada gain ratio.

El punto importante es que distintos árboles no solo difieren por detalles de implementación. Cambian también en cómo definen "buen split" y, por lo tanto, en el tipo de estructura que tienden a privilegiar.

### 11.4 Árboles para regresión

En regresión la lógica general es la misma, pero la impureza se mide mediante dispersión o error cuadrático dentro de cada nodo. El árbol busca divisiones que hagan más homogéneos los valores de la respuesta en los hijos y, al final, suele predecir la media del target en cada hoja.

Esto convierte al árbol de regresión en un modelo por regiones: el espacio de features queda particionado y cada región recibe una predicción constante. Esa forma explica tanto su flexibilidad para capturar no linealidades como su tendencia a producir superficies escalonadas.

### 11.5 Crecimiento, poda y sobreajuste

Si se deja crecer un árbol sin restricciones, es muy probable que termine memorizando detalles accidentales del conjunto de entrenamiento. La **pre-poda** limita profundidad, tamaño mínimo de nodo o mejora mínima por split. La **post-poda** permite crecer más y luego recorta subárboles cuya complejidad no se justifica.

La lección general va más allá de los árboles. Un modelo interpretable no es útil solo por ser legible; también debe generalizar. La poda es la forma en que esta familia reconoce que la claridad aparente de un árbol muy profundo puede ser, en realidad, una forma visual de sobreajuste.

### 11.6 Importancia de variables y cautelas

Los árboles ofrecen medidas de importancia basadas en la reducción acumulada de impureza. Son útiles, pero no inocentes. Tienden a favorecer variables con muchos puntos de corte posibles o alta cardinalidad. Además, si varias columnas comparten señal, la importancia puede repartirse de manera arbitraria entre ellas.

Por eso conviene complementar con permutation importance, estabilidad entre reentrenamientos y lectura sustantiva. Una variable importante para el árbol no es automáticamente una variable causal ni la mejor candidata para intervenir en el mundo real.

### 11.7 Cuándo usar árboles

Los árboles son atractivos cuando importa la interpretabilidad, cuando se esperan interacciones y no linealidades, cuando hay mezcla de variables numéricas y categóricas y cuando se desea una primera aproximación muy legible del problema. Como modelo único, suelen perder frente a ensambles bien construidos. Pero como herramienta conceptual y como baseline interpretable siguen siendo valiosos.

Elegir un árbol no es resignar performance por claridad de forma automática. Es priorizar una combinación particular de legibilidad, flexibilidad local y riesgo de inestabilidad.

## Capítulo 12. Ensambles: combinar modelos para ganar estabilidad o capacidad

### 12.1 La intuición estadística detrás de los ensambles

Si varios modelos se equivocan de manera distinta, combinarlos puede reducir error total. Esa es la intuición central de los ensambles. En forma simplificada, si \(M\) predictores tienen varianza individual \(\sigma^2\) y correlación promedio \(\rho\), la varianza de su promedio puede escribirse como

\[
\text{Var}(\bar f) = \rho\sigma^2 + \frac{1-\rho}{M}\sigma^2
\]

La fórmula enseña dos cosas a la vez. Promediar ayuda. Pero ayuda mucho más cuando los modelos no son demasiado parecidos entre sí. No basta con tener muchos modelos; hace falta también diversidad.

### 12.2 Bagging

El **bagging** entrena muchos modelos sobre muestras bootstrap del conjunto de entrenamiento y luego agrega sus predicciones. Su efecto principal es reducir varianza, especialmente cuando el modelo base es inestable, como ocurre con los árboles.

Cada muestra bootstrap toma \(n\) observaciones con reemplazo desde el conjunto original de tamaño \(n\). Algunas se repiten y otras quedan afuera. Esas observaciones omitidas pueden usarse como validación interna aproximada mediante el esquema out-of-bag. La belleza del bagging está en su sencillez: aprovecha la inestabilidad del modelo base para transformarla en diversidad útil.

### 12.3 Random Forest

Random Forest agrega una capa más de aleatoriedad al bagging de árboles. En cada split solo se considera un subconjunto aleatorio de variables candidatas. Esta restricción reduce la correlación entre árboles, lo cual vuelve más efectivo el promedio final.

El resultado suele ser un modelo robusto, competitivo y relativamente poco sensible a tuning fino, especialmente en datos tabulares. Su costo es una pérdida de interpretabilidad respecto del árbol único. Pero precisamente ahí aparece su interés pedagógico: muestra cómo sacrificar algo de claridad local puede devolver mucha estabilidad global.

### 12.4 Boosting: aprender corrigiendo errores previos

Mientras bagging trabaja en paralelo para reducir varianza, el **boosting** construye un modelo secuencialmente, corrigiendo errores de etapas previas. En AdaBoost se reponderan observaciones difíciles. En Gradient Boosting, cada nuevo modelo se ajusta a los residuos o, más precisamente, al gradiente negativo de la pérdida actual.

En forma funcional,

\[
F_m(x)=F_{m-1}(x)+\eta h_m(x)
\]

donde \(F_m\) es el ensamble acumulado, \(h_m\) el nuevo modelo débil y \(\eta\) la tasa de aprendizaje. La idea es profunda: en lugar de ajustar un único modelo grande, construimos gradualmente una función cada vez más capaz de corregir sus propias limitaciones.

### 12.5 XGBoost y regularización moderna en árboles

XGBoost es una realización particularmente eficiente y regularizada del gradient boosting sobre árboles. Su objetivo puede expresarse, en forma esquemática, como

\[
\text{Obj} = \sum_{i=1}^{n} l(y_i,\hat y_i) + \sum_{k=1}^{K}\Omega(f_k)
\]

donde \(l\) mide el error por observación y \(\Omega(f_k)\) penaliza la complejidad de cada árbol del ensamble. Una forma común de esa penalización es

\[
\Omega(f)=\gamma T + \frac{\lambda}{2}\sum_{j=1}^{T} w_j^2
\]

Lo importante no es recordar la fórmula exacta, sino entender su lógica: el boosting moderno no solo agrega árboles, también regula explícitamente cuánto pueden complicarse.

### 12.6 Voting, stacking, cascading, ensambles homogéneos e híbridos

No todo ensamble es bagging o boosting. En **voting**, varios modelos votan o promedian probabilidades. En **stacking**, las predicciones de modelos base se convierten en entrada de un meta-modelo que aprende a combinarlas. Para que el stacking sea honesto, esas predicciones deben generarse fuera de fold; de lo contrario, el meta-modelo aprende sobre leakage.

También existen estructuras en cascada, donde la salida de una etapa condiciona la siguiente, y ensambles híbridos que mezclan familias distintas de modelos. La idea de fondo es siempre la misma: explotar complementariedades sin olvidar que cada mecanismo de combinación introduce también nuevos riesgos de complejidad y sobreajuste.

### 12.7 Performance versus interpretabilidad

Los ensambles suelen ganar performance, robustez y capacidad de capturar estructura compleja. Pero esa ganancia rara vez es gratuita: la transparencia inmediata disminuye. La tensión entre desempeño e interpretabilidad no debe negarse ni resolverse con frases vacías. Debe explicitarse.

En algunos problemas la mejora predictiva justifica el costo interpretativo. En otros, un árbol podado o una logística regularizada pueden ser preferibles aunque pierdan algunos puntos de métrica. La respuesta madura no busca el mejor modelo en abstracto, sino el mejor compromiso para el problema concreto.

## Parte VII. Aprendizaje no supervisado y reducción de dimensión

No todos los problemas vienen con etiquetas. A veces la tarea consiste en descubrir estructura, resumir información o construir una representación más compacta. Esta parte estudia dos familias fundamentales para esos escenarios: clustering y reducción de dimensionalidad. Ambas son especialmente peligrosas cuando se las usa de manera automática, porque obligan a preguntarse qué significa "parecido" y qué estructura queremos preservar.

## Capítulo 13. Clustering: descubrir estructura sin etiquetas

### 13.1 Qué problema intenta resolver el clustering

Clustering no es adivinar clases ocultas que supuestamente ya existen en la naturaleza. Es construir una partición del espacio de datos tal que las observaciones del mismo grupo resulten más parecidas entre sí que a las de otros grupos, según una noción elegida de similitud.

Esta aclaración es crucial. Los clusters no son entidades ontológicas garantizadas por la realidad. Son estructuras dependientes de la representación, la escala, la métrica y el algoritmo. Por eso agrupar bien exige, antes que nada, saber qué significa "parecido" en el problema que tenemos delante.

### 13.2 Distancia, escala y significado de "parecido"

En datos numéricos suele aparecer la distancia euclídea como elección natural, pero no siempre es adecuada. En datos mixtos, categóricos o textuales puede ser preferible otra noción de disimilitud. Además, si las variables están en escalas muy diferentes, una de ellas puede dominar la distancia total y distorsionar la agrupación.

El clustering obliga a pensar con particular claridad la representación. Agrupar sobre variables mal escaladas o sobre una mezcla de señales relevantes y ruido irrelevante produce grupos aparentemente nítidos, pero conceptualmente vacíos. El algoritmo no corrige una mala noción de parecido; la amplifica.

### 13.3 Tendencia al clustering y estadístico de Hopkins

Antes de elegir un algoritmo, puede ser útil preguntarse si el dataset muestra alguna tendencia real a formar agrupamientos o si se parece más a una nube sin estructura clara. El estadístico de **Hopkins** es una herramienta clásica para esa pregunta. Compara distancias entre puntos reales y distancias generadas desde puntos aleatorios al dataset observado.

Valores cercanos a 1 sugieren tendencia a clustering; valores cercanos a 0.5 sugieren una estructura más aleatoria. No es un veredicto absoluto, pero cumple una función importante: recordarnos que agrupar no siempre es una decisión justificada.

### 13.4 K-means en profundidad

K-means busca particionar las observaciones en \(K\) grupos minimizando la suma de cuadrados intra-cluster:

\[
\min_{C_1,\ldots,C_K}\sum_{k=1}^{K}\sum_{x_i \in C_k}\|x_i-\mu_k\|^2
\]

donde \(\mu_k\) es el centroide del cluster \(C_k\). El algoritmo alterna dos pasos: asigna cada punto al centroide más cercano y luego recalcula cada centroide como promedio de los puntos asignados.

La idea es simple, pero sus consecuencias son profundas. K-means supone grupos aproximadamente convexos y definidos por distancia a un centro. Además, solo garantiza convergencia a un mínimo local, de modo que la inicialización importa. Por eso suelen usarse varios reinicios o estrategias como k-means++.

### 13.5 Un ejemplo mínimo para entender K-means

Si tomamos los puntos unidimensionales \(1,2,3,10,11,12\) y fijamos \(K=2\), el algoritmo tiende a encontrar naturalmente los grupos \(\{1,2,3\}\) y \(\{10,11,12\}\). El ejemplo es trivial, pero enseña algo importante: K-means no "descubre" la verdad de los grupos, sino la partición que mejor optimiza una función bajo una geometría concreta.

Cuando los grupos están menos separados o aparecen outliers, diferentes inicializaciones pueden conducir a soluciones distintas. Esa sensibilidad muestra por qué la estabilidad también es parte de la evaluación en no supervisado.

### 13.6 Elegir \(K\): codo, silhouette y estabilidad

Elegir el número de clusters no es un acto mágico. El método del **codo** observa cómo disminuye la inercia intra-cluster al aumentar \(K\) y busca un punto a partir del cual la mejora marginal se aplana. El índice silhouette evalúa cohesión y separación. El análisis de estabilidad pregunta cuánto cambian los grupos bajo pequeñas perturbaciones o remuestreos.

Ninguno de estos criterios reemplaza al dominio. Puede haber varios valores de \(K\) defendibles según la granularidad que interese. También puede ocurrir que el mejor \(K\) estadístico no sea el más útil para la acción posterior. En clustering, la decisión metodológica nunca queda cerrada por un único número.

### 13.7 Clustering jerárquico

El clustering jerárquico no fija \(K\) desde el principio. Construye una secuencia de fusiones o particiones que puede visualizarse mediante un dendrograma. En su versión aglomerativa comienza con cada observación como cluster propio y luego fusiona grupos según una regla de enlace.

Single linkage favorece cadenas; complete linkage favorece grupos compactos; average linkage intermedia; Ward tiende a minimizar el aumento de varianza intra-cluster. Su fortaleza principal es la visión multiescala: permite explorar estructuras a distintos niveles de resolución. Su costo, especialmente en bases grandes, es computacional y también interpretativo.

### 13.8 DBSCAN: densidad, ruido y formas arbitrarias

DBSCAN define clusters como regiones densas separadas por regiones de baja densidad. Requiere dos parámetros, \(\varepsilon\) y \(minPts\), y tiene dos virtudes fuertes: detecta grupos de forma arbitraria y etiqueta explícitamente observaciones como ruido.

Esa misma virtud trae una dificultad. Si la densidad varía mucho entre regiones, un único \(\varepsilon\) puede ser demasiado pequeño para algunos grupos y demasiado grande para otros. DBSCAN es una herramienta excelente cuando la idea de densidad coincide con la estructura del fenómeno; fuera de ese caso, su comportamiento puede degradarse rápidamente.

### 13.9 Interpretar clusters y evitar errores frecuentes

Obtener clusters es apenas el comienzo. Luego hay que describirlos, compararlos y preguntarse si habilitan decisiones distintas. Para eso suele ser útil mirar centroides, distribuciones características y variables que diferencian grupos. Pero la interpretación debe mantenerse disciplinada: es fácil inventar relatos sobre clusters que en realidad son artefactos de la escala o del algoritmo.

Los errores más frecuentes son bastante estables: creer que todo cluster hallado es real, elegir \(K\) solo por un índice, ignorar la sensibilidad a la representación y confundir grupos construidos algorítmicamente con clases verdaderas del fenómeno. Clustering exige, quizá más que otros temas, humildad interpretativa.

## Capítulo 14. Reducción de dimensionalidad: comprimir sin perder estructura esencial

### 14.1 Por qué reducir dimensión

Reducir dimensión no sirve solo para "dibujar en 2D". También puede ayudar a combatir ruido, colinealidad, costos computacionales, dificultad de visualización y degradación de algoritmos basados en distancias. En muchos problemas la complejidad efectiva de los datos es menor que el número bruto de variables, y encontrar esa estructura más compacta mejora tanto el análisis como el modelado posterior.

La pregunta central es siempre la misma: qué estructura queremos preservar. Varianza global, distancias, vecindades locales o geometría de variedad no son objetivos equivalentes. Elegir una técnica de reducción equivale a elegir cuál de esos aspectos sacrificar lo menos posible.

### 14.2 Maldición de la dimensionalidad

Cuando la dimensión crece, muchas intuiciones geométricas de baja dimensión dejan de funcionar. Los puntos tienden a volverse todos lejanos entre sí, los volúmenes crecen de manera explosiva y la noción de vecindad pierde nitidez. Este fenómeno, conocido como **maldición de la dimensionalidad**, explica por qué KNN, ciertos métodos de clustering y algunas medidas de densidad se degradan en espacios de alta dimensión.

Reducir dimensión no elimina mágicamente el problema, pero puede mitigarlo si la estructura relevante del dato vive cerca de un subespacio o de una variedad de menor dimensión efectiva. Esa es la esperanza que justifican muchas técnicas de compresión.

### 14.3 PCA: idea, formulación y geometría

El análisis de componentes principales busca nuevas direcciones ortogonales que capturen la mayor varianza posible. Si \(X\) es la matriz de datos centrada, la primera componente principal resuelve

\[
\max_{\|w\|=1}\text{Var}(Xw)
\]

lo cual equivale a maximizar

\[
w^T\Sigma w
\]

donde \(\Sigma\) es la matriz de covarianza. La solución lleva a los autovectores de \(\Sigma\), y el asociado al mayor autovalor define la primera componente.

Esta formulación deja una idea importante: PCA no busca variables predictivas ni componentes "útiles" en sentido de negocio. Busca direcciones de máxima varianza global bajo una restricción lineal y ortogonal. Ese objetivo es potente, pero no debe confundirse con otros.

### 14.4 Scores, loadings y varianza explicada

Los **loadings** indican cuánto pesa cada variable original en cada componente. Los **scores** son las coordenadas de las observaciones proyectadas sobre esas nuevas direcciones. Si los autovalores son \(\lambda_1,\ldots,\lambda_p\), la proporción de varianza explicada por la componente \(j\) es

\[
\frac{\lambda_j}{\sum_{m=1}^{p}\lambda_m}
\]

El scree plot ayuda a decidir cuántas componentes conservar, pero no existe un umbral mágico universal. Conservar 90%, 95% o 99% de varianza puede ser razonable o absurdo según el problema. Lo importante es relacionar ese corte con el objetivo de la reducción.

### 14.5 Qué preserva PCA y qué no

PCA preserva, del mejor modo posible bajo su restricción, varianza global lineal. No preserva necesariamente separabilidad de clases, estructura no lineal ni interpretabilidad sustantiva. Una variable con poca varianza puede ser altamente predictiva y quedar relegada por PCA. Además, el método depende de la escala: si una variable domina por sus unidades, dominará también las primeras componentes si no se estandariza.

Por eso PCA no debe aplicarse como gesto automático. Es una herramienta muy valiosa cuando su objetivo coincide con el del análisis. Fuera de ese caso, puede comprimir precisamente lo que no convenía sacrificar.

### 14.6 Relación entre PCA, ruido, colinealidad y overfitting

Cuando muchas variables están fuertemente correlacionadas, PCA puede condensar esa redundancia en pocas componentes y estabilizar modelos posteriores. También puede ayudar a filtrar ruido distribuido en direcciones de baja varianza. En ese sentido, funciona como herramienta de compresión y, a veces, de regularización implícita.

El costo es interpretativo. Las componentes son combinaciones lineales, no variables originales fáciles de comunicar. La ganancia en estabilidad debe sopesarse contra la pérdida de claridad. Reducir dimensión nunca es gratis: cambia lo que podemos explicar.

### 14.7 MDS: preservar distancias

El **multidimensional scaling** parte de una matriz de distancias o disimilitudes y busca una configuración en baja dimensión que las preserve lo mejor posible. A diferencia de PCA, no prioriza varianza global, sino proximidades entre observaciones. En su versión métrica suele minimizar una medida de stress como

\[
\text{Stress}=\sqrt{\frac{\sum_{i<j}(d_{ij}-\delta_{ij})^2}{\sum_{i<j}\delta_{ij}^2}}
\]

donde \(\delta_{ij}\) son las disimilitudes originales y \(d_{ij}\) las distancias en el espacio reducido. La interpretación es directa: queremos deformar lo menos posible la geometría relacional del conjunto.

### 14.8 t-SNE: vecindades locales para visualización

t-SNE es una técnica no lineal orientada principalmente a visualización. Su objetivo es preservar vecindades locales: puntos cercanos en alta dimensión deberían permanecer relativamente cercanos en baja dimensión. Esa prioridad la vuelve muy poderosa para mostrar estructura local, pero también muy fácil de malinterpretar.

Las distancias globales en un mapa t-SNE no deben leerse como si fueran fieles. Que dos grupos aparezcan muy separados en el gráfico no significa necesariamente que estén tan lejos en el espacio original. Además, hiperparámetros como la perplexity influyen mucho en el resultado. t-SNE es excelente para explorar; es mala como prueba concluyente de cuántos clusters "hay".

### 14.9 ISOMAP: variedades y distancias geodésicas

ISOMAP parte de la hipótesis de que los datos viven sobre una variedad no lineal embebida en alta dimensión. En lugar de preservar distancias euclídeas directas, intenta preservar distancias geodésicas aproximadas sobre esa variedad, construidas a partir de un grafo de vecinos.

Cuando esa hipótesis es razonable, ISOMAP puede "desenrollar" estructuras que PCA no capta. Su debilidad es la sensibilidad a ruido, desconexiones y a una mala construcción del grafo de vecindad. Como toda técnica no lineal, exige que el supuesto geométrico tenga sentido en el problema concreto.

### 14.10 Cómo comparar PCA, MDS, t-SNE e ISOMAP

PCA privilegia varianza global lineal. MDS privilegia preservación de distancias. t-SNE privilegia vecindades locales para visualización. ISOMAP privilegia geometría de variedad mediante distancias geodésicas. Las cuatro técnicas reducen dimensión, pero no preservan la misma estructura ni sirven para la misma tarea.

Confundirlas como si todas "sirvieran para bajar columnas" es un error conceptual serio. La pregunta correcta siempre es: qué estructura quiero conservar y para qué usaré esa representación reducida.

## Parte VIII. Redes neuronales y representación de texto

Las técnicas estudiadas hasta aquí muestran muchas formas de aprender, pero todavía mantienen una estructura de representación relativamente explícita. En redes neuronales y NLP aparece con fuerza otra idea: aprender o construir representaciones cada vez más útiles a partir de objetos originalmente difíciles de modelar, como imágenes, audio o texto. Esta parte introduce esa lógica sin perder de vista el criterio metodológico general.

## Capítulo 15. Redes neuronales: de la linealidad a representaciones jerárquicas

### 15.1 El perceptrón simple

La neurona artificial más elemental calcula una combinación lineal de las entradas y aplica luego una función de activación:

\[
a=\phi(w^Tx+b)
\]

donde \(w\) son los pesos, \(b\) el sesgo y \(\phi\) una no linealidad. El perceptrón simple puede verse como un clasificador lineal. Si las clases son linealmente separables, existe un hiperplano que el perceptrón puede aprender mediante ajustes sucesivos basados en el error.

Una regla clásica de actualización toma la forma

\[
w \leftarrow w + \eta (y-\hat y)x
\]

y su interpretación es transparente: si el ejemplo fue clasificado de manera incorrecta, los pesos se corrigen en la dirección que vuelve más compatible la salida con la etiqueta observada.

### 15.2 La limitación del perceptrón y el problema XOR

El perceptrón simple no puede resolver problemas no linealmente separables. El ejemplo canónico es XOR. Su valor pedagógico es enorme porque muestra que acumular linealidades no crea no linealidad. Una composición de transformaciones lineales sigue siendo lineal.

De ahí surge la necesidad de funciones de activación no lineales y de varias capas. La limitación del perceptrón no es un detalle histórico: es el punto exacto donde aparece la idea de representación jerárquica.

### 15.3 Perceptrón multicapa y composición de funciones

Un perceptrón multicapa construye representaciones intermedias:

\[
h^{(1)} = \phi(W^{(1)}x+b^{(1)}), \qquad
h^{(2)} = \phi(W^{(2)}h^{(1)}+b^{(2)}), \qquad \ldots
\]

hasta llegar a una salida final \(\hat y\). Cada capa transforma la representación previa y permite capturar estructuras cada vez más complejas.

La intuición importante no es solo que "más capas aprenden más", sino que cada capa puede descubrir patrones útiles para la siguiente. En ese sentido, las redes neuronales no solo ajustan una función; aprenden una secuencia de representaciones.

### 15.4 Función de pérdida y descenso por gradiente

Como en otros modelos, entrenar una red significa minimizar una pérdida \(\mathcal{L}(\theta)\), donde \(\theta\) reúne todos los pesos y sesgos. El descenso por gradiente actualiza parámetros según

\[
\theta_{t+1} = \theta_t - \eta \nabla_{\theta}\mathcal{L}(\theta_t)
\]

La tasa de aprendizaje \(\eta\) controla el tamaño del paso. Si es muy grande, el entrenamiento puede oscilar o divergir. Si es muy pequeña, el avance puede volverse desesperantemente lento. La optimización en redes no es un detalle técnico periférico: condiciona fuertemente qué tipo de solución llegamos a encontrar.

### 15.5 Backpropagation: por qué funciona

Backpropagation no es magia ni una receta a memorizar. Es la aplicación sistemática de la regla de la cadena para calcular de manera eficiente cómo afecta cada parámetro a la pérdida final. Como la salida depende de capas intermedias, y esas capas dependen de otras anteriores, el efecto de un peso temprano debe propagarse a través de toda la red.

El algoritmo resuelve un problema computacional muy concreto: evitar recalcular de manera ingenua una enorme cantidad de derivadas repetidas. Entender esto vale mucho más que recordar el nombre del procedimiento.

### 15.6 Optimizadores: SGD, momentum y Adam

El gradiente puede calcularse sobre todo el dataset, sobre mini-batches o incluso observación por observación. En la práctica, los mini-batches ofrecen un buen equilibrio entre estabilidad y costo computacional. Sobre esa base aparecen variantes como **momentum**, que suaviza oscilaciones acumulando dirección, y **Adam**, que adapta la magnitud de los pasos usando estimaciones móviles del gradiente.

No cambian el objetivo de fondo, pero sí la dinámica con la que recorremos el paisaje de optimización. Esa dinámica puede marcar la diferencia entre entrenar una red útil y quedar atrapados en una mala solución o en una convergencia ineficiente.

### 15.7 Problemas de entrenamiento y regularización

Las redes profundas pueden sufrir gradientes que se desvanecen o explotan, sobreajuste, dependencia fuerte de la inicialización y sensibilidad a hiperparámetros. Por eso aparecen técnicas como ReLU, batch normalization, weight decay, dropout, early stopping o data augmentation.

Cada una responde a un problema concreto. Dropout fuerza a que la red no dependa demasiado de trayectorias específicas de activación. Early stopping corta el entrenamiento cuando la validación deja de mejorar. Weight decay penaliza pesos grandes. La enseñanza importante es que entrenar redes no es solo elegir una arquitectura: es también controlar el proceso de aprendizaje.

### 15.8 SOM: mapas autoorganizados

Los **Self-Organizing Maps** representan otra cara de las redes: la del aprendizaje no supervisado orientado a preservar cierta topología. Proyectan datos de alta dimensión sobre una grilla, intentando que observaciones parecidas queden cercanas también en el mapa resultante.

Aunque hoy tengan menos protagonismo que otras arquitecturas, siguen siendo conceptualmente valiosos porque muestran que la idea de red neuronal no se agota en clasificar o predecir. También puede consistir en organizar representaciones.

### 15.9 Cuándo usar deep learning y cuándo no

El aprendizaje profundo brilla especialmente con grandes volúmenes de datos no estructurados y con tareas donde la representación jerárquica aporta una ventaja clara, como visión, audio o lenguaje. En muchos problemas tabulares medianos, sin embargo, ensambles de árboles pueden igualar o superar a redes con menor costo y mayor interpretabilidad.

Usar deep learning por prestigio y no por necesidad es un error frecuente. Elegirlo bien exige preguntar si el problema realmente necesita la complejidad adicional que trae consigo.

## Capítulo 16. NLP aplicado: del texto crudo a una representación útil

### 16.1 Por qué el texto es un dato especial

El texto no llega al modelo como un vector numérico listo para usar, sino como una secuencia simbólica cargada de ambigüedad, contexto, ironía, polisemia y variación morfológica. La primera tarea del NLP consiste, por eso, en construir una representación adecuada. En texto, representar bien suele ser más decisivo que la elección del clasificador final.

Además, el lenguaje arrastra historia social, convenciones de género discursivo, ruido de escritura y cambios de dominio. Trabajar con texto obliga a recordar constantemente que los datos no son simplemente números esperando ser procesados.

### 16.2 Pipeline clásico de texto

Un pipeline clásico incluye tokenización, normalización, manejo de stopwords, stemming o lematización, construcción de n-gramas y vectorización. Pero no existe un preprocesamiento universalmente correcto. Eliminar negaciones puede destruir señal en análisis de sentimiento. Un stemming agresivo puede colapsar palabras distintas. Mantener signos de puntuación puede ser irrelevante en una tarea y muy informativo en otra.

El criterio correcto es siempre el mismo: cada decisión de preprocesamiento debe justificarse por el tipo de tarea y por la representación que queremos preservar.

### 16.3 Bag of Words

En **Bag of Words** cada documento se representa por el conteo de términos de un vocabulario. Se pierde el orden, pero se conserva qué palabras aparecen y con qué frecuencia. La simplificación parece brutal y, en cierto sentido, lo es. Sin embargo, funciona sorprendentemente bien en muchas tareas porque una parte considerable de la señal discriminativa se encuentra en la presencia o ausencia de ciertos términos.

La lección aquí es pedagógica y metodológica a la vez: una representación simple puede ser muy eficaz si retiene la estructura verdaderamente relevante para la tarea.

### 16.4 TF-IDF

TF-IDF refina Bag of Words ponderando los términos por frecuencia local y rareza global:

\[
\text{tfidf}(t,d)=\text{tf}(t,d)\cdot \log\left(\frac{N}{df(t)}\right)
\]

La intuición es clara. Si una palabra aparece mucho en un documento pero también en casi todos los documentos, discrimina poco. Si aparece mucho en un documento y poco en el corpus, probablemente aporte señal. TF-IDF no "entiende" el texto, pero mejora notablemente la representación para muchas tareas clásicas.

### 16.5 Naive Bayes multinomial en texto

Naive Bayes multinomial es especialmente natural en representación de conteos o frecuencias. Aunque el supuesto de independencia entre palabras sea fuerte, el modelo rinde bien a menudo porque aprovecha muy bien la estructura dispersa del texto vectorizado.

Además, ofrece una interpretación pedagógica valiosa: cada término actúa como evidencia a favor o en contra de una clase. Esa lectura ayuda a entender por qué modelos aparentemente simples pueden ser competitivos en NLP clásico.

### 16.6 Lexicones y análisis de sentimiento

Los enfoques basados en lexicones asignan polaridad o intensidad emocional a palabras y luego agregan esa información para estimar sentimiento. Funcionan como baseline y pueden ser útiles en dominios acotados, pero tropiezan rápido con negaciones, ironía, contexto y polisemia.

Una expresión como "no estuvo nada mal" basta para mostrar el problema. Las palabras por separado no alcanzan para recuperar el sentido composicional. Los lexicones enseñan algo importante precisamente por sus límites: el lenguaje no es una bolsa de términos con signo fijo.

### 16.7 Texto para clasificación y para regresión

El texto no se usa solo para clasificar. También puede ser insumo de problemas de regresión, como estimar complejidad de tickets, duración esperada de un trámite o valoración numérica de una reseña. En esos casos aparece una cuestión adicional: cómo formular correctamente el target. A veces una regresión es natural; otras conviene pensar en categorías ordinales o en ranking.

Este tipo de problemas es pedagógicamente rico porque obliga a integrar representación textual, elección del target, métrica y validación. En NLP, como en el resto de la ciencia de datos, el modelo nunca está solo.

### 16.8 Embeddings y representaciones distribuidas

Los embeddings densos, como Word2Vec o GloVe, representan palabras en espacios continuos donde cierta proximidad geométrica captura similitud semántica. Los modelos contextualizados, como BERT y sus derivados, van más lejos: la representación de una palabra cambia según el contexto en el que aparece.

La historia general del NLP puede leerse como una historia de representaciones cada vez más expresivas. Incluso si el curso trabaja sobre todo con BoW y TF-IDF, entender esta evolución ayuda a ver por qué la representación es el verdadero corazón del campo.

### 16.9 Riesgos y sesgos en NLP

El lenguaje contiene sesgos sociales, culturales e históricos. Un modelo entrenado sobre corpus sesgados puede reproducirlos o amplificarlos. Además, ironía, jergas, dominio específico o cambios de contexto pueden degradar severamente el desempeño.

Por eso el NLP serio no termina en una métrica promedio. Requiere análisis de errores, revisión cualitativa de ejemplos y, cuando corresponde, evaluación segmentada por dominios o subgrupos. El texto exige tanto técnica como sensibilidad interpretativa.

## Parte IX. Extensiones metodológicas y criterio profesional

Toda la materia converge, finalmente, en una pregunta más amplia que la de "qué modelo rinde mejor". ¿Cómo se usa de manera responsable un sistema construido con datos? ¿Cómo se monitorea cuando el mundo cambia? ¿Cómo se comunica lo que sí y lo que no puede afirmarse? Esta última parte reúne extensiones metodológicas y criterios de práctica que distinguen a un usuario de herramientas de un profesional capaz de sostener decisiones técnicas.

## Capítulo 17. Series temporales: cuando el tiempo no puede ignorarse

### 17.1 Qué cambia cuando hay dependencia temporal

En una serie temporal las observaciones están ordenadas y suelen depender unas de otras. Eso rompe la comodidad del supuesto i.i.d. que subyace a muchos métodos estándar. Si mezclamos temporalmente los datos, podemos filtrar futuro hacia el pasado y construir una validación artificialmente optimista.

Cuando el tiempo importa, cambia la manera de definir features, de partir los datos y de interpretar los errores. No se trata solo de agregar una columna de fecha. Se trata de reconocer que la estructura temporal modifica la lógica entera del problema.

### 17.2 Tendencia, estacionalidad, ciclo y ruido

Una serie suele pensarse como combinación de tendencia, estacionalidad, ciclo y ruido. La **tendencia** describe un movimiento de largo plazo. La **estacionalidad** introduce patrones periódicos relativamente regulares. Los **ciclos** capturan oscilaciones más largas y menos rígidas. El **ruido** recoge variación no explicada.

Esta descomposición no es meramente descriptiva. Ayuda a decidir qué features temporales construir y qué baselines tienen sentido. En series temporales, los modelos ingenuos son especialmente importantes: repetir el último valor, copiar el valor del período análogo anterior o usar un promedio histórico puede ser una referencia muy exigente. Si un modelo sofisticado no supera con claridad a un baseline temporal razonable, la complejidad extra probablemente no esté justificada.

### 17.3 Lags, rolling windows y features temporales

Las features temporales típicas son los **lags**, las medias móviles, los acumulados, las diferencias y los indicadores de calendario. Todas intentan condensar dependencia temporal en una representación utilizable por el modelo.

La regla crítica es causal: una feature construida en \(t\) solo puede depender de información disponible hasta \(t\). Parece obvio, pero muchas fugas temporales nacen precisamente de violar esta condición al construir rolling windows o agregados retrospectivos de forma incorrecta.

### 17.4 Evaluación temporal

La validación temporal debe imitar el uso real del sistema. Los esquemas walk-forward entrenan en una ventana histórica y validan en un bloque futuro, repitiendo el proceso con ventanas crecientes o deslizantes. Esto permite medir no solo precisión promedio, sino también estabilidad a lo largo del tiempo.

En problemas temporales esa estabilidad es central. Un modelo puede rendir muy bien en un tramo y deteriorarse después por cambios de contexto, estacionalidad o comportamiento. Evaluar bien es, también aquí, diseñar un experimento que respete la estructura del mundo.

## Capítulo 18. Interpretabilidad, MLOps, sesgo y gobernanza

### 18.1 Interpretabilidad intrínseca y explicabilidad post hoc

No todos los modelos son igual de transparentes. En regresión lineal o árboles pequeños, la estructura del modelo ya ofrece cierto grado de interpretabilidad intrínseca. En ensambles complejos y redes profundas esa transparencia disminuye y aparecen técnicas **post hoc** que intentan resumir el comportamiento del modelo.

La distinción importa mucho. Una explicación post hoc no convierte al modelo en simple; apenas construye una aproximación o una narrativa útil sobre su comportamiento. Confundir explicación con comprensión total es una fuente frecuente de exceso de confianza.

### 18.2 Importancia global y explicación local

La **importancia global** intenta responder qué variables influyen más en el comportamiento general del modelo. Las explicaciones **locales** intentan responder por qué un caso particular recibió cierta predicción. Ambas son valiosas, pero ninguna debe leerse de forma ingenua.

Herramientas como permutation importance, PDP, LIME o SHAP pueden ofrecer información muy útil si se entienden sus supuestos. Un PDP puede resultar engañoso cuando hay fuerte correlación entre variables. SHAP distribuye contribuciones bajo una lógica bien definida, pero no transforma asociación en causalidad. La regla es clara: explicar un modelo no es demostrar qué pasaría si interviniéramos sobre el mundo.

### 18.3 Comunicar resultados con honestidad técnica

Un resultado técnicamente correcto y mal comunicado puede inducir decisiones peores que un modelo más simple y mejor explicado. Comunicar bien no es simplificar hasta vaciar, sino dejar claro sobre qué datos se evaluó el modelo, qué métricas se usaron, qué límites persisten, qué segmentos funcionan peor y qué tipo de decisión respalda realmente la salida.

Decir "el modelo obtuvo 0.89 de AUC" rara vez alcanza. Hace falta explicar si esa cifra es estable, si la clase está desbalanceada, si las probabilidades están calibradas, si el resultado cambia mucho por subgrupo y cuál es el costo operativo del error. La comunicación es parte del trabajo técnico, no su envoltorio.

### 18.4 Reproducibilidad y ciclo de vida del modelo

Una solución de ciencia de datos no termina en un notebook. Para ser profesional debe poder reproducirse, auditarse y mantenerse. Eso exige versionar datos y código, registrar hiperparámetros, preservar artefactos del modelo y documentar cómo se obtuvo cada resultado.

La reproducibilidad no es solo una virtud académica. Sin ella es casi imposible comparar experimentos, diagnosticar fallas, explicar cambios de performance o repetir un entrenamiento con confianza. En producción, además, aparece el problema del ciclo de vida: cómo desplegar, monitorear, reentrenar y retirar modelos de manera controlada.

### 18.5 Drift: cuando el mundo cambia

Después del despliegue, el problema sigue moviéndose. Puede cambiar la distribución de las features (**data drift**) o puede cambiar la relación entre features y target (**concept drift**). En ambos casos, un modelo entrenado sobre el pasado puede empezar a degradarse.

Por eso el monitoreo no es un lujo. Hay que vigilar distribución de entradas, desempeño, calibración y comportamiento por segmentos críticos. Un modelo útil no es solo uno que fue bueno el día que se evaluó, sino uno cuyo deterioro puede detectarse a tiempo.

### 18.6 Fuentes de sesgo algorítmico

El sesgo puede entrar en muchas etapas: muestreo, etiquetado, elección del target, variables proxy, historia institucional o uso del modelo fuera del dominio donde fue entrenado. Un modelo con muy buena métrica global puede perjudicar sistemáticamente a subgrupos específicos y ocultarlo detrás de un promedio tranquilizador.

Por eso la evaluación responsable exige mirar cortes segmentados, estabilidad por población y consecuencias reales del error. Nociones como paridad demográfica o igualdad de oportunidad ayudan a organizar la discusión, pero ninguna define por sí sola la solución correcta. La equidad no es un número único: es una tensión entre criterios técnicos, normativos y operativos.

### 18.7 Gobernanza mínima responsable

La gobernanza de modelos incluye documentación, trazabilidad, criterios de aprobación, monitoreo continuo, auditoría y, cuando corresponde, revisión humana. No es un apéndice moral separado de la técnica. Es la forma institucional de garantizar que el sistema se use dentro de sus condiciones de validez.

Cuanto más importante es la decisión asistida por el modelo, más necesaria se vuelve esta capa de control. La buena práctica profesional no termina en el mejor score disponible.

## Capítulo 19. Ejemplo integrador y estrategia de estudio para dominio experto

### 19.1 Un ejemplo completo: predicción de morosidad

Consideremos un problema integral de riesgo: predecir si una solicitud aprobada incurrirá en mora superior a 90 días dentro de los próximos 12 meses. Una formulación rigurosa podría ser: "Para cada solicitud aprobada al momento de originación, estimar la probabilidad de mora mayor a 90 días en los siguientes 12 meses, usando exclusivamente información disponible al momento del otorgamiento". Con esa sola frase ya quedan fijados unidad de análisis, target, horizonte temporal y restricción de información.

El trabajo posterior organiza toda la materia. Primero, revisar calidad de datos: ingresos faltantes, consistencia laboral, historial crediticio, posible fuga por variables construidas después del otorgamiento. Luego, diseñar la representación: imputación segmentada, indicadores de faltante, codificación de categóricas, creación de razones financieras y tratamiento de extremos. Después, validar sin autoengaño: partición temporal o por cohortes si el problema lo requiere, métricas sensibles al desbalance y análisis de calibración. Recién entonces tiene sentido comparar una logística regularizada, un Random Forest y un XGBoost, no solo por score, sino por estabilidad, interpretabilidad y costo operativo de sus errores.

Este ejemplo sintetiza la arquitectura completa del curso. La pregunta central nunca es solo "qué modelo gana", sino "qué sistema de decisión queda mejor justificado por los datos, la validación y el contexto".

### 19.2 Qué debería poder hacer un estudiante al terminar

Al final de la materia, un estudiante sólido debería poder traducir una necesidad real a un problema modelable, definir con precisión unidad de análisis y target, detectar leakage antes de entrenar, justificar una estrategia de preprocesamiento según las variables y el modelo, elegir métricas coherentes con el costo del error y explicar la lógica profunda de las familias de métodos estudiadas.

Pero ese dominio técnico no basta por sí solo. También debería poder distinguir predicción de causalidad, interpretar resultados sin sobreafirmar y comparar técnicas atendiendo a sus supuestos, sus ventajas y sus límites. En otras palabras, debería poder argumentar.

### 19.3 Cómo suelen evaluar los exámenes

Los exámenes de esta materia suelen castigar menos la falta de memoria puntual que la falta de estructura argumentativa. No alcanza con definir precision, entropía o PCA. Hay que explicar qué problema resuelven, por qué se introducen, cuándo son útiles y qué errores corrigen o pueden inducir.

Las preguntas importantes casi siempre apuntan a esas relaciones: por qué este problema es clasificación y no regresión; por qué accuracy engaña; por qué un árbol profundo sobreajusta; qué preserva PCA y por qué no es lo mismo que t-SNE; cómo interpretar un coeficiente logístico; qué cambia cuando hay tiempo. La respuesta excelente define, motiva, formaliza e ilustra.

### 19.4 Errores que más suelen costar

Los errores más caros, académica y profesionalmente, son bastante constantes. Elegir algoritmo antes de formular el problema. Usar métricas por costumbre. Preprocesar antes del split. Confundir correlación con causalidad. Interpretar clusters como verdades naturales. Creer que más complejidad implica automáticamente mejor generalización. Reportar una única métrica sin analizar estabilidad ni subgrupos.

Todos esos errores tienen una raíz común: perder de vista la arquitectura completa del razonamiento y quedarse con una parte aislada del pipeline. La materia, bien estudiada, existe justamente para evitar esa fragmentación.

### 19.5 Cómo estudiar esta materia de verdad

Estudiar bien esta disciplina requiere trabajar en tres capas simultáneas. La primera es la intuición: poder explicar cada concepto con palabras claras y sin esconderse detrás de la notación. La segunda es la formalización: poder escribir las expresiones esenciales, interpretar cada símbolo y entender qué problema resuelven. La tercera es la aplicación: saber cuándo una técnica tiene sentido, qué supuestos la sostienen, cómo se evalúa y qué límites tiene.

Una práctica muy efectiva consiste en tomar cada tema y responder, por escrito, siempre las mismas preguntas: qué problema obliga a introducirlo, cuál es su intuición, cómo se formula, cómo se interpreta, qué ejemplo lo aclara, en qué falla una comprensión ingenua y con qué otros temas se conecta. Cuando eso se vuelve posible de manera sostenida, deja de haber "resumen" y empieza a haber dominio.

## Cierre

La ciencia de datos se vuelve realmente comprensible cuando deja de verse como una colección de técnicas y empieza a verse como una arquitectura intelectual. Primero se formula bien el problema. Luego se examina críticamente el dato. Después se construye una representación adecuada, se elige un modelo compatible con esa representación, se evalúa sin autoengaño, se interpreta con honestidad y se decide con responsabilidad.

Cada bloque de la materia ocupa un lugar preciso en esa arquitectura. Los fundamentos definen el lenguaje del problema. El análisis de calidad y el EDA enseñan a leer evidencia imperfecta. El preprocesamiento y la ingeniería de variables construyen el espacio en el que el aprendizaje será posible o fallará. La validación protege contra conclusiones ilusorias. Los modelos supervisados muestran distintas formas de sesgo inductivo. El aprendizaje no supervisado y la reducción de dimensión obligan a pensar qué estructura queremos preservar. Las redes y el NLP muestran que representar bien puede ser más importante que elegir un algoritmo de moda. La interpretabilidad, el MLOps y la gobernanza recuerdan que un modelo útil no termina en una métrica.

Si al terminar este tratado el lector puede enfrentarse a un problema nuevo y preguntarse, con precisión, qué quiere predecir o comprender, con qué datos, bajo qué supuestos, con qué riesgo de error, con qué validación y con qué límites, entonces la materia habrá cumplido su objetivo. No habrá aprendido solo herramientas. Habrá aprendido a pensar con datos.
