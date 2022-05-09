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
