## ESTANDARES DE EVALUACION
___
Tanto las pruebas de penetración como las evaluaciones de vulnerabilidad deben cumplir con estándares específicos para ser acreditados y aceptados por los gobiernos y las autoridades legales. Dichos estándares ayudan a garantizar que la evaluación se lleve a cabo de manera exhaustiva y generalmente acordada para aumentar la eficiencia de estas evaluaciones y reducir la probabilidad de un ataque a la organización.
___
### ESTANDARES DE CUMPLIMIENTO

Cada organismo de cumplimiento normativo tiene sus propios estándares de seguridad de la información que las organizaciones deben cumplir para mantener su acreditación. Los grandes actores del cumplimiento en la seguridad de la información son `PCI`, `HIPAA`, `FISMA` e `ISO 27001`.

Estas acreditaciones son necesarias porque certifica que una organización ha hecho que un proveedor externo evalúe su entorno. Las organizaciones también confían en estas acreditaciones para las operaciones comerciales, ya que algunas empresas no harán negocios sin las acreditaciones específicas de las organizaciones.
___
### ESTANDAR DE SEGURIDAD DE DATOS DE LA INDUSTRIA DE TARJETAS DE PAGO (PCI DSS)

El estándar de seguridad de datos de la industria de tarjetas de pago (PCI DSS) es un estándar comúnmente conocido en seguridad de la información que implementa requisitos para organizaciones que manejan tarjetas de crédito. De acuerdo con las regulaciones gubernamentales, las organizaciones que almacenan, procesan o transmiten datos de titulares de tarjetas deben implementar las pautas de PCI DSS. Esto incluiría bancos o tiendas en línea que manejan sus propias soluciones de pago (por ejemplo, Amazon).

Los requisitos de PCI DSS incluyen el escaneo interno y externo de los activos. Por ejemplo, cualquier dato de tarjeta de crédito que se procese o transmita debe realizarse en un Entorno de datos del titular de la tarjeta (CDE). El entorno de CDE debe estar adecuadamente segmentado de los activos normales. Los entornos CDE se separan del entorno habitual de una organización para proteger los datos de los titulares de tarjetas y evitar que se vean comprometidos durante un ataque y limitar el acceso interno a los datos.

