## GESTION DE SERVICIOS Y PROCESOS
___
En general, hay dos tipos de servicios: internos, los servicios relevantes que se requieren al iniciar el sistema, que por ejemplo, realizan tareas relacionadas con el hardware, y los servicios que instala el usuario, que generalmente incluyen todos los servicios del servidor. Dichos servicios se ejecutan en segundo plano sin ninguna interacción del usuario. Estos también se denominan `daemons` y se identifican con la letra `'d'` al final del nombre del programa, por ejemplo, `sshd` o `systemd`.

La mayoría de las distribuciones de Linux ahora han cambiado a `systemd`. Este `daemon` es un `Init process` iniciado primero y, por lo tanto, tiene el ID de proceso (PID) 1. Este daemon supervisa y se encarga del inicio y la detención ordenados de otros servicios. Todos los procesos tienen un PID asignado que se puede ver en `/proc/` con el número correspondiente. Dicho proceso puede tener un ID de proceso principal (PPID), conocido como proceso secundario.

Además de `systemctl`, también podemos usar `update-rc.d` para administrar los enlaces del script de inicio de SysV. Echemos un vistazo a algunos ejemplos. Usaremos el servidor `OpenSSH` en estos ejemplos. Si no lo tenemos instalado, instálelo antes de continuar con esta sección.
___
### SYSTEMCTL

Luego de instalar `OpenSSH` en nuestra VM, podemos iniciar el servicio con el siguiente comando.

~~~
Juceco@htb[/htb]$ systemctl start ssh
~~~

Después de haber iniciado el servicio, ahora podemos verificar si se ejecuta sin errores.

~~~
Juceco@htb[/htb]$ systemctl status ssh

● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-05-14 15:08:23 CEST; 24h ago
   Main PID: 846 (sshd)
   Tasks: 1 (limit: 4681)
   CGroup: /system.slice/ssh.service
           └─846 /usr/sbin/sshd -D

Mai 14 15:08:22 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 14 15:08:23 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:23 inlane sshd[846]: Server listening on :: port 22.
Mai 14 15:08:23 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 14 15:08:30 inlane systemd[1]: Reloading OpenBSD Secure Shell server.
Mai 14 15:08:31 inlane sshd[846]: Received SIGHUP; restarting.
Mai 14 15:08:31 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:31 inlane sshd[846]: Server listening on :: port 22.
~~~

Para agregar OpenSSH al script SysV para indicarle al sistema que ejecute este servicio después del inicio, podemos vincularlo con el siguiente comando:

~~~
Juceco@htb[/htb]$ systemctl enable ssh

Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
~~~

Una vez que reiniciamos el sistema, el servidor OpenSSH se ejecutará automáticamente. Podemos comprobar esto con una herramienta llamada `ps`.

~~~
Juceco@htb[/htb]$ ps -aux | grep ssh

root       846  0.0  0.1  72300  5660 ?        Ss   Mai14   0:00 /usr/sbin/sshd -D
~~~

También podemos usar `systemctl` para listar todos los servicios.

~~~
Juceco@htb[/htb]$ systemctl list-units --type=service

UNIT                                                       LOAD   ACTIVE SUB     DESCRIPTION              
accounts-daemon.service                                    loaded active running Accounts Service         
acpid.service                                              loaded active running ACPI event daemon        
apache2.service                                            loaded active running The Apache HTTP Server   
apparmor.service                                           loaded active exited  AppArmor initialization  
apport.service                                             loaded active exited  LSB: automatic crash repor
avahi-daemon.service                                       loaded active running Avahi mDNS/DNS-SD Stack  
bolt.service                                               loaded active running Thunderbolt system servi
~~~

Es muy posible que los servicios no se inicien debido a un error. Para ver el problema, podemos usar la herramienta `journalctl` para ver los registros.

~~~
Juceco@htb[/htb]$ journalctl -u ssh.service --no-pager

-- Logs begin at Wed 2020-05-13 17:30:52 CEST, end at Fri 2020-05-15 16:00:14 CEST. --
Mai 13 20:38:44 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 13 20:38:44 inlane sshd[2722]: Server listening on 0.0.0.0 port 22.
Mai 13 20:38:44 inlane sshd[2722]: Server listening on :: port 22.
Mai 13 20:38:44 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 13 20:39:06 inlane sshd[3939]: Connection closed by 10.22.2.1 port 36444 [preauth]
Mai 13 20:39:27 inlane sshd[3942]: Accepted password for master from 10.22.2.1 port 36452 ssh2
Mai 13 20:39:27 inlane sshd[3942]: pam_unix(sshd:session): session opened for user master by (uid=0)
Mai 13 20:39:28 inlane sshd[3942]: pam_unix(sshd:session): session closed for user master
Mai 14 02:04:49 inlane sshd[2722]: Received signal 15; terminating.
Mai 14 02:04:49 inlane systemd[1]: Stopping OpenBSD Secure Shell server...
Mai 14 02:04:49 inlane systemd[1]: Stopped OpenBSD Secure Shell server.
-- Reboot --
~~~
___
### MATAR PROCESO

