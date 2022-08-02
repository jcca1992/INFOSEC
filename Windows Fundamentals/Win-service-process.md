## SERVICIOS Y PROCESOS DE WINDOWS

### **SERVICIOS DE WINDOWS**

Los servicios son un componente importante del sistema operativo Windows. Permiten la creación y gestión de procesos de larga ejecución. Los servicios de Windows se pueden iniciar automáticamente al arrancar el sistema sin la intervención del usuario. Estos servicios pueden continuar ejecutándose en segundo plano incluso después de que el usuario cierre la sesión de su cuenta en el sistema.

También se pueden crear aplicaciones para instalarlas como un servicio, como una aplicación de monitoreo de red instalada en un servidor. Los servicios en Windows son responsables de muchas funciones dentro del sistema operativo Windows, como las funciones de red, realizar diagnósticos del sistema, administrar las credenciales de los usuarios, controlar las actualizaciones de Windows y más.

Los servicios de Windows se administran a través del sistema Administrador de control de servicios (SCM), accesible a través del complemento MMC `services.msc` .

Este complemento proporciona una interfaz GUI para interactuar y administrar servicios y muestra información sobre cada servicio instalado. Esta información incluye el nombre del servicio, la descripción, el estado, el tipo de inicio y el usuario con el que se ejecuta el servicio.

También es posible consultar y administrar servicios a través de la línea de comandos usando `sc.exe` usando cmdlets de [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7) como `Get-Service`.

~~~
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl


Name                : AdobeARMservice
DisplayName         : Adobe Acrobat Update Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : Appinfo
DisplayName         : Application Information
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {RpcSs, ProfSvc}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess, Win32ShareProcess
~~~

Los estados del servicio pueden aparecer como Running, Stopped o Paused, y se pueden configurar para que se inicien de forma manual, automática o con un retraso en el arranque del sistema. Los servicios también se pueden mostrar en el estado Starting o Stopping si alguna acción los ha activado para iniciarse o detenerse. Windows tiene tres categorías de servicios: Servicios locales, Servicios de red y Servicios del sistema. Por lo general, los servicios solo pueden ser creados, modificados y eliminados por usuarios con privilegios administrativos. Las configuraciones incorrectas en torno a los permisos de servicio son un vector común de escalada de privilegios en los sistemas Windows.

