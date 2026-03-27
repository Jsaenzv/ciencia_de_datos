# Tratado de Ciencia de Datos: fundamentos para entender desde la base

## Prólogo

La ciencia de datos se aprende mal cuando se la presenta como un catálogo de algoritmos. En ese formato el estudiante acumula nombres, recuerda qué botón apretar en una librería y quizá hasta obtiene métricas decorosas, pero no alcanza a ver qué problema resuelve cada idea ni por qué una decisión metodológica tiene sentido y otra no. Entonces puede repetir procedimientos, pero no reconstruir razonamientos.

Conviene mirar la disciplina como una cadena de traducciones. En el mundo ocurre un fenómeno; ese fenómeno deja rastros; esos rastros se organizan en datos; sobre esos datos construimos una representación; sobre esa representación entrenamos un modelo; y con la salida del modelo alguien toma una decisión. La continuidad entre esos eslabones importa más que cualquier algoritmo aislado. Cuando esa continuidad se rompe, el proyecto puede seguir luciendo técnico y sofisticado, pero ya empezó a fallar en lo esencial.

Este tratado está escrito con una intención deliberada: no ofrecer una colección de apuntes, sino una explicación continua que permita entender desde la raíz por qué existe cada concepto. La matemática aparecerá cuando haga falta, no como adorno ni como ritual, sino como una forma más precisa de decir algo que primero debe resultar comprensible en lenguaje humano. Un modelo de riesgo, una matriz de diseño, una curva ROC o una componente principal no son objetos para memorizar; son respuestas a problemas muy concretos que aparecen cuando intentamos razonar bien con datos imperfectos.

La meta, por eso, no es solo aprobar una materia ni sumar herramientas al currículum. La meta es algo más sobrio y más valioso: que, frente a un problema nuevo, el lector pueda volver al punto de partida, preguntarse qué quiere entender o anticipar, qué rastro observable tiene de ese fenómeno, qué representación respeta mejor el sentido del problema, cómo debería evaluarse el error y qué puede afirmarse con honestidad después de modelar. Si eso ocurre, entonces la ciencia de datos deja de ser una secuencia de recetas y empieza a convertirse en una forma rigurosa de pensar.

## Parte I. Fundamentos: qué problema resuelve realmente la ciencia de datos

Antes de hablar de modelos conviene fijar el terreno. La ciencia de datos no trabaja sobre la realidad en estado puro, sino sobre observaciones parciales, ruidosas y muchas veces sesgadas de esa realidad. Por eso su pregunta más profunda no es qué algoritmo usar, sino cómo traducir con cuidado un fenómeno del mundo a un problema que pueda representarse, aprenderse y evaluarse sin engañarnos sobre lo que realmente sabemos.

## Capítulo 1. La ciencia de datos como disciplina de representación y decisión

### 1.1 Qué es y qué no es la ciencia de datos

La necesidad de la ciencia de datos aparece cada vez que alguien debe decidir en condiciones de incertidumbre. Una empresa quisiera anticipar qué clientes están por irse. Un hospital quisiera detectar pacientes con mayor riesgo. Un banco quisiera estimar la probabilidad de mora antes de otorgar un crédito. En todos esos casos el fenómeno que importa existe en el mundo, pero no se presenta ante nosotros de manera completa ni transparente. Lo que tenemos son huellas: consumos, reclamos, resultados clínicos, transacciones, textos, mediciones, registros administrativos.

La disciplina nace precisamente en esa distancia entre el fenómeno y su rastro observable. Su tarea no consiste en extraer una verdad pura escondida dentro de una tabla, sino en construir representaciones suficientemente buenas como para describir, segmentar o predecir con menos error del que tendríamos sin ellas. Vista así, la ciencia de datos es una práctica de modelado bajo información incompleta. Trabaja con evidencia indirecta, con señales mezcladas con ruido y con decisiones que nunca desaparecen detrás de la técnica, porque al final siempre habrá alguien que deberá actuar en función de la salida del sistema.

Esa mirada también aclara qué no es. No es sinónimo de inteligencia artificial, aunque a veces se cruce con ella. No es simplemente estadística con computadoras, aunque la estadística sea uno de sus lenguajes más importantes. No es solo programar pipelines, aunque sin implementación rigurosa las ideas no se sostienen. Y, sobre todo, no es una máquina automática que convierte datos en certeza. Su objeto real es más modesto y más serio: razonar con rastros imperfectos del mundo para equivocarse menos al decidir.

### 1.2 Fenómeno, dato, variable y decisión

Uno de los primeros aprendizajes importantes consiste en no mezclar planos. El fenómeno es aquello que queremos entender o anticipar: abandono, fraude, demanda, mora, riesgo, satisfacción, tiempo de resolución, intención de compra. El dato es el registro concreto que tenemos a mano: un consumo, una llamada, un monto, una fecha, una respuesta de encuesta, una frase escrita por un usuario. La variable es una propiedad observada o construida sobre cada unidad. La decisión es la acción que alguien tomará a partir de esa información: aprobar, intervenir, priorizar, ofrecer, alertar, recomendar.

Una analogía sencilla ayuda a fijar la diferencia. Si el fenómeno es la fiebre de un paciente, el termómetro no es la fiebre: es un instrumento que la registra con cierta precisión, con cierto error y dentro de ciertas condiciones. Del mismo modo, un dataset no es el fenómeno. Es el conjunto de marcas que distintos instrumentos, procesos y reglas institucionales dejaron sobre ese fenómeno. Cuando se olvida esto, se empieza a tratar a la tabla como si fuera la realidad misma, y con ese gesto se pierden preguntas decisivas sobre medición, sesgo y significado.

Pensemos en un problema de churn. El abandono real del cliente ocurre en el mundo. Lo que la organización observa son trazas como frecuencia de uso, reclamos, demoras de pago o historial de interacción. A partir de esas trazas se construyen variables, por ejemplo los días desde el último uso o la caída de actividad en las últimas cuatro semanas. Con la salida del modelo no se obtiene todavía una verdad metafísica sobre la voluntad del cliente, sino una ayuda para decidir si conviene activar una campaña de retención. Si esos niveles se confunden, el problema entero queda mal planteado antes de entrenar un solo modelo.

### 1.3 Dataset, población, muestra y unidad de análisis

Una base de datos suele verse como una tabla, pero conceptualmente es algo más delicado. Es una muestra finita tomada de una población o, más ampliamente, de un proceso generador de datos que imaginamos mayor que lo efectivamente observado. En forma tabular solemos escribir

\[
X \in \mathbb{R}^{n \times p}
\]

donde \(n\) es el número de observaciones y \(p\) el número de variables predictoras. Si estamos en un problema supervisado, además aparece una respuesta

\[
y \in \mathbb{R}^n
\]

o bien un vector de etiquetas discretas.

La notación es útil, pero enseguida tapa la pregunta que más importa: qué representa una fila. Esa pregunta define la unidad de análisis. Una fila puede ser un cliente, una transacción, una imagen, un documento, una consulta médica, un dispositivo, un día o una ventana temporal. Y ese detalle, que a veces parece meramente administrativo, en realidad redefine el problema completo. Si cada fila corresponde a un cliente, el modelo compara clientes entre sí. Si cada fila corresponde a una semana por cliente, el modelo compara estados temporales del mismo cliente y de otros. El target cambia, la validación cambia y también cambia el sentido del error.

Por eso conviene desconfiar del supuesto de independencia cuando aparece una tabla prolija con filas y columnas. Dos registros del mismo paciente, del mismo comercio o del mismo usuario no se vuelven observaciones intercambiables solo porque estén en renglones distintos. El famoso supuesto i.i.d. no viene garantizado por el formato del archivo. Hay que ganárselo entendiendo el proceso que produjo esos datos.

### 1.4 Features, target y variable respuesta

En aprendizaje supervisado distinguimos entre las variables de entrada y la variable de salida. Las primeras reciben distintos nombres, pero la palabra feature se volvió habitual. La segunda suele llamarse target, etiqueta o variable respuesta. Detrás de esa separación aparentemente simple hay una decisión de enorme importancia: qué parte de lo observado usaremos como información disponible y qué parte consideraremos el resultado que queremos anticipar.

Elegir features no es juntar todas las columnas disponibles. Es formular una hipótesis sobre qué aspectos observables del fenómeno pueden aportar señal. Algunas variables no dicen casi nada. Otras duplican información ya presente. Otras cambian de significado con el tiempo. Otras contienen datos que, al momento de decidir, todavía no existirían y por eso introducen fuga de información. En ciencia de datos rara vez la dificultad principal es la falta de columnas; mucho más frecuente es la falta de criterio para distinguir cuáles son legítimas y cuáles no.

Con el target ocurre algo todavía más delicado. Muchas formulaciones empiezan con expresiones vagas, como "cliente riesgoso" o "paciente crítico". Eso puede servir para una conversación inicial, pero no alcanza para entrenar. Un target modelable exige una regla operativa. En churn, por ejemplo, no alcanza con decir que queremos "clientes que se van". Hay que especificar algo como: cliente que cancela el servicio dentro de los próximos sesenta días y no se reactiva en ese período. Recién en ese momento el problema deja de ser una intuición razonable y pasa a convertirse en un objeto técnico que puede discutirse, medirse y auditarse.

### 1.5 Tipos de problemas: clasificación, regresión, clustering y otras tareas

La primera gran bifurcación práctica surge de la forma que debe tener la respuesta. Si lo que se espera es una categoría, estamos frente a un problema de clasificación. Si lo que importa es estimar una magnitud numérica, hablamos de regresión. Si no existen etiquetas y el interés está en descubrir estructura, agrupar casos o resumir información, entramos en el territorio del aprendizaje no supervisado, donde clustering es una de las tareas más conocidas.

Sin embargo, esa clasificación solo cobra sentido cuando se la conecta con la decisión que vendrá después. El riesgo crediticio puede formularse como una clasificación binaria, como la estimación continua de una pérdida esperada o como un ranking de solicitantes ordenados por riesgo. Ninguna de esas formulaciones es superior en abstracto. La mejor será la que traduzca con más fidelidad la manera en que la organización debe actuar. Si solo pueden revisarse manualmente cien casos por semana, tal vez un ranking sea más natural que una clase dura. Si el costo importa en unidades monetarias, quizá una regresión tenga más sentido que un sí o no.

Lo importante, entonces, no es memorizar etiquetas escolares, sino entender que cada familia de problemas arrastra una noción distinta de error, una salida distinta del modelo y una forma distinta de convertir esa salida en decisión.

### 1.6 Aprendizaje supervisado y no supervisado

En aprendizaje supervisado observamos pares \((x_i, y_i)\) y buscamos una regla que conecte entradas con salidas. Lo que intenta aprenderse no es la lista de respuestas observadas, sino una relación más general que permita anticipar la salida de casos nuevos. Si pudiéramos ver la distribución real \(P\) que genera los datos, la tarea ideal sería elegir una función \(f\) que minimizara el riesgo esperado

\[
R(f) = \mathbb{E}_{(X,Y)\sim P}[L(Y, f(X))]
\]

donde \(L\) es una función de pérdida. Como esa distribución es desconocida, trabajamos con lo único que tenemos: la muestra observada. Entonces minimizamos el riesgo empírico

\[
\hat R_n(f) = \frac{1}{n}\sum_{i=1}^{n} L(y_i, f(x_i)).
\]

Dicho en lenguaje llano, el modelo aprende a equivocarse poco en los datos disponibles con la esperanza de que esa habilidad se extienda a datos futuros. Toda la teoría de generalización, con sus precauciones y sus límites, nace de la distancia entre esas dos cosas.

En aprendizaje no supervisado, en cambio, observamos solo \(x_i\). Ya no existe una respuesta correcta externa que nos diga qué tan bien estamos. El problema pasa a ser encontrar estructura, compresión o representación útil sin una etiqueta que funcione como árbitro. Eso vuelve al criterio más importante. Cuando no hay un target que discipline el análisis, el peso de las decisiones conceptuales sobre la representación y el objetivo aumenta mucho.

### 1.7 Predicción versus causalidad

Pocas confusiones cuestan tanto como creer que un buen predictor ya es, por eso mismo, una explicación causal. La predicción pregunta qué puede anticiparse dado lo que se observa. La causalidad pregunta qué ocurriría si interviniéramos sobre una variable y alteráramos el sistema. En el primer caso trabajamos con regularidades observadas; en el segundo, con mundos contrafácticos.

En lenguaje probabilístico, la predicción suele moverse alrededor de expresiones como \(P(Y \mid X)\). La causalidad exige pensar en cantidades del tipo

\[
P(Y \mid do(X=x)),
\]

donde \(X\) no solo se observa, sino que se fija por intervención. El contraste parece pequeño en la notación, pero conceptualmente es enorme. Una variable puede ser excelente para anticipar un resultado sin que manipularla cambie ese resultado.

En un problema de abandono, por ejemplo, la cantidad de reclamos al call center puede predecir muy bien qué clientes terminarán yéndose. Eso no autoriza a concluir que hacerlos reclamar más provocaría abandono. Es mucho más plausible que los clientes que ya están insatisfechos tiendan a reclamar más. Un modelo predictivo puede ser utilísimo para asignar recursos de retención y, al mismo tiempo, no decir nada directo sobre qué intervención es causalmente eficaz.

### 1.8 Función de pérdida, riesgo y criterio de decisión

Todo modelo optimiza algo, aunque el usuario no siempre sea consciente de ello. Ese algo suele ser una función de pérdida, una manera de cuantificar el costo de equivocarse. En clasificación binaria, la pérdida 0-1 trata todos los errores por igual. En regresión, la pérdida cuadrática castiga especialmente los errores grandes. En clasificación probabilística, la log-loss penaliza con dureza las predicciones muy confiadas cuando resultan erróneas.

Pero conviene distinguir dos niveles. Una cosa es la pérdida con la que se entrena el modelo. Otra, la regla con la que después se actúa. Si el modelo produce una probabilidad \(\hat p(x)\), alguien deberá decidir a partir de qué valor se considera un caso positivo. Ese umbral no debería salir de la costumbre, sino del costo relativo de los errores. Si llamar positivo a un caso negativo cuesta \(C_{FP}\) y dejar pasar un verdadero positivo cuesta \(C_{FN}\), una regla elemental de costo esperado sugiere clasificar como positivo cuando

\[
(1-p)C_{FP} < pC_{FN},
\]

lo que equivale a

\[
p > \frac{C_{FP}}{C_{FP} + C_{FN}}.
\]

La fórmula es sencilla, pero la enseñanza es profunda. El umbral correcto depende de cuánto duele cada tipo de error. Si perder un caso importante es muy costoso, el sistema deberá ser más sensible y aceptar más falsas alarmas. Si una intervención innecesaria es muy cara, el umbral debería subir. Lo que aquí se juega no es una preferencia matemática, sino la traducción entre el lenguaje del modelo y el lenguaje de la decisión.

## Capítulo 2. Cómo formular correctamente un problema de datos

