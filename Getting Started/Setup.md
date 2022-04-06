## **PRIMEROS PASOS CON DISTRIBUCION PENTES**

Cualquiera que busque iniciar un camino técnico en seguridad de la información debe sentirse cómodo con una amplia gama de tecnologías y sistemas operativos. Como pentesters, debemos comprender cómo configurar, mantener y proteger las máquinas de ataque Linux y Windows. Según el entorno del cliente o el alcance de la evaluación, es posible que utilicemos una máquina virtual Linux o Windows en nuestra máquina, nuestro sistema operativo base, una caja de Linux en la nube, una máquina virtual instalada dentro del entorno del cliente o incluso realizar pruebas directamente desde una estación de trabajo propia del cliente para simular una amenaza interna (suponiendo un escenario de ruptura). 
___

### ESCOGIENDO DISTRIBUCION

Hay muchas distribuciones de Linux (distros) para pruebas de penetración. Hay bastantes distribuciones preexistentes basadas en Debian precargadas con muchas herramientas que necesitamos para realizar nuestras evaluaciones. Muchas de estas herramientas rara vez se requieren y ninguna distribución contiene todas las herramientas que necesitamos para realizar nuestras evaluaciones. A medida que aprendemos y progresamos en nuestras carreras, gravitaremos hacia herramientas específicas y tendremos una lista de "imprescindibles" para agregar a una nueva distribución. A medida que avanzamos, es posible que incluso prefiramos personalizar completamente nuestra propia VM de pentesting a partir de una imagen base de Debian o Ubuntu, pero la creación de una VM totalmente personalizada está fuera del alcance de este módulo.

La elección de una distro es individual, y como hemos dicho, incluso podemos optar por crear y mantener la nuestra propia desde cero. Existen innumerables distribuciones de Linux que sirven para varios propósitos, algunas personalizadas explícitamente para pruebas de penetración, otras orientadas a pruebas de penetración de aplicaciones web, análisis forense, etc.

Esta sección cubrirá la configuración y el trabajo con [Parrot OS](https://parrotlinux.org/). Esta distro se utiliza para el Pwnbox que veremos a lo largo de Academy, personalizado para practicar y resolver ejercicios a lo largo de los distintos módulos que encontraremos. 

Es importante tener en cuenta que cada prueba de penetración o evaluación de seguridad se debe realizar desde una máquina virtual recién instalada para evitar incluir detalles relevantes para la seguridad de otro entorno de cliente en nuestros informes por accidente o retener datos confidenciales del cliente durante períodos de tiempo significativos. Por esta razón, debemos tener la capacidad de instalar rápidamente una nueva máquina de pentest y tener procesos implementados (automatización, scripts, procedimientos detallados, etc.) para configurar rápidamente nuestra(s) distribución(es) de elección para cada evaluación que realizamos. 
___
### CONFIGURANDO DISTRO DE PENTEST

Hay muchas maneras de configurar nuestra distribución de pentest local. Podemos instalarlo como nuestro sistema operativo base (aunque no se recomienda), configurar nuestra estación de trabajo para un arranque dual (que requiere mucho tiempo para alternar entre sistemas operativos) o instalar mediante virtualización.

Tenemos bastantes opciones disponibles: Hyper-V en Windows, como máquinas virtuales en hipervisores bare metal como Proxmox o VMware ESXi o usando hipervisores gratuitos como VirtualBox o VMware Workstation Player, que se pueden instalar y usar como hipervisores. en los sistemas operativos Windows y Linux.
Otra opción es VMware Workstation, que requiere una licencia paga pero ofrece muchas más funciones que las opciones gratuitas. 

Un hipervisor es un software que nos permite crear y ejecutar máquinas virtuales (VM). Nos permitirá usar nuestra computadora host (de escritorio o portátil) para ejecutar varias máquinas virtuales compartiendo virtualmente la memoria y los recursos de procesamiento.
Las máquinas virtuales en un hipervisor se ejecutan aisladas del sistema operativo principal, lo que ofrece una capa de aislamiento y protección entre nuestra red de producción y las redes vulnerables, como Hack The Box, o cuando se conecta a entornos cliente desde una máquina virtual (aunque surgen vulnerabilidades de ruptura de máquinas virtuales). de vez en cuando). 

Dependiendo de la cantidad de recursos que tenga nuestro sistema host (es decir, RAM), generalmente podemos ejecutar algunas máquinas virtuales a la vez. A menudo, es útil poner en marcha una VM durante una evaluación para probar un exploit o intentar recrear una aplicación como objetivo y máquinas levantadas en un entorno de laboratorio para probar las últimas herramientas, exploits y técnicas. Todos los que trabajan en una función técnica de seguridad de la información deben sentirse cómodos trabajando con uno o más hipervisores y construyendo máquinas virtuales de manera competente tanto para el trabajo como para la práctica. 

