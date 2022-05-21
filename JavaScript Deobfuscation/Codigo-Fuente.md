## CODIGO FUENTE

La mayoría de los sitios web hoy en día utilizan JavaScript para realizar sus funciones. Mientras que `HTML` se usa para determinar los campos y parámetros principales del sitio web, y `CSS` se usa para determinar su diseño, `JavaScript` se usa para realizar las funciones necesarias para ejecutar el sitio web. Esto sucede en segundo plano, y solo vemos la bonita interfaz del sitio web e interactuamos con ella.

Aunque todo este código fuente está disponible en el lado del cliente, nuestros navegadores lo procesan, por lo que a menudo no prestamos atención al código fuente HTML. Sin embargo, si queremos comprender las funcionalidades del lado del cliente de una página determinada, generalmente comenzamos por echar un vistazo al código fuente de la página. Esta sección mostrará cómo podemos descubrir el código fuente que contiene todo esto y comprender su uso general.
___

### **HTML**

Comenzaremos iniciando el ejercicio a continuación, abra Firefox en nuestro PwnBox y visite la URL que se muestra en la pregunta:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_mainsite.jpg)

Como podemos ver, el sitio web dice `Secret Serial Generator`, sin tener ningún campo de entrada ni mostrar ninguna funcionalidad clara. Entonces, nuestro siguiente paso es alcanzar su código fuente. Podemos hacerlo presionando `[CTRL + U]`, lo que debería abrir la vista de código fuente del sitio web:

![](https://academy.hackthebox.com/storage/modules/41/js_deobf_mainsite_source_1.jpg)

Como podemos ver, podemos visualizar el código fuente HTML del sitio web.
___

### **CSS**

El código `CSS` se define `internamente` dentro del mismo archivo `HTML` entre los elementos `<style>` o se define externamente en un archivo `.css` separado y se hace referencia dentro del código `HTML`.

En este caso, vemos que el `CSS` está definido internamente, como se ve en el fragmento de código a continuación:

~~~
<style>
        *,
        html {
            margin: 0;
            padding: 0;
            border: 0;
        }
        ...SNIP...
        h1 {
            font-size: 144px;
        }
        p {
            font-size: 64px;
        }
    </style>
~~~

Si el estilo `CSS` de una página se define externamente, se hace referencia al archivo `.css` externo con la etiqueta `<link>` dentro del encabezado HTML, de la siguiente manera:

~~~
<head>
    <link rel="stylesheet" href="style.css">
</head>
~~~
___

### **JAVASCRIPT**

El mismo concepto se aplica a `JavaScript`. Puede escribirse internamente entre elementos `<script>` o escribirse en un archivo `.js` separado y referenciarse dentro del código `HTML`.

Podemos ver en nuestra fuente `HTML` que el archivo `.js` está referenciado externamente:

~~~
<script src="secret.js"></script>
~~~

Podemos consultar el script haciendo clic en `secret.js`, que debería llevarnos directamente al script. Cuando lo visitamos, vemos que el código es muy complicado y no se puede comprender:

~~~
eval(function (p, a, c, k, e, d) { e = function (c) { '...SNIP... |true|function'.split('|'), 0, {}))
~~~

La razón detrás de esto es la ofuscación del código. ¿Qué es? ¿Cómo se hace? ¿Dónde se usa?
___

### **RETO**

Repite lo que aprendiste en esta sección, y deberías encontrar una bandera secreta, ¿cuál es?

`R:` HTB{4lw4y5_r34d_7h3_50urc3}

~~~
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
    <script src="secret.js"></script>
    <!-- HTB{4lw4y5_r34d_7h3_50urc3} -->
~~~