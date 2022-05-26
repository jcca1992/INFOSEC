## DESOFUSCACION

Ahora que entendemos cómo funciona la ofuscación del código, comencemos nuestro aprendizaje hacia la deofuscación. Así como existen herramientas para ofuscar el código automáticamente, existen herramientas para embellecer y desofuscar el código automáticamente.
___

### **EMBELLECER**

Vemos que el código actual que tenemos está todo escrito en una sola línea. Esto se conoce como código `JavaScript minificado`. Para formatear correctamente el código, necesitamos `Embellecer` nuestro código. El método más básico para hacerlo es a través de nuestras `herramientas de desarrollo de navegador`.

Por ejemplo, si estuviéramos usando Firefox, podemos abrir el depurador del navegador con [`CTRL+SHIFT+Z`] y luego hacer clic en nuestro script `secret.js`. Esto mostrará la secuencia de comandos en su formato original, pero podemos hacer clic en el botón `'{ }'` en la parte inferior, que imprimirá la secuencia de comandos en su formato JavaScript adecuado:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_pretty_print.jpg)

Además, podemos utilizar muchas herramientas en línea o complementos de edición de código, como [Prettier](https://prettier.io/playground/) o [Beautifier](https://beautifier.io/). Copiemos el script `secret.js`:

~~~
eval(function (p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }('g 4(){0 5="6{7!}";0 1=8 a();0 2="/9.c";1.d("e",2,f);1.b(3)}', 17, 17, 'var|xhr|url|null|generateSerial|flag|HTB|flag|new|serial|XMLHttpRequest|send|php|open|POST|true|function'.split('|'), 0, {}))
~~~

Podemos ver que ambos sitios web hacen un buen trabajo al formatear el código:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_prettier_1.jpg)

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_beautifier_1.jpg)

Sin embargo, el código todavía no es muy fácil de leer. Esto se debe a que el código con el que estamos tratando no solo se minimizó sino que también se ofuscó. Por lo tanto, simplemente formatear o embellecer el código no será suficiente. Para eso, necesitaremos herramientas para desofuscar el código.
___

### **DESOFUSCACION**

Podemos encontrar muchas buenas herramientas en línea para desofuscar el código JavaScript y convertirlo en algo que podamos entender. Una buena herramienta es [JSNice](http://www.jsnice.org/). Intentemos copiar nuestro código ofuscado anterior y ejecutarlo en JSNice haciendo clic en el botón `Nicify JavaScript`.

>Tip: debemos hacer clic en el botón de opciones junto al botón "Nicificar JavaScript" y anular la selección de "Inferir tipos" para reducir el desorden del código con comentarios.

>Tip: Asegúrese de no dejar líneas vacías antes de la secuencia de comandos, ya que puede afectar el proceso de desofuscación y generar resultados inexactos.

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_jsnice_1.jpg)

Podemos ver que esta herramienta hace un trabajo mucho mejor al descifrar el código JavaScript y nos dio un resultado que podemos entender:

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

Como se mencionó anteriormente, el método de ofuscación utilizado anteriormente es el `empaquetado`. Otra forma de `descomprimir` dicho código es encontrar el valor de `return` al final y usar `console.log` para imprimirlo en lugar de ejecutarlo.
___

### ** INGENIERIA REVERSA**

Aunque estas herramientas están haciendo un buen trabajo hasta ahora al aclarar el código en algo que podamos entender, una vez que el código se vuelve más ofuscado y codificado, sería mucho más difícil para las herramientas automatizadas limpiarlo. Esto es especialmente cierto si el código se ofuscó con una herramienta de ofuscación personalizada.

Tendríamos que aplicar ingeniería inversa manualmente al código para comprender cómo se ofuscó y su funcionalidad para tales casos. Si está interesado en saber más sobre la ingeniería inversa y la desofuscación avanzada de JavaScript, puede consultar el módulo [Codificación segura 101](https://academy.hackthebox.com/module/details/38), que debería cubrir este tema en profundidad.
___

### **RETO**
Usando lo que aprendiste en esta sección, intenta desofuscar 'secret.js' para obtener el contenido de la bandera. ¿Qué es la bandera?

R: HTB{1_4m_7h3_53r14l_g3n3r470r!}

Codigo Ofuscado
~~~
eval(function (p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }('g 4(){0 5="6{7!}";0 1=8 a();0 2="/9.c";1.d("e",2,f);1.b(3)}', 17, 17, 'var|xhr|url|null|generateSerial|flag|HTB|1_4m_7h3_53r14l_g3n3r470r|new|serial|XMLHttpRequest|send|php|open|POST|true|function'.split('|'), 0, {}))
~~~

Desofuscado con [JSNice](http://www.jsnice.org/)
~~~
'use strict';
/**
 * @return {undefined}
 */
function generateSerial() {
  /** @type {string} */
  var flag = "HTB{1_4m_7h3_53r14l_g3n3r470r!}";
  /** @type {!XMLHttpRequest} */
  var xhr = new XMLHttpRequest;
  /** @type {string} */
  var url = "/serial.php";
  xhr.open("POST", url, true);
  xhr.send(null);
}
;
~~~