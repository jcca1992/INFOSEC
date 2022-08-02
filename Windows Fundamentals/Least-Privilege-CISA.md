## Principio de minimo privilegio

Published: September 14, 2005 | Last revised: May 10, 2013 
Author(s): Michael Gegick Sean Barnum 
Maturity Levels and Audience Indicators: L4  / D/P  L  
SDLC Life Cycles: Design  
Copyright: Copyright © Cigital, Inc. 2005-2007. Cigital retains copyrights to this material.
___
### Abstracto

Solo se deben asignar los derechos mínimos necesarios a un sujeto que solicita acceso a un recurso y deben estar vigentes durante el menor tiempo necesario (recuerde renunciar a los privilegios). Otorgar permisos a un usuario más allá del alcance de los derechos necesarios de una acción puede permitir que ese usuario obtenga o cambie información de formas no deseadas. Por lo tanto, la delegación cuidadosa de los derechos de acceso puede limitar el daño de un sistema por parte de los atacantes.
___
### Extractos de descripción detallada

Según Saltzer y Schroeder [Saltzer 75] en "Principios básicos de protección de la información", página 9:

>Mínimo privilegio: cada programa y cada usuario del sistema debe operar utilizando el conjunto mínimo de privilegios necesarios para completar el trabajo. Principalmente, este principio limita el daño que puede resultar de un accidente o error. También reduce al mínimo el número de posibles interacciones entre programas privilegiados para un funcionamiento correcto, de modo que es menos probable que se produzcan usos de privilegios involuntarios, no deseados o inadecuados. Así, si surge una duda relacionada con el mal uso de un privilegio, se minimiza el número de programas que deben ser auditados. Dicho de otra manera, si un mecanismo puede proporcionar "`firewalls`", el principio de privilegio mínimo proporciona una justificación sobre dónde instalar los firewalls. La regla de seguridad militar de "necesidad de saber" es un ejemplo de este principio.

Según Bishop [Bishop 03] en el Capítulo 13, "Principios de diseño", Sección 13.2.1, "Principio de privilegio mínimo", páginas 343-344:

>Este principio restringe cómo se otorgan los privilegios.

Definición 13-1. El Principio del Mínimo Privilegio establece que a un sujeto se le deben otorgar solo los privilegios necesarios para que complete su tarea.

Si un sujeto no necesita un derecho de acceso, el sujeto no debería tener ese derecho. Además, la función del sujeto (en oposición a su identidad) debe controlar la asignación de derechos. Si una acción específica requiere que se aumenten los derechos de acceso de un sujeto, esos derechos adicionales deben renunciarse inmediatamente después de completar la acción. Este es el análogo de la regla de "necesidad de saber": si el sujeto no necesita acceder a un objeto para realizar su tarea, no debería tener derecho a acceder a ese objeto. Más precisamente, si un sujeto necesita agregar a un objeto, pero no alterar la información ya contenida en el objeto, se le debe otorgar derechos de agregar y no derechos de escritura.

En la práctica, la mayoría de los sistemas no tienen la granularidad necesaria de privilegios y permisos para aplicar este principio con precisión. Los diseñadores de mecanismos de seguridad aplican este principio lo mejor que pueden. En dichos sistemas, las consecuencias de los problemas de seguridad suelen ser más graves que las consecuencias en los sistemas que se adhieren a este principio.

Este principio requiere que los procesos se limiten a un dominio de protección tan pequeño como sea posible.

Ejemplo 1

El sistema operativo UNIX no aplica controles de acceso a la raíz del usuario. Ese usuario puede finalizar cualquier proceso y leer, escribir o eliminar cualquier archivo. Por lo tanto, los usuarios que crean copias de seguridad también pueden eliminar archivos. La cuenta de administrador en Windows tiene los mismos poderes.

Ejemplo 2

Un servidor de correo acepta correo de Internet y copia los mensajes en un directorio de cola; un servidor local completará la entrega. Necesita derechos para acceder al puerto de red apropiado, para crear archivos en el directorio de cola y para modificar esos archivos (para que pueda copiar el mensaje en el archivo, reescribir la dirección de entrega si es necesario y agregar las líneas "Recibido" correspondientes) . Debe ceder el derecho de acceder al archivo tan pronto como haya terminado de escribir el archivo en el directorio de spool, porque no necesita acceder a ese archivo nuevamente. El servidor no debería poder acceder a los archivos de ningún usuario ni a ningún otro archivo que no sean sus propios archivos de configuración.

