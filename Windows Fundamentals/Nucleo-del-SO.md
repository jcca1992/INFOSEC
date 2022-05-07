## **ESTRUCTURA DEL SISTEMA OPERATIVO**

En los sistemas operativos Windows, el directorio raíz es <letra_unidad>:\ (comúnmente unidad C). El directorio raíz (también conocido como partición de arranque) es donde está instalado el sistema operativo. A otras unidades físicas y virtuales se les asignan otras letras, por ejemplo, Datos (E:). La estructura de directorios de la partición de arranque es la siguiente:

|Directorio|Funcion|
|--|--|
|Perflogs| Puede contener registros de rendimiento de Windows, pero está vacío de forma predeterminada.| 
|Program Files| En los sistemas de 32 bits, todos los programas de 16 y 32 bits se instalan aquí. En los sistemas de 64 bits, solo se instalan aquí los programas de 64 bits.| 
|Program Files (x86)| Los programas de 32 y 16 bits se instalan aquí en las ediciones de Windows de 64 bits.|
|ProgramData| Esta es una carpeta oculta que contiene datos que son esenciales para que se ejecuten ciertos programas instalados. El programa puede acceder a estos datos sin importar qué usuario los esté ejecutando.| 
|Users| Esta carpeta contiene perfiles de usuario para cada usuario que inicia sesión en el sistema y contiene las dos carpetas Pública y Predeterminada.|
|Default| La mayoría de los archivos necesarios para el sistema operativo Windows se encuentran aquí.|
|Public| Esta carpeta está diseñada para que los usuarios de computadoras compartan archivos y todos los usuarios pueden acceder a ella de forma predeterminada. Esta carpeta se comparte en la red de forma predeterminada, pero requiere una cuenta de red válida para acceder.|
|AppData| Los datos y la configuración de la aplicación por usuario se almacenan en una subcarpeta de usuario oculta (es decir, cliff.moore\AppData). Cada una de estas carpetas contiene tres subcarpetas. La carpeta Roaming contiene datos independientes de la máquina que deben seguir el perfil del usuario, como diccionarios personalizados. La carpeta Local es específica de la computadora y nunca se sincroniza a través de la red. LocalLow es similar a la carpeta Local, pero tiene un nivel de integridad de datos más bajo. Por lo tanto, puede ser utilizado, por ejemplo, por un navegador web configurado en modo protegido o seguro.|
|Windows| La mayoría de los archivos necesarios para el sistema operativo Windows se encuentran aquí.|
|System, System32, SysWOW64| Contiene todas las DLL necesarias para las funciones principales de Windows y la API de Windows. El sistema operativo busca en estas carpetas cada vez que un programa solicita cargar una DLL sin especificar una ruta absoluta.|
|WinSxS| La tienda de componentes de Windows contiene una copia de todos los componentes, actualizaciones y paquetes de servicio de Windows.|
___

### **EXPLORANDO ARCHIVOS CON LINEA DE COMANDOS**

Podemos explorar el sistema de archivos usando el comando [dir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir).

~~~
C:\htb> dir c:\ /a
 Volume in drive C has no label.
 Volume Serial Number is F416-77BE

 Directory of c:\

08/16/2020  10:33 AM    <DIR>          $Recycle.Bin
06/25/2020  06:25 PM    <DIR>          $WinREAgent
07/02/2020  12:55 PM             1,024 AMTAG.BIN
06/25/2020  03:38 PM    <JUNCTION>     Documents and Settings [C:\Users]
08/13/2020  06:03 PM             8,192 DumpStack.log
08/17/2020  12:11 PM             8,192 DumpStack.log.tmp
08/27/2020  10:42 AM    37,752,373,248 hiberfil.sys
08/17/2020  12:11 PM    13,421,772,800 pagefile.sys
12/07/2019  05:14 AM    <DIR>          PerfLogs
08/24/2020  10:38 AM    <DIR>          Program Files
07/09/2020  06:08 PM    <DIR>          Program Files (x86)
08/24/2020  10:41 AM    <DIR>          ProgramData
06/25/2020  03:38 PM    <DIR>          Recovery
06/25/2020  03:57 PM             2,918 RHDSetup.log
08/17/2020  12:11 PM        16,777,216 swapfile.sys
08/26/2020  02:51 PM    <DIR>          System Volume Information
08/16/2020  10:33 AM    <DIR>          Users
08/17/2020  11:38 PM    <DIR>          Windows
               7 File(s) 51,190,943,590 bytes
              13 Dir(s)  261,310,697,472 bytes free
