## INSTRUMENTACION DE AMINISTRACION DE WINDOWS (WMI)

WMI es un subsistema de PowerShell que brinda a los administradores de sistemas herramientas poderosas para monitorear el sistema. El objetivo de WMI es consolidar la gestión de dispositivos y aplicaciones en las redes corporativas. WMI es una parte central del sistema operativo Windows y viene preinstalado desde Windows 2000. Está formado por los siguientes componentes:

|Componente|Descripcion|
|--|--|
|WMI service|El proceso de instrumentación de administración de Windows, que se ejecuta automáticamente al arrancar y actúa como intermediario entre los proveedores de WMI, el repositorio de WMI y las aplicaciones de administración.|
|Managed objects|Cualquier componente lógico o físico que WMI pueda administrar.|
|WMI providers|Objetos que monitorean eventosy datos relacionados con un objeto específico.|
|Classes|Estos son utilizados por los proveedores de WMI para pasar datos al servicio WMI.|
|Methods|Estos se adjuntan a las clases y permiten realizar acciones. Por ejemplo, se pueden usar métodos para iniciar y detener procesos en máquinas remotas.|
|WMI repository|Una base de datos que almacena todos los datos estáticos relacionados con WMI.|
|CMI Object Manager|El sistema que solicita datos de los proveedores de WMI y los devuelve a la aplicación que los solicita.|
|WMI API|Permite que las aplicaciones accedan a la infraestructura de WMI.|
|WMI Consumer|Envía consultas a los objetos a través del Administrador de objetos CMI.|

Algunos de los usos de WMI son:

+ Información de estado para sistemas locales y remotos
+ Configuración de ajustes de seguridad en máquinas y aplicaciones remotas
+ Configuración y cambio de permisos de usuarios y grupos
+ Configuración o modificación de las propiedades del sistema
+ Ejecución de código
+ Programación de procesos
+ Configuración del registro

Todas estas tareas se pueden realizar mediante una combinación de PowerShell y la interfaz de línea de comandos de WMI (WMIC). WMI se puede ejecutar a través del símbolo del sistema de Windows escribiendo `WMIC` para abrir un shell interactivo o ejecutando un comando directamente, como `wmic computersystem get name` para obtener el nombre de host. Podemos ver una lista de comandos y alias de WMIC escribiendo `WMIC /?`.

~~~
C:\htb> wmic /?

WMIC is deprecated.

[global switches] <command>

The following global switches are available:
/NAMESPACE           Path for the namespace the alias operate against.
/ROLE                Path for the role containing the alias definitions.
/NODE                Servers the alias will operate against.
/IMPLEVEL            Client impersonation level.
/AUTHLEVEL           Client authentication level.
/LOCALE              Language id the client should use.
/PRIVILEGES          Enable or disable all privileges.
/TRACE               Outputs debugging information to stderr.
/RECORD              Logs all input commands and output.
/INTERACTIVE         Sets or resets the interactive mode.
/FAILFAST            Sets or resets the FailFast mode.
/USER                User to be used during the session.
/PASSWORD            Password to be used for session login.
/OUTPUT              Specifies the mode for output redirection.
/APPEND              Specifies the mode for output redirection.
/AGGREGATE           Sets or resets aggregate mode.
/AUTHORITY           Specifies the <authority type> for the connection.
/?[:<BRIEF|FULL>]    Usage information.

For more information on a specific global switch, type: switch-name /?

Press any key to continue, or press the ESCAPE key to stop
~~~

El siguiente ejemplo de comando enumera información sobre el sistema operativo.

~~~
C:\htb> wmic os list brief

BuildNumber  Organization  RegisteredUser  SerialNumber             SystemDirectory      Version
19041                      Owner           00123-00123-00123-AAOEM  C:\Windows\system32  10.0.19041
~~~

WMIC utiliza alias y verbos, adverbios e interruptores asociados. El ejemplo de comando anterior usa `LIST` para mostrar datos y el adverbio `BRIEF` para proporcionar solo el conjunto básico de propiedades. Una lista detallada de verbos, interruptores y adverbios está disponible [aquí](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmic). WMI se puede usar con PowerShell mediante el [módulo](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) `Get-WmiObject`. Este módulo se utiliza para obtener instancias de clases WMI o información sobre las clases disponibles. Este módulo se puede utilizar contra máquinas locales o remotas.

Aquí podemos obtener información sobre el sistema operativo.

~~~
PS C:\htb> Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumber,Version | ft

SystemDirectory     BuildNumber SerialNumber            Version
---------------     ----------- ------------            -------
C:\Windows\system32 19041       00123-00123-00123-AAOEM 10.0.19041
~~~

También podemos usar el [módulo](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/invoke-wmimethod?view=powershell-5.1) `Invoke-WmiMethod`, que se usa para llamar a los métodos de los objetos WMI. Un ejemplo simple es cambiar el nombre de un archivo. Podemos ver que el comando se completó correctamente porque `ReturnValue` está establecido en 0.

~~~
PS C:\htb> Invoke-WmiMethod -Path "CIM_DataFile.Name='C:\users\public\spns.csv'" -Name Rename -ArgumentList "C:\Users\Public\kerberoasted_users.csv"


__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 0
PSComputerName   :
~~~

Esta sección proporciona una breve descripción general de `WMI`, `WMIC` y la combinación de `WMIC` y `PowerShell`. `WMI` tiene una amplia variedad de usos para los operadores del equipo azul y del equipo rojo. Las secciones posteriores de este curso mostrarán algunas formas en que `WMI` puede aprovecharse ofensivamente tanto para la enumeración como para el movimiento lateral.
___
### RETO

+ Utilice WMI para encontrar el número de serie del sistema.

`R: 00329-10280-00000-AA938`

~~~
PS C:\Users\htb-student> wmic os list brief
BuildNumber  Organization  RegisteredUser  SerialNumber             SystemDirectory      Version
19041                      mrb3n           00329-10280-00000-AA938  C:\WINDOWS\system32  10.0.19041
~~~