Según Viega y McGraw [Viega 02] en el Capítulo 5, "Principios rectores para la seguridad del software", en "Principio 4: Siga el principio de privilegio mínimo" de las páginas 100-103:

>El principio de privilegio mínimo establece que solo debe otorgarse el acceso mínimo necesario para realizar una operación, y que el acceso debe otorgarse solo durante el tiempo mínimo necesario. (Este principio fue introducido por Saltzer y Schroeder.)

Cuando otorga acceso a partes de un sistema, siempre existe el riesgo de que se abuse de los privilegios asociados con ese acceso.

Este problema comienza a ser común en las políticas de seguridad que se envían con productos destinados a ejecutarse en un entorno restringido. Algunos proveedores ofrecen aplicaciones que funcionan como applets de Java. Los applets suelen constituir código móvil, que un navegador web trata con sospecha por defecto. Dicho código se ejecuta en un espacio aislado, donde el comportamiento del subprograma está restringido en función de una política de seguridad que establece un usuario. Los proveedores rara vez practican el principio de privilegio mínimo cuando sugieren una política para usar con su código, porque hacerlo requeriría mucho esfuerzo de su parte. Es mucho más fácil simplemente enviar una política que diga "dejar que mi código haga cualquier cosa". Las personas generalmente instalarán políticas de seguridad proporcionadas por el proveedor, quizás porque confían en el proveedor, o quizás porque es demasiado complicado averiguar qué política de seguridad hace el mejor trabajo para minimizar los privilegios que se deben otorgar a la aplicación del proveedor.

La pereza a menudo va en contra del principio del mínimo privilegio. No dejes que eso suceda en tu código.