~~~

El comando [tree](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tree) es útil para mostrar gráficamente la estructura de directorios de una ruta o disco.

~~~
C:\htb> tree "c:\Program Files (x86)\VMware"
Folder PATH listing
Volume serial number is F416-77BE
C:\PROGRAM FILES (X86)\VMWARE
├───VMware VIX
│   ├───doc
│   │   ├───errors
│   │   ├───features
│   │   ├───lang
│   │   │   └───c
│   │   │       └───functions
│   │   └───types
│   ├───samples
│   └───Workstation-15.0.0
│       ├───32bit
│       └───64bit
└───VMware Workstation
    ├───env
    ├───hostd
    │   ├───coreLocale
    │   │   └───en
    │   ├───docroot
    │   │   ├───client
    │   │   └───sdk
    │   ├───extensions
    │   │   └───hostdiag
    │   │       └───locale
    │   │           └───en
    │   └───vimLocale
    │       └───en
    ├───ico
    ├───messages
    │   ├───ja
    │   └───zh_CN
    ├───OVFTool
    │   ├───env
    │   │   └───en
    │   └───schemas
    │       ├───DMTF
    │       └───vmware
    ├───Resources
    ├───tools-upgraders
    └───x64
~~~

El comando `tree` puede proporcionarnos una gran cantidad de información. El siguiente comando se puede usar para recorrer todos los archivos en la unidad C, una pantalla a la vez. Este comando se puede modificar para que se ejecute en cualquier directorio.

~~~
tree c:\ /f | more
~~~

### RETO

Busque el directorio no estándar en la unidad C. Envíe el contenido del archivo flag guardado en este directorio.

R:

~~~
PS C:\Users\htb-student> tree c:\ /f | more
Folder PATH listing
Volume serial number is 905B-28C3
C:\
├───75afac25577675a9bfafd2405602
├───Academy
│       flag.txt
│
├───PerfLogs
├───Program Files
│   ├───Common Files
│   │   ├───microsoft shared
│   │   │   ├───ink
│   │   │   │   │   Alphabet.xml
│   │   │   │   │   Content.xml
│   │   │   │   │   hwrcommonlm.dat
│   │   │   │   │   hwrenclm.dat
│   │   │   │   │   hwrenUSlm.dat
│   │   │   │   │   hwrlatinlm.dat
│   │   │   │   │   hwrusash.dat
│   │   │   │   │   InkDiv.dll
│   │   │   │   │   InkObj.dll
│   │   │   │   │   InputPersonalization.exe
│   │   │   │   │   ipsar.xml
│   │   │   │   │   ipscat.xml
│   │   │   │   │   ipschs.xml
│   │   │   │   │   ipscht.xml
│   │   │   │   │   ipscsy.xml
│   │   │   │   │   ipsdan.xml
│   │   │   │   │   ipsdeu.xml
│   │   │   │   │   ipsel.xml
│   │   │   │   │   ipsen.xml
│   │   │   │   │   ipsesp.xml
│   │   │   │   │   ipsfin.xml
│   │   │   │   │   ipsfra.xml
│   │   │   │   │   ipshe.xml
│   │   │   │   │   ipshi.xml
│   │   │   │   │   ipshrv.xml
│   │   │   │   │   ipsid.xml
│   │   │   │   │   ipsita.xml
│   │   │   │   │   ipsjpn.xml
│   │   │   │   │   ipskor.xml
│   │   │   │   │   IpsMigrationPlugin.dll
│   │   │   │   │   ipsnld.xml
│   │   │   │   │   ipsnor.xml
│   │   │   │   │   ipsplk.xml
│   │   │   │   │   IpsPlugin.dll
│   │   │   │   │   ipsptb.xml
│   │   │   │   │   ipsptg.xml
~~~

al filtrar con el pipe line `|` y `more` estamos indicandole que no nos muestre todo directamente sino que nos muestre una parte si queremos ver mas presionamos enter y nos muestra otra parte. Tenemos que presionar `q`para salir del `more`

~~~
PS C:\Users\htb-student> Get-Content C:\Academy\flag.txt
c8fe8d977d3a0c655ed7cf81e4d13c75
~~~
___

## **SISTEMA DE ARCHIVO**

