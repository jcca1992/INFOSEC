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


___
#### [Siguiente (Evaluacion de Vulnerabilidad)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Work-File-Directories.md)