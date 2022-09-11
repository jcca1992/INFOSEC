## EVALUACIONES DE SEGURIDAD
___
Cada organización debe realizar diferentes tipos de `evaluaciones de seguridad` en sus `redes`, `computadoras` y `aplicaciones` al menos cada cierto tiempo. El objetivo principal de la mayoría de los tipos de evaluaciones de seguridad es encontrar y confirmar la presencia de vulnerabilidades, de modo que podamos trabajar para `parchearlas`, `mitigarlas` o `eliminarlas`. Existen diferentes formas y metodologías para probar qué tan seguro es un sistema informático. Algunos tipos de evaluaciones de seguridad son más apropiados para ciertas redes que para otras. Pero todos tienen un propósito en la mejora de la ciberseguridad. Todas las organizaciones tienen diferentes requisitos de cumplimiento y tolerancia al riesgo, enfrentan diferentes amenazas y tienen diferentes modelos comerciales que determinan los tipos de sistemas que ejecutan externa e internamente. Algunas organizaciones tienen una postura de seguridad mucho más madura que sus pares y pueden enfocarse en simulaciones avanzadas de `read team` realizadas por terceros, mientras que otras aún están trabajando para establecer una seguridad básica. Independientemente, todas las organizaciones deben mantenerse al tanto de las vulnerabilidades heredadas y recientes y tener un sistema para detectar y mitigar los riesgos para sus sistemas y datos.
___
### EVALUACIONES DE VULNERABILIDAD

Las `evaluaciones de vulnerabilidad` son apropiadas para todas las organizaciones y redes. Una evaluación de vulnerabilidad se basa en un estándar de seguridad particular y se analiza el cumplimiento de estos estándares (por ejemplo, revisando una checklist).

Una evaluación de vulnerabilidad puede basarse en varios estándares de seguridad. Los estándares que se aplican a una red en particular dependerán de muchos factores. Estos factores pueden incluir regulaciones de seguridad de datos regionales y específicas de la industria, el tamaño y la forma de la red de una empresa, qué tipos de aplicaciones usan o desarrollan y su nivel de madurez de seguridad.

Las evaluaciones de vulnerabilidad se pueden realizar de forma independiente o junto con otras evaluaciones de seguridad, según la situación de una organización.
___
### PRUEBA DE PENETRACION

Se llaman pruebas de penetración porque los evaluadores las realizan para determinar si pueden penetrar en una red y cómo. Un pentest es un tipo de ataque cibernético simulado, y los Pentesters realizan acciones que un actor de amenazas puede realizar para ver si ciertos tipos de vulnerabilidades son posibles. La diferencia clave entre un pentest y un ciberataque real es que el primero se realiza con el pleno consentimiento legal de la entidad que se somete al pentesting. Ya sea que un pentester sea un empleado o un contratista externo, deberá firmar un extenso documento legal con la empresa que describa lo que se le permite hacer y lo que no se le permite hacer.

Al igual que con una evaluación de vulnerabilidades, un pentest eficaz dará como resultado un informe detallado lleno de información que se puede utilizar para mejorar la seguridad de una red. Se pueden realizar todo tipo de pentests de acuerdo con las necesidades específicas de una organización.

El pentesting de `black box` se realiza sin conocimiento de la configuración o las aplicaciones de una red. Por lo general, a un evaluador se le otorgará acceso a la red (o un puerto Ethernet y tendrá que omitir el control de acceso a la red NAC)) y nada más (lo que les obligará a realizar su propio descubrimiento de direcciones IP) si el pentest es interno o nada más que la empresa. nombre si el pentest es desde un punto de vista externo. Este tipo de pentesting suele ser realizado por terceros desde la perspectiva de un `atacante externo`. A menudo, el cliente le pedirá al pentester que le muestre las direcciones IP internas/externas/los rangos de red descubiertos para que pueda confirmar la propiedad y anotar cualquier host que deba considerarse fuera del alcance.

El pentesting `Grey Box` se realiza con un poco de conocimiento de la red que se está probando, desde una perspectiva equivalente a la de un empleado que no trabaja en el departamento de TI, como un `recepcionista` o un `agente de servicio al cliente`. El cliente generalmente le dará al evaluador rangos de red dentro del alcance o direcciones IP individuales en una situación de Grey Box.

Las pruebas de penetración `White Box` generalmente se llevan a cabo otorgando al petester acceso completo a todos los sistemas, configuraciones, documentos de compilación, etc., y el código fuente si las aplicaciones web están dentro del alcance. El objetivo aquí es descubrir tantos defectos como sea posible que serían difíciles o imposibles de descubrir a ciegas en un tiempo razonable.

A menudo, los pentesters se especializan en un área en particular. Los pentesters deben tener conocimiento de muchas tecnologías diferentes, pero aún así suelen tener una especialidad.

