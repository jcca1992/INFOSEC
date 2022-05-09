## **NTFS VS PERMISOS COMPARTIDOS**

Microsoft posee más del [70%](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-201804-202104) de la cuota de mercado mundial de sistemas operativos de escritorio con Windows. Esto explica por qué la mayoría de los autores de malware eligen escribir malware para Windows y por qué muchos perciben que Windows es menos seguro que otros sistemas operativos. Desde una perspectiva comercial, tiene sentido que los autores de malware gasten recursos en escribir malware para Windows. Es un objetivo de alto valor. La idea de que cualquier sistema operativo es inmune al malware es una falacia técnica. Si el software se puede escribir para un sistema operativo, entonces se puede escribir un virus para un sistema operativo. Tenga en cuenta que un virus, por definición, es software escrito con intenciones maliciosas y se puede escribir para cualquier sistema operativo. Muchas variantes de malware escritas para Windows pueden propagarse por la red a través de recursos compartidos de red con la aplicación de permisos indulgentes. También vale la pena señalar que, hasta el día de hoy, la infame vulnerabilidad `EternalBlue` todavía persigue a los sistemas Windows sin parches que ejecutan `SMBv1` y, a menudo, allana el camino para que el ransomware cierre las organizaciones.

El protocolo de bloque de mensajes del servidor, en ingles `Server Message Block protocol (SMB)` se usa en Windows para conectar recursos compartidos como archivos e impresoras. Se utiliza en entornos de empresas grandes, medianas y pequeñas. Vea la imagen a continuación para visualizar este concepto:

