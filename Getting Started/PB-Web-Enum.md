## **ENUMERACION WEB**

Al realizar un análisis de servicios, a menudo nos encontramos con servidores web que se ejecutan en los puertos 80 y 443. Los servidores web alojan aplicaciones web (a veces más de 1) que a menudo proporcionan una superficie de ataque considerable y un objetivo de gran valor durante una prueba de penetración. La enumeración web adecuada es crítica, especialmente cuando una organización no está exponiendo muchos servicios o esos servicios están debidamente parcheados. 

___

### GOBUSTER

Después de descubrir una aplicación web, siempre vale la pena comprobar si podemos descubrir archivos o directorios ocultos en el servidor web que no estén destinados al acceso público. Podemos usar una herramienta como ffuf o GoBuster para realizar esta enumeración de directorios. A veces encontraremos funciones ocultas o páginas/directorios que exponen datos confidenciales que se pueden aprovechar para acceder a la aplicación web o incluso a la ejecución remota de código en el propio servidor web. 

___
### ENUMERACION DE DIRECTORIOS/ARCHIVOS

GoBuster es una herramienta versátil que permite realizar fuerza bruta de DNS, vhost y directorios. La herramienta tiene una funcionalidad adicional, como la enumeración de depósitos públicos de AWS S3. Para los propósitos de este módulo, estamos interesados en los modos de fuerza bruta del directorio (y archivo) especificados con el switch `dir`. Ejecutemos un escaneo simple usando la worldlist `dirb common.txt.`

~~~
$ gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.10.121/
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/12/11 21:47:25 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htpasswd (Status: 403)
/.htaccess (Status: 403)
/index.php (Status: 200)
/server-status (Status: 403)
/wordpress (Status: 301)
===============================================================
2020/12/11 21:47:46 Finished
===============================================================
~~~

Un status code HTTP de 200 revela que la solicitud del recurso fue exitosa, mientras que un status code HTTP 403 indica que tenemos prohibido acceder al recurso. Un status code 301 indica que estamos siendo redirigidos, lo cual no es un caso de falla. Vale la pena familiarizarse con los distintos status code HTTP, que se pueden encontrar [aquí](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). El módulo de la Academia de solicitudes web también cubre los status code HTTP con mayor profundidad. 

El escaneo se completó con éxito e identifica una instalación de WordPress en `/wordpress`. WordPress es el CMS (Content Management System) Sistema de gestión de contenido más utilizado y tiene una superficie de ataque potencial enorme. En este caso, visitar `http://10.10.10.121/wordpress` en un navegador revela que WordPress todavía está en modo de configuración, lo que nos permitirá obtener la ejecución remota de código (RCE) en el servidor. 

