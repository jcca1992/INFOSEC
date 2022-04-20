## **ERRORES COMUNES**

Mientras realizamos pruebas de penetración o atacamos cajas/laboratorios HTB, podemos cometer muchos errores comunes que obstaculizarán nuestro progreso. En esta sección, discutiremos algunos de estos escollos comunes y cómo superarlos.
___

### *PROBLEMAS CON VPN*

A veces podemos enfrentarnos a problemas relacionados con las conexiones VPN a la red de HTB labs. Primero, debemos asegurarnos de que estamos realmente conectados a la red HTB.

#### *MANTENERSE CONECTADO A VPN*

El método más sencillo para comprobar si nos hemos conectado con éxito a la red VPN es comprobar si tenemos `la Secuencia de inicialización completada` al final de nuestros mensajes de conexión VPN:

~~~
Juceco@htb[/htb]$ sudo openvpn ./htb.ovpn

...SNIP...

Initialization Sequence Completed
~~~

#### *OBTENIENDO DIRECCION VPN*

Otra forma de comprobar si estamos conectados a la red VPN es comprobando nuestra dirección VPN tun0, que podemos encontrar con el siguiente comando:

~~~
Juceco@htb[/htb]$ ip -4 a show tun0

6: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 500
    inet 10.10.10.1/23 scope global tun0
       valid_lft forever preferred_lft forever
~~~

Mientras recuperemos nuestra IP, entonces deberíamos estar conectados a la red VPN.

#### *COMPRBACION DE TABLA DE ENRUTAMIENTO*

Otra forma de verificar la conectividad es usar el comando `sudo netstat -rn` para ver nuestra tabla de enrutamiento:

~~~
Juceco@htb[/htb]$ sudo netstat -rn

[sudo] password for user: 

Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.195.2   0.0.0.0         UG        0 0          0 eth0
10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0
10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0
192.168.1.0   0.0.0.0         255.255.255.0   U         0 0          0 eth0
~~~

#### *HACIENDO PING EN PUERTA DE ENLACE*

Desde aquí, podemos ver que estamos conectados a la red 10.10.14.0/23 en el adaptador tun0 y tenemos acceso a la red 10.129.0.0/16 y podemos hacer ping a la puerta de enlace 10.10.14.1 para confirmar el acceso.

~~~
Juceco@htb[/htb]$ ping -c 4 10.10.14.1
PING 10.10.14.1 (10.10.14.1) 56(84) bytes of data.
64 bytes from 10.10.14.1: icmp_seq=1 ttl=64 time=111 ms
64 bytes from 10.10.14.1: icmp_seq=2 ttl=64 time=111 ms
64 bytes from 10.10.14.1: icmp_seq=3 ttl=64 time=111 ms
64 bytes from 10.10.14.1: icmp_seq=4 ttl=64 time=111 ms

--- 10.10.14.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3012ms
rtt min/avg/max/mdev = 110.574/110.793/111.056/0.174 ms
~~~

Finalmente, podemos atacar un host de destino asignado en la red 10.129.0.0/16 o comenzar la enumeración de hosts en vivo.

#### *TRABAJANDO CON 2 DISPOSITIVOS*

La VPN HTB no se puede conectar a más de un dispositivo simultáneamente. Si estamos conectados en un dispositivo e intentamos conectarnos desde otro dispositivo, el segundo intento de conexión fallará.

Por ejemplo, esto puede suceder cuando nuestra conexión VPN está conectada en nuestro PwnBox, y luego intentamos conectarnos desde nuestro Parrot VM al mismo tiempo. Alternativamente, quizás estemos conectados en nuestra máquina virtual Parrot y luego queramos cambiar a una máquina virtual de Windows para probar algo.

#### *CHEQUEANDO REGION*

Si sentimos un retraso notable en nuestra conexión VPN, como latencia en pings o conexiones ssh, debemos asegurarnos de estar conectados a la región más adecuada. HTB proporciona servidores VPN en todo el mundo, en `Europa`, `EE.UU.`, `Australia` y `Singapur`. Idealmente, ayudaría si intentáramos conectarnos al servidor más cercano a nosotros para obtener la mejor conexión posible.

