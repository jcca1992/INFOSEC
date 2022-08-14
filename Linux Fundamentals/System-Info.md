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

~~~
cry0l1t3@htb[/htb]$ uname -a

Linux box 4.15.0-99-generic #100-Ubuntu SMP Wed Apr 22 20:32:56 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
~~~
Del comando anterior, podemos ver que el nombre del kernel es `Linux`, el nombre del host es `box`, el kernel release es `4.15.0-99-generic`, la version del kernel es `#100-Ubuntu SMP Wed Apr 22 20:32:56 UTC 2020`, y así sucesivamente. Ejecutar cualquiera de estas opciones por sí sola nos dará la salida de bits específica que nos interesa.


`Uname para obtener el Kernel Release`

Supongamos que queremos imprimir la versión del kernel para buscar rápidamente posibles vulnerabilidades del kernel. Podemos escribir `uname -r` para obtener esta información.

~~~
cry0l1t3@htb[/htb]$ uname -r

4.15.0-99-generic
~~~

Con esta información, podríamos ir y buscar "exploit genérico 4.15.0-99", y el primer [resultado](https://www.exploit-db.com/exploits/47163) nos parece útil de inmediato.

Es muy recomendable estudiar los comandos y comprender para qué sirven y qué información pueden proporcionar. Aunque es un poco tedioso, podemos aprender mucho estudiando las páginas de manual de los comandos comunes. Incluso podemos descubrir cosas que ni siquiera sabíamos que eran posibles con un comando dado. Esta información no solo se utiliza para trabajar con Linux. También se usará más adelante para descubrir vulnerabilidades y configuraciones incorrectas en el sistema Linux que pueden contribuir a la escalada de privilegios. Aquí hay algunos ejercicios opcionales que podemos resolver con fines de práctica, que nos ayudarán a familiarizarnos con algunos de los comandos.
___

### INICIAR SESION CON SSH

`Secure Shell (SSH)` se refiere a un protocolo que permite a los clientes acceder y ejecutar comandos o acciones en equipos remotos. En hosts y servidores basados en Linux que se ejecutan u otro sistema operativo similar a Unix, SSH es una de las herramientas estándar instaladas permanentemente y es la opción preferida por muchos administradores para configurar y mantener una computadora a través del acceso remoto. Es un protocolo antiguo y muy probado que no requiere ni ofrece una interfaz gráfica de usuario (GUI). Por ello, funciona de forma muy eficiente y ocupa muy pocos recursos. Usamos este tipo de conexión en las siguientes secciones y en la mayoría de los otros módulos para ofrecer la posibilidad de probar los comandos y acciones aprendidos en un entorno seguro. Podemos conectarnos a nuestros objetivos con el siguiente comando:

~~~
Juceco@htb[/htb]$ ssh [username]@[IP address]
~~~
___

### RETO

+ Averigüe el nombre del hardware de la máquina y envíelo como respuesta.

`R: x86_64`

~~~
htb-student@nixfund:~$ uname -m
x86_64
~~~

___
+ ¿Cuál es la ruta al directorio de inicio de htb-student?

`R: /home/htb-student`

~~~
htb-student@nixfund:~$ pwd
/home/htb-student
~~~

___
+ ¿Cuál es la ruta al correo de htb-student?

`R: /var/mail/htb-student`

~~~
htb-student@nixfund:~$ env | grep MAIL
MAIL=/var/mail/htb-student
~~~

___
+ ¿Qué shell se especifica para el usuario htb-student?

`R: /bin/bash`

Vemos al final htb-student:x:1002:1002::/home/htb-student:/bin/bash
~~~
htb-student@nixfund:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:109:1::/var/cache/pollinate:/bin/false
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
postfix:x:111:114::/var/spool/postfix:/usr/sbin/nologin
proftpd:x:112:65534::/run/proftpd:/usr/sbin/nologin
ftp:x:113:65534::/srv/ftp:/usr/sbin/nologin
dovecot:x:114:117:Dovecot mail server,,,:/usr/lib/dovecot:/usr/sbin/nologin
dovenull:x:115:118:Dovecot login user,,,:/nonexistent:/usr/sbin/nologin
htb-student:x:1002:1002::/home/htb-student:/bin/bash
mysql:x:116:120:MySQL Server,,,:/nonexistent:/bin/false
~~~

___
+ ¿Qué versión del kernel está instalada en el sistema? (Formato: 1.22.3)

`R: 4.15.0`

~~~
htb-student@nixfund:~$ uname -r
4.15.0-123-generic
~~~

___
+ ¿Cuál es el nombre de la interfaz de red en la que la MTU está configurada en 1500?

`R: ens192`

MTU es Maximum Transmission Unit es el mayor tamaño posible de una unidad de datos en un protocolo de capa de red (PDU de capa de red) que puede ser utilizado en una comunicación 
~~~
htb-student@nixfund:~$ ifconfig
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.124.204  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 fe80::250:56ff:feb9:7d6  prefixlen 64  scopeid 0x20<link>
        inet6 dead:beef::250:56ff:feb9:7d6  prefixlen 64  scopeid 0x0<global>
        ether 00:50:56:b9:07:d6  txqueuelen 1000  (Ethernet)
        RX packets 515  bytes 49734 (49.7 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 509  bytes 81159 (81.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 902  bytes 71550 (71.5 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 902  bytes 71550 (71.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
~~~
___
#### [Anterior (Obteniendo Ayuda)]()
#### [Siguiente (Administracion de Usuario)]()