### 2.1 Del problema del mundo real al problema modelable

En la práctica, casi nadie necesita "aplicar un algoritmo". Lo que las organizaciones necesitan es reducir mora, anticipar fraude, priorizar casos, mejorar retención, ordenar atención, estimar demanda o comprender segmentos. El primer trabajo serio de la ciencia de datos consiste en traducir esas necesidades, que suelen venir expresadas en lenguaje cotidiano o institucional, a una pregunta que el sistema pueda aprender y evaluar.

Esa traducción exige fijar varias piezas a la vez. Hay que decidir qué representa una observación, qué evento o magnitud se quiere modelar, con qué horizonte temporal, con qué información disponible al momento de decidir y con qué criterio se medirá el error. Cuando alguna de esas piezas queda implícita, el proyecto puede avanzar durante semanas y seguir respondiendo una pregunta incorrecta. El problema no aparecerá como un bug visible; aparecerá como una desconexión silenciosa entre la métrica del modelo y la decisión real.

### 2.2 Unidad de análisis, granularidad y nivel de agregación

La granularidad es el nivel de detalle con el que el fenómeno queda representado. Y no existe un nivel universalmente correcto. Una fila por transacción conserva mucha información local, pero introduce dependencia temporal y puede volver el problema muy grande y ruidoso. Una fila mensual por cliente simplifica el análisis, aunque a costa de borrar patrones finos. La pregunta siempre es la misma: qué resolución necesita realmente la decisión que vendrá después.

En detección de fraude, por ejemplo, una transacción individual puede ser la unidad natural porque el comportamiento sospechoso se juega en la secuencia inmediata y en el contexto cercano. Si agregáramos todo a nivel mensual por cliente, se perdería justamente la información que vuelve identificable el fenómeno. En cambio, para estimar el riesgo general de abandono quizá una foto mensual sea suficiente y hasta más estable. La granularidad correcta no es la más detallada, sino la que conserva la estructura relevante sin deformar el problema.

### 2.3 Horizonte temporal y disponibilidad de información

En problemas predictivos no alcanza con decir qué queremos anticipar. También hay que dejar claro cuándo queremos anticiparlo. Ese instante organiza todo lo demás. Predecir mora a doce meses en el momento de originación del crédito no es el mismo problema que predecir atraso a treinta días usando comportamiento reciente. Cambian las variables legítimas, cambia la dificultad del target y cambia el tipo de decisión que la salida puede sostener.

La regla de oro es simple y severa: cada feature debe representar información que estaría realmente disponible en el momento de decidir. Si una variable se conoce después del evento o se construye usando ventanas que miran hacia adelante, el modelo aprende con pistas del futuro. Eso produce lo que se llama data leakage. El resultado suele ser tentador porque las métricas suben, pero es un éxito ficticio. Un modelo que necesita el futuro para predecir el futuro no entendió nada; solo hizo trampa sin saberlo.

### 2.4 Ejemplo completo de formulación: predicción de abandono

Vale la pena ver cómo cambia el problema cuando la formulación se vuelve precisa. La frase "queremos saber qué clientes se van a ir" todavía es demasiado vaga. En cambio, si escribimos que para cada cliente activo al cierre de cada mes queremos estimar la probabilidad de cancelar el servicio dentro de los siguientes sesenta días utilizando exclusivamente información disponible hasta ese cierre, ya cambió la calidad intelectual del proyecto. Ahora sabemos qué representa una observación, cuál es el target, qué horizonte temporal se está usando y qué información es legítima.

Además, esa redacción deja claro algo crucial: el modelo produce una probabilidad, no una acción automática. La acción vendrá después, cuando se compare el costo de intervenir con el costo de perder al cliente. Redactar así el problema no es una formalidad burocrática. Es la forma de obligar a todos los involucrados a hablar del mismo objeto.

### 2.5 Clasificación, regresión o clustering: cómo decidir

Muchas veces la salida deseada parece dictar de inmediato el tipo de problema, pero las decisiones maduras nacen cuando varias formulaciones son posibles. Si lo importante es responder sí o no, clasificación será natural. Si interesa una magnitud, la regresión suele ser el punto de partida. Si no existe target y lo que se busca es segmentar o resumir estructura, clustering puede tener sentido.

Sin embargo, la formulación correcta no se decide mirando solo la naturaleza del dato, sino la forma que deberá tomar la acción. En algunos contextos basta con ordenar casos por prioridad. En otros se necesita una probabilidad bien calibrada. En otros el negocio está definido en dinero y conviene modelar directamente pérdida esperada. La pregunta fértil no es "qué algoritmo quiero usar", sino "qué tipo de salida permite decidir mejor".

### 2.6 Costo del error y diseño de la métrica

Cada problema arrastra su propia economía del error. Un falso negativo puede ser gravísimo en screening médico, donde dejar pasar un caso relevante tiene consecuencias mucho más costosas que generar una falsa alarma. En un filtro de spam puede ocurrir lo contrario: quizás sea más grave ocultar un correo importante que tolerar varios mensajes basura. Cuando se entiende esto, la elección de la métrica deja de ser un trámite posterior y se vuelve parte de la formulación del problema.

Accuracy, F1, AUC, MAE o RMSE no son trofeos universales. Son maneras distintas de decir, en lenguaje numérico, qué clase de error pesa más. Por eso una métrica solo es buena si representa, al menos de forma aproximada, el costo real del problema. Pensarla después del entrenamiento es llegar tarde; debería formar parte de la definición inicial del objetivo.

### 2.7 Cómo se nota que un problema está mal formulado

Los síntomas de una mala formulación se parecen bastante entre sí. El target cambia de significado a medida que avanza el trabajo. Nadie puede explicar con precisión qué información era legal al momento de predecir. La métrica elegida no tiene relación con la decisión. Se combinan períodos o poblaciones que no deberían mezclarse. O, más sencillamente, se habla de una necesidad de negocio y se termina modelando otra cosa.

Cuando eso ocurre, el proyecto no fracasa necesariamente de manera visible. Puede entregar gráficos prolijos y métricas vistosas. Lo peligroso es justamente esa apariencia de normalidad. Gran parte del oficio profesional consiste en detectar a tiempo cuándo se está respondiendo con precisión una pregunta que nunca debió haberse formulado así.

## Parte II. Leer el dato antes de modelarlo

Una vez formulado el problema, aparece una obligación que suele subestimarse: aprender a leer el dataset como evidencia y no como materia prima neutra. Antes de construir modelos conviene entender qué representan realmente las columnas, qué sesgos trae la muestra, qué cosas faltan, qué significados cambiaron con el tiempo y qué transformaciones son legítimas. Modelar sin esa lectura previa es parecido a sacar conclusiones sobre un experimento sin haber entendido cómo se midió.

## Capítulo 3. Anatomía del dataset y calidad de datos

### 3.1 Tipos de variables y escalas de medición

No alcanza con separar variables numéricas y categóricas. Esa división es demasiado gruesa para muchas decisiones importantes. Una variable nominal distingue categorías sin orden intrínseco, como una marca o una ciudad. Una ordinal agrega un orden, como un nivel de severidad o un tramo educativo. Las variables de intervalo permiten comparar diferencias, aunque no cuentan con un cero absoluto natural; la temperatura en Celsius es el ejemplo clásico. Las variables de razón, en cambio, sí tienen un cero interpretable y admiten comparaciones multiplicativas, como ingresos, peso o distancia.

La razón por la que esto importa es sencilla: la escala de medición decide qué operaciones tienen sentido. Promediar códigos postales no dice nada. Tratar como continua una categoría nominal codificada con números impone una geometría artificial. Ignorar el orden de una variable ordinal es tirar información a la basura. Cuando alguien codifica nivel educativo como 1, 2, 3 y 4, debe recordar que esa secuencia puede servir para ordenar, pero no garantiza que la distancia entre 1 y 2 sea comparable con la distancia entre 3 y 4. No todos los números miden.

### 3.2 Calidad de datos: completitud, validez, consistencia y unicidad

La expresión "limpieza de datos" suele mezclar problemas de naturaleza muy distinta. Conviene desarmarla. La completitud pregunta qué parte de la información falta. La validez pregunta si los valores observados respetan rangos, tipos y formatos plausibles. La consistencia examina si distintas columnas o tablas se contradicen entre sí. La unicidad revisa si hay duplicados indebidos. Y a todo eso debería agregarse una dimensión que en la práctica vale oro: la trazabilidad, es decir, saber de dónde proviene cada dato y qué transformaciones atravesó.

Cada dimensión afecta algo diferente del modelado. Un faltante puede obligar a imputar. Una inconsistencia puede revelar una regla de negocio mal implementada. Un duplicado puede sobreponderar casos que deberían contarse una sola vez. Una columna cuyo significado cambió con el tiempo puede volver inútil una validación temporal. Por eso la calidad no es un trámite previo, sino una parte del sentido mismo del dato.

### 3.3 Duplicados, inconsistencias y errores semánticos

No todo duplicado es un error y no todo error es visible como duplicado. Dos filas idénticas pueden provenir de una duplicación indebida de ingesta, pero también pueden representar dos eventos legítimos que, por casualidad, comparten todos los valores observados. Antes de borrar, hay que entender qué representa realmente una fila.

Los problemas semánticos suelen ser todavía más peligrosos porque no siempre aparecen en los controles formales. Una misma columna puede mezclar monedas diferentes, unidades incompatibles o definiciones administrativas que cambiaron sin cambiar el nombre del campo. Algo tan simple como una variable llamada `ingreso` puede significar, según el sistema y el período, ingreso personal mensual, ingreso anual declarado o ingreso del hogar. Un resumen estadístico no detecta ese problema. Hace falta lectura crítica y contexto.

### 3.4 Mecanismos de datos faltantes

Una celda vacía no es simplemente ausencia. También es una pista sobre cómo ocurrió la medición. La clasificación clásica distingue entre MCAR, MAR y MNAR. En el primer caso, la falta es completamente aleatoria respecto del valor faltante y del resto de las variables. En MAR, la ausencia depende de información observada. En MNAR, la ausencia depende del propio valor faltante o de factores no observados.

La intuición detrás de esas siglas importa más que las siglas mismas. Si un ingreso falta porque un formulario se rompió al azar, el problema es uno. Si falta porque las personas de mayores ingresos prefieren no declararlo, el problema es muy distinto. En el segundo caso la ausencia ya está informando algo sobre el fenómeno. Pensemos en una encuesta de salud: si el peso falta más entre adultos mayores, la ausencia podría estar vinculada a la edad. Si falta sobre todo entre personas con obesidad severa porque evitan responder, la historia cambia por completo. Imputar de la misma manera en ambos escenarios es contar dos cuentos incompatibles como si fueran el mismo.

### 3.5 Outliers: error, rareza o parte esencial del fenómeno

Un valor extremo produce la tentación inmediata de borrarlo. A veces esa decisión es correcta. Otras veces es un error grave. Un outlier puede ser una medición defectuosa, un caso válido pero raro o, incluso, el tipo de caso que más importa detectar. La pregunta útil no es solo cuán lejos está del centro, sino qué historia plausible explica esa distancia.

En fraude, las observaciones raras pueden ser justamente el corazón del problema. En un sensor industrial, ciertos extremos pueden ser puro ruido instrumental. En una variable de ingresos, un valor muy alto puede corresponder a un cliente real de patrimonio excepcional y no a un error de carga. Las reglas estadísticas basadas en z-scores o rangos intercuartílicos sirven como alarma, no como sentencia final. La interpretación depende del fenómeno y del propósito del análisis.

### 3.6 Sesgo de muestreo, representatividad y desbalance de clases

Un dataset puede estar formalmente limpio y, sin embargo, representar mal el mundo sobre el que luego se lo usará. Ese es uno de los problemas más profundos de la disciplina. Si la muestra está sesgada, el modelo aprenderá una versión sesgada de la normalidad. Ningún algoritmo sofisticado corrige por sí solo una muestra que no refleja la población de interés.

La situación aparece con nitidez en riesgo crediticio, donde a veces se entrena con clientes históricamente aprobados y luego se pretende generalizar sobre solicitantes futuros que no necesariamente se parecen a ellos. También aparece en problemas de clases raras. Cuando la clase positiva es muy infrecuente, accuracy se vuelve engañosa y muchas intuiciones basadas en tasas globales se distorsionan. Pero la rareza matemática no es el verdadero problema. El problema de fondo es qué mundo normal aprende el modelo a partir de la muestra que le dimos.

### 3.7 Qué debe salir de un análisis de calidad

Al terminar un análisis de calidad serio, uno debería poder responder con precisión qué representa cada fila, qué significa cada columna, qué problemas de completitud, validez, consistencia o unicidad aparecieron, qué mecanismos plausibles explican los faltantes, qué variables son peligrosas por posible fuga de información y qué sesgos de muestreo limitan la generalización.

Si al final solo queda una lista dispersa de observaciones, entonces todavía no hubo diagnóstico. Lo que debería quedar es un mapa de restricciones y advertencias que condicione explícitamente el modelado posterior. Leer el dato bien es empezar a modelar bien.

## Capítulo 4. Análisis exploratorio de datos: leer antes de modelar

### 4.1 El sentido profundo del EDA

El análisis exploratorio de datos suele reducirse a "hacer gráficos", pero su función real es mucho más seria. Es el primer momento en que intentamos entender, con evidencia, cómo está organizado el problema. Un buen EDA reduce sorpresa, detecta inconsistencias conceptuales y cambia decisiones posteriores. Si después del análisis exploratorio nada se modifica en la formulación, en las transformaciones candidatas o en las sospechas metodológicas, probablemente el ejercicio fue decorativo.

Explorar no significa pasear por la base sin rumbo. Significa interrogarla. Cómo se distribuyen las variables. Qué asimetrías dominan. Qué categorías son centrales y cuáles marginales. Qué relaciones parecen lineales y cuáles no. Dónde hay subgrupos, saturaciones, colas largas, escalas incompatibles o señales demasiado buenas para ser verdad. El EDA no reemplaza el modelado, pero sí mejora drásticamente la calidad de las preguntas con las que después se modela.

### 4.2 Estadística descriptiva univariada

Mirar una variable por vez parece una tarea elemental, pero encierra varias intuiciones profundas. La media

\[
\bar x = \frac{1}{n}\sum_{i=1}^{n} x_i
\]

resume un centro aritmético, aunque no siempre coincide con lo que un observador llamaría valor típico. La mediana divide la muestra en dos mitades y suele resistir mejor la influencia de valores extremos. La varianza muestral

\[
s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar x)^2
\]

y el desvío estándar describen dispersión, mientras que el rango intercuartílico

\[
IQR = Q_3 - Q_1
\]

resume la amplitud de la zona central de la distribución.

Lo decisivo no es repetir estas definiciones, sino aprender a leer lo que insinúan. Cuando la media queda bastante por encima de la mediana suele haber una cola larga hacia la derecha. Cuando el IQR es pequeño y, al mismo tiempo, aparecen valores enormes, tal vez un puñado de casos raros esté dominando la escala. Cuando una variable casi no varía, quizá aporte poco a muchos modelos. La estadística descriptiva tiene sentido cuando ayuda a imaginar la forma del dato, no cuando se convierte en un rosario de números sin interpretación.