![](https://academy.hackthebox.com/storage/modules/49/smb_diagram.png)

>Nota: Cada vez que vea una visualización o diagrama de un concepto, tómese su tiempo para comprenderlo a fondo. Una imagen puede valer más que mil palabras, pero es muy tentador omitirla al leer.

Los permisos NTFS y los permisos compartidos a menudo se entienden como iguales. Tenga en cuenta que no son lo mismo, pero a menudo se aplican al mismo recurso compartido. Echemos un vistazo a los permisos individuales que se pueden configurar para proteger y otorgar acceso a los objetos a un recurso compartido de red alojado en un sistema operativo Windows que ejecuta el sistema de archivos NTFS.
___

### **RECURSOS COMPARTIDOS**

|Permisos|Descripcion|
|--|--|
|Full Control| Los usuarios pueden realizar todas las acciones otorgadas por los permisos Change y Read, así como cambiar los permisos para archivos y subcarpetas NTFS.|
|Change|Los usuarios pueden leer, editar, eliminar y agregar archivos y subcarpetas|
|Read|Los usuarios pueden ver el contenido de archivos y subcarpetas|
___

### **PERMISOS BASICOS NTFS**

|Permiso|Descripcion|
|--|--|
|Full Control| Los usuarios pueden agregar, editar, mover, eliminar archivos y carpetas, así como cambiar los permisos NTFS que se aplican a todas las carpetas permitidas.|
|Modify| A los usuarios se les otorgan o niegan permisos para ver y modificar archivos y carpetas. Esto incluye agregar o eliminar archivos|
|Read & Execute| A los usuarios se les permite o deniega permisos para leer el contenido de los archivos y ejecutar programas|
|List folder contents| A los usuarios se les permite o deniega permisos para ver una lista de archivos y subcarpetas|
|Read| A los usuarios se les permite o deniega permisos para leer el contenido de los archivos|
|Write| A los usuarios se les permite o deniega permisos para escribir cambios en un archivo y agregar nuevos archivos a una carpeta|
|Special Permissions| Una variedad de opciones de permisos avanzados|
___

### **PERMISOS ESPECIALES NTFS**

|Permiso|Descripcion|
|--|--|
|Full control| A los usuarios se les permite o deniega permisos para agregar, editar, mover, eliminar archivos y carpetas, así como cambiar los permisos NTFS que se aplican a todas las carpetas permitidas.|
|Traverse folder / execute file| A los usuarios se les permite o se les niegan permisos para acceder a una subcarpeta dentro de una estructura de directorio incluso si se le niega el acceso al contenido en el nivel de la carpeta principal. Los usuarios también pueden permitir o denegar permisos para ejecutar programas|
|List folder/read data| A los usuarios se les permite o deniega permisos para ver archivos y carpetas contenidos en la carpeta principal. También se puede permitir a los usuarios abrir y ver archivos|
|Read attributes| A los usuarios se les otorgan o niegan permisos para ver los atributos básicos de un archivo o carpeta. Ejemplos de atributos básicos: sistema, archivo, solo lectura y oculto|
|Read extended attributes| A los usuarios se les otorgan o niegan permisos para ver los atributos extendidos de un archivo o carpeta. Los atributos difieren según el programa.|
|Create files/write data| A los usuarios se les permite o deniega permisos para crear archivos dentro de una carpeta y realizar cambios en un archivo.|
|Create folders/append data| A los usuarios se les permite o deniega permisos para crear subcarpetas dentro de una carpeta. Se pueden agregar datos a los archivos, pero el contenido preexistente no se puede sobrescribir.|
|Write attributes| A los usuarios se les permite o deniega cambiar los atributos del archivo. Este permiso no otorga acceso para crear archivos o carpetas.|
|Write extended attributes| A los usuarios se les permite o se les niegan permisos para cambiar los atributos extendidos en un archivo o carpeta. Los atributos difieren según el programa.|
|Delete subfolders and files| A los usuarios se les permite o deniega permisos para eliminar subcarpetas y archivos. Las carpetas principales no se eliminarán|
|Delete| A los usuarios se les permite o deniega permisos para eliminar carpetas principales, subcarpetas y archivos.|
|Read permissions| Los usuarios tienen permitidos o denegados permisos para leer los permisos de una carpeta.|
|Change permissions| A los usuarios se les permite o deniega permisos para cambiar los permisos de un archivo o carpeta|
|Take ownership| A los usuarios se les permite o se les niega el permiso para tomar posesión de un archivo o carpeta. El propietario de un archivo tiene permisos completos para cambiar cualquier permiso.|

Tenga en cuenta que los permisos NTFS se aplican al sistema donde se alojan la carpeta y los archivos. Las carpetas creadas en NTFS heredan los permisos de las carpetas principales de forma predeterminada. Es posible deshabilitar la herencia para establecer permisos personalizados en carpetas principales y subcarpetas, como lo haremos más adelante en este módulo. Los permisos para compartir se aplican cuando se accede a la carpeta a través de SMB, generalmente desde un sistema diferente a través de la red. Esto significa que alguien que inició sesión localmente en la máquina o a través de RDP puede acceder a la carpeta y los archivos compartidos simplemente navegando a la ubicación en el sistema de archivos y solo necesita considerar los permisos NTFS. Los permisos a nivel de NTFS brindan a los administradores un control mucho más granular sobre lo que los usuarios pueden hacer dentro de una carpeta o archivo.
___

### **CREANDO RECURSOS COMPARTIDOS**

Para obtener una comprensión fundamental sólida de SMB y su relación con NTFS, crearemos un recurso compartido de red en el `Windows 10 de la maquina objetivo`.

>Nota: Es una experiencia de aprendizaje ideal tener el Pwnbox abierto en pantalla completa en un monitor separado para que podamos tener al menos una pantalla dedicada a mostrar el contenido escrito y una pantalla para las maquinas con las que estamos interactuando. Alternativamente, si solo tenemos acceso a una pantalla, podemos usar esa para interacciones con maquinas y un teléfono inteligente o tableta para hacer referencia al contenido escrito.

En este caso, crearemos una carpeta compartida creando primero una nueva carpeta en el escritorio de Windows 10. Tenga en cuenta que en la mayoría de los entornos empresariales grandes, los recursos compartidos se crean en una red de área de almacenamiento (SAN), un dispositivo de almacenamiento conectado a la red (NAS) o una partición separada en unidades a las que se accede a través de un sistema operativo de servidor como Windows Server. Si alguna vez nos encontramos con acciones en un sistema operativo de escritorio, será una pequeña empresa o podría ser un sistema `beachhead` utilizado por un pentester o un atacante malicioso para recopilar y filtrar datos.

Pasaremos por este proceso usando la GUI en Windows

#### *CREANDO EL ARCHIVO*

![](https://academy.hackthebox.com/storage/modules/49/creating_directory.png)

Vamos a utilizar la opción `Advanced Sharing` para configurar nuestro recurso compartido.

#### *COMPARTIENDO ARCHIVO*

![](https://academy.hackthebox.com/storage/modules/49/configuring_share.png)

Observe cómo el nombre del recurso compartido automáticamente toma como valor predeterminado el nombre de la carpeta. Además, podemos ver que es posible limitar la cantidad de usuarios que se pueden conectar a este recurso compartido simultáneamente. En un entorno del mundo real, es una buena práctica que los administradores establezcan este número de acuerdo con la cantidad de usuarios que necesitan acceder regularmente al recurso que se comparte.

Similar a los permisos NTFS, existe una `lista de control de acceso (ACL)` para los recursos compartidos. Podemos considerar esto como la lista de permisos SMB. Tenga en cuenta que con los recursos compartidos, las listas de permisos de SMB y NTFS se aplican a todos los recursos que se comparten en Windows. La ACL contiene `entradas de control de acceso (ACE)`. Por lo general, estas ACE están compuestas por `usuarios` y `grupos` (también llamados principales de seguridad), ya que son un mecanismo adecuado para administrar y rastrear el acceso a los recursos compartidos.

Observe la entrada de control de acceso predeterminada y la configuración de permisos.

#### *COMPARTIR PERMISOS ACL (PESTAÑA COMPARTIR)*

![](https://academy.hackthebox.com/storage/modules/49/share_permissions.png)

Por ahora, aplicaremos esta configuración para probar el efecto de esta ACL y los permisos aplicados tal cual. Probaremos la conectividad desde el Pwnbox abriendo la terminal y usando `smbclient`.

>Nota: Un servidor es técnicamente una función de software utilizada para atender las solicitudes de un cliente. En este caso, el Pwnbox es nuestro cliente y la maquina target con Windows 10 es nuestro servidor.

#### *USANDO SMBCLIENT PARA CONECTARSE AL RECURSO COMPARTIDO*

~~~
Juceco@htb[/htb]$ smbclient -L IPaddressOfTarget -U htb-student
Enter WORKGROUP\htb-student's password: 

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	Company Data    Disk      
	IPC$            IPC       Remote IPC
~~~

¿Qué podría impedirnos acceder a este recurso compartido si todas nuestras entradas son correctas y nuestra lista de permisos tiene el grupo `all` presente con al menos permisos de lectura?
___

### **CONSIDERACIONES DE FIREWALL DE WINDOWS DEFENDER**

Es el Firewall de Windows Defender el que podría estar bloqueando el acceso al recurso compartido SMB. Dado que nos estamos conectando desde un sistema basado en Linux, el firewall ha bloqueado el acceso desde cualquier dispositivo que no esté unido al mismo `workgroup`. También es importante tener en cuenta que cuando un sistema Windows forma parte de un grupo de trabajo, todas las solicitudes de `inicio de sesión de red `se autentican en la base de datos `SAM` de ese sistema Windows en particular. Cuando un sistema Windows se une a un entorno de dominio de Windows, todas las solicitudes de inicio de sesión de red se autentican en `Active Directory`. La principal diferencia entre un grupo de trabajo y un Dominio de Windows en términos de autenticación es que con un grupo de trabajo se usa la base de datos SAM local y en un Dominio de Windows se usa una base de datos centralizada basada en la red (Active Directory). Debemos conocer esta información cuando intentemos iniciar sesión y autenticarnos con un sistema Windows. Considere dónde está alojada la cuenta de htb-student para conectarse correctamente al objetivo.

En términos de conexiones de bloqueo de firewall, esto se puede probar desactivando completamente cada perfil de firewall en Windows o habilitando reglas de firewall entrantes predefinidas específicas en la c`onfiguración de seguridad avanzada de Firewall de Windows Defender`. Como la mayoría de los firewalls, el Firewall de Windows Defender permite o deniega el tráfico (solicitudes de acceso y conexión en este caso) `entrante` y/o `saliente`.

Las diferentes reglas de entrada y salida están asociadas con los diferentes perfiles de firewall en defender.

Perfiles de Firewall de Windows Defender:
+ `Public`
+ `Private`
+ `Domain`

Es una buena práctica habilitar reglas predefinidas o agregar excepciones personalizadas en lugar de desactivar el firewall por completo. Desafortunadamente, es muy común que los firewalls se dejen completamente desactivados por conveniencia o por falta de comprensión. Las reglas de firewall en los sistemas de escritorio se pueden administrar de forma centralizada cuando se unen a un entorno de dominio de Windows mediante el uso de la directiva de grupo. Los conceptos y configuraciones de directivas de grupo están fuera del alcance de este módulo.

Una vez que las reglas de firewall `entrantes` adecuadas estén habilitadas, nos conectaremos con éxito al recurso compartido. Tenga en cuenta que solo podemos conectarnos al recurso compartido porque la cuenta de usuario que estamos usando (`htb-student`) está en el `grupo Todos`. Recuerde que dejamos los permisos de uso compartido específicos para el grupo Todos establecidos en Lectura, lo que literalmente significa que solo podremos Leer archivos en este recurso compartido. Una vez que se establece una conexión con un recurso compartido, podemos crear un `punto de montaje` desde nuestro Pwnbox al sistema de archivos del Windows 10 de la maquina objetivo. Aquí es donde también debemos considerar que los permisos NTFS se aplican junto con los permisos compartidos. Recuerde que NTFS es el sistema de archivos predeterminado en Windows. Volvamos a nuestra sesión xfreerdp con nuestro Windows 10 de la maquina objetivo y echemos un vistazo a los permisos NTFS en la carpeta Data Company.