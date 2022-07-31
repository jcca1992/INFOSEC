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
|svchost.exe with RPCSS|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).|
|svchost.exe with Dcom/PnP|Administra los servicios del sistema que se ejecutan desde bibliotecas de vínculos dinámicos (archivos con la extensión .dll), como "Actualizaciones automáticas", "Firewall de Windows" y "Plug and Play". Utiliza los servicios del modelo de objetos de componentes distribuidos (DCOM) y Plug and Play (PnP).|