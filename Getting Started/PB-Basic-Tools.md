## **HERRAMIENTAS BASICAS**

Herramientas como SSH, Netcat, Tmux y Vim son esenciales y la mayoría de los profesionales de la seguridad de la información las utilizan a diario. Aunque estas herramientas no pretenden ser herramientas de prueba de penetración, son fundamentales para el proceso de prueba de penetración, por lo que debemos dominarlas. 
___
### USANDO SSH

Secure Shell (SSH) es un protocolo de red que se ejecuta en el puerto 22 de forma predeterminada y brinda a los usuarios, como administradores de sistemas, una forma segura de acceder a una computadora de forma remota. SSH se puede configurar con autenticación de contraseña o sin contraseña usando autenticación de clave pública mediante un par de claves pública/privada de SSH. SSH se puede utilizar para acceder de forma remota a sistemas en la misma red, a través de Internet, facilitar las conexiones a recursos en otras redes mediante el reenvío de puertos/proxy, y cargar/descargar archivos hacia y desde sistemas remotos. 

SSH utiliza un modelo cliente-servidor, conectando un usuario que ejecuta una aplicación de cliente SSH como OpenSSH a un servidor SSH. Mientras atacamos una box o durante una evaluación del mundo real, a menudo obtenemos credenciales de texto claro o una clave privada SSH que se puede aprovechar para conectarnos directamente a un sistema a través de SSH. Una conexión SSH suele ser mucho más estable que una conexión shell inversa y, a menudo, se puede usar como un "jump host" para enumerar y atacar otros hosts en la red, transferir herramientas, configurar la persistencia, etc. Si obtenemos un conjunto de credenciales , podemos usar SSH para iniciar sesión de forma remota en el servidor usando el nombre de usuario en la IP del servidor remoto, de la siguiente manera: 

~~~
$ ssh Bob@10.10.10.10

Bob@remotehost's password: *********

Bob@remotehost#
~~~

También es posible leer claves privadas locales en un sistema comprometido o agregar nuestra clave pública para obtener acceso SSH a un usuario específico, como veremos en una sección posterior. Como podemos ver, SSH es una excelente herramienta para conectarse de forma segura a una máquina remota. También proporciona una forma de asignar puertos locales en la máquina remota a nuestro host local, lo que puede resultar útil en ocasiones. 
___

### USANDO NETCAT

Netcat, ncat o nc es una excelente utilidad de red para interactuar con los puertos TCP/UDP. Se puede utilizar para muchas cosas durante un pentest. Su uso principal es para conectarse a shells, de lo que hablaremos más adelante en este módulo. Además de eso, netcat se puede usar para conectarse a cualquier puerto de escucha e interactuar con el servicio que se ejecuta en ese puerto. Por ejemplo, SSH está programado para manejar conexiones a través del puerto 22 para enviar todos los datos y claves. Podemos conectarnos al puerto TCP 22 con netcat: 

~~~
$ netcat 10.10.10.10 22

SSH-2.0-OpenSSH_8.4p1 Debian-3
~~~

Como podemos ver, el puerto 22 nos envió su banner, indicando que SSH se está ejecutando en él. Esta técnica se llama Banner Grabbing y puede ayudar a identificar qué servicio se está ejecutando en un puerto en particular. Netcat viene preinstalado en la mayoría de las distribuciones de Linux. También podemos descargar una copia para máquinas Windows. Hay otra alternativa de Windows a netcat codificada en PowerShell llamada PowerCat. Netcat también se puede usar para transferir archivos entre máquinas, como veremos más adelante. 

Otra utilidad de red similar es socat, que tiene algunas funciones que netcat no admite, como el reenvío de puertos y la conexión a dispositivos serie. Socat también se puede usar para actualizar un shell a un TTY completamente interactivo. Veremos algunos ejemplos de esto en una sección posterior. Socat es una utilidad muy útil que debería ser parte del conjunto de herramientas de cada probador de penetración. Un binario independiente de Socat se puede transferir a un sistema después de obtener la ejecución remota del código para obtener una conexión de shell inversa más estable. 

socat es como netcat con esteroides y es una navaja suiza muy potente para redes. Socat se puede usar para pasar TTY completo a través de conexiones TCP. 

Si socat está instalado en el servidor de la víctima, puede iniciar una reverse shell con él. También debe captar la conexión con socat para obtener todas las funciones. 
___
### USANDO TMUX

Los multiplexores de terminales, como tmux o Screen, son excelentes utilidades para expandir las funciones de un terminal Linux estándar, como tener múltiples ventanas dentro de un terminal y saltar entre ellas. Veamos algunos ejemplos del uso de tmux, que es el más común de los dos. Si tmux no está presente en nuestro sistema Linux, podemos instalarlo con el siguiente comando: 

~~~
$ sudo apt install tmux -y
~~~

