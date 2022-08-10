## ASEGURAMIENTO DE HABILIDADES

Inlanefreight recientemente tuvo un incidente en el que un empleado de marketing descontento accedió a un recurso compartido de recursos humanos alojado internamente y eliminó varios archivos y carpetas confidenciales. Afortunadamente, el equipo de TI tenía buenas copias de seguridad y restauró todos los datos eliminados. Ahora existe la preocupación de que este empleado descontento haya podido acceder a la parte de recursos humanos en primer lugar. Después de realizar una evaluación de seguridad, descubrió que es posible que el equipo de TI no comprenda completamente cómo funcionan los permisos en Windows. Está realizando una capacitación y una demostración para mostrarle al departamento buenas prácticas de seguridad con el uso compartido de archivos en un entorno de Windows, así como la visualización de servicios desde la línea de comandos para verificar cualquier posible manipulación.

>Nota: Es importante que cada paso se complete en el orden en que se presentan. Comenzando con el paso 1 y avanzando hasta el paso 8, incluidas todas las especificaciones asociadas con cada paso. Tenga en cuenta que cada paso está diseñado para brindarle la oportunidad de aplicar las habilidades y los conceptos que se enseñan a lo largo de este módulo. Tómese su tiempo, diviértase y siéntase libre de comunicarse si se queda atascado.

En esta demostración, usted va:

1. Crear una carpeta compartida llamada Datos de la empresa (`Company Data`)
2. Crear de una subcarpeta llamada HR dentro de la carpeta de datos de la empresa
3. Crear un usuario llamado Jim
    + Desmarque: El usuario debe cambiar la contraseña al iniciar sesión

Nos vamos a Computer Management, en Local Users and Group y en la carpeta Users

![3](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/3.png)

Presionamos click derecho, en `New User` agregamos los datos y desmarcamos `User must change at next logon`

![4](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/4.png)

4. Crear un grupo de seguridad llamado HR

Ahora seleccionamos `Groups` y hacemos el mismo procedimiento

![5](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/5.png)

Presionamos add para agregar Usuarios al grupo

![6](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/6.png)

5. Adición de Jim al grupo de seguridad HR
buscamos Jim y presionamos Check Names
![7](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/7.png)

6. Adición del grupo de seguridad HR a la carpeta de datos de la empresa compartida y a la lista de permisos de NTFS
    + Eliminar el grupo predeterminado que está presente
    + Compartir permisos: Permitir cambiar y leer
    + Deshabilite la herencia antes de emitir permisos NTFS específicos
    + Permisos NTFS: Modificar, Leer y Ejecutar, Mostrar el contenido de la carpeta, Leer, Escribir
7. Adición del grupo de seguridad de recursos humanos a la lista de permisos NTFS de la subcarpeta de recursos humanos
    + Eliminar el grupo predeterminado que está presente
    + Deshabilite la herencia antes de emitir permisos NTFS específicos
    + Permisos NTFS: Modificar, Leer y Ejecutar, Mostrar el contenido de la carpeta, Leer y Escribir
8. Uso de PowerShell para enumerar detalles sobre un servicio

![8](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/8.png)
![9](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/9.png)
![10](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/10.png)
![11](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/11.png)
![12](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/12.png)
![13](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/13.png)
![14](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/14.png)
![15](https://github.com/jcca1992/INFOSEC/blob/main/Windows%20Fundamentals/Images/15.png)
___

+ ¿Cuál es el nombre del grupo que está presente en la LCA de permisos de uso compartido de datos de la empresa de forma predeterminada?
`R: Everyone`

+ ¿Cuál es el nombre de la pestaña que le permite configurar los permisos de NTFS?
R: `Security`

+ ¿Cuál es el nombre del servicio asociado con Windows Update?
R: `wuauserv`

~~~
PS C:\WINDOWS\system32> get-service | ? {$_.DisplayName -eq "Windows Update"}

Status   Name               DisplayName
------   ----               -----------
Running  wuauserv           Windows Update
~~~

+ Enumere el SID asociado con la cuenta de usuario Jim que creó.

`R: S-1-5-21-2614195641-1726409526-3792725429-1006`

~~~
C:\WINDOWS\system32>wmic useraccount where name="Jim" get sid
SID
S-1-5-21-2614195641-1726409526-3792725429-1006
~~~


+ Enumere el SID asociado con el grupo de seguridad de recursos humanos que creó.

`R: S-1-5-21-2614195641-1726409526-3792725429-1007`

~~~
C:\WINDOWS\system32>wmic group where name="HR" get sid
SID
S-1-5-21-2614195641-1726409526-3792725429-1007
~~~