Un proceso puede estar en los siguientes estados:
+ Running
+ Waiting (esperando un evento o recurso del sistema)
+ Stopped
+ Zombie (detenido pero aún tiene una entrada en la tabla de procesos).

Los procesos se pueden controlar mediante `kill`, `pkill`, `pgrep` y `killall`. Para interactuar con un proceso, debemos enviarle una señal. Podemos ver todas las señales con el siguiente comando:

~~~
Juceco@htb[/htb]$ kill -l

 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
~~~

Los más utilizados son:
|Signal|Descripcion|
|--|--|
|`1`| `SIGHUP` - Se envía a un proceso cuando se cierra la terminal que lo controla.|
|`2`| `SIGINT` - Enviado cuando un usuario presiona `[Ctrl] + C` en la terminal de control para interrumpir un proceso.|
|`3`| `SIGQUIT`: se envía cuando un usuario presiona `[Ctrl] + D` para salir.|
|`9`| `SIGKILL`: elimina inmediatamente un proceso sin operaciones de limpieza.|
|`15`| `SIGTERM` - Terminación del programa.|
|`19`| `SIGSTOP` - Detiene el programa. Ya no se puede manejar.|
|`20`| `SIGTSTP`: se envía cuando un usuario presiona `[Ctrl] + Z` para solicitar la suspensión de un servicio. El usuario puede manejarlo después.|

Por ejemplo, si un programa se congelara, podríamos forzarlo a matarlo con el siguiente comando:

~~~
Juceco@htb[/htb]$ kill 9 <PID> 
~~~
___
### PROCESOS EN SEGUNDO PLANO

En ocasiones será necesario poner en segundo plano el escaneo o proceso que acabamos de iniciar para continuar usando la sesión actual para interactuar con el sistema o iniciar otros procesos. Como ya hemos visto, esto lo podemos hacer con el atajo `[Ctrl + Z]`. Como se mencionó anteriormente, enviamos la señal `SIGTSTP` al kernel, que suspende el proceso.

~~~
Juceco@htb[/htb]$ ping -c 10 www.hackthebox.eu

Juceco@htb[/htb]$ vim tmpfile
[Ctrl + Z]
[2]+  Stopped                 vim tmpfile
~~~

Ahora todos los procesos en segundo plano se pueden mostrar con el siguiente comando.

~~~
Juceco@htb[/htb]$ jobs

[1]+  Stopped                 ping -c 10 www.hackthebox.eu
[2]+  Stopped                 vim tmpfile
~~~
El atajo `[Ctrl + Z]` suspende los procesos y no se ejecutarán más. Para mantenerlo funcionando en segundo plano, debemos ingresar el comando `bg` para poner el proceso en segundo plano.

~~~
Juceco@htb[/htb]$ bg

Juceco@htb[/htb]$ 
--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 113482ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
~~~

Otra opción es configurar automáticamente el proceso con un signo AND (`&`) al final del comando.

~~~
Juceco@htb[/htb]$ ping -c 10 www.hackthebox.eu &

[1] 10825
PING www.hackthebox.eu (172.67.1.1) 56(84) bytes of data.
~~~

Una vez finalice el proceso, veremos los resultados.

~~~
Juceco@htb[/htb]$ 

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9210ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
~~~
___
### PROCESOS A PRIMER PLANO

Después de eso, podemos usar el comando de `jobs` para enumerar todos los procesos en segundo plano. Los procesos en segundo plano no requieren la interacción del usuario, y podemos usar la misma sesión de shell sin esperar hasta que el proceso finalice primero. Una vez que el escaneo o el proceso termine su trabajo, la terminal nos notificará que el proceso ha terminado.

~~~
Juceco@htb[/htb]$ jobs

[1]+  Running                 ping -c 10 www.hackthebox.eu &
~~~

Si queremos que el proceso en segundo plano pase al primer plano e interactuar con él nuevamente, podemos usar el comando `fg <ID>`.

~~~
Juceco@htb[/htb]$ fg 1
ping -c 10 www.hackthebox.eu

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9206ms
~~~
___
### EJECUTAR MULTIPLES COMANDOS

Hay tres posibilidades para ejecutar varios comandos, uno tras otro. Estos están separados por:
+ Punto y coma (`;`)
+ Caracteres dobles de ampersand (`&&`)
+ Pipes (`|`)

La diferencia entre ellos radica en el tratamiento de los procesos anteriores y depende de si el proceso anterior se completó con éxito o con errores. El punto y coma (`;`) es un separador de comandos y ejecuta los comandos ignorando los resultados y errores de los comandos anteriores.

