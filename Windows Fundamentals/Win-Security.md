## WINDOWS SECURITY

La seguridad es un tema crítico en los sistemas operativos de Windows. Los sistemas de Windows tienen muchas partes móviles que presentan una vasta superficie de ataque. Debido a las muchas aplicaciones, características y capas de configuración incorporadas, los sistemas de Windows se pueden configurar fácilmente, abriéndolos para atacar incluso si están completamente parcheados.

Tiene muchas características incorporadas que se pueden abusar y ha sufrido una amplia variedad de vulnerabilidades críticas, lo que resulta en hazañas remotas y locales muy utilizadas y muy efectivas.

Microsoft ha mejorado la seguridad de Windows a lo largo de los años. A medida que la interconexión de nuestro mundo continúa expandiéndose y los atacantes se vuelven más sofisticados, Microsoft ha seguido agregando nuevas características que los administradores de sistemas pueden utilizar para endurecer los sistemas y bloquear activamente y detectar intentos de intrusión y mal uso.

Windows sigue ciertos principios de seguridad. Estas son unidades en el sistema que pueden ser autorizadas o autenticadas para una acción particular. Estas unidades incluyen usuarios, computadoras en la red, hilos o procesos. Los principios están diseñados para dificultar que los atacantes o el software malicioso obtengan acceso no autorizado y exploten el sistema de manera no deseada.
___
### IDENTIFICADOR DE SEGURIDAD (SID)

Cada una de las entidades de seguridad del sistema tiene un identificador de seguridad único (SID). El sistema genera automáticamente SIDS. Esto significa que incluso si, por ejemplo, tenemos dos usuarios idénticos en el sistema, Windows puede distinguir los dos y sus derechos en función de su SIDS. Los SIDS son valores de cadena con diferentes longitudes, que se almacenan en la base de datos de seguridad. Estas SID se agregan al token de acceso del usuario para identificar todas las acciones que el usuario está autorizado a tomar.

Un SID consiste en la autoridad del identificador y la ID relativa (RID). En un entorno de dominio `Active Directory` (AD), el SID también incluye el dominio SID.

~~~
PS C:\htb> whoami /user

USER INFORMATION
----------------

User Name           SID
=================== =============================================
ws01\bob S-1-5-21-674899381-4069889467-2080702030-1002
~~~

El SID se descompone en este patrón

~~~
(SID)-(revision level)-(identifier-authority)-(subauthority1)-(subauthority2)-(etc)
~~~

Desglosemos el SID pieza por pieza.

|Numero|Significado|Descripcion|
|--|--|--|
|S|SID|Identifica la cadena como un SID.|
|1|Nivel de Revision|Hasta la fecha, esto nunca ha cambiado y siempre ha sido `1`.|
|5|Identificador de Autoridad|Una cadena de 48 bits que identifica la autoridad (la computadora o la red) que creó el SID.|
|21|Subautoridad 1| Este es un número variable que identifica la relación o grupo del usuario descrito por el SID a la autoridad que la creó. Nos dice en qué orden esta autoridad creó la cuenta del usuario.|
|674899381-4069889467-2080702030|Subautoridad 2|Nos dice qué computadora (o dominio) creó el número|
|1002|Subautoridad 3|El RID que distingue una cuenta de otra. Nos dice si este usuario es un usuario normal, un invitado, un administrador o parte de algún otro grupo|
___

### ADMINISTRADOR DE CUENTAS DE SEGURIDAD (SAM) Y CONTROL DE ACCESO DE ENTRADAS (ACE)

SAM otorga derechos a una red para ejecutar procesos específicos.

Los derechos de acceso en sí son administrados por el Control de Acceso de Entradas (ACE) en las listas de control de acceso (ACL). Las ACL contienen ACE que definen qué usuarios, grupos o procesos tienen acceso a un archivo o para ejecutar un proceso, por ejemplo