>>Ejemplo 1

    Por ejemplo, supongamos que se va de vacaciones y le da a un amigo la llave de su casa, solo para alimentar a las mascotas, recoger el correo, etc. Si bien puede confiar en un amigo, siempre existe la posibilidad de que haya una fiesta. en su casa sin su consentimiento, o que sucederá algo más que no le gusta. Ya sea que confíes o no en tu amigo, realmente no hay necesidad de ponerte en riesgo al darle más acceso del necesario. Por ejemplo, si no tiene mascotas, pero solo necesita un amigo para recoger nuestro correo de vez en cuando, debe ceder solo la llave del buzón. Si bien tu amigo puede encontrar una buena manera de abusar de ese privilegio, al menos no tienes que preocuparte por la posibilidad de un abuso adicional. Si das la llave de la casa innecesariamente, todo eso cambia.

    Del mismo modo, si consigue un cuidador de la casa mientras está de vacaciones, no es probable que deje que esa persona se quede con sus llaves cuando no esté de vacaciones. Si lo hace, se expone a un riesgo adicional. Cada vez que una llave de su casa está fuera de su control, existe el riesgo de que esa llave se duplique. Si hay una llave fuera de su control y no está en casa, existe el riesgo de que la llave se esté utilizando para entrar en su casa. Cualquier período de tiempo en el que alguien tenga su clave y no esté siendo supervisado por usted constituye una ventana de tiempo en la que es vulnerable a un ataque. Desea mantener dichas ventanas de vulnerabilidad lo más cortas posible, para minimizar sus riesgos.

    Ejemplo 2

     Otro buen ejemplo del mundo real aparece en el sistema de autorización de seguridad del gobierno de los EE. UU.; en particular con la noción de "necesidad de saber". Si tiene autorización para ver cualquier documento secreto, no puede exigir ver ningún documento secreto que sepa que existe. Si pudiera, sería muy fácil abusar del nivel de autorización de seguridad. En cambio, las personas solo pueden acceder a documentos que son relevantes para cualquier tarea que se supone que deben realizar.

     Ejemplo 3

    Algunas de las violaciones más famosas del principio de privilegio mínimo existen en los sistemas UNIX. Por ejemplo, en los sistemas UNIX, los privilegios de raíz son necesarios para vincular un programa a un número de puerto inferior a 1024. Por ejemplo, para ejecutar un servidor de correo en el puerto 25, el puerto SMTP tradicional, un programa necesita los privilegios del usuario raíz. Sin embargo, una vez que un programa se instala en el puerto 25, no existe una necesidad imperiosa de que vuelva a usar los privilegios de root. Un programa consciente de la seguridad renuncia a los privilegios de raíz tan pronto como sea posible y le hará saber al sistema operativo que nunca más debería requerir esos privilegios en esta ejecución (consulte el Capítulo 8 [en Construcción de software seguro] para una discusión sobre los privilegios). Un gran problema con muchos servidores de correo electrónico es que no renuncian a sus permisos de root una vez que toman el puerto de correo (Sendmail es un ejemplo clásico). Por lo tanto, si alguien alguna vez encuentra una manera de engañar a un servidor de correo de este tipo para que haga algo nefasto, podrá obtener la raíz. Entonces, si un atacante malintencionado encontrara un desbordamiento de pila adecuado en Sendmail (consulte el Capítulo 7 [en Creación de software seguro]), ese desbordamiento podría usarse para engañar al programa para que ejecute código arbitrario como raíz. Dado el permiso de root, cualquier cosa válida que intente el atacante tendrá éxito. El problema de ceder privilegios es especialmente grave en Java, ya que no existe una forma independiente del sistema operativo de ceder permisos.

    Ejemplo 4

     Otro escenario común involucra a un programador que puede desear acceder a algún tipo de objeto de datos, pero solo necesita leer del objeto. Digamos que el programador en realidad solicita más privilegios de los necesarios, por el motivo que sea. Los programadores hacen esto para hacer la vida más fácil. Por ejemplo, uno podría decir: "Algún día podría necesitar escribir en este objeto, y sería horrible tener que regresar y cambiar esta solicitud". Los valores predeterminados inseguros también pueden conducir a una violación aquí. Por ejemplo, hay varias llamadas en la API de Windows para acceder a objetos que otorgan todos los accesos si pasa "0" como argumento. Para obtener algo más restrictivo, necesitaría pasar un montón de banderas (O juntas). Muchos programadores simplemente se quedarán con el valor predeterminado, siempre que funcione, ya que es lo más fácil.

De acuerdo con Howard y LeBlanc [Howard 02] en el Capítulo 3, "Principios de seguridad por los que vivir", en "Use Least Privilege" de las páginas 60-61:

>Todas las aplicaciones deben ejecutarse con el mínimo de privilegios para realizar el trabajo y no más. A menudo analizo productos que deben ejecutarse en el contexto de seguridad de una cuenta administrativa, o peor aún, como un servicio que se ejecuta como la cuenta del sistema local, cuando, con un poco de reflexión, los diseñadores de productos podrían no haber requerido tales cuentas privilegiadas. La razón para ejecutar con privilegios mínimos es bastante simple. Si se encuentra una vulnerabilidad de seguridad en el código y un atacante puede inyectar código en su proceso, hacer que el código realice tareas confidenciales o ejecutar un caballo de Troya o un virus, el código malicioso se ejecutará con los mismos privilegios que el proceso comprometido. Si el proceso se ejecuta como administrador, el código malicioso se ejecuta como administrador. Es por eso que recomendamos que las personas no se ejecuten como miembros del grupo de administradores locales en sus computadoras, en caso de que se ejecute un virus o algún otro código malicioso.

Adelante, admítelo: estás conectado a tu computadora como miembro del grupo de administradores locales, ¿no es así? No lo estoy. No lo he estado en más de tres años, y todo funciona bien. código, depuro código, envío correos electrónicos, sincronizo con mi Pocket PC, creo documentación para un sitio de intranet y hago una miríada de otras cosas. Para hacer todo esto, no necesita derechos de administrador, así que ¿por qué ejecutar como ¿un administrador? (Admitiré que cuando construyo una computadora nueva me agrego al grupo de administradores, instalo todas las aplicaciones que necesito y luego me elimino rápidamente).

