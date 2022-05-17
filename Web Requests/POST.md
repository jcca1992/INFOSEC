## **POST**

En la sección anterior, vimos cómo las solicitudes `GET` pueden ser utilizadas por aplicaciones web para funcionalidades como búsqueda y acceso a páginas. Sin embargo, cada vez que las aplicaciones web necesitan transferir archivos o mover los parámetros de usuario de la URL, utilizan solicitudes `POST`.

A diferencia de HTTP GET, que coloca los parámetros de usuario dentro de la URL, HTTP POST coloca los parámetros de usuario dentro del cuerpo de la solicitud HTTP. Esto tiene tres beneficios principales:

+ `Falta de registro:` dado que las solicitudes POST pueden transferir archivos grandes (por ejemplo, la carga de archivos), no sería eficiente que el servidor registrara todos los archivos cargados como parte de la URL solicitada, como sería el caso de un archivo cargado a través de una solicitud GET. .
+ `Menos requisitos de codificación:` las direcciones URL están diseñadas para compartirse, lo que significa que deben ajustarse a los caracteres que se pueden convertir en letras. La solicitud POST coloca datos en el cuerpo que pueden aceptar datos binarios. Los únicos caracteres que deben codificarse son los que se utilizan para separar parámetros.
+ `Se pueden enviar más datos:` la longitud máxima de URL varía entre navegadores (Chrome/Firefox/IE), servidores web (IIS, Apache, nginx), redes de entrega de contenido (Fastly, Cloudfront, Cloudflare) e incluso acortadores de URL (bit.ly , amzn.to). En términos generales, la longitud de una URL debe mantenerse por debajo de los 2000 caracteres, por lo que no pueden manejar una gran cantidad de datos.

Entonces, veamos algunos ejemplos de cómo funcionan las solicitudes POST y cómo podemos utilizar herramientas como cURL o herramientas de desarrollo del navegador para leer y enviar solicitudes POST.
___

### **FORMULARIOS DE INICIO DE SESION**

El ejercicio al final de esta sección es similar al ejemplo que vimos en la sección GET. Sin embargo, una vez que visitamos la aplicación web, vemos que utiliza un formulario de inicio de sesión PHP en lugar de autenticación básica HTTP:

![](https://academy.hackthebox.com/storage/modules/35/web_requests_post_login.jpg)

Si intentamos iniciar sesión con `admin:admin`, ingresamos y vemos una función de búsqueda similar a la que vimos anteriormente en la sección GET:

![](https://academy.hackthebox.com/storage/modules/35/web_requests_login_search.jpg)

Si borramos la pestaña Network en las herramientas de desarrollo de nuestro navegador e intentamos iniciar sesión nuevamente, veremos que se envían muchas solicitudes. Podemos filtrar las solicitudes por la IP de nuestro servidor, por lo que solo mostraría las solicitudes que van al servidor web de la aplicación web (es decir, filtrar las solicitudes externas), y notaremos que se envía la siguiente solicitud POST:

![](https://academy.hackthebox.com/storage/modules/35/web_requests_login_request.jpg)

Podemos hacer clic en la solicitud, hacer clic en la pestaña `Request` (que muestra el cuerpo de la solicitud) y luego hacer clic en el botón `Raw` para mostrar los datos de la solicitud sin procesar. Vemos que los siguientes datos se envían como datos de solicitud POST:

~~~
username=admin&password=admin
~~~

Con los datos de la solicitud a mano, podemos intentar enviar una solicitud similar con cURL, para ver si esto también nos permitiría iniciar sesión. Además, como hicimos en la sección anterior, simplemente podemos hacer clic derecho en la solicitud y seleccionar `Copy>Copy as cURL.` Sin embargo, es importante poder crear solicitudes POST manualmente, así que intentemos hacerlo.

Usaremos el indicador `-X POST` para enviar una solicitud `POST`. Luego, para agregar nuestros datos POST, podemos usar el indicador `-d` y agregar los datos anteriores después, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/

...SNIP...
        <em>Type a city name and hit <strong>Enter</strong></em>
...SNIP...
~~~

Si examinamos el código HTML, no veremos el código del formulario de inicio de sesión, pero veremos el código de la función de búsqueda, lo que indica que efectivamente nos autenticamos.

>Consejo: Muchos formularios de inicio de sesión nos redirigirían a una página diferente una vez autenticados (por ejemplo, /dashboard.php). Si queremos seguir la redirección con cURL, podemos usar el indicador `-L`.
___

### **COOKIES AUTENTICADAS**

Si nos autenticamos con éxito, deberíamos haber recibido una cookie para que nuestros navegadores puedan conservar nuestra autenticación y no necesitamos iniciar sesión cada vez que visitamos la página. Podemos usar los indicadores `-v` o `-i` para ver la respuesta, que debe contener el encabezado `Set-Cookie` con nuestra cookie autenticada:

~~~
Juceco@htb[/htb]$ curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -i

HTTP/1.1 200 OK
Date: 
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1; path=/

...SNIP...
        <em>Type a city name and hit <strong>Enter</strong></em>
...SNIP...
~~~

Con nuestra cookie autenticada, ahora deberíamos poder interactuar con la aplicación web sin necesidad de proporcionar nuestras credenciales cada vez. Para probar esto, podemos configurar la cookie anterior con el indicador `-b` en cURL, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/

...SNIP...
        <em>Type a city name and hit <strong>Enter</strong></em>
...SNIP...
~~~

Como podemos ver, de hecho fuimos autenticados y llegamos a la función de búsqueda. También es posible especificar la cookie como encabezado, de la siguiente manera:

~~~
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
~~~

