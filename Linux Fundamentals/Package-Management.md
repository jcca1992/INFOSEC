## GESTION DE PAQUETES
___

Ya sea que trabaje como administrador del sistema, mantenga sus propias máquinas Linux en casa o construya, actualice y mantenga su distro de pentest preferida, es crucial tener un conocimiento firme de los administradores de paquetes Linux disponibles y las diversas formas de utilizarlos para instalar, actualizar o eliminar paquetes. Los paquetes son archivos que contienen binarios de software, archivos de configuración, información sobre dependencias y realizan un seguimiento de las actualizaciones y mejoras. Las características que brindan la mayoría de los sistemas de administración de paquetes son:

+ Descarga de paquetes
+ Resolución de Dependencias
+ Un formato de paquete binario estándar
+ Ubicaciones comunes de instalación y configuración
+ Configuración y funcionalidade adicionales relacionadas con el sistema
+ Control de calidad

Podemos usar muchos sistemas de administración de paquetes diferentes que cubren diferentes tipos de archivos como ".deb", ".rpm" y otros. El requisito de gestión de paquetes es que el software que se instalará esté disponible como un paquete correspondiente. Por lo general, esto se crea, ofrece y mantiene de forma centralizada en las distribuciones de Linux. De esta forma, el software se integra directamente en el sistema y sus diversos directorios se distribuyen por todo el sistema. Los cambios del software de administración de paquetes en el sistema para instalar el paquete se toman del paquete y el software de administración de paquetes los implementa. Si el software de administración de paquetes reconoce que se requieren paquetes adicionales para el correcto funcionamiento del paquete que aún no se ha instalado, se incluye una dependencia y se advierte al administrador o se intenta recargar el software faltante desde un repositorio, por ejemplo, e instalarlo. de antemano.

Si se eliminó un software instalado, el sistema de administración de paquetes vuelve a tomar la información del paquete, lo modifica en función de su configuración y elimina los archivos. Existen diferentes programas de gestión de paquetes que podemos utilizar para ello. Aquí hay una lista de ejemplos de tales programas:

|Comando|Descripcion|
|--|--|
|`dpkg`| `dpkg` es una herramienta para instalar, compilar, eliminar y administrar paquetes de Debian. El front-end principal y más fácil de usar para `dpkg` es aptitude.|
|`apt`|Apt proporciona una interfaz de línea de comandos de alto nivel para el sistema de gestión de paquetes. |
|`aptitude`|Aptitude es una alternativa a apt y es una interfaz de alto nivel para el administrador de paquetes.|
|`snap`|Instale, configure, actualice y elimine paquetes instantáneos. `snap` permiten la distribución segura de las últimas aplicaciones y utilidades para la nube, servidores, escritorios e IOT.|
|`gem`|Gem es la interfaz de RubyGems, el administrador de paquetes estándar para Ruby.
|`pip`|Pip es un instalador de paquetes de Python recomendado para instalar paquetes de Python que no están disponibles en el archivo de Debian. Puede funcionar con repositorios de control de versiones (actualmente solo repositorios de Git, Mercurial y Bazaar), registra la salida de manera extensa y evita instalaciones parciales al descargar todos los requisitos antes de comenzar la instalación.|
|`git`|Git es un sistema de control de revisión distribuido, escalable y rápido con un conjunto de comandos inusualmente rico que proporciona operaciones de alto nivel y acceso completo a los elementos internos.|

Es muy recomendable configurar nuestra máquina virtual (VM) localmente para experimentar con ella. Experimentemos un poco en nuestra máquina virtual local y ampliémosla con algunos paquetes adicionales. Primero, instalemos `git` usando `apt`.
___

### ADVANCED PACKAGE MANAGER (APT)

Las distribuciones de Linux basadas en Debian usan el administrador de paquetes `APT`. Un paquete es un archivo de almacenamiento que contiene varios archivos ".deb". La utilidad `dpkg` se utiliza para instalar programas desde el archivo ".deb" asociado. `APT` facilita la actualización e instalación de programas porque muchos programas tienen dependencias. Al instalar un programa desde un archivo ".deb" independiente, es posible que tengamos problemas de dependencia y necesitemos descargar e instalar uno o varios paquetes adicionales. `APT` lo hace más fácil y eficiente al empaquetar todas las dependencias necesarias para instalar un programa.

Cada distribución de Linux utiliza repositorios de software que se actualizan con frecuencia. Cuando actualizamos un programa o instalamos uno nuevo, el sistema consulta estos repositorios por el paquete deseado. Los repositorios se pueden etiquetar como stable, testing, o unstable. La mayoría de las distribuciones de Linux utilizan el repositorio más estable o "main". Esto se puede verificar viendo el contenido del archivo `/etc/apt/sources.list`. La lista de repositorios para Parrot OS está en `/etc/apt/sources.list.d/parrot.list`.

~~~
Juceco@htb[/htb]$ cat /etc/apt/sources.list.d/parrot.list

# parrot repository
# this file was automatically generated by parrot-mirror-selector
deb http://htb.deb.parrot.sh/parrot/ rolling main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling main contrib non-free
deb http://htb.deb.parrot.sh/parrot/ rolling-security main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling-security main contrib non-free

~~~
APT utiliza una base de datos llamada caché de APT. Esto se utiliza para proporcionar información sobre los paquetes instalados en nuestro sistema sin conexión. Podemos buscar en la caché de APT, por ejemplo, para encontrar todos los paquetes relacionados con `Impacket`.

~~~
Juceco@htb[/htb]$ apt-cache search impacket