Los permisos para acceder a objetos de seguridad son dados por el descriptor de seguridad, clasificados en dos tipos de ACL: la lista de control de acceso discrecional (DACL) o la lista de control de acceso al sistema (SACL). Cada hilo y proceso iniciado o inicializado por un usuario pasa por un proceso de autorización. Una parte integral de este proceso son los tokens de acceso, validados por la Autoridad de Seguridad local (LSA). Además de SID, estos tokens de acceso contienen otra información relevante para la seguridad. Comprender estas funcionalidades es una parte esencial de aprender a usar y trabajar en torno a estos mecanismos de seguridad durante la fase de escalada de privilegios.
___
### CONTROL DE CUENTAS DE USUARIOS (UAC)

[El control de acceso al usuario (UAC)](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works) es una característica de seguridad en Windows para evitar que el malware ejecute o manipule procesos que podrían dañar la computadora o su contenido. Existe el modo de aprobación de administrador en UAC, que está diseñado para evitar que se instale un software no deseado sin el conocimiento del administrador o para evitar que se realicen cambios en todo el sistema. Seguramente ya ha visto el mensaje de consentimiento si ha instalado un software específico, y su sistema ha pedido confirmación si desea que lo instalen. Dado que la instalación requiere derechos de administrador, aparece una ventana, preguntándole si desea confirmar la instalación. Con un usuario estándar que no tiene derechos para la instalación, la ejecución será denegada o se le solicitará la contraseña del administrador. Este indicador de consentimiento interrumpe la ejecución de scripts o binarios que el malware o los atacantes intentan ejecutar hasta que el usuario ingrese la contraseña o confirme la ejecución. Para comprender cómo funciona UAC, necesitamos saber cómo está estructurado y cómo funciona, y qué desencadena el indicador de consentimiento. El siguiente diagrama, adaptado de la fuente [aquí](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works), ilustra cómo funciona UAC.

