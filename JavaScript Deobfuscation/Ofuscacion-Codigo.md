## OFUSCACION DE CODIGO

Antes de comenzar a aprender sobre la `desofuscación`, primero debemos aprender sobre la `ofuscación del código`. Sin comprender cómo se ofusca el código, es posible que no podamos desofuscar el código con éxito, especialmente si se ofuscó con un ofuscador personalizado.
___

### **QUE ES OFUSCACION**

La ofuscación es una técnica utilizada para hacer que un script sea más difícil de leer para los humanos, pero permite que funcione igual desde un punto de vista técnico, aunque el rendimiento puede ser más lento. Esto generalmente se logra automáticamente mediante el uso de una herramienta de ofuscación, que toma el código como entrada e intenta volver a escribir el código de una manera que sea mucho más difícil de leer, según su diseño.

Por ejemplo, los ofuscadores de código a menudo convierten el código en un diccionario de todas las palabras y símbolos utilizados dentro del código y luego intentan reconstruir el código original durante la ejecución haciendo referencia a cada palabra y símbolo del diccionario. El siguiente es un ejemplo de un código JavaScipt simple ofuscado:

![](https://academy.hackthebox.com/storage/modules/41/obfuscation_example.jpg)

Los códigos escritos en muchos idiomas se publican y ejecutan sin compilarse en lenguajes `interpretados`, como `Python`, `PHP` y `JavaScript`. Si bien `Python` y `PHP` generalmente residen en el lado del servidor y, por lo tanto, están ocultos para los usuarios finales, `JavaScript` generalmente se usa dentro de los navegadores en el `lado del cliente`, y el código se envía al usuario y se ejecuta en texto sin cifrar. Esta es la razón por la que la ofuscación se usa muy a menudo con `JavaScript`.
___

### **CASOS DE USO**

Hay muchas razones por las que los desarrolladores pueden considerar ofuscar su código. Una razón común es ocultar el código original y sus funciones para evitar que se reutilice o se copie sin el permiso del desarrollador, lo que dificulta la ingeniería inversa de la funcionalidad original del código. Otra razón es proporcionar una capa de seguridad cuando se trata de autenticación o cifrado para evitar ataques a las vulnerabilidades que puedan encontrarse dentro del código.

>IMPORTANTE: Debe tenerse en cuenta que no se recomienda realizar la autenticación o el cifrado en el lado del cliente, ya que el código es más propenso a los ataques de esta manera.

Sin embargo, el uso más común de ofuscación es para acciones maliciosas. Es común que los atacantes y los actores malintencionados ofusquen sus scripts maliciosos para evitar que los sistemas de detección y prevención de intrusiones los detecten. En la siguiente sección, aprenderemos a ofuscar un código JavaScript simple e intentaremos ejecutarlo antes y después de la ofuscación para notar las diferencias.