## FILTRO DE CONTENIDO
___
En la última sección, aprendimos sobre las redirecciones que podemos usar para redirigir los resultados de un programa a otro para su procesamiento. Para leer archivos, no necesariamente tenemos que usar un editor para eso. Hay dos herramientas llamadas `more` y `less`, que son muy idénticas. Son `pagers` fundamentales que nos permiten desplazarnos por el archivo en una vista interactiva. Echemos un vistazo a algunos ejemplos.
___
### MORE

~~~
Juceco@htb[/htb]$ more /etc/passwd
~~~

Después de leer el contenido usando `cat` y redirigirlo a `more`, se abre el `pager` mencionado anteriormente y automáticamente comenzaremos desde el principio del archivo.

~~~
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
--More--
~~~

Con la tecla `[Q]` podemos salir de este `pager`. Notaremos que la salida permanece en la terminal.
___
### LESS

Si ahora echamos un vistazo a la herramienta `less`, notaremos en la página de manual que contiene muchas más funciones que `more`.

~~~
Juceco@htb[/htb]$ less /etc/passwd
~~~

La presentación es casi la misma que con `more`.

~~~
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
:
~~~

Al cerrar less con la tecla `[Q]`, notaremos que la salida que hemos visto, a diferencia de `more`, no se queda en el terminal.
___
### HEAD

En ocasiones solo nos interesarán temas puntuales ya sea al principio del archivo o al final. Si solo queremos obtener `las primeras` líneas del archivo, podemos usar la herramienta `head`. Por defecto, `head` imprime las primeras diez líneas del archivo o entrada dada, si no se especifica lo contrario.

~~~
Juceco@htb[/htb]$ head /etc/passwd

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
~~~
___
### TAIL

Si solo queremos ver las últimas partes de un archivo o resultados, podemos usar la contrapartida de `head` llamada `tail`, que devuelve las últimas diez líneas.

~~~
Juceco@htb[/htb]$ tail /etc/passwd

miredo:x:115:65534::/var/run/miredo:/usr/sbin/nologin
usbmux:x:116:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
rtkit:x:117:119:RealtimeKit,,,:/proc:/usr/sbin/nologin
nm-openvpn:x:118:120:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
nm-openconnect:x:119:121:NetworkManager OpenConnect plugin,,,:/var/lib/NetworkManager:/usr/sbin/nologin
pulse:x:120:122:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
beef-xss:x:121:124::/var/lib/beef-xss:/usr/sbin/nologin
lightdm:x:122:125:Light Display Manager:/var/lib/lightdm:/bin/false
do-agent:x:998:998::/home/do-agent:/bin/false
user6:x:1000:1000:,,,:/home/user6:/bin/bash
~~~
___
### SORT

Dependiendo de qué resultados y archivos se traten, rara vez se ordenan. A menudo, es necesario ordenar los resultados deseados alfabética o numéricamente para obtener una mejor visión general. Para ello, podemos utilizar una herramienta llamada `sort`.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | sort

_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
dovecot:x:114:117:Dovecot mail server,,,:/usr/lib/dovecot:/usr/sbin/nologin
dovenull:x:115:118:Dovecot login user,,,:/nonexistent:/usr/sbin/nologin
ftp:x:113:65534::/srv/ftp:/usr/sbin/nologin
games:x:5:60:games:/usr/games:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
htb-student:x:1002:1002::/home/htb-student:/bin/bash
<SNIP>
~~~
Como podemos ver ahora, la salida ya no comienza con la raíz, sino que ahora está ordenada alfabéticamente.
___
### GREP

Más a menudo, solo buscaremos resultados específicos que contengan patrones que hayamos definido. Una de las herramientas más utilizadas para esto es `grep`, que ofrece muchas características diferentes. En consecuencia, podemos buscar usuarios que tengan la shell predeterminado "`/bin/bash`" establecido como ejemplo.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep "/bin/bash"

root:x:0:0:root:/root:/bin/bash
mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
htb-student:x:1002:1002::/home/htb-student:/bin/bash
~~~

Otra posibilidad es excluir resultados específicos. Para ello se utiliza la opción "`-v`" con `grep`. En el siguiente ejemplo, excluimos a todos los usuarios que han deshabilitado la shell estándar con el nombre "`/bin/false`" o "`/usr/bin/nologin`".

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin"

root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
postgres:x:111:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
user6:x:1000:1000:,,,:/home/user6:/bin/bash
~~~
___
### CUT

Los resultados específicos con diferentes caracteres se pueden separar como delimitadores. Aquí es útil saber cómo eliminar delimitadores específicos y mostrar las palabras en una línea en una posición específica. Una de las herramientas que se pueden utilizar para esto es `cut`. Por lo tanto, usamos la opción "`-d`" y establecemos el delimitador en el carácter de dos puntos (`:`) y definimos con la opción "`-f`" la posición en la línea que queremos generar.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1

root
sync
mrb3n
cry0l1t3
htb-student
~~~
___
### TR

