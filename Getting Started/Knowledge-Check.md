Ahora reunamos todo lo que aprendimos en este módulo y ataquemos nuestro primera maquina sin guía.

### TIPS

Recuerde que la enumeración es un proceso iterativo. Después de realizar nuestros escaneos de puertos `Nmap`, asegúrese de realizar una enumeración detallada de todos los puertos abiertos en función de lo que se está ejecutando en los puertos descubiertos. Sigue el mismo proceso que hicimos con `Nibbles`:

+ Enumeración y escaneo con `Nmap` - realice un escaneo rápido de puertos abiertos seguido de un escaneo de puerto completo.

+ Huella web: verifique cualquier puerto web identificado para ejecutar aplicaciones web, cualquier archivo o directorio oculto. Algunas herramientas útiles para esta fase incluyen `whatweb` y `Gobuster`

+ Si identifica la URL del sitio web, puede agregarla a su archivo '/etc/hosts' con la IP que obtiene en la pregunta a continuación, para que se cargue normalmente "aunque esto no es necesario".

+ Después de identificar las tecnologías en uso, use una herramienta como `Searchsploit` para encontrar exploits públicos o busque en Google técnicas de explotación manual.

+ Después de obtener un punto de apoyo inicial, use el truco `pty` de `Python3` para actualizar a un pseudo TTY

+ Realice una enumeración manual y automatizada del sistema de archivos en busca de configuraciones incorrectas, servicios con vulnerabilidades conocidas y datos confidenciales en texto no cifrado, como credenciales.

+ Organice estos datos fuera de línea para determinar las diversas formas de escalar los privilegios para rootear en este objetivo

Hay dos formas de ganar foothold: una usando `Metasploit` y otra a través de un proceso manual. Nos desafiamos a trabajar y obtener una comprensión de ambos métodos.

Hay dos formas de escalar los privilegios para rootear en el objetivo después de obtener un punto de apoyo. Utilice scripts de ayuda como [LinEnum](https://github.com/rebootuser/LinEnum) y [LinPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) para ayudarlo. Filtre la información con las dos conocidas técnicas de escalada de privilegios.

## RETO

Genere el objetivo, consiga un punto de apoyo y envíe el contenido de la bandera user.txt.

Después de obtener un punto de apoyo en el objetivo, aumente los privilegios a la raíz y envíe el contenido del indicador root.txt.