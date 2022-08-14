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






___
#### [Anterior (Gestion de Paquetes)]()
#### [Siguiente (Trabajar con Servicios Web)]()