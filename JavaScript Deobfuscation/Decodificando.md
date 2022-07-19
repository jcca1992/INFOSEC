## DECODIFICANDO

Después de hacer el ejercicio de la sección anterior, obtuvimos un extraño bloque de texto que parece estar codificado:

~~~
curl http:/SERVER_IP:PORT/serial.php -X POST -d "param1=sample"

ZG8gdGhlIGV4ZXJjaXNlLCBkb24ndCBjb3B5IGFuZCBwYXN0ZSA7KQo=
~~~

Este es otro aspecto importante de la ofuscación al que nos referimos en `Más ofuscación` en la sección `Ofuscación avanzada`. Muchas técnicas pueden ofuscar aún más el código y hacerlo menos legible para los humanos y menos detectable para los sistemas. Por esa razón, muy a menudo encontrará código ofuscado que contiene bloques de texto codificados que se decodifican al ejecutarse. Cubriremos 3 de los métodos de codificación de texto más utilizados:

+ base64
+ hex
+ rot13
___

### **BASE64**

La codificación `base64` generalmente se usa para reducir el uso de caracteres especiales, ya que cualquier carácter codificado en `base64` se representaría en caracteres alfanuméricos, además de + y / solamente. Independientemente de la entrada, incluso si está en formato binario, solo los usaria la cadena codificada en base64 resultante.

#### *DECTECTANDO BASE64*

Las cadenas codificadas en `base64` se detectan fácilmente ya que solo contienen caracteres alfanuméricos. Sin embargo, la característica más distintiva de `base64` es su relleno con caracteres `=`. La longitud de las cadenas codificadas en `base64` debe ser un múltiplo de 4. Si la salida resultante tiene solo 3 caracteres, por ejemplo, se agrega un `=` adicional como relleno, y así sucesivamente.

#### *CODIFICAR BASE64*

Para codificar cualquier texto en `base64` en Linux, podemos repetirlo y canalizarlo con `'|'` a `base64`:
~~~
Juceco@htb[/htb]$ echo https://www.hackthebox.eu/ | base64

aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K
~~~

#### *DECODIFICAR BASE64*

Si queremos decodificar cualquier cadena codificada en `base64`, podemos usar `base64 -d`, de la siguiente manera:
~~~
Juceco@htb[/htb]$ echo https://www.hackthebox.eu/ | base64

aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K
~~~
___

### **HEX**

Otro método de codificación común es la codificación hexadecimal, que codifica cada carácter en su orden hexadecimal en la tabla `ASCII`. Por ejemplo, `a` es `61` en `hex`, `b` es `62`, `c` es `63`, etc. Puede encontrar la tabla `ASCII` completa en Linux usando el comando `man ascii`.

#### *DECTECTANDO HEX*

Cualquier cadena codificada en `hex` estaría compuesta solo por caracteres hexadecimales, que son solo 16 caracteres: 0-9 y a-f. Eso hace que detectar cadenas codificadas en `hex` sea tan fácil como detectar cadenas codificadas en `base64`.

#### *CODIFICAR HEX*

Para codificar cualquier cadena en hex en Linux, podemos usar el comando `xxd -p`:

~~~
Juceco@htb[/htb]$ echo https://www.hackthebox.eu/ | xxd -p

68747470733a2f2f7777772e6861636b746865626f782e65752f0a
~~~

#### *DECODIFICAR HEX*

Para decodificar una cadena codificada en `hex`, podemos usar el comando `xxd -p -r`:

~~~
Juceco@htb[/htb]$ echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r

https://www.hackthebox.eu/
~~~
___

### **CAESAR/ROT13**

Otra técnica de codificación común, y muy antigua, es el cifrado Caesar, que cambia cada letra por un número fijo. Por ejemplo, el cambio de 1 carácter hace que `a` se convierta en `b`, y `b` se convierta en `c`, y así sucesivamente. Muchas variaciones del cifrado Caesar usan un número diferente de cambios, el más común de los cuales es `rot13`, que cambia cada carácter 13 veces hacia adelante.

#### *DETECTANDO CAESAR/ROT13*

Aunque este método de codificación hace que cualquier texto parezca aleatorio, aún es posible detectarlo porque cada carácter se asigna a un carácter específico. Por ejemplo, en `rot13`,` http://www` se convierte en `uggc://jjj`, que todavía tiene algunas similitudes y puede reconocerse como tal.