Para cambiar nuestro servidor VPN, vaya a [HackTheBox](https://app.hackthebox.eu/home), haga clic en el ícono superior derecho que dice `Lab Access` o `Offline`, haga clic en `Labs` y luego haga clic en `OpenVPN`. Una vez que lo hagamos, deberíamos poder elegir la ubicación de nuestro servidor VPN y elegir cualquiera de los servidores dentro de esa región:

![](https://academy.hackthebox.com/storage/modules/77/htb_vpn.jpg)

>Nota: Los usuarios con una suscripción gratuita solo pueden conectarse a 1-3 servidores gratuitos en cada región. Los usuarios con una suscripción VIP pueden conectarse a servidores VIP, que brindan una conexión más rápida con menos tráfico.

#### *VPN TROUBLESHOOTING*

En caso de que enfrentemos problemas técnicos al conectarnos a la VPN, podemos encontrar una guía detallada sobre cómo solucionar problemas de conexiones VPN en esta [página de ayuda de HackTheBox](https://help.hackthebox.eu/troubleshooting/v2-vpn-connection-troubleshooting).
___

### PROBLEMAS CON PROXY BURP SUITE

[Burp Suite](https://portswigger.net/burp/communitydownload) es una herramienta crucial durante las pruebas de penetración de aplicaciones web (así como otros tipos de evaluación). Burp Suite es un proxy de aplicación web y puede causar algunos problemas en nuestros sistemas.

#### *NO DESHABILITAR EL PROXY*

Cuando activamos el proxy Burp en nuestro navegador, Burp comenzará a capturar nuestro tráfico e interceptará nuestras solicitudes. Esto hará que detenga cualquier solicitud que hagamos en el navegador, es decir, visitar una página hasta que vayamos a Burp, examinemos la solicitud y reenviemos la solicitud.

Un error común es olvidarse de apagar el proxy del navegador después de cerrar Burp, por lo que sigue interceptando nuestras solicitudes. Si esto sucede, veremos que nuestro navegador no está cargando ninguna página, por lo que debemos verificar si el proxy del navegador aún está activo. Podemos hacerlo haciendo clic en el ícono del complemento `Foxy Proxy` en Firefox y asegurándonos de que esté configurado en `Turn off`:

![](https://academy.hackthebox.com/storage/modules/30/foxyproxy_options.jpg)

Si no estamos usando un complemento como `Foxy Proxy`, podemos verificar la configuración de conexión del navegador y asegurarnos de que el proxy esté apagado. Una vez que lo hagamos, deberíamos poder continuar navegando sin ningún problema.
___

### CAMBIANDO LLAVE SSH Y CONTRASEÑA

En caso de que comencemos a enfrentar algunos problemas con la conexión a los servidores SSH o la conexión a nuestra máquina desde un servidor remoto, es posible que deseemos renovar o cambiar nuestra clave SSH y contraseña para asegurarnos de que no estén causando ningún problema. Podemos hacer esto con el comando `ssh-keygen`, de la siguiente manera:

~~~
Juceco@htb[/htb]$ ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (/home/parrot/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:

Your identification has been saved in /home/parrot/.ssh/id_rsa
Our public key has been saved in /home/parrot/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:...SNIP... parrot@parrot
The key's randomart image is:
+---[RSA 3072]----+
|            o..  |
|     ...SNIP     |
|     ...SNIP     |
|     ...SNIP     |
|     ...SNIP     |
|     ...SNIP     |
|     ...SNIP     |
|       + +oo+o   |
+----[SHA256]-----+
~~~

De forma predeterminada, las claves SSH se almacenan en la carpeta `.ssh` dentro de nuestra carpeta de inicio (por ejemplo, `/home/htb-student/.ssh`). Si quisiéramos crear una clave ssh en un directorio diferente, podríamos ingresar una ruta absoluta para la clave cuando se nos solicite. Podemos cifrar nuestra clave SSH con una contraseña cuando se nos solicite o dejarla vacía si no queremos usar contraseña.
___

## **OBTENIENDO AYUDA**

Cuando comenzamos a trabajar en maquinas en Hack The Box, es probable que nos quedemos atascados en ciertas áreas y que necesitemos pedirle a otros jugadores algunos consejos y sugerencias para poder avanzar en la maquina. Aquí hay algunas áreas en las que podemos obtener ayuda.

>Nota: Durante cualquiera de sus actividades a través de Hack The Box, siempre debe seguir las Reglas de HTB, que se pueden encontrar en este [enlace](https://www.hackthebox.eu/home/rules).
___
### **FORO**

[El foro de Hack The Box](https://forum.hackthebox.eu/) es un lugar excelente para debatir maquinas y desafíos de Hack The Box en vivo y retirados.

![](https://academy.hackthebox.com/storage/modules/77/htb_forums.jpg)

Cada maquina tiene un hilo de discusión oficial, en el que se mantienen debates en vivo sobre todos los aspectos de la maquina, sin dar spoilers ni indicaciones claras sobre cómo explotar la maquina. Si queremos trabajar en una maquina retirada y no queremos seguir un tutorial paso a paso, podemos leer hilos de maquinas anteriores y buscar sugerencias y consejos. De esta manera, podemos desafiarnos a nosotros mismos en maquinas más antiguas y consultar el hilo de discusión oficial de la maquina en caso de que nos quedemos atascados.
___
### **DISCORD**

Otro gran lugar para discutir las maquinas y los desafíos de Hack The Box, además de varios otros aspectos de Hack The Box, es el servidor oficial HTB Discord. Podemos unirnos al servidor oficial de HTB Discord haciendo clic en este [enlace](https://discord.gg/hRXnCFA). Se nos pedirá que descarguemos la aplicación Discord y creemos nuestra cuenta Discord para acceder al servidor HTB.

El servidor de Discord cubre una amplia gama de temas relacionados con Hack The Box, y también puedes conocer las últimas noticias y torneos. El servidor de Discord también cubre debates sobre anuncios generales, maquinas y desafíos, módulos de la academia, HTB Labs, piratería y Pentesting y soporte general.

![](https://academy.hackthebox.com/storage/modules/77/htb_discord.jpg)

>Asegúrese de leer las Reglas del servidor para evitar cualquier irregularidad.
___
### **HACER PREGUNTAS CON EFICACIA**

Para obtener la mejor orientación sobre nuestro problema, debemos hacer nuestras preguntas de manera efectiva.

Asegúrese de incluir lo siguiente en nuestra pregunta:

+ ¿En qué punto de la maquina o desafío estamos atascados 'ejemplo, usuario o raíz'?
+ ¿Qué pasos hemos dado hasta ahora para llegar al punto en el que nos encontramos?
+ ¿En qué paso estamos fallando y qué hemos hecho para resolver nuestro problema?
+ Siempre trate de ser muy específico sobre la ayuda que necesitamos , en lugar de pedir ayuda general.

Cuando formulamos nuestra pregunta utilizando los puntos anteriores, podemos obtener algunas ideas que se nos hayan pasado por alto, lo que puede ayudarnos a responder la pregunta nosotros mismos. A veces, un comentario de otra persona nos llevará a un "¡ajá!" momento, y podremos responder a su pregunta esencialmente. Otras veces encontraremos a alguien dispuesto a trabajar con nosotros para resolver nuestro problema. Siempre es mejor abordar las cosas desde una perspectiva de aprendizaje y no solo buscar respuestas para obtener puntos o mejorar nuestro posicionamiento en el salón de la fama. `NO` debemos hacer nada de lo siguiente:

+ Regale spoilers o indicaciones claras que puedan solucionar la máquina para otras personas.
+ Ser vagos en nuestra descripción sin proporcionar suficientes detalles de lo que hemos hecho.
___
### **RESPONDER PREGUNTAS CON EFICACIA**

Si hemos completado un cuadro o desafío específico, es posible que queramos ayudar a otros respondiendo a sus preguntas. Involucrarse en la comunidad nos ayuda a construir conexiones y formar nuestro equipo, lo cual es una excelente manera de mejorar nuestro nivel general de pentest y seguridad de la información. También puede ayudarnos a construir nuestro perfil Hack The Box y fortalecer su comprensión de la maquina o el desafío que acaba de completar. Al responder preguntas, asegúrese de hacer lo siguiente:

+ Sea lo más libre de spoilers posible y no entregue instrucciones directas sobre cómo completar el paso actual o la maquina completa
+ Brinde sugerencias o tips menores que puedan conducir a la dirección correcta para completar, y no brinde sugerencias sutanciosas para completar la maquina
+ Compartir recursos que encontramos útiles
+ Comparta consejos sobre puntos en los que nos estábamos atascando
___
### **OBTENER AYUDA TECNICA**

En caso de que nos enfrentemos a algún problema técnico, siempre podemos buscar el problema al que nos enfrentamos en las [preguntas frecuentes oficiales de HTB](https://help.hackthebox.eu/), que contiene muchos artículos muy detallados y útiles. Es muy probable que encontremos una respuesta a nuestra pregunta allí. Si no podemos encontrar una respuesta a nuestra pregunta, siéntase libre de generar un [ticket](https://hackthebox.atlassian.net/servicedesk/customer/portal/1) con HTB Support.