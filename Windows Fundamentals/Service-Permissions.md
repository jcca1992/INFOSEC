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
___
### **EXAMINANDO SERVICIOS CON SC**

`Sc` también se puede utilizar para configurar y administrar servicios. Experimentemos con algunos comandos.

~~~
C:\Users\htb-student>sc qc wuauserv
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: wuauserv
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\WINDOWS\system32\svchost.exe -k netsvcs -p
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Update
        DEPENDENCIES       : rpcss
        SERVICE_START_NAME : LocalSystem
~~~

El comando `sc qc` se utiliza para consultar el servicio. Aquí es donde puede resultar útil conocer los nombres de los servicios. Si quisiéramos consultar un servicio en un dispositivo a través de la red, podríamos especificar el nombre de host o la dirección IP inmediatamente después de `sc`.

~~~
C:\Users\htb-student>sc //hostname or ip of box query ServiceName
~~~

También podemos usar `sc` para iniciar y detener servicios.

~~~
C:\Users\htb-student> sc stop wuauserv

[SC] OpenService FAILED 5:

Access is denied.
~~~

Observe cómo se nos niega el acceso para realizar esta acción sin ejecutarla dentro de un contexto administrativo. Si ejecutamos un símbolo del sistema con `privilegios elevados`, se nos permitirá completar esta acción.

~~~
C:\WINDOWS\system32> sc config wuauserv binPath=C:\Winbows\Perfectlylegitprogram.exe

[SC] ChangeServiceConfig SUCCESS

C:\WINDOWS\system32> sc qc wuauserv

[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: wuauserv
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Winbows\Perfectlylegitprogram.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Update
        DEPENDENCIES       : rpcss
        SERVICE_START_NAME : LocalSystem

~~~

Si estuviéramos investigando una situación en la que sospecháramos que el sistema tiene malware, `sc` nos daría la capacidad de buscar y analizar rápidamente los servicios de destino común y los servicios recién creados. También es mucho más amigable con los scripts que utilizar herramientas `GUI` como `services.msc`.

Otra forma útil de examinar los permisos de servicio mediante `sc` es a través del comando `sdshow`.

~~~
C:\WINDOWS\system32> sc sdshow wuauserv

D:(A;;CCLCSWRPLORC;;;AU)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)S:(AU;FA;CCDCLCSWRPWPDTLOSDRCWDWO;;;WD)
~~~

A primera vista, la salida parece una locura. Casi parece que hemos hecho algo mal en nuestro comando, pero esta locura tiene un significado. Cada objeto con nombre en Windows es un [objeto asegurable](https://docs.microsoft.com/en-us/windows/win32/secauthz/securable-objects), e incluso algunos objetos sin nombre son asegurables. Si es asegurable en un sistema operativo Windows, tendrá un [descriptor de seguridad](https://docs.microsoft.com/en-us/windows/win32/secauthz/security-descriptors). Los descriptores de seguridad identifican al propietario del objeto y un grupo principal que contiene una Lista de control de acceso discrecional `Discretionary Access Control List` (`DACL`) y una Lista de control de acceso al sistema `System Access Control List` (`SACL`).

Por lo general, una `DACL` se usa para controlar el acceso a un objeto y una `SACL` se usa para contabilizar y registrar los intentos de acceso. Esta sección examinará la `DACL`, pero los mismos conceptos se aplicarían a una `SACL`.

~~~
D:(A;;CCLCSWRPLORC;;;AU)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)
~~~

Esta amalgama de caracteres agrupados y delimitados por paréntesis abiertos y cerrados está en un formato conocido como Lenguaje de definición de descriptores de seguridad `Security Descriptor Definition Language` (`SDDL`).

Podemos tener la tentación de leer de izquierda a derecha porque así es como se escribe típicamente el idioma inglés, pero puede ser muy diferente cuando interactuamos con computadoras. Lea el descriptor de seguridad completo para el servicio `Windows Update` (`wuauserv`) en este orden, comenzando con la primera letra y el conjunto de paréntesis:

`D: (A;;CCLCSWRPLORC;;;AU)`

1. D: - los caracteres anteriores son permisos DACL
2. AU - define la entidad de seguridad de Usuarios Autenticados
3. A;; - se permite el acceso
4. CC - SERVICE_QUERY_CONFIG es el nombre completo y es una consulta al service control manager (SCM) para la configuración del servicio
5. LC - SERVICE_QUERY_STATUS es el nombre completo y es una consulta al service control manager (SCM) para el estado actual del servicio
6. SW - SERVICE_ENUMERATE_DEPENDENTS es el nombre completo y enumerará una lista de servicios dependientes
7. RP - SERVICE_START es el nombre completo e iniciará el servicio
8. LO - SERVICE_INTERROGATE es el nombre completo y consultará el estado actual del servicio
9. RC - READ_CONTROL  es el nombre completo y consultará el descriptor de seguridad del servicio

Mientras leemos el descriptor de seguridad, puede ser fácil perderse en el orden aparentemente aleatorio de los caracteres, pero recuerde que esencialmente estamos viendo entradas de control de acceso en una lista de control de acceso. Cada conjunto de 2 caracteres entre los puntos y comas representa acciones que un usuario o grupo específico puede realizar.

`;;CCLCSWRPLORC;;;`

Después del último conjunto de puntos y comas, los caracteres especifican la entidad de seguridad (Usuario y/o Grupo) que tiene permiso para realizar esas acciones.

`;;;AU`

El carácter inmediatamente después de los paréntesis de apertura y antes del primer conjunto de punto y coma define si las acciones están permitidas o denegadas.

`A;;`

Este descriptor de seguridad completo asociado con el servicio `Windows Update` (`wuauserv`) tiene tres conjuntos de entradas de control de acceso porque hay tres enditades de seguridad diferentes. Cada entidad de seguridad tiene permisos específicos aplicados.
___
### **EXAMINANDO SERVICIOS CON POWERSHELL**
Con el `cmdlet` `Get-Acl` de PowerShell, podemos examinar los permisos del servicio seleccionando la ruta de un servicio específico en el registro.

~~~
PS C:\Users\htb-student> Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List

Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\wuauserv
Owner  : NT AUTHORITY\SYSTEM
Group  : NT AUTHORITY\SYSTEM
Access : BUILTIN\Users Allow  ReadKey
         BUILTIN\Users Allow  -2147483648
         BUILTIN\Administrators Allow  FullControl
         BUILTIN\Administrators Allow  268435456
         NT AUTHORITY\SYSTEM Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  268435456
         CREATOR OWNER Allow  268435456
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadKey
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  -2147483648
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         ReadKey
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         -2147483648
Audit  :
Sddl   : O:SYG:SYD:AI(A;ID;KR;;;BU)(A;CIIOID;GR;;;BU)(A;ID;KA;;;BA)(A;CIIOID;GA;;;BA)(A;ID;KA;;;SY)(A;CIIOID;GA;;;SY)(A
         ;CIIOID;GA;;;CO)(A;ID;KR;;;AC)(A;CIIOID;GR;;;AC)(A;ID;KR;;;S-1-15-3-1024-1065365936-1281604716-3511738428-1654
         721687-432734479-3232135806-4053264122-3456934681)(A;CIIOID;GR;;;S-1-15-3-1024-1065365936-1281604716-351173842
         8-1654721687-432734479-3232135806-4053264122-3456934681)
~~~

Observe cómo este comando devuelve permisos de cuenta específicos en un formato fácil de leer y en `SDDL`. Además, el `SID` que representa cada entidad de seguridad (usuario y/o grupo) está presente en el `SDDL`. Esto es algo que no obtenemos cuando ejecutamos `sc` desde el símbolo del sistema.

Saber cómo interactuar con los servicios y sus permisos asociados desde la línea de comandos facilita la creación de scripts para estas tareas. Si bien es bueno saber cómo realizar estas tareas desde la `GUI`, no escala bien a medida que comenzamos a ingresar a entornos de red y dominios más grandes.