#### *CODIFICAR ROT13*

No hay un comando específico en Linux para realizar la codificación `rot13`. Sin embargo, es bastante fácil crear nuestro propio comando para cambiar el carácter:

~~~
Juceco@htb[/htb]$ echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'

uggcf://jjj.unpxgurobk.rh/
~~~

#### *CODIFICAR ROT13*

También podemos usar el mismo comando anterior para decodificar rot13:
~~~
Juceco@htb[/htb]$ echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'

https://www.hackthebox.eu/
~~~
Otra opción para codificar/decodificar rot13 sería usar una herramienta en línea, como [rot13](https://rot13.com/).
___

### **OTROS TIPOS DE CODIFICACIONES**

Hay cientos de otros métodos de codificación que podemos encontrar en línea. Aunque estos son los más comunes, en ocasiones nos encontraremos con otros métodos de codificación, que pueden requerir algo de experiencia para identificarlos y decodificarlos.

`Si se enfrenta a tipos de codificación similares, primero intente determinar el tipo de codificación y luego busque herramientas en línea para decodificarlo.`

Algunas herramientas pueden ayudarnos a determinar automáticamente el tipo de codificación, como [Cipher Identifier](https://www.boxentriq.com/code-breaking/cipher-identifier). Pruebe las cadenas codificadas anteriores con Cipher Identifier, para ver si puede identificar correctamente el método de codificación.

Además de la codificación, muchas herramientas de ofuscación utilizan el cifrado, que consiste en codificar una cadena con una clave, lo que puede hacer que el código ofuscado sea muy difícil de aplicar ingeniería inversa y desofuscar, especialmente si la clave de descifrado no está almacenada dentro del propio script.
___

### RETO

Usando lo que aprendiste en esta sección, determina el tipo de codificación usada en la cadena que obtuviste en el ejercicio anterior y decodifícala. Para obtener el indicador, puede enviar una solicitud 'POST' a 'serial.php' y configurar los datos como "serial=YOUR_DECODED_OUTPUT".

`R: HTB{ju57_4n07h3r_r4nd0m_53r14l}`

Como es costumbre vamos a revisar por encima la pagina
~~~
┌──(root㉿kali)-[/home/kali]
└─# curl http://104.248.173.13:31030                        
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
    <script src="secret.js"></script>
    <!-- HTB{4lw4y5_r34d_7h3_50urc3} -->
</head>

<body>
    <div class="center">
        <h1>Secret Serial Generator</h1>
        <p>This page generates secret serials!</p>
    </div>
</body>

</html>
~~~

Ahora repetimos el procedimiento que vimos en el ejercicio de [HTTP-Request](https://github.com/jcca1992/INFOSEC/blob/HackTheBox/JavaScript%20Deobfuscation/HTTP-Request.md) para obtener la bandera codificada

~~~                                                                                 
┌──(root㉿kali)-[/home/kali]
└─# curl http://104.248.173.13:31030/serial.php -X POST -d "param1=sample"
N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz
~~~

Analizando la codificacion podemos presumir que es con `Rot13` asi que lo vamos a decodificar
~~~
┌──(root㉿kali)-[/home/kali]
└─# echo N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz | tr 'A-Za-z' 'N-ZA-Mn-za-m'    
A2tkAI8kAI9uK3ZmL3VmA19gZmH1ATpm
~~~

Al tratar de decodificar nos arroja un codigo igualmente codificado, por lo que no seria `Rot13`, asi que probamos con el otro metodo de codificacion mas parecido que seria `Base64`
~~~                                                                                 
┌──(root㉿kali)-[/home/kali]
└─# echo N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz | base64 -d                 
7h15_15_a_s3cr37_m3554g3
~~~

Como vemos el codigo fue decodificado con `base64`, ahora como nos instruye el enunciado enviamos la informacion con POST para que nos arroje la bandera, hay que tener mucho cuidado con errores de tipeo ya que al haberlo no nos arrojara la bandera
~~~
┌──(root㉿kali)-[/home/kali]
└─# curl http://104.248.173.13:31030/serial.php -X POST -d "serial=7h15_15_a_s3cr37_m3554g3" 
HTB{ju57_4n07h3r_r4nd0m_53r14l}
~~~