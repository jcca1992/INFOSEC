## INFORMACION DEL SISTEMA
___
Dado que trabajaremos con muchos sistemas Linux diferentes, necesitamos aprender la estructura y la información sobre el sistema, sus procesos, configuraciones de red, usuarios, directorios, configuraciones de usuario y los parámetros correspondientes. Aquí hay una lista de las herramientas necesarias que nos ayudarán a obtener la información anterior. La mayoría de ellos están instalados por defecto.

|Comando|Descripcion|
|--|--|
|`whoami`|Muestra el nombre de usuario actual.|
|`id`|Devuelve la identidad de los usuarios|
|`hostname`|Establece o imprime el nombre del sistema host actual.|
|`uname`|Imprime información básica sobre el nombre del sistema operativo y el hardware del sistema.|
|`pwd`|Devuelve el nombre del directorio de trabajo.|
|`ifconfig`|La utilidad ifconfig se usa para asignar o ver una dirección a una interfaz de red y/o configurar los parámetros de la interfaz de red.|
|`ip`|Ip es una utilidad para mostrar o manipular enrutamiento, dispositivos de red, interfaces y túneles.|
|`netstat`| Muestra el estado de la red.|
|`ss`| Otra utilidad para investigar sockets.|
|`ps`|Muestra el estado del proceso.|
|`who`| Muestra quién está conectado.|
|`env`|Imprime el entorno o establece y ejecuta el comando.|
|`lsblk`|Enumera los dispositivos de bloque.|
|`lsusb`|Lista dispositivos USB|
|`lsof`|Enumera los archivos abiertos.|
|`lspci`|Enumera los dispositivos PCI.|

Veamos algunos ejemplos.

`Hostname`

El comando de nombre de host se explica por sí mismo y solo imprimirá el nombre de la computadora en la que estamos conectados

~~~
┌─[root@PCnotebook]─[/home/juliocesar]
└──╼ #hostname
PCnotebook
~~~

`Whoami`

Este comando rápido y fácil se puede usar en sistemas Windows y Linux para obtener nuestro nombre de usuario actual. Durante una evaluación de seguridad, obtenemos acceso de shell inverso en un host, y uno de los primeros bits de conocimiento de la situación que debemos hacer es averiguar con qué usuario nos estamos ejecutando. A partir de ahí, podemos averiguar si el usuario tiene algún privilegio o acceso especial.

~~~
┌─[juliocesar@PCnotebook]─[~]
└──╼ $whoami
juliocesar
~~~

`Id`

El comando `id` amplía el comando `whoami` e imprime nuestra membresía e ID de grupo efectivas. Esto puede ser de interés para los evaluadores de penetración que buscan ver qué acceso puede tener un usuario y los administradores de sistemas que buscan auditar los permisos de la cuenta y la membresía del grupo. En este resultado, el grupo `hackthebox` es de interés porque no es estándar, el grupo `adm` significa que el usuario puede leer archivos de registro en `/var/log` y podría obtener acceso a información confidencial, la membresía en el grupo sudo es de particular importancia. interés ya que esto significa que nuestro usuario puede ejecutar algunos o todos los comandos como el todopoderoso usuario root. Los derechos de `Sudo` podrían ayudarnos a aumentar los privilegios o podrían ser una señal para un administrador de sistemas de que pueden necesitar auditar permisos y membresías de grupos para eliminar cualquier acceso que no sea necesario para que un usuario determinado lleve a cabo sus tareas diarias.

~~~
┌─[juliocesar@PCnotebook]─[~]
└──╼ $id
uid=1000(juliocesar) gid=1003(juliocesar) groups=1003(juliocesar),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),119(bluetooth),1000(lpadmin),1001(scanner),1002(docker)
~~~

`Uname`

Profundicemos un poco más en el comando `uname`. Si escribimos `man uname` en nuestra terminal, abriremos la página de manual para el comando, que mostrará las posibles opciones que podemos ejecutar con el comando y los resultado

~~~

UNAME(1)                                    User Commands                                   UNAME(1)

NAME
       uname - print system information

SYNOPSIS
       uname [OPTION]...

DESCRIPTION
       Print certain system information.  With no OPTION, same as -s.

       -a, --all
              print all information, in the following order, except omit -p and -i if unknown:

       -s, --kernel-name
              print the kernel name

       -n, --nodename
              print the network node hostname

       -r, --kernel-release
              print the kernel release

       -v, --kernel-version
              print the kernel version

       -m, --machine
              print the machine hardware name

       -p, --processor
              print the processor type (non-portable)

       -i, --hardware-platform
              print the hardware platform (non-portable)

       -o, --operating-system
~~~

Ejecutar `uname -a` imprimirá toda la información sobre la máquina en un orden específico: nombre del kernel, nombre del host, lanzamiento del kernel, versión del kernel, nombre del hardware de la máquina y sistema operativo. El indicador `-a` omitirá `-p` (tipo de procesador) y `-i` (plataforma de hardware) si son desconocidos.

~~~
┌─[juliocesar@PCnotebook]─[~]
└──╼ $uname -a
Linux PCnotebook 5.18.0-14parrot1-amd64 #1 SMP PREEMPT_DYNAMIC Debian 5.18.14-1parrot1 (2022-08-07) x86_64 GNU/Linux
~~~

Del comando anterior, podemos ver que el nombre del kernel es `Linux`, el nombre del host es `PCNotebook`, el kernel release es `5.18.0-14parrot1-amd64`, la version del kernel es `#1 SMP PREEMPT_DYNAMIC Debian 5.18.14-1parrot1 (2022-08-07)`, y así sucesivamente. Ejecutar cualquiera de estas opciones por sí sola nos dará la salida de bits específica que nos interesa.

`Uname para obtener el Kernel Release`

Supongamos que queremos imprimir la versión del kernel para buscar rápidamente posibles vulnerabilidades del kernel. Podemos escribir `uname -r` para obtener esta información.

~~~
┌─[juliocesar@PCnotebook]─[~]
└──╼ $uname -r
5.18.0-14parrot1-amd64
~~~

Con esta información, podríamos ir y buscar "exploit genérico 4.15.0-99", y el primer resultado nos parece útil de inmediato.

Es muy recomendable estudiar los comandos y comprender para qué sirven y qué información pueden proporcionar. Aunque es un poco tedioso, podemos aprender mucho estudiando las páginas de manual de los comandos comunes. Incluso podemos descubrir cosas que ni siquiera sabíamos que eran posibles con un comando dado. Esta información no solo se utiliza para trabajar con Linux. Sin embargo, también se usará más adelante para descubrir vulnerabilidades y configuraciones incorrectas en el sistema Linux que pueden contribuir a la escalada de privilegios. Aquí hay algunos ejercicios opcionales que podemos resolver con fines de práctica, que nos ayudarán a familiarizarnos con algunos de los comandos.

___
#### [Anterior (Obteniendo Ayuda)]()
#### [Siguiente (Administracion de Usuario)]()
