## **CRUD API**

Vimos ejemplos de una aplicación web de `búsqueda de ciudades` que utiliza parámetros de PHP para buscar el nombre de una ciudad en las secciones anteriores. Esta sección analizará cómo una aplicación web de este tipo puede utilizar las API para realizar lo mismo, e interactuaremos directamente con el punto final de la API.
___

### **APIs**

Hay varios tipos de API. Muchas API se utilizan para interactuar con una base de datos, de modo que podamos especificar la tabla solicitada y la fila solicitada dentro de nuestra consulta API, y luego usar un método HTTP para realizar la operación necesaria. Por ejemplo, para el punto final `api.php` en nuestro ejemplo, si quisiéramos actualizar la tabla `city` en la base de datos, y la fila que actualizaremos tiene el nombre de ciudad de `london`, entonces la URL se vería así:

~~~
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...SNIP...
~~~
___

### **CRUD**

Como podemos ver, podemos especificar fácilmente la tabla y la fila en la que queremos realizar una operación a través de dichas API. Entonces podemos utilizar diferentes métodos HTTP para realizar diferentes operaciones en esa fila. En general, las API realizan 4 operaciones principales en la entidad de la base de datos solicitada:

|Operación|Método HTTP|Descripción|
|--|--|--|
|`Create`|`POST`| Agrega los datos especificados a la tabla de la base de datos.|
|`Read`|`GET`| Lee la entidad especificada de la tabla de la base de datos.|
|`Updated`|`PUT`| Actualiza los datos de la tabla de base de datos especificada.|
|`Delete`|`DELETE`| Elimina la fila especificada de la tabla de la base de datos.|

Estas cuatro operaciones están vinculadas principalmente a las API CRUD comúnmente conocidas, pero el mismo principio también se usa en las API REST y varios otros tipos de API. Por supuesto, no todas las API funcionan de la misma manera, y el control de acceso de los usuarios limitará qué acciones podemos realizar y qué resultados podemos ver. El módulo [Introducción a las aplicaciones web](https://academy.hackthebox.com/module/details/75) explica con más detalle estos conceptos, por lo que puede consultarlo para obtener más detalles sobre las API y su uso.
___

### **READ**

Lo primero que haremos al interactuar con una API es leer datos. Como se mencionó anteriormente, simplemente podemos especificar el nombre de la tabla después de la API (por ejemplo, /`city`) y luego especificar nuestro término de búsqueda (por ejemplo, /`london`), de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl http://<SERVER_IP>:<PORT>/api.php/city/london

[{"city_name":"London","country_name":"(UK)"}]
~~~

Vemos que el resultado se envía como una cadena JSON. Para formatearlo correctamente en formato JSON, podemos canalizar la salida a la utilidad `jq`, que lo formateará correctamente. También silenciaremos cualquier salida cURL innecesaria con `-s`, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq

[
  {
    "city_name": "London",
    "country_name": "(UK)"
  }
]
~~~

Como podemos ver, obtuvimos la salida en una salida bien formateada. También podemos proporcionar un término de búsqueda y obtener todos los resultados coincidentes:

~~~
Juceco@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/le | jq

[
  {
    "city_name": "Leeds",
    "country_name": "(UK)"
  },
  {
    "city_name": "Dudley",
    "country_name": "(UK)"
  },
  {
    "city_name": "Leicester",
    "country_name": "(UK)"
  },
  ...SNIP...
]
~~~

Finalmente, podemos pasar una cadena vacía para recuperar todas las entradas en la tabla:

~~~
Juceco@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq

[
  {
    "city_name": "London",
    "country_name": "(UK)"
  },
  {
    "city_name": "Birmingham",
    "country_name": "(UK)"
  },
  {
    "city_name": "Leeds",
    "country_name": "(UK)"
  },
  ...SNIP...
]
~~~

>Intente visitar cualquiera de los enlaces anteriores usando su navegador, para ver cómo se representa el resultado.
___

### **CREATE**

Para agregar una nueva entrada, podemos usar una solicitud HTTP POST, que es bastante similar a lo que hemos realizado en la sección anterior. Simplemente podemos PUBLICAR nuestros datos JSON y se agregarán a la tabla. Como esta API usa datos JSON, también estableceremos el encabezado `Content-Type` en JSON, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
~~~

Ahora, podemos leer el contenido de la ciudad que agregamos (`HTB_City`), para ver si se agregó con éxito:

~~~
Juceco@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq

[
  {
    "city_name": "HTB_City",
    "country_name": "HTB"
  }
]
~~~

Como podemos ver, se creó una nueva ciudad, que antes no existía.

>Ejercicio: intente agregar una nueva ciudad a través de las herramientas de desarrollo del navegador, usando una de las solicitudes Fetch POST que usó en la sección anterior.
___

### **UPDATE**

Ahora que sabemos cómo leer y escribir entradas a través de las API, comencemos a discutir otros dos métodos HTTP que no hemos usado hasta ahora: `PUT` y `DELETE`. Como se mencionó al comienzo de la sección, `PUT` se usa para actualizar las entradas de la API y modificar sus detalles, mientras que `DELETE` se usa para eliminar una entidad específica.

>Nota: El método HTTP `PATCH` también se puede usar para actualizar las entradas de la API en lugar de `PUT`. Para ser precisos, `PATCH` se usa para actualizar parcialmente una entrada (solo modifica algunos de sus datos "por ejemplo, solo city_name"), mientras que `PUT` se usa para actualizar la entrada completa. También podemos usar el método HTTP `OPTIONS` para ver cuál de los dos es aceptado por el servidor y luego usar el método apropiado en consecuencia. En esta sección, nos centraremos en el método `PUT`, aunque su uso es bastante similar.

Usar `PUT` es bastante similar a `POST` en este caso, con la única diferencia de que tenemos que especificar el nombre de la entidad que queremos editar en la URL, de lo contrario, la API no sabrá qué entidad editar. Entonces, todo lo que tenemos que hacer es especificar el nombre de la `ciudad` en la URL, cambiar el método de solicitud a `PUT` y proporcionar los datos JSON como hicimos con `POST`, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
~~~

