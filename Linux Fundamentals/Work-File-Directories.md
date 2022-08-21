## TRABAJANDO CON ARCHIVOS Y DIRECTORIOS
___
Crear, mover y copiar

A continuación, trabajemos con archivos y directorios y aprendamos a crear, renombrar, mover, copiar y eliminar. Primero, creemos un archivo vacío y un directorio. Podemos usar touch para crear un archivo vacío y mkdir para crear un directorio.

La sintaxis para esto es la siguiente:

`Touch`
~~~
Juceco@htb[/htb]$ touch <name>
~~~

`mkdir`
~~~
Juceco@htb[/htb]$ mkdir <name>
~~~

En este ejemplo, llamamos al archivo info.txt y el directorio Storage. Para crearlos, seguimos los comandos y su sintaxis que se muestran arriba.

`Crear un archivo vacio`
~~~
Juceco@htb[/htb]$ touch info.txt
~~~

`Crear un directorio`
~~~
Juceco@htb[/htb]$ mkdir Storage
~~~

Es posible que queramos tener directorios específicos en el directorio, y llevaría mucho tiempo crear este comando para cada directorio. El comando `mkdir` tiene una opción con el indicador `-p` para agregar directorios principales.

~~~
Juceco@htb[/htb]$ mkdir -p Storage/local/user/documents
~~~

Podemos ver toda la estructura después de crear los directorios principales con la herramienta `tree`.

~~~
Juceco@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            └── documents

4 directories, 1 file
~~~

También podemos crear archivos directamente en los directorios especificando la ruta donde se debe almacenar el archivo. El truco es usar el punto único (.) para decirle al sistema que queremos comenzar desde el directorio actual. Entonces, el comando para crear otro archivo vacío se ve así:


___
#### [Anterior (Navegando)]()
#### [Siguiente (Editando Archivos)]()