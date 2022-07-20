## NIBBLES-ENUMERACION

Hay 201 maquinas independientes de varios sistemas operativos y niveles de dificultad disponibles en la plataforma HTB con membresía VIP al momento de escribir esto. Esta membresía incluye un recorrido oficial creado por HTB para cada máquina retirada. También podemos encontrar tutoriales en blogs y videos para la mayoría de las cajas con una búsqueda rápida en Google.

Para nuestros propósitos, repasemos la maquina `Nibbles`, un cuadro de Linux fácil de calificar que muestra tácticas de enumeración comunes, explotación básica de aplicaciones web y una configuración incorrecta relacionada con archivos para escalar privilegios.

![Nibbles](https://academy.hackthebox.com/storage/modules/77/nibbles_card.png)

Veamos primero algunas estadísticas de la máquina:

|Nombre de maquina|Nibbles|
|--|--|
|Creador| mrb3n|
| sistema operativo | Linux |
| Dificultad | Facil |
| User Path | Web |
| Escalada de privilegios | archivos escribibles por todos / Configuración incorrecta de Sudoers |
| Video Ippsec | [https://www.youtube.com/watch?v=s_0GcRGv6Ds](https://www.youtube.com/watch?v=s_0GcRGv6Ds)|
| Tutorial | [https://0xdf.gitlab.io/2018/06/30/htb-nibbles.html](https://0xdf.gitlab.io/2018/06/30/htb-nibbles.html) |

Nuestro primer paso al acercarnos a cualquier máquina es realizar una enumeración básica. Primero, comencemos con lo que sabemos sobre el objetivo. Ya sabemos la dirección IP del objetivo, que es Linux y tiene un vector de ataque relacionado con la web. A esto lo llamamos un enfoque `Gray-Box` porque tenemos cierta información sobre el objetivo. En la plataforma HTB, las 20 máquinas de lanzamiento semanal "activas" se abordan desde una perspectiva de `Black-Box`. Los usuarios reciben la dirección IP y el tipo de sistema operativo de antemano, pero no información adicional sobre el objetivo para formular sus ataques. Esta es la razón por la que la enumeración exhaustiva es fundamental y, a menudo, es un proceso iterativo.

Antes de continuar, demos un paso atrás y veamos los diversos enfoques para las acciones de prueba de penetración. Hay tres tipos principales, `Black-Box`, `Gray_Box` y `White-Box`, y cada uno difiere en el objetivo y el enfoque.

| Enfoque | Descripcion |
| -- | -- |
| `Black-Box` | Nivel bajo o sin conocimiento de un objetivo. El analista debe realizar un reconocimiento en profundidad para conocer el objetivo. Esta puede ser un pentest externo en el que al evaluador solo se le da el nombre de la empresa y no se proporciona más información, como las direcciones IP de destino, o un pentest interno en el que el evaluador tiene que pasar por alto los controles para obtener acceso inicial a la red o puede conectarse a la red interna pero no tiene información sobre las redes o hosts internos. Este tipo de pentest simula un ataque real, pero no es tan completo como otros tipos de evaluación y podría dejar configuraciones erróneas o vulnerabilidades sin descubrir. |
| `Gray-Box` | En una prueba de `Gray-Box`, el probador recibe una cierta cantidad de información por adelantado. Puede ser una lista de rangos o direcciones IP dentro del alcance, credenciales de bajo nivel para una aplicación web o Active Directory, o algunos diagramas de aplicación o red. Este tipo de pentest puede simular una información interna maliciosa o ver lo que un atacante puede hacer con un bajo nivel de acceso. En este escenario, el probador generalmente dedicará menos tiempo al reconocimiento y más tiempo a buscar configuraciones incorrectas e intentar explotar. |
| `White-Box` | En este tipo de prueba, el analista tiene acceso completo. En una prueba de aplicación web, se les pueden proporcionar credenciales de nivel de administrador, acceso al código fuente, diagramas de compilación, etc., para buscar vulnerabilidades lógicas y otras fallas difíciles de descubrir. En una prueba de red, es posible que se les proporcionen credenciales de nivel de administrador para investigar en Active Directory u otros sistemas en busca de errores de configuración que, de otro modo, se podrían pasar por alto. Este tipo de evaluación es muy completo ya que el probador tendrá acceso a ambos lados de un objetivo y realizará un análisis completo.|

___

### NMAP

Comencemos con un escaneo rápido de `nmap` para buscar puertos abiertos usando el comando `nmap -sV --open -oA nibbles_initial_scan <dirección IP>`. Esto ejecutará una enumeración de servicios (`-sV`) contra los 1000 puertos principales predeterminados y solo devolverá puertos abiertos (`--open`). Podemos verificar qué puertos escanea `nmap` para un tipo de escaneo dado ejecutando un escaneo sin un objetivo especificado, usando el comando `nmap -v -oG -`. Aquí enviaremos el formato greppable a stdout con `-oG -` y `-v` para una salida detallada. Dado que no se especifica ningún objetivo, la exploración fallará pero mostrará los puertos explorados.

~~~
Juceco@htb[/htb]$ nmap -v -oG -

# Nmap 7.80 scan initiated Wed Dec 16 23:22:26 2020 as: nmap -v -oG -

# Ports scanned: TCP(1000;1,3-4,6-7,9,13,17,19-26,30,32-33,37,42-43,49,53,70,79-85,88-90,99-100,106,109-111,113,119,125,135,139,143-144,146,161,163,179,199,211-212,222,254-256,259,264,...SNIP...992-993,995,999-1002,1007,1009-1011,56737-56738,57294,57797,58080,60020,60443,61532,61900,62078,63331,64623,64680,65000,65129,65389) UDP(0;) SCTP(0;) PROTOCOLS(0;)

WARNING: No targets were specified, so 0 hosts scanned.

# Nmap done at Wed Dec 16 23:22:26 2020 -- 0 IP addresses (0 hosts up) scanned in 0.04 seconds
~~~

Finalmente, generaremos todos los formatos de escaneo usando `-oA`. Esto incluye salida XML, salida greppable y salida de texto que pueden sernos útiles más adelante. Es esencial adquirir el hábito de tomar notas extensas y guardar toda la salida de la consola desde el principio. Cuanto mejor seamos en esto mientras practicamos, más natural se volverá en los compromisos del mundo real. La toma de notas adecuada es fundamental para nosotros como Pentesters y acelerará significativamente el proceso de informes y garantizará que no se pierda evidencia. También es fundamental mantener registros detallados con sello de tiempo de los intentos de escaneo y explotación en una interrupción o incidente en el que el cliente necesita información sobre nuestras actividades.

~~~
Juceco@htb[/htb]$ nmap -sV --open -oA nibbles_initial_scan 10.129.42.190

Starting Nmap 7.80 ( https://nmap.org ) at 2020-12-16 23:18 EST

Nmap scan report for 10.129.42.190
Host is up (0.11s latency).
Not shown: 991 closed ports, 7 filtered ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd <REDACTED> ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.82 seconds
~~~

A partir del resultado del escaneo inicial, podemos ver que el host es probablemente Ubuntu Linux y expone un servidor web Apache en el puerto 80 y un servidor OpenSSH en el puerto 22. SSH, o Secure Shell, es un protocolo que generalmente se usa para el acceso remoto a Linux o anfitriones Unix. SSH también se puede usar para acceder al host de Windows y ahora es nativo de Windows 10 desde la versión 1809. También podemos ver que los tres tipos de resultados de escaneo se crearon en nuestro directorio de trabajo.

~~~
Juceco@htb[/htb]$ ls

nibbles_initial_scan.gnmap  nibbles_initial_scan.nmap  nibbles_initial_scan.xml

~~~

Antes de comenzar a hurgar en los puertos abiertos, podemos ejecutar un escaneo de puerto TCP completo usando el comando `nmap -p- --open -oA nibbles_full_tcp_scan 10.129.42.190`. Esto verificará si hay servicios que se ejecutan en puertos no estándar que nuestro escaneo inicial puede haber pasado por alto. Dado que esto escanea todos los 65 535 puertos TCP, puede llevar mucho tiempo terminar dependiendo de la red. Podemos dejar esto ejecutándose en segundo plano y continuar con nuestra enumeración. El uso de `nc` para capturar banners confirma lo que nos dijo nmap; el objetivo ejecuta un servidor web Apache y un servidor OpenSSH.

~~~
Juceco@htb[/htb]$ nc -nv 10.129.42.190 22

(UNKNOWN) [10.129.42.190] 22 (ssh) open
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.8
~~~

`nc` nos dice que el puerto 80 ejecuta un servidor HTTP (web) pero no muestra el banner.

~~~
Juceco@htb[/htb]$ nc -nv 10.129.42.190 80

(UNKNOWN) [10.129.42.190] 80 (http) open
~~~

Al revisar nuestra otra ventana de terminal, podemos ver que el escaneo completo de puertos (`-p-`) ha finalizado y no ha encontrado ningún puerto adicional. Realicemos un escaneo de [script](https://nmap.org/book/man-nse.html) `nmap` usando el indicador `-sC`. Esta flag utiliza los scripts predeterminados, que se enumeran [aquí](https://nmap.org/nsedoc/categories/default.html). Estos scripts pueden ser intrusivos, por lo que siempre es importante comprender exactamente cómo funcionan nuestras herramientas. Ejecutamos el comando `nmap -sC -p 22,80 -oA nibbles_script_scan 10.129.42.190`. Dado que ya sabemos qué puertos están abiertos, podemos ahorrar tiempo y limitar el tráfico de escáner innecesario especificando los puertos de destino con `-p`.

~~~
Juceco@htb[/htb]$ nmap -sC -p 22,80 -oA nibbles_script_scan 10.129.42.190

Starting Nmap 7.80 ( https://nmap.org ) at 2020-12-16 23:39 EST
Nmap scan report for 10.129.42.190
Host is up (0.11s latency).

PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
80/tcp open  http
|_http-title: Site doesn't have a title (text/html).

Nmap done: 1 IP address (1 host up) scanned in 4.42 seconds
~~~

El escaneo del script no nos dio nada útil. Completemos nuestra enumeración `nmap` usando el [script http-enum](https://nmap.org/nsedoc/scripts/http-enum.html), que se puede usar para enumerar directorios de aplicaciones web comunes. Este escaneo tampoco descubrió nada útil.

~~~
Juceco@htb[/htb]$ nmap -sV --script=http-enum -oA nibbles_nmap_http_enum 10.129.42.190 

Starting Nmap 7.80 ( https://nmap.org ) at 2020-12-16 23:41 EST
Nmap scan report for 10.129.42.190
Host is up (0.11s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd <REDACTED> ((Ubuntu))
|_http-server-header: Apache/<REDACTED> (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.23 seconds
~~~
### RETO

+ Ejecute un escaneo de script nmap en el objetivo. ¿Cuál es la versión de Apache que se ejecuta en el servidor? (formato de respuesta: X.X.XX)

`R: 2.4.18`

~~~
# nmap -sV 10.129.200.170      
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-07 23:15 UTC
Nmap scan report for 10.129.200.170
Host is up (0.15s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.66 seconds
~~~