Hay 5 tipos de sistemas de archivos de Windows: FAT12, FAT16, FAT32, NTFS y exFAT. FAT12 y FAT16 ya no se usan en los sistemas operativos Windows modernos. Nos referiremos a los sistemas de archivos FAT32 y exFAT para esta capacitación, pero nuestro enfoque principal será el sistema de archivos NTFS.

FAT32 (File Allocation Table/Tabla de asignación de archivos) se usa ampliamente en muchos tipos de dispositivos de almacenamiento, como dispositivos de memoria USB y tarjetas SD, pero también se puede usar para formatear discos duros. El "32" en el nombre se refiere al hecho de que FAT32 usa 32 bits de datos para identificar grupos de datos en un dispositivo de almacenamiento.

`Pros de FAT32:`
+ Compatibilidad con dispositivos: se puede usar en computadoras, cámaras digitales, consolas de juegos, teléfonos inteligentes, tabletas y más.
+ Compatibilidad cruzada del sistema operativo: funciona en todos los sistemas operativos Windows a partir de Windows 95 y también es compatible con MacOS y Linux.

`Contras de FAT32:`
+ Solo se puede usar con archivos de menos de 4 GB.
+ Sin funciones integradas de protección de datos o compresión de archivos.
+ Debe usar herramientas de terceros para el cifrado de archivos.

NTFS (New Technology File System/Sistema de archivos de nueva tecnología) es el sistema de archivos predeterminado de Windows desde Windows NT 3.1. Además de compensar las deficiencias de FAT32, NTFS también tiene un mejor soporte para metadatos y un mejor rendimiento debido a la estructuración de datos mejorada.

`Pros de NTFS:`
+ NTFS es confiable y puede restaurar la consistencia del sistema de archivos en caso de una falla del sistema o pérdida de energía.
+ Brinda seguridad al permitirnos establecer permisos granulares en archivos y carpetas.
+ Admite particiones de gran tamaño.
+ Tiene un diario incorporado, lo que significa que las modificaciones de archivos (adición, modificación, eliminación) se registran.

`Contras de NTFS:`
+ La mayoría de los dispositivos móviles no admiten NTFS de forma nativa.
+ Los dispositivos multimedia más antiguos, como televisores y cámaras digitales, no ofrecen soporte para dispositivos de almacenamiento NTFS.
___

### **PERMISOS**

El sistema de archivos NTFS tiene muchos permisos básicos y avanzados. Algunos de los tipos de permisos clave son:

|Tipo de permiso|Descripcion|
|--|--|
|Full Control|Permite leer, escribir, cambiar, borrar archivos y carpetas.|
|Modify|Permite leer, escribir y borrar archivos/carpetas.|
|List Folder Contents|Permite ver y enumerar carpetas y subcarpetas, así como ejecutar archivos. Las carpetas solo heredan este permiso.|
|Read and Execute|Permite ver y enumerar archivos y subcarpetas, así como ejecutar archivos. Los archivos y las carpetas heredan este permiso.|
|Write|Permite agregar archivos a carpetas y subcarpetas y escribir en un archivo.|
|Read|Permite ver y enumerar carpetas y subcarpetas y ver el contenido de un archivo.|
|Traverse Folder|Esto permite o deniega la capacidad de moverse a través de carpetas para llegar a otros archivos o carpetas. Por ejemplo, es posible que un usuario no tenga permiso para enumerar el contenido del directorio o ver archivos en el directorio de documentos o aplicaciones web en este ejemplo c:\users\bsmith\documents\webapps\backups\backup_02042020.zip pero con los permisos de Traverse Folder aplicados, pueden acceder al archivo de copia de seguridad.|

Los archivos y las carpetas heredan los permisos NTFS de su carpeta principal para facilitar la administración, por lo que los administradores no necesitan establecer permisos de forma explícita para cada archivo y carpeta, ya que llevaría mucho tiempo. Si es necesario establecer permisos explícitamente, un administrador puede deshabilitar la herencia de permisos para los archivos y carpetas necesarios y luego establecer los permisos directamente en cada uno.
___

### **(ICACLS) LISTA DE CONTROL DE ACCESO DE CONTROL DE INTEGRIDAD**

Los permisos NTFS en archivos y carpetas en Windows se pueden administrar utilizando la GUI del Explorador de archivos en la pestaña de seguridad. Aparte de la GUI, también podemos lograr un buen nivel de granularidad sobre los permisos de archivos NTFS en Windows desde la línea de comandos usando la utilidad icacls.