### 4.3 Frecuencias y análisis de variables categóricas

En variables categóricas el centro no importa del mismo modo; lo que importa es cómo se reparte la masa entre categorías. Conviene mirar frecuencias absolutas y relativas, cardinalidad, categorías raras y codificaciones inconsistentes. A menudo, el análisis de categóricas descubre antes que nadie problemas semánticos silenciosos. Una columna de provincia que contiene `Bs As`, `Buenos Aires` y `BUENOS_AIRES` no está revelando tres regiones distintas, sino una misma etiqueta escrita de tres maneras.

También aquí la intuición disciplinada importa más que la mecánica. Una categoría poco frecuente puede ser ruido o puede representar un segmento pequeño pero estratégico. Una alta cardinalidad puede volver incómodo el modelado, pero también puede contener información fina valiosa. Explorar una variable categórica es empezar a decidir qué estructura conviene preservar y cuál conviene simplificar.

### 4.4 Relaciones entre variables: correlación, dependencia y no linealidad

El siguiente paso es mirar variables en relación. La correlación de Pearson

\[
r_{XY} = \frac{\sum_{i=1}^{n}(x_i-\bar x)(y_i-\bar y)}
{\sqrt{\sum_{i=1}^{n}(x_i-\bar x)^2}\sqrt{\sum_{i=1}^{n}(y_i-\bar y)^2}}
\]

resume asociación lineal entre dos magnitudes. Es una herramienta valiosa, siempre que no se la confunda con una definición completa de dependencia. Una relación puede ser intensísima y, sin embargo, no quedar reflejada en la correlación lineal. Si \(Y = X^2\) y \(X\) se distribuye simétricamente alrededor de cero, la dependencia es total y la correlación puede ser cercana a cero.

Esa pequeña paradoja enseña algo importante. Correlación es una lente, no el fenómeno entero. Por eso un EDA serio no se conforma con coeficientes agregados. Busca curvaturas, saturaciones, interacciones, regímenes distintos por subgrupos, heterocedasticidad y cambios de comportamiento en distintos rangos. La pregunta no es solo si dos variables se mueven juntas, sino de qué manera lo hacen.

### 4.5 Visualización con propósito analítico

Una buena visualización no es la más vistosa, sino la que deja ver una estructura que importa. Un histograma ayuda a imaginar la forma de una distribución. Un boxplot resume dispersión y extremos. Un scatterplot permite ver curvatura, nubes separadas, densidades desiguales y relaciones que un coeficiente no captura. Un mapa de calor puede orientar una vista panorámica, aunque rara vez reemplaza el examen fino de las relaciones más importantes.

La visualización vale cuando está al servicio de una pregunta. Qué se muestra. Por qué podría importar. Qué decisión metodológica sugiere. Cuando esas tres cosas no están claras, el gráfico se convierte en decoración estadística. Y decorar no es explorar.

### 4.6 EDA respecto del target

Explorar respecto del target significa mirar cómo cambia la representación de los datos cuando el foco se posa sobre la variable que queremos predecir. En clasificación conviene comparar distribuciones entre clases, ver qué variables parecen separar mejor y dónde la diferencia es mínima. En regresión interesa entender si la relación parece aproximadamente lineal, si hay umbrales, escalones, saturaciones o varianzas que crecen con el nivel del target.

Este paso orienta de manera muy concreta el modelado. Si una covariable se relaciona con la respuesta de forma claramente no lineal, quizá convenga transformarla o usar una familia de modelos más flexible. Si la clase positiva es rarísima, el EDA ya debería advertir que accuracy contará una historia engañosa. Explorar con el target presente no anticipa el resultado final del modelo, pero sí ayuda a no llegar a ciegas a la siguiente etapa.

### 4.7 El EDA como detector de leakage

Algunas fugas de información se descubren recién en producción, pero muchas dejan huellas visibles antes de entrenar nada. Variables que separan casi perfectamente las clases, timestamps sospechosamente posteriores al evento, columnas generadas por el proceso de resolución del caso o identificadores administrativos con contenido implícito son señales de alarma.

Hay una regla empírica que conviene tomarse en serio: cuando una variable parece milagrosamente buena, antes de celebrar hay que sospechar. En ciencia de datos, los resultados demasiado perfectos suelen deberle más al leakage que a una comprensión excepcional del fenómeno.

### 4.8 Qué debería producir un buen EDA

Al final de un EDA bien hecho debería quedar una imagen más nítida del problema. Qué distribuciones dominan. Qué asimetrías o colas largas exigen atención. Qué variables están mal definidas, escaladas o codificadas. Qué relaciones con el target parecen prometedoras. Qué transformaciones podrían ayudar. Qué familias de modelos son plausibles. Qué riesgos metodológicos ya se ven venir.

El EDA todavía no produce un modelo, pero sí algo igual de valioso: produce mejores preguntas y reduce el autoengaño. Y en una disciplina que vive de distinguir estructura de ruido, esa ganancia intelectual no es accesoria.

## Parte III. Preprocesar no es maquillar: representar bien para poder aprender

Después de comprender el dato aparece la siguiente capa del problema: cómo construir una representación sobre la que un modelo pueda aprender sin traicionar el sentido del fenómeno. Preprocesar no es embellecer la tabla para que un algoritmo la tolere; es decidir qué estructura conviene preservar, qué ruido conviene amortiguar y qué supuestos estamos introduciendo al transformar.

## Capítulo 5. Preprocesamiento y construcción de representación

### 5.1 El principio rector: los modelos aprenden sobre representaciones

Ningún modelo aprende sobre la realidad directamente. Aprende sobre la forma particular en que la realidad fue codificada en una tabla, en una secuencia o en un vector. Si la representación es pobre, ilegítima o confusa, el modelo aprenderá exactamente eso. Esta idea, aunque simple, explica buena parte de la práctica real: muchas mejoras grandes no provienen de cambiar de algoritmo, sino de cambiar la manera en que el problema está escrito.

Puede pensarse al algoritmo como a un lector. El fenómeno es lo que querríamos contarle, pero las features son el idioma en que finalmente se lo escribimos. Si ese idioma conserva mal las relaciones relevantes, el lector no tiene cómo inferirlas. Por eso la construcción de representación no es una etapa auxiliar. Es una de las zonas donde más claramente se vuelve visible el entendimiento del problema.

### 5.2 Pipelines y separación entre entrenamiento, validación y prueba

Hay un error experimental muy frecuente: imputar, escalar, reducir dimensión o seleccionar variables usando toda la base, y recién después separar entrenamiento y evaluación. A primera vista parece una sutileza técnica; en realidad es una fuga de información. Si una transformación aprende algo de los datos, debe aprenderlo exclusivamente sobre entrenamiento y luego aplicarlo al resto.

Si estandarizamos una variable como

\[
z_{ij} = \frac{x_{ij} - \mu_j}{\sigma_j},
\]

los parámetros \(\mu_j\) y \(\sigma_j\) deben estimarse en el conjunto de entrenamiento. Recién después se usan para transformar validación o prueba:

\[
z_{ij}^{(test)} = \frac{x_{ij}^{(test)} - \mu_j^{(train)}}{\sigma_j^{(train)}}.
\]

Lo mismo vale para imputadores, PCA, target encoding, selectores de variables y cualquier transformación que aprenda estructura. Los pipelines no son una comodidad de software; son una forma de imponer honestidad experimental.

### 5.3 Imputación: decidir bajo incertidumbre

Imputar significa tomar una decisión sobre un valor que no observamos. Por eso no debería presentarse como un simple rellenado. Imputar con la media puede ser cómodo, pero reduce varianza y distorsiona relaciones. La mediana resiste mejor a los extremos. La moda sirve en variables categóricas. La imputación por grupos incorpora contexto. Métodos basados en vecinos o en modelos multivariados intentan reconstruir mejor la estructura conjunta.

La elección tiene sentido solo cuando se la conecta con la historia del faltante. Si la ausencia misma contiene señal, conviene marcarla con un indicador adicional. Imaginemos un formulario financiero donde el ingreso falta con más frecuencia entre trabajadores informales. Si reemplazamos esos faltantes por la media sin dejar rastro de la ausencia, el modelo verá ingresos "normales" allí donde en realidad había incertidumbre informativa asociada a un patrón social específico. A veces el indicador de faltante informa más que el valor imputado.

### 5.4 Tratamiento de outliers

Con los valores extremos no existe una receta universal porque tampoco existe una causa universal. Si hay evidencia de error de medición o de carga, corregir o excluir puede ser razonable. Si el valor es legítimo pero muy influyente, quizá convenga transformarlo o winsorizarlo. Si representa un régimen importante del fenómeno, preservarlo puede ser imprescindible.

Un árbol de decisión suele tolerar mejor algunos extremos que una regresión lineal. Un sistema de detección de anomalías, por definición, vive de lo raro. Por eso los umbrales estadísticos sirven como punto de partida, no como veredicto. El tratamiento correcto depende del fenómeno, del modelo y de la pregunta que motiva el análisis.

### 5.5 Encoding de variables categóricas

Muchos modelos necesitan entradas numéricas, de modo que las categorías deben codificarse. Pero codificar no es traducir letras a números de manera inocente. Es imponer una geometría. Con one-hot encoding cada categoría nominal pasa a ocupar su propia dimensión y no se inventa un orden inexistente. El ordinal encoding solo tiene sentido cuando el orden es real y relevante. El target encoding puede ser muy potente en alta cardinalidad, aunque exige una disciplina experimental rigurosa porque utiliza información de la variable respuesta.

El problema de fondo es siempre el mismo. Si asignamos 1, 2, 3 y 4 a categorías nominales y luego alimentamos esos números a un modelo sensible a distancias, acabamos de introducir sin querer una estructura de cercanía que nunca estuvo en el fenómeno. La tabla quedó numérica, sí, pero a costa de contarle al modelo una historia geométrica falsa.

### 5.6 Escalado, normalización y sensibilidad del modelo

La necesidad de escalar depende de la manera en que el modelo mira el espacio de datos. En métodos basados en distancias, como KNN, o en algoritmos optimizados por gradiente, como regresión regularizada, SVM o redes neuronales, la escala puede alterar fuertemente el aprendizaje. Una variable expresada en miles puede dominar a otra expresada en décimas, aunque sea menos relevante.

La estandarización centra y divide por el desvío estándar. La normalización min-max fuerza un rango acotado. El robust scaling usa mediana e IQR y resiste mejor a los extremos. En cambio, los árboles suelen ser mucho menos sensibles a la escala porque comparan cada variable contra umbrales propios. La enseñanza importante aquí es que no hay una receta universal del tipo "siempre hay que escalar". Lo que hay es una pregunta geométrica: cómo percibe el modelo la proximidad, la magnitud y la orientación en el espacio representado.

### 5.7 Discretización y transformaciones de distribución

Transformar una variable puede ser muy útil o muy destructivo. Discretizar una magnitud continua mejora a veces interpretabilidad y robustez, pero también puede borrar información fina. Una transformación logarítmica de la forma \(\log(x+c)\) comprime colas largas, reduce asimetría y a menudo vuelve más manejables relaciones que, en escala original, estaban dominadas por unos pocos valores enormes.

La clave está en que la transformación responda a un problema real. Si el target es extremadamente asimétrico y los errores grandes resultan desproporcionadamente influyentes, trabajar en escala logarítmica puede estabilizar el aprendizaje. Pero después la interpretación cambia. Los coeficientes, residuos y errores ya no viven en la escala original. Transformar por costumbre y olvidar ese cambio de significado es una forma muy elegante de equivocarse.

### 5.8 Feature engineering: donde el dominio se vuelve variable

La ingeniería de variables es el lugar donde el conocimiento del fenómeno se convierte en estructura utilizable por el modelo. Razones como ingreso sobre cuota, variables de recencia, recuentos en ventanas temporales, interacciones o indicadores de comportamiento condensan relaciones que el problema considera relevantes. A menudo esas construcciones dicen más que las columnas crudas de las que surgieron.

Una buena feature no es un truco brillante. Es una hipótesis explícita sobre qué aspecto del mundo importa. En mora, por ejemplo, el porcentaje del límite de crédito utilizado suele ser más informativo que saldo y límite por separado porque captura de inmediato una relación de tensión financiera. Quien entiende el fenómeno suele mejorar antes la representación que el algoritmo.

### 5.9 Selección de variables y multicolinealidad

No todas las variables contribuyen de la misma manera. Algunas son irrelevantes. Otras son casi duplicados disfrazados. En modelos lineales, la multicolinealidad vuelve inestable la estimación porque el sistema tiene dificultades para repartir efecto entre columnas muy parecidas. Formalmente, cuando algunas columnas de \(X\) se aproximan a combinaciones lineales de otras, la matriz \(X^TX\) se vuelve mal condicionada y la estimación de los coeficientes se vuelve sensible a pequeñas perturbaciones.

En esas condiciones, la predicción puede mantenerse razonable mientras la interpretación de los coeficientes se desordena por completo. Herramientas como VIF, regularización o selección explícita de variables ayudan, pero la lección conceptual es anterior a cualquier técnica: más columnas no significan necesariamente más información. A veces significan la misma señal repetida varias veces con disfraces distintos.

### 5.10 Qué preprocesamiento necesita cada familia de modelos

El preprocesamiento nunca existe en abstracto. Un KNN o una SVM suelen necesitar escalado cuidadoso porque viven de distancias y márgenes geométricos. La regresión lineal y la logística lo agradecen especialmente cuando aparece regularización. Los árboles toleran mejor escalas heterogéneas y relaciones no lineales sin demasiada intervención. PCA exige centrado y, con frecuencia, estandarización. El texto requiere otra idea de representación por completo.

La respuesta madura no consiste en recitar una lista de recetas, sino en explicar por qué determinado modelo es sensible a cierta transformación y otro no. En ciencia de datos, el buen criterio suele reconocer relaciones de necesidad entre etapas que al principiante le parecen independientes.

## Parte IV. Evaluar sin autoengañarse

Una vez que el modelo aprende sobre la representación surge la pregunta más delicada de toda la materia: cómo distinguir si aprendió algo que generaliza o simplemente memorizó detalles accidentales de la muestra. La evaluación existe para responder esa pregunta con la mayor honestidad posible. Sin ella, las métricas de entrenamiento pueden convertirse en una de las formas más sofisticadas del autoengaño técnico.

## Capítulo 6. Generalización, validación y diseño experimental

### 6.1 El problema central: entrenar no es demostrar

Que un modelo funcione bien sobre los datos con los que fue ajustado no prueba nada sobre su desempeño futuro. Esa frase merece ser repetida porque es una de las ideas más importantes del área. Un algoritmo flexible puede capturar estructura real o puede adaptarse a peculiaridades irrepetibles del conjunto de entrenamiento. A simple vista, ambos escenarios pueden lucir igual.

