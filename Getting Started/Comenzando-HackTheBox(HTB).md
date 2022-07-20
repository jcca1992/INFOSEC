## **EMPEZANDO**

Como principiante en seguridad de la información, puede ser extremadamente desalentador saber por dónde empezar. Hemos visto a personas de todos los ámbitos de la vida comenzar con poco o ningún conocimiento y tener éxito en la plataforma HTB y, en consecuencia, comenzar el viaje hacia una carrera técnica. Hay muchos recursos excelentes para principiantes, que incluyen capacitación gratuita y paga, máquinas y laboratorios vulnerables, sitios web de tutoriales, blogs, canales de YouTube, entre otros.

A lo largo de nuestro viaje, veremos continuamente los términos aprendizaje `guided` y `exploratory`.

HTB Academy sigue un enfoque de aprendizaje guiado en el que los estudiantes trabajan en un módulo sobre un tema determinado, leen el material y reproducen los ejemplos para reforzar los temas presentados. La mayoría de las secciones del módulo tienen uno o más ejercicios prácticos para evaluar el conocimiento de los estudiantes sobre un tema determinado. Muchos módulos culminan en una evaluación de habilidades de varios pasos para evaluar la comprensión de los estudiantes del material presentado dentro de las secciones del módulo cuando se aplica a un escenario del mundo real.

El aprendizaje guiado tiene la ventaja de proporcionar a los estudiantes métodos estructurados para aprender varias técnicas de una manera que construya correctamente su conocimiento, además de proporcionar material adicional, conocimientos previos y vínculos del mundo real para aprender sobre un tema en profundidad mientras se fuerza. para que pongan a prueba sus conocimientos en varios puntos de control a lo largo del proceso de aprendizaje.

La plataforma principal de HTB sigue un enfoque de aprendizaje exploratorio para poner a los usuarios en una amplia variedad de escenarios del mundo real en los que tienen que usar sus habilidades técnicas y procesos como la enumeración para lograr un objetivo, a menudo desconocido. La plataforma ofrece desafíos únicos en categorías como ingeniería inversa, criptografía, esteganografía, pwn, web, análisis forense, OSINT, dispositivos móviles, hardware y más en varios niveles de dificultad diseñados para evaluar las habilidades técnicas y de pensamiento crítico.

También hay máquinas individuales (Boxes) de varios tipos de sistemas operativos, minilaboratorios pequeños (y desafiantes) llamados Endgames, Fortresses que son máquinas individuales que contienen muchos desafíos y Pro Labs, que son grandes redes empresariales simuladas donde los usuarios pueden realizar un simulacro. prueba de penetración en varios niveles de dificultad.

Siempre hay máquinas y desafíos "activos" gratuitos que los usuarios deben abordar desde un enfoque de "caja negra" o con poco o ningún conocimiento previo de la tarea en cuestión. Las máquinas, los desafíos y los finales se "retiran" y están disponibles para los usuarios VIP junto con tutoriales oficiales para ayudar en el proceso de aprendizaje. Cuando el contenido se retira de la plataforma, la comunidad puede crear tutoriales en blogs y videos. Vale la pena leer varios blogs/ver varios videos en la misma máquina retirada para ver las diferentes perspectivas y estilos que los usuarios toman al abordar una tarea para comenzar a construir el enfoque con el que se sienta más cómodo.

El principal beneficio del enfoque de aprendizaje exploratorio es que nos permite confiar en nuestras habilidades para ingresar a las máquinas y resolver desafíos, lo que nos ayuda a construir nuestras metodologías y técnicas y nos ayuda a dar forma a nuestro estilo de prueba de penetración.

**Siempre es bueno mezclar los dos estilos de aprendizaje para desarrollar nuestras habilidades con la estructura adecuada de conocimiento y desafiarnos a nosotros mismos a profundizar nuestra comprensión de las habilidades que aprendimos.**

___

### RECURSOS

Al comenzar, la gran cantidad de contenido disponible en la web puede ser abrumadora. Además, no es fácil saber por dónde empezar y la calidad de los materiales disponibles. Lo que sigue son algunos recursos fuera de HTB que recomendamos a cualquiera que comience su viaje o busque mejorar su conjunto de habilidades y aprender nuevos trucos.

#### *MAQUINAS VULNERABLES/APLICACIONES*

