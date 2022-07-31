## SERVICIOS Y PROCESOS DE WINDOWS

### **SERVICIOS DE WINDOWS**

Los servicios son un componente importante del sistema operativo Windows. Permiten la creación y gestión de procesos de larga ejecución. Los servicios de Windows se pueden iniciar automáticamente al arrancar el sistema sin la intervención del usuario. Estos servicios pueden continuar ejecutándose en segundo plano incluso después de que el usuario cierre la sesión de su cuenta en el sistema.

También se pueden crear aplicaciones para instalarlas como un servicio, como una aplicación de monitoreo de red instalada en un servidor. Los servicios en Windows son responsables de muchas funciones dentro del sistema operativo Windows, como las funciones de red, realizar diagnósticos del sistema, administrar las credenciales de los usuarios, controlar las actualizaciones de Windows y más.

Los servicios de Windows se administran a través del sistema Administrador de control de servicios (SCM), accesible a través del complemento services.msc MMC.

Este complemento proporciona una interfaz GUI para interactuar y administrar servicios y muestra información sobre cada servicio instalado. Esta información incluye el nombre del servicio, la descripción, el estado, el tipo de inicio y el usuario con el que se ejecuta el servicio.

También es posible consultar y administrar servicios a través de la línea de comandos usando sc.exe usando cmdlets de PowerShell como Get-Service.