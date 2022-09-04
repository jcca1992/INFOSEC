## NAVEGACION
___
Una de las mejores maneras de aprender algo nuevo es experimentar con ello. Aquí cubrimos las secciones sobre cómo navegar a través de Linux, crear, mover, editar y eliminar archivos y carpetas, encontrarlos en el sistema operativo, diferentes tipos de redireccionamientos y qué son los descriptores de archivos. Luego encontraremos algunos atajos que facilitarán nuestro trabajo con el shell. Se recomienda experimentar en nuestra máquina virtual alojada localmente. Asegúrese de haber creado un snapshot para nuestra VM en caso de que nuestro sistema se dañe inesperadamente.

Comencemos con la navegación. Antes de movernos por el sistema, tenemos que averiguar en qué directorio estamos. Podemos saber dónde estamos con el comando `pwd`.

~~~
cry0l1t3@htb[~]$ pwd

/home/cry0l1t3
~~~

Solo se necesita el comando `ls` para la navegación. Tiene muchas opciones adicionales que pueden complementar la visualización del contenido en la carpeta actual.

~~~
cry0l1t3@htb[~]$ ls

Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
~~~

~~~
cry0l1t3@htb[~]$ ls -l

total 32
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:37 Desktop
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Documents
drwxr-xr-x 3 cry0l1t3 cry0l1t3 4096 Nov 15 03:26 Downloads
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Music
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Pictures
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Public
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Templates
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Videos
~~~

Sin embargo, no veremos todo lo que hay en esta carpeta. Para enumerar el contenido de un directorio, no necesariamente necesitamos navegar allí primero. También podemos usar "ls" para especificar la ruta donde queremos conocer los contenidos.

~~~
cry0l1t3@htb[~]$ ls -l /var/

total 52
drwxr-xr-x  2 root root     4096 Mai 15 18:54 backups
drwxr-xr-x 18 root root     4096 Nov 15 16:55 cache
drwxrwsrwt  2 root whoopsie 4096 Jul 25  2018 crash
drwxr-xr-x 66 root root     4096 Mai 15 03:08 lib
drwxrwsr-x  2 root staff    4096 Nov 24  2018 local 
<SNIP>
~~~

Podemos hacer lo mismo si queremos navegar hacia el directorio. Para movernos por los directorios, usamos el comando `cd`. Cambiemos al directorio `/dev/shm`. Por supuesto, podemos ir primero al directorio `/dev` y luego a `/shm`. No obstante, también podemos entrar directamente en la ruta completa y saltar directamente allí.

~~~
cry0l1t3@htb[~]$ cd /dev/shm

cry0l1t3@htb[/dev/shm]$
~~~

Como antes estábamos en el directorio de inicio, podemos volver rápidamente al directorio en el que estuvimos por última vez.

~~~
cry0l1t3@htb[/dev/shm]$ cd -

cry0l1t3@htb[~]$ 
~~~

La shell también nos ofrece la función de autocompletar, que facilita la navegación. Si ahora escribimos `cd /dev/s` y luego presionamos `[TAB] dos veces`, obtendremos todas las entradas que comienzan con la letra "`s`" en el directorio de `/dev/`.

~~~
cry0l1t3@htb[~]$ cd /dev/s [TAB 2x]

shm/ snd/
~~~

Si agregamos la letra "`h`" a la letra "`s`", el shell completará la entrada, ya que de lo contrario no habrá carpetas en este directorio que comiencen con las letras "`sh`". Si ahora mostramos todo el contenido del directorio, solo veremos los siguientes contenidos.

~~~
cry0l1t3@htb[/dev/shm]$ ls -la

total 0
drwxrwxrwt  2 root root   40 Mai 15 18:31 .
drwxr-xr-x 17 root root 4000 Mai 14 20:45 ..
~~~

La primera entrada con un solo punto (`.`) indica el directorio actual en el que nos encontramos actualmente. La segunda entrada con dos puntos (`..`) representa el directorio principal `/dev`. Esto significa que podemos saltar al directorio principal con el siguiente comando.

~~~
cry0l1t3@htb[/dev/shm]$ cd ..

cry0l1t3@htb[/dev]$
~~~

Dado que nuestro shell está lleno de algunos registros, podemos limpiar el shell con el comando `clear`. Sin embargo, volvamos al directorio `/dev/shm` anterior.

~~~
cry0l1t3@htb[/dev]$ cd shm && clear
~~~
___
### RETO

¿Cuál es el nombre del archivo de "history" oculto en el directorio home del usuario htb?

`R: .bash_history`

~~~
htb-student@nixfund:~$ ls -la
total 32
drwxr-xr-x 4 htb-student htb-student 4096 Aug  3  2021 .
drwxr-xr-x 5 root        root        4096 Aug  3  2021 ..
-rw------- 1 htb-student htb-student    5 Sep 23  2020 .bash_history
-rw-r--r-- 1 htb-student htb-student  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 htb-student htb-student 3771 Apr  4  2018 .bashrc
drwx------ 2 htb-student htb-student 4096 Aug  3  2021 .cache
drwx------ 3 htb-student htb-student 4096 Aug  3  2021 .gnupg
-rw-r--r-- 1 htb-student htb-student  807 Apr  4  2018 .profile
~~~


¿Cuál es el número de índice del archivo "sudoers" en el directorio "/etc"?

`R: 147627`

~~~
htb-student@nixfund:/etc$ ls -i
<SNIP>
146891 cron.d                         146910 iscsi            148303 passwd-                  147627 sudoers
146892 cron.daily                     148324 issue            146927 perl                     146948 sudoers.d
146893 cron.hourly                    148325 issue.net        148876 php                      147628 sysctl.conf
146894 cron.monthly                   146911 kernel           146928 pm                       146949 sysctl.d
<SNIP>
~~~
___
#### [Anterior (Trabajar con Servicios Web)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Work-Web-Service.md)
#### [Siguiente (Trabajando con Archivos y Directorios)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Work-File-Directories.md)