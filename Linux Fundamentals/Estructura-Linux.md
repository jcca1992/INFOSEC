## ESTRUCTURA DE LINUX
___

### HISTORIA

Muchos eventos llevaron a la creación del primer kernel de Linux y, en última instancia, al sistema operativo (OS) Linux, comenzando con el lanzamiento del sistema operativo Unix por parte de Ken Thompson y Dennis Ritchie (quienes trabajaban para AT&T en ese momento) en 1970. El Berkeley Software Distribution (BSD) se lanzó en 1977, pero como contenía el código Unix propiedad de AT&T, una demanda resultante limitó el desarrollo de BSD. Richard Stallman inició el proyecto GNU en 1983. Su objetivo era crear un sistema operativo libre similar a Unix, y parte de su trabajo resultó en la creación de la Licencia Pública General GNU (GPL). Los proyectos de otros a lo largo de los años no lograron dar como resultado un kernel libre y funcional que sería ampliamente adoptado hasta la creación del kernel de Linux.

Al principio, Linux fue un proyecto personal iniciado en 1991 por un estudiante finlandés llamado Linus Torvalds. Su objetivo era crear un kernel de sistema operativo nuevo y gratuito. A lo largo de los años, el kernel de Linux ha pasado de una pequeña cantidad de archivos escritos en C bajo licencia que prohibía la distribución comercial a la última versión con más de 23 millones de líneas de código fuente (comentarios excluidos), con licencia GNU General Public License v2.

Linux está disponible en más de 600 distribuciones (o un sistema operativo basado en el kernel de Linux y software y bibliotecas compatibles). Algunos de los más populares y conocidos son Ubuntu, Debian, Fedora, OpenSUSE, Elementary, Manjaro, Gentoo Linux, RedHat y Linux Mint.

En general, Linux se considera más seguro que otros sistemas operativos y, si bien ha tenido muchas vulnerabilidades del kernel en el pasado, cada vez es menos frecuente. Es menos susceptible al malware que los sistemas operativos Windows y se actualiza con mucha frecuencia. Linux también es muy estable y, en general, ofrece un rendimiento muy alto para el usuario final. Sin embargo, puede ser más difícil para los principiantes y no tiene tantos controladores de hardware como Windows.

Dado que Linux es gratuito y de código abierto, cualquier persona puede modificar y distribuir el código fuente con o sin fines comerciales. Los sistemas operativos basados en Linux se ejecutan en servidores, mainframes, computadoras de escritorio, sistemas integrados como `Routers`, televisores, consolas de videojuegos y más. El sistema operativo general de Android que se ejecuta en teléfonos inteligentes y tabletas se basa en el kernel de Linux y, debido a esto, Linux es el sistema operativo más instalado.

Linux es un sistema operativo como Windows, iOS, Android o macOS. Un sistema operativo es un software que administra todos los recursos de hardware asociados con nuestra computadora. Eso significa que un sistema operativo gestiona toda la comunicación entre el software y el hardware. Además, existen muchas distribuciones diferentes (distro). Es como una versión de los sistemas operativos Windows.

Con las instancias interactivas en Hack The Box, tenemos acceso a `Pwnbox`, una versión personalizada de `Parrot OS`. Este será el sistema operativo principal con el que trabajaremos a través de los módulos. Parrot OS es una distribución de Linux basada en Debian que se centra en la seguridad, la privacidad y el desarrollo.
___
### FILOSOFIA

Linux sigue cinco principios fundamentales:

|Principio|Descripcion|
|--|--|
|`Todo es un archivo`|Todos los archivos de configuración para los diversos servicios que se ejecutan en el sistema operativo Linux se almacenan en uno o más archivos de texto.|
|`Pequeño, programas de un solo propósito`|Linux ofrece muchas herramientas diferentes con las que trabajaremos, que se pueden combinar para trabajar juntas.|
|`Capacidad de enlazar programas para realizar tareas complejas`|La integración y combinación de diferentes herramientas nos permite llevar a cabo muchas tareas grandes y complejas, como procesar o filtrar resultados de datos específicos.|
|`Evita las interfaces de usuario cautivas`| Linux está diseñado para funcionar principalmente con el shell (o terminal), lo que le da al usuario un mayor control sobre el sistema operativo.|
|`Datos de configuración almacenados en un archivo de texto`| Un ejemplo de dicho archivo es el archivo `/etc/passwd`, que almacena todos los usuarios registrados en el sistema.|
___
### COMPONENTES

