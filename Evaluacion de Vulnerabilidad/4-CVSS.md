## SISTEMA COMUN DE PUNTUACION DE VULNERABILIDAD (CVSS)

Hay varias formas de calificar o calcular la gravedad de las vulnerabilidades. El Sistema Común de Puntuación de Vulnerabilidad (CVSS) es un estándar de la industria para realizar estos cálculos. Muchas herramientas de escaneo aplicarán estos puntajes a cada hallazgo como parte de los resultados del escaneo, pero es importante que comprendamos cómo se derivan estos puntajes en caso de que alguna vez necesitemos calcular uno a mano o justificar el puntaje aplicado a una vulnerabilidad determinada. 

El CVSS se utiliza a menudo junto con el llamado Microsoft DREAD. DREAD es un sistema de evaluación de riesgos desarrollado por Microsoft para ayudar a los profesionales de seguridad de TI a evaluar la gravedad de las amenazas y vulnerabilidades de seguridad. Se utiliza para realizar un análisis de riesgos mediante el uso de una escala de 10 puntos para evaluar la gravedad de las amenazas y vulnerabilidades de seguridad. Con ello calculamos el riesgo de una amenaza o vulnerabilidad en base a cinco factores principales:

+ Daño potencial
+ Reproduccion
+ Explotacion
+ Usuarios afectados
+ Descubrimiento

El modelo es esencial para la estrategia de seguridad de Microsoft y se utiliza para monitorear, evaluar y responder a amenazas y vulnerabilidades de seguridad en los productos de Microsoft. También sirve como referencia para que los profesionales y gerentes de seguridad de TI realicen su evaluación de riesgos y priorización de amenazas y vulnerabilidades de seguridad.
___
### PUNTUACION DE RIESGO

El sistema CVSS ayuda a categorizar el riesgo asociado con un problema y permite a las organizaciones priorizar los problemas según la calificación. La puntuación CVSS consiste en la explotabilidad y el impacto de un problema. Las medidas de explotabilidad consisten en vector de acceso, complejidad de acceso y autenticación. Las métricas de impacto consisten en la tríada de la CIA (CID), que incluye confidencialidad, integridad y disponibilidad.

Intserar imagen
___
### GRUPO DE METRICAS BASE

El grupo de métricas base CVSS representa las características de vulnerabilidad y consta de métricas de explotabilidad y métricas de impacto.

#### Métricas de explotabilidad
Las métricas de explotabilidad son una forma de evaluar los medios técnicos necesarios para explotar el problema utilizando las siguientes métricas:

+ Vector de ataque
+ Complejidad del ataque
+ Privilegios requeridos
+ La interacción del usuario

#### Métricas de impacto
Las métricas de impacto representan las repercusiones de explotar con éxito un problema y lo que se ve afectado en un entorno, y se basan en la tríada de la CIA. La tríada de la CIA es un acrónimo de 'Confidencialidad', 'Integridad' y 'Disponibilidad'.

insertar imagen cid

'El impacto de la confidencialidad' se relaciona con proteger la información y garantizar que solo las personas autorizadas tengan acceso. Por ejemplo, un valor de gravedad alto sería en el caso de que un atacante robe contraseñas o claves de cifrado. Un valor de gravedad bajo se relacionaría con un atacante que toma información que puede no ser un activo vital para una organización.

'El Impacto de Integridad' se relaciona con que la información no se cambie ni se altere para mantener la precisión. Por ejemplo, una gravedad alta sería si un atacante modificara archivos comerciales cruciales en el entorno de una organización. Un valor de gravedad bajo sería si un atacante no pudiera controlar específicamente la cantidad de archivos cambiados o modificados.

'El impacto en la disponibilidad' se relaciona con tener información fácilmente accesible para los requisitos comerciales. Por ejemplo, un valor alto sería si un atacante provocara que un entorno no estuviera completamente disponible para los negocios. Un valor bajo sería si un atacante no pudiera denegar por completo el acceso a los activos comerciales y los usuarios aún pudieran acceder a algunos activos de la organización.