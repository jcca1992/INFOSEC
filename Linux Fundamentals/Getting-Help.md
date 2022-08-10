## OBTENIENDO AYUDA
___
Siempre nos tropezaremos con herramientas cuyos parámetros opcionales no conocemos de memoria o herramientas que nunca antes habíamos visto. Por lo tanto, es vital saber cómo podemos ayudarnos a familiarizarnos con esas herramientas. Las dos primeras formas son las páginas `man` y las funciones de ayuda. Siempre es una buena idea familiarizarnos primero con la herramienta que queremos probar. También aprenderemos algunos posibles trucos con algunas de las herramientas que creíamos que no eran posibles. En las páginas del manual, encontraremos los manuales detallados con explicaciones detalladas.

`Sintaxis:`
~~~
Juceco@htb[/htb]$ man <tool>
~~~

Echemos un vistazo a un ejemplo:

`Ejemplo:`
~~~
Juceco@htb[/htb]$ man curl

curl(1)                                                             Curl Manual                                                            curl(1)

NAME
       curl - transfer a URL

SYNOPSIS
       curl [options] [URL...]

DESCRIPTION
       curl  is  a tool to transfer data from or to a server, using one of the supported protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS,  
       IMAP, IMAPS,  LDAP,  LDAPS,  POP3,  POP3S,  RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET, and TFTP). The command is designed to work without user interaction.

       curl offers a busload of useful tricks like proxy support, user authentication, FTP upload, HTTP post, SSL connections, cookies, file transfer resume, Metalink,  and more. As we will see below, the number of features will make our head spin!

       curl is powered by libcurl for all transfer-related features.  See libcurl(3) for details.

Manual page curl(1) line 1 (press h for help or q to quit)
~~~

Después de ver algunos ejemplos, también podemos ver rápidamente los parámetros opcionales sin navegar por la documentación completa. Tenemos varias formas de hacerlo.

`Sintaxis:`
~~~
Juceco@htb[/htb]$ <tool> --help
~~~

`Ejemplo:`
~~~
Juceco@htb[/htb]$ curl --help

Usage: curl [options...] <url>
     --abstract-unix-socket <path> Connect via abstract Unix domain socket
     --anyauth       Pick any authentication method
 -a, --append        Append to target file when uploading
     --basic         Use HTTP Basic Authentication
     --cacert <file> CA certificate to verify peer against
     --capath <dir>  CA directory to verify peer against
 -E, --cert <certificate[:password]> Client certificate file and password
<SNIP>
~~~
También podemos usar la versión corta de la misma:

`Sintaxis:`
~~~
Juceco@htb[/htb]$ <tool> -h
~~~

`Ejemplo:`
~~~
Juceco@htb[/htb]$ curl -h

Usage: curl [options...] <url>
     --abstract-unix-socket <path> Connect via abstract Unix domain socket
     --anyauth       Pick any authentication method
 -a, --append        Append to target file when uploading
     --basic         Use HTTP Basic Authentication
     --cacert <file> CA certificate to verify peer against
     --capath <dir>  CA directory to verify peer against
 -E, --cert <certificate[:password]> Client certificate file and password
<SNIP>
~~~
Como podemos ver, los resultados entre sí no difieren en este ejemplo. Otra herramienta que puede ser útil al principio es `apropos`. Cada página del manual tiene una breve descripción disponible dentro de ella. Esta herramienta busca en las descripciones instancias de una palabra clave determinada.

`Sintaxis:`
~~~
Juceco@htb[/htb]$ apropos <keyword>
~~~

`Ejemplo:`
~~~
Juceco@htb[/htb]$ apropos sudo

sudo (8)             - execute a command as another user
sudo.conf (5)        - configuration for sudo front end
sudo_plugin (8)      - Sudo Plugin API
sudo_root (8)        - How to run administrative commands
sudoedit (8)         - execute a command as another user
sudoers (5)          - default sudo security policy plugin
sudoreplay (8)       - replay sudo session logs
visudo (8)           - edit the sudoers file
~~~

Otro recurso útil para obtener ayuda si tenemos problemas para entender un comando largo es: https://explainshell.com/
___
#### [Anterior (Descripcion del Prompt)]()
#### [Siguiente (Informacion del Sistema)]()
