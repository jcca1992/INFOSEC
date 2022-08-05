## ASEGURAMIENTO DE HABILIDADES

Inlanefreight recientemente tuvo un incidente en el que un empleado de marketing descontento accedió a un recurso compartido de recursos humanos alojado internamente y eliminó varios archivos y carpetas confidenciales. Afortunadamente, el equipo de TI tenía buenas copias de seguridad y restauró todos los datos eliminados. Ahora existe la preocupación de que este empleado descontento haya podido acceder a la parte de recursos humanos en primer lugar. Después de realizar una evaluación de seguridad, descubrió que es posible que el equipo de TI no comprenda completamente cómo funcionan los permisos en Windows. Está realizando una capacitación y una demostración para mostrarle al departamento buenas prácticas de seguridad con el uso compartido de archivos en un entorno de Windows, así como la visualización de servicios desde la línea de comandos para verificar cualquier posible manipulación.

>Nota: Es importante que cada paso se complete en el orden en que se presentan. Comenzando con el paso 1 y avanzando hasta el paso 8, incluidas todas las especificaciones asociadas con cada paso. Tenga en cuenta que cada paso está diseñado para brindarle la oportunidad de aplicar las habilidades y los conceptos que se enseñan a lo largo de este módulo. Tómese su tiempo, diviértase y siéntase libre de comunicarse si se queda atascado.

En esta demostración, usted es:

Creación de una carpeta compartida llamada Datos de la empresa
Creación de una subcarpeta llamada HR dentro de la carpeta de datos de la empresa
Creando un usuario llamado Jim
+ Desmarque: El usuario debe cambiar la contraseña al iniciar sesión
Crear un grupo de seguridad llamado HR
Adición de Jim al grupo de seguridad de recursos humanos
Adición del grupo de seguridad de recursos humanos a la carpeta de datos de la empresa compartida y a la lista de permisos de NTFS
+ Eliminar el grupo predeterminado que está presente
+ Compartir permisos: Permitir cambiar y leer
+ Deshabilite la herencia antes de emitir permisos NTFS específicos
+ Permisos NTFS: Modificar, Leer y Ejecutar, Mostrar el contenido de la carpeta, Leer, Escribir
Adición del grupo de seguridad de recursos humanos a la lista de permisos NTFS de la subcarpeta de recursos humanos
+ Eliminar el grupo predeterminado que está presente
+ Deshabilite la herencia antes de emitir permisos NTFS específicos
+ Permisos NTFS: Modificar, Leer y Ejecutar, Mostrar el contenido de la carpeta, Leer y Escribir
Uso de PowerShell para enumerar detalles sobre un servicio

¿Cuál es el nombre del grupo que está presente en la LCA de permisos de uso compartido de datos de la empresa de forma predeterminada?

¿Cuál es el nombre de la pestaña que le permite configurar los permisos de NTFS?

¿Cuál es el nombre del servicio asociado con Windows Update?

Enumere el SID asociado con la cuenta de usuario Jim que creó.

Enumere el SID asociado con el grupo de seguridad de recursos humanos que creó.