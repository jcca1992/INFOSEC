## DESCRIPCION DEL PROMPT

El prompt de bash es fácil de entender y, de forma predeterminada, incluye información como el usuario, el nombre de host y el directorio de trabajo actual. El formato puede ser algo como esto:

~~~
<username>@<hostname><current working directory>$
~~~

El directorio de inicio de un usuario está marcado con una virgulilla <`~`> y es la carpeta predeterminada cuando iniciamos sesión.

~~~
<username>@<hostname>[~]$
~~~

El signo de dólar, en este caso, representa a un usuario. Tan pronto como iniciamos sesión como root, el carácter cambia a un hash <`#`> y se ve así:

~~~
root@htb[/htb]#
~~~

Vemos aquí lo mismo que cuando trabajamos en la GUI de Windows. Estamos registrados como un usuario en una computadora con un nombre específico, y sabemos en qué directorio estamos cuando navegamos a través de nuestro sistema. El prompt de Bash también se puede personalizar y cambiar según nuestras propias necesidades. El ajuste del prompt de bash está fuera del alcance de este módulo. Sin embargo, podemos mirar [bashrcgenerator](http://bashrcgenerator.com/) y [powerline](https://github.com/powerline/powerline), lo que nos da la posibilidad de adaptar nuestro prompt a nuestras necesidades.
___
#### [Anterior (Introduccion a Shell)]()
#### [Siguiente (Obteniendo Ayuda)]()
