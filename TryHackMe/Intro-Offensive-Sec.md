## INTRODUCCION A LA SEGURIDAD OFENSIVA

Antes de entrar en las carreras de seguridad cibernética y qué es la seguridad ofensiva, vamos a hackear (y sí, es legal, todos los ejercicios son simulaciones falsas)

Tu primer hack

Haga clic en el botón "Iniciar máquina". Una vez cargado, tendrá acceso a una máquina que usará para piratear una aplicación bancaria falsa llamada `FakeBank`.

Usaremos una aplicación de línea de comandos llamada `GoBuster` para usar fuerza bruta en el sitio web de `FakeBank` para encontrar directorios y páginas ocultos. `GoBuster` tomará una lista de posibles nombres de páginas o directorios e intentará acceder a un sitio web con cada uno de ellos; si la página existe, te lo dice.

Paso 1) Abre una terminal

Una terminal, también conocida como línea de comandos, nos permite interactuar con una computadora sin usar una interfaz gráfica de usuario. En la máquina, abra la terminal usando el ícono Terminal: ![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5bec5dfd73790a7d06282266/room-content/443d573c553e59e4897aa99d2e77b679.png)


Paso 2) Encuentra páginas web ocultas

La mayoría de las empresas tendrán una página de portal de administración, que le dará a su personal acceso a los controles administrativos básicos para las operaciones diarias. Para un banco, un empleado puede necesitar transferir dinero hacia y desde las cuentas de los clientes. A menudo, estas páginas no se hacen privadas, lo que permite a los atacantes encontrar páginas ocultas que muestran o dan acceso a controles de administración o datos confidenciales.

Escriba el siguiente comando en la terminal para encontrar páginas potencialmente ocultas en el sitio web de FakeBank utilizando GoBuster (una aplicación de seguridad de línea de comandos).

~~~
ubuntu@tryhackme:~/Desktop$ gobuster -u http://fakebank.com -w wordlist.txt

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://fakebank.com/
[+] Threads      : 10
[+] Wordlist     : wordlist.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2022/07/25 23:41:55 Starting gobuster
=====================================================
/images (Status: 301)
/bank-transfer (Status: 200)
=====================================================
2022/07/25 23:42:04 Finished
=====================================================

~~~

No se preocupe si no ha usado una terminal antes: ¡TryHackMe lo guía a través de todo!

En el comando anterior, `-u` se usa para indicar el sitio web que estamos escaneando, `-w` toma una lista de palabras para iterar y encontrar páginas ocultas.

Verá que GoBuster escanea el sitio web con cada palabra en la lista, encontrando páginas que existen en el sitio. GoBuster le habrá dicho las páginas que encontró en la lista de nombres de páginas/directorios (indicado por Estado: 200).

~~~
=====================================================
/images (Status: 301)
/bank-transfer (Status: 200)
=====================================================
~~~

Paso 3) Hackear el banco

Debería haber encontrado una página de transferencia bancaria secreta que le permita transferir dinero entre cuentas en el banco (/bank-transfer). Escriba la página oculta en el sitio web de FakeBank en la máquina.

Esta página permite a un atacante robar dinero de cualquier cuenta bancaria, lo cual es un riesgo crítico para el banco. Como hacker ético, usted (con permiso) encontrará vulnerabilidades en su aplicación y las informará al banco para que las arregle antes de que un hacker las explote.

Transfiera $2000 de la cuenta bancaria 2276 a su cuenta (número de cuenta 8881).

### PREGUNTAS
+ Cuando hayas transferido dinero a tu cuenta, vuelve a la página de tu cuenta bancaria. ¿Cuál es la respuesta que se muestra en su página de saldo bancario?

`R: BANK-HACKED`
___

### ¿QUE ES SEGURIDAD OFENSIVA?

En resumen, la seguridad ofensiva es el proceso de irrumpir en los sistemas informáticos, explotar errores de software y encontrar lagunas en las aplicaciones para obtener acceso no autorizado a ellas.

Para vencer a un hacker, debe comportarse como un pirata informático, encontrar vulnerabilidades y recomendar parches antes de que lo haga un ciberdelincuente.

Por otro lado, también está la seguridad defensiva, que es el proceso de proteger la red y los sistemas informáticos de una organización mediante el análisis y la seguridad de cualquier amenaza digital potencial; Obtenga más información en la sala `digital forensics`.

En un rol cibernético defensivo, podría estar investigando computadoras o dispositivos infectados para comprender cómo fue hackeado, rastrear a los ciberdelincuentes o monitorear la infraestructura en busca de actividad maliciosa.
___

### ¿COMO PUEDO COMENZAR?

Las personas a menudo se preguntan cómo otros se convierten en hackers (consultores de seguridad) o defensores (analistas de seguridad que luchan contra el ciberdelito), y la respuesta es simple. Divídalo, aprenda un área de seguridad cibernética que le interese y practique regularmente con ejercicios prácticos. Desarrolle el hábito de aprender un poco cada día en TryHackMe o cualquier otra plataforma y adquirirá el conocimiento para obtener su primer trabajo en la industria.

### ¿QUE CARRERAS HAY?

La sala de carreras cibernéticas profundiza en las diferentes carreras cibernéticas. Sin embargo, aquí hay una breve descripción de algunos roles de seguridad ofensivos:

+ `Pentester`: responsable de probar productos tecnológicos para encontrar vulnerabilidades de seguridad explotables.
+ `Red Teamer`: desempeña el papel de un adversario, ataca a una organización y proporciona comentarios desde la perspectiva del enemigo.
+ `Ingeniero de seguridad`: diseñe, supervise y mantenga controles, redes y sistemas de seguridad para ayudar a prevenir ataques cibernéticos.
___