La evaluación no es entonces una ceremonia al final del pipeline. Es el marco que da sentido a todo lo anterior. Solo cuando diseñamos con cuidado una situación parecida al uso futuro podemos estimar si el comportamiento observado tiene chances de sostenerse fuera de muestra.

### 6.2 Conjuntos de entrenamiento, validación y prueba

La separación clásica entre entrenamiento, validación y prueba refleja tres funciones distintas. El conjunto de entrenamiento se usa para ajustar parámetros. El de validación ayuda a comparar variantes, hiperparámetros o familias de modelos. El de prueba queda reservado para estimar el desempeño final en datos que no intervinieron en las decisiones anteriores.

La lógica es simple, pero a menudo se viola sin advertirlo. En cuanto usamos un conjunto para decidir algo del modelo, ese conjunto deja de ser una referencia externa. Ya influyó en la construcción del sistema. Por eso el conjunto de prueba debe cuidarse como un recurso escaso. No es un adorno metodológico; es una barrera contra la inflación artificial de confianza.

### 6.3 Hold-out y validación cruzada

El esquema hold-out realiza una sola partición entre entrenamiento y prueba. Es rápido y claro, pero la estimación resultante puede depender demasiado del azar de esa división particular. La validación cruzada k-fold busca corregir ese problema repartiendo los datos en varios pliegues y repitiendo entrenamiento y validación sobre combinaciones distintas.

El principio que importa es más general que la técnica concreta. El esquema de validación debe parecerse a la situación real de generalización. Si hay clases desbalanceadas, conviene estratificar. Si hay grupos naturales, como múltiples observaciones por paciente, conviene separar por grupo. Si se mezclan en la evaluación casos que en la realidad no serían independientes, el diseño experimental ya empezó a contar una historia demasiado optimista.

### 6.4 Validación temporal y walk-forward

Cuando el tiempo participa del problema, particionar al azar puede destruir la lógica de uso. El futuro no debería ayudar a predecir el pasado. Por eso en series temporales o en contextos con deriva conviene respetar la cronología. La validación walk-forward hace exactamente eso: entrena sobre un bloque histórico y valida sobre un bloque posterior, repitiendo el proceso a medida que avanza la línea temporal.

Si un modelo solo funciona cuando se mezclan pasado y futuro, el problema no es que el fenómeno sea difícil; el problema es que la evaluación fue irreconocible respecto del escenario real. Y una evaluación irreconocible produce una tranquilidad que no merece ser creída.

### 6.5 Nested cross-validation y comparación honesta de modelos

Cuando el ajuste de hiperparámetros es intenso, incluso la validación cruzada puede volverse optimista si se usa a la vez para elegir y para estimar el rendimiento. La nested cross-validation separa esas funciones mediante un bucle interno, dedicado a la selección, y un bucle externo, reservado para evaluar.

En la práctica cotidiana no siempre será imprescindible, pero su lección conceptual es muy valiosa. Cada decisión guiada por los datos consume información. Si una misma evidencia sirve primero para elegir y luego para confirmar, nuestra confianza crece más rápido que el conocimiento real.

### 6.6 Overfitting, underfitting y curvas de aprendizaje

El underfitting aparece cuando el modelo es demasiado rígido o la representación demasiado pobre. En ese caso el error queda alto tanto en entrenamiento como en validación. El overfitting aparece cuando el modelo se adapta en exceso a las particularidades del entrenamiento: allí el error de entrenamiento cae mucho, mientras la validación no acompaña o incluso empeora.

Las curvas de aprendizaje permiten ver estos regímenes con más claridad que un score aislado. Si con más datos el error de validación disminuye y la brecha respecto del entrenamiento se reduce, probablemente estamos frente a un problema de alta varianza. Si ambos errores siguen altos, quizá falten mejores features, más flexibilidad o incluso una reformulación del target. Mirar cómo cambia el modelo cuando cambia la cantidad de información dice mucho más que quedarse con una única cifra final.

### 6.7 Sesgo y varianza como marco unificador

En regresión con pérdida cuadrática suele escribirse la descomposición

\[
\mathbb{E}\left[(Y-\hat f(X))^2\right] = \text{Bias}^2 + \text{Variance} + \text{Noise}.
\]

El sesgo mide cuánto se aparta, en promedio, la predicción esperada del modelo respecto de la función verdadera. La varianza mide cuánto cambiaría esa predicción si reentrenáramos con otras muestras. El ruido representa la parte irreducible del fenómeno, aquello que no depende de haber elegido mal el modelo, sino de la propia incertidumbre del sistema observado.

La utilidad de esta descomposición es conceptual. Ayuda a ver que distintas intervenciones corrigen problemas distintos. Aumentar flexibilidad puede reducir sesgo, pero aumentar varianza. Regularizar hace lo contrario. Agregar datos ayuda sobre todo contra la varianza. Mejorar la representación puede bajar ambas cosas a la vez. No hace falta recitar la fórmula para beneficiarse de esta forma de pensar; basta con entender qué clase de error estamos intentando mover.

### 6.8 Significancia estadística y relevancia práctica

Una mejora puede ser estadísticamente estable y, sin embargo, irrelevante para la decisión. También puede ocurrir lo contrario: una diferencia modesta en promedio puede volverse muy importante justo en el segmento donde el negocio o el riesgo realmente se juega. Por eso evaluar no es solo comparar medias o reportar intervalos. Es traducir diferencias a consecuencias.

Cuántos casos adicionales se detectan. Cuánto costo se evita. Qué tan estable es la mejora entre folds, períodos o subgrupos. Si la ganancia aparece donde importa o se diluye precisamente en la zona crítica. La estadística ayuda a no sobrerreaccionar al azar; el criterio práctico ayuda a no confundir una diferencia medible con una diferencia valiosa.

## Capítulo 7. Métricas: cómo cuantificar el error sin perder el sentido del problema

### 7.1 Matriz de confusión y lectura estructural del error

En clasificación binaria, la matriz de confusión organiza el resultado en cuatro celdas: verdaderos positivos, falsos positivos, verdaderos negativos y falsos negativos. Esa tabla aparentemente simple es más instructiva que muchos scores agregados porque obliga a mirar la anatomía del error.

Supongamos mil casos, cincuenta positivos reales, cuarenta detectados correctamente, diez positivos que el sistema dejó pasar y sesenta negativos marcados erróneamente como positivos. En ese escenario, \(TP=40\), \(FN=10\), \(FP=60\) y \(TN=890\). Mucho antes de calcular métricas derivadas, ya podemos razonar. Vemos cuántos casos se recuperan, cuántas falsas alarmas se generan y qué tipo de costo podría dominar. La matriz de confusión importa porque devuelve estructura a lo que, de otro modo, se resumiría en un solo número.

### 7.2 Accuracy, precision, recall, especificidad y F1

La accuracy

\[
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
\]

mide la proporción total de aciertos. Puede ser útil, pero se vuelve engañosa cuando una clase domina. La precision

\[
\text{Precision} = \frac{TP}{TP + FP}
\]

responde a una pregunta muy concreta: de todo lo que marqué como positivo, cuánto era realmente positivo. El recall

\[
\text{Recall} = \frac{TP}{TP + FN}
\]

invierte la mirada: de todos los positivos reales, cuántos encontré. La especificidad

\[
\text{Specificity} = \frac{TN}{TN + FP}
\]

mide la capacidad de reconocer negativos. Y el F1-score

\[
F_1 = 2\cdot \frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
\]

intenta equilibrar precisión y recall castigando situaciones en las que una de las dos es muy baja.

Más que repetir fórmulas, conviene recordar la pregunta humana que cada una responde. Si la clase positiva es muy rara, accuracy puede ser impecable aun cuando el modelo ignore casi todos los casos importantes. En cambio, recall y precision obligan a mirar más de cerca dónde se está pagando el error.

### 7.3 Curvas ROC, AUC y curvas Precision-Recall

Muchos modelos no entregan una clase, sino un score o una probabilidad. Al variar el umbral, cambian las tasas de verdaderos y falsos positivos. La curva ROC resume justamente esa familia de compromisos, y el AUC-ROC mide la capacidad global del modelo para discriminar entre clases a lo largo de todos los umbrales posibles.

La curva Precision-Recall pone el foco en otro aspecto: cuánta precisión conserva el modelo a medida que aumenta la recuperación de positivos. En contextos con clase rara suele ser más informativa que la ROC, porque allí un modelo puede mantener un AUC-ROC razonable y, sin embargo, tener una precision muy pobre justo en la región que operativamente interesa. Elegir entre ROC y PR no es una cuestión estética. Es decidir qué tipo de degradación queremos hacer visible.

### 7.4 Umbral de decisión y calibración

Usar 0.5 como umbral por costumbre rara vez está justificado. El umbral correcto depende del costo relativo de los errores, del volumen de casos que el sistema puede procesar y del tipo de política operativa que se quiere sostener. Un modelo probabilístico bien usado no obliga a cortar siempre en el mismo punto; ofrece información para elegir un corte razonable.

Además, discriminar bien no es lo mismo que estar calibrado. Un modelo está bien calibrado cuando, entre los casos a los que asigna probabilidad 0.8, el evento ocurre aproximadamente el 80% de las veces. Un sistema puede ordenar correctamente los casos de mayor a menor riesgo y, al mismo tiempo, sobrestimar o subestimar sistemáticamente esas probabilidades. Si la salida se usará como score de ranking, tal vez eso no sea grave. Si la salida se interpretará como probabilidad operativa, la calibración se vuelve central.

### 7.5 Métricas de regresión

En regresión el error se mide como distancia entre el valor real y la predicción. El MAE

\[
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat y_i|
\]

tiene la ventaja de mantenerse en las mismas unidades del target y no exagerar tanto el peso de los errores extremos. El MSE

\[
MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i-\hat y_i)^2
\]

y su raíz, el RMSE, penalizan con mayor dureza los errores grandes. El coeficiente

\[
R^2 = 1 - \frac{\sum_{i=1}^{n}(y_i-\hat y_i)^2}{\sum_{i=1}^{n}(y_i-\bar y)^2}
\]

indica cuánto mejora el modelo respecto de predecir siempre la media.

Cada una de estas métricas cuenta una historia distinta. MAE es más fiel a un costo lineal del error. RMSE refleja mejor escenarios donde los errores grandes son especialmente caros. \(R^2\) resume ganancia relativa respecto de un baseline simple, pero no garantiza que el error absoluto sea aceptable. Dos modelos con el mismo \(R^2\) pueden ser muy distintos desde el punto de vista operativo.

### 7.6 Métricas internas y externas en clustering

Cuando no hay etiquetas observadas, la evaluación deja de parecerse a la de clasificación o regresión. El índice silhouette,

\[
s(i)=\frac{b(i)-a(i)}{\max\{a(i),b(i)\}},
\]

compara qué tan cerca está un punto de su propio cluster respecto del cluster alternativo más próximo. Valores altos sugieren buena cohesión y separación.

Existen también índices como Davies-Bouldin y, cuando hay etiquetas externas disponibles para comparar, medidas como ARI o NMI. Pero ninguna métrica interna reemplaza la interpretación sustantiva. Un clustering puede lucir impecable según silhouette y seguir siendo inútil si la representación inicial ya había borrado la estructura relevante o si los grupos no sostienen decisiones diferentes.

### 7.7 Elegir la métrica correcta

La mejor métrica no es la más difundida, sino la que mejor refleja la clase de error que de verdad importa. En un problema con positivos raros y valiosos, accuracy puede ser un espejismo. Si la prioridad está en la calidad probabilística, la calibración gana peso. Si solo interesan los primeros lugares de un ranking, quizá convenga mirar precisión en top-k antes que un score global.

La madurez técnica se hace visible aquí. No en saber muchas métricas, sino en poder justificar por qué una de ellas traduce mejor que otra el costo real del error y la forma concreta de la decisión.

## Parte V. Modelos supervisados fundamentales

Recién después de formular bien el problema, leer el dato, construir la representación y diseñar la evaluación tiene pleno sentido estudiar modelos concretos. El objetivo de esta parte no es acumular recetas, sino entender distintas formas de imponer estructura sobre un mismo problema.

## Capítulo 8. Regresión lineal: una base que sigue siendo esencial

### 8.1 Por qué sigue importando un modelo lineal

La regresión lineal conserva un valor extraordinario porque hace visibles, con muy poca niebla, las conexiones entre hipótesis, pérdida, estimación e interpretación. Además sigue siendo útil en muchos escenarios donde importa la explicabilidad, donde la relación es aproximadamente aditiva o donde se necesita un baseline fuerte y legible.

El modelo clásico para una respuesta continua se escribe

\[
y = \beta_0 + \beta_1x_1 + \cdots + \beta_px_p + \varepsilon,
\]

donde \(\beta_0\) es el intercepto, los \(\beta_j\) son coeficientes y \(\varepsilon\) representa aquello que el modelo no consigue explicar. Que el modelo sea lineal significa que es lineal en los parámetros. Podemos incluir \(x^2\), interacciones o \(\log x\) y seguir en la familia lineal mientras la combinación permanezca lineal en \(\beta\).

### 8.2 Mínimos cuadrados ordinarios

El problema que resuelve la regresión lineal puede plantearse de una manera casi geométrica: entre todos los planos o hiperplanos posibles, queremos el que deja la respuesta observada lo más cerca posible. En forma matricial escribimos

\[
y = X\beta + \varepsilon
\]

y estimamos \(\beta\) resolviendo

\[
\hat\beta = \arg\min_\beta \|y - X\beta\|_2^2.
\]

Es decir, buscamos los coeficientes que minimizan la suma de cuadrados de los residuos.

Al derivar esa expresión surgen las ecuaciones normales

\[
X^TX\hat\beta = X^Ty,
\]

y, cuando \(X^TX\) es invertible,

\[
\hat\beta = (X^TX)^{-1}X^Ty.
\]

Estas fórmulas no son solo álgebra. Dicen algo intuitivo. \(X^TX\) resume cómo se relacionan las variables entre sí. \(X^Ty\) resume cómo se alinean con la respuesta. Resolver el sistema es encontrar el compromiso lineal que mejor aproxima esa relación en el sentido de la pérdida cuadrática.

### 8.3 Interpretación geométrica

La lectura geométrica hace todavía más transparente el asunto. La solución de mínimos cuadrados puede verse como la proyección ortogonal del vector \(y\) sobre el subespacio generado por las columnas de \(X\). El modelo busca, entre todas las combinaciones lineales posibles de las features, aquella que queda más cerca de la respuesta observada.

Eso explica de inmediato por qué la colinealidad genera problemas. Si varias columnas apuntan casi en la misma dirección, el subespacio queda mal condicionado y el reparto de efecto entre variables se vuelve inestable. La geometría muestra con claridad algo que, leído solo en la fórmula, podría parecer una rareza algebraica.

### 8.4 Cómo interpretar los coeficientes