Una vez que tenemos tmux, podemos iniciarlo ingresando el comando tmux: 
![tmux](https://academy.hackthebox.com/storage/modules/77/getting_started_tmux_1.jpg)

La tecla predeterminada para ingresar el prefijo de comandos tmux es [CTRL + B]. Para abrir una nueva ventana en tmux, podemos presionar el prefijo 'i.e. [CTRL + B]' y luego presiona C: 

![CTRL+B](https://academy.hackthebox.com/storage/modules/77/getting_started_tmux_2.jpg)

Vemos las ventanas numeradas en la parte inferior. Podemos cambiar a cada ventana presionando el prefijo y luego ingresando el número de la ventana, como 0 o 1. También podemos dividir una ventana verticalmente en paneles presionando el prefijo y luego [MAYÚS + %]: 

![MAYUS+%](https://academy.hackthebox.com/storage/modules/77/getting_started_tmux_3.jpg)

También podemos dividirnos en paneles horizontales presionando el prefijo y luego [MAYÚS + "]: 

![MAYUS+"](https://academy.hackthebox.com/storage/modules/77/getting_started_tmux_4.jpg)

Podemos cambiar entre paneles presionando el prefijo y luego las flechas izquierda o derecha para el cambio horizontal o las flechas hacia arriba o hacia abajo para el cambio vertical. Los comandos anteriores cubren algunos usos básicos de tmux. Es una herramienta poderosa y se puede usar para muchas cosas, incluido el registro, que es muy importante durante cualquier compromiso técnico. [Esta hoja de trucos](https://tmuxcheatsheet.com/) es una referencia muy útil. Además, este video [Introducción a tmux](https://www.youtube.com/watch?v=Lqehvpe_djs) de ipsec vale la pena.
___

### USANDO VIM

Vim es un excelente editor de texto que se puede usar para escribir código o editar archivos de texto en sistemas Linux. Uno de los grandes beneficios de usar Vim es que se basa completamente en el teclado, por lo que no tiene que usar el mouse, lo que (una vez que lo tengamos) aumentará significativamente su productividad y eficiencia al escribir/editar código. Por lo general, encontramos Vim o Vi instalado en sistemas Linux, por lo que aprender a usarlo nos permite editar archivos incluso en sistemas remotos. Vim también tiene muchas otras características, como extensiones y complementos, que pueden extender significativamente su uso y convertirse en un excelente editor de código. Veamos algunos de los conceptos básicos de Vim. Para abrir un archivo con Vim, podemos agregar el nombre del archivo después: 

~~~
$vim /etc/hosts
~~~

![vim](https://academy.hackthebox.com/storage/modules/77/getting_started_vim_1.jpg)

Si queremos crear un archivo nuevo, ingrese el nombre del archivo nuevo y Vim abrirá una ventana nueva con ese archivo. Una vez que abrimos un archivo, estamos en modo normal de solo lectura, lo que nos permite navegar y leer el archivo. Para editar el archivo, presionamos i para ingresar al modo de inserción, que se muestra con "-- INSERT --" en la parte inferior de Vim. Posteriormente, podemos mover el cursor de texto y editar el archivo: 

![INSERT](https://academy.hackthebox.com/storage/modules/77/getting_started_vim_2.jpg)

Una vez que hayamos terminado de editar un archivo, podemos presionar la tecla de escape esc para salir del modo de inserción y volver al modo normal. Cuando estamos en modo normal, podemos usar las siguientes teclas para realizar algunos atajos útiles: 

| Command |	Description |
|--|--|
| x | Cut character |
| dw | Cut word |
| dd | Cut full line |
| yw | Copy word |
| yy | Copy full line |
| p | Paste |

~~~
Tips: podemos multiplicar cualquier comando para que se ejecute varias veces agregando un número antes. Por ejemplo, '4yw' copiaría 4 palabras en lugar de una, y así sucesivamente.
~~~
Si queremos guardar un archivo o salir de Vim, tenemos que pulsar : para entrar en modo comando. Una vez que lo hagamos, veremos cualquier comando que escribamos en la parte inferior de la ventana de vim:

![:](https://academy.hackthebox.com/storage/modules/77/getting_started_vim_3.jpg)

Hay muchos comandos disponibles para nosotros. Los siguientes son algunos de ellos: 

| Command | Description |
|--|--|
| :1 |	Go to line number 1. |
| :w |	Write the file, save |
| :q |	Quit |
| :q! |	Quit without saving |
| :wq |	Write and quit |

Vim es una herramienta muy poderosa y tiene muchos otros comandos y funciones. Esta [hoja de trucos](https://vimsheet.com/) es un excelente recurso para desbloquear aún más el poder de Vim.
___

### RETO

+ Aplique lo que aprendió en esta sección para tomar el banner del servidor y enviarlo como respuesta.

`R: SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.1`

~~~
┌──(root㉿kali)-[/home/kali]
└─# netcat 178.62.104.23 31245
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.1
~~~