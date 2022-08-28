## BUSCAR ARCHIVOS Y DIRECTORIOS
___
### IMPORTANCIA DE LA BUSQUEDA

Es crucial poder encontrar los archivos y carpetas que necesitamos. Una vez que hayamos accedido a un sistema basado en Linux, será fundamental encontrar archivos de configuración, scripts creados por los usuarios o el administrador, y otros archivos y carpetas. No tenemos que navegar manualmente a través de cada carpeta y verificar cuándo se modificó por última vez. Hay algunas herramientas que podemos usar para hacer este trabajo más fácil.
___
### WICH

Una de las herramientas comunes es `Wich`. Esta herramienta devuelve la ruta al archivo o enlace que se debe ejecutar. Esto nos permite determinar si programas específicos, como `cURL`, `netcat`, `wget`, `python`, `gcc`, están disponibles en el sistema operativo. Usémoslo para buscar `Python` en nuestra instancia interactiva.

~~~
Juceco@htb[/htb]$ which python

/usr/bin/python
~~~

Si el programa que buscamos no existe, no se mostrarán resultados.
___
### FIND

Otra herramienta útil es `find`. Además de la función para encontrar archivos y carpetas, esta herramienta también contiene la función para filtrar los resultados. Podemos usar parámetros de filtro como el tamaño del archivo o la fecha. También podemos especificar si solo buscamos archivos o carpetas.

**Sintaxis**
~~~
Juceco@htb[/htb]$ find <location> <options>
~~~

~~~
Juceco@htb[/htb]$ find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null

-rw-r--r-- 1 root root 136392 Apr 25 20:29 /usr/src/linux-headers-5.5.0-1parrot1-amd64/include/config/auto.conf
-rw-r--r-- 1 root root 82290 Apr 25 20:29 /usr/src/linux-headers-5.5.0-1parrot1-amd64/include/config/tristate.conf
-rw-r--r-- 1 root root 95813 May  7 14:33 /usr/share/metasploit-framework/data/jtr/repeats32.conf
-rw-r--r-- 1 root root 60346 May  7 14:33 /usr/share/metasploit-framework/data/jtr/dynamic.conf
-rw-r--r-- 1 root root 96249 May  7 14:33 /usr/share/metasploit-framework/data/jtr/dumb32.conf
-rw-r--r-- 1 root root 54755 May  7 14:33 /usr/share/metasploit-framework/data/jtr/repeats16.conf
-rw-r--r-- 1 root root 22635 May  7 14:33 /usr/share/metasploit-framework/data/jtr/korelogic.conf
-rwxr-xr-x 1 root root 108534 May  7 14:33 /usr/share/metasploit-framework/data/jtr/john.conf
-rw-r--r-- 1 root root 55285 May  7 14:33 /usr/share/metasploit-framework/data/jtr/dumb16.conf
-rw-r--r-- 1 root root 21254 May  2 11:59 /usr/share/doc/sqlmap/examples/sqlmap.conf
-rw-r--r-- 1 root root 25086 Mar  4 22:04 /etc/dnsmasq.conf
-rw-r--r-- 1 root root 21254 May  2 11:59 /etc/sqlmap/sqlmap.conf
~~~

Ahora echemos un vistazo más de cerca a las opciones que usamos en el comando anterior. Si pasamos el mouse sobre las respectivas opciones, aparecerá una pequeña ventana con una explicación. Estas explicaciones también se encontrarán en otros módulos, lo que debería ayudarnos si aún no estamos familiarizados con una de las herramientas.

|Opcion|Descripcion|
|--|--|
|`-type f`|De este modo, definimos el tipo de objeto buscado. En este caso, `'f'` significa `'archivo'`(File).|
|`-name *.conf`|
___
### LOCATE

___
### RETO

+ ¿Cuál es el nombre del archivo de configuración que se creó después del 2020-03-03 y tiene menos de 28k pero más de 25k?

+ ¿Cuántos archivos existen en el sistema que tienen la extensión ".bak"?

+ Envíe la ruta completa del binario "xxd".
___
#### [Anterior (Editando Archivos)]()
#### [Siguiente (Descriptores y Redirecciones de Archivos)]()