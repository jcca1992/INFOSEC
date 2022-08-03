## INTERACTUANDO CON EL SO WINDOWS

### **INTERFAZ GRAFICA DE USUARIO (GUI)**

El concepto de una interfaz gráfica de usuario (GUI) fue introducido a fines de la década de 1970 por el laboratorio de investigación de Xerox Palo Alto. Se agregó a los sistemas operativos de Apple y Microsoft para abordar las preocupaciones de usabilidad de los usuarios cotidianos que probablemente tendrían dificultades para navegar por la línea de comandos. La mayoría de los usuarios ocasionales de computadoras con Windows nunca necesitan interactuar con el sistema operativo a través de la línea de comandos. Como su nombre indica, una GUI proporciona a los usuarios una interfaz interactiva de apuntar y hacer clic para interactuar con el sistema operativo y las aplicaciones y servicios instalados.

La introducción de la GUI abrió un atractivo generalizado y el acceso a las computadoras en muchos grupos demográficos, ya que los usuarios podrían interactuar con su computadora sin tener que memorizar comandos o conocer ningún lenguaje de programación. Los administradores de sistemas suelen utilizar sistemas basados en GUI para administrar `Active Directory`, configurar `IIS` o interactuar con bases de datos.
___

### **PROTOCOLO DE ESCRITORIO REMOTO (RDP)**