![](https://academy.hackthebox.com/storage/modules/49/uacarchitecture1.png)
___
### REGISTRO

El [registro](https://en.wikipedia.org/wiki/Windows_Registry) es una base de datos jerárquica en Windows, crítico para el sistema operativo. Almacena la configuración de bajo nivel para el sistema operativo Windows y las aplicaciones que eligen usarlo. Se divide en datos específicos de la computadora y específicos del usuario. Podemos abrir el editor de registro escribiendo `regedit` desde la línea de comandos o la barra de búsqueda de Windows.

![](https://academy.hackthebox.com/storage/modules/49/regedit.png)

La estructura de árbol consta de carpetas principales (teclas de raíz) en las que se encuentran las subcarpetas (subkeyas) con sus entradas/archivos (valores). Hay 11 tipos diferentes de valores que se pueden ingresar en una subpuesta.

|Valor|Tipo|
|--|--|
|REG_BINARY|Datos binarios en cualquier forma.|
|REG_DWORD|Un número de 32 bits.|
|REG_DWORD_LITTLE_ENDIAN|Un número de 32 bits en formato little-Endian. Windows está diseñado para ejecutarse con arquitecturas de computadoras little-Endian. Por lo tanto, este valor se define como reg_dword en los archivos de encabezado de Windows.|
|REG_DWORD_BIG_ENDIAN|Un número de 32 bits en formato de big-endian. Algunos sistemas UNIX admiten arquitecturas de big-endian.|
|REG_EXPAND_SZ|Una cadena null-terminated que contiene referencias no expandidas a las variables de entorno (por ejemplo, "%path%"). Será una cadena Unicode o ANSI dependiendo de si usa las funciones Unicode o ANSI. Para expandir las referencias variables de entorno, use la función [ExpandenVironmentTrings](https://docs.microsoft.com/en-us/windows/win32/api/processenv/nf-processenv-expandenvironmentstringsa).|
|REG_LINK|Una cadena Unicode null-terminated que contiene la ruta objetivo de un enlace simbólico creado llamando a la función [RegCreateKeyex](https://docs.microsoft.com/en-us/windows/desktop/api/Winreg/nf-winreg-regcreatekeyexa) con REG_OPTION_CREATE_LINK.|
|REG_MULTI_SZ|Una secuencia de cadenas null-terminated, terminada por una cadena vacía (\ 0). El siguiente es un ejemplo: String1 \ 0String2 \ 0String3 \ 0LastString \ 0 \ 0 La primera \ 0 termina la primera cadena, la segunda a la última \ 0 termina la última cadena, y la final \ 0 termina la secuencia. Tenga en cuenta que el terminador final debe tenerse en cuenta en la longitud de la cadena.|
|REG_NONE|Sin tipo de valor definido.|
|REG_QWORD| Un numero de 64 Bit|
|REG_QWORD_LITTLE_ENDIAN|Un número de 64 bits en formato little-Endian. Windows está diseñado para ejecutarse con arquitecturas de computadoras little-Endian. Por lo tanto, este valor se define como reg_qword en los archivos de encabezado de Windows.|
|REG_SZ|Una cadena null-terminated . Esto será una cadena Unicode o ANSI, dependiendo de si usa las funciones Unicode o ANSI.|

>Endianness designa el formato en el que se almacenan los datos de más de un byte en un ordenador. Usando este criterio el sistema 
`big-endian` adoptado por Motorola entre otros, consiste en representar los bytes en el orden "natural": así el valor hexadecimal 0x4A3B2C1D se codificaría en memoria en la secuencia {4A, 3B, 2C, 1D}. En el sistema 
`little-endian` adoptado por Intel, entre otros, el mismo valor se codificaría como {1D, 2C, 3B, 4A}, de manera que de este modo se hace más intuitivo el acceso a datos, porque se efectúa fácilmente de manera incremental de menos relevante a más relevante (siempre se opera con incrementos de contador en la memoria), en un paralelismo a "lo importante no es como empiezan las cosas, sino como acaban."

>null-terminated es una cadena de caracteres almacenada como una matriz y termina con un caracter nulo (un caracter con valor 0)

Fuente: https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-value-types

Cada carpeta en `Computer` es una Key. Todas las Key raices comienzan con `HKEY`. Una key como `HKEY-LOCAL-MACHINE` se abrevia como `HKLM`. HKLM contiene todas las configuraciones que son relevantes para el sistema local. Esta Key Raiz contiene seis subkeys como `SAM`, `SEGURIDAD`, `SISTEMA`, `SOFTWARE`, `HARDWARE` y `BCD`, cargadas en la memoria en el momento del arranque (excepto HARDWARE que se carga dinámicamente).

![](https://academy.hackthebox.com/storage/modules/49/regedit2.png)

Todo el registro del sistema se almacena en varios archivos en el sistema operativo. Puede encontrarlos en `C:\Windows\System32\Config\`.

~~~
PS C:\htb> ls

    Directory: C:\Windows\system32\config

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         12/7/2019   4:14 AM                Journal
d-----         12/7/2019   4:14 AM                RegBack
d-----         12/7/2019   4:14 AM                systemprofile
d-----         8/12/2020   1:43 AM                TxR
-a----         8/13/2020   6:02 PM        1048576 BBI
-a----         6/25/2020   4:36 PM          28672 BCD-Template
-a----         8/30/2020  12:17 PM       33816576 COMPONENTS
-a----         8/13/2020   6:02 PM         524288 DEFAULT
-a----         8/26/2020   7:51 PM        4603904 DRIVERS
-a----         6/25/2020   3:37 PM          32768 ELAM
-a----         8/13/2020   6:02 PM          65536 SAM
-a----         8/13/2020   6:02 PM          65536 SECURITY
-a----         8/13/2020   6:02 PM       87818240 SOFTWARE
-a----         8/13/2020   6:02 PM       17039360 SYSTEM
~~~

La sección `user-specific registry` (`HKCU`) se almacena en la carpeta del usuario (es decir, `C:\Windows\Users\<USERNAME>\Ntuser.dat`).

~~~
PS C:\htb> gci -Hidden

    Directory: C:\Users\bob

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--h--         6/25/2020   5:12 PM                AppData
d--hsl         6/25/2020   5:12 PM                Application Data
d--hsl         6/25/2020   5:12 PM                Cookies
d--hsl         6/25/2020   5:12 PM                Local Settings
d--h--         6/25/2020   5:12 PM                MicrosoftEdgeBackups
d--hsl         6/25/2020   5:12 PM                My Documents
d--hsl         6/25/2020   5:12 PM                NetHood
d--hsl         6/25/2020   5:12 PM                PrintHood
d--hsl         6/25/2020   5:12 PM                Recent
d--hsl         6/25/2020   5:12 PM                SendTo
d--hsl         6/25/2020   5:12 PM                Start Menu
d--hsl         6/25/2020   5:12 PM                Templates
-a-h--         8/13/2020   6:01 PM        2883584 NTUSER.DAT
-a-hs-         6/25/2020   5:12 PM         524288 ntuser.dat.LOG1
-a-hs-         6/25/2020   5:12 PM        1011712 ntuser.dat.LOG2
-a-hs-         8/17/2020   5:46 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.0.regtrans-ms
-a-hs-         8/17/2020  12:13 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.1.regtrans-ms
-a-hs-         8/17/2020  12:13 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.2.regtrans-ms
-a-hs-         8/17/2020   5:46 PM          65536 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.blf
-a-hs-         6/25/2020   5:15 PM          65536 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TM.blf
-a-hs-         6/25/2020   5:12 PM         524288 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TMContainer000000000
                                                  00000000001.regtrans-ms
-a-hs-         6/25/2020   5:12 PM         524288 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TMContainer000000000
                                                  00000000002.regtrans-ms
---hs-         6/25/2020   5:12 PM             20 ntuser.ini
~~~
___

### KEYS DE REGISTRO RUN Y RUNONCE

También existen las denominadas secciones de registro, que contienen un grupo lógico de keys, subkeys y valores para admitir software y archivos cargados en la memoria cuando se inicia el sistema operativo o un usuario inicia sesión. Estas secciones son útiles para mantener el acceso al sistema. Estas se denominan [keys de registro Run y RunOnce](https://docs.microsoft.com/en-us/windows/win32/setupapi/run-and-runonce-registry-keys).

El registro de Windows incluye las siguientes cuatro keys:

~~~
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
~~~
Este es un ejemplo de la key `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run` mientras está conectado a un sistema.

~~~
PS C:\htb> reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
    SecurityHealth    REG_EXPAND_SZ    %windir%\system32\SecurityHealthSystray.exe
    RTHDVCPL    REG_SZ    "C:\Program Files\Realtek\Audio\HDA\RtkNGUI64.exe" -s
    Greenshot    REG_SZ    C:\Program Files\Greenshot\Greenshot.exe
~~~

Aquí hay un ejemplo de `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run` que muestra las aplicaciones que se ejecutan bajo el usuario actual mientras está conectado a un sistema.

~~~
PS C:\htb> reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
    OneDrive    REG_SZ    "C:\Users\bob\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background
    OPENVPN-GUI    REG_SZ    C:\Program Files\OpenVPN\bin\openvpn-gui.exe
    Docker Desktop    REG_SZ    C:\Program Files\Docker\Docker\Docker Desktop.exe
~~~
___

### LISTA BLANCA DE APLICACIONES (WHITELISTING)

Una lista blanca de aplicaciones es una lista de aplicaciones de software aprobadas o ejecutables que pueden estar presentes y ejecutarse en un sistema. El objetivo es proteger el entorno de malware dañino y software no aprobado que no se alinea con las necesidades comerciales específicas de una organización. La implementación de una lista blanca forzada puede ser un desafío, especialmente en una red grande. Una organización debe implementar una lista blanca en modo de auditoría inicialmente para asegurarse de que todas las aplicaciones necesarias estén en la lista blanca y no estén bloqueadas por un error de omisión, lo que puede causar más problemas de los que soluciona.

La lista negra, por el contrario, especifica una lista de software o aplicaciones dañinos o no permitidos para bloquear, y todos los demás pueden ejecutarse e instalarse. La inclusión en la lista blanca se basa en un principio de "confianza cero" en el que todo el software o las aplicaciones se consideran "malos", excepto aquellos específicamente permitidos. El mantenimiento de una lista blanca generalmente se escucha menos, ya que un administrador del sistema solo necesitará especificar qué está permitido y no actualizar constantemente una "lista negra" con nuevas aplicaciones maliciosas.

Organizaciones como [NIST](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-167.pdf) recomiendan la inclusión en la lista blanca, especialmente en entornos de alta seguridad.
___

### APPLOCKER

[AppLocker](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview) es la solución de lista blanca de aplicaciones de Microsoft y se introdujo por primera vez en Windows 7. `AppLocker` brinda a los administradores del sistema control sobre qué aplicaciones y archivos pueden ejecutar los usuarios. Brinda control granular sobre ejecutables, scripts, archivos de instalación de Windows, DLL, aplicaciones empaquetadas e instaladores de aplicaciones empaquetadas.

Permite crear reglas basadas en atributos de archivo, como el nombre del editor (que puede derivarse de la firma digital), el nombre del producto, el nombre del archivo y la versión. Las reglas también se pueden configurar en función de rutas de archivos y hashes. Las reglas se pueden aplicar a grupos de seguridad o usuarios individuales, según la necesidad empresarial. `AppLocker` se puede implementar primero en modo de auditoría para probar el impacto antes de aplicar todas las reglas.
___

### POLITICAS DE GRUPO LOCAL

La política de grupo permite a los administradores establecer, configurar y ajustar una variedad de configuraciones. En un entorno de dominio, las políticas de grupo se envían desde un controlador de dominio a todas las máquinas unidas al dominio a las que están vinculados los objetos de política de grupo (GPO). Estas configuraciones también se pueden definir en máquinas individuales usando la `Política de Grupo Local`.

La Politica de Grupo se puede configurar localmente, tanto en entornos de dominio como en entornos sin dominio. La Política de Grupo Local se puede usar para modificar ciertas configuraciones gráficas y de red a las que de otro modo no se puede acceder a través del Panel de control. También se puede usar para bloquear una política de computadora individual con configuraciones de seguridad estrictas, como permitir solo la instalacióny ejecución de ciertos programas o hacer cumplir requisitos estrictos de contraseña de cuenta de usuario.

Podemos abrir el Editor de Politicas de Grupo Local abriendo el menú Inicio y escribiendo `gpedit.msc`. El editor se divide en dos categorías bajo Política de Equipo Local: `Configuración del equipo` y `Configuración del usuario`.

![](https://academy.hackthebox.com/storage/modules/49/Local_GP.png)

Por ejemplo, podemos abrir la `Política de Equipo Local` para habilitar Credential Guard habilitando la configuración `Turn On Virtualization Based Security`. Credential Guard es una característica de Windows 10 que protege contra ataques de robo de credenciales al aislar el proceso `LSA` del sistema operativo.

![](https://academy.hackthebox.com/storage/modules/49/credguard.png)

También podemos habilitar la auditoría de cuentas ajustada y configurar `AppLocker` desde el Editor de Políticas de Grupo Local. Vale la pena explorar la Política de Grupo Local y conocer la amplia variedad de formas en que se puede usar para bloquear un sistema Windows.
___
### ANTIVIRUS WINDOWS DEFENDER

Windows Defender Antivirus (Defender), anteriormente conocido como Windows Defender, es un antivirus integrado que se instala de forma gratuita con los sistemas operativos Windows. Se lanzó por primera vez como una herramienta antispyware descargable para Windows XP y Server 2003. Defender comenzó a venir preempaquetado como parte del sistema operativo con Windows Vista y Server 2008. El programa cambió su nombre a Windows Defender Antivirus con Windows 10 Creators Update.

`Defender` viene con varias funciones, como protección en tiempo real, que protege el dispositivo de amenazas conocidas en tiempo real y protección proporcionada por la nube, que funciona junto con el envío automático de muestras para cargar archivos sospechosos para su análisis. Cuando los archivos se envían al servicio de protección en la nube, se "`bloquean`" para evitar cualquier comportamiento potencialmente malicioso hasta que se complete el análisis. Otra característica es la protección contra manipulaciones, que evita que la configuración de seguridad se cambie a través del Registro, los `cmdlets` de PowerShell o la Política de Grupo.

Windows Defender se administra desde el Centro de Seguridad, desde el cual se pueden habilitar y administrar una variedad de funciones y configuraciones de seguridad adicionales.

![](https://academy.hackthebox.com/storage/modules/49/Defender_sec_center.png)

La configuración de protección en tiempo real se puede ajustar para agregar archivos, carpetas y áreas de memoria para controlar el acceso a las carpetas para evitar cambios no autorizados. También podemos agregar archivos o carpetas a una lista de exclusión, para que no sean escaneados. Un ejemplo sería excluir del análisis una carpeta de herramientas utilizadas para pruebas de penetración, ya que se marcarán como maliciosas y se pondrán en cuarentena o se eliminarán del sistema. El acceso controlado a carpetas es la protección contra ransomware integrada de Defender.

Podemos usar el cmdlet `Get-MpComputerStatus` de PowerShell para verificar qué configuraciones de protección están habilitadas.

~~~
PS C:\htb> Get-MpComputerStatus | findstr "True"
AMServiceEnabled                : True
AntispywareEnabled              : True
AntivirusEnabled                : True
BehaviorMonitorEnabled          : True
IoavProtectionEnabled           : True
IsTamperProtected               : True
NISEnabled                      : True
OnAccessProtectionEnabled       : True
RealTimeProtectionEnabled       : True
~~~

Si bien ninguna solución antivirus es perfecta, Windows Defender se desempeña muy bien en las pruebas mensuales de tasa de detección en comparación con otras soluciones, incluso las pagas. Además, dado que viene preinstalado como parte del sistema operativo, no introduce "inflación" en el sistema, como otros programas que agregan extensiones de navegador y rastreadores. Se sabe que otros productos ralentizan el sistema debido a la forma en que se conectan al sistema operativo.

Windows Defender no está exento de fallas y debe ser parte de una estrategia de defensa en profundidad basada en los principios básicos de configuración y administración de parches, no tratado como una panacea para proteger nuestros sistemas. Las definiciones se actualizan constantemente y las nuevas versiones de Windows Defender están integradas en las principales versiones operativas, como Windows 10, versión 1909, que es la versión más reciente en el momento de escribir este artículo.

Windows Defender recogerá `payloads` útiles de `frameworks` comunes de código abierto como `Metasploit` o versiones inalteradas de herramientas como `Mimikatz`.

![](https://academy.hackthebox.com/storage/modules/49/meterp_caught.png)

Aunque cada vez es más difícil, todavía es posible eludir por completo las protecciones de Windows Defender impuestas por la última versión con las definiciones más actualizadas instaladas.
___
### RETO

+ Busque el SID del usuario bob.smith.

`R: S-1-5-21-2614195641-1726409526-3792725429-1003`

Por CMD
~~~
C:\WINDOWS\system32>wmic useraccount where name="bob.smith" get sid
SID
S-1-5-21-2614195641-1726409526-3792725429-1003
~~~

Por PowerShell de forma extensa
~~~
PS C:\users> get-wmiobject win32_useraccount


AccountType : 512
Caption     : WS01\Administrator
Domain      : WS01
SID         : S-1-5-21-2614195641-1726409526-3792725429-500
FullName    :
Name        : Administrator

AccountType : 512
Caption     : WS01\bob.smith
Domain      : WS01
SID         : S-1-5-21-2614195641-1726409526-3792725429-1003
FullName    :
Name        : bob.smith
~~~

Por PowerShell un poco mas directa
~~~
PS C:\users> get-wmiobject win32_useraccount | select name,sid

name               sid
----               ---
Administrator      S-1-5-21-2614195641-1726409526-3792725429-500
bob.smith          S-1-5-21-2614195641-1726409526-3792725429-1003
DefaultAccount     S-1-5-21-2614195641-1726409526-3792725429-503
defaultuser0       S-1-5-21-2614195641-1726409526-3792725429-1000
Guest              S-1-5-21-2614195641-1726409526-3792725429-501
htb-student        S-1-5-21-2614195641-1726409526-3792725429-1002
mrb3n              S-1-5-21-2614195641-1726409526-3792725429-1001
WDAGUtilityAccount S-1-5-21-2614195641-1726409526-3792725429-504
~~~
___
+ ¿Qué aplicación de seguridad de terceros está deshabilitada al inicio para el usuario actual? (La respuesta distingue entre mayúsculas y minúsculas).

`R: NordVPN`

~~~
PS C:\WINDOWS\system32> reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
    OneDrive    REG_SZ    "C:\Users\htb-student\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background
    NordVPN    REG_SZ    C:\Program Files\NordVPN\NordVPN.exe
~~~
___











