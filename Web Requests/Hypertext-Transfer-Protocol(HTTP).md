## **HYPERTEXT TRANSFER PROTOCOL (HTTP)**

Protocolo de transferencia de hipertexto. Hoy en día, la mayoría de las aplicaciones que usamos interactúan constantemente con Internet, tanto web como aplicaciones móviles. La mayoría de las comunicaciones por Internet se realizan con solicitudes web a través del protocolo HTTP. [HTTP](https://tools.ietf.org/html/rfc2616) es un protocolo de nivel de aplicación utilizado para acceder a los recursos de la World Wide Web. El término `hipertexto` significa texto que contiene enlaces a otros recursos y texto que los lectores pueden interpretar fácilmente.

La comunicación HTTP consta de un cliente y un servidor, donde el cliente solicita al servidor un recurso. El servidor procesa las solicitudes y devuelve el recurso solicitado. El puerto predeterminado para la comunicación HTTP es el puerto `80`, aunque se puede cambiar a cualquier otro puerto, según la configuración del servidor web. Las mismas solicitudes se utilizan cuando usamos Internet para visitar diferentes sitios web. Ingresamos un `"Fully Qualified Domain Name"` (`FQDN`) que seria nombre de dominio completo  como un `"Uniform Resourse Locator"` (`URL`) que en español seria localizador uniforme de recursos para llegar al sitio web deseado, como www.hackthebox.com.
___

### **URL**

Se accede a los recursos sobre HTTP a través de una URL, lo que ofrece muchas más especificaciones que simplemente especificar un sitio web que queremos visitar. Veamos la estructura de una URL:

![](https://academy.hackthebox.com/storage/modules/35/url_structure.png)

Esto es lo que significa cada componente:

| Componente | Ejemplo | Descripcion |
| -- | -- | -- |
| `Scheme` | `http://https://` | Se utiliza para identificar el protocolo al que accede el cliente y termina con dos puntos y una barra inclinada doble. (`://`) |
| `User Info` | `admin:password@` | Este es un componente opcional que contiene las credenciales (separadas por dos puntos `:`) utilizadas para autenticarse en el host, y está separada del host con un signo de arroba (`@`) |
| `Host` | `inlanefreight.com` | El host significa la ubicación del recurso. Puede ser un nombre de host o una dirección IP |
| `Port` | `:80` | El Puerto está separado del `Host` por dos puntos (`:`). Si no se especifica ningún puerto, los esquemas `http` se establecen de forma predeterminada en el puerto `80` y `https` en el puerto `443`. |
| `Path` | `/dashboard.php` | Esto apunta al recurso al que se accede, que puede ser un archivo o una carpeta. Si no se especifica una ruta, el servidor devuelve el recurso predeterminado (por ejemplo, `index.html`). |
| `Query String` | `?login=true` | El Query String comienza con un signo de interrogación (`?`) y consta de un parámetro (p. ej., `Login`) y un valor (p. ej., `True`). Los parámetros múltiples se pueden separar con un ampersand (`&`). |
| `Fragments` | `#status` | Los fragmentos son procesados por los navegadores del lado del cliente para ubicar secciones dentro del recurso principal (por ejemplo, un encabezado o sección en la página). |

No todos los componentes son necesarios para acceder a un recurso. Los principales campos obligatorios son el scheme y el host, sin los cuales la solicitud no tendría recurso para solicitar.
___

### **FLUJO HTTP**

![](https://academy.hackthebox.com/storage/modules/35/HTTP_Flow.png)

El diagrama anterior presenta la anatomía de una solicitud HTTP a un nivel muy alto. La primera vez que un usuario ingresa la URL (`inlanefreight.com`) en el navegador, envía una solicitud a un servidor DNS (Resolución de nombres de dominio) para resolver el dominio y obtener su IP. El servidor DNS busca la dirección IP de `inlanefreight.com` y la devuelve. Todos los nombres de dominio deben resolverse de esta manera, ya que un servidor no puede comunicarse sin una dirección IP.

>Nota: Nuestros navegadores generalmente primero buscan registros en el archivo local '`/etc/hosts`' y, si el dominio solicitado no existe dentro de él, entonces contactarían con otros servidores DNS. Podemos usar '`/etc/hosts`' para agregar manualmente registros para la resolución de DNS, agregando la IP seguida del nombre de dominio.

Una vez que el navegador obtiene la dirección IP vinculada al dominio solicitado, envía una solicitud GET al puerto HTTP predeterminado (por ejemplo, `80`), solicitando la raiz `/`ruta. Luego, el servidor web recibe la solicitud y la procesa. De forma predeterminada, los servidores están configurados para devolver un archivo de índice cuando se recibe una solicitud de `/`.

En este caso, el servidor web lee y devuelve el contenido de `index.html` como una respuesta HTTP. La respuesta también contiene el código de estado (por ejemplo, `200 OK`), que indica que la solicitud se procesó correctamente. Luego, el navegador web muestra el contenido de `index.html` y se lo presenta al usuario.

>Nota: este módulo se centra principalmente en las solicitudes web HTTP. Para obtener más información sobre HTML y aplicaciones web, puede consultar el módulo [Introducción a las aplicaciones web](https://academy.hackthebox.com/module/details/75).
___

### **cURL**

En este módulo, enviaremos solicitudes web a través de dos de las herramientas más importantes para cualquier pentester web, un navegador web, como Chrome o Firefox, y la herramienta de línea de comando `cURL`.

[cURL](https://curl.haxx.se/) (URL de cliente) es una herramienta y biblioteca de línea de comandos que admite principalmente HTTP junto con muchos otros protocolos. Esto lo convierte en un buen candidato para secuencias de comandos y automatización, lo que lo hace esencial para enviar varios tipos de solicitudes web desde la línea de comandos, lo cual es necesario para muchos tipos de pruebas de penetración web.