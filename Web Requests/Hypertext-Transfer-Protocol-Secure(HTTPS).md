## **HYPERTEXT TRANSFER PROTOCOL SECURE (HTTPS)**

En la sección anterior, discutimos cómo se envían y procesan las solicitudes HTTP. Sin embargo, uno de los inconvenientes importantes de HTTP es que todos los datos se transfieren en texto claro. Esto significa que cualquier persona entre el origen y el destino puede realizar un ataque Man-in-the-middle (MiTM) para ver los datos transferidos.

Para contrarrestar este problema, se creó el [protocolo HTTPS (HTTP Secure)](https://tools.ietf.org/html/rfc2660), en el que todas las comunicaciones se transfieren en un formato encriptado, por lo que incluso si un tercero intercepta la solicitud, no podrá extraer los datos de ella. Por esta razón, HTTPS se ha convertido en el esquema principal para sitios web en Internet, y HTTP se está eliminando gradualmente, y pronto la mayoría de los navegadores web no permitirán visitar sitios web HTTP.
___

### **DESCRIPCION HTTPS**

Si examinamos una solicitud HTTP, podemos ver el efecto de no aplicar comunicaciones seguras entre un navegador web y una aplicación web. Por ejemplo, el siguiente es el contenido de una solicitud de inicio de sesión HTTP:

![](https://academy.hackthebox.com/storage/modules/35/https_clear.png)

Podemos ver que las credenciales de inicio de sesión se pueden ver en texto claro. Esto facilitaría que alguien en la misma red (como una red inalámbrica pública) capture la solicitud y reutilice las credenciales con fines maliciosos.

Por el contrario, cuando alguien intercepta y analiza el tráfico de una solicitud HTTPS, vería algo como lo siguiente:

![](https://academy.hackthebox.com/storage/modules/35/https_google_enc.png)

Como podemos ver, los datos se transfieren como un único flujo cifrado, lo que hace que sea muy difícil para cualquiera capturar información como credenciales o cualquier otro dato confidencial.

Los sitios web que aplican HTTPS se pueden identificar a través de `https://` en su URL (por ejemplo, https://www.google.com), así como el icono de candado en la barra de direcciones del navegador web, a la izquierda de la URL:

![](https://academy.hackthebox.com/storage/modules/35/https_google.png)

Entonces, si visitamos un sitio web que utiliza HTTPS, como Google, todo el tráfico estaría encriptado.

>Nota: Aunque los datos transferidos a través del protocolo HTTPS pueden estar encriptados, la solicitud aún puede revelar la URL visitada si se contactó con un servidor DNS de texto sin cifrar. Por esta razón, se recomienda utilizar servidores DNS encriptados (p. ej., 8.8.8.8 o 1.2.3.4) o utilizar un servicio VPN para garantizar que todo el tráfico esté encriptado correctamente.

Veamos cómo funciona HTTPS a un alto nivel:

![](https://academy.hackthebox.com/storage/modules/35/HTTPS_Flow.png)

Si escribimos `http://` en lugar de `https://` para visitar un sitio web que aplica HTTPS, el navegador intenta resolver el dominio y redirige al usuario al servidor web que aloja el sitio web de destino. Primero se envía una solicitud al puerto `80`, que es el protocolo HTTP sin cifrar. El servidor detecta esto y redirige al cliente al puerto HTTPS seguro `443` en su lugar. Esto se hace a través del código de respuesta `301 Moved Permanently`, que discutiremos en una próxima sección.

A continuación, el cliente (navegador web) envía un paquete de "hola del cliente", brindando información sobre sí mismo. Después de esto, el servidor responde con "servidor hola", seguido de un [intercambio de claves](https://en.wikipedia.org/wiki/Key_exchange) para intercambiar certificados SSL. El cliente verifica la clave/certificado y envía uno propio. Después de esto, se inicia un [protocolo de enlace](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake) cifrado para confirmar si el cifrado y la transferencia funcionan correctamente.

Una vez que el protocolo de enlace se completa con éxito, continúa la comunicación HTTP normal, que se cifra después de eso. Esta es una descripción general de muy alto nivel del intercambio de claves, que está más allá del alcance de este módulo.

>Nota: Dependiendo de las circunstancias, un atacante puede realizar un ataque de degradación de HTTP, que degrada la comunicación de HTTPS a HTTP, haciendo que los datos se transfieran en texto claro. Esto se hace configurando un proxy Man-In-The-Middle (MITM) para transferir todo el tráfico a través del host del atacante sin el conocimiento del usuario. Sin embargo, la mayoría de los navegadores, servidores y aplicaciones web modernos protegen contra este ataque.
___

### **cURL PARA HTTPS**

cURL debe manejar automáticamente todos los estándares de comunicación HTTPS y realizar un protocolo de enlace seguro y luego cifrar y descifrar los datos automáticamente. Sin embargo, si alguna vez nos ponemos en contacto con un sitio web con un certificado SSL no válido o desactualizado, cURL de forma predeterminada no procederá con la comunicación para protegerse contra los ataques MITM mencionados anteriormente:

~~~
Juceco@htb[/htb]$ curl https://inlanefreight.com

curl: (60) SSL certificate problem: Invalid certificate chain
More details here: https://curl.haxx.se/docs/sslcerts.html
...SNIP...
~~~

Los navegadores web modernos harían lo mismo, advirtiendo al usuario que no visite un sitio web con un certificado SSL no válido.

Es posible que enfrentemos un problema de este tipo al probar una aplicación web local o con una aplicación web alojada con fines prácticos, ya que es posible que dichas aplicaciones web aún no hayan implementado un certificado SSL válido. Para omitir la verificación del certificado con cURL, podemos usar el indicador `-k`:

~~~
Juceco@htb[/htb]$ curl -k https://inlanefreight.com

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
...SNIP...
~~~

Como podemos ver, la solicitud pasó esta vez y recibimos los datos de respuesta.