Podemos enumerar los permisos NTFS en un directorio específico ejecutando `icacls` desde dentro del directorio de trabajo o `icacls C:\Windows` en un directorio que no se encuentra actualmente.

~~~
C:\htb> icacls c:\windows
c:\windows NT SERVICE\TrustedInstaller:(F)
           NT SERVICE\TrustedInstaller:(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(M)
           NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
           BUILTIN\Administrators:(M)
           BUILTIN\Administrators:(OI)(CI)(IO)(F)
           BUILTIN\Users:(RX)
           BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
           CREATOR OWNER:(OI)(CI)(IO)(F)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
~~~

El nivel de acceso al recurso aparece después de cada usuario en la salida. Los posibles ajustes de herencia son:
+ (CI): container inherit (herencia del contenedor)
+ (OI): object inherit (herencia del objeto)
+ (IO): inherit only (solo heredar)
+ (NP): do not propagate inherit (no propagar herencia)
+ (I): permission inherited from parent container (permiso heredado del contenedor principal)

En el ejemplo anterior, la cuenta `NT AUTHORITY\SYSTEM` tiene permisos de herencia de objeto, herencia de contenedor, solo herencia y acceso completo. Esto significa que esta cuenta tiene control total sobre todos los objetos del sistema de archivos en este directorio y subdirectorios.

Los permisos de acceso básicos son los siguientes:
+ F : full access
+ D : delete access
+ N : no access
+ M : modify access
+ RX : read and execute access
+ R : read-only access
+ W : write-only access

Podemos agregar y eliminar permisos a través de la línea de comando usando `icacls`. Aquí estamos ejecutando `icacls` en el contexto de una cuenta de administrador local que muestra el directorio `C:\users` donde el usuario `joe` no tiene permisos de escritura.

~~~
C:\htb> icacls c:\Users
c:\Users NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         BUILTIN\Users:(RX)
         BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
         Everyone:(RX)
         Everyone:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
~~~

Usando el comando `icacls c:\users /grant joe:f` podemos otorgar al usuario joe control total sobre el directorio, pero dado que (oi) y (ci) no se incluyeron en el comando, el usuario joe solo tendrá derechos sobre la carpeta `c:\users` pero no sobre los subdirectorios de usuario y los archivos contenidos en ellos.

~~~
C:\htb> icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files
~~~

~~~
C:\htb> icacls c:\users
c:\users WS01\joe:(F)
         NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         BUILTIN\Users:(RX)
         BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
         Everyone:(RX)
         Everyone:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
~~~

Estos permisos se pueden revocar con el comando `icacls c:\users /remove joe`.

`icacls` es muy poderoso y se puede usar en una configuración de dominio para otorgar a ciertos usuarios o grupos permisos específicos sobre un archivo o carpeta, denegar acceso explícitamente, habilitar o deshabilitar permisos de herencia y cambiar la propiedad del directorio/archivo.

Puede encontrar una lista completa de los argumentos de la línea de comandos de `icacls` y la configuración detallada de los permisos [aquí](https://ss64.com/nt/icacls.html).
___

### RETO

¿Qué usuario del sistema tiene control total sobre el directorio c:\users?

R: bob.smith

~~~
PS C:\Users\htb-student> icacls c:\Users
c:\Users Everyone:(OI)(CI)(RX)
         NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         WS01\bob.smith:(OI)(CI)(F)
         BUILTIN\Users:(OI)(CI)(RX)
~~~
___

## **NTFS VS PERMISOS COMPARTIDOS**

Microsoft posee más del [70%](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-201804-202104) de la cuota de mercado mundial de sistemas operativos de escritorio con Windows. Esto explica por qué la mayoría de los autores de malware eligen escribir malware para Windows y por qué muchos perciben que Windows es menos seguro que otros sistemas operativos. Desde una perspectiva comercial, tiene sentido que los autores de malware gasten recursos en escribir malware para Windows. Es un objetivo de alto valor. La idea de que cualquier sistema operativo es inmune al malware es una falacia técnica. Si el software se puede escribir para un sistema operativo, entonces se puede escribir un virus para un sistema operativo. Tenga en cuenta que un virus, por definición, es software escrito con intenciones maliciosas y se puede escribir para cualquier sistema operativo. Muchas variantes de malware escritas para Windows pueden propagarse por la red a través de recursos compartidos de red con la aplicación de permisos indulgentes. También vale la pena señalar que, hasta el día de hoy, la infame vulnerabilidad `EternalBlue` todavía persigue a los sistemas Windows sin parches que ejecutan `SMBv1` y, a menudo, allana el camino para que el ransomware cierre las organizaciones.

El protocolo de bloque de mensajes del servidor, en ingles `Server Message Block protocol (SMB)` se usa en Windows para conectar recursos compartidos como archivos e impresoras. Se utiliza en entornos de empresas grandes, medianas y pequeñas. Vea la imagen a continuación para visualizar este concepto:

![](https://academy.hackthebox.com/storage/modules/49/smb_diagram.png)

>Nota: Cada vez que vea una visualización o diagrama de un concepto, tómese su tiempo para comprenderlo a fondo. Una imagen puede valer más que mil palabras, pero es muy tentador omitirla al leer.

Los permisos NTFS y los permisos compartidos a menudo se entienden como iguales. Tenga en cuenta que no son lo mismo, pero a menudo se aplican al mismo recurso compartido. Echemos un vistazo a los permisos individuales que se pueden configurar para proteger y otorgar acceso a los objetos a un recurso compartido de red alojado en un sistema operativo Windows que ejecuta el sistema de archivos NTFS.
___

### **RECURSOS COMPARTIDOS**

|Permisos|Descripcion|
|--|--|
|Full Control| Los usuarios pueden realizar todas las acciones otorgadas por los permisos Change y Read, así como cambiar los permisos para archivos y subcarpetas NTFS.|
|Change|Los usuarios pueden leer, editar, eliminar y agregar archivos y subcarpetas|
|Read|Los usuarios pueden ver el contenido de archivos y subcarpetas|
___

### **PERMISOS BASICOS NTFS**

|Permiso|Descripcion|
|--|--|
|Full Control| Los usuarios pueden agregar, editar, mover, eliminar archivos y carpetas, así como cambiar los permisos NTFS que se aplican a todas las carpetas permitidas.|
|Modify| A los usuarios se les otorgan o niegan permisos para ver y modificar archivos y carpetas. Esto incluye agregar o eliminar archivos|
|Read & Execute| A los usuarios se les permite o deniega permisos para leer el contenido de los archivos y ejecutar programas|
|List folder contents| A los usuarios se les permite o deniega permisos para ver una lista de archivos y subcarpetas|
|Read| A los usuarios se les permite o deniega permisos para leer el contenido de los archivos|
|Write| A los usuarios se les permite o deniega permisos para escribir cambios en un archivo y agregar nuevos archivos a una carpeta|
|Special Permissions| Una variedad de opciones de permisos avanzados|
___

### **PERMISOS ESPECIALES NTFS**

|Permiso|Descripcion|
|--|--|
|Full control| A los usuarios se les permite o deniega permisos para agregar, editar, mover, eliminar archivos y carpetas, así como cambiar los permisos NTFS que se aplican a todas las carpetas permitidas.|
|Traverse folder / execute file| A los usuarios se les permite o se les niegan permisos para acceder a una subcarpeta dentro de una estructura de directorio incluso si se le niega el acceso al contenido en el nivel de la carpeta principal. Los usuarios también pueden permitir o denegar permisos para ejecutar programas|
|List folder/read data| A los usuarios se les permite o deniega permisos para ver archivos y carpetas contenidos en la carpeta principal. También se puede permitir a los usuarios abrir y ver archivos|
|Read attributes| A los usuarios se les otorgan o niegan permisos para ver los atributos básicos de un archivo o carpeta. Ejemplos de atributos básicos: sistema, archivo, solo lectura y oculto|
|Read extended attributes| A los usuarios se les otorgan o niegan permisos para ver los atributos extendidos de un archivo o carpeta. Los atributos difieren según el programa.|
|Create files/write data| A los usuarios se les permite o deniega permisos para crear archivos dentro de una carpeta y realizar cambios en un archivo.|
|Create folders/append data| A los usuarios se les permite o deniega permisos para crear subcarpetas dentro de una carpeta. Se pueden agregar datos a los archivos, pero el contenido preexistente no se puede sobrescribir.|
|Write attributes| A los usuarios se les permite o deniega cambiar los atributos del archivo. Este permiso no otorga acceso para crear archivos o carpetas.|
|Write extended attributes| A los usuarios se les permite o se les niegan permisos para cambiar los atributos extendidos en un archivo o carpeta. Los atributos difieren según el programa.|
|Delete subfolders and files| A los usuarios se les permite o deniega permisos para eliminar subcarpetas y archivos. Las carpetas principales no se eliminarán|
|Delete| A los usuarios se les permite o deniega permisos para eliminar carpetas principales, subcarpetas y archivos.|
|Read permissions| Los usuarios tienen permitidos o denegados permisos para leer los permisos de una carpeta.|
|Change permissions| A los usuarios se les permite o deniega permisos para cambiar los permisos de un archivo o carpeta|
|Take ownership| A los usuarios se les permite o se les niega el permiso para tomar posesión de un archivo o carpeta. El propietario de un archivo tiene permisos completos para cambiar cualquier permiso.|

Tenga en cuenta que los permisos NTFS se aplican al sistema donde se alojan la carpeta y los archivos. Las carpetas creadas en NTFS heredan los permisos de las carpetas principales de forma predeterminada. Es posible deshabilitar la herencia para establecer permisos personalizados en carpetas principales y subcarpetas, como lo haremos más adelante en este módulo. Los permisos para compartir se aplican cuando se accede a la carpeta a través de SMB, generalmente desde un sistema diferente a través de la red. Esto significa que alguien que inició sesión localmente en la máquina o a través de RDP puede acceder a la carpeta y los archivos compartidos simplemente navegando a la ubicación en el sistema de archivos y solo necesita considerar los permisos NTFS. Los permisos a nivel de NTFS brindan a los administradores un control mucho más granular sobre lo que los usuarios pueden hacer dentro de una carpeta o archivo.
___

### **CREANDO RECURSOS COMPARTIDOS**

Para obtener una comprensión fundamental sólida de SMB y su relación con NTFS, crearemos un recurso compartido de red en el `Windows 10 de la maquina objetivo`.

>Nota: Es una experiencia de aprendizaje ideal tener el Pwnbox abierto en pantalla completa en un monitor separado para que podamos tener al menos una pantalla dedicada a mostrar el contenido escrito y una pantalla para las maquinas con las que estamos interactuando. Alternativamente, si solo tenemos acceso a una pantalla, podemos usar esa para interacciones con maquinas y un teléfono inteligente o tableta para hacer referencia al contenido escrito.

En este caso, crearemos una carpeta compartida creando primero una nueva carpeta en el escritorio de Windows 10. Tenga en cuenta que en la mayoría de los entornos empresariales grandes, los recursos compartidos se crean en una red de área de almacenamiento (SAN), un dispositivo de almacenamiento conectado a la red (NAS) o una partición separada en unidades a las que se accede a través de un sistema operativo de servidor como Windows Server. Si alguna vez nos encontramos con acciones en un sistema operativo de escritorio, será una pequeña empresa o podría ser un sistema `beachhead` utilizado por un pentester o un atacante malicioso para recopilar y filtrar datos.

Pasaremos por este proceso usando la GUI en Windows

#### *CREANDO EL ARCHIVO*

![](https://academy.hackthebox.com/storage/modules/49/creating_directory.png)

Vamos a utilizar la opción `Advanced Sharing` para configurar nuestro recurso compartido.

#### *COMPARTIENDO ARCHIVO*

![](https://academy.hackthebox.com/storage/modules/49/configuring_share.png)

Observe cómo el nombre del recurso compartido automáticamente toma como valor predeterminado el nombre de la carpeta. Además, podemos ver que es posible limitar la cantidad de usuarios que se pueden conectar a este recurso compartido simultáneamente. En un entorno del mundo real, es una buena práctica que los administradores establezcan este número de acuerdo con la cantidad de usuarios que necesitan acceder regularmente al recurso que se comparte.

Similar a los permisos NTFS, existe una `lista de control de acceso (ACL)` para los recursos compartidos. Podemos considerar esto como la lista de permisos SMB. Tenga en cuenta que con los recursos compartidos, las listas de permisos de SMB y NTFS se aplican a todos los recursos que se comparten en Windows. La ACL contiene `entradas de control de acceso (ACE)`. Por lo general, estas ACE están compuestas por `usuarios` y `grupos` (también llamados principales de seguridad), ya que son un mecanismo adecuado para administrar y rastrear el acceso a los recursos compartidos.

Observe la entrada de control de acceso predeterminada y la configuración de permisos.

#### *COMPARTIR PERMISOS ACL (PESTAÑA COMPARTIR)*

![](https://academy.hackthebox.com/storage/modules/49/share_permissions.png)

Por ahora, aplicaremos esta configuración para probar el efecto de esta ACL y los permisos aplicados tal cual. Probaremos la conectividad desde el Pwnbox abriendo la terminal y usando `smbclient`.

>Nota: Un servidor es técnicamente una función de software utilizada para atender las solicitudes de un cliente. En este caso, el Pwnbox es nuestro cliente y la maquina target con Windows 10 es nuestro servidor.

#### *USANDO SMBCLIENT PARA CONECTARSE AL RECURSO COMPARTIDO*

~~~
Juceco@htb[/htb]$ smbclient -L IPaddressOfTarget -U htb-student
Enter WORKGROUP\htb-student's password: 

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	Company Data    Disk      
	IPC$            IPC       Remote IPC
~~~

¿Qué podría impedirnos acceder a este recurso compartido si todas nuestras entradas son correctas y nuestra lista de permisos tiene el grupo `all` presente con al menos permisos de lectura?
___

### **CONSIDERACIONES DE FIREWALL DE WINDOWS DEFENDER**

Es el Firewall de Windows Defender el que podría estar bloqueando el acceso al recurso compartido SMB. Dado que nos estamos conectando desde un sistema basado en Linux, el firewall ha bloqueado el acceso desde cualquier dispositivo que no esté unido al mismo `workgroup`. También es importante tener en cuenta que cuando un sistema Windows forma parte de un grupo de trabajo, todas las solicitudes de `inicio de sesión de red `se autentican en la base de datos `SAM` de ese sistema Windows en particular. Cuando un sistema Windows se une a un entorno de dominio de Windows, todas las solicitudes de inicio de sesión de red se autentican en `Active Directory`. La principal diferencia entre un grupo de trabajo y un Dominio de Windows en términos de autenticación es que con un grupo de trabajo se usa la base de datos SAM local y en un Dominio de Windows se usa una base de datos centralizada basada en la red (Active Directory). Debemos conocer esta información cuando intentemos iniciar sesión y autenticarnos con un sistema Windows. Considere dónde está alojada la cuenta de htb-student para conectarse correctamente al objetivo.

En términos de conexiones de bloqueo de firewall, esto se puede probar desactivando completamente cada perfil de firewall en Windows o habilitando reglas de firewall entrantes predefinidas específicas en la c`onfiguración de seguridad avanzada de Firewall de Windows Defender`. Como la mayoría de los firewalls, el Firewall de Windows Defender permite o deniega el tráfico (solicitudes de acceso y conexión en este caso) `entrante` y/o `saliente`.

Las diferentes reglas de entrada y salida están asociadas con los diferentes perfiles de firewall en defender.

Perfiles de Firewall de Windows Defender:
+ `Public`
+ `Private`
+ `Domain`

Es una buena práctica habilitar reglas predefinidas o agregar excepciones personalizadas en lugar de desactivar el firewall por completo. Desafortunadamente, es muy común que los firewalls se dejen completamente desactivados por conveniencia o por falta de comprensión. Las reglas de firewall en los sistemas de escritorio se pueden administrar de forma centralizada cuando se unen a un entorno de dominio de Windows mediante el uso de la directiva de grupo. Los conceptos y configuraciones de directivas de grupo están fuera del alcance de este módulo.

Una vez que las reglas de firewall `entrantes` adecuadas estén habilitadas, nos conectaremos con éxito al recurso compartido. Tenga en cuenta que solo podemos conectarnos al recurso compartido porque la cuenta de usuario que estamos usando (`htb-student`) está en el `grupo Todos`. Recuerde que dejamos los permisos de uso compartido específicos para el grupo Todos establecidos en Lectura, lo que literalmente significa que solo podremos Leer archivos en este recurso compartido. Una vez que se establece una conexión con un recurso compartido, podemos crear un `punto de montaje` desde nuestro Pwnbox al sistema de archivos del Windows 10 de la maquina objetivo. Aquí es donde también debemos considerar que los permisos NTFS se aplican junto con los permisos compartidos. Recuerde que NTFS es el sistema de archivos predeterminado en Windows. Volvamos a nuestra sesión xfreerdp con nuestro Windows 10 de la maquina objetivo y echemos un vistazo a los permisos NTFS en la carpeta Data Company.