En Windows, tenemos algunos [servicios críticos del sistema](https://docs.microsoft.com/en-us/windows/win32/rstmgr/critical-system-services) que no se pueden detener y reiniciar sin reiniciar el sistema. Si actualizamos algún archivo o recurso en uso por uno de estos servicios, debemos reiniciar el sistema.

|Servicio|Descripcion|
|--|--|
|smss.exe|Subsistema Gestor de sesiones. Responsable del manejo de sesiones en el sistema.|
|csrss.exe|Proceso de tiempo de ejecución del servidor del cliente. La parte del modo de usuario del subsistema de Windows.|
|wininit.exe|Inicia el archivo Wininit.ini que enumera todos los cambios que se deben realizar en Windows cuando se reinicia la computadora después de instalar un programa.|
|logonui.exe|Se utiliza para facilitar el inicio de sesión del usuario en una PC|
|lsass.exe| El servidor de autenticación de seguridad local verifica la validez de los inicios de sesión de los usuarios en una PC o servidor. Genera el proceso encargado de autenticar a los usuarios para el servicio Winlogon.|
|services.exe|Gestiona la operación de iniciar y detener los servicios.|
|winlogon.exe|Responsable de manejar la secuencia de atención segura, cargar un perfil de usuario al iniciar sesión y bloquear la computadora cuando se ejecuta un protector de pantalla.|
|System|Un proceso del sistema en segundo plano que ejecuta el kernel de Windows.|
|svchost.exe with RPCSS|Administra los servicios del sistema que se ejecutan desde bibliotecas de vínculos dinámicos (archivos con la extensión .dll), como "Actualizaciones automáticas", "Firewall de Windows" y "Plug and Play". Utiliza el servicio de llamada a procedimiento remoto (RPC) (RPCSS).|
|svchost.exe with Dcom/PnP|Administra los servicios del sistema que se ejecutan desde bibliotecas de vínculos dinámicos (archivos con la extensión .dll), como "Actualizaciones automáticas", "Firewall de Windows" y "Plug and Play". Utiliza los servicios del modelo de objetos de componentes distribuidos (DCOM) y Plug and Play (PnP).|

Este [enlace](https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_components#Services) tiene una lista de los componentes de Windows, incluidos los servicios clave.
___

### **PROCESOS**

Los procesos se ejecutan en segundo plano en los sistemas Windows. Se ejecutan automáticamente como parte del sistema operativo Windows o las inician otras aplicaciones instaladas.

Los procesos asociados con las aplicaciones instaladas a menudo se pueden terminar sin causar un impacto severo en el sistema operativo. Ciertos procesos son críticos y, si se terminan, impedirán que ciertos componentes del sistema operativo funcionen correctamente. Algunos ejemplos incluyen Windows Logon Application, System, System Idle Process, Windows Start-Up Application, Client Server Runtime, Windows Session Manager, Service Host, el proceso Local Security Authority Subsystem Service (LSASS)
___

#### LOCAL SECURITY AUTHORITY SUBSYSTEM SERVICE (LSASS)

`lsass.exe` es el proceso responsable de hacer cumplir la política de seguridad en los sistemas Windows. Cuando un usuario intenta iniciar sesión en el sistema, este proceso verifica su intento de inicio de sesión y crea tokens de acceso basados en los niveles de permiso del usuario. LSASS también es responsable de los cambios de contraseña de la cuenta de usuario. Todos los eventos asociados con este proceso (intentos de inicio/cierre de sesión, etc.) se registran en el registro de seguridad de Windows. LSASS es un objetivo de valor extremadamente alto, ya que existen varias herramientas para extraer tanto el texto sin cifrar como las credenciales fragmentadas almacenadas en la memoria mediante este proceso.
___

### **SYSINTERNALS TOOLS**

La suite SysInternals Tools es un conjunto de aplicaciones portátiles de Windows que se pueden usar para administrar sistemas Windows (en su mayor parte sin necesidad de instalación). Las herramientas se pueden descargar desde el sitio web de Microsoft o cargándolas directamente desde un recurso compartido de archivos con acceso a Internet escribiendo \\live.sysinternals.com\tools en una ventana del Explorador de Windows.

Por ejemplo, podemos ejecutar `procdump.exe` directamente desde este recurso compartido sin descargarlo directamente al disco.

~~~
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula

ProcDump v9.0 - Sysinternals process dump utility
Copyright (C) 2009-2017 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

Monitors a process and writes a dump file when the process exceeds the
specified criteria or has an exception.

Capture Usage:
   procdump.exe [-mm] [-ma] [-mp] [-mc Mask] [-md Callback_DLL] [-mk]
                [-n Count]
                [-s Seconds]
                [-c|-cl CPU_Usage [-u]]
                [-m|-ml Commit_Usage]
                [-p|-pl Counter_Threshold]
                [-h]
                [-e [1 [-g] [-b]]]
                [-l]
                [-t]
                [-f  Include_Filter, ...]
                [-fx Exclude_Filter, ...]
                [-o]
                [-r [1..5] [-a]]
                [-wer]
                [-64]
                {
                 {{[-w] Process_Name | Service_Name | PID} [Dump_File | Dump_Folder]}
                |
                 {-x Dump_Folder Image_File [Argument, ...]}
                }
				
<SNIP>
~~~

El paquete incluye herramientas como `Process Explorer`, una versión mejorada de `Task Manager` y `Process Monitor`, que se pueden usar para monitorear el sistema de archivos, el registro y la actividad de la red relacionada con cualquier proceso que se ejecute en el sistema. Algunas herramientas adicionales son `TCPView`, que se usa para monitorear la actividad de Internet, y `PSExec`, que se puede usar para administrar y conectarse a sistemas a través del protocolo `SMB` de forma remota.

Estas herramientas pueden ser útiles para que los pentesters, por ejemplo, descubran procesos interesantes y posibles rutas de escalada de privilegios, así como para el movimiento lateral.
___
### **TASK MANAGER**

El Administrador de tareas de Windows es una poderosa herramienta para administrar sistemas Windows. Proporciona información sobre procesos en ejecución, rendimiento del sistema, servicios en ejecución, programas de inicio, usuarios registrados, procesos de usuarios registrados y servicios. El Administrador de tareas se puede abrir haciendo clic derecho en la barra de tareas y seleccionando Administrador de tareas, presionando ctrl + shift + Esc, presionando ctrl + alt + del y seleccionando Administrador de tareas, abriendo el menú de inicio y escribiendo Administrador de tareas, o escribiendo `taskmgr` desde un CMD o consola PowerShell.

![](https://academy.hackthebox.com/storage/modules/49/taskmgr.png)

|Tab|Descripcion|
|--|--|
|Processes|Muestra una lista de aplicaciones en ejecución y procesos en segundo plano junto con la CPU, la memoria, el disco, la red y el uso de energía para cada uno.|
|Performance|Muestra gráficos y datos como la utilización de la CPU, el tiempo de actividad del sistema, el uso de la memoria, el disco y las redes y el uso de la GPU. También podemos abrir el `Monitor de recursos`, que nos brinda una vista mucho más detallada del uso actual de recursos de CPU, memoria, disco y red.|

![](https://academy.hackthebox.com/storage/modules/49/resource_monitor.png)

|Tab|Descripcion|
|--|--|
|App History|Muestra el uso de recursos de la cuenta de usuario actual para cada aplicación durante un período de tiempo.|
|Startup|Muestra qué aplicaciones están configuradas para iniciarse en el arranque, así como el impacto en el proceso de inicio.|
|Users|Muestra los usuarios registrados y los procesos/uso de recursos asociados con su sesión.|
|Details|Muestra el nombre, el ID del proceso (PID), el estado, el nombre de usuario asociado, la CPU y el uso de la memoria para cada aplicación en ejecución.|
|Services|Muestra el nombre, PID, descripción y estado de cada servicio instalado. También se puede acceder al complemento Servicios desde esta pestaña.|
___

### **EXPLORADOR DE PROCESOS**

[Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) es parte del conjunto de herramientas de Sysinternals. Esta herramienta puede mostrar qué identificadores y procesos DLL se cargan cuando se ejecuta un programa. Process Explorer muestra una lista de los procesos que se están ejecutando actualmente y, a partir de ahí, podemos ver qué controles ha seleccionado el proceso en una vista o las DLL y los archivos de intercambio de memoria que se han cargado en otra vista. También podemos buscar dentro de la herramienta para mostrar qué procesos se relacionan con un identificador o DLL específico. La herramienta también se puede utilizar para analizar las relaciones entre procesos primarios y secundarios para ver qué procesos secundarios genera una aplicación y ayudar a solucionar cualquier problema, como los procesos huérfanos, que pueden quedar atrás cuando finaliza un proceso.
___

### RETO

+ Identifique uno de los servicios de actualización no estándar que se ejecutan en el host. Envíe el nombre completo del ejecutable del servicio (no el DisplayName) como su respuesta.

`R: FoxitReaderUpdateService.exe`


~~~
PS C:\WINDOWS\system32> Get-Service | ? {$_.Status -eq "Running"}

Status   Name               DisplayName
------   ----               -----------
Running  Appinfo            Application Information
Running  AudioEndpointBu... Windows Audio Endpoint Builder
Running  Audiosrv           Windows Audio
Running  BFE                Base Filtering Engine
Running  BITS               Background Intelligent Transfer Ser...
Running  BrokerInfrastru... Background Tasks Infrastructure Ser...
Running  Browser            Computer Browser
Running  BthAvctpSvc        AVCTP service
Running  cbdhsvc_652c8      cbdhsvc_652c8
Running  CDPSvc             Connected Devices Platform Service
Running  CDPUserSvc_652c8   CDPUserSvc_652c8
Running  CertPropSvc        Certificate Propagation
Running  ClipSVC            Client License Service (ClipSVC)
Running  COMSysApp          COM+ System Application
Running  CoreMessagingRe... CoreMessaging
Running  CryptSvc           Cryptographic Services
Running  DcomLaunch         DCOM Server Process Launcher
Running  Dhcp               DHCP Client
Running  DiagTrack          Connected User Experiences and Tele...
Running  DispBrokerDeskt... Display Policy Service
Running  Dnscache           DNS Client
Running  DPS                Diagnostic Policy Service
Running  DsmSvc             Device Setup Manager
Running  DusmSvc            Data Usage
Running  EventLog           Windows Event Log
Running  EventSystem        COM+ Event System
Running  FontCache          Windows Font Cache Service
Running  FoxitReaderUpda... Foxit Reader Update Service
Running  InstallService     Microsoft Store Install Service
Running  iphlpsvc           IP Helper
Running  KeyIso             CNG Key Isolation
Running  LanmanServer       Server
Running  LanmanWorkstation  Workstation
Running  LicenseManager     Windows License Manager Service
Running  lmhosts            TCP/IP NetBIOS Helper
Running  LSM                Local Session Manager
Running  mpssvc             Windows Defender Firewall
Running  MSDTC              Distributed Transaction Coordinator
Running  NcbService         Network Connection Broker
Running  netprofm           Network List Service
Running  NlaSvc             Network Location Awareness
Running  nsi                Network Store Interface Service
Running  OneSyncSvc_652c8   OneSyncSvc_652c8
Running  PlugPlay           Plug and Play
Running  Power              Power
Running  ProfSvc            User Profile Service
Running  RmSvc              Radio Management Service
Running  RpcEptMapper       RPC Endpoint Mapper
Running  RpcSs              Remote Procedure Call (RPC)
Running  SamSs              Security Accounts Manager
Running  ScDeviceEnum       Smart Card Device Enumeration Service
Running  Schedule           Task Scheduler
Running  SecurityHealthS... Windows Security Service
Running  SENS               System Event Notification Service
Running  SessionEnv         Remote Desktop Configuration
Running  SgrmBroker         System Guard Runtime Monitor Broker
Running  ShellHWDetection   Shell Hardware Detection
Running  Spooler            Print Spooler
Running  SSDPSRV            SSDP Discovery
Running  StateRepository    State Repository Service
Running  StorSvc            Storage Service
Running  SysMain            SysMain
Running  SystemEventsBroker System Events Broker
Running  TabletInputService Touch Keyboard and Handwriting Pane...
Running  TermService        Remote Desktop Services
Running  Themes             Themes
Running  TimeBrokerSvc      Time Broker
Running  TokenBroker        Web Account Manager
Running  TrkWks             Distributed Link Tracking Client
Running  UmRdpService       Remote Desktop Services UserMode Po...
Running  UserManager        User Manager
Running  UsoSvc             Update Orchestrator Service
Running  VaultSvc           Credential Manager
Running  VGAuthService      VMware Alias Manager and Ticket Ser...
Running  vm3dservice        VMware SVGA Helper Service
Running  VMTools            VMware Tools
Running  Wcmsvc             Windows Connection Manager
Running  WdiServiceHost     Diagnostic Service Host
Running  WdNisSvc           Microsoft Defender Antivirus Networ...
Running  WinDefend          Microsoft Defender Antivirus Service
Running  WinHttpAutoProx... WinHTTP Web Proxy Auto-Discovery Se...
Running  Winmgmt            Windows Management Instrumentation
Running  WpnService         Windows Push Notifications System S...
Running  WpnUserService_... WpnUserService_652c8
Running  wscsvc             Security Center
Running  WSearch            Windows Search
Running  wuauserv           Windows Update
~~~

![]()