Para tener éxito, debemos trabajar continuamente para perfeccionar nuestro oficio. Una excelente manera es configurar un laboratorio en el hogar para intentar reproducir vulnerabilidades, configurar aplicaciones y servicios vulnerables, ver los efectos de las recomendaciones de remediación y tener un lugar seguro para practicar nuevas técnicas de ataque/exploits. Podemos construir nuestro laboratorio en una computadora portátil o de escritorio vieja, pero preferiblemente usando un servidor para instalar un hipervisor completo. 

Para nuestros propósitos, usaremos una versión modificada de Parrot Security (Pwnbox), disponible aquí para construir una máquina virtual local. Podemos elegir dos formatos: 

+ Optical disc image (ISO)
+ Open Virtual Appliance (OVA)




### ISO

El archivo ISO es esencialmente solo un CD-ROM que se puede montar dentro de nuestro hipervisor de elección para construir la máquina virtual instalando el sistema operativo nosotros mismos. Una ISO nos brinda más espacio para la personalización, por ejemplo, distribución del teclado, configuración regional, cambio de entorno de escritorio, particiones personalizadas, etc. y, por lo tanto, un enfoque más granular al configurar nuestra máquina virtual de ataque. 



### OVA

El archivo OVA es un dispositivo virtual prediseñado que contiene un archivo XML OVF que especifica la configuración de hardware de la máquina virtual y un VMDK, que es el disco virtual en el que está instalado el sistema operativo. Un OVA está preconstruido y, por lo tanto, se puede implementar rápidamente para ponerse en marcha más rápido.

Una vez en funcionamiento, podemos comenzar a explorar el sistema operativo, familiarizarnos con las herramientas y realizar las personalizaciones deseadas. El equipo de Parrot Linux mantiene una variedad de documentación útil: 

___

### PRACTICANDO CON PARROT

Nos encontraremos con Parrot Linux en Academy. La versión en el navegador, o Pwnbox, está disponible en cualquier sección del módulo que requiera interacción con un host de destino en nuestro entorno de laboratorio. Haga clic en el botón Iniciar instancia a continuación y comience a familiarizarse con Pwnbox. Todas las secciones del módulo interactivo se pueden completar desde nuestra propia máquina virtual después de generar una imagen de Docker o generar un host de destino o múltiples hosts y descargar una clave VPN. El uso de Pwnbox no es un requisito, pero es útil porque todo el trabajo de Academy se puede completar dentro de nuestro navegador sin necesidad de ningún software de virtualización o recursos adicionales para ejecutar una máquina virtual.

Se puede acceder a las instancias de Docker sin necesidad de una conexión VPN independiente. Los hosts específicos (es decir, los objetivos de Active Directory) requieren acceso VPN si no se accede desde Pwnbox. Si este es el caso, aparecerá un botón para descargar una clave VPN después de generar el objetivo. Comenzaremos a trabajar con hosts de destino más adelante en este módulo. 

___

## **MANTENERSE ORGANIZADO**

Ya sea que estemos realizando evaluaciones de clientes, usando CTF, tomando un curso en Academy o en otro lugar, o usando Boxes/labs de HTB, la organización siempre es crucial. Es fundamental priorizar una documentación clara y precisa desde el principio. Esta habilidad nos beneficiará sin importar el camino que tomemos en seguridad de la información o incluso en otras carreras. 
___

### ESTRUCTURA DE CARPETAS

Al atacar una simple box, lab o cliente, debemos tener una estructura de carpetas clara en nuestra máquina de ataque para guardar datos tales como: información de alcance, datos de enumeración, evidencia de intentos de exploits, datos confidenciales como credenciales y otros datos obtenidos. durante el reconocimiento, la explotación y la posexplotación. Una estructura de carpetas de muestra puede tener el siguiente aspecto: 
~~~
$tree Projects/

Projects/
└── Acme Company
    ├── EPT
    │   ├── evidence
    │   │   ├── credentials
    │   │   ├── data
    │   │   └── screenshots
    │   ├── logs
    │   ├── scans
    │   ├── scope
    │   └── tools
    └── IPT
        ├── evidence
        │   ├── credentials
        │   ├── data
        │   └── screenshots
        ├── logs
        ├── scans
        ├── scope
        └── tools
~~~
Aquí tenemos una carpeta para el cliente Acme Company con dos evaluaciones, Prueba de Penetración Interna (IPT) y Prueba de Penetración Externa (EPT). Debajo de cada carpeta, tenemos subcarpetas para guardar datos de escaneo, cualquier herramienta relevante, salida de registro, información de alcance (es decir, listas de IP/redes para alimentar nuestras herramientas de escaneo) y una carpeta de evidencia que puede contener cualquier credencial recuperada durante la evaluación. , cualquier dato relevante recuperado, así como capturas de pantalla. 

