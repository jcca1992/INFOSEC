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

Estas cuatro operaciones están vinculadas principalmente a las API CRUD comúnmente conocidas, pero el mismo principio también se usa en las API REST y varios otros tipos de API. Por supuesto, no todas las API funcionan de la misma manera, y el control de acceso de los usuarios limitará qué acciones podemos realizar y qué resultados podemos ver. El módulo Introducción a las aplicaciones web explica con más detalle estos conceptos, por lo que puede consultarlo para obtener más detalles sobre las API y su uso.