|Componente|Descripcion|
|--|--|
|`Bootloader`|Un fragmento de código que se ejecuta para guiar el proceso de arranque para iniciar el sistema operativo. Parrot Linux utiliza el cargador de arranque (Bootloader) GRUB.|
|`OS Kernel`|El kernel es el componente principal de un sistema operativo. Administra los recursos para los dispositivos de I/O del sistema a nivel de hardware.|
|`Daemons`|Los servicios en segundo plano se denominan "demonios" en Linux. Su propósito es garantizar que las funciones clave, como la programación, la impresión y la multimedia, funcionen correctamente. Estos pequeños programas se cargan después de que iniciamos o iniciamos sesión en la computadora.|
|`OS Shell`| El shell del sistema operativo o el intérprete de lenguaje de comandos (también conocido como la línea de comandos) es la interfaz entre el sistema operativo y el usuario. Esta interfaz permite al usuario decirle al sistema operativo qué hacer. Los shells más utilizados son Bash, Tcsh/Csh, Ksh, Zsh y Fish.|
|`Graphics server`| Esto proporciona un subsistema gráfico (servidor) llamado "X" o "X-server" que permite que los programas gráficos se ejecuten de forma local o remota en el sistema de ventanas X.|
|`Window Manager`| También conocida como interfaz gráfica de usuario (GUI). Hay muchas opciones, incluidas GNOME, KDE, MATE, Unity y Cinnamon. Un entorno de escritorio suele tener varias aplicaciones, incluidos navegadores web y de archivos. Estos permiten al usuario acceder y administrar las funciones y servicios esenciales y de acceso frecuente de un sistema operativo.|
|`Utilities`| Las aplicaciones o utilidades son programas que realizan funciones particulares para el usuario u otro programa.|
___
### ARQUITECTURA DE LINUX

El sistema operativo Linux se puede dividir en capas:

|Capa|Descripcion|
|`Hardware`|Dispositivos periféricos como la memoria RAM del sistema, el disco duro, la CPU y otros.|
|`Kernel`|El núcleo del sistema operativo Linux cuya función es virtualizar y controlar los recursos comunes de hardware de la computadora, como la CPU, la memoria asignada, los datos a los que se accede y otros. El kernel le da a cada proceso sus propios recursos virtuales, previene y mitiga conflictos entre diferentes procesos.|
|`Shell`|Una interfaz de línea de comandos (CLI), también conocida como shell, en la que un usuario puede ingresar comandos para ejecutar las funciones del kernel.|
|`System Utility`|Pone a disposición del usuario toda la funcionalidad del sistema operativo.|
___
### JERARQUIA DEL SISTEMA DE ARCHIVOS

El sistema operativo Linux está estructurado en una jerarquía similar a un árbol y está documentado en el Estándar de [jerarquía del sistema de archivos](http://www.pathname.com/fhs/) (FHS). Linux está estructurado con los siguientes directorios estándar de nivel superior:

![](https://academy.hackthebox.com/storage/modules/18/NEW_filesystem.png)

|Ruta|Descripcion|
|--|--|
|`/`|El directorio de nivel superior es el sistema de archivos raíz (Root) y contiene todos los archivos necesarios para iniciar el sistema operativo antes de montar otros sistemas de archivos, así como los archivos necesarios para iniciar los otros sistemas de archivos. Después del arranque, todos los demás sistemas de archivos se montan en puntos de montaje estándar como subdirectorios de la raíz.|
|`/bin`|Contiene binarios de comandos esenciales.|
|`/boot`|Consiste en el Bootloader estático, el ejecutable del kernel y los archivos necesarios para arrancar el sistema operativo Linux.|
|`/dev`|Contiene archivos de dispositivos para facilitar el acceso a todos los dispositivos de hardware conectados al sistema.|
|`/etc`|Archivos de configuración del sistema local. Los archivos de configuración de las aplicaciones instaladas también se pueden guardar aquí.|
|`/home`|Cada usuario en el sistema tiene un subdirectorio aquí para almacenamiento.|
|`/lib`|Archivos de biblioteca compartida que se requieren para el arranque del sistema.|
|`/media`|Los dispositivos de medios extraíbles externos, como las unidades USB, se montan aquí.|
|`/mnt`|Punto de montaje temporal para sistemas de archivos normales.|
|`/opt`|Aquí se pueden guardar archivos opcionales, como herramientas de terceros.|
|`/root`|El directorio de inicio para el usuario root.|
|`/sbin`|Este directorio contiene ejecutables utilizados para la administración del sistema (archivos binarios del sistema).|
|`/tmp`|El sistema operativo y muchos programas utilizan este directorio para almacenar archivos temporales. Este directorio generalmente se borra al iniciar el sistema y se puede eliminar en otros momentos sin previo aviso.|
|`/usr`|Contiene ejecutables, bibliotecas, archivos man, etc.|
|`/var`|Este directorio contiene archivos de datos variables, como archivos de registro, buzones de correo electrónico, archivos relacionados con aplicaciones web, archivos cron y más.|
___
#### [Siguiente (Introduccion a Shell)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Intro-Shell.md)
