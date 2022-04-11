Podemos usar whatweb para intentar identificar la aplicación web en uso.

~~~
Juceco@htb[/htb]$ whatweb 10.129.42.190

http://10.129.42.190 [200 OK] Apache[2.4.18], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)], IP[10.129.42.190]
~~~

Esta herramienta no identifica ninguna tecnología web estándar en uso. Navegar hasta el objetivo en `Firefox` nos muestra un simple "¡Hola mundo!".

![](https://academy.hackthebox.com/storage/modules/77/nibbles_hello2.png)

Verificar la fuente de la página revela un comentario interesante. `CTRL+U`

![](https://academy.hackthebox.com/storage/modules/77/nibbles_comment1.png)

También podemos verificar esto con cURL.

~~~
Juceco@htb[/htb]$ curl http://10.129.42.190

<b>Hello world!</b>

<!-- /nibbleblog/ directory. Nothing interesting here! -->
~~~

El comentario HTML menciona un directorio llamado `nibbleblog`. Comprobemos esto con `whatweb`.

~~~
Juceco@htb[/htb]$ whatweb http://10.129.42.190/nibbleblog

http://10.129.42.190/nibbleblog [301 Moved Permanently] Apache[2.4.18], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)], IP[10.129.42.190], RedirectLocation[http://10.129.42.190/nibbleblog/], Title[301 Moved Permanently]
http://10.129.42.190/nibbleblog/ [200 OK] Apache[2.4.18], Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)], IP[10.129.42.190], JQuery, MetaGenerator[Nibbleblog], PoweredBy[Nibbleblog], Script, Title[Nibbles - Yum yum]
~~~

Ahora estamos empezando a tener una mejor imagen de las cosas. Podemos ver algunas de las tecnologías en uso, como [HTML5](https://en.wikipedia.org/wiki/HTML5), [jQuery](https://en.wikipedia.org/wiki/JQuery) y [PHP](https://en.wikipedia.org/wiki/PHP). También podemos ver que el sitio ejecuta [Nibbleblog](https://www.nibbleblog.com/), que es un motor de blogs gratuito creado con PHP.

___

### ENUMERACION DE DIRECTORIO

Navegando hasta el directorio `/nibbleblog` en Firefox, no vemos nada interesante en la página principal.

![](https://academy.hackthebox.com/storage/modules/77/yumyum_.png)

Una búsqueda rápida en Google de "nibbleblog exploit" produce esta [vulnerabilidad de carga de archivos de Nibblblog](https://www.rapid7.com/db/modules/exploit/multi/http/nibbleblog_file_upload/). La falla permite que un atacante autenticado cargue y ejecute código PHP arbitrario en el servidor web subyacente. El módulo `Metasploit` en cuestión funciona para la versión `4.0.3.` Todavía no sabemos la versión exacta de `Nibbleblog` en uso, pero es una buena apuesta que sea vulnerable a esto. Si observamos el código fuente del módulo `Metasploit`, podemos ver que el exploit utiliza credenciales proporcionadas por el usuario para autenticar el portal de administración en `/admin.php`.

Usemos [Gobuster](https://github.com/OJ/gobuster) para ser minuciosos y verificar si hay otras páginas/directorios accesibles.

~~~
Juceco@htb[/htb]$ gobuster dir -u http://10.129.42.190/nibbleblog/ --wordlist /usr/share/dirb/wordlists/common.txt

===============================================================

Gobuster v3.0.1

by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================

[+] Url:            http://10.129.42.190/nibbleblog/
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/12/17 00:10:47 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 301)
/admin.php (Status: 200)
/content (Status: 301)
/index.php (Status: 200)
/languages (Status: 301)
/plugins (Status: 301)
/README (Status: 200)
/themes (Status: 301)
===============================================================
2020/12/17 00:11:38 Finished
===============================================================
~~~

`Gobuster` termina muy rápido y confirma la presencia de la página `admin.php`. Podemos consultar la página `README` para obtener información interesante, como el número de versión.

~~~
Juceco@htb[/htb]$ curl http://10.129.42.190/nibbleblog/README

====== Nibbleblog ======
Version: v4.0.3
Codename: Coffee
Release date: 2014-04-01

Site: http://www.nibbleblog.com
Blog: http://blog.nibbleblog.com
Help & Support: http://forum.nibbleblog.com
Documentation: http://docs.nibbleblog.com

===== Social =====

* Twitter: http://twitter.com/nibbleblog
* Facebook: http://www.facebook.com/nibbleblog
* Google+: http://google.com/+nibbleblog

===== System Requirements =====

* PHP v5.2 or higher
* PHP module - DOM
* PHP module - SimpleXML
* PHP module - GD
* Directory “content” writable by Apache/PHP

<SNIP>
~~~

Entonces validamos que la versión 4.0.3 está en uso, confirmando que esta versión probablemente sea vulnerable al módulo `Metasploit` (aunque podría ser una página `README` antigua). No se nos ocurre nada más interesante. Echemos un vistazo a la página de inicio de sesión del portal de administración.

![](https://academy.hackthebox.com/storage/modules/77/nibble_admin.png)

Ahora, para usar el exploit mencionado anteriormente, necesitaremos credenciales de administrador válidas. Podemos probar algunas técnicas de omisión de autorización y pares de credenciales comunes manualmente, como `admin:admin` y `admin:password`, sin éxito. Hay una función de restablecimiento de contraseña, pero recibimos un correo electrónico de error. Además, demasiados intentos de inicio de sesión activan demasiado rápido un bloqueo con el mensaje `Nibbleblog security error - Blacklist protection`.

Volvamos a nuestro directorio de resultados de fuerza bruta. el codigo de estado `200` muestran páginas y directorios a los que se puede acceder directamente. Los códigos de estado `403` en la salida indican que el acceso a estos recursos está prohibido. Finalmente, el `301` es una redirección permanente. Exploremos cada uno de estos. Navegando en `nibbleblog/themes/`. Podemos ver que la lista de directorios está habilitada en la aplicación web. ¿Quizás podamos encontrar algo interesante mientras husmeamos?

![](https://academy.hackthebox.com/storage/modules/77/nibbles_dir_listing.png)

Navegar `nibbleblog/content` muestra algunos subdirectorios `públicos`, `privados` y `tmp interesantes`. Buscando por un tiempo, encontramos un archivo `users.xml` que al menos parece confirmar que el nombre de usuario es realmente admin. También muestra las direcciones IP en la lista negra. Podemos solicitar este archivo con `cURL` y prettify la salida `XML` usando [xmllint](https://linux.die.net/man/1/xmllint).

~~~
Juceco@htb[/htb]$ curl -s http://10.129.42.190/nibbleblog/content/private/users.xml | xmllint  --format -

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<users>
  <user username="admin">
    <id type="integer">0</id>
    <session_fail_count type="integer">2</session_fail_count>
    <session_date type="integer">1608182184</session_date>
  </user>
  <blacklist type="string" ip="10.10.10.1">
    <date type="integer">1512964659</date>
    <fail_count type="integer">1</fail_count>
  </blacklist>
  <blacklist type="string" ip="10.10.14.2">
    <date type="integer">1608182171</date>
    <fail_count type="integer">5</fail_count>
  </blacklist>
</users>
~~~

En este punto, tenemos un nombre de usuario válido pero no una contraseña. Las búsquedas en la documentación relacionada con Nibbleblog muestran que la contraseña se establece durante la instalación y que no existe una contraseña predeterminada conocida. Hasta este punto, tenemos las siguientes piezas del rompecabezas:

+ Una instalación de Nibbleblog potencialmente vulnerable a una vulnerabilidad de carga de archivos autenticados

+ Un portal de administración en `nibbleblog/admin.php`

+ Listado de directorio que confirmó que `admin` es un nombre de usuario válido

+ La protección de fuerza bruta de inicio de sesión incluye nuestra dirección IP en la lista negra después de demasiados intentos de inicio de sesión no válidos. Esto deja la fuerza bruta de inicio de sesión con una herramienta como [Hydra](https://github.com/vanhauser-thc/thc-hydra) fuera de la mesa

No hay otros puertos abiertos y no encontramos ningún otro directorio. Lo cual podemos confirmar realizando fuerza bruta de directorio adicional contra el root de la aplicación web

~~~
Juceco@htb[/htb]$ gobuster dir -u http://10.129.42.190/ --wordlist /usr/share/dirb/wordlists/common.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.129.42.190/
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/12/17 00:36:55 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/server-status (Status: 403)
===============================================================
2020/12/17 00:37:46 Finished
===============================================================
~~~

Echando otro vistazo a todos los directorios expuestos, encontramos un archivo `config.xml`.

~~~
Juceco@htb[/htb]$ curl -s http://10.129.42.190/nibbleblog/content/private/config.xml | xmllint --format -

<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<config>
  <name type="string">Nibbles</name>
  <slogan type="string">Yum yum</slogan>
  <footer type="string">Powered by Nibbleblog</footer>
  <advanced_post_options type="integer">0</advanced_post_options>
  <url type="string">http://10.129.42.190/nibbleblog/</url>
  <path type="string">/nibbleblog/</path>
  <items_rss type="integer">4</items_rss>
  <items_page type="integer">6</items_page>
  <language type="string">en_US</language>
  <timezone type="string">UTC</timezone>
  <timestamp_format type="string">%d %B, %Y</timestamp_format>
  <locale type="string">en_US</locale>
  <img_resize type="integer">1</img_resize>
  <img_resize_width type="integer">1000</img_resize_width>
  <img_resize_height type="integer">600</img_resize_height>
  <img_resize_quality type="integer">100</img_resize_quality>
  <img_resize_option type="string">auto</img_resize_option>
  <img_thumbnail type="integer">1</img_thumbnail>
  <img_thumbnail_width type="integer">190</img_thumbnail_width>
  <img_thumbnail_height type="integer">190</img_thumbnail_height>
  <img_thumbnail_quality type="integer">100</img_thumbnail_quality>
  <img_thumbnail_option type="string">landscape</img_thumbnail_option>
  <theme type="string">simpler</theme>
  <notification_comments type="integer">1</notification_comments>
  <notification_session_fail type="integer">0</notification_session_fail>
  <notification_session_start type="integer">0</notification_session_start>
  <notification_email_to type="string">admin@nibbles.com</notification_email_to>
  <notification_email_from type="string">noreply@10.10.10.134</notification_email_from>
  <seo_site_title type="string">Nibbles - Yum yum</seo_site_title>
  <seo_site_description type="string"/>
  <seo_keywords type="string"/>
  <seo_robots type="string"/>
  <seo_google_code type="string"/>
  <seo_bing_code type="string"/>
  <seo_author type="string"/>
  <friendly_urls type="integer">0</friendly_urls>
  <default_homepage type="integer">0</default_homepage>
</config>

~~~

Verificándolo, con la esperanza de que las pruebas de contraseñas sean infructuosas, pero vemos dos menciones de `nibbles` en el título del sitio, así como en la dirección de correo electrónico de notificación. Este es también el nombre de la box. ¿Podría ser esta la contraseña de administrador?

Al realizar el descifrado de contraseñas sin conexión con una herramienta como `Hashcat` o intentar adivinar una contraseña, es importante tener en cuenta toda la información que tenemos delante. No es raro descifrar con éxito un hash de contraseña (como la frase de contraseña de la red inalámbrica de una empresa) usando una lista de palabras generada al rastrear su sitio web usando una herramienta como [CeWL](https://github.com/digininja/CeWL).

![](https://academy.hackthebox.com/storage/modules/77/nibbles_loggedin.png)

Esto nos muestra cuán crucial es la enumeración minuciosa. Recapitulemos lo que hemos encontrado hasta ahora:

+ Comenzamos con un escaneo `nmap` simple que muestra dos puertos abiertos

+ Descubrimos una instancia de `Nibbleblog`.

+ Se analizó las tecnologías en uso usando `whatweb`

+ Encontramos la página del portal de inicio de sesión del administrador en `admin.php`

+ Descubrímos que la lista de directorios está habilitada y navegué por varios directorios

+ Confirmado que `admin` era el nombre de usuario válido

+ Descubrimos de la manera difícil que la lista negra de IP está habilitada para evitar intentos de inicio de sesión de fuerza bruta

+ Pistas descubiertas que nos llevaron a una contraseña de administrador válida de nibbles

Esto demuestra que necesitamos un proceso claro y repetible que usaremos una y otra vez, sin importar si estamos atacando una sola maquina en HTB, realizando una prueba de penetración de aplicaciones web para un cliente o atacando un gran entorno de Active Directory. Tenga en cuenta que la enumeración iterativa, junto con la toma de notas detallada, es una de las claves del éxito en este campo. A medida que avanza en su carrera, a menudo se maravillará de cómo el alcance inicial de una prueba de penetración parecía extremadamente pequeño y "aburrido", pero una vez que profundice y realice rondas y rondas de enumeración y retire las capas, puede encontrar un servicio expuesto en un high port o alguna página o directorio olvidado que puede conducir a la exposición de datos confidenciales o incluso a un punto de apoyo.