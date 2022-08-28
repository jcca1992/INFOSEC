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
___
#### [Anterior (Descriptores y Redirecciones de Archivos)]()
#### [Siguiente (Gestion de Permisos)]()