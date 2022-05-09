## ** HTTP HEADERS (CABECERAS)**

Hemos visto ejemplos de solicitudes HTTP y encabezados de respuesta en la sección anterior. Dichos encabezados HTTP pasan información entre el cliente y el servidor. Algunos encabezados solo se usan con solicitudes o respuestas, mientras que otros encabezados generales son comunes a ambos.

Los encabezados pueden tener uno o varios valores, agregados después del nombre del encabezado y separados por dos puntos. Podemos dividir los encabezados en las siguientes categorías:

+ `Encabezados generales`
+ `Encabezados de entidad`
+ `Encabezados de solicitud`
+ `Encabezados de respuesta`
+ `Encabezados de seguridad`

Analicemos cada una de estas categorías.
___

### **ENCABEZADOS GENERALES**

Los [encabezados generales](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html) se utilizan tanto en las solicitudes como en las respuestas HTTP. Son contextuales y se utilizan para` describir el mensaje más que su contenido`.

|Encabezado|Ejemplo|Descripción|
|--|--|--|
|`Date`|`Date: Wed, 16 Feb 2022 10:38:44 GMT`| Contiene la fecha y la hora en que se originó el mensaje. Es preferible convertir la hora a la zona horaria [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) estándar.|
|`Connection`| `Connection: close` | Dicta si la conexión de red actual debe permanecer activa después de que finalice la solicitud. Dos valores de uso común para este encabezado son `close` y `keep-alive`. El valor `close` del cliente o del servidor significa que les gustaría terminar la conexión, mientras que el encabezado `keep-alive` indica que la conexión debe permanecer abierta para recibir más datos y entradas.|
___

### **ENCABEZADOS DE ENTIDAD**

Al igual que los encabezados generales, los [encabezados de entidad](https://www.w3.org/Protocols/rfc2616/rfc2616-sec7.html) pueden ser `comunes tanto para la solicitud como para la respuesta`. Estos encabezados se utilizan para `describir el contenido` (entidad) transferido por un mensaje. Por lo general, se encuentran en respuestas y solicitudes POST o PUT.

|Encabezado|Ejemplo|Descripción|
|--|--|--|
| `Content-Type` | `Content-Type: text/html` | Se utiliza para describir el tipo de recurso que se transfiere. Los navegadores agregan automáticamente el valor en el lado del cliente y lo devuelven en la respuesta del servidor. El campo `charset` indica el estándar de codificación, como [UTF-8](https://en.wikipedia.org/wiki/UTF-8).|
|`Media-Type` | `Media-Type: application/pdf`| El `media-type` es similar al `Content-Type` y describe los datos que se transfieren. Este encabezado puede desempeñar un papel crucial para que el servidor interprete nuestra entrada. El campo `charset` también se puede usar con este encabezado.|
|
|
|
___
### **ENCABEZADOS DE SOLICITUD**

|Encabezado|Ejemplo|Descripción|
|--|--|--|
___
### **ENCABEZADOS DE RESPUESTA**

|Encabezado|Ejemplo|Descripción|
|--|--|--|
___
### **ENCABEZADOS DE SEGURIDAD**

|Encabezado|Ejemplo|Descripción|
|--|--|--|
___
### **cURL**
___
### **DEVTOOLS DE NAVEGADOR**


