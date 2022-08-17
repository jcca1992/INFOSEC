## TRABAJAR CON SERVICIOS WEB
___

Otro componente esencial es la comunicación con los servidores web. Hay muchas formas diferentes de configurar servidores web en sistemas operativos Linux. Uno de los servidores web más utilizados y extendidos, además de IIS y Nginx, es Apache. Para un servidor web Apache, podemos usar módulos apropiados, que pueden encriptar la comunicación entre el navegador y el servidor web (`mod_ssl`), usarlo como servidor proxy (`mod_proxy`) o realizar manipulaciones complejas de datos de encabezado HTTP (`mod_headers`) y URL (`mod_rewrite`).

Apache ofrece la posibilidad de crear páginas web de forma dinámica utilizando lenguajes de script del lado del servidor. Los lenguajes de secuencias de comandos comúnmente utilizados son PHP, Perl o Ruby. Otros lenguajes son Python, JavaScript, Lua y .NET, que pueden usarse para esto. Podemos instalar el servidor web Apache con el siguiente comando.

~~~
Juceco@htb[/htb]$ apt install apache2 -y

Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  apache2-doc apache2-suexec-pristine | apache2-suexec-custom
The following NEW packages will be installed:
  apache2
0 upgraded, 1 newly installed, 0 to remove and 17 not upgraded.
Need to get 95,1 kB of archives.
After this operation, 535 kB of additional disk space will be used.
Get:1 http://de.archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2 amd64 2.4.29-1ubuntu4.13 [95,1 kB]
Fetched 95,1 kB in 0s (270 kB/s)   
<SNIP>
~~~

Podemos revisar si el servidor esta iniciado con systemctl

~~~
┌─[✗]─[juliocesar@PCnotebook]─[/home/juliocesar]
└──╼ #systemctl status  apache2
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; disabled; preset: disabled)
     Active: inactive (dead)
       Docs: https://httpd.apache.org/docs/2.4/
~~~

Si no esta iniciado lo podemos iniciar con `systemctl start apache2`

~~~
┌─[root@PCnotebook]─[/home/juliocesar/Downloads]
└──╼ #systemctl status  apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; disabled; preset: disabled)
     Active: active (running) since Wed 2022-08-17 16:18:35 -04; 2s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 7539 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
   Main PID: 7543 (apache2)
      Tasks: 6 (limit: 3937)
     Memory: 12.1M
        CPU: 81ms
     CGroup: /system.slice/apache2.service
             ├─7543 /usr/sbin/apache2 -k start
             ├─7545 /usr/sbin/apache2 -k start
             ├─7546 /usr/sbin/apache2 -k start
             ├─7547 /usr/sbin/apache2 -k start
             ├─7548 /usr/sbin/apache2 -k start
             └─7549 /usr/sbin/apache2 -k start

ago 17 16:18:35 PCnotebook systemd[1]: Starting The Apache HTTP Server...
ago 17 16:18:35 PCnotebook apachectl[7542]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, u>
ago 17 16:18:35 PCnotebook systemd[1]: Started The Apache HTTP Server.
~~~

Para detenerlo usamos el comando `systemctl stop apache2`

Después de haberlo iniciado, podemos navegar usando nuestro navegador a la página predeterminada (http://localhost).

![](https://academy.hackthebox.com/storage/modules/18/apache-default.png)

Esta es la página predeterminada después de la instalación y sirve para confirmar que el servidor web funciona correctamente.
___
### CURL

`cURL` es una herramienta que nos permite transferir archivos desde el shell a través de protocolos como `HTTP`, `HTTPS`, `FTP`, `SFTP`, `FTPS` o `SCP`. Esta herramienta nos brinda la posibilidad de controlar y probar sitios web de forma remota. Además del contenido de los servidores remotos, también podemos ver solicitudes individuales para ver la comunicación del cliente y el servidor. Por lo general, `cURL` ya está instalado en la mayoría de los sistemas Linux. Esta es otra razón crítica para familiarizarnos con esta herramienta, ya que puede hacer que algunos procesos sean mucho más fáciles más adelante.

~~~
Juceco@htb[/htb]$ curl http://localhost

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <!--
    Modified from the Debian original for Ubuntu
    Last updated: 2016-11-16
    See: https://launchpad.net/bugs/1288690
  -->
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
    <style type="text/css" media="screen">
...SNIP...
~~~

En la etiqueta del título, podemos ver que es el mismo texto que sale de nuestro navegador. Esto nos permite inspeccionar el código fuente del sitio web y obtener información de él. Sin embargo, volveremos a esto en otro módulo.
___
### WGET

Una alternativa a curl es la herramienta wget. Con esta herramienta podemos descargar archivos de servidores FTP o HTTP directamente desde el terminal y sirve como un buen gestor de descargas. Si usamos wget de la misma manera, la diferencia con curl es que el contenido del sitio web se descarga y almacena localmente, como se muestra en el siguiente ejemplo.

~~~
Juceco@htb[/htb]$ wget http://localhost

--2020-05-15 17:43:52--  http://localhost/
Resolving localhost (localhost)... 127.0.0.1
Connecting to localhost (localhost)|127.0.0.1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10918 (11K) [text/html]
Saving to: 'index.html'

index.html                 100%[=======================================>]  10,66K  --.-KB/s    in 0s      

2020-05-15 17:43:52 (33,0 MB/s) - ‘index.html’ saved [10918/10918]
~~~
___
### PYTHON 3

Otra opción que se usa a menudo cuando se trata de la transferencia de datos es el uso de Python 3. En este caso, el directorio raíz del servidor web es donde se ejecuta el comando para iniciar el servidor. Para este ejemplo, estamos en un directorio donde está instalado WordPress y contiene un "readme.html". Ahora, iniciemos el servidor web Python 3 y veamos si podemos acceder a él usando el navegador.

~~~
Juceco@htb[/htb]$ python3 -m http.server

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
~~~

![](https://academy.hackthebox.com/storage/modules/18/python3-browser.png)

Podemos ver qué solicitudes se realizaron si ahora miramos los eventos de nuestro servidor web Python 3.

~~~
Juceco@htb[/htb]$ python3 -m http.server

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
127.0.0.1 - - [15/May/2020 17:56:29] "GET /readme.html HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/css/install.css?ver=20100228 HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.png HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.svg?ver=20131107 HTTP/1.1" 200 -
~~~

+ Encuentre una manera de iniciar un servidor HTTP simple dentro de Pwnbox o su VM local usando "npm". Envíe el comando que inicia el servidor web en el puerto 8080 (utilice el argumento corto para especificar el número de puerto).

`R: http-server -p 8080`

http-server (Node.js)

~~~
$ npm install -g http-server   # install dependency
$ http-server -p 8080
~~~

+ Encuentre una manera de iniciar un servidor HTTP simple dentro de Pwnbox o su VM local usando "php". Envíe el comando que inicia el servidor web en el host local (127.0.0.1) en el puerto 8080.

`R: php -S 127.0.0.1:8080`

~~~
$ php -S 127.0.0.1:8000
~~~
___
#### [Anterior (Gestion de Servicios y Procesos)]()
#### [Siguiente (Navegacion)]()