Bajo una especificación razonable, el coeficiente \(\beta_j\) indica cuánto cambia la respuesta esperada cuando \(x_j\) aumenta una unidad, manteniendo constantes las demás variables. Esa frase exige mucho más cuidado del que parece. No describe un efecto puro del mundo; describe una relación condicional dentro del modelo.

Si, por ejemplo, \(\beta_{\text{horas}} = 2\) en un modelo de notas y horas de estudio, la lectura es que, manteniendo constantes las otras variables incluidas, una hora adicional se asocia con dos puntos más en la nota esperada. Eso no significa que forzar causalmente una hora extra produzca exactamente ese aumento. La interpretación es asociativa, no intervencional.

Además, las transformaciones cambian la lectura. Si el target se modeló en logaritmos, los coeficientes suelen interpretarse en términos porcentuales aproximados. Si una variable fue estandarizada, una unidad ya no es una unidad del fenómeno, sino un desvío estándar. Interpretar bien exige recordar siempre en qué escala vive el modelo.

### 8.5 Supuestos clásicos y qué significa violarlos

Los supuestos clásicos de la regresión lineal no deberían estudiarse como una lista ceremonial. Cada uno protege una parte de la interpretación. Si la relación relevante no es aproximadamente lineal y no incorporamos transformaciones o términos adicionales, el modelo queda mal especificado. Si hay heterocedasticidad, algunos resultados inferenciales pierden confiabilidad. Si los errores están correlacionados en el tiempo o por grupos, la evaluación puede resultar demasiado optimista. Si la multicolinealidad es severa, los coeficientes se vuelven inestables.

La mejor forma de recordarlos es preguntarse qué se rompe cuando fallan. Un supuesto no es una superstición estadística; es una condición bajo la cual ciertas lecturas del modelo dejan de ser razonables.

### 8.6 Residuos, diagnóstico y límites del modelo

El residuo \(e_i = y_i - \hat y_i\) condensa el error del modelo para una observación. Mirar residuos es mirar qué parte del fenómeno quedó afuera. Un patrón curvo en el gráfico de residuos sugiere que el modelo lineal dejó no linealidad sin capturar. Un abanico puede insinuar heterocedasticidad. Unos pocos puntos muy influyentes pueden estar moviendo toda la estimación.

Una de las grandes virtudes pedagógicas de la regresión lineal es que expone con nitidez sus propias limitaciones. No es solo un modelo útil; es también una escuela de pensamiento sobre lo que significa ajustar, diagnosticar y reconocer cuándo la forma elegida ya no alcanza.

### 8.7 Regularización: Ridge, Lasso y Elastic Net

Cuando hay muchas variables, colinealidad o riesgo de sobreajuste, conviene penalizar complejidad. En Ridge se minimiza

\[
\|y-X\beta\|_2^2 + \lambda\|\beta\|_2^2.
\]

En Lasso se usa

\[
\|y-X\beta\|_2^2 + \lambda\|\beta\|_1,
\]

y en Elastic Net se combinan ambas penalizaciones.

La intuición es más importante que la expresión formal. Aceptamos introducir un poco de sesgo para reducir varianza y ganar estabilidad. Ridge tiende a encoger coeficientes sin anularlos. Lasso puede llevar algunos exactamente a cero y, por eso, ayuda también como mecanismo de selección. En todos los casos la regularización expresa una idea general: preferimos modelos que expliquen bien sin apoyarse en una complejidad excesivamente frágil.

### 8.8 Cuándo conviene usar regresión lineal

La regresión lineal conviene cuando importa la interpretabilidad, cuando una relación aditiva resulta razonable, cuando se necesita un baseline fuerte o cuando el costo de un modelo más opaco no está justificado. También conviene como herramienta intelectual, porque obliga a pensar con cuidado en escala, supuestos, residuos y causalidad mal entendida.

No es un modelo viejo superado por otros más modernos. Es una pieza fundamental de alfabetización estadística y, en muchos contextos, una solución plenamente competitiva.

## Capítulo 9. Regresión logística: probabilidad para decisiones binarias

### 9.1 Por qué no alcanza la regresión lineal para clasificar

Cuando el target es binario, ajustar una recta y luego aplicar un umbral parece una salida tentadora. El problema es que la recta no está obligada a permanecer entre 0 y 1 y, además, la relación entre covariables y probabilidad rara vez es lineal en la escala original. La regresión logística resuelve esa dificultad modelando

\[
P(Y=1 \mid X=x)=\sigma(\beta_0+\beta^Tx),
\]

donde

\[
\sigma(z)=\frac{1}{1+e^{-z}}
\]

es la función sigmoide.

La magia aquí no es misteriosa. La combinación lineal sigue existiendo, pero ahora pasa por una función que la convierte en probabilidad válida. Eso permite conservar buena parte de la interpretabilidad lineal sin violentar la naturaleza binaria del problema.

### 9.2 Odds y logit

La intuición profunda de la logística aparece cuando dejamos de mirar la probabilidad y pasamos a mirar los odds

\[
\text{odds}=\frac{p}{1-p}.
\]

Si tomamos logaritmos obtenemos el logit:

\[
\log\left(\frac{p}{1-p}\right)=\beta_0+\beta^Tx.
\]

La linealidad, entonces, no vive en la probabilidad sino en el log-odds. Eso importa mucho porque evita una interpretación ingenua. El modelo no dice que cada unidad adicional en una variable aumenta la probabilidad en una cantidad fija. Dice que cambia linealmente el log-odds, y esa modificación se traduce a la probabilidad de manera no lineal.

### 9.3 Estimación por máxima verosimilitud

Si cada \(Y_i\) se modela como una Bernoulli con probabilidad \(p_i\), la verosimilitud conjunta es

\[
\mathcal{L}(\beta) = \prod_{i=1}^{n} p_i^{y_i}(1-p_i)^{1-y_i}.
\]

Tomar logaritmos simplifica el cálculo y lleva a

\[
\ell(\beta)=\sum_{i=1}^{n}\left[y_i\log p_i + (1-y_i)\log(1-p_i)\right].
\]

Maximizar esta expresión equivale a minimizar la log-loss. La lectura intuitiva es clara: elegimos los parámetros que vuelven más plausible la muestra observada bajo el modelo. La logística no surge de una costumbre computacional, sino de una forma muy natural de ajustar probabilidades a datos binarios.

### 9.4 Cómo interpretar un coeficiente logístico

Si \(x_j\) aumenta una unidad, el log-odds cambia en \(\beta_j\). Al exponenciar ese coeficiente obtenemos

\[
e^{\beta_j},
\]

que indica por qué factor se multiplican los odds cuando \(x_j\) aumenta una unidad, manteniendo constantes las demás variables. Si \(e^{\beta_j}=1.5\), los odds aumentan un 50%. Esa frase no equivale a decir que la probabilidad sube cincuenta puntos porcentuales. El cambio en la probabilidad depende del punto de partida. Cerca de 0.5 la variación puede ser notable; cerca de 0 o de 1, mucho menor.

Entender esta diferencia marca una frontera clara entre repetir una interpretación estándar y comprender realmente qué está diciendo el modelo.

### 9.5 Umbral, desbalance y decisión

La regresión logística produce probabilidades. Convertirlas en clases exige elegir un umbral, y usar 0.5 por simple reflejo puede ser una mala idea en problemas desbalanceados o con costos asimétricos. Allí aparece una de las grandes virtudes de la logística: obliga a separar tres capas que suelen confundirse, la estimación de probabilidad, la regla de clasificación y la decisión operativa.

Esa separación es conceptualmente saludable. Permite discutir calibración sin mezclarla con ranking, y costos operativos sin mezclarlo todo con la función de entrenamiento. Como baseline, la logística sigue siendo excelente precisamente porque ordena bien estas ideas.

### 9.6 Regularización y calibración

La logística admite penalizaciones L1 y L2 igual que la regresión lineal, lo que resulta muy útil frente a alta dimensionalidad, colinealidad y riesgo de sobreajuste. Pero además introduce con mucha nitidez otra cuestión: un modelo puede discriminar bien y, sin embargo, estar mal calibrado.

En problemas donde la probabilidad se interpreta directamente, como seguros, salud o riesgo, esa distinción es decisiva. A veces basta con saber quién está antes y quién después en un ranking. Otras veces necesitamos que un 0.2 se parezca de verdad a una probabilidad del 20%. La logística permite ver con claridad que ambas exigencias no son equivalentes.

### 9.7 Cuándo falla la intuición lineal

Pensar la logística como una versión lineal para clasificación ayuda al comienzo, pero se vuelve engañoso si se toma demasiado literalmente. La probabilidad no cambia linealmente con la covariable. Los efectos marginales dependen de la zona del espacio donde se evalúan. Un mismo cambio en una feature no tiene el mismo impacto cuando el caso ya es muy probable que cuando está cerca del umbral.

Comprender esta curvatura evita muchos malentendidos. La logística no es solo una receta útil; es una invitación a pensar cómo una relación aparentemente simple cambia de significado cuando lo que está en juego es una probabilidad.

## Capítulo 10. KNN, Naive Bayes y SVM: tres lógicas distintas de aprendizaje

### 10.1 K-Nearest Neighbors: aprender por vecindad

KNN parte de una intuición muy natural: casos parecidos deberían tener respuestas parecidas. Cuando llega una observación nueva, el algoritmo busca los \(k\) vecinos más cercanos y decide a partir de ellos, ya sea por voto en clasificación o por promedio en regresión.

La dificultad real está escondida en una palabra aparentemente obvia: parecido. Si usamos distancia euclídea,

\[
d(x,x')=\sqrt{\sum_{j=1}^{p}(x_j-x'_j)^2},
\]

las variables mal escaladas o irrelevantes pueden dominar por completo la noción de cercanía. Además, \(k\) regula un compromiso central. Con valores pequeños el modelo se adapta mucho al detalle local y puede volverse ruidoso. Con valores grandes gana estabilidad, pero corre el riesgo de borrar estructura fina. KNN enseña con crudeza que aprender también es definir una geometría razonable para comparar casos.

### 10.2 Naive Bayes: independencia condicional como aproximación útil

Naive Bayes parte del teorema de Bayes,

\[
P(Y \mid X)=\frac{P(X \mid Y)P(Y)}{P(X)},
\]

y hace un supuesto fuerte:

\[
P(X_1,\ldots,X_p \mid Y)=\prod_{j=1}^{p}P(X_j \mid Y).
\]

Asume, en otras palabras, independencia condicional entre features dada la clase.

Ese supuesto rara vez es literalmente verdadero. Sin embargo, el modelo suele funcionar sorprendentemente bien. La lección es profunda. Un modelo puede ser útil aun cuando su descripción del mundo sea tosca, siempre que conserve la estructura suficiente para decidir. Naive Bayes muestra que, a veces, una simplificación brutal gana por manejabilidad y por resistencia en alta dimensión, especialmente en texto.

### 10.3 SVM: margen máximo y robustez geométrica

Las máquinas de soporte vectorial parten de otra intuición. Si los datos son separables, no alcanza con encontrar una frontera cualquiera. Conviene buscar la que los separe con el mayor margen posible. Formalmente, queremos hallar \(w\) y \(b\) tales que

\[
y_i(w^Tx_i+b)\ge 1
\]

para cada observación, minimizando al mismo tiempo

\[
\frac{1}{2}\|w\|^2.
\]

Minimizar la norma de \(w\) equivale a maximizar el margen.

La intuición es geométrica y muy poderosa. Una frontera que deja más espacio libre alrededor suele generalizar mejor que otra que pasa demasiado ajustada al entrenamiento. SVM convierte esa idea en un criterio preciso.

### 10.4 Kernel trick y no linealidad

Cuando una frontera lineal no alcanza, la idea de kernel permite trabajar como si los datos se proyectaran a un espacio de mayor dimensión sin construir explícitamente esa proyección. El resultado práctico es que podemos obtener fronteras no lineales manteniendo una formulación geométrica bien controlada.

El kernel RBF es el ejemplo clásico. Sus hiperparámetros, en particular \(C\) y \(\gamma\), regulan cuánto se toleran errores y cuán local se vuelve la influencia de cada punto. Si \(\gamma\) es demasiado alto, cada observación domina solo una vecindad mínima y el sistema puede sobreajustar con facilidad. La flexibilidad aparece, pero no gratis.

### 10.5 Comparación conceptual entre KNN, Naive Bayes y SVM

Estos tres métodos son especialmente valiosos juntos porque enseñan tres filosofías distintas de aprendizaje. KNN aprende por similitud local. Naive Bayes simplifica la estructura probabilística para volverla manejable. SVM construye una frontera guiada por un principio geométrico de margen.

Saber compararlos no es recordar una tabla de pros y contras. Es entender qué sesgo inductivo trae cada uno y en qué tipo de problema ese sesgo se vuelve razonable. Cuando se aprende a ver eso, los algoritmos dejan de ser nombres sueltos y pasan a formar familias de ideas.

## Parte VI. Árboles y ensambles: potencia, no linealidad y control de varianza

Las familias lineales no agotan la práctica real. En datos tabulares, los árboles y sus ensambles ocupan un lugar central porque capturan interacciones y no linealidades con mucha eficacia. También enseñan de manera muy concreta qué significa flexibilidad, por qué un modelo puede volverse inestable y cómo la agregación permite recuperar robustez.

## Capítulo 11. Árboles de decisión: reglas interpretables con riesgo de inestabilidad

### 11.1 La idea básica de particionar recursivamente

Un árbol de decisión no ajusta una fórmula global. Divide el espacio de variables mediante preguntas sucesivas del tipo "¿\(x_j < t\)?", con el objetivo de obtener subconjuntos cada vez más homogéneos respecto del target. Vista así, la idea es muy intuitiva: en lugar de buscar una regla única para todo el espacio, el modelo va separando regiones donde el comportamiento se parece más.

Esa estrategia permite capturar interacciones y no linealidades con poco preprocesamiento. Pero también introduce una cierta fragilidad. La estructura del árbol depende de las particiones elegidas y pequeñas perturbaciones en la muestra pueden alterar bastante la forma final. Su virtud y su debilidad nacen de la misma fuente: la flexibilidad local.

### 11.2 Entropía, información y algoritmo ID3

En clasificación, una manera clásica de medir la impureza de un nodo es la entropía

\[
H(S) = -\sum_{k=1}^{K} p_k \log_2 p_k,
\]

donde \(p_k\) es la proporción de la clase \(k\) en el nodo \(S\). Si el nodo es puro, la entropía vale cero. Si las clases están mezcladas, aumenta. ID3 elige las particiones que más reducen esa incertidumbre.

Lo importante es entender la lógica. El árbol busca preguntas que ordenen el target, que vuelvan menos confuso el reparto de clases dentro de cada rama. No persigue divisiones "bonitas" ni semánticamente atractivas. Persigue divisiones que reduzcan incertidumbre según el criterio elegido.

### 11.3 Gini, CART y C4.5

Otra medida muy usada es el índice de Gini,

\[
Gini(S) = 1 - \sum_{k=1}^{K} p_k^2.
\]