![wordpress](https://academy.hackthebox.com/storage/modules/77/wordpress.txt)

___
### ENUMERACION DE SUBDOMINIO DNS

También puede haber recursos esenciales alojados en subdominios, como paneles de administración o aplicaciones con funciones adicionales que podrían explotarse. Podemos usar GoBuster para enumerar los subdominios disponibles de un dominio dado usando el indicador dns para especificar el modo DNS. Primero, clonemos el [repositorio](https://github.com/danielmiessler/SecLists) SecLists GitHub, que contiene muchas listas útiles para fuzzing y explotación: 

~~~
$ git clone https://github.com/danielmiessler/SecLists
~~~

~~~
$ sudo apt install seclists -y
~~~

A continuación, agregue un servidor DNS como 1.1.1.1 al archivo `/etc/resolv.conf`. Nos centraremos en el dominio `inlanefreight.com`, el sitio web de una empresa ficticia de transporte y logística. 

~~~
$ gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Domain:     inlanefreight.com
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/SecLists/Discovery/DNS/namelist.txt
===============================================================
2020/12/17 23:08:55 Starting gobuster
===============================================================
Found: blog.inlanefreight.com
Found: customer.inlanefreight.com
Found: my.inlanefreight.com
Found: ns1.inlanefreight.com
Found: ns2.inlanefreight.com
Found: ns3.inlanefreight.com
===============================================================
2020/12/17 23:10:34 Finished
===============================================================

~~~

Este escaneo revela varios subdominios interesantes que podríamos examinar más a fondo. El módulo [Atacar aplicaciones web](https://academy.hackthebox.com/module/details/54) con Ffuf brinda más detalles sobre la enumeración web y la fuzzing. 
___
### TIPS DE ENUMERACION WEB

Veamos algunos consejos de enumeración web adicionales que ayudarán a completar las máquinas en HTB y en el mundo real. 
___
### BANNER GRABBING/ENCABEZADOS DE SERVIDORES WEB

En la última sección, discutimos la captura de banners para propósitos generales. Los headers del servidor web brindan una buena imagen de lo que está alojado en un servidor web. Pueden revelar el framework de aplicación específico en uso, las opciones de autenticación y si al servidor le faltan opciones de seguridad esenciales o si se ha configurado incorrectamente. Podemos usar `cURL` para recuperar la información del encabezado del servidor desde la línea de comandos. `cURL` es otra adición esencial a nuestro kit de herramientas de pentest, y se recomienda familiarizarse con sus muchas opciones. 

~~~
$ curl -IL https://www.inlanefreight.com

HTTP/1.1 200 OK
Date: Fri, 18 Dec 2020 22:24:05 GMT
Server: Apache/2.4.29 (Ubuntu)
Link: <https://www.inlanefreight.com/index.php/wp-json/>; rel="https://api.w.org/"
Link: <https://www.inlanefreight.com/>; rel=shortlink
Content-Type: text/html; charset=UTF-8
~~~

Otra herramienta útil es EyeWitness, que se puede usar para tomar screenshots de las aplicaciones web objetivos, tomar huellas digitales e identificar posibles credenciales predeterminadas. 

___
### WHATWEB

Podemos extraer la versión de los servidores web, los frameworks de soporte y las aplicaciones utilizando la herramienta de línea de comandos `whatweb`. Esta información puede ayudarnos a identificar las tecnologías en uso y comenzar a buscar posibles vulnerabilidades. 

~~~
$ whatweb 10.10.10.121

http://10.10.10.121 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], Email[license@php.net], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.121], Title[PHP 7.4.3 - phpinfo()]
~~~

`Whatweb` es una herramienta útil y contiene muchas funciones para automatizar la enumeración de aplicaciones web en una red. 

~~~
$ whatweb --no-errors 10.10.10.0/24

http://10.10.10.11 [200 OK] Country[RESERVED][ZZ], HTTPServer[nginx/1.14.1], IP[10.10.10.11], PoweredBy[Red,nginx], Title[Test Page for the Nginx HTTP Server on Red Hat Enterprise Linux], nginx[1.14.1]
http://10.10.10.100 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.100], Title[File Sharing Service]
http://10.10.10.121 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], Email[license@php.net], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.121], Title[PHP 7.4.3 - phpinfo()]
http://10.10.10.247 [200 OK] Bootstrap, Country[RESERVED][ZZ], Email[contact@cross-fit.htb], Frame, HTML5, HTTPServer[OpenBSD httpd], IP[10.10.10.247], JQuery[3.3.1], PHP[7.4.12], Script, Title[Fine Wines], X-Powered-By[PHP/7.4.12], X-UA-Compatible[ie=edge]
~~~

___
### CERTIFICADOS

Los certificados SSL/TLS son otra fuente de información potencialmente valiosa si se utiliza HTTPS. Navega en `https://10.10.10.121/` a continuacion revela el detalle de los certificados, incluida la dirección de correo electrónico y el nombre de la empresa. Estos podrían usarse potencialmente para realizar un ataque de phishing si esto está dentro del alcance de una evaluación. 

![Certificado](https://academy.hackthebox.com/storage/modules/77/cert.txt)

___
### ROBOTS.TXT

Es común que los sitios web contengan un archivo `robots.txt`, cuyo propósito es instruir a los rastreadores web de los motores de búsqueda, como Googlebot, a qué recursos se puede o no acceder para la indexación. El archivo `robots.txt` puede proporcionar información valiosa, como la ubicación de archivos privados y páginas de administración. En este caso, vemos que el archivo `robots.txt` contiene dos entradas no permitidas. 

![robot.txt](https://academy.hackthebox.com/storage/modules/77/robots.txt)

Navega en `http://10.10.10.121/private` en un navegador revela una página de inicio de sesión de administrador de HTB. 

![HTB](https://academy.hackthebox.com/storage/modules/77/academy.txt)

___
### CODIGO FUENTE

También vale la pena comprobar el código fuente de cualquier página web con la que nos encontremos. Podemos presionar `[CTRL + U]` para que aparezca la ventana del código fuente en un navegador. Este ejemplo revela un comentario de desarrollador que contiene credenciales para una cuenta de prueba, que podría usarse para iniciar sesión en el sitio web. 

![Codigo-Fuente](https://academy.hackthebox.com/storage/modules/77/academy.txt)
___
### RETO

+ Intente ejecutar algunas de las técnicas de enumeración web que aprendió en esta sección en el servidor y use la información que obtiene para obtener la bandera.

`R:HTB{w3b_3num3r4710n_r3v34l5_53cr375}`

Lo primero que hay que revisar es los directorios que se pueden acceder, como vemos esta `robots.txt` e `index.php`
~~~
┌──(root㉿kali)-[/home/kali]
└─# gobuster dir -u http://138.68.190.93:31070/ -w /usr/share/dirb/wordlists/common.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://138.68.190.93:31070/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/07/20 17:13:42 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 281]
/.htpasswd            (Status: 403) [Size: 281]
/.htaccess            (Status: 403) [Size: 281]
/index.php            (Status: 200) [Size: 990]
/robots.txt           (Status: 200) [Size: 45] 
/server-status        (Status: 403) [Size: 281]
/wordpress            (Status: 301) [Size: 327] [--> http://138.68.190.93:31070/wordpress/]
Progress: 4487 / 4615 (97.23%)                                                   Progress: 4514 / 4615 (97.81%)                                                   Progress: 4537 / 4615 (98.31%)                                                   Progress: 4561 / 4615 (98.83%)                                                   Progress: 4585 / 4615 (99.35%)                                                   Progress: 4611 / 4615 (99.91%)                                                                                                                                              
===============================================================
2022/07/20 17:15:21 Finished
===============================================================
~~~

Vamos a revisar en el navegador que tenemos en index.php, como vemos la pagina principal no muestra nada de utilidad
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/1.png)


Veamos el codigo fuente de la pagina con CTRL+U, de igual forma no muestra nada interesante
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/2.png)


ingresamos ahora a robots.txt y vemos algo de mucha utilidad disallow: /admin-login-page.php 
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/3.png)


ingresamos a esa direccion y como podemos apreciar es la pagina para validar credenciales
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/4.png)


Vemos el codigo fuente y en verde nos resalta una nota que nos indica que usuario y contraseña seria admin:password123
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/5.png)

Colocamos usuario y contraseña para acceder a la informacion
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/6.png)

![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/PB-Web-Enum/7.png)