Hay muchos recursos disponibles para practicar vulnerabilidades web y de red comunes en un entorno seguro y controlado. Los siguientes son algunos ejemplos de aplicaciones web vulnerables a propósito y máquinas vulnerables que podemos configurar en un entorno de laboratorio para practicar más.
|||
|--|--|
|[OWAS Juice Shop](https://owasp.org/www-project-juice-shop/)|Es una aplicación web vulnerable moderna escrita en Node.js, Express y Angular que muestra todo el [OWASP Top Ten](https://owasp.org/www-project-top-ten) junto con muchas otras fallas de seguridad de aplicaciones del mundo real.|
| [Metasploitable 2](https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/) | Es una máquina virtual Ubuntu Linux deliberadamente vulnerable que se puede usar para practicar la enumeración, la explotación automatizada y manual.|
| [Metasploitable 3](https://github.com/rapid7/metasploitable3)| Es una plantilla para crear una máquina virtual de Windows vulnerable configurada con una amplia gama de vulnerabilidades.|
|[DVWA](https://github.com/digininja/DVWA)|Esta es una aplicación web PHP/MySQL vulnerable que muestra muchas vulnerabilidades comunes de aplicaciones web con diversos grados de dificultad.|

Vale la pena aprender a configurarlos en su entorno de laboratorio para obtener práctica adicional configurando máquinas virtuales y trabajando con configuraciones comunes, como configurar un servidor web. Aparte de estas máquinas/aplicaciones vulnerables, también podemos configurar muchas máquinas y aplicaciones en un entorno de laboratorio para practicar la configuración, enumeración/explotación y remediación.
___
#### *CANALES DE YOUTUBE*

Hay muchos canales de YouTube que muestran pruebas de penetración/técnicas de hacking. Algunos que vale la pena marcar son:

|||
|--|--|
|[IppSec](https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA)| Proporciona un recorrido extremadamente detallado de cada maquina HTB retirada llena de información de su propia experiencia, así como videos sobre diversas técnicas.|
|[VbScrub](https://www.youtube.com/channel/UCpoyhjwNIWZmsiKNKpsMAQQ)|Proporciona videos HTB, así como videos sobre técnicas, centrándose principalmente en la explotación de Active Directory.|
|[STÖK](https://www.youtube.com/channel/UCQN2DsjnYH60SFBIA6IkNwg)|Proporciona videos sobre varios temas relacionados con la seguridad de la información, centrándose principalmente en recompensas de errores y pruebas de penetración de aplicaciones web.|
|[LiveOverflow](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w)| Proporciona videos sobre una amplia variedad de temas técnicos de seguridad de la información.|
#### *BLOGS*

Hay demasiados blogs por ahí para enumerarlos a todos. Si realiza una búsqueda en Google de un tutorial de la mayoría de las maquinas HTB retiradas, generalmente se encontrará con los mismos blogs una y otra vez. Estos pueden ser excelentes para ver la perspectiva de otra persona sobre el mismo tema, especialmente si sus publicaciones contienen información "adicional" sobre el objetivo que otros blogs no cubren.
Un gran blog que vale la pena revisar es [0xdf hacks stuff](https://0xdf.gitlab.io/). Este blog tiene recorridos fantásticos de la mayoría de las cajas HTB retiradas, cada una con una sección "Beyong Root" que cubre algún aspecto único de la caja que notó el autor. El blog también tiene publicaciones sobre diversas técnicas, análisis de malware y reseñas de eventos anteriores de CTF.

En cualquier punto del proceso de aprendizaje, vale la pena leer tanto material como sea posible para comprender mejor un tema y obtener diferentes perspectivas. Además de los blogs relacionados con cajas HTB retiradas, también vale la pena buscar artículos de blog sobre exploits/ataques recientes, técnicas de explotación de Active Directory, artículos de eventos de CTF y artículos de informes de bug bounty. Todos estos pueden contener una gran cantidad de información que puede ayudar a conectar algunos puntos en nuestro aprendizaje o incluso enseñarnos algo nuevo que puede ser útil en una evaluación.
#### *TUTORIALES WEBSITES*

Existen muchos sitios web de tutoriales para practicar las habilidades informáticas fundamentales, como la creación de scripts.
Dos excelentes sitios web de tutoriales son [Under The Wire](https://www.underthewire.tech/index.htm) y [Over The Wire](https://overthewire.org/wargames/). Estos sitios web están configurados para ayudar a capacitar a los usuarios en el uso de `Windows PowerShell` y la línea de comandos de Linux, respectivamente, a través de varios escenarios en un formato de "juegos de guerra".
Llevan al usuario a través de varios niveles, que consisten en tareas o desafíos para capacitarlos en el uso básico y avanzado de la línea de comandos de Windows y Linux y scripts de `Bash` y `PowerShell`. Estas habilidades son primordiales para cualquiera que busque tener éxito en esta industria.
#### *HTB STARTING POINT*

[Starting Point](https://www.hackthebox.eu/home/start) es una introducción a los laboratorios HTB y máquinas/desafíos básicos. Después de completar un tutorial que cubre la conexión a VPN, la enumeración, la obtención de un foothold y la escalada de privilegios contra un solo objetivo, se nos presentan varias máquinas clasificadas como `easy` que pueden ser atacadas antes de acceder al resto de la plataforma HTB.
#### *HTB TRACKS*

En la plataforma principal de HTB Tracks, "selecciones de máquinas y desafíos unidos para que los usuarios progresen, dominando un tema en particular". Las pistas cubren una variedad de temas y se agregan continuamente a la plataforma. Su objetivo es ayudar a los estudiantes a mantenerse enfocados en una meta específica de manera estructurada mientras siguen un enfoque de aprendizaje exploratorio.
#### *MAQUINAS HTB PARA PRINCIPIANTES*

||||||
|--|--|--|--|--|
|[Lame](https://www.hackthebox.eu/home/machines/profile/1)|[Blue](https://www.hackthebox.eu/home/machines/profile/51)|[Nibbles](https://www.hackthebox.eu/home/machines/profile/121)|[Shocker](https://www.hackthebox.eu/home/machines/profile/108)|[Jerry](https://www.hackthebox.eu/home/machines/profile/144)|

Si prefiere ver un tutorial en video mientras trabaja en una máquina fácil, las siguientes listas de reproducción del canal de IppSec tienen tutoriales para varias maquinas HTB fáciles de Linux/Windows:

|||
|--|--|
|[Easy Linux Boxes](https://www.youtube.com/watch?list=PLidcsTyj9JXJfpkDrttTdk1MNT6CDwVZF&v=byYZl9CSFtM)|[Easy Windows Boxes](https://www.youtube.com/watch?list=PLidcsTyj9JXL4Jv6u9qi8TcUgsNoKKHNn&v=N2ahkarb-zI)|
#### *DESAFIOS HTB PARA PRINCIPIANTES*

La plataforma HTB contiene desafíos únicos en una variedad de categorías. Algunos desafíos para principiantes incluyen:

||||
|--|--|--|
|[Find The Easy Pass](https://www.hackthebox.eu/home/challenges/Reversing?name=Find%20The%20Easy%20Pass)|[Weak RSA](https://www.hackthebox.eu/home/challenges/Crypto?name=Weak%20RSA)|[You know 0xDiablos](https://www.hackthebox.eu/home/challenges/Pwn?name=You%20know%200xDiablos)|
#### *DANTE PROLAB*

La plataforma HTB tiene varios Pro Labs que son redes empresariales simuladas con muchos hosts interconectados que los jugadores pueden usar para practicar sus habilidades en una red que contiene múltiples objetivos.
La explotación exitosa de hosts específicos generará información que ayudará a los jugadores cuando ataquen hosts encontrados más adelante en el laboratorio.

El Dante Pro Lab es el laboratorio más amigable para principiantes ofrecido hasta la fecha. Este laboratorio está dirigido a jugadores con cierta experiencia en la realización de ataques de red y aplicaciones web y una comprensión de los conceptos de redes y los conceptos básicos de las metodologías de penetración, como escaneo/enumeración, movimiento lateral, escalada de privilegios, posexplotación, etc.

Ahora que hemos cubierto la terminología y las técnicas básicas y el escaneo/enumeración, juntemos las piezas recorriendo paso a paso un cuadro HTB fácil de calificar.
___
## **NAVEGANDO POR HTB**

Hack The Box proporciona una gran cantidad de información para cualquier persona que se inicie en las pruebas de penetración o busque mejorar sus habilidades. El sitio web ofrece muchas oportunidades de aprendizaje, por lo que es imprescindible comprender su estructura y diseño para aprovechar al máximo la experiencia de aprendizaje.
___
### PERFIL

Puede acceder a su página de perfil de HTB en el panel izquierdo o haciendo clic en su nombre de usuario en el panel superior.

![Perfil](https://academy.hackthebox.com/storage/modules/77/htb_profile.jpg)

Su página de perfil muestra sus estadísticas de HTB, incluido su rango, el progreso hacia el siguiente rango, el porcentaje para poseer varios desafíos y laboratorios de HTB y otras estadísticas similares.

![Estadistica de perfil](https://academy.hackthebox.com/storage/modules/77/htb_profile_2.jpg)

También puede encontrar estadísticas detalladas sobre su máquina y el progreso del desafío, junto con su historial de progreso para cada uno. También puede encontrar varias insignias y certificados que ha obtenido. Puede compartir su página de perfil haciendo clic en el botón Compartir perfil.
___
### RANKINGS

La página de clasificaciones de HTB muestra las clasificaciones actuales de usuarios, equipos, universidades, países y miembros VIP.

![ranquin](https://academy.hackthebox.com/storage/modules/77/htb_rankings.jpg)

También puede ver su clasificación entre otros usuarios, su mejor clasificación y su progreso general. Además de eso, su clasificación y puntos se suman a la clasificación de su país, que también puede ver en la página de clasificación de países. Si estás en un equipo o universidad, tus puntos también se suman. Si es un usuario con una suscripción VIP, puede ver su rango VIP, que cuenta los puntos ganados en todas las máquinas, incluidas las máquinas retiradas. Cuando comenzamos en la página principal de HackTheBox, vemos la pestaña Labs en el panel lateral, que incluye las siguientes secciones principales:

|||||||
|--|--|--|--|--|--|
|`Tracks`| `Machines`|`Challenges`|`Fortress`|`Endgame`|`Pro Lab`|

![Clasificacion](https://academy.hackthebox.com/storage/modules/77/htb_main_1.jpg)


___
### TRACKS

![](https://academy.hackthebox.com/storage/modules/77/htb_tracks.jpg)

HTB Tracks es una excelente función que ayuda a planificar sus próximas máquinas y desafíos. Un track o ruta es una selección de máquinas y desafíos unidos para que los usuarios progresen a través del dominio de un tema en particular. Ya sea que recién esté comenzando, quiera probar sus habilidades de Active Directory o esté listo para un desafío en Expert Track, encontrará una ruta adecuada que incluye una gran selección de máquinas y desafíos que lo ayudarán a mejorar su conjunto de habilidades. en un área específica. Las rutas son creadas por el equipo de HTB, empresas, universidades e incluso usuarios. Cuando haces clic en una ruta, verás todas sus máquinas y desafíos, tu progreso en cada uno y tu progreso general en la ruta.

![](https://academy.hackthebox.com/storage/modules/77/beginner_track.jpg)

Puede inscribirse fácilmente en la ruta y comenzar a trabajar en ella.
___
### MAQUINAS

A continuación, tenemos la página de Máquinas, una de las páginas más populares de HackTheBox.

![](https://academy.hackthebox.com/storage/modules/77/htb_machines_1.jpg)

Lo primero que verá son dos máquinas recomendadas que puede jugar, una es la última máquina semanal lanzada y la otra es una máquina Staff Pick recomendada por el personal de HTB.

![](https://academy.hackthebox.com/storage/modules/77/htb_machines_2.jpg)

Si se desplaza hacia abajo, encontrará una lista de todas las máquinas HTB en dos pestañas: `Active` y `Retired`.

Las Máquinas Activas son las que te dan puntos para tu ranking, pero tendrás que resolverlas por tu cuenta usando tus conocimientos de pentesting. Siempre hay 20 máquinas activas repartidas entre dificultades. Se agrega una nueva máquina semanalmente, y una de las activas se retira, y sus puntos se borran para todos.

Las máquinas retiradas son todas las máquinas presentadas anteriormente como una máquina activa semanal. Puede encontrar un tutorial para cada una de ellas, pero las máquinas retiradas no le darán ningún punto para su clasificación, aunque sí le otorgan puntos de clasificación VIP, como se discutió anteriormente.

>Nota: Solo se puede acceder a las máquinas retiradas con una suscripción VIP, ya que solo se puede acceder de forma gratuita a las dos máquinas retiradas más recientemente.

Puede filtrar las máquinas según las máquinas que haya completado o no y según su dificultad o tipo de sistema operativo. También puede ordenar las máquinas por su fecha de lanzamiento, calificación o dificultad calificada por el usuario. Si hacemos clic en cualquier máquina, se nos lleva a su página específica de máquina.

![](https://academy.hackthebox.com/storage/modules/77/machine_page.jpg)

Podrá jugar en la máquina haciendo clic en `Join Machine`, después de lo cual obtendrá la IP de la máquina, a la que podrá acceder una vez que esté conectado a través de `HTB VPN`. También puede enviar los indicadores de usuario y root que encuentre en esta página.

Si la máquina está retirada, puede hacer clic en la pestaña Tutoriales para ver una lista de tutoriales proporcionados, tanto escritos como en videos. Finalmente, puede consultar las estadísticas y las pestañas de actividad para ver las estadísticas y la actividad de los usuarios más recientes.
___
### DESAFIOS

![](https://academy.hackthebox.com/storage/modules/77/htb_challenges.jpg)

El diseño de la página de desafíos es similar al de la página de máquinas. Encontrarás desafíos activos y retirados clasificados en diez categorías diferentes, cada una de las cuales tiene un máximo de 10 desafíos. Puede hacer clic en cualquier categoría para obtener una vista previa de la lista de desafíos dentro de ella, y luego puede hacer clic en cualquier desafío para ver su página y enviar sus banderas.
___
### FORTRESS

Las Fortress son laboratorios vulnerables creados por empresas externas y alojados en HackTheBox.

![](https://academy.hackthebox.com/storage/modules/77/htb_fortress_1.jpg)

Cada laboratorio tiene varias flags que se pueden encontrar y enviar a la página de Fortress. Una vez que haya completado el laboratorio encontrando todas las flags, recibirá una insignia de la empresa que creó la fortaleza. Algunas empresas también brindan ofertas de trabajo que están vinculadas a completar los laboratorios para calificar. Necesitas tener el rango HTB Hacker y superior para jugar fortalezas. Intenta subir tu clasificación jugando máquinas activas y desafíos para calificar.


___
### ENDGAME

Los endgames son laboratorios virtuales que contienen varias máquinas conectadas a una sola red. Los escenarios se esfuerzan por reflejar una situación del mundo real que puede encontrar al realizar una prueba de penetración para una empresa real.

![](https://academy.hackthebox.com/storage/modules/77/htb_endgame_1.jpg)

Al igual que las máquinas, cada laboratorio de Endgame tiene una ruta de ataque específica que debes explotar. Sin embargo, como Endgames tiene múltiples máquinas, podemos aprender rutas de ataque específicas que de otro modo no podríamos aprender usando una sola máquina. Debes tener el ragon Gurú en HTB y superior para jugar Active Endgames. Los finales retirados solo están disponibles para usuarios con una suscripción VIP y se pueden jugar en cualquier rango.
___
### PRO LABS

Los Pro Labs son la mejor experiencia de laboratorio, ya que están diseñados para simular una infraestructura empresarial del mundo real, lo cual es una excelente oportunidad para probar sus habilidades de pentesting.

![](https://academy.hackthebox.com/storage/modules/77/htb_prolab_dante.jpg)

Los laboratorios profesionales son grandes y pueden tardar un tiempo en terminar y aprender todas sus rutas de ataque y desafíos de seguridad. Cada Pro Lab tiene un escenario específico y un nivel de dificultad:

| Lab | Escenario |
| -- | -- |
| `Dante`| Apto para principiantes para aprender técnicas y metodologías comunes de pentesting, herramientas comunes de pentesting y vulnerabilidades comunes.| 
| `Offshore`| Laboratorio de Active Directory que simula una red corporativa del mundo real.|
| `Cybernetics` | Simula un entorno de red de Active Directory completamente mejorado y actualizado, que está protegido contra ataques. Está dirigido a probadores de penetración experimentados y Red Teamers. |
| `RastaLabs` | Entorno de simulación Red Team, que presenta una combinación de configuraciones erróneas de ataque y usuarios simulados. |
| `APTLabs` | Este laboratorio simula un ataque dirigido por un agente de amenaza externo contra un MSP (proveedor de servicios administrados) y es el laboratorio profesional más avanzado que se ofrece en este momento. |

Pro Labs requiere un plan de suscripción por separado. Una vez que completa un Pro Lab, obtiene un Certificado de finalización HTB.
___
### BATTLEGROUNDS

La última incorporación a HackThebox es HTB Battlegrounds.

![](https://academy.hackthebox.com/storage/modules/77/htb_battlegrounds.jpg)

HTB Battlegrounds es un juego de estrategia y hacking en tiempo real. Puedes jugar en un equipo de 4 o en un equipo de 2.

Las batallas de Cyber Mayhem se basan en el estilo ataque/defensa, en el que a cada equipo se le asignan varias máquinas que tienen que defenderse de los ataques mientras atacan las máquinas del otro equipo. Cada ataque/defensa te da una cierta cantidad de puntos, y cada bandera recogida también cuenta. Juegas durante una cierta cantidad de tiempo y el equipo con más puntos al final gana.

HTB Battlegrounds está disponible para que todos jueguen, pero hay un límite en la cantidad de partidos permitidos, de la siguiente manera:

| Estatus | Partidas |
| -- | -- |
| Free User | 2 partidas por mes |
| VIP | 5 partidas por mes |
| VIP+ | 10 partidas por mes |

El modo Server Siege es un estilo solo de ataque, en el que gana el equipo que puede hackear al otro equipo más rápido. Puedes encontrar un artículo detallado sobre HTB Battlegrounds en este [enlace](https://help.hackthebox.com/en/articles/5185620-gs-how-to-play-battlegrounds).
___