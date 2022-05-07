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