[RDP](https://support.microsoft.com/en-us/help/186607/understanding-the-remote-desktop-protocol-rdp) es un protocolo propietario de Microsoft que permite a un usuario conectarse a un sistema remoto a través de una conexión de red y obtener una interfaz gráfica de usuario. El usuario se conecta mediante el software de cliente `RDP` a un sistema de destino que ejecuta el software de servidor RDP. `RDP` usa el `puerto 3389` para abrir un canal de red dedicado para enviar datos de un lado a otro. Cuando se conecta a través de `RDP`, un usuario puede acceder a la `GUI` como si estuviera realmente sentado frente a la computadora e iniciando sesión localmente. Los administradores de sistemas suelen utilizar `RDP` para administrar rápidamente sistemas remotos. También puede permitir a los usuarios acceder a las computadoras de su trabajo cuando viajan o trabajan desde casa después de conectarse a una `red privada virtual` (`VPN`).
___
### **LINEA DE COMANDOS DE WINDOWS**

Las interfaces de línea de comandos brindan a los usuarios un mayor control sobre sus sistemas y se pueden usar para realizar una amplia variedad de tareas administrativas y de resolución de problemas del día a día. Se puede aprovechar para introducir la automatización para realizar ciertas tareas rápidamente (como agregar muchos usuarios a un dominio a la vez). En los sistemas operativos Windows, las dos formas principales de interactuar con el sistema desde la línea de comandos son mediante el `símbolo del sistema` (`CMD`) y `PowerShell`.

La [Referencia de comandos de Windows](https://download.microsoft.com/download/5/8/9/58911986-D4AD-4695-BF63-F734CD4DF8F2/ws-commands.pdf) de Microsoft es una referencia completa de comandos de la A a la Z que incluye una descripción general, ejemplos de uso y sintaxis de comandos para la mayoría de los comandos de Windows, y se recomienda familiarizarse con ella.
___

### **CMD**

El símbolo del sistema (`cmd.exe`) se utiliza para ingresar y ejecutar comandos. Un usuario puede ingresar comandos únicos, como `ipconfig`, para ver la información de la dirección IP o realizar tareas más avanzadas, como configurar tareas programadas o crear secuencias de comandos y archivos por lotes. El símbolo del sistema se puede abrir desde el menú Inicio, escribiendo `cmd` en el cuadro de diálogo de ejecución, o ejecutando directamente el binario desde `C:\Windows\system32\cmd.exe`.

Después de ejecutar `cmd.exe`, podemos escribir `help` para ver una lista de los comandos disponibles.

~~~
C:\htb> help
For more information on a specific command, type HELP command-name
ASSOC          Displays or modifies file extension associations.
ATTRIB         Displays or changes file attributes.
BREAK          Sets or clears extended CTRL+C checking.
BCDEDIT        Sets properties in boot database to control boot loading.
CACLS          Displays or modifies access control lists (ACLs) of files.
CALL           Calls one batch program from another.
CD             Displays the name of or changes the current directory.
CHCP           Displays or sets the active code page number.
CHDIR          Displays the name of or changes the current directory.
CHKDSK         Checks a disk and displays a status report.
CHKNTFS        Displays or modifies the checking of disk at boot time.
CLS            Clears the screen.
CMD            Starts a new instance of the Windows command interpreter.
COLOR          Sets the default console foreground and background colors.
COMP           Compares the contents of two files or sets of files.
COMPACT        Displays or alters the compression of files on NTFS partitions.
CONVERT        Converts FAT volumes to NTFS.  You cannot convert the
               current drive.
COPY           Copies one or more files to another location.

<SNIP>
~~~

Para obtener más información sobre un comando específico, podemos escribir `help <nombre del comando>`.

~~~
C:\htb> help schtasks

SCHTASKS /parameter [arguments]

Description:
    Enables an administrator to create, delete, query, change, run and
    end scheduled tasks on a local or remote system.

Parameter List:
    /Create         Creates a new scheduled task.

    /Delete         Deletes the scheduled task(s).

    /Query          Displays all scheduled tasks.

    /Change         Changes the properties of scheduled task.

    /Run            Runs the scheduled task on demand.

    /End            Stops the currently running scheduled task.

    /ShowSid        Shows the security identifier corresponding to a scheduled task name.

    /?              Displays this help message.

Examples:
    SCHTASKS
    SCHTASKS /?
    SCHTASKS /Run /?
    SCHTASKS /End /?
    SCHTASKS /Create /?
    SCHTASKS /Delete /?
    SCHTASKS /Query  /?
    SCHTASKS /Change /?
    SCHTASKS /ShowSid /?
~~~

Tenga en cuenta que ciertos comandos tienen sus propios menús de ayuda, a los que se puede acceder escribiendo `<comando> /?`. Por ejemplo, la información sobre el comando `ipconfig` se puede ver a continuación.

~~~
C:\htb> ipconfig /?

USAGE:
    ipconfig [/allcompartments] [/? | /all |
                                 /renew [adapter] | /release [adapter] |
                                 /renew6 [adapter] | /release6 [adapter] |
                                 /flushdns | /displaydns | /registerdns |
                                 /showclassid adapter |
                                 /setclassid adapter [classid] |
                                 /showclassid6 adapter |
                                 /setclassid6 adapter [classid] ]

where
    adapter             Connection name
                       (wildcard characters * and ? allowed, see examples)

    Options:
       /?               Display this help message
       /all             Display full configuration information.
       /release         Release the IPv4 address for the specified adapter.
       /release6        Release the IPv6 address for the specified adapter.
       /renew           Renew the IPv4 address for the specified adapter.
       /renew6          Renew the IPv6 address for the specified adapter.
       /flushdns        Purges the DNS Resolver cache.
       /registerdns     Refreshes all DHCP leases and re-registers DNS names
       /displaydns      Display the contents of the DNS Resolver Cache.
       /showclassid     Displays all the dhcp class IDs allowed for adapter.
       /setclassid      Modifies the dhcp class id.
       /showclassid6    Displays all the IPv6 DHCP class IDs allowed for adapter.
       /setclassid6     Modifies the IPv6 DHCP class id.

<SNIP
~~~
___
### **POWERSHELL**

Windows PowerShell es un shell de comandos diseñado por Microsoft para estar más orientado a los administradores de sistemas. PowerShell, al igual que la línea de comandos de Windows, tiene un símbolo del sistema interactivo, así como un potente entorno de secuencias de comandos. PowerShell se basa en `.NET Framework`, que se utiliza para crear y ejecutar aplicaciones en Windows. Esto lo convierte en una herramienta muy poderosa para interactuar directamente con el sistema operativo.

Al igual que el símbolo del sistema, PowerShell nos brinda acceso directo al sistema de archivos y ejecutamos la mayoría de los mismos comandos que podemos dentro de un shell de cmd.
___

### **CMDLETS**

PowerShell utiliza `cmdlets`, que son pequeñas herramientas de una sola función integradas en el shell. Hay más de 100 cmdlets principales y se han escrito muchos adicionales, o podemos crear los nuestros para realizar tareas más complejas. PowerShell también admite secuencias de comandos simples y complejas que se utilizan para tareas de administración del sistema, automatización y más.

Los cmdlets tienen la forma de `verbo-sustantivo`. Por ejemplo, el comando `Get-ChildItem` se puede usar para listar nuestro directorio actual. Los `cmdlets` también aceptan argumentos o indicadores. Podemos escribir `Get-ChildItem` y presionar la tecla `tab` para recorrer los argumentos. Un comando como `Get-ChildItem -Recurse` nos mostrará el contenido de nuestro directorio de trabajo actual y todos los subdirectorios. Otro ejemplo sería `Get-ChildItem -Path C:\Users\Administrator\Documents` para obtener el contenido de otro directorio. Finalmente, podemos combinar argumentos como este para listar el contenido de todos los subdirectorios dentro de otro directorio recursivamente: `Get-ChildItem -Path C:\Users\Administrator\Downloads -Recurse`.

### **ALIAS**

Muchos cmdlets en PowerShell también tienen alias. Por ejemplo, los alias del cmdlet `Set-Location`, para cambiar de directorio, son `cd` o `sl`. Mientras tanto, los alias de `Get-ChildItem` son `ls` y `gci`. Podemos ver todos los alias disponibles escribiendo `Get-Alias`.

~~~
PS C:\htb> get-alias

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           % -> ForEach-Object
Alias           ? -> Where-Object
Alias           ac -> Add-Content
Alias           asnp -> Add-PSSnapin
Alias           cat -> Get-Content
Alias           cd -> Set-Location
Alias           CFS -> ConvertFrom-String                          3.1.0.0    Microsoft.PowerShell.Utility
Alias           chdir -> Set-Location
Alias           clc -> Clear-Content
Alias           clear -> Clear-Host
Alias           clhy -> Clear-History
Alias           cli -> Clear-Item
Alias           clp -> Clear-ItemProperty
~~~

También podemos configurar nuestros propios alias con `New-Alias` y obtener el alias para cualquier cmdlet con` Get-Alias -Name`.

~~~
PS C:\htb> New-Alias -Name "Show-Files" Get-ChildItem
PS C:\> Get-Alias -Name "Show-Files"

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Show-Files
~~~

PowerShell tiene un sistema de ayuda para cmdlets, funciones, scripts y conceptos. Esto no está instalado de manera predeterminada, pero podemos ejecutar el comando `Get-Help <cmdlet-name> -Online` para abrir la ayuda en línea para un cmdlet o una función en nuestro navegador web. Podemos escribir `Update-Help` para descargar e instalar archivos de ayuda localmente.

~~~
PS C:\htb> help

TOPIC
    Windows PowerShell Help System

SHORT DESCRIPTION
    Displays help about Windows PowerShell cmdlets and concepts.

LONG DESCRIPTION
    Windows PowerShell Help describes Windows PowerShell cmdlets,
    functions, scripts, and modules, and explains concepts, including
    the elements of the Windows PowerShell language.

    Windows PowerShell does not include help files, but you can read the
    help topics online, or use the Update-Help cmdlet to download help files
    to your computer and then use the Get-Help cmdlet to display the help
    topics at the command line.

    You can also use the Update-Help cmdlet to download updated help files
    as they are released so that your local help content is never obsolete.

    Without help files, Get-Help displays auto-generated help for cmdlets,
    functions, and scripts.


  ONLINE HELP
    You can find help for Windows PowerShell online in the TechNet Library
    beginning at http://go.microsoft.com/fwlink/?LinkID=108518.

    To open online help for any cmdlet or function, type:

        Get-Help <cmdlet-name> -Online
		
<SNIP>
~~~

Escribir un comando como `Get-Help Get-AppPackage` devolverá solo la ayuda parcial a menos que los archivos de ayuda estén instalados.

~~~
PS C:\htb>  Get-Help Get-AppPackage

NAME
    Get-AppxPackage

SYNTAX
    Get-AppxPackage [[-Name] <string>] [[-Publisher] <string>] [-AllUsers] [-PackageTypeFilter {None | Main |
    Framework | Resource | Bundle | Xap | Optional | All}] [-User <string>] [-Volume <AppxVolume>]
    [<CommonParameters>]


ALIASES
    Get-AppPackage


REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.

~~~

### **EJECUCION DE SCRIPTS**

`PowerShell` ISE (Integrated Scripting Environment-entorno de secuencias de comandos integrado) permite a los usuarios escribir secuencias de comandos de PowerShell sobre la marcha. También tiene una función de autocompletar o búsqueda para los comandos de PowerShell. PowerShell ISE nos permite escribir y ejecutar scripts en la misma consola, lo que permite una depuración rápida.

Podemos ejecutar scripts de PowerShell de varias maneras. Si conocemos las funciones, podemos ejecutar el script localmente o después de cargarlo en la memoria con una base de descarga como el siguiente ejemplo.

~~~
PS C:\htb> .\PowerView.ps1;Get-LocalGroup |fl

Description     : Users of Docker Desktop
Name            : docker-users
SID             : S-1-5-21-674899381-4069889467-2080702030-1004
PrincipalSource : Local
ObjectClass     : Group

Description     : VMware User Group
Name            : __vmware__
SID             : S-1-5-21-674899381-4069889467-2080702030-1003
PrincipalSource : Local
ObjectClass     : Group

Description     : Members of this group can remotely query authorization attributes and permissions for resources on
                  this computer.
Name            : Access Control Assistance Operators
SID             : S-1-5-32-579
PrincipalSource : Local
ObjectClass     : Group

Description     : Administrators have complete and unrestricted access to the computer/domain
Name            : Administrators
SID             : S-1-5-32-544
PrincipalSource : Local

<SNIP>
~~~

Una forma común de trabajar con un script en PowerShell es importarlo para que todas las funciones estén disponibles en nuestra sesión actual de la consola de PowerShell: `Import-Module .\PowerView.ps1.` Luego podemos iniciar un comando y recorrer las opciones o escribir `Get-Module` para enumerar todos los módulos cargados y sus comandos asociados.

~~~
PS C:\htb> Get-Module | select Name,ExportedCommands | fl


Name             : Appx
ExportedCommands : {[Add-AppxPackage, Add-AppxPackage], [Add-AppxVolume, Add-AppxVolume], [Dismount-AppxVolume,
                   Dismount-AppxVolume], [Get-AppxDefaultVolume, Get-AppxDefaultVolume]...}

Name             : Microsoft.PowerShell.LocalAccounts
ExportedCommands : {[Add-LocalGroupMember, Add-LocalGroupMember], [Disable-LocalUser, Disable-LocalUser],
                   [Enable-LocalUser, Enable-LocalUser], [Get-LocalGroup, Get-LocalGroup]...}

Name             : Microsoft.PowerShell.Management
ExportedCommands : {[Add-Computer, Add-Computer], [Add-Content, Add-Content], [Checkpoint-Computer,
                   Checkpoint-Computer], [Clear-Content, Clear-Content]...}

Name             : Microsoft.PowerShell.Utility
ExportedCommands : {[Add-Member, Add-Member], [Add-Type, Add-Type], [Clear-Variable, Clear-Variable], [Compare-Object,
                   Compare-Object]...}

Name             : PSReadline
ExportedCommands : {[Get-PSReadLineKeyHandler, Get-PSReadLineKeyHandler], [Get-PSReadLineOption,
                   Get-PSReadLineOption], [Remove-PSReadLineKeyHandler, Remove-PSReadLineKeyHandler],
                   [Set-PSReadLineKeyHandler, Set-PSReadLineKeyHandler]...}
~~~
___

### **POLITICAS DE EJECUCION**

A veces nos encontraremos con que no podemos ejecutar scripts en un sistema. Esto se debe a una función de seguridad que intenta evitar la ejecución de scripts maliciosos denominada política de ejecución. Las posibles políticas son:

|Politica|Descripcion|
|--|--|
|`AllSigned`|Todos los scripts pueden ejecutarse, pero un editor de confianza debe firmar los scripts y los archivos de configuración. Esto incluye scripts remotos y locales. Recibimos un aviso antes de ejecutar secuencias de comandos firmadas por editores que aún no hemos enumerado como confiables o no confiables|
|`Bypass`|No se bloquean scripts ni archivos de configuración, y el usuario no recibe advertencias ni avisos.|
|`Default`|Esto establece la política de ejecución predeterminada, `Restricted` para equipos de escritorio Windows y `RemoteSigned` para servidores Windows.|
|`RemoteSigned`|Los scripts pueden ejecutarse, pero requieren una firma digital en los scripts que se descargan de Internet. No se requieren firmas digitales para los scripts que se escriben localmente.|
|`Restricted`|Esto permite comandos individuales pero no permite que se ejecuten scripts. Se bloquean todos los tipos de archivos de script, incluidos los archivos de configuración (`.ps1xml`), los archivos de script de módulo (`.psm1`) y los perfiles de PowerShell (.`ps1`).|
|`Undefined`|No se establece ninguna política de ejecución para la extension actual. Si la política de ejecución para TODAS las extensiones se establece en indefinido, se utilizará la política de ejecución predeterminada de `Restricted`.|
|`Unrestricted`|Esta es la política de ejecución predeterminada para equipos que no son de Windows y no se puede cambiar. Esta política permite que se ejecuten secuencias de comandos sin firmar, pero advierte al usuario antes de ejecutar secuencias de comandos que no son de la zona de la intranet local.|

A continuación se muestra un ejemplo de la política de ejecución actual para todos los ámbitos.

~~~
PS C:\htb> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       Undefined
 LocalMachine    RemoteSigned
~~~

La política de ejecución no pretende ser un control de seguridad que restrinja las acciones del usuario. Un usuario puede omitir fácilmente la política escribiendo el contenido del script directamente en la ventana de PowerShell, descargando e invocando el script o especificando el script como un comando codificado. También se puede eludir ajustando la política de ejecución (si el usuario tiene los derechos adecuados) o configurando la política de ejecución para el alcance del proceso actual (lo que puede hacer casi cualquier usuario, ya que no requiere un cambio de configuración y solo será establecido para la duración de la sesión del usuario).

A continuación se muestra un ejemplo de cómo cambiar la política de ejecución para el proceso actual (sesión).

~~~
PS C:\htb> Set-ExecutionPolicy Bypass -Scope Process

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
~~~

Ahora podemos ver que la política de ejecución ha sido cambiada.

~~~
PS C:\htb>  Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process          Bypass
  CurrentUser       Undefined
 LocalMachine    RemoteSigned
~~~

¿Cuál es el alias establecido para el comando ipconfig.exe?

Busque el conjunto de políticas de ejecución para el ámbito de LocalMachine.