## HTTP REQUEST

En la sección anterior, descubrimos que la función principal de `secret.js` es enviar una solicitud `POST` vacía a `/serial.php`. En esta sección, intentaremos hacer lo mismo usando `cURL` para enviar una solicitud `POST` a `/serial.php`. Para obtener más información sobre `cURL` y las solicitudes web, puede consultar el módulo `Web Requests`.
___

### **CURL**

`cURL` es una poderosa herramienta de línea de comandos utilizada en distribuciones de Linux, macOS e incluso las últimas versiones de Windows PowerShell. Podemos solicitar cualquier sitio web simplemente proporcionando su URL, y lo obtendríamos en formato de texto, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl http://SERVER_IP:PORT/

</html>
<!DOCTYPE html>

<head>
    <title>Secret Serial Generator</title>
    <style>
        *,
        html {
            margin: 0;
            padding: 0;
            border: 0;
...SNIP...
        <h1>Secret Serial Generator</h1>
        <p>This page generates secret serials!</p>
    </div>
</body>

</html>
~~~

Este es el mismo `HTML` por el que pasamos cuando revisamos el código fuente en la primera sección.
___

### **SOLICITUD POST**

Para enviar una solicitud `POST`, debemos agregar el indicador `-X POST` a nuestro comando, y debe enviar una solicitud `POST`:

~~~
Juceco@htb[/htb]$ curl -s http://SERVER_IP:PORT/ -X POST
~~~

>Sugerencia: agregamos el indicador "-s" para reducir el desorden de la respuesta con datos innecesarios

Sin embargo, la solicitud `POST` generalmente contiene datos `POST`. Para enviar datos, podemos usar el indicador "`-d "param1=sample"`" e incluir nuestros datos para cada parámetro, de la siguiente manera:

~~~
Juceco@htb[/htb]$ curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"
~~~

Ahora que sabemos cómo usar `cURL` para enviar solicitudes `POST` básicas, en la siguiente sección, utilizaremos esto para replicar lo que hace `server.js` para comprender mejor su propósito.
___

### **RETO**

+ Intente aplicar lo que aprendió en esta sección enviando una solicitud 'POST' a '/serial.php'. ¿Cuál es la respuesta que obtienes?

`R: N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz`

~~~
┌──(root㉿kali)-[/home/kali]
└─# curl -s http://134.122.111.24:31366/serial.php -X POST -d "param1=sample"
N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz                                                                                 
~~~