Es una preferencia personal, pero algunas personas crean una carpeta para cada host de destino y guardan capturas de pantalla dentro de ella. Otros organizan sus notas por host o red y guardan capturas de pantalla directamente en la herramienta para tomar notas. Experimente con estructuras de carpetas y vea qué funciona mejor para mantenerse organizado y trabajar de manera más eficiente. 

___

### HERRAMIENTAS PARA TOMAR NOTAS

La productividad y la organización son muy importantes. Un pentester muy técnico pero desorganizado tendrá dificultades para tener éxito en esta industria. Se pueden utilizar varias herramientas para la organización y la toma de notas. Seleccionar una herramienta para tomar notas es muy individual. Es posible que algunos de nosotros no necesitemos una característica que otra persona requiere en función de su flujo de trabajo. Algunas excelentes opciones para explorar incluyen: 

+ [Cherrytree](https://www.giuspen.com/cherrytree)
+ [Visual Studio Code](https://code.visualstudio.com/)
+ [Evernote](https://evernote.com/)
+ [Notion](https://www.notion.so/)
+ [GitBook](https://www.gitbook.com/)
+ [Sublime Text](https://www.sublimetext.com/)
+ [Notepad++](https://notepad-plus-plus.org/downloads)

Algunos de estos están más enfocados en la toma de notas, mientras que otros, como Notion y GitBook, tienen características más ricas que se pueden usar para crear páginas de tipo Wiki, hojas de trucos y más. Es importante asegurarse de que los datos de los clientes solo se almacenen localmente y no se sincronicen con la nube si se utiliza una de estas herramientas en evaluaciones del mundo real. 
~~~
Sugerencia: aprender el lenguaje Markdown es fácil y muy útil para tomar notas, ya que se puede representar fácilmente de una manera organizada y visualmente atractiva. 
~~~

___


### OTRAS HERRAMIENTAS Y SUGERENCIAS

Cada profesional de seguridad de la información debe mantener una base de conocimientos. Puede tener el formato que elija (aunque se recomiendan las herramientas anteriores). Esta base de conocimientos debe contener guías de referencia rápida para las tareas de configuración que realizamos en la mayoría de las evaluaciones y cheat sheets para los comandos comunes que usamos en cada fase de una evaluación. .

A medida que completamos box, labs, evaluaciones, cursos de capacitación, etc., debemos agregar cada carga útil, comando, sugerencia, ya que nunca sabemos cuándo puede ser útil. Tenerlos accesibles aumentará nuestra eficiencia y productividad en general. Cada módulo de HTB Academy tiene una hoja de trucos con los comandos relevantes que se muestran en las secciones del módulo, que puede descargar y conservar para futuras consultas. 

También debemos mantener checklist, plantillas de informes para varios tipos de evaluación y crear una base de datos de hallazgos/vulnerabilidad. Esta base de datos puede tomar la forma de una hoja de cálculo o algo más complejo e incluir un título de hallazgo, descripción, impacto, consejos de remediación y referencias. Tener estos hallazgos ya escritos nos ahorrará un tiempo considerable y reelaboración durante la fase de informes, ya que la mayor parte de los hallazgos ya estarán escritos y probablemente solo requieran cierta personalización para el entorno de destino. 
___

## **CONECTARSE USANDO VPN**

Una red privada virtual (VPN) nos permite conectarnos a una red privada (interna) y acceder a hosts y recursos como si estuviéramos conectados directamente a la red privada de destino. Es un canal de comunicaciones seguro sobre redes públicas compartidas para conectarse a una red privada (es decir, un empleado que se conecta de forma remota a la red corporativa de su empresa desde su hogar). Las VPN brindan un grado de privacidad y seguridad al cifrar las comunicaciones a través del canal para evitar las escuchas y el acceso a los datos que atraviesan el canal. 

En un nivel alto, la VPN funciona enrutando la conexión a Internet de nuestro dispositivo de conexión a través del servidor privado de la VPN de destino en lugar de nuestro proveedor de servicios de Internet (ISP). Cuando se conecta a una VPN, los datos se originan en el servidor VPN en lugar de en nuestra computadora y parecerá que se originan en una dirección IP pública que no es la nuestra.

Hay dos tipos principales de VPN de acceso remoto: VPN basada en cliente y VPN SSL. SSL VPN utiliza el navegador web como cliente VPN. La conexión se establece entre el navegador y una puerta de enlace SSL VPN y se puede configurar para permitir el acceso únicamente a aplicaciones basadas en la web, como sitios de correo electrónico e intranet, o incluso a la red interna, pero sin la necesidad de que el usuario final instale o use ningun software especializado. La VPN basada en cliente requiere el uso de software de cliente para establecer la conexión VPN. Una vez conectado, el host del usuario funcionará principalmente como si estuviera conectado directamente a la red de la empresa y podrá acceder a cualquier recurso (aplicaciones, hosts, subredes, etc.) permitido por la configuración del servidor. Algunas VPN corporativas brindarán a los empleados acceso completo a la red corporativa interna, mientras que otras colocarán a los usuarios en un segmento específico reservado para trabajadores remotos. 
___


### ¿POR QUE USAR VPN?

Podemos usar un servicio VPN como NordVPN o Private Internet Access y conectarnos a un servidor VPN en otra parte de nuestro país o en otra región del mundo para ocultar nuestro tráfico de navegación o disfrazar nuestra dirección IP pública. Esto puede proporcionarnos cierto nivel de seguridad y privacidad. Aún así, dado que nos estamos conectando al servidor de una empresa, siempre existe la posibilidad de que se registren datos o que el servicio VPN no siga las mejores prácticas de seguridad o las características de seguridad que anuncian. El uso de un servicio VPN conlleva el riesgo de que el proveedor no haga lo que dice y registre todos los datos. El uso de un servicio VPN no garantiza el anonimato ni la privacidad, pero es útil para eludir ciertas restricciones de red/cortafuegos o cuando se conecta a una posible red hostil (es decir, una red inalámbrica pública del aeropuerto). Nunca se debe usar un servicio VPN con la idea de que nos protegerá de las consecuencias de realizar actividades nefastas. 
___

### CONECTAR A VPN HTB

HTB y otros servicios que ofrecen VM/redes vulnerables a propósito requieren que los jugadores se conecten a la red de destino a través de una VPN para acceder a la red del laboratorio privado. Los hosts dentro de las redes HTB no pueden conectarse directamente a Internet. Cuando nos conectamos a HTB VPN (o cualquier laboratorio de pruebas de penetración/piratería), siempre debemos considerar que la red es "hostil". Solo debemos conectarnos desde una máquina virtual, deshabilitar la autenticación de contraseña si SSH está habilitado en nuestra VM atacante, bloquear cualquier servidor web y no dejar información confidencial en nuestra VM de ataque (es decir, no usar HTB u otras redes vulnerables con la misma VM que utilizamos para realizar evaluaciones de clientes). Al conectarnos a una VPN (ya sea dentro de HTB Academy o la plataforma principal de HTB), nos conectamos usando el siguiente comando: 
~~~
$ sudo openvpn user.ovpn

Thu Dec 10 18:42:41 2020 OpenVPN 2.4.9 x86_64-pc-linux-gnu [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on Apr 21 2020
Thu Dec 10 18:42:41 2020 library versions: OpenSSL 1.1.1g  21 Apr 2020, LZO 2.10
Thu Dec 10 18:42:41 2020 Outgoing Control Channel Authentication: Using 256 bit message hash 'SHA256' for HMAC authentication
Thu Dec 10 18:42:41 2020 Incoming Control Channel Authentication: Using 256 bit message hash 'SHA256' for HMAC authentication
Thu Dec 10 18:42:41 2020 TCP/UDP: Preserving recently used remote address: [AF_INET]
Thu Dec 10 18:42:41 2020 Socket Buffers: R=[212992->212992] S=[212992->212992]
Thu Dec 10 18:42:41 2020 UDP link local: (not bound)
<SNIP>
Thu Dec 10 18:42:41 2020 Initialization Sequence Completed
~~~

La última línea Initialization Sequence Completed nos dice que nos conectamos con éxito a la VPN. 

Donde sudo le dice a nuestro host que ejecute el comando como usuario root elevado, openvpn es el cliente VPN y el archivo user.ovpn es la clave VPN que descargamos desde la sección del módulo Academy o la página principal de acceso a la plataforma HTB. Si escribimos ifconfig en otra ventana de terminal, veremos un adaptador tun si nos conectamos con éxito a la VPN. 
~~~
$ ifconfig

<SNIP>

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.10.x.2  netmask 255.255.254.0  destination 10.10.x.2
        inet6 dead:beef:1::2000  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::d82f:301a:a94a:8723  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen
~~~
Escribir netstat -rn nos mostrará las redes accesibles a través de la VPN. 

~~~
$netstat -rn

Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.1.2     0.0.0.0         UG        0 0          0 eth0
10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0
10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
~~~
Aquí puede ver que se puede acceder a la red 10.129.0.0/16 utilizada para las máquinas HTB Academy a través del adaptador tun0 a través de la red 10.10.14.0/23. 
___