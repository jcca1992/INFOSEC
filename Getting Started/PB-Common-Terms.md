## **TERMINOS COMUNES**

Las pruebas de penetración/hacking son un campo enorme. Nos encontraremos con innumerables tecnologías a lo largo de nuestras carreras. Estos son algunos de los términos y tecnologías más comunes con los que nos encontraremos repetidamente y que debemos dominar con firmeza. Esta no es una lista exhaustiva, pero es suficiente para comenzar con módulos fundamentales y boxes HTB fáciles. 
___

### SHELL

Shell es un término muy común que escucharemos una y otra vez durante nuestro viaje. Tiene algunos significados. En un sistema Linux, el shell es un programa que recibe información del usuario a través del teclado y pasa estos comandos al sistema operativo para realizar una función específica. En los primeros días de la informática, el shell era la única interfaz disponible para interactuar con los sistemas. Desde entonces, han surgido muchos más tipos y versiones de sistemas operativos junto con la interfaz gráfica de usuario (GUI) para complementar las interfaces de línea de comandos (shell), como la terminal de Linux, la línea de comandos de Windows (cmd.exe) y Windows PowerShell. . 

La mayoría de los sistemas Linux utilizan un programa llamado Bash (Bourne Again Shell) como programa shell para interactuar con el sistema operativo. Bash es una versión mejorada de sh, el programa shell original de los sistemas Unix. Además de bash, también hay otras shells, incluidas, entre otras, Zsh, Tcsh, Ksh, Fish shell, etc. 

A menudo leemos o escuchamos a otros hablar sobre "getting a shell" en una box (system). Esto significa que el host de destino ha sido explotado y hemos obtenido acceso a nivel de shell (generalmente bash o sh) y podemos ejecutar comandos de forma interactiva como si estuviéramos sentados conectados al host. Se puede obtener un shell explotando una aplicación web o una vulnerabilidad de red/servicio u obteniendo credenciales e iniciando sesión en el host de destino de forma remota. Hay tres tipos principales de conexiones de shell: 
~~~
Shell Type 	    Description
Reverse shell 	Inicia una conexión de regreso a un "listener" en nuestra box de ataque. 
		
Bind shell 	    "Binds" a un puerto específico en el host de destino y espera una conexión desde nuestra box de ataque. 
		