Los pentesters de aplicaciones evalúan aplicaciones web, aplicaciones de cliente pesado ( thick-client applications), API y aplicaciones móviles. A menudo, estarán bien versados ​​en la revisión del código fuente y podrán evaluar una aplicación web determinada desde el punto de vista de una black box o white box (normalmente, una revisión segura del código).

Los pentesters de red o infraestructura evalúan todos los aspectos de una red informática, incluidos sus dispositivos de red, como enrutadores y cortafuegos, estaciones de trabajo, servidores y aplicaciones. Estos tipos de evaluadores de penetración generalmente deben tener una sólida comprensión de las redes, Windows, Linux, Active Directory y al menos un lenguaje de secuencias de comandos. Los escáneres de vulnerabilidades de red, como Nessus, se pueden usar junto con otras herramientas durante el petesting de red, pero el escaneo de vulnerabilidades de red es solo una parte de un pentest adecuado. Es importante tener en cuenta que existen diferentes tipos de pentests (evasivos, no evasivos, evasivos híbridos). Un escáner como Nessus solo se usaría durante un pentest no evasivo cuyo objetivo es encontrar la mayor cantidad posible de fallas en la red. Además, el escaneo de vulnerabilidades solo sería una pequeña parte de este tipo de prueba de penetración. Los escáneres de vulnerabilidad son útiles pero limitados y no pueden reemplazar el toque humano y otras herramientas y técnicas.

Los pentesters `físicos` intentan aprovechar las debilidades de la seguridad física y las fallas en los procesos para obtener acceso a una instalación como un centro de datos o un edificio de oficinas.

+ ¿Puedes abrir una puerta de forma no intencionada?
+ ¿Puedes seguir a alguien al centro de datos?
+ ¿Puedes arrastrarte por un conducto de ventilación?

Los pentesters de `ingeniería social` prueban a los seres humanos.

+ ¿Se puede engañar a los empleados con phishing, vishing (phishing por teléfono) u otras estafas?
+ ¿Puede un pentester de ingeniería social acercarse a una recepcionista y decirle: "sí, trabajo aquí"?

Pentesting es más apropiado para organizaciones con un nivel de madurez de seguridad medio o alto. La madurez de la seguridad mide qué tan bien desarrollado está el programa de ciberseguridad de una empresa, y la madurez de la seguridad tarda años en desarrollarse. Implica contratar profesionales de seguridad cibernética con conocimientos, tener políticas de seguridad bien diseñadas y aplicación (como configuración, parches y gestión de vulnerabilidades), estándares de refuerzo básicos para todos los tipos de dispositivos en la red, cumplimiento normativo sólido, `planes de respuesta a incidentes cibernéticos` bien ejecutados, un `CSIRT` (`equipo de respuesta a incidentes de seguridad informática`) experimentado, un proceso de control de cambios establecido, un `CISO` (`director de seguridad de la información`), un `CTO` (`director técnico`), pruebas de seguridad frecuentes realizadas a lo largo de los años y una sólida cultura de seguridad. La cultura de seguridad tiene que ver con la actitud y los hábitos que tienen los empleados hacia la ciberseguridad. Parte de esto se puede enseñar a través de programas de capacitación en concientización sobre seguridad y parte al incorporar la seguridad en la cultura de la empresa. Todos, desde las secretarias hasta los administradores de sistemas y el personal de nivel C, deben ser conscientes de la seguridad, comprender cómo evitar prácticas riesgosas y aprender a reconocer actividades sospechosas que deben informarse al personal de seguridad.

Es posible que las organizaciones con un nivel de madurez de seguridad más bajo deseen centrarse en las evaluaciones de vulnerabilidades porque una prueba de penetración podría encontrar demasiadas vulnerabilidades para ser útil y podría abrumar al personal encargado de la reparación. Antes de considerar las pruebas de penetración, debe haber un registro de seguimiento de las evaluaciones de vulnerabilidad y las acciones tomadas en respuesta a las evaluaciones de vulnerabilidad.
___
### EVALUACIONES DE VULNERABILIDAD VS PENTESTS

`Las evaluaciones de vulnerabilidad` y las pruebas de penetración son dos evaluaciones completamente diferentes. Las evaluaciones de vulnerabilidades buscan vulnerabilidades en las redes sin simular ataques cibernéticos. Todas las empresas deberían realizar evaluaciones de vulnerabilidad cada cierto tiempo. Se podría utilizar una amplia variedad de estándares de seguridad para una evaluación de vulnerabilidad, como el cumplimiento de `GDPR` o los estándares de seguridad de aplicaciones web `OWASP`. Una evaluación de vulnerabilidad pasa por una lista de verificación.
+ ¿Cumplimos con este estándar?
+ ¿Tenemos esta configuración?

