## EXPERIENCIA DE ESCRITORIO VS SERVER CORE

[Windows Server Core](https://docs.microsoft.com/en-us/windows-server/administration/server-core/what-is-server-core) se lanzó por primera vez con Windows Server 2008 como un entorno de servidor minimalista que solo contenía la funcionalidad del servidor clave. Como resultado, Server Core tiene requisitos de administración más bajos, una superficie de ataque más pequeña y utiliza menos espacio en disco y memoria que su contraparte de experiencia de escritorio (GUI). En el núcleo del servidor, todas las tareas de configuración y mantenimiento se realizan a través de la línea de comandos, PowerShell o la administración remota con MMC o herramientas de administración de servidores remotos (`RSAT`).

Si bien `Server Core` tiene como objetivo tener una huella más pequeña al carecer de una GUI, algunos programas gráficos aún son compatibles, como el editor de registros, el bloc de notas, la información del sistema, el instalador de Windows, el administrador de tareas y el PowerShell. También admite algunas herramientas de suite `Sysinternals`, como `Active Directory Explorer`, `Process Explorer`, `Process Monitor` y `TCPView`.

A partir de Windows Server 2019, la experiencia de escritorio o Server Core debe seleccionarse en la instalación, y ninguno de los dos puede retroceder (es decir, convertir el Server Core en la experiencia de escritorio). Una vez instalado, la configuración inicial para el núcleo del servidor se puede realizar a través de `Sconfig`, que es una interfaz basada en texto (en realidad un VBScript ejecutado por WScript). `Sconfig` se utiliza para realizar una variedad de comandos comunes, como configurar redes, verificar e instalar actualizaciones de Windows, administración de cuentas, configuración de administración remota, activación de ventanas y más.

![](https://academy.hackthebox.com/storage/modules/49/sconfig.png)

Ciertas aplicaciones del servidor no pueden ejecutarse en Server Core, incluidos Microsoft Server Virtual Machine Manager 2019 (SCVMM), System Center Data Protection Manager 2019, SharePoint Server 2019, Project Server 2019.

En resumen, Server Core es más liviano y menos intensivo en recursos, pero tiene una curva de aprendizaje más pronunciada y puede ser más difícil de administrar. También tiene algunas limitaciones, como realizar tareas de gestión utilizando ciertos programas de GUI.

En cualquier entorno, la determinación entre el uso de la experiencia de escritorio o Server Core para la instalación de un servidor debe hacerse tanto por la necesidad comercial como por el uso previsto del servidor y el nivel de habilidad de los administradores responsables de mantenerlo. La siguiente tabla muestra algunas de las aplicaciones disponibles en la experiencia de Server Core vs. Desktop. Esta es una lista de aplicaciones comunes y no una lista exhaustiva.

|Aplicacion|Server Core| Desktop Experience|
|--|--|--|
|Command prompt|Available|Available|
|Windows PowerShell/ Microsoft .NET|Available|Available|
|Regedit|Available|Available|
|Diskmgmt.msc|Not Available|Available|
|Server Manager|Not Available|Available|
|Mmc.exe|Not Available|Available|
|Eventvwr|Not Available|Available|
|Services.msc|Not Available|Available|
|Control Panel|Not Available|Available|
|Windows Explorer|Not Available|Available|
|Taskmgr|Available|Available|
|Internet Explorer or Edge|Not Available|Available|
|Remote Desktop Services|Available|Available|