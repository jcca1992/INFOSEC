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