La idea es análoga: cuanto más puro es el nodo, menor es su impureza. CART suele usar Gini en clasificación y error cuadrático en regresión. C4.5 extiende ideas de ID3 para manejar mejor variables continuas, faltantes y ciertos sesgos hacia atributos con muchas categorías.

No hace falta memorizar siglas como si fueran genealogías de software. Lo que conviene retener es que distintos árboles definen de manera ligeramente distinta qué significa un buen split y, por lo tanto, qué estructura tienden a privilegiar.

### 11.4 Árboles para regresión

En regresión, el árbol particiona el espacio de modo que los valores del target dentro de cada nodo sean lo más homogéneos posible. Al final, cada hoja suele predecir la media de la respuesta en esa región. La intuición es la de un modelo por regiones: el espacio se corta en partes y cada parte recibe una predicción constante.

Esa estructura explica tanto su potencia como sus límites. Permite capturar relaciones no lineales sin necesidad de escribirlas explícitamente, pero produce superficies escalonadas y puede volverse muy sensible a cambios pequeños en la muestra.

### 11.5 Crecimiento, poda y sobreajuste

Si se deja crecer un árbol sin restricciones, lo más probable es que termine memorizando detalles accidentales del entrenamiento. La pre-poda impone límites de profundidad, tamaño mínimo de nodo o mejora mínima para dividir. La post-poda deja crecer más y luego recorta ramas que no justifican la complejidad que agregan.

Aquí aparece una lección general que trasciende a los árboles. La interpretabilidad de una estructura no la vuelve automáticamente buena. Un árbol muy profundo puede verse explícito y legible en el papel, pero ser simplemente una representación gráfica del sobreajuste.

### 11.6 Importancia de variables y cautelas

Los árboles suelen ofrecer medidas de importancia basadas en la reducción acumulada de impureza. Son útiles, aunque no inocentes. Tienden a favorecer variables con muchos puntos de corte o alta cardinalidad y, cuando varias columnas comparten señal, la importancia puede repartirse de maneras inestables o arbitrarias.

Por eso conviene complementar esa lectura con permutation importance, con análisis de estabilidad entre reentrenamientos y con criterio sustantivo. Que una variable sea importante para el árbol no significa, por sí solo, que intervenir sobre ella en el mundo real vaya a producir el efecto deseado.

### 11.7 Cuándo usar árboles

Los árboles resultan atractivos cuando interesa una primera aproximación legible, cuando se esperan interacciones y no linealidades, o cuando el preprocesamiento debe mantenerse relativamente simple. Como modelo único suelen ser superados por ensambles más estables, pero siguen siendo extraordinariamente valiosos como baseline interpretable y como herramienta conceptual para pensar el espacio de decisiones.

## Capítulo 12. Ensambles: combinar modelos para ganar estabilidad o capacidad

### 12.1 La intuición estadística detrás de los ensambles

Si varios modelos se equivocan de maneras diferentes, promediarlos puede reducir el error total. En una forma simplificada, si \(M\) predictores tienen varianza \(\sigma^2\) y correlación promedio \(\rho\), la varianza de su promedio puede escribirse como

\[
\text{Var}(\bar f) = \rho\sigma^2 + \frac{1-\rho}{M}\sigma^2.
\]

La lectura es instructiva. Proliferar modelos ayuda, pero ayuda mucho más cuando no todos se equivocan de la misma manera. Cantidad sin diversidad rinde bastante menos de lo que uno imagina. Esta es la intuición estadística que sostiene a los ensambles.

### 12.2 Bagging

El bagging entrena muchos modelos sobre muestras bootstrap distintas del conjunto de entrenamiento y luego agrega sus predicciones. Su efecto principal es reducir varianza, algo especialmente valioso cuando el modelo base es inestable, como ocurre con los árboles.

Hay algo conceptualmente elegante en esta idea. Toma una debilidad del árbol, su sensibilidad a pequeños cambios en la muestra, y la convierte en fuente de diversidad útil. Lo que individualmente era fragilidad se transforma, al agregarse, en robustez colectiva.

### 12.3 Random Forest

Random Forest suma al bagging una fuente adicional de aleatoriedad: en cada split solo considera un subconjunto aleatorio de variables. Eso disminuye la correlación entre árboles y hace más efectivo el promedio final. En datos tabulares, esa combinación suele producir modelos muy competitivos con poco ajuste fino.

El costo es la pérdida de transparencia inmediata respecto del árbol único. Pero la ganancia en estabilidad y desempeño suele ser grande. Aparece aquí una tensión clásica de la disciplina: a medida que la representación colectiva se vuelve más poderosa, la explicación local se vuelve menos directa.

### 12.4 Boosting: aprender corrigiendo errores previos

Mientras el bagging trabaja en paralelo para reducir varianza, el boosting construye el modelo secuencialmente. Cada nuevo componente intenta corregir los errores que el ensamble actual todavía comete. En Gradient Boosting esa idea puede resumirse como

\[
F_m(x)=F_{m-1}(x)+\eta h_m(x),
\]

donde \(h_m\) es un modelo débil que corrige el gradiente negativo de la pérdida actual y \(\eta\) es la tasa de aprendizaje.

La intuición es poderosa. En vez de buscar desde el inicio una solución compleja, el modelo va afinando una función paso a paso, enfocándose cada vez en lo que todavía no resolvió bien. Esa capacidad de corregir secuencialmente vuelve al boosting muy expresivo, pero también lo hace sensible a la regularización y al tuning.

### 12.5 XGBoost y regularización moderna en árboles

XGBoost es una implementación particularmente eficiente y regularizada del gradient boosting sobre árboles. Su objetivo, de manera simplificada, puede escribirse como

\[
\text{Obj} = \sum_{i=1}^{n} l(y_i,\hat y_i) + \sum_{k=1}^{K}\Omega(f_k),
\]

donde la primera parte mide error y la segunda penaliza complejidad de cada árbol.

Lo importante aquí no es aprender de memoria los detalles de una librería famosa, sino comprender la idea que cristaliza: combinar la potencia del boosting con mecanismos explícitos para controlar sobreajuste y costo computacional. En esa síntesis se explica buena parte de su éxito práctico.

### 12.6 Voting, stacking, cascading, ensambles homogéneos e híbridos

No todos los ensambles son bagging o boosting. En voting, varios modelos votan o promedian probabilidades. En stacking, las salidas de distintos modelos se convierten en entradas de un meta-modelo que aprende a combinarlas. Para que ese procedimiento sea honesto, esas predicciones base deben generarse fuera de fold; de lo contrario, el meta-modelo aprendería sobre leakage.

La idea que unifica estas variantes es aprovechar complementariedades entre modelos sin olvidar que cada nueva capa también añade complejidad experimental. Un ensamble no es bueno por acumular piezas, sino por extraer diversidad útil sin perder control metodológico.

### 12.7 Performance versus interpretabilidad

Los ensambles suelen ganar rendimiento y robustez, pero rara vez gratis. La transparencia inmediata disminuye. Ese costo no debe esconderse detrás de frases cómodas. Debe asumirse como una decisión de diseño. En algunos problemas la mejora predictiva justifica con claridad esa pérdida interpretativa. En otros, una logística o un árbol pequeño pueden ser preferibles aunque cedan algunos puntos de score.

No existe una respuesta universal. Existe un equilibrio entre precisión, estabilidad, explicabilidad, costo de mantenimiento y consecuencias de error. La madurez profesional consiste en elegir ese equilibrio con argumentos, no por inercia ni por moda.

## Parte VII. Aprendizaje no supervisado y reducción de dimensión

No todos los problemas vienen con etiquetas. A veces la tarea es encontrar estructura, resumir información o construir una representación más compacta. Estos problemas obligan a pensar con especial cuidado qué significa que dos casos sean parecidos y qué estructura queremos preservar cuando comprimimos el mundo observado.

## Capítulo 13. Clustering: descubrir estructura sin etiquetas

### 13.1 Qué problema intenta resolver el clustering

Clustering no consiste en revelar una verdad oculta escrita de antemano en la naturaleza. Consiste en construir una partición del espacio de datos tal que los puntos del mismo grupo resulten más parecidos entre sí que a los de otros grupos, según una noción elegida de similitud. Esa aclaración es importante porque evita una ilusión frecuente: creer que los clusters encontrados existen con independencia del método, la escala y la representación.

En realidad, los clusters dependen de cómo escribimos el problema. Cambiar la escala, la métrica o el algoritmo puede alterar por completo la estructura hallada. Eso no invalida el clustering. Solo obliga a usarlo con humildad conceptual.

### 13.2 Distancia, escala y significado de "parecido"

En datos numéricos suele usarse distancia euclídea, pero no siempre es la noción correcta de cercanía. En datos mixtos, categóricos o textuales puede ser más razonable otra disimilitud. Además, si las escalas son muy diferentes, una sola variable puede dominar la distancia total y ordenar el espacio a su antojo.

El clustering vuelve especialmente visible una verdad general de la ciencia de datos: el algoritmo no corrige una representación pobre; la amplifica. Si la noción de parecido no tiene sentido para el problema, los grupos que aparezcan serán técnicamente consistentes con esa mala elección, pero sustantivamente irrelevantes.

### 13.3 Tendencia al clustering y estadístico de Hopkins

Antes de agrupar conviene preguntarse si el dataset muestra realmente indicios de estructura agrupable o si se parece más a una nube continua sin grupos claros. El estadístico de Hopkins se diseñó precisamente para esa intuición. Compara distancias entre puntos observados y puntos aleatorios generados en el mismo espacio. Valores cercanos a 1 sugieren tendencia al agrupamiento; valores alrededor de 0.5 sugieren aleatoriedad.

Más allá del detalle técnico, la enseñanza es saludable. No siempre está justificado clusterizar. A veces lo que falta no es un mejor algoritmo, sino una buena razón para agrupar.

### 13.4 K-means en profundidad

K-means busca particionar las observaciones en \(K\) grupos minimizando la suma de cuadrados intra-cluster:

\[
\min_{C_1,\ldots,C_K}\sum_{k=1}^{K}\sum_{x_i \in C_k}\|x_i-\mu_k\|^2,
\]

donde \(\mu_k\) es el centroide del cluster \(C_k\).

La intuición es directa. Cada grupo debería ser compacto alrededor de un centro. Esa sencillez vuelve a K-means útil y rápido, pero también lo ata a una geometría particular. Funciona mejor cuando los grupos son aproximadamente convexos y separados por cercanía a centroides. Si la estructura real del dato tiene formas extrañas o densidades muy diferentes, su criterio empieza a forzar una realidad que quizás no exista.

### 13.5 K-means en un caso sencillo

Si tomamos los puntos unidimensionales \(1,2,3,10,11,12\) y fijamos \(K=2\), el algoritmo tenderá a separar \(\{1,2,3\}\) de \(\{10,11,12\}\). El ejemplo es simple, pero ilustra algo importante. K-means no descubre una esencia metafísica del conjunto. Encuentra la partición que mejor optimiza su función objetivo bajo la geometría que supone. Cuando aparecen outliers o grupos de forma no convexa, esa geometría deja de ajustarse bien al problema.

### 13.6 Elegir \(K\): codo, silhouette y estabilidad

Elegir el número de clusters no es un acto mágico. El método del codo observa cómo cae la inercia intra-cluster al aumentar \(K\). El índice silhouette agrega información sobre cohesión y separación. El análisis de estabilidad pregunta cuánto cambian los grupos frente a perturbaciones o remuestreos.

Ninguno de estos criterios agota la interpretación. Puede haber varios valores de \(K\) razonables según el nivel de granularidad que interese. En clustering, como en casi todo lo importante, el número correcto no emerge por sí solo de una fórmula. Surge de una negociación entre estructura matemática y propósito analítico.

### 13.7 Clustering jerárquico

El clustering jerárquico no fija \(K\) desde el comienzo. Construye una secuencia de fusiones o particiones que puede visualizarse como un dendrograma. En la versión aglomerativa se empieza con cada observación como cluster propio y luego se van fusionando grupos según una regla de enlace.

Su interés principal es que ofrece una visión multiescala. Permite mirar la estructura a distintos niveles de resolución y preguntarse dónde empiezan a aparecer grupos razonables. A la vez, obliga a recordar que distintas reglas de enlace privilegian distintas nociones de proximidad, de modo que el dendrograma tampoco es una verdad desnuda del dato.

### 13.8 DBSCAN: densidad, ruido y formas arbitrarias

DBSCAN cambia de lógica. En lugar de buscar centroides, define clusters como regiones densas separadas por regiones de baja densidad. Esa idea le da dos virtudes muy potentes: detecta grupos de forma arbitraria y puede identificar explícitamente puntos de ruido.

Su límite aparece cuando la densidad cambia mucho entre regiones. Un único valor de \(\varepsilon\) puede ser demasiado exigente para algunos grupos y demasiado permisivo para otros. Como siempre, el algoritmo resulta adecuado cuando la estructura que presupone coincide con la estructura del fenómeno.

### 13.9 Interpretar clusters y evitar errores frecuentes

Obtener clusters es apenas el comienzo. Luego hay que describirlos, compararlos y preguntarse si habilitan decisiones distintas. Esa interpretación exige mirar centroides, distribuciones características, estabilidad y sentido sustantivo. Sin esa capa posterior, el clustering corre el riesgo de convertirse en un dibujo seductor sin consecuencia analítica.

Los errores frecuentes se repiten: creer que cualquier cluster hallado es real, elegir \(K\) solo por un índice, ignorar la sensibilidad a la representación y confundir grupos algorítmicos con clases verdaderas del mundo. El clustering es útil cuando se lo usa como herramienta para pensar estructura, no como oráculo ontológico.

## Capítulo 14. Reducción de dimensionalidad: comprimir sin perder estructura esencial

### 14.1 Por qué reducir dimensión

Reducir dimensión sirve para varias cosas a la vez. Puede ayudar a combatir ruido, resumir información redundante, aliviar costo computacional, mejorar algunos algoritmos sensibles a espacios altos y facilitar visualizaciones. Pero debajo de esas motivaciones prácticas hay una pregunta más profunda: qué estructura queremos conservar cuando comprimimos.

No todas las técnicas preservan lo mismo. Algunas priorizan varianza global. Otras, distancias. Otras, vecindades locales. Otras suponen que los datos viven cerca de una variedad no lineal. Elegir un método de reducción es decidir qué sacrificio consideramos menos grave.

### 14.2 Maldición de la dimensionalidad

Cuando la dimensión crece, varias intuiciones geométricas de baja dimensión se rompen. Los puntos tienden a parecer todos lejanos. Los volúmenes crecen explosivamente. La noción de vecindad pierde nitidez. A este fenómeno se lo llama maldición de la dimensionalidad.

No se trata de una maldición mística, sino de un cambio real en la geometría del espacio. Métodos basados en distancia o densidad empiezan a degradarse porque la información útil se diluye. Reducir dimensión no elimina mágicamente el problema, pero puede mitigarlo si la estructura relevante vive cerca de un subespacio o una variedad de menor dimensión efectiva.

