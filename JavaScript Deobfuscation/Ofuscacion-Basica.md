## OFUSCACION BASICA

La ofuscación de código generalmente no se realiza manualmente, ya que existen muchas herramientas para varios lenguajes que realizan ofuscación de código automatizada. Se pueden encontrar muchas herramientas en línea para hacerlo, aunque muchos actores maliciosos y desarrolladores profesionales desarrollan sus propias herramientas de ofuscación para dificultar la desofuscación.
___

### **EJECUTANDO CODIGO JAVASCRIPT**

Tomemos la siguiente línea de código como ejemplo e intentemos ofuscarla:

~~~
console.log('HTB JavaScript Deobfuscation Module');
~~~

Primero, probemos ejecutar este código en texto sin cifrar, para verlo funcionar en acción. Podemos ir a JSConsole, pegar el código y presionar enter, y ver su salida:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_jsconsole_1_1.jpg)

Vemos que esta línea de código imprime `HTB JavaScript Deofuscation Module`, que se realiza mediante la función `console.log`.
___

### **MINIMIZAR CODIGO JS**

Una forma común de reducir la legibilidad de un fragmento de código JavaScript mientras se mantiene completamente funcional es la minificación de JavaScript. `Minificar el código` significa tener todo el código en una sola línea (a menudo muy larga). Minificarlo es útil para códigos más largos, ya que si nuestro código solo constara de una sola línea, no se vería muy diferente cuando se minificara.

Muchas herramientas pueden ayudarnos a minimizar el código JavaScript, como [javascript-minifier](https://javascript-minifier.com/). Simplemente copiamos nuestro código y hacemos clic en Minificar, y obtenemos el resultado minimizado a la derecha:

![](https://academy.hackthebox.com/storage/modules/41/js_minify_1.jpg)

Una vez más, podemos copiar el código minificado a [JSConsole](https://jsconsole.com/) y ejecutarlo, y vemos que se ejecuta como se esperaba. Por lo general, el código JavaScript minificado se guarda con la extensión `.min.js.`

>Nota: La minificación de código no es exclusiva de JavaScript y se puede aplicar a muchos otros lenguajes, como se puede ver en [javascript-minifier](https://javascript-minifier.com/).
___

### **EMPAQUETAR CODIGO JS**

Ahora, ofusquemos nuestra línea de código para que sea más oscura y difícil de leer. Primero, probaremos [BeautifyTools](http://beautifytools.com/javascript-obfuscator.php) para ofuscar nuestro código:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_obfuscator.jpg)

~~~
eval(function(p,a,c,k,e,d){e=function(c){return c};if(!''.replace(/^/,String)){while(c--){d[c]=k[c]||c}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('5.4(\'3 2 1 0\');',6,6,'Module|Deobfuscation|JavaScript|HTB|log|console'.split('|'),0,{}))
~~

Vemos que nuestro código se volvió mucho más ofuscado y difícil de leer. Podemos copiar este código en [jsconsole](https://jsconsole.com/), para verificar que aún cumple su función principal:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_jsconsole_3_1.jpg)

Vemos que obtenemos la misma salida.

>Nota: el tipo de ofuscación anterior se conoce como "empaquetamiento", que generalmente se reconoce a partir de los seis argumentos de función utilizados en la función inicial "función (p, a, c, k, e, d)".

Una herramienta de ofuscación de `empaquetado` generalmente intenta convertir todas las palabras y símbolos del código en una lista o diccionario y luego referirse a ellos usando la función `(p,a,c,k,e,d)` para reconstruir el código original durante ejecución. El `(p,a,c,k,e,d)` puede ser diferente de un empacador a otro. Sin embargo, suele contener un cierto orden en el que se empaquetan las palabras y símbolos del código original para saber cómo ordenarlos durante la ejecución.

Si bien un empaquetador hace un gran trabajo al reducir la legibilidad del código, aún podemos ver sus cadenas principales escritas en texto no cifrado, lo que puede revelar parte de su funcionalidad. Esta es la razón por la que es posible que deseemos buscar mejores formas de ofuscar nuestro código.