~~~
Juceco@htb[/htb]$ echo '1'; echo '2'; echo '3'

1
2
3
~~~

Por ejemplo, si ejecutamos el mismo comando pero lo reemplazamos en segundo lugar, el comando `ls` con un archivo que no existe, obtenemos un error y el tercer comando se ejecutará de todos modos.

~~~
Juceco@htb[/htb]$ echo '1'; ls MISSING_FILE; echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
3
~~~

Sin embargo, se ve diferente si usamos los caracteres AND dobles (`&&`) para ejecutar los comandos uno tras otro. Si hay un error en uno de los comandos, los siguientes no se ejecutarán más y se detendrá todo el proceso.

~~~
Juceco@htb[/htb]$ echo '1' && ls MISSING_FILE && echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
~~~

Pipes (`|`) dependen no solo del funcionamiento correcto y sin errores de los procesos anteriores, sino también de los resultados de los procesos anteriores. Nos ocuparemos de las canalizaciones más adelante en la sección `Descriptores de archivos y redirecciones`.
___
### RETO

+ Use el comando "systemctl" para enumerar todas las unidades de servicios y envíe el nombre de la unidad con la descripción "Load AppArmor profiles managed internally by snapd" como respuesta.

`R: snapd.apparmor.service`

~~~
htb-student@nixfund:~$ systemctl
UNIT                          LOAD   ACTIVE SUB       DESCRIPTION                           
dev-hugepages.mount           loaded active mounted   Huge Pages File System   
dev-mqueue.mount              loaded active mounted   POSIX Message Queue File System
run-user-1002.mount           loaded active mounted   /run/user/1002           
run-vmblock\x2dfuse.mount     loaded active mounted   VMware vmblock fuse mount
snap-core-10126.mount         loaded active mounted   Mount unit for core, revision 10126
snap-core-10185.mount         loaded active mounted   Mount unit for core, revision 10185
snap-core18-1885.mount        loaded active mounted   Mount unit for core18, revision 1885
snap-core18-1932.mount        loaded active mounted   Mount unit for core18, revision 1932
snap-powershell-137.mount     loaded active mounted   Mount unit for powershell, revision 137
snap-powershell-149.mount     loaded active mounted   Mount unit for powershell, revision 149
apache2.service               loaded active running   The Apache HTTP Server   
apparmor.service              loaded active exited    AppArmor initialization  
apport.service                loaded active exited    LSB: automatic crash report generation
atd.service                   loaded active running   Deferred execution scheduler
blk-availability.service      loaded active exited    Availability of block devices
cloud-config.service          loaded active exited    Apply the settings specified in cloud-config
cloud-final.service           loaded active exited    Execute cloud user/final scripts
cloud-init-local.service      loaded active exited    Initial cloud-init job (pre-networking)
console-setup.service         loaded active exited    Set console font and keymap
cron.service                  loaded active running   Regular background program processing daemon
dbus.service                  loaded active running   D-Bus System Message Bus 
dovecot.service               loaded active running   Dovecot IMAP/POP3 email server
ebtables.service              loaded active exited    ebtables ruleset management
getty@tty1.service            loaded active running   Getty on tty1            
grub-common.service           loaded active exited    LSB: Record successful boot for GRUB
ifup@ens192.service           loaded active exited    ifup for ens192          
irqbalance.service            loaded active running   irqbalance daemon        
keyboard-setup.service        loaded active exited    Set the console keyboard layout
lxcfs.service                 loaded active running   FUSE filesystem for LXC  
lxd-containers.service        loaded active exited    LXD - container startup/shutdown
mysql.service                 loaded active running   MySQL Community Server   
networkd-dispatcher.service   loaded active running   Dispatcher daemon for systemd-networkd
networking.service            loaded active exited    Raise network interfaces 
nmbd.service                  loaded active running   Samba NMB Daemon         
open-vm-tools.service         loaded active running   Service for virtual machines hosted on VMware
polkit.service                loaded active running   Authorization Manager    
postfix.service               loaded active exited    Postfix Mail Transport Agent
postfix@-.service             loaded active running   Postfix Mail Transport Agent (instance -)
proftpd.service               loaded active running   LSB: Starts ProFTPD daemon
rsyslog.service               loaded active running   System Logging Service   
setvtrgb.service              loaded active exited    Set console scheme       
smbd.service                  loaded active running   Samba SMB Daemon         
snapd.apparmor.service        loaded active exited    Load AppArmor profiles managed internally by snapd
snapd.seeded.service          loaded active exited    Wait until snapd is fully seeded
snapd.service                 loaded active running   Snap Daemon              
ssh.service                   loaded active running   OpenBSD Secure Shell server
~~~___
#### [Anterior (Gestion de Paquetes)]()
#### [Siguiente (Trabajar con Servicios Web)]()