### 14.3 PCA: idea, formulación y geometría

PCA busca nuevas direcciones ortogonales que capturen la mayor varianza posible. Si \(X\) es la matriz centrada, la primera componente principal resuelve

\[
\max_{\|w\|=1}\text{Var}(Xw),
\]

lo que equivale a maximizar

\[
w^T\Sigma w,
\]

donde \(\Sigma\) es la matriz de covarianza. La solución está dada por el autovector asociado al mayor autovalor.

La intuición geométrica es clara. PCA gira el sistema de coordenadas para mirar la nube de puntos desde la dirección donde más se estira. No busca la variable más importante en sentido sustantivo. Busca la dirección de mayor varianza lineal. Ese matiz es la diferencia entre usar PCA con criterio y usarlo como un rito automático.

### 14.4 Scores, loadings y varianza explicada

Los loadings indican cuánto pesa cada variable original en cada componente. Los scores son las coordenadas de cada observación en el nuevo sistema de ejes. Si los autovalores son \(\lambda_1,\ldots,\lambda_p\), la proporción de varianza explicada por la componente \(j\) es

\[
\frac{\lambda_j}{\sum_{m=1}^{p}\lambda_m}.
\]

El scree plot ayuda a decidir cuántas componentes conservar, pero no existe un porcentaje mágico universal. Guardar el 95% de la varianza puede ser sensato en un caso y completamente irrelevante en otro. Todo depende de para qué se reducirá la dimensión y qué estructura conviene no perder.

### 14.5 Qué preserva PCA y qué no

PCA preserva, del mejor modo posible bajo sus restricciones, varianza global lineal. No preserva necesariamente separabilidad de clases, estructura no lineal ni interpretabilidad sustantiva. Una variable con poca varianza puede ser muy predictiva y quedar relegada por completo. Por eso usar PCA no equivale a mejorar el modelado. A veces ayuda mucho; otras veces elimina justo la señal que importaba.

El error frecuente consiste en tratar a PCA como un preprocesamiento universalmente beneficioso. En realidad, solo conviene cuando la estructura que queremos conservar se parece a la que PCA privilegia.

### 14.6 Relación entre PCA, ruido, colinealidad y overfitting

Cuando muchas variables están correlacionadas, PCA puede condensar esa redundancia en unas pocas componentes y estabilizar modelos posteriores. También puede servir para filtrar ruido distribuido en direcciones de baja varianza. En ese sentido, actúa como una especie de compresión estructurada.

El precio es interpretativo. Las nuevas variables dejan de tener un significado directo en el lenguaje original del problema. Como tantas veces en ciencia de datos, ganar estabilidad implica aceptar una pérdida en transparencia semántica.

### 14.7 MDS: preservar distancias

El multidimensional scaling parte de una matriz de distancias o disimilitudes y busca una representación de baja dimensión que las preserve lo mejor posible. A diferencia de PCA, su prioridad no es la varianza, sino la geometría relacional entre observaciones.

Esa diferencia lo vuelve especialmente útil cuando las distancias originales tienen sentido por sí mismas y queremos una representación visual o analítica que conserve aproximadamente esa estructura. No responde a la misma pregunta que PCA, y por eso no debe compararse con él como si fueran versiones fuertes y débiles de una misma idea.

### 14.8 t-SNE: vecindades locales para visualización

t-SNE es una técnica no lineal orientada sobre todo a visualización. Su objetivo es preservar vecindades locales: puntos cercanos en alta dimensión deberían permanecer cercanos en baja dimensión. Gracias a eso suele producir mapas muy expresivos para explorar estructura local.

Pero precisamente por esa fortaleza aparece el riesgo de mala interpretación. Las distancias globales en un mapa t-SNE no deben leerse como si fueran fieles al espacio original. Ver grupos separados en dos dimensiones no prueba por sí solo que existan clusters bien definidos en alta dimensión. t-SNE es valioso como lente exploratoria, no como certificado automático de estructura.

### 14.9 ISOMAP: variedades y distancias geodésicas

ISOMAP parte de la idea de que los datos pueden vivir sobre una variedad no lineal embebida en un espacio de alta dimensión. En vez de preservar distancias euclídeas directas, intenta preservar distancias geodésicas aproximadas sobre esa variedad.

Cuando esa hipótesis es razonable, puede recuperar estructura que PCA pierde por completo. Pero si la variedad no existe o el grafo de vecindad se construye mal, el método se degrada rápido. Una vez más, el valor de la técnica depende de que su supuesto geométrico dialogue bien con el fenómeno.

### 14.10 Cómo comparar PCA, MDS, t-SNE e ISOMAP

PCA prioriza varianza global lineal. MDS prioriza distancias. t-SNE prioriza vecindades locales para visualización. ISOMAP prioriza geometría de variedad mediante distancias geodésicas. Todas son técnicas de reducción de dimensión, pero preservar dimensión no es preservar estructura de una única manera.

La pregunta correcta no es cuál es mejor en abstracto, sino qué aspecto del espacio necesitamos conservar para el uso que queremos dar a la representación. Comprender esa diferencia evita mucho uso automático de herramientas poderosas.

## Parte VIII. Redes neuronales y representación de texto

Hasta aquí vimos muchos modelos que trabajan sobre representaciones bastante explícitas. En redes neuronales y en procesamiento de lenguaje natural aparece con más fuerza otra idea: la propia representación puede ser aprendida o construida de formas mucho más ricas, especialmente cuando los datos son complejos, como texto, audio o imágenes.

## Capítulo 15. Redes neuronales: de la linealidad a representaciones jerárquicas

### 15.1 El perceptrón simple

La unidad más básica de una red neuronal calcula una combinación lineal y aplica una activación:

\[
a=\phi(w^Tx+b),
\]

donde \(w\) son pesos, \(b\) es un sesgo y \(\phi\) una función de activación.

El perceptrón simple puede verse como un clasificador lineal. Su valor pedagógico es enorme porque muestra que aprender consiste, en este nivel elemental, en ajustar una frontera que separe ejemplos en el espacio de entrada. La red neuronal no aparece de la nada como una caja mágica. Nace de extender, capa tras capa, esta idea básica.

### 15.2 La limitación del perceptrón y el problema XOR

El perceptrón simple no puede resolver problemas no linealmente separables. El ejemplo clásico es XOR. Lo interesante de este límite no es el ejercicio escolar, sino la lección conceptual que encierra: acumular linealidades no crea no linealidad. Una composición de transformaciones lineales sigue siendo lineal.

Esa constatación obliga a introducir capas ocultas y funciones de activación no lineales. Ahí empieza a aparecer la noción de representación jerárquica. La red deja de ser un simple separador lineal y pasa a aprender transformaciones intermedias del espacio.

### 15.3 Perceptrón multicapa y composición de funciones

En un perceptrón multicapa, cada nivel produce una representación que alimenta al siguiente:

\[
h^{(1)} = \phi(W^{(1)}x+b^{(1)}), \qquad
h^{(2)} = \phi(W^{(2)}h^{(1)}+b^{(2)}), \qquad \ldots
\]

hasta llegar a una salida final \(\hat y\).

La intuición importante es que la red no solo ajusta una función de entrada a salida. Aprende una secuencia de representaciones cada vez más adecuadas para la tarea. En visión, eso puede significar pasar de bordes a formas. En lenguaje, de tokens a relaciones contextuales. El poder de las redes no proviene solo de tener muchos parámetros, sino de organizar esos parámetros en una composición rica de transformaciones.

### 15.4 Función de pérdida y descenso por gradiente

Como en otros modelos, entrenar una red significa minimizar una pérdida \(\mathcal{L}(\theta)\), donde \(\theta\) agrupa todos los parámetros. El descenso por gradiente actualiza

\[
\theta_{t+1} = \theta_t - \eta \nabla_{\theta}\mathcal{L}(\theta_t),
\]

donde \(\eta\) es la tasa de aprendizaje.

La idea general es sencilla. Si el gradiente señala la dirección de mayor aumento de la pérdida, avanzar en sentido contrario debería reducirla. La dificultad práctica está en que el paisaje de optimización de una red profunda puede ser irregular, sensible a escalas, a inicialización y a hiperparámetros. Lo esencial, sin embargo, sigue siendo comprensible: la red aprende ajustando miles o millones de parámetros para bajar una medida explícita de error.

### 15.5 Backpropagation: por qué funciona

Backpropagation no es un hechizo reservado a especialistas. Es la aplicación eficiente de la regla de la cadena para calcular cómo influye cada parámetro sobre la pérdida final. Un peso temprano afecta la salida a través de muchas capas, de modo que derivar ingenuamente sería costoso y redundante. El algoritmo reutiliza derivadas intermedias y hace viable el cálculo.

Entender eso vale más que memorizar el nombre del procedimiento. Lo que hace backpropagation es resolver un problema de eficiencia en el cálculo de gradientes dentro de una composición de funciones. Una vez visto así, la técnica deja de parecer misteriosa.

### 15.6 Optimizadores: SGD, momentum y Adam

El gradiente puede estimarse usando todo el dataset, observaciones individuales o mini-batches. En la práctica, los mini-batches ofrecen un equilibrio razonable entre estabilidad y costo. Sobre esa base aparecen variantes como momentum, que acumula dirección para amortiguar oscilaciones, y Adam, que adapta el tamaño del paso a partir de estimaciones móviles del gradiente.

No cambian el objetivo final, pero sí la dinámica con que la red lo busca. Y esa dinámica importa mucho. Un buen optimizador no reemplaza una mala representación ni una mala arquitectura, pero puede marcar la diferencia entre un entrenamiento que converge de forma útil y otro que se atasca.

### 15.7 Problemas de entrenamiento y regularización

Las redes profundas pueden sufrir gradientes que se desvanecen o explotan, sobreajuste, fuerte sensibilidad a la inicialización y dependencia de hiperparámetros. Por eso aparecen técnicas como ReLU, batch normalization, weight decay, dropout, early stopping o data augmentation.

La manera más sana de estudiarlas es recordar qué problema corrige cada una. ReLU ayuda a aliviar ciertas dificultades de gradiente. Batch normalization estabiliza distribuciones internas durante el entrenamiento. Dropout y weight decay actúan como regularizadores. Early stopping detiene el aprendizaje antes de que la red empiece a memorizar ruido. La lista deja de ser un inventario arbitrario cuando se la entiende como respuesta a obstáculos concretos.

### 15.8 SOM: mapas autoorganizados

Los Self-Organizing Maps muestran otra cara de las redes neuronales. Allí el foco no está en predecir una etiqueta, sino en proyectar datos de alta dimensión sobre una grilla preservando, en lo posible, vecindad y topología. Aunque hoy tengan menos protagonismo en aplicaciones masivas, conservan un gran valor conceptual.

Recuerdan que una red también puede servir para organizar representaciones y explorar estructura, no solo para clasificar. En ese sentido, enlazan el mundo de las redes con problemas clásicos del aprendizaje no supervisado.

### 15.9 Cuándo usar deep learning y cuándo no

El aprendizaje profundo brilla especialmente cuando hay grandes volúmenes de datos no estructurados y la posibilidad de aprender representaciones jerárquicas marca una diferencia clara, como ocurre en visión, audio o lenguaje. En cambio, en muchos problemas tabulares de tamaño moderado, ensambles de árboles pueden igualar o superar a redes profundas con menor costo y mayor interpretabilidad.

Usar deep learning por prestigio y no por necesidad es un error frecuente. La complejidad solo se justifica cuando resuelve un problema real que métodos más simples no están resolviendo bien.

## Capítulo 16. NLP aplicado: del texto crudo a una representación útil

### 16.1 Por qué el texto es un dato especial

El texto no llega como un vector numérico listo para alimentar un modelo. Llega como una secuencia simbólica cargada de contexto, ambigüedad, ironía, polisemia y variación morfológica. Antes de modelarlo hay que decidir cómo representarlo, y esa decisión suele ser más importante que la elección del clasificador final.

En cierto sentido, NLP hace especialmente visible una verdad que ya recorría toda la materia: aprender bien depende de representar bien. La diferencia es que, en texto, esa dependencia se vuelve imposible de ignorar.

### 16.2 Pipeline clásico de texto

Un pipeline clásico de procesamiento de texto incluye tokenización, normalización, filtrado de stopwords, stemming o lematización, construcción de n-gramas y vectorización. Pero no existe un preprocesamiento universalmente correcto. Cada decisión preserva algo y destruye algo.

Eliminar negaciones puede arruinar una tarea de sentimiento. Un stemming agresivo puede mezclar palabras que convendría distinguir. Mantener puntuación puede ser irrelevante en una aplicación y decisivo en otra. En texto, más que en muchos otros dominios, cada paso de preprocesamiento es una hipótesis sobre qué parte del lenguaje queremos conservar.

### 16.3 Bag of Words

En Bag of Words, cada documento se representa por conteos de términos de un vocabulario. Se pierde el orden, pero se conserva qué palabras aparecen y con qué frecuencia. La simplificación es brutal y, sin embargo, puede funcionar muy bien.

Esa aparente paradoja es instructiva. Muestra que una representación simple puede ser poderosa cuando retiene justo la parte de la estructura que importa para la tarea. No siempre hace falta capturar toda la riqueza del lenguaje para resolver un problema concreto.

### 16.4 TF-IDF

TF-IDF pondera cada término por su frecuencia en el documento y por su rareza en el corpus:

\[
\text{tfidf}(t,d)=\text{tf}(t,d)\cdot \log\left(\frac{N}{df(t)}\right).
\]

La idea intuitiva es clara. Una palabra muy frecuente en un documento, pero igualmente frecuente en todos los documentos, discrimina poco. En cambio, una palabra frecuente en un documento y rara en el corpus probablemente aporte información más específica. TF-IDF no inventa semántica profunda, pero mejora la representación al redistribuir peso hacia los términos más distintivos.

### 16.5 Naive Bayes multinomial en texto

Naive Bayes multinomial se adapta especialmente bien a representaciones de conteos o frecuencias. Aunque el supuesto de independencia entre palabras sea fuerte, suele rendir bien porque aprovecha la estructura dispersa y de alta dimensión del texto vectorizado.

Además, ofrece una intuición pedagógica atractiva. Cada término aporta evidencia a favor o en contra de una clase. Al observar cómo se acumula esa evidencia, el modelo vuelve visible una forma simple y poderosa de razonar probabilísticamente sobre documentos.

### 16.6 Lexicones y análisis de sentimiento

Los enfoques basados en lexicones asignan polaridad o intensidad a palabras y luego agregan esa información para estimar sentimiento. Funcionan bien como baseline o en dominios acotados, pero tropiezan rápido con negaciones, ironía y contexto. La frase "no estuvo nada mal" basta para mostrar el problema. El sentido no es una suma lineal de palabras con signo fijo; depende de composición, orden y situación.

Por eso los lexicones son útiles para entender el problema, pero insuficientes cuando el lenguaje empieza a comportarse como lenguaje y no como una bolsa de términos polarizados.