Otra posibilidad de reemplazar ciertos caracteres de una línea con caracteres definidos por nosotros es la herramienta `tr`. Como primera opción definimos qué carácter queremos reemplazar, y como segunda opción definimos el carácter por el que queremos reemplazarlo. En el siguiente ejemplo, reemplazamos el carácter de dos puntos con un espacio.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "

root x 0 0 root /root /bin/bash
sync x 4 65534 sync /bin /bin/sync
mrb3n x 1000 1000 mrb3n /home/mrb3n /bin/bash
cry0l1t3 x 1001 1001  /home/cry0l1t3 /bin/bash
htb-student x 1002 1002  /home/htb-student /bin/bash
~~~
___
### COLUMN

Dado que tales resultados a menudo pueden tener una representación poco clara, la herramienta `column` es adecuada para mostrar dichos resultados en forma tabular usando la "`-t`".

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t

root         x  0     0      root               /root        /bin/bash
sync         x  4     65534  sync               /bin         /bin/sync
mrb3n        x  1000  1000   mrb3n              /home/mrb3n  /bin/bash
cry0l1t3     x  1001  1001   /home/cry0l1t3     /bin/bash
htb-student  x  1002  1002   /home/htb-student  /bin/bash
~~~
___
### AWK

Como nos habrámos dado cuenta, el usuario "`postgres`" tiene una fila de más. Para simplificar al máximo la clasificación de dichos resultados, la programación (`g`)`awk` es útil, ya que nos permite mostrar el primer (`$1`) y el último (`$NF`) resultado de la línea.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'

root /bin/bash
sync /bin/sync
mrb3n /bin/bash
cry0l1t3 /bin/bash
htb-student /bin/bash
~~~
___
### SED

Llegarán momentos en los que querremos cambiar nombres específicos en todo el archivo o entrada estándar. Una de las herramientas que podemos usar para esto es el editor de secuencias llamado `sed`. Uno de los usos más comunes de esto es sustituir texto. Aquí, `sed` busca patrones que hemos definido en forma de expresiones regulares (`regex`) y los reemplaza con otro patrón que también hemos definido. Centrémonos en los últimos resultados y digamos que queremos reemplazar la palabra "`bin`" con "`HTB`".

El indicador "`s`" al principio representa el comando sustituto. Luego especificamos el patrón que queremos reemplazar. Después de la barra inclinada (`/`), ingresamos el patrón que queremos usar como reemplazo en la tercera posición. Finalmente, usamos el indicador "`g`", que significa reemplazar todas las coincidencias.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'

root /HTB/bash
sync /HTB/sync
mrb3n /HTB/bash
cry0l1t3 /HTB/bash
htb-student /HTB/bash
~~~
___
### WC

Por último, pero no menos importante, a menudo será útil saber cuántas coincidencias exitosas tenemos. Para evitar contar las líneas o caracteres manualmente, podemos utilizar la herramienta `wc`. Con la opción "`-l`", especificamos que solo se cuentan las líneas.

~~~
Juceco@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l

5
~~~
___
### PRACTICA

Puede ser un poco abrumador al principio lidiar con tantas herramientas diferentes y sus funciones si no estamos familiarizados con ellas. Tómese su tiempo y experimente con las herramientas. Eche un vistazo a las páginas man (`man <herramienta>`) o llame a la ayuda (`<herramienta> -h` / `<herramienta> --help`). La mejor manera de familiarizarse con todas las herramientas es practicar. Intenta usarlos con la mayor frecuencia posible, y podremos filtrar muchas cosas de manera intuitiva después de poco tiempo.
___
### RETO

+ ¿Cuántos servicios están escuchando en el sistema en todas las interfaces? (No en localhost e IPv4 solamente)

`R: 7`

~~~
htb-student@nixfund:~$ netstat -ln4 |grep LISTEN| grep -v 127 | wc -l
7
~~~
___
+ Determine con qué usuario se está ejecutando el servidor ProFTPd. Envíe el nombre de usuario como respuesta.

`R: proftpd`

~~~
htb-student@nixfund:~$ cat /etc/passwd |grep proftpd
proftpd:x:112:65534::/run/proftpd:/usr/sbin/nologin
~~~

~~~
htb-student@nixfund:~$ ps aux | grep proftpd
proftpd   1616  0.0  0.1 126444  3692 ?        Ss   14:49   0:00 proftpd: (accepting connections)
htb-stu+  5917  0.0  0.0  14864  1016 pts/0    R+   15:08   0:00 grep --color=auto proftpd
~~~
___
+ Use cURL desde su Pwnbox (no desde la máquina de destino) para obtener el código fuente del sitio web "https://www.inlanefreight.com" y filtre todas las rutas únicas de ese dominio. Envíe el número de estas rutas como respuesta

`R: 34`

___
#### [Anterior (Descriptores y Redirecciones de Archivos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/File-Descriptors-Redireccion.md)
#### [Siguiente (Gestion de Permisos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Perm-Management.md)