Durante una evaluación de vulnerabilidades, el evaluador generalmente ejecutará un análisis de vulnerabilidades y luego realizará una validación de las vulnerabilidades críticas, de riesgo alto y medio. Esto significa que mostrarán evidencia de que la vulnerabilidad existe y no es un falso positivo, muchas veces utilizando otras herramientas, pero no buscarán realizar escalada de privilegios, movimiento lateral, post-explotación, etc., si validan, por ejemplo, un Vulnerabilidad de ejecución remota de código.

`Las pruebas de penetración`, según su tipo, evalúan la seguridad de diferentes activos y el impacto de los problemas presentes en el entorno. Las pruebas de penetración pueden incluir tácticas manuales y automatizadas para evaluar la postura de seguridad de una organización. A menudo, también dan una mejor idea de qué tan seguros son los activos de una empresa desde una perspectiva de prueba. `Un pentest` es un ataque cibernético simulado para ver si se puede penetrar la red y cómo. Independientemente del tamaño, la industria o el diseño de la red de una empresa, las pruebas de penetración solo deben realizarse después de que algunas evaluaciones de vulnerabilidad se hayan realizado con éxito y con correcciones. Una empresa puede realizar evaluaciones de vulnerabilidad y pentests en el mismo año. Pueden complementarse entre sí. Pero son tipos muy diferentes de pruebas de seguridad que se usan en diferentes situaciones, y una no es "mejor" que la otra.

**Pentest vs Evaluacion de vulnerabilidad**

Pentest
+ `+`Proporciona un análisis en profundidad o vulnerabilidades una seguridad organizacional general.
+ `+`Proporciona recomendaciones lógicas y realistas adaptadas a la organización objetivo.
+ `-`Cuesta mucho más tiempo y dinero.
+ `-`Requieren un conocimiento de seguridad más profundo.

Evaluacion de vulnerabilidad
+ `+`Método rentable para identificar vulnerabilidades de bajo perfil.
+ `+`Las habilidades necesarias para realizar la evaluación son bajas.
+ `-`Es posible que no identifique vulnerabilidades que requieran inspección manual.
+ `-`Posibles falsos positivos.
+ `-`Recomendaciones genéricas que pueden no ser relevantes. 