![](https://academy.hackthebox.com/storage/modules/108/graphics/PCI-DSS-Goals.png)

1. Construir y mantener una red segura
2. Protejer datos del titular de la tarjeta
3. Mantener un programa de gestion de vulnerabilidades
4. Implementar una medida fuerte de control de acceso
5. Probar y monitorear la red regularmente
6. Mantener una politica de seguridad de la informacion
___
### LEY DE PORTABILIDAD Y RESPONSABILIDAD DEL SEGURO MEDICO (HIPAA)

HIPAA es la [Ley de Portabilidad y Responsabilidad de los Seguros Médicos](https://www.hipaa.com/), que se utiliza para proteger los datos de los pacientes. HIPAA no requiere necesariamente análisis o evaluaciones de vulnerabilidades; sin embargo, se requiere una evaluación de riesgos y una identificación de vulnerabilidad para mantener la acreditación de HIPAA.

___
### LEY FEDERAL DE GESTION DE LA SEGURIDAD DE LA INFORMACION (FISMA)

La [Ley Federal de Gestión de la Seguridad de la Información (FISMA)](https://www.cisa.gov/federal-information-security-modernization-act) es un conjunto de normas y directrices que se utilizan para salvaguardar las operaciones y la información del gobierno. La ley requiere que una organización proporcione documentación y prueba de un programa de gestión de vulnerabilidades para mantener la disponibilidad, confidencialidad e integridad adecuadas de los sistemas de tecnología de la información.
___
### ISO 27001

ISO 27001 es un estándar utilizado en todo el mundo para gestionar la seguridad de la información. [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html) requiere que las organizaciones realicen escaneos externos e internos trimestrales.

Aunque el cumplimiento es esencial, no debe impulsar un programa de gestión de vulnerabilidades. La gestión de vulnerabilidades debe considerar la singularidad de un entorno y el apetito de riesgo asociado a una organización.

La Organización Internacional para la Estandarización (ISO) mantiene estándares técnicos para casi cualquier cosa que puedas imaginar. La norma ISO 27001 se ocupa de la seguridad de la información. El cumplimiento de la norma ISO 27001 depende del mantenimiento de un Sistema de gestión de la seguridad de la información eficaz. Para garantizar el cumplimiento, las organizaciones deben realizar pruebas de penetración cuidadosamente diseñadas.
___
### PRUEBA DE PENETRACION ESTANDAR

Las pruebas de penetración no deben realizarse sin reglas o pautas. Siempre debe haber un alcance específicamente definido para un pentest, y el propietario de una red debe tener un contrato legal firmado con los pentesters que describa lo que pueden hacer y lo que no pueden hacer. Pentesting también debe llevarse a cabo de tal manera que se cause un daño mínimo a las computadoras y redes de una empresa. Los evaluadores de penetración deben evitar realizar cambios siempre que sea posible (como cambiar la contraseña de una cuenta) y limitar la cantidad de datos eliminados de la red de un cliente. Por ejemplo, en lugar de eliminar documentos confidenciales de un archivo compartido, una captura de pantalla de los nombres de las carpetas debería ser suficiente para demostrar el riesgo.

Además del alcance y la legalidad, también existen varios estándares de pentesting, según el tipo de sistema informático que se esté evaluando. Estos son algunos de los estándares más comunes que puede usar como pentester.
___
### PTES

El [Estándar de Ejecución de Pruebas de Penetración](http://www.pentest-standard.org/index.php/Main_Page) (`PTES`) se puede aplicar a todo tipo de pruebas de penetración. Describe las fases de una prueba de penetración y cómo deben llevarse a cabo. Estas son las secciones en el PTES:

+ Interacciones previas al compromiso
+ La recogida de información
+ Modelado de amenazas
+ Análisis de vulnerabilidad
+ Explotación
+ Publicar explotación
+ Informes
___
### OSSTMM

OSSTMM es el Manual de metodología de prueba de seguridad de código abierto, otro conjunto de pautas que los pentesters pueden usar para asegurarse de que están haciendo su trabajo correctamente. Se puede utilizar junto con otros estándares de pentest.

OSSTMM se divide en cinco canales diferentes para cinco áreas diferentes de pentesting:

+ Seguridad humana (los seres humanos están sujetos a hazañas de ingeniería social)
+ Seguridad física
+ Comunicaciones inalámbricas (incluidas, entre otras, tecnologías como WiFi y Bluetooth)
+ telecomunicaciones
+ Redes de datos
___
### NIST

El NIST (Instituto Nacional de Estándares y Tecnología) es bien conocido por su [framework de Ciberseguridad NIST](https://www.nist.gov/cyberframework), un sistema para diseñar políticas y procedimientos de respuesta a incidentes. NIST también tiene un marco de prueba de penetración. Las fases del marco NIST incluyen:

+ Planificación
+ Descubrimiento
+ Ataque
+ Informes
___
### OWASP

OWASP significa [Proyecto de Seguridad de Aplicaciones Web Abiertas](https://owasp.org/). Por lo general, es la organización a la que acudir para definir los estándares de prueba y clasificar los riesgos para las aplicaciones web.

OWASP mantiene algunos estándares diferentes y guías útiles para evaluar diversas tecnologías:

+ [Guía de pruebas de seguridad web (WSTG)](https://owasp.org/www-project-web-security-testing-guide/)
+ [Guía de prueba de seguridad móvil (MSTG)](https://owasp.org/www-project-mobile-security-testing-guide/)
+ [Metodología de prueba de seguridad de firmware](https://github.com/scriptingxss/owasp-fstm)
___
#### [Anterior (Evaluacion de Vulnerabilidad)](https://github.com/jcca1992/INFOSEC/blob/main/Evaluacion%20de%20Vulnerabilidad/2-Vulnerability-Assessments.md)
#### [Siguiente (Common Vulnerability Scoring System (CVSS))](https://github.com/jcca1992/INFOSEC/blob/main/Evaluacion%20de%20Vulnerabilidad/4-CVSS.md)