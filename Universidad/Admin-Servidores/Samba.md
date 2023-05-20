## SAMBA

Un Servidor Samba es un conjunto de aplicaciones Linux, basadas en el protocolo `SMB`, que permiten compartir archivos en red.

Nos permite crear un `Servidor de Archivos` y Recursos compartidos, y administrarlos otorgando los permisos deseados a los usuarios y grupos que creemos.

De esta forma podremos compartir archivos y directorios desde equipos Linux a equipos Windows (y también con equipos Linux, claro). También podremos compartir impresoras.
___
### Configuracion de Samba

Primero actualizamos e instalamos samba

~~~
ciber@leones:~$sudo apt update
Obj:1 http://cl.archive.ubuntu.com/ubuntu jammy InRelease
Obj:2 http://cl.archive.ubuntu.com/ubuntu jammy-updates InRelease
Obj:3 http://cl.archive.ubuntu.com/ubuntu jammy-backports InRelease
Obj:4 http://cl.archive.ubuntu.com/ubuntu jammy-security InRelease
Leyendo lista de paquetes...
Creando árbol de dependencias...
Leyendo la información de estado...
Todos los paquetes están actualizados.
~~~
~~~
ciber@leones:~$sudo apt upgrade
Leyendo lista de paquetes...
Creando árbol de dependencias...
Leyendo la información de estado...
Calculando la actualización...
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
~~~

En este caso ya estan instalados y actualizados

Procedemos a instalar `samba` con el siguiente comando

~~~
ciber@leones:~$sudo apt install samba
~~~

Revisamos el estatus de samba

~~~
ciber@leones:~$sudo systemctl status smbd
● smbd.service - Samba SMB Daemon
     Loaded: loaded (/lib/systemd/system/smbd.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2023-05-20 18:16:53 UTC; 15min ago
       Docs: man:smbd(8)
             man:samba(7)
             man:smb.conf(5)
    Process: 805 ExecStartPre=/usr/share/samba/update-apparmor-samba-profile (code=exited, status=0/SUCCESS)
   Main PID: 809 (smbd)
     Status: "smbd: ready to serve connections..."
      Tasks: 5 (limit: 2234)
     Memory: 21.4M
        CPU: 387ms
     CGroup: /system.slice/smbd.service
             ├─ 809 /usr/sbin/smbd --foreground --no-process-group
             ├─ 816 /usr/sbin/smbd --foreground --no-process-group
             ├─ 817 /usr/sbin/smbd --foreground --no-process-group
             ├─ 818 /usr/lib/x86_64-linux-gnu/samba/samba-bgqd --ready-signal-fd=45 --parent-watch-fd=11 --debuglevel=0 -F
             └─2902 /usr/sbin/smbd --foreground --no-process-group

may 20 18:16:52 leones systemd[1]: Starting Samba SMB Daemon...
may 20 18:16:53 leones systemd[1]: Started Samba SMB Daemon.
may 20 18:26:44 leones smbd[2896]: pam_unix(samba:session): session closed for user nobody
may 20 18:27:02 leones smbd[2897]: pam_unix(samba:session): session closed for user nobody
may 20 18:27:15 leones smbd[2902]: pam_unix(samba:session): session opened for user ciber(uid=1000) by (uid=0)
~~~

Como podemos observar esta habilitado (`enabled`) y activo (`active running`)

Tenemos que revisar el estatus del firewall (`ufw`) y darle permiso a samba

~~~
ciber@leones:~$sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                                   
22/tcp (v6)                ALLOW       Anywhere (v6)             
~~~

Solo esta permitido el puerto 22 con protocolo tcp

~~~
ciber@leones:~$sudo ufw allow samba
Rule added
Rule added (v6)
~~~
~~~
ciber@leones:~$sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                  
Samba                      ALLOW       Anywhere                  
22/tcp (v6)                ALLOW       Anywhere (v6)             
Samba (v6)                 ALLOW       Anywhere (v6) 
~~~

Con el siguiente comando podemos ver las politicas por defecto en el apartado `Default`, en este caso se esta denegando `deny (incoming)` el trafico entrante pero permitiendo el trafico saliente `allow (outgoing)`

~~~
ciber@leones:~$sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
137,138/udp (Samba)        ALLOW IN    Anywhere                  
139,445/tcp (Samba)        ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
137,138/udp (Samba (v6))   ALLOW IN    Anywhere (v6)             
139,445/tcp (Samba (v6))   ALLOW IN    Anywhere (v6)             
~~~

Ahora tenemos que crear las carpetas `samba`y dentro de samba `public`
~~~
ciber@leones:~$ sudo mkdir samba
ciber@leones:~$ cd samba
ciber@leones:~/samba$ sudo mkdir public
~~~

Al crear las carpetas recordemos que quedan con propietario y grupo `root`

~~~
ciber@leones:~/samba$ ls -l
total 4
drwxr-xr-x 2 root root 4096 may 20 01:23 public
~~~

Cambiaremos los permisos de la carpeta `public` para que nadie sea el propietario y no este en ningun grupo
~~~
ciber@leones:~/samba$ sudo chown -R nobody:nogroup public/
~~~

Daremos todos los permisos a todos (`0777`)
~~~
ciber@leones:~/samba$ sudo chmod -R 0777 public/
~~~

Ahora cambiaremos el grupo a `sambashare`
~~~
ciber@leones:~/samba$ sudo chgrp sambashare public/
~~~
~~~
ciber@leones:~/samba$ ls -l
total 4
drwxrwxrwx 2 nobody sambashare 4096 may 20 18:24 public
~~~

Crearemos el usuario y la clave para acceder a samba

la sinopsis del comando `smbpasswd` es: 
~~~
smbpasswd [opciones] [usuario]
~~~
~~~
ciber@leones:~/samba$ sudo smbpasswd -L	-a ciber
New SMB password:
Retype new SMB password:
added user ciber.
~~~
la flag `-L`indica que es en modo local
la flag `-a`indica que se esta agregando usuario


Habilitamos el usuario
~~~
ciber@leones:~/samba$ sudo smbpasswd -L -e ciber
Enabled user ciber.
~~~

Ahora procedemos a editar el documento de configuracion de samba
~~~
ciber@leones:~/samba$ sudo nano /etc/samba/smb.conf
~~~
~~~
# This is the main Samba configuration file. You should read the
# smb.conf(5) manual page in order to understand the options listed
# here. Samba has a huge number of configurable options most of which 
# are not shown in this example
#
# Some options that are often worth tuning have been included as
# commented-out examples in this file.
#  - When such options are commented with ";", the proposed setting
#    differs from the default Samba behaviour
#  - When commented with "#", the proposed setting is the default
#    behaviour of Samba but the option is considered important
#    enough to be mentioned here
#
# NOTE: Whenever you modify this file you should run the command
# "testparm" to check that you have not made any basic syntactic 
# errors. 

...

# Windows clients look for this share name as a source of downloadable
# printer drivers
[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
# Uncomment to allow remote administration of Windows print drivers.
# You may need to replace 'lpadmin' with the name of the group your
# admin users are members of.
# Please note that you also need to set appropriate Unix permissions
# to the drivers directory for these users to have write rights in it
;   write list = root, @lpadmin
~~~

Al final escribimos las siguientes configuraciones de la carpeta `public`
~~~
...
# to the drivers directory for these users to have write rights in it
;   write list = root, @lpadmin

[public]
   path = /home/ciber/samba/public
   guest ok = no
   writeable = yes
   write list = ciber
   browseable = yes
   create mask = 0777
   directory mask = 0777
~~~

Reiniciamos el servicio `smbd`
~~~
ciber@leones:~/samba$ sudo /etc/init.d/smbd restart
Restarting smbd (via systemctl): smbd.service.
~~~

Ahora solo debemos revisar la `ip` con el comando `ifconfig` e ingresar desde el explorador de archivos

~~~
ciber@leones:~/samba$ ifconfig
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.132  netmask 255.255.255.0  broadcast 192.168.1.255
...
~~~

![Explorador de archivos](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/Explo_archivos.jpg)


colocamos la ip del servidor con doble barra invertida (`\\`)

![Explorador de archivos IP](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/Explo_archivos_ip.jpg)

Aparece la carpeta publica y accedemos con el usuario y la contraseña que habiamos creado

![credenciales](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/Credenciales.jpg)