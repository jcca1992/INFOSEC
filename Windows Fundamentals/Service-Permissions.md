## PERMISOS DE SERVICIO

Recuerde que los servicios permiten la gestión de procesos de ejecución prolongada y son una parte fundamental de los sistemas operativos Windows. Los administradores de sistemas a menudo los pasan por alto como posibles vectores de amenazas que pueden usarse para cargar archivos `DLL maliciosos`, ejecutar aplicaciones sin acceso a una cuenta de administrador, escalar privilegios e incluso mantener la persistencia. Estos vectores de amenazas en los servicios de Windows a menudo surgen a través de configuraciones incorrectas de permisos de servicio implementadas por software de terceros y fáciles de cometer errores por parte de los administradores durante los procesos de instalación.

El primer paso para darse cuenta de la importancia de los permisos de servicio es simplemente comprender que existen y tenerlos en cuenta. En los sistemas operativos de servidor, los servicios de red críticos como `DHCP` y los servicios de dominio de `Active Directory` generalmente se instalan utilizando la cuenta asignada al administrador que realiza la instalación. Parte del proceso de instalación incluye la asignación de un servicio específico para que se ejecute con las credenciales y los privilegios de un usuario designado, que de forma predeterminada se establece dentro del contexto del usuario que ha iniciado sesión actualmente.

Por ejemplo, si iniciamos sesión como Bob en un servidor durante la instalación de `DHCP`, ese servicio se configurará para ejecutarse como Bob a menos que se especifique lo contrario. ¿Qué cosas malas pueden salir de esto? Bueno, ¿y si Bob deja la organización o lo despiden? La práctica comercial típica sería deshabilitar la cuenta de Bob como parte de su proceso de salida. En este caso, ¿qué pasaría con `DHCP` y otros servicios que se ejecutan con la cuenta de Bob? Esos servicios no se iniciarían. `DHCP` o Protocolo de configuración dinámica de host es responsable de arrendar direcciones IP a las computadoras en la red. Si este servicio se detiene en un servidor `DHCP` de Windows, los clientes que soliciten una dirección IP no la recibirán. Esto significa que una configuración incorrecta del servicio podría provocar tiempo de inactividad y pérdida de productividad. Se recomienda encarecidamente crear una cuenta de usuario individual para ejecutar servicios de red críticos. Estas se conocen como `cuentas de servicio`.

También debemos tener en cuenta los permisos del servicio y los permisos de los directorios desde los que se ejecutan porque es posible reemplazar la ruta a un ejecutable con una `DLL maliciosa` o un archivo ejecutable. Examinemos los permisos de los servicios que se ejecutan en Windows 10 para obtener una mejor comprensión de esto.
___

### **EXAMINANDO SERVICIOS CON SERVICES.MSC**

Como se discutió en la sección de procesos y servicios, podemos usar `services.msc` para ver y administrar casi todos los detalles relacionados con todos los servicios. Echemos un vistazo más de cerca al servicio asociado con `Windows Update (wuauserv)`.

![](https://academy.hackthebox.com/storage/modules/49/service_properties.png)

Tome nota de las diferentes propiedades disponibles para su visualización y configuración. Conocer el nombre del servicio es especialmente útil cuando se usan herramientas de línea de comandos para examinar y administrar servicios. La ruta al ejecutable es la ruta completa al programa `C:\WINDOWS\system32\svchost.exe -k netsvcs -p` y el comando a ejecutar cuando se inicia el servicio. Si los permisos NTFS del directorio de destino están configurados con permisos débiles, un atacante podría reemplazar el ejecutable original con uno creado con fines malintencionados. Hablamos más sobre los permisos de NTFS en la sección Permisos de NTFS frente a recursos compartidos de este módulo.

![](https://academy.hackthebox.com/storage/modules/49/logon.png)

La mayoría de los servicios se ejecutan con privilegios de `LocalSystem` de forma predeterminada, que es el nivel de acceso más alto permitido en un sistema operativo Windows individual. No todas las aplicaciones necesitan permisos de `Local System account`, por lo que es conveniente realizar una investigación caso por caso al considerar la instalación de nuevas aplicaciones en un entorno de Windows. Es una buena práctica identificar las aplicaciones que pueden ejecutarse con los mínimos privilegios posibles para alinearse con el principio de mínimo privilegio.

[Aquí hay un desglose del principio de privilegio mínimo](https://us-cert.cisa.gov/bsi/articles/knowledge/principles/least-privilege)

Cuentas de servicio integradas notables en Windows:
+ LocalService
+ NetworkService
+ LocalSystem

Nota: También podemos crear nuevas cuentas y usarlas con el único propósito de ejecutar un servicio.

![](https://academy.hackthebox.com/storage/modules/49/recovery_screen.png)

La pestaña de recuperación permite configurar los pasos en caso de que falle un servicio. Observe cómo se puede configurar este servicio para ejecutar un programa después del primer error. Este es otro vector más que un atacante podría usar para ejecutar programas maliciosos utilizando un servicio legítimo.
