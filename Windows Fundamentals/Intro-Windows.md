Como pentesters, es importante tener conocimiento de una amplia variedad de tecnologías. Una comprensión profunda de los sistemas operativos Windows y Linux es beneficiosa en una amplia gama de tipos de evaluación. La mayoría de los sistemas que encontramos durante las evaluaciones, ya sea en las instalaciones o en la nube, se basarán en estos dos sistemas operativos. Es importante comprender cómo atacar y defender estos sistemas operativos y cómo se pueden usar como plataforma para realizar más actividades de pruebas de penetración.
___

### **EL SISTEMA OPERATIVO WINDOWS**

Microsoft presentó por primera vez el sistema operativo Windows el 20 de noviembre de 1985. La primera versión de Windows fue un shell de sistema operativo gráfico para MS-DOS. Las versiones posteriores de Windows Desktop introdujeron los programas Windows File Manager, Program Manager y Print Manager.

Windows 95 fue la primera integración completa de Windows y DOS y ofreció soporte de Internet integrado por primera vez. Esta versión también presentó el navegador web Internet Explorer. Desde la versión inicial, se han lanzado más de una docena de versiones de Windows, como Windows XP, Vista y 8, hasta la versión actual: Windows 10. Con el tiempo, Microsoft ha ofrecido varias ediciones de cada versión de escritorio de Windows para todos, desde consumidores ocasionales hasta clientes empresariales.

Windows Server se lanzó por primera vez en 1993 con el lanzamiento de Windows NT 3.1 Advanced Server. Windows NT experimentó varias actualizaciones a lo largo de los años, agregando tecnologías como Internet Information Services (IIS), varios protocolos de red, asistentes administrativos para facilitar las tareas de administración y más. Con el lanzamiento de Windows 2000, Microsoft debutó con Active Directory, originalmente destinado a ayudar a los administradores de sistemas a configurar el uso compartido de archivos, el cifrado de datos, VPN, etc. Windows Server 2000 también incluía Microsoft Management Console (MMC) y volúmenes de disco dinámico compatibles.

Luego vino Windows Server 2003 con roles de servidor, un firewall integrado, el olume Shadow Copy Service y más. Windows Server 2008 incluía clústeres de conmutación por error, software de virtualización Hyper-V, Server Core, Visor de eventos y mejoras importantes en Active Directory. A lo largo de los años, Microsoft lanzó más versiones de Server, incluidas Server 2012, Server 2016 y, más recientemente, Server 2019. Esta última versión agregó soporte para Kubernetes, contenedores de Linux y funciones de seguridad más avanzadas.

A medida que se introducen nuevas versiones de Windows, las versiones anteriores quedan obsoletas y ya no reciben actualizaciones de Microsoft (a menos que se compre un contrato de soporte a largo plazo en algunos casos). Windows Server 2008 y 2012 llegaron al final de su vida útil para las actualizaciones de seguridad el 14 de enero de 2020. Actualmente, solo Server 2012 R2 y versiones posteriores son compatibles. Sin embargo, Microsoft ha lanzado parches fuera de banda para versiones anteriores de Windows en los últimos años debido al descubrimiento de la vulnerabilidad crítica SMBv1 (EternalBlue).

Muchas versiones de Windows ahora se consideran "heredadas" y ya no son compatibles. Las organizaciones a menudo se encuentran ejecutando varios sistemas operativos más antiguos para admitir aplicaciones críticas o debido a preocupaciones operativas o presupuestarias. Un tester debe comprender las diferencias entre las versiones y las diversas configuraciones erróneas y vulnerabilidades inherentes a cada una.
___

### **VERSIONES DE WINDOWS**

| S.O | Numero de Version |
| -- | -- |
| Windows NT 4 | 4.0 |
| Windows 2000 | 5.0 |
| Windows XP | 5.1 |
| Windows Server 2003, 2003 R2 | 5.2 |
| Windows Vista, Server 2008 | 6.0 |
| Windows 7, Server 2008 R2 | 6.1 |
| Windows 8, Server 2012 | 6.2 |
| Windows 8.1, Server 2012 R2 |	6.3| 
| Windows 10, Server 2016, Server 2019 |10.0|

Podemos usar el comando [cmdlet](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7) o el comando [Get-WmiObject](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) para buscar información sobre el sistema operativo. Este cmdlet se puede usar para obtener instancias de clases de WMI o información sobre las clases de WMI disponibles. Hay una variedad de formas de encontrar la versión y el número de compilación de nuestro sistema. Podemos obtener fácilmente esta información utilizando la clase `win32_OperatingSystem`, que muestra que estamos en un host de Windows 10, número de compilación 19041.

~~~
PS C:\htb> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

Version    BuildNumber
-------    -----------
10.0.19041 19041
~~~

Algunas otras clases útiles que se pueden usar con `Get-WmiObject` son `Win32_Process` para obtener una lista de procesos, `Win32_Service` para obtener una lista de servicios `Win32_Bios` para obtener información del BIOS. Podemos usar el parámetro `ComputerName` para obtener información sobre equipos remotos. `GetWmiObject` se puede usar para iniciar y detener servicios en computadoras locales y remotas, y más. Puede encontrar más información sobre el cmdlet [aquí](https://ss64.com/ps/get-wmiobject.html) y [aquí](https://adamtheautomator.com/get-wmiobject/).
___

### **TARGET**

Conéctese a través de Escritorio remoto (RDP) usando el siguiente comando:

~~~
Juceco@htb[/htb]$ xfreerdp /v:<targetIp> /u:htb-student /p:Password
~~~

>Nota: la instancia de destino puede tardar entre 1 y 2 minutos en generarse.

user "htb-student" and password "Academy_WinFun!" 

¿Cuál es el número de compilación de la estación de trabajo de destino?

¿Qué versión de Windows NT está instalada en la estación de trabajo? (es decir, Windows X - distingue entre mayúsculas y minúsculas)