![](https://academy.hackthebox.com/storage/modules/108/graphics/VulnerabilityAssessment_Diagram_02.png)

Una organización puede beneficiarse más de una `evaluación de vulnerabilidades` que de una prueba de penetración si desea recibir una vista mensual o trimestral de los problemas comúnmente conocidos de un proveedor externo. Sin embargo, una organización se beneficiaría más de una `prueba de penetración` si está buscando un enfoque que utilice técnicas manuales y automatizadas para identificar problemas fuera de lo que identificaría un escáner de vulnerabilidades durante una evaluación de vulnerabilidades. Una prueba de penetración también podría ilustrar una cadena de ataque de la vida real que un atacante podría utilizar para acceder al entorno de una organización. Las personas que realizan pruebas de penetración tienen experiencia especializada en pruebas de redes, pruebas inalámbricas, ingeniería social, aplicaciones web y otras áreas.

Para las organizaciones que reciben evaluaciones de pruebas de penetración de forma anual o semestral, sigue siendo fundamental que evalúen periódicamente su entorno con análisis de vulnerabilidades internas para identificar nuevas vulnerabilidades a medida que los proveedores las publican.
___
### OTROS TIPOS DE EVALUACIONES DE SEGURIDAD

Las evaluaciones de vulnerabilidad y las pruebas de penetración no son los únicos tipos de evaluaciones de seguridad que una organización puede realizar para proteger sus activos. También pueden ser necesarios otros tipos de evaluaciones, dependiendo del tipo de organización.
___
### AUDITORIAS DE SEGURIDAD

Las evaluaciones de vulnerabilidad se realizan porque una organización elige realizarlas y puede controlar cómo y cuándo se evalúan. Las auditorías de seguridad son diferentes. `Las auditorías de seguridad` suelen ser requisitos externos a la organización y, por lo general, las exigen `agencias gubernamentales` o `asociaciones industriales` para garantizar que una organización cumpla con las normas de seguridad específicas.

Por ejemplo, todos los minoristas, restaurantes y proveedores de servicios en línea y fuera de línea que aceptan las principales tarjetas de crédito (Visa, MasterCard, AMEX, etc.) deben cumplir con el ["Estándar de seguridad de datos de la industria de tarjetas de pago" PCI-DSS](https://www.pcicomplianceguide.org/faq/#1). PCI DSS es una regulación aplicada por el [Consejo de Estándares de Seguridad de la Industria de Tarjetas de Pago](https://www.pcisecuritystandards.org/), una organización dirigida por compañías de tarjetas de crédito y entidades de la industria de servicios financieros. Una empresa que acepta pagos con tarjeta de crédito y débito puede ser auditada para verificar el cumplimiento de PCI DSS, y el incumplimiento podría resultar en multas y ya no se le permitiría aceptar esos métodos de pago.

Independientemente de las reglamentaciones para las que se pueda auditar una organización, es su responsabilidad realizar evaluaciones de vulnerabilidad para asegurarse de que cumplen antes de estar sujetas a una auditoría de seguridad sorpresa.
___
### BUG BOUNTIES

Los `programas bug bounties` son implementados por todo tipo de organizaciones. Invitan a miembros del público en general, con algunas restricciones (generalmente sin escaneo automático), a encontrar vulnerabilidades de seguridad en sus aplicaciones. A los cazarrecompensas se les puede pagar desde unos pocos cientos de dólares hasta cientos de miles de dólares por sus hallazgos, que es un pequeño precio a pagar por una empresa para evitar que una vulnerabilidad crítica de ejecución remota de código caiga en manos equivocadas.

Las empresas más grandes con grandes bases de clientes y una gran madurez en seguridad son apropiadas para los programas bug bounties . Necesitan tener un equipo dedicado a clasificar y analizar los informes de errores y estar en una situación en la que puedan tolerar a los extraños que buscan vulnerabilidades en sus productos.

Empresas como Microsoft y Apple son ideales para tener programas bug bounties debido a sus millones de clientes y su sólida madurez de seguridad.
___
### EVALUACION DEL EQUIPO ROJO

Las empresas con presupuestos más grandes y más recursos pueden contratar sus propios equipos rojos dedicados o utilizar los servicios de firmas consultoras de terceros para realizar evaluaciones de equipos rojos. Un equipo rojo está formado por profesionales de seguridad ofensiva que tienen una experiencia considerable en pruebas de penetración. Un equipo rojo juega un papel vital en la postura de seguridad de una organización.

Un equipo rojo es un tipo de pentesting evasivo de black box, que simula todo tipo de ciberataques desde la perspectiva de un actor de amenazas externo. Estas evaluaciones suelen tener un objetivo final (es decir, llegar a un servidor o base de datos críticos, etc.). Los evaluadores solo informan las vulnerabilidades que llevaron a la finalización del objetivo, no tantas vulnerabilidades como con una prueba de penetración.

Si una empresa tiene su propio equipo rojo interno, su trabajo es realizar pruebas de penetración más específicas con un conocimiento interno de su red. Un equipo rojo debe participar constantemente en `campañas` de equipos rojos. Las campañas podrían basarse en nuevos exploits cibernéticos descubiertos a través de las `acciones de grupos de amenazas persistentes avanzadas` (`APT`), por ejemplo. Otras campañas podrían apuntar a tipos específicos de vulnerabilidades para explorarlas en gran detalle una vez que una organización haya sido consciente de ellas.

Idealmente, si una empresa puede permitírselo y ha estado desarrollando su madurez de seguridad, debería realizar evaluaciones periódicas de vulnerabilidad por su cuenta, contratar a terceros para realizar pruebas de penetración o evaluaciones de equipo rojo y, si corresponde, crear un equipo rojo interno para realizar pruebas de penetración de gray box y white box con parámetros y alcances más específicos.
___
### EVALUACION DE EQUIPO PURPURA

Un equipo azul está formado por especialistas en seguridad defensiva. Suelen ser personas que trabajan en un `SOC` (`centro de operaciones de seguridad`) o un `CSIRT` (`equipo de respuesta a incidentes de seguridad informática`). A menudo, también tienen experiencia con análisis forense digital. Entonces, si los equipos azules son defensivos y los equipos rojos son ofensivos, el rojo mezclado con azul es morado.

`¿Qué es un equipo morado?`

Los `equipos morados` se forman cuando los especialistas en seguridad `ofensiva` y `defensiva` trabajan juntos con un objetivo común: mejorar la seguridad de su red. Los equipos rojos encuentran problemas de seguridad y los equipos azules aprenden sobre esos problemas de sus equipos rojos y trabajan para solucionarlos. Una evaluación del equipo morado es como una evaluación del equipo rojo, pero el equipo azul también está involucrado en cada paso. El equipo azul puede incluso desempeñar un papel en el diseño de campañas. "Necesitamos mejorar nuestro cumplimiento de PCI DSS. Así que veamos al equipo rojo probar nuestros sistemas de punto de venta y brindar retroalimentacion y comentarios activos durante su trabajo".
___
### CONTINUEMOS

Ahora que hemos repasado los tipos de evaluación clave a los que puede someterse una organización, analicemos las evaluaciones de vulnerabilidad más en profundidad para comprender mejor los términos clave y una metodología de muestra.
___
#### [Siguiente (Evaluacion de Vulnerabilidad)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Work-File-Directories.md)