impacket-scripts - Links to useful impacket scripts examples
polenum - Extracts the password policy from a Windows system
python-pcapy - Python interface to the libpcap packet capture library (Python 2)
python3-impacket - Python3 module to easily build and dissect network protocols
python3-pcapy - Python interface to the libpcap packet capture library (Python 3)
~~~

Entonces podemos ver información adicional sobre un paquete.

~~~
Juceco@htb[/htb]$ apt-cache show impacket-scripts

Package: impacket-scripts
Version: 1.4
Architecture: all
Maintainer: Kali Developers <devel@kali.org>
Installed-Size: 13
Depends: python3-impacket (>= 0.9.20), python3-ldap3 (>= 2.5.0), python3-ldapdomaindump
Breaks: python-impacket (<< 0.9.18)
Replaces: python-impacket (<< 0.9.18)
Priority: optional
Section: misc
Filename: pool/main/i/impacket-scripts/impacket-scripts_1.4_all.deb
Size: 2080
<SNIP>
~~~

También podemos listar todos los paquetes instalados.

~~~
Juceco@htb[/htb]$ apt list --installed

Listing... Done
accountsservice/rolling,now 0.6.55-2 amd64 [installed,automatic]
adapta-gtk-theme/rolling,now 3.95.0.11-1 all [installed]
adduser/rolling,now 3.118 all [installed]
adwaita-icon-theme/rolling,now 3.36.1-2 all [installed,automatic]
aircrack-ng/rolling,now 1:1.6-4 amd64 [installed,automatic]
<SNIP>
~~~

Si nos faltan algunos paquetes, podemos buscarlo e instalarlo usando el siguiente comando.

~~~
Juceco@htb[/htb]$ sudo apt install impacket-scripts -y

Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  impacket-scripts
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,080 B of archives.
After this operation, 13.3 kB of additional disk space will be used.
Get:1 https://euro2-emea-mirror.parrot.sh/mirrors/parrot rolling/main amd64 impacket-scripts all 1.4 [2,080 B]
Fetched 2,080 B in 0s (15.2 kB/s)
Selecting previously unselected package impacket-scripts.
(Reading database ... 378459 files and directories currently installed.)
Preparing to unpack .../impacket-scripts_1.4_all.deb ...
Unpacking impacket-scripts (1.4) ...
Setting up impacket-scripts (1.4) ...
Scanning application launchers
Removing duplicate launchers from Debian
Launchers are updated
~~~
___

### GIT

Ahora que tenemos `git` instalado, podemos usarlo para descargar herramientas útiles de Github. Uno de estos proyectos se llama 'Nishang'. Nos ocuparemos y trabajaremos con el proyecto en sí más adelante. Primero, debemos navegar al [repositorio del proyecto](https://github.com/samratashok/nishang) y copiar el enlace de Github antes de usar git para descargarlo.

![1]()

Sin embargo, antes de descargar el proyecto y sus scripts y listas, debemos crear una carpeta en particular.

~~~
Juceco@htb[/htb]$ mkdir ~/nishang/ && git clone https://github.com/samratashok/nishang.git ~/nishang

Cloning into '/opt/nishang/'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 1691 (delta 4), reused 6 (delta 2), pack-reused 1676
Receiving objects: 100% (1691/1691), 7.84 MiB | 4.86 MiB/s, done.
Resolving deltas: 100% (1055/1055), done.
~~~
___
### DPKG

También podemos descargar los programas y herramientas de los repositorios por separado. En este ejemplo, descargamos 'strace' para Ubuntu 18.04 LTS.

~~~
Juceco@htb[/htb]$ wget http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb

--2020-05-15 03:27:17--  http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb
Resolving archive.ubuntu.com (archive.ubuntu.com)... 91.189.88.142, 91.189.88.152, 2001:67c:1562::18, ...
Connecting to archive.ubuntu.com (archive.ubuntu.com)|91.189.88.142|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 333388 (326K) [application/x-debian-package]
Saving to: ‘strace_4.21-1ubuntu1_amd64.deb’

strace_4.21-1ubuntu1_amd64.deb       100%[===================================================================>] 325,57K  --.-KB/s    in 0,1s    

2020-05-15 03:27:18 (2,69 MB/s) - ‘strace_4.21-1ubuntu1_amd64.deb’ saved [333388/333388]
~~~

Además, ahora podemos usar tanto apt como dpkg para instalar el paquete. Dado que ya hemos trabajado con apt, pasaremos a dpkg en el siguiente ejemplo.

~~~
Juceco@htb[/htb]$ sudo dpkg -i strace_4.21-1ubuntu1_amd64.deb 

(Reading database ... 154680 files and directories currently installed.)
Preparing to unpack strace_4.21-1ubuntu1_amd64.deb ...
Unpacking strace (4.21-1ubuntu1) over (4.21-1ubuntu1) ...
Setting up strace (4.21-1ubuntu1) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
~~~

Con esto, ya tenemos instalada la herramienta y podemos probar si funciona correctamente.

~~~
Juceco@htb[/htb]$ strace -h

usage: strace [-CdffhiqrtttTvVwxxy] [-I n] [-e expr]...
              [-a column] [-o file] [-s strsize] [-P path]...
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]
   or: strace -c[dfw] [-I n] [-e expr]... [-O overhead] [-S sortby]
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]

Output format:
  -a column      alignment COLUMN for printing syscall results (default 40)
  -i             print instruction pointer at time of syscall
~~~

>Ejercicio opcional:

>Busque la herramienta "evil-winrm" en Github e instálela en nuestras instancias interactivas. Pruebe todos los diferentes métodos de instalación.
___
#### [Anterior (Gestion de Usuarios)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/User-Management.md)
#### [Siguiente (Gestion de servicios y procesos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Service-Process-Management.md)