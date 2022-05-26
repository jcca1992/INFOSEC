## ANALISIS DE CODIGO

Ahora que hemos desofuscado el código, podemos comenzar a revisarlo:

~~~
'use strict';
function generateSerial() {
  ...SNIP...
  var xhr = new XMLHttpRequest;
  var url = "/serial.php";
  xhr.open("POST", url, true);
  xhr.send(null);
};
~~~

Vemos que el archivo `secret.js` contiene solo una función, `generateSerial`.
___

### **HTTP REQUEST**

Veamos cada línea de la función `generateSerial`.

#### *VARIABLES DEL CODIGO*

La función comienza definiendo una variable `xhr`, que crea un objeto de `XMLHttpRequest`. Como es posible que no sepamos exactamente qué hace `XMLHttpRequest` en JavaScript, busquemos `XMLHttpRequest` en Google para ver para qué se usa.
Después de leer sobre esto, vemos que es una función de JavaScript que maneja las solicitudes web.

La segunda variable definida es la variable `URL`, que contiene una URL a `/serial.php`, que debe estar en el mismo dominio, ya que no se especificó ningún dominio.

#### *FUNCIONES DEL CODIGO*

A continuación, vemos que `xhr.open` se usa con `"POST"` y `URL`. Podemos buscar en Google esta función una vez más, y vemos que abre la solicitud HTTP definida como `GET` o `POST` en la URL, y luego la siguiente línea `xhr.send` enviaría la solicitud.

Por lo tanto, todo lo que está haciendo `generateSerial` es simplemente enviar una solicitud `POST` a `/serial.php`, sin incluir ningún dato `POST` ni recuperar nada a cambio.

Los desarrolladores pueden haber implementado esta función cada vez que necesitan generar una serie, como al hacer clic en un determinado botón `Generate Serial`, por ejemplo. Sin embargo, dado que no vimos ningún elemento HTML similar que genere publicaciones seriadas, los desarrolladores aún no deben haber usado esta función y la guardaron para uso futuro.

Con el uso de desofuscación de código y análisis de código, pudimos descubrir esta función. Ahora podemos intentar replicar su funcionalidad para ver si se maneja en el lado del servidor al enviar una solicitud `POST`. Si la función está habilitada y manejada en el lado del servidor, podemos descubrir una funcionalidad inédita, que generalmente tiende a tener errores y vulnerabilidades dentro de ellos.

