## **GET**

Cada vez que visitamos cualquier URL, nuestros navegadores realizan una solicitud GET de forma predeterminada para obtener los recursos remotos alojados en esa URL. Una vez que el navegador recibe la página inicial que está solicitando; puede enviar otras solicitudes utilizando varios métodos HTTP. Esto se puede observar a través de la pestaña Network en las herramientas de desarrollo del navegador, como se vio en la sección anterior.

>Ejercicio: elija cualquier sitio web de su elección y controle la pestaña Network en las herramientas de desarrollo del navegador mientras lo visita para comprender el rendimiento de la página. Esta técnica se puede utilizar para comprender a fondo cómo una aplicación web interactúa con su backend, lo que puede ser un ejercicio esencial para cualquier evaluación de aplicaciones web o ejercicio de bug bounty.
___

### **HTTP AUTENTICACION BASICA**

Cuando visitamos el ejercicio que se encuentra al final de esta sección, nos solicita ingresar un nombre de usuario y una contraseña. A diferencia de los formularios de inicio de sesión habituales, que utilizan parámetros HTTP para validar las credenciales del usuario (por ejemplo, solicitud POST), este tipo de autenticación utiliza una `autenticación HTTP básica`, que el servidor web maneja directamente para proteger una página o directorio específico, sin interactuar directamente con la aplicación web

Para acceder a la página, debemos ingresar un par de credenciales válidas, que son `admin:admin` en este caso:

![](https://academy.hackthebox.com/storage/modules/35/http_auth_login.jpg)

Una vez que ingresamos las credenciales, obtendríamos acceso a la página:

![](https://academy.hackthebox.com/storage/modules/35/http_auth_index.jpg)

Intentemos acceder a la página con cURL y agregaremos `-i` para ver los encabezados de respuesta:

~~~
Juceco@htb[/htb]$ curl -i http://<SERVER_IP>:<PORT>/
HTTP/1.1 401 Authorization Required
Date: Mon, 21 Feb 2022 13:11:46 GMT
Server: Apache/2.4.41 (Ubuntu)
Cache-Control: no-cache, must-revalidate, max-age=0
WWW-Authenticate: Basic realm="Access denied"
Content-Length: 13
Content-Type: text/html; charset=UTF-8

Access denied
~~~

Como podemos ver, obtenemos `Access denied` en el cuerpo de la respuesta, y también obtenemos `Basic realm="Access denied"` en el encabezado `WWW-Authenticate`, lo que de hecho confirma que esta página usó `autenticación HTTP básica`, como se explica en la sección Encabezados . Para proporcionar las credenciales a través de cURL, podemos usar el indicador `-u`, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -u admin:admin http://<SERVER_IP>:<PORT>/

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
~~~

Esta vez obtenemos la página en la respuesta. Hay otro método en el que podemos proporcionar las credenciales de `autenticación HTTP básicas`, que es directamente a través de la URL como (`username:password@URL`), como discutimos en la primera sección. Si intentamos lo mismo con cURL o nuestro navegador, también obtendremos acceso a la página:

~~~
Juceco@htb[/htb]$ curl http://admin:admin@<SERVER_IP>:<PORT>/

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
~~~

También podemos intentar visitar la misma URL en un navegador, y también deberíamos autenticarnos.

>Ejercicio: intente ver los encabezados de respuesta agregando -i a la solicitud anterior y vea cómo una respuesta autenticada difiere de una no autenticada.
___

### **HTTP ENCABEZADO DE AUTORIZACION**

Si agregamos el indicador `-v` a cualquiera de nuestros comandos cURL anteriores:

~~~
Juceco@htb[/htb]$ curl -v http://admin:admin@<SERVER_IP>:<PORT>/

*   Trying <SERVER_IP>:<PORT>...
* Connected to <SERVER_IP> (<SERVER_IP>) port PORT (#0)
* Server auth using Basic with user 'admin'
> GET / HTTP/1.1
> Host: <SERVER_IP>
> Authorization: Basic YWRtaW46YWRtaW4=
> User-Agent: curl/7.77.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Mon, 21 Feb 2022 13:19:57 GMT
< Server: Apache/2.4.41 (Ubuntu)
< Cache-Control: no-store, no-cache, must-revalidate
< Expires: Thu, 19 Nov 1981 08:52:00 GMT
< Pragma: no-cache
< Vary: Accept-Encoding
< Content-Length: 1453
< Content-Type: text/html; charset=UTF-8
< 

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
~~~

Como usamos la `autenticación HTTP básica`, vemos que nuestra solicitud HTTP establece el encabezado `Authorization` en `Basic YWRtaW46YWRtaW4=`, que es el valor codificado en base64 de `admin:admin`. Si estuviéramos usando un método moderno de autenticación (por ejemplo, `JWT`), la `Authorization` sería de tipo `Bearer` y contendría un token encriptado más largo.

Intentemos configurar manualmente la `Authorization`, sin proporcionar las credenciales, para ver si nos permite acceder a la página. Podemos configurar el encabezado con el indicador `-H` y usaremos el mismo valor de la solicitud HTTP anterior. Podemos agregar el indicador `-H` varias veces para especificar varios encabezados:

~~~
Juceco@htb[/htb]$ curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/

<!DOCTYPE html
<html lang="en">

<head>
...SNIP...
~~~

Como vemos, esto también nos dio acceso a la página. Estos son algunos métodos que podemos usar para autenticarnos en la página. La mayoría de las aplicaciones web modernas utilizan formularios de inicio de sesión creados con el lenguaje de secuencias de comandos de back-end (por ejemplo, PHP), que utilizan solicitudes HTTP POST para autenticar a los usuarios y luego devuelven una cookie para mantener su autenticación.
___

### **PARAMETROS GET**

Una vez que estamos autenticados, tenemos acceso a una función de búsqueda de ciudades, en la que podemos ingresar un término de búsqueda y obtener una lista de ciudades coincidentes:

![](https://academy.hackthebox.com/storage/modules/35/http_auth_index.jpg)

A medida que la página devuelve nuestros resultados, puede ponerse en contacto con un recurso remoto para obtener la información y luego mostrarlos en la página. Para verificar esto, podemos abrir las herramientas de desarrollo del navegador e ir a la pestaña Network, o usar el atajo [`CTRL+SHIFT+E`] para llegar a la misma pestaña. Antes de ingresar nuestro término de búsqueda y ver las solicitudes, es posible que debamos hacer clic en el ícono de la papelera en la parte superior izquierda, para asegurarnos de eliminar las solicitudes anteriores y solo monitorear las solicitudes más nuevas:

![](https://academy.hackthebox.com/storage/modules/35/network_clear_requests.jpg)

Después de eso, podemos ingresar cualquier término de búsqueda y presionar enter, e inmediatamente notaremos que se envía una nueva solicitud al backend:

![](https://academy.hackthebox.com/storage/modules/35/web_requests_get_search.jpg)

Cuando hacemos clic en la solicitud, se envía a `search.php` con el parámetro GET `search=le` utilizado en la URL. Esto nos ayuda a comprender que la función de búsqueda solicita otra página para los resultados.

Ahora, podemos enviar la misma solicitud directamente a `search.php` para obtener los resultados de búsqueda completos, aunque probablemente los devolverá en un formato específico (por ejemplo, JSON) sin tener el diseño HTML que se muestra en la captura de pantalla anterior.

Para enviar una solicitud GET con cURL, podemos usar exactamente la misma URL que se ve en las capturas de pantalla anteriores, ya que las solicitudes GET colocan sus parámetros en la URL. Sin embargo, las herramientas de desarrollo del navegador brindan un método más conveniente para obtener el comando cURL. Podemos hacer clic con el botón derecho en la solicitud y seleccionar `Copy>Copy as cURL.` Luego, podemos pegar el comando copiado en nuestra terminal y ejecutarlo, y deberíamos obtener exactamente la misma respuesta:

~~~
Juceco@htb[/htb]$ curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='

Leeds (UK)
Leicester (UK)
~~~

>Nota: El comando copiado contendrá todos los encabezados utilizados en la solicitud HTTP. Sin embargo, podemos eliminar la mayoría de ellos y mantener solo los encabezados de autenticación necesarios, como el encabezado de `Authorization`.

También podemos repetir la solicitud exacta dentro de las herramientas de desarrollo del navegador, seleccionando `Copy>Copy as Fetch`. Esto copiará la misma solicitud HTTP utilizando la libreria JavaScript Fetch. Luego, podemos ir a la pestaña de la consola de JavaScript haciendo clic en [`CTRL+MAYÚS+K`], pegar nuestro comando Fetch y presionar enter para enviar la solicitud:

![](https://academy.hackthebox.com/storage/modules/35/web_requests_fetch_search.jpg)

Como vemos, el navegador envió nuestra solicitud y podemos ver la respuesta devuelta después. Podemos hacer clic en la respuesta para ver sus detalles, ampliar varios detalles y leerlos.

>Nota: en caso que aparezca `Scam Warning: Take care when pasting things you don’t understand...` se puede escribir `allow pasting`. No hay problema si aparece `Uncaught SyntaxError: unexpected token: identifier` ya estaria permitido, podremos reiniciar firefox para asegurar. En caso de que no funcione podemos probar mas metodos ingresando [aqui](https://www.brainytechz.com/2021/05/how-to-fix-mozilla-firefox-scam-warning.html)
___

### RETO

El ejercicio anterior parece estar roto, ya que arroja resultados incorrectos. Use las herramientas de desarrollo del navegador para ver cuál es la solicitud que envía cuando buscamos, y use cURL para buscar 'flag' y obtener la bandera.

R: flag: HTB{curl_g3773r}

~~~
┌──(root㉿kali)-[/home/kali]
└─# curl 'http://178.62.119.24:30083/search.php?search=flag' -H 'Authorization: Basic YWRtaW46YWRtaW4='
flag: HTB{curl_g3773r}
~~~