### 16.7 Texto para clasificación y para regresión

El texto puede usarse para clasificar documentos, pero también para problemas de regresión, como estimar complejidad de tickets, duración esperada de trámites o valoración numérica de reseñas. En esos casos vuelve a aparecer una idea central del curso: el target debe estar cuidadosamente formulado.

NLP no es un mundo separado del resto de la materia. Repite la misma arquitectura conceptual. Hay que definir el problema, representar el dato, elegir un modelo, evaluar con criterio e interpretar sin sobreactuar lo que el sistema sabe.

### 16.8 Embeddings y representaciones distribuidas

Los embeddings densos, como Word2Vec o GloVe, representan palabras en espacios continuos donde la proximidad geométrica captura parte de la similitud semántica. Los modelos contextualizados, como BERT, van más lejos: la representación de una palabra depende del contexto en el que aparece.

La historia del NLP puede leerse, en buena medida, como una historia sobre representaciones cada vez más ricas. Incluso cuando se trabaja con métodos clásicos, entender esa evolución ayuda a ver por qué el corazón del área no está solo en el clasificador, sino en el modo en que el lenguaje queda traducido al espacio donde el aprendizaje será posible.

### 16.9 Riesgos y sesgos en NLP

El lenguaje arrastra sesgos sociales, culturales e históricos. Un modelo entrenado sobre corpus sesgados puede reproducirlos o amplificarlos. Además, ironía, jerga, dominio específico y cambio de contexto pueden degradar el desempeño mucho más de lo que sugiere una métrica promedio.

Por eso el NLP serio necesita análisis de errores cualitativo, revisión de ejemplos y, cuando corresponde, evaluación segmentada. Trabajar con texto exige técnica, pero también sensibilidad interpretativa.

## Parte IX. Extensiones metodológicas y criterio profesional

Toda la materia converge, finalmente, en una pregunta más amplia que "qué modelo rinde mejor". Cómo se usa responsablemente un sistema construido con datos. Cómo se monitorea cuando el mundo cambia. Cómo se comunica lo que sí puede decirse y lo que no. Cómo se evita que una métrica momentáneamente buena oculte fragilidades conceptuales o institucionales.

## Capítulo 17. Series temporales: cuando el tiempo no puede ignorarse

### 17.1 Qué cambia cuando hay dependencia temporal

En una serie temporal las observaciones están ordenadas y suelen depender unas de otras. Eso rompe el supuesto i.i.d. que muchos métodos generales toman como punto de partida. No basta con agregar una columna de fecha y seguir igual. Cuando el tiempo importa, cambia la lógica entera del problema.

Cambian las features, porque ahora una parte importante de la señal está en los lags y en las ventanas. Cambia la validación, porque el futuro no debe contaminar el entrenamiento. Cambia la idea de generalización, porque no solo interesa funcionar fuera de muestra, sino funcionar hacia adelante en un sistema que puede derivar.

### 17.2 Tendencia, estacionalidad, ciclo y ruido

Una serie suele pensarse como combinación de tendencia, estacionalidad, ciclo y ruido. La tendencia describe movimientos de largo plazo. La estacionalidad recoge patrones periódicos regulares. Los ciclos reflejan oscilaciones más largas y menos rígidas. El ruido representa la parte no explicada.

Esta descomposición no es una taxonomía decorativa. Organiza la intuición sobre qué parte del comportamiento conviene modelar explícitamente y qué baselines tienen sentido. En series temporales, de hecho, los baselines ingenuos son mucho más competitivos de lo que muchos estudiantes imaginan. Repetir el último valor o el valor del mismo período anterior es una vara inicial bastante exigente.

### 17.3 Lags, rolling windows y features temporales

Las features temporales típicas incluyen lags, medias móviles, acumulados, diferencias e indicadores de calendario. Todas intentan condensar dependencia temporal en variables utilizables por un modelo. Pero la regla causal aquí es estricta: una feature construida en el instante \(t\) solo puede depender de información disponible hasta \(t\).

Muchas fugas temporales nacen de olvidar esa condición al construir ventanas móviles o agregados. El error es especialmente traicionero porque la variable resultante parece numéricamente correcta. Solo deja de ser legítima cuando uno recuerda cuál era el instante real de decisión.

### 17.4 Evaluación temporal

La validación temporal debe imitar el uso futuro del sistema. Los esquemas walk-forward entrenan en una ventana histórica y validan en un bloque posterior, repitiendo el procedimiento con ventanas crecientes o deslizantes. Esa lógica respeta la flecha del tiempo y deja ver si el modelo se sostiene a medida que cambian las condiciones.

En problemas temporales no importa solo la precisión promedio. Importa la estabilidad a lo largo del tiempo. Un modelo puede lucir muy bien en un tramo y degradarse luego por deriva, estacionalidades nuevas o cambios de contexto. Medir solo un promedio puede esconder esa fragilidad.

## Capítulo 18. Interpretabilidad, MLOps, sesgo y gobernanza

### 18.1 Interpretabilidad intrínseca y explicabilidad post hoc

Algunos modelos son más transparentes por construcción, como la regresión lineal o los árboles pequeños. Otros requieren técnicas post hoc para resumir su comportamiento. Esa distinción importa mucho porque una explicación post hoc no convierte al modelo en simple. Solo ofrece una ventana útil, parcial y muchas veces dependiente de supuestos.

Confundir explicación con comprensión total es una fuente frecuente de exceso de confianza. Un gráfico elegante no anula la complejidad del sistema que intenta resumir.

### 18.2 Importancia global y explicación local

La importancia global intenta responder qué variables influyen más en el comportamiento general del modelo. Las explicaciones locales intentan entender por qué un caso particular recibió cierta predicción. Herramientas como permutation importance, PDP, LIME o SHAP pueden ser muy valiosas si se interpretan con cuidado.

Pero ninguna de ellas transforma una asociación observada en una afirmación causal. La frontera entre explicar el comportamiento del modelo y explicar el mundo debe permanecer clara. Cuando esa frontera se desdibuja, la interpretabilidad deja de ser una ayuda y pasa a ser una forma sofisticada de confusión.

### 18.3 Comunicar resultados con honestidad técnica

Un resultado técnicamente correcto y mal comunicado puede producir peores decisiones que un modelo más simple y mejor explicado. Comunicar bien no es vaciar de contenido técnico la discusión, sino poner en palabras comprensibles lo que de verdad sabemos y lo que sigue siendo incierto.

Decir que un modelo tiene AUC 0.89 casi nunca alcanza. Hace falta aclarar sobre qué datos se evaluó, con qué estabilidad, con qué desbalance, con qué calibración, con qué desempeño por subgrupos y con qué costo operativo del error. La honestidad técnica consiste justamente en no presentar una cifra como si resumiera todo lo importante.

### 18.4 Reproducibilidad y ciclo de vida del modelo

Una solución profesional no termina en un notebook. Debe poder reproducirse, auditarse, mantenerse y, si hace falta, retirarse. Eso exige versionar código y datos, registrar hiperparámetros, preservar artefactos y documentar el proceso. Sin reproducibilidad, comparar experimentos o investigar fallas se vuelve innecesariamente difícil.

Además, una vez desplegado, el modelo entra en un ciclo de vida. Hay que monitorearlo, reentrenarlo cuando corresponde, revisar su degradación y administrar su reemplazo. La parte operativa no es un apéndice de la teoría. Es el lugar donde la teoría demuestra si realmente estaba pensada para el mundo.

### 18.5 Drift: cuando el mundo cambia

Después del despliegue, el mundo sigue moviéndose. Puede cambiar la distribución de las features, lo que suele llamarse data drift, o puede cambiar la relación entre features y target, lo que se conoce como concept drift. En ambos casos, un modelo entrenado sobre el pasado puede degradarse aunque su implementación no tenga ningún error.

Por eso el monitoreo no es opcional. Hay que mirar entradas, desempeño, calibración y comportamiento por segmentos. Un modelo útil no es solo el que funcionó bien el día de la validación, sino el que puede ser supervisado mientras las condiciones del mundo cambian.

### 18.6 Fuentes de sesgo algorítmico

El sesgo puede entrar por la muestra, por el etiquetado, por la definición del target, por variables proxy, por la historia institucional o por el uso del modelo fuera del dominio para el que fue pensado. Una métrica promedio aceptable puede ocultar daños sistemáticos sobre subgrupos específicos.

Por eso evaluar de manera responsable exige mirar cortes segmentados y consecuencias reales del error. La equidad no se reduce a un único número. Es una tensión entre criterios técnicos, legales, normativos y operativos que debe discutirse de frente.

### 18.7 Gobernanza mínima responsable

La gobernanza de modelos incluye documentación, trazabilidad, criterios de aprobación, monitoreo, auditoría y, cuando corresponde, revisión humana. No es un suplemento moral separado de la técnica. Es la forma institucional de asegurar que el sistema se use dentro de sus condiciones de validez y que sus límites no queden ocultos detrás de la automatización.

Cuanto más importante es la decisión asistida por el modelo, más necesaria se vuelve esta capa de control. Allí se juega buena parte de la diferencia entre una solución técnicamente interesante y una solución profesionalmente responsable.

## Capítulo 19. Ejemplo integrador y estrategia de estudio para dominio experto

### 19.1 Un ejemplo completo: predicción de morosidad

Consideremos un problema integral de riesgo: estimar si una solicitud aprobada incurrirá en mora mayor a noventa días dentro de los próximos doce meses. Si la formulación se escribe con rigor, diría algo así: para cada solicitud aprobada al momento de originación, estimar la probabilidad de mora mayor a noventa días durante los siguientes doce meses usando exclusivamente información disponible al momento del otorgamiento.

Con esa sola frase se ordena toda la materia. La unidad de análisis es la solicitud. El target queda definido operativamente. El horizonte temporal es explícito. La restricción de información también. A partir de ahí se puede revisar calidad de datos, detectar faltantes e inconsistencias, cuidar leakage, construir features financieras plausibles, elegir una validación coherente con el tiempo y comparar, por ejemplo, una logística regularizada con un Random Forest o un XGBoost. La discusión ya no se limita al score. Incluye estabilidad, interpretabilidad, costo del error y condiciones de uso.

Lo valioso del ejemplo no es el dominio financiero en sí mismo. Es ver cómo encajan en una sola arquitectura todas las piezas dispersas del curso. Cuando el ejemplo se entiende así, deja de ser un caso aislado y pasa a ser una plantilla mental para atacar problemas nuevos.

### 19.2 Qué debería poder hacer un estudiante al terminar

Un estudiante sólido debería poder traducir una necesidad real a un problema modelable, definir con precisión la unidad de análisis y el target, detectar posibles fugas de información, justificar un preprocesamiento según la naturaleza del dato y del modelo, elegir métricas coherentes con el costo del error e interpretar resultados sin exagerar lo que el sistema realmente sabe.

También debería poder distinguir con claridad entre predicción y causalidad, comparar técnicas por sus supuestos y límites, y explicar con sus propias palabras por qué una decisión metodológica tiene sentido. Una formación genuina no se reconoce por la cantidad de herramientas recordadas, sino por la capacidad de argumentar con ellas.

### 19.3 Cómo suelen evaluar los exámenes

Los exámenes serios suelen castigar menos la falta de memoria puntual que la falta de estructura conceptual. No alcanza con definir entropía, F1 o PCA. Lo que normalmente se pide es poder reconstruir qué problema resuelven, por qué aparecen, cómo se interpretan y qué errores conceptuales comete alguien que los usa de manera ingenua.

Por eso conviene estudiar buscando conexiones. Por qué este problema se formula como clasificación y no como regresión. Por qué accuracy engaña cuando las clases están desbalanceadas. Por qué un árbol profundo puede sobreajustar. Qué preserva PCA y por qué no es equivalente a t-SNE. Cómo se interpreta un coeficiente logístico. Qué cambia cuando el tiempo importa. Esas son las preguntas que revelan si hubo comprensión o solo acumulación de definiciones.

### 19.4 Errores que más suelen costar

Los errores más caros, tanto académica como profesionalmente, suelen repetirse. Elegir algoritmo antes de formular el problema. Usar métricas por costumbre. Preprocesar antes del split. Confundir correlación con causalidad. Tratar clusters como si fueran clases naturales descubiertas en el mundo. Suponer que más complejidad equivale automáticamente a mejor generalización. Reportar una sola métrica sin analizar estabilidad ni subgrupos.

Todos esos errores comparten una raíz. Nacen de perder de vista la arquitectura completa del razonamiento y quedarse con una técnica aislada. La ciencia de datos empieza a madurar cuando uno deja de ver herramientas sueltas y empieza a ver decisiones encadenadas.

### 19.5 Cómo estudiar esta materia de verdad

Estudiar bien esta disciplina exige moverse en varias capas al mismo tiempo. Hay que poder explicar un concepto con palabras simples y precisas, escribir sus expresiones esenciales cuando hace falta y, sobre todo, justificar en qué contextos conviene usarlo y en cuáles no. La intuición sin formalización se vuelve difusa. La formalización sin intuición se vuelve frágil. El criterio de uso es lo que las conecta.

Una práctica especialmente fecunda consiste en tomar cada tema y reconstruirlo desde el origen. Qué problema hace necesaria esta idea. Qué intuición la vuelve razonable. Qué expresión formal la fija con precisión. Qué ejemplo la aclara. Con qué conceptos se conecta y dónde falla una comprensión ingenua. Cuando uno puede hacer eso sin depender de un guion externo, deja de memorizar y empieza a dominar.

## Cierre

La ciencia de datos se vuelve realmente comprensible cuando deja de verse como una colección de técnicas y pasa a verse como una arquitectura intelectual. Primero se formula bien el problema. Luego se examina críticamente el dato. Después se construye una representación adecuada, se elige un modelo compatible con esa representación, se evalúa con honestidad, se interpreta sin exageraciones y se decide con responsabilidad.

Cada bloque de la materia ocupa un lugar preciso en esa arquitectura. Los fundamentos enseñan a no confundir fenómeno con dato. El análisis de calidad y el EDA enseñan a leer evidencia imperfecta. El preprocesamiento construye el espacio en el que el aprendizaje será posible o fracasará. La validación protege contra conclusiones ilusorias. Los modelos supervisados muestran distintas maneras de imponer estructura. El aprendizaje no supervisado y la reducción de dimensión obligan a pensar qué clase de orden queremos preservar. Las redes y el NLP recuerdan que, muchas veces, representar bien es más importante que seguir modas. La interpretabilidad, el monitoreo y la gobernanza muestran que un modelo útil no termina en una métrica.

Si al terminar este tratado el lector puede enfrentarse a un problema nuevo y preguntarse, con claridad, qué quiere entender o anticipar, con qué datos, bajo qué supuestos, con qué riesgo de error, con qué validación y con qué límites, entonces la materia habrá quedado comprendida desde la base. No habrá memorizado una secuencia de herramientas. Habrá aprendido a pensar con datos.
