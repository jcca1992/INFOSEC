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