Cuando cree su aplicación, anote a qué recursos debe acceder y qué tareas especiales debe realizar. Los ejemplos de recursos incluyen archivos y datos de registro; ejemplos de tareas especiales incluyen la capacidad de iniciar sesión en cuentas de usuario en el sistema, depurar procesos o respaldar datos. A menudo encontrará que no necesita muchos privilegios o capacidades especiales para realizar cualquier tarea. Una vez que tenga una lista de todos sus recursos, determine qué se debe hacer con esos recursos. Por ejemplo, es posible que un usuario necesite leer y escribir en los recursos, pero no crearlos ni eliminarlos. Armado con esta información, puede determinar si el usuario necesita ejecutarse como administrador para usar su aplicación. Las posibilidades son buenas de que ella no lo haga.

Un uso común del privilegio mínimo nuevamente involucra a los bancos. La parte más valiosa de un banco es la bóveda, pero los cajeros generalmente no tienen acceso a la bóveda. De esa forma, un atacante podría amenazar a un cajero para acceder a la bóveda, pero el cajero simplemente no sabrá cómo hacerlo.

Para una mirada humorística al principio de privilegio mínimo, consulte "Si no nos presentamos como administradores, las cosas se rompen" en el Apéndice B [en Escritura de código seguro], "Excusas ridículas que hemos escuchado". Además, consulte el Capítulo 7 [en Escritura de código seguro] para obtener una descripción completa de cómo puede evitar requerir privilegios peligrosos.

Sugerencia: si su aplicación no se ejecuta a menos que el usuario (o la identidad del proceso de servicio) sea un administrador o la cuenta del sistema, determine el motivo. Hay muchas posibilidades de que los privilegios elevados sean innecesarios.

De acuerdo con NIST [NIST 01] en la Sección 3.3, "Principios de seguridad de TI", de la página 16:

>Implementar el privilegio mínimo.

    El concepto de acceso limitado, o "privilegio mínimo", consiste simplemente en no proporcionar más autorizaciones de las necesarias para realizar las funciones requeridas. Esto quizás se aplica con mayor frecuencia en la administración del sistema. Su objetivo es reducir el riesgo al limitar la cantidad de personas con acceso a los controles de seguridad críticos del sistema; es decir, controlar quién puede habilitar o deshabilitar las funciones de seguridad del sistema o cambiar los privilegios de los usuarios o programas. Las mejores prácticas sugieren que es mejor tener varios administradores con acceso limitado a los recursos de seguridad en lugar de una persona con permisos de "superusuario".

    Se debe considerar la implementación de controles de acceso basados ​​en roles para varios aspectos del uso del sistema, no solo para la administración. La política de seguridad del sistema puede identificar y definir los distintos roles de los usuarios o procesos. A cada rol se le asignan los permisos necesarios para realizar sus funciones. Cada permiso especifica un acceso permitido a un recurso en particular (como acceso de "lectura" y "escritura" a un archivo o directorio específico, acceso de "conexión" a un host y puerto determinados, etc.). A menos que se otorgue un permiso de forma explícita, el usuario o proceso no debería poder acceder al recurso protegido.

Según Schneier [Schneier 00] en "Procesos de seguridad":

>Límite de privilegio.

     No le dé a ningún usuario más privilegios de los que absolutamente necesita para hacer su trabajo. Así como no le daría a un empleado al azar las llaves de la oficina del CEO, no le dé una contraseña para los archivos del CEO.

### Que sale mal

Según McGraw y Viega [McGraw 03]

>Los pequeños problemas pueden convertirse en grandes problemas cuando ocurren en secciones privilegiadas del código (piense en el código SUID o en el código que debe ejecutarse como Administrador para que funcione). A veces se introducen mediante la instalación o la configuración, algo que es imposible de controlar para un desarrollador. Por ejemplo, los usuarios suelen instalar un servidor web y ejecutarlo en un espacio de proceso de usuario real, sin crear un "nadie" sin privilegios como destino. Considere también que los binarios SUID de Solaris se pueden ejecutar sin un conjunto de bits s, lo que presenta un riesgo de seguridad inaceptable.

     Incluso si reparte los privilegios con cuidado, renunciar a ellos no siempre es una tarea trivial. No obstante, hazlo siempre que puedas.

     