Web shell 	    Ejecuta comandos del sistema operativo a través del navegador web, normalmente no interactivo o semi-interactivo. 
                También se puede usar para ejecutar comandos únicos (es decir,aprovechar una vulnerabilidad de carga de archivos
                y cargar un script PHP para ejecutar un solo comando. 
~~~
Cada tipo de shell tiene su caso de uso, y de la misma manera que hay muchas formas de obtener un shell, el programa de ayuda que usamos para obtener un shell puede estar escrito en muchos lenguajes (Python, Perl, Go, Bash, Java, awk , PHP, etc). Estos pueden ser pequeños scripts o programas más grandes y complejos para facilitar una conexión desde el host de destino a nuestro sistema atacante y obtener acceso "shell". El acceso de Shell se discutirá en profundidad en una sección posterior.
___

### PORT (puerto)

Se puede pensar en un puerto como una ventana o puerta en una casa (la casa es un sistema remoto), si una ventana o puerta se deja abierta o no se bloquea correctamente, a menudo podemos obtener acceso no autorizado a una casa. Esto es similar en computación. Los puertos son puntos virtuales donde comienzan y terminan las conexiones de red. Están basados en software y son administrados por el sistema operativo host. Los puertos están asociados con un proceso o servicio específico y permiten que las computadoras diferencien entre diferentes tipos de tráfico (el tráfico SSH fluye a un puerto diferente al de las solicitudes web para acceder a un sitio web, aunque las solicitudes de acceso se envíen a través de la misma conexión de red). 


A cada puerto se le asigna un número y muchos están estandarizados en todos los dispositivos conectados a la red (aunque un servicio puede configurarse para ejecutarse en un puerto no estándar). Por ejemplo, los mensajes HTTP (tráfico del sitio web) generalmente van al puerto 80, mientras que los mensajes HTTPS van al puerto 443 a menos que se configure de otra manera. Encontraremos aplicaciones web que se ejecutan en puertos no estándar, pero generalmente las encontramos en los puertos 80 y 443. Los números de puerto nos permiten acceder a servicios o aplicaciones específicos que se ejecutan en dispositivos de destino. En un nivel muy alto, los puertos ayudan a las computadoras a comprender cómo manejar los distintos tipos de datos que reciben. 

Hay dos categorías de puertos, Protocolo de control de transmisión (TCP) y Protocolo de datagramas de usuario (UDP).
TCP está orientado a la conexión, lo que significa que se debe establecer una conexión entre un cliente y un servidor antes de poder enviar los datos. El servidor debe estar en estado de escucha esperando solicitudes de conexión de los clientes.
UDP utiliza un modelo de comunicación sin conexión. No hay "Handshake" y, por lo tanto, introduce cierta falta de confiabilidad ya que no hay garantía de entrega de datos. UDP es útil cuando la corrección/comprobación de errores no es necesaria o la propia aplicación la maneja. UDP es adecuado para aplicaciones que ejecutan tareas sensibles al tiempo, ya que descartar paquetes es más rápido que esperar paquetes retrasados debido a la retransmisión, como es el caso de TCP, y puede afectar significativamente un sistema en tiempo real. Hay 65535 puertos TCP y 65535 puertos UDP diferentes, cada uno indicado por un número. Algunos de los puertos TCP y UDP más conocidos se enumeran a continuación: 

| Puerto | Protocolo |
| -- | -- |
| 20/21 (TCP) | FTP |
| 22 (TCP) | SSH |
| 23 (TCP) | Telnet |
| 25 (TCP) | SMTP |
| 80 (TCP) | HTTP |
| 161 (TCP/UDP) | SNMP |
| 389 (TCP/UDP) | LDAP |
| 443 (TCP) | SSL/TLS (HTTPS) |
| 445 (TCP) | SMB |
| 3389 (TCP)| RDP |

Como profesionales de la seguridad de la información, debemos poder recordar rápidamente grandes cantidades de información sobre una amplia variedad de temas. Es esencial para nosotros, especialmente como pentesters, tener una comprensión firme de muchos puertos TCP y UDP y ser capaces de reconocerlos rápidamente solo por su número (es decir, saber que el puerto 21 es FTP, el puerto 80 es HTTP, el puerto 88 es Kerberos) sin tener que buscarlo. Esto vendrá con la práctica y la repetición y eventualmente se convertirá en una segunda naturaleza a medida que ataquemos más cajas, laboratorios y redes del mundo real nos ayudará a trabajar de manera más eficiente y priorizar mejor nuestros esfuerzos y ataques de enumeración. 

Guías como [esta](https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-sg-en-4/ch-ports.html) y [esta](https://packetlife.net/media/library/23/common-ports.pdf) son excelentes recursos para aprender puertos TCP y UDP estándar y menos comunes. Ponte a prueba para memorizar tantos de estos como sea posible e investiga un poco sobre cada uno de los protocolos enumerados en la tabla anterior. Esta es una gran [referencia](https://nullsec.us/top-1-000-tcp-and-udp-ports-nmap-default/) sobre los 1000 puertos TCP y UDP principales de nmap junto con los 100 servicios principales escaneados por nmap. 
___

### WEB SERVER

Un servidor web es una aplicación que se ejecuta en el servidor back-end, que maneja todo el tráfico HTTP desde el navegador del lado del cliente, lo enruta a las páginas de destino de las solicitudes y finalmente responde al navegador del lado del cliente. Los servidores web generalmente se ejecutan en los puertos TCP 80 o 443 y son responsables de conectar a los usuarios finales a varias partes de la aplicación web, además de manejar sus diversas respuestas

Como las aplicaciones web tienden a estar abiertas para la interacción pública y están orientadas a Internet, pueden comprometer el servidor de back-end si sufren alguna vulnerabilidad. Las aplicaciones web pueden proporcionar una amplia superficie de ataque, lo que las convierte en un objetivo de gran valor para los atacantes y los pentesters. 

Muchos tipos de vulnerabilidades pueden afectar a las aplicaciones web. A menudo oiremos o veremos referencias a [OWASP Top 10](https://owasp.org/www-project-top-ten/). Esta es una lista estandarizada de las 10 principales vulnerabilidades de aplicaciones web mantenidas por Open Web Application Security Project (OWASP). Esta lista se considera las 10 vulnerabilidades más peligrosas y no es una lista exhaustiva de todas las posibles vulnerabilidades de aplicaciones web. Las metodologías de evaluación de la seguridad de las aplicaciones web a menudo se basan en los 10 principales de OWASP como punto de partida para las principales categorías de fallas que un asesor debe verificar. La lista actual de OWASP Top 10 es: 

| Numero | Categoria | Descripcion |
| -- | -- | -- |
| 1. | Broken Access Control | Las restricciones no se implementan adecuadamente para evitar que los                       usuarios accedan a las cuentas de otros usuarios, vean datos confidenciales, accedan a funciones no autorizadas, modifiquen datos, etc. |
| 2. | Cryptographic Failures | Fallas relacionadas con la criptografía que a menudo conducen a la exposición de datos confidenciales o al compromiso del sistema. |
| 3. | Injection | Dato suministrado por usuario no es validado, filtrado u organizado por la aplicacion. Algunos ejemplos de inyecciones son inyección SQL, inyección de comando, inyección LDAP, etc. |
| 4. | Insecure Design | Estos problemas ocurren cuando la aplicación no está diseñada teniendo en cuenta la seguridad. |
| 5. | Security Misconfiguration | Falta de refuerzo de seguridad adecuado en cualquier parte de la aplication stack, configuraciones predeterminadas inseguras, almacenamiento en la nube abierto, mensajes de error detallados que revelan demasiada información. | 
| 6. | Vulnerable and Outdated Components | Usar componentes (tanto del lado del cliente como del lado del servidor) que son vulnerables, no compatibles o desactualizados. |
| 7. | Identification and Authentication Failures | Ataques relacionados con la autenticación que se dirigen a la identificacion, la autenticación y la gestión de sesiones del usuario |
| 8. | Software and Data Integrity Failures | Las fallas en la integridad del software y los datos se relacionan con el código y la infraestructura que no protegen contra las violaciones de la integridad. Un ejemplo de esto es cuando una aplicación se basa en complementos, bibliotecas o módulos de fuentes, repositorios y redes de entrega de contenido (CDN) que no son de confianza |
| 9. | Security Logging and Monitoring Failures | Esta categoría es para ayudar a detectar, escalar y responder a infracciones activas. Sin registro y monitoreo, las infracciones no se pueden detectar.|
| 10. | Server-Side Request Forgery | Las fallas de SSRF ocurren cada vez que una aplicación web obtiene un recurso remoto sin validar la URL proporcionada por el usuario. Permite a un atacante obligar a la aplicación a enviar una solicitud manipulada a un destino inesperado, incluso cuando está protegida por un firewall, VPN u otro tipo de lista de control de acceso a la red (ACL). | 


Es esencial familiarizarse con cada una de estas categorías y las diversas vulnerabilidades que se ajustan a cada una. Las vulnerabilidades de las aplicaciones web se tratarán en profundidad en módulos posteriores. Para obtener más información sobre las aplicaciones web, consulte el módulo Introducción a las aplicaciones web.