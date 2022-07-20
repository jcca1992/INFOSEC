## **EVALUACION DE HABILIDADES**

Durante nuestra prueba de penetración, encontramos un servidor web que contiene JavaScript y API. Necesitamos determinar su funcionalidad para comprender cómo puede afectar negativamente a nuestro cliente.

+ Trate de estudiar el código HTML de la página web e identifique el código JavaScript usado dentro de él. ¿Cuál es el nombre del archivo JavaScript que se está utilizando?

`R:api.min.js`
~~~
┌──(root㉿kali)-[/home/kali]
└─# curl http://159.65.59.5:32222
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
        }

        html {
            width: 100%;
            height: 100%;
        }

        body {
            width: 100%;
            height: 100%;
            position: relative;
            background-color: #6fb3eb;
        }

        .center {
            width: 100%;
            height: 50%;
            margin: 0;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            font-family: "Helvetica", Helvetica, sans-serif;
            text-align: center;
        }

        h1 {
            font-size: 144px;
        }

        p {
            font-size: 64px;
        }
    </style>
    <script src="api.min.js"></script>
</head>

<body>
    <div class="center">
        <h1>API Keys</h1>
        <p>API Keys control panel</p>
    </div>
</body>

</html>
~~~

+ Una vez que encuentre el código JavaScript, intente ejecutarlo para ver si realiza alguna función interesante. ¿Obtuviste algo a cambio?

`R: HTB{j4v45cr1p7_3num3r4710n_15_k3y}`

En el navegador ingresamos a la URL y vemos el codigo fuente con `CTRL+U` o podemos ingresar a las herramientas de desarrollador directamente en la pagina con `CTRL+SHIFT+I` en la pestaña `Debugger` vemos el `api` ingresamos y vemos lo siguiente
~~~
eval(function (p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }('t 5(){6 7=\'1{n\'+\'8\'+\'9\'+\'a\'+\'b\'+\'c!\'+\'}\',0=d e(),2=\'/4\'+\'.g\';0[\'f\'](\'i\',2,!![]),0[\'k\'](l)}m[\'o\'](\'1{j\'+\'p\'+\'q\'+\'r\'+\'s\'+\'h\'+\'3}\');', 30, 30, 'xhr|HTB|_0x437f8b|k3y|keys|apiKeys|var|flag|3v3r_|run_0|bfu5c|473d_|c0d3|new|XMLHttpRequest|open|php|n_15_|POST||send|null|console||log|4v45c|r1p7_|3num3|r4710|function'.split('|'), 0, {}))
~~~

Podemos darle formato al codigo para hacerlo un poco mas legible presionando `{}` es indiferente porque ya podemos ver que el codigo esta ofuscado
~~~
eval(function (p, a, c, k, e, d) {
  e = function (c) {
    return c.toString(36)
  };
  if (!''.replace(/^/, String)) {
    while (c--) {
      d[c.toString(a)] = k[c] || c.toString(a)
    }
    k = [
      function (e) {
        return d[e]
      }
    ];
    e = function () {
      return '\\w+'
    };
    c = 1
  };
  while (c--) {
    if (k[c]) {
      p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c])
    }
  }
  return p
}('t 5(){6 7=\'1{n\'+\'8\'+\'9\'+\'a\'+\'b\'+\'c!\'+\'}\',0=d e(),2=\'/4\'+\'.g\';0[\'f\'](\'i\',2,!![]),0[\'k\'](l)}m[\'o\'](\'1{j\'+\'p\'+\'q\'+\'r\'+\'s\'+\'h\'+\'3}\');', 30, 30, 'xhr|HTB|_0x437f8b|k3y|keys|apiKeys|var|flag|3v3r_|run_0|bfu5c|473d_|c0d3|new|XMLHttpRequest|open|php|n_15_|POST||send|null|console||log|4v45c|r1p7_|3num3|r4710|function'.split('|'), 0, {
}))
~~~

Vamos a deofuscar el codigo con [JSNice](http://www.jsnice.org/) y vemos que tenemos console log y la flag separada por caracteres especiales
~~~
'use strict';
function apiKeys() {
  var flag = "HTB{n" + "3v3r_" + "run_0" + "bfu5c" + "473d_" + "c0d3!" + "}";
  var xhr = new XMLHttpRequest;
  var url = "/keys" + ".php";
  xhr["open"]("POST", url, !![]);
  xhr["send"](null);
}
console["log"]("HTB{j" + "4v45c" + "r1p7_" + "3num3" + "r4710" + "n_15_" + "k3y}");
~~~

+ Como habrás notado, el código JavaScript está ofuscado. Intente aplicar las habilidades que aprendió en este módulo para desofuscar el código y recuperar la variable 'bandera'.
`R: HTB{n3v3r_run_0bfu5c473d_c0d3!}`

asi como hicimos con la informacion en consolelog eliminamos los caracteres especiales como las comillas `""` y los signos `+`

+ Intente analizar el código JavaScript desofuscado y comprenda su funcionalidad principal. Una vez que lo haga, intente replicar lo que está haciendo para obtener una clave secreta. ¿Cuál es la clave?

`R:4150495f70336e5f37333537316e365f31355f66756e`
vemos la variable `var url = "/keys" + ".php"` por lo que desofuscado quedaria como `var url = /keys.php` y asi como en los ejercicios anteriores enviamos un parametro con `POST` y nos muestra el codigo
~~~
┌──(root㉿kali)-[/home/kali]
└─# curl http://159.65.59.5:32758/keys.php -X POST -d "param1=sample"
4150495f70336e5f37333537316e365f31355f66756e
~~~

+ Una vez que tenga la clave secreta, intente decidir su método de codificación y decodificarlo. A continuación, envíe una solicitud 'POST' a la misma página anterior con la clave decodificada como "key=DECODED_KEY". ¿Cuál es la bandera que tienes?

`R: HTB{r34dy_70_h4ck_my_w4y_1n_2_HTB}`

Podemos deducir que el codigo que muestra esta codificado en hex, asi que lo decodificaremos segun lo aprenido
~~~
┌──(root㉿kali)-[/home/kali]
└─# echo 4150495f70336e5f37333537316e365f31355f66756e | xxd -p -r 
API_p3n_73571n6_15_fun
~~~

Ahora enviaremos los datos decodificados segun el enunicado para obtener la bandera
~~~
┌──(root㉿kali)-[/home/kali]
└─# curl http://159.65.59.5:32758/keys.php -X POST -d "key=API_p3n_73571n6_15_fun"
HTB{r34dy_70_h4ck_my_w4y_1n_2_HTB}
~~~