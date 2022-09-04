## TRABAJANDO CON ARCHIVOS Y DIRECTORIOS
___
Crear, mover y copiar

A continuación, trabajemos con archivos y directorios y aprendamos a crear, renombrar, mover, copiar y eliminar. Primero, creemos un archivo vacío y un directorio. Podemos usar touch para crear un archivo vacío y mkdir para crear un directorio.

La sintaxis para esto es la siguiente:

**Touch:**
~~~
Juceco@htb[/htb]$ touch <name>
~~~

**mkdir:**
~~~
Juceco@htb[/htb]$ mkdir <name>
~~~

En este ejemplo, llamamos al archivo info.txt y el directorio Storage. Para crearlos, seguimos los comandos y su sintaxis que se muestran arriba.

**Crear un Archivo Vacio:**
~~~
Juceco@htb[/htb]$ touch info.txt
~~~

**Crear un Directorio:**
~~~
Juceco@htb[/htb]$ mkdir Storage
~~~

Es posible que queramos tener directorios específicos en el directorio, y llevaría mucho tiempo crear este comando para cada directorio. El comando `mkdir` tiene una opción con el indicador `-p` para agregar directorios principales.

~~~
Juceco@htb[/htb]$ mkdir -p Storage/local/user/documents
~~~

Podemos ver toda la estructura después de crear los directorios principales con la herramienta `tree`.

~~~
Juceco@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            └── documents

4 directories, 1 file
~~~

También podemos crear archivos directamente en los directorios especificando la ruta donde se debe almacenar el archivo. El truco es usar el punto (`.`) para decirle al sistema que queremos comenzar desde el directorio actual. Entonces, el comando para crear otro archivo vacío se ve así:

~~~
Juceco@htb[/htb]$ touch ./Storage/local/user/userinfo.txt
~~~

~~~
Juceco@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            ├── documents
            └── userinfo.txt

4 directories, 2 files
~~~

Con el comando `mv` podemos mover y también renombrar archivos y directorios. La sintaxis para esto se ve así:

`mv`
~~~
Juceco@htb[/htb]$ mv <file/directory> <renamed file/directory>
~~~

Primero, cambiemos el nombre del archivo `info.txt` a `information.txt` y luego muévalo al directorio `Storage`.

**Renombrar Archivos:**
~~~
Juceco@htb[/htb]$ mv info.txt information.txt
~~~

Ahora vamos a crear un archivo llamado `readme.txt` en el directorio actual y luego copiar los archivos `information.txt` y `readme.txt` en el directorio `Storage/`.

**Crear el readme.txt:**
~~~
Juceco@htb[/htb]$ touch readme.txt 
~~~

**Mover el Archivo al Directorio Especifico:**
~~~
Juceco@htb[/htb]$ mv information.txt readme.txt Storage/
~~~

~~~
Juceco@htb[/htb]$ tree .

.
└── Storage
    ├── information.txt
    ├── local
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 3 files
~~~

Supongamos que queremos tener el archivo `readme.txt` en el directorio `local/`. Luego podemos copiarlos allí con las rutas especificadas.

**Copiar readme.txt:**
~~~
Juceco@htb[/htb]$ cp Storage/readme.txt Storage/local/
~~~

Ahora podemos comprobar si el archivo vuelve a utilizar la herramienta `tree`.

~~~
Juceco@htb[/htb]$ tree .

.
└── Storage
    ├── information.txt
    ├── local
    │   ├── readme.txt
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 4 files
~~~
___
### RETO

+ ¿Cuál es el nombre del último archivo modificado en el directorio "/var/backups"?

`R: apt.extended_states.0`

~~~
htb-student@nixfund:/var/backups$ ls -lt
total 2160
-rw-r--r-- 1 root root    41872 Nov 12  2020 apt.extended_states.0
-rw-r--r-- 1 root root     4437 Nov 12  2020 apt.extended_states.1.gz
-rw-r--r-- 1 root root   742750 Nov 11  2020 dpkg.status.0
-rw-r--r-- 1 root root   206270 Nov 11  2020 dpkg.status.1.gz
-rw-r--r-- 1 root root   206270 Nov  5  2020 dpkg.status.2.gz
-rw-r--r-- 1 root root   206270 Nov  5  2020 dpkg.status.3.gz
-rw-r--r-- 1 root root   206270 Nov  5  2020 dpkg.status.4.gz
-rw-r--r-- 1 root root   206270 Nov  5  2020 dpkg.status.5.gz
-rw-r--r-- 1 root root   206270 Nov  5  2020 dpkg.status.6.gz
-rw-r--r-- 1 root root    51200 Oct 29  2020 alternatives.tar.0
-rw-r--r-- 1 root root     4623 Oct 22  2020 apt.extended_states.2.gz
-rw-r--r-- 1 root root     2497 Oct 16  2020 alternatives.tar.1.gz
-rw-r--r-- 1 root root     4601 Oct 15  2020 apt.extended_states.3.gz
-rw-r--r-- 1 root root     2492 Sep 24  2020 alternatives.tar.2.gz
-rw-r--r-- 1 root root      367 Sep 23  2020 dpkg.statoverride.0
-rw-r--r-- 1 root root      229 Sep 23  2020 dpkg.statoverride.1.gz
<SNIP>
~~~

___
+ ¿Cuál es el número de inodo del archivo "shadow.bak" en el directorio "/var/backups"?

`R: 265293`

~~~
htb-student@nixfund:/var/backups$ ls -i
262248 alternatives.tar.0        262203 dpkg.diversions.1.gz    262311 dpkg.statoverride.3.gz  262220 dpkg.status.5.gz
262559 alternatives.tar.1.gz     262264 dpkg.diversions.2.gz    262247 dpkg.statoverride.4.gz  262230 dpkg.status.6.gz
262261 alternatives.tar.2.gz     262257 dpkg.diversions.3.gz    262250 dpkg.statoverride.5.gz  265226 group.bak
266334 apt.extended_states.0     262246 dpkg.diversions.4.gz    262236 dpkg.statoverride.6.gz  265817 gshadow.bak
266335 apt.extended_states.1.gz  262249 dpkg.diversions.5.gz    263999 dpkg.status.0           264599 passwd.bak
266430 apt.extended_states.2.gz  262235 dpkg.diversions.6.gz    262179 dpkg.status.1.gz        265293 shadow.bak
264827 apt.extended_states.3.gz  262231 dpkg.statoverride.0     262234 dpkg.status.2.gz
262233 apt.extended_states.4.gz  262205 dpkg.statoverride.1.gz  262241 dpkg.status.3.gz
262178 dpkg.diversions.0         262310 dpkg.statoverride.2.gz  262243 dpkg.status.4.gz
~~~
___
#### [Anterior (Navegando)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Navigation.md)
#### [Siguiente (Editando Archivos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Editing-Files.md)