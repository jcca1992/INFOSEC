## DESCRIPTORES Y REDIRECCIONAMIENTO DE ARCHIVOS
___
### DESCRIPTOR DE ARCHIVOS

Un descriptor de archivo (FD) en los sistemas operativos Unix/Linux es un indicador de la conexión que mantiene el kernel para realizar operaciones de entrada/salida (I/O). En los sistemas operativos basados en Windows, se denomina identificador de archivo. Es la conexión (generalmente a un archivo) desde el Sistema Operativo para realizar operaciones de I/O (Input/Output de Bytes). De forma predeterminada, los primeros tres descriptores de archivos en Linux son:
1. Flujo de datos para entrada.
    + `STDIN – 0`
2. Flujo de datos para salida.
    + `STDOUT – 1`
3. Flujo de datos para la salida que se relaciona con la ocurrencia de un error. 
    + `STDERR – 2`
___

### STDIN Y STDOUT

Veamos un ejemplo con `cat`. Al ejecutar `cat`, le damos al programa en ejecución nuestra entrada estándar (`STDIN - FD 0`), marcada en `verde`, en este caso "ALGUNA ENTRADA". Tan pronto como hayamos confirmado nuestra entrada con `[ENTER]`, se devuelve al terminal como salida estándar (`STDOUT - FD 1`), marcada en rojo.

![](https://academy.hackthebox.com/storage/modules/18/find0.png)
___
### STDOUT Y STDERR

En el siguiente ejemplo, al usar el comando `find`, veremos la salida estándar (`STDOUT - FD 1`) marcada en verde y el error estándar (`STDERR - FD 2`) marcado en rojo.

~~~
Juceco@htb[/htb]$ find /etc/ -name shadow
~~~

![](https://academy.hackthebox.com/storage/modules/18/find1.png)

En este caso, el error se marca y se muestra con "`Permiso denegado`". Podemos verificar esto redirigiendo el descriptor de archivo para los errores (`FD 2 - STDERR`) a "`/dev/null`". De esta forma, redirigimos los errores resultantes al "null device", que descarta todos los datos.

~~~
Juceco@htb[/htb]$ find /etc/ -name shadow 2>/dev/null
~~~

![](https://academy.hackthebox.com/storage/modules/18/find2.png)
___
### REDIRIGIR STDOUT A UN ARCHIVO

Ahora podemos ver que todos los errores (`STDERR`) presentados anteriormente con "`Permiso denegado`" ya no se muestran. El único resultado que vemos ahora es la salida estándar (`STDOUT`), que también podemos redirigir a un archivo con el nombre `results.txt` que solo contendrá la salida estándar sin los errores estándar.

~~~
Juceco@htb[/htb]$ find /etc/ -name shadow 2>/dev/null > results.txt
~~~

![](https://academy.hackthebox.com/storage/modules/18/find3.png)
___
### REDIRIGIR STDOUT Y STDERR A ARCHIVOS SEPARADOS

Deberíamos haber notado que no usamos un número antes del signo mayor que (`>`) en el último ejemplo. Esto se debe a que antes redirigimos todos los errores estándar al "`null device`", y la única salida que obtenemos es la salida estándar (`FD 1 - STDOUT`). Para hacer esto más preciso, redirigiremos el error estándar (`FD 2 - STDERR`) y la salida estándar (`FD 1 - STDOUT`) a archivos diferentes.

~~~
Juceco@htb[/htb]$ find /etc/ -name shadow 2> stderr.txt 1> stdout.txt
~~~

![](https://academy.hackthebox.com/storage/modules/18/find4.png)

### REDIRIGIR STDIN

Como ya hemos visto, en combinación con los descriptores de archivo, podemos redirigir errores y generar resultados con un carácter mayor que (`>`). Esto también funciona con el signo inferior a (`<`). Sin embargo, el signo inferior a sirve como entrada estándar (`FD 0 - STDIN`). Estos caracteres se pueden ver como "`direction`" en forma de flecha que nos dice "`from where`" y "`where to`" se deben redirigir los datos. Usamos el comando `cat` para usar el contenido del archivo "`stdout.txt`" como `STDIN`.

~~~
Juceco@htb[/htb]$ cat < stdout.txt
~~~

![](https://academy.hackthebox.com/storage/modules/18/find5.png)
___
### REDIRIGIR STDOUT Y AGREGAR A UN ARCHIVO

Cuando usamos el signo mayor que (`>`) para redirigir nuestro `STDOUT`, se crea automáticamente un nuevo archivo si aún no existe. Si este archivo existe, se sobrescribirá sin pedir confirmación. Si queremos agregar `STDOUT` a nuestro archivo existente, podemos usar el signo doble mayor que (`>>`).

~~~
Juceco@htb[/htb]$ find /etc/ -name passwd >> stdout.txt 2>/dev/null
~~~

![](https://academy.hackthebox.com/storage/modules/18/find9.png)
___
### EDIRIGIR FLUJO STDIN A UN ARCHIVO

También podemos usar los caracteres dobles menor que (`<<`) para agregar nuestra entrada estándar a través de una transmisión. Podemos usar la llamada función `End-Of-File` (`EOF`) de un archivo del sistema Linux, que define el final de la entrada. En el siguiente ejemplo, usaremos el comando `cat` para leer nuestra entrada de transmisión a través de la transmisión y dirigirla a un archivo llamado "`stream.txt`".

~~~
Juceco@htb[/htb]$ cat << EOF > stream.txt
~~~

![](https://academy.hackthebox.com/storage/modules/18/find6.png)
___
### PIPES

Otra forma de redirigir `STDOUT` es usar conductos (`|`). Estos son útiles cuando queremos usar el `STDOUT` de un programa para ser procesado por otro. Una de las herramientas más utilizadas es `grep`, que usaremos en el siguiente ejemplo. Grep se usa para filtrar `STDOUT` según el patrón que definamos. En el siguiente ejemplo, usamos el comando `find` para buscar todos los archivos en el directorio "`/etc/`" con una extensión "`.conf`". Cualquier error se redirige al "`null device`" (`/dev/null`). Usando `grep`, filtramos los resultados y especificamos que solo se deben mostrar las líneas que contienen el patrón "`systemd`".

~~~
Juceco@htb[/htb]$ find /etc/ -name *.conf 2>/dev/null | grep systemd
~~~

![](https://academy.hackthebox.com/storage/modules/18/find7.png)

Las redirecciones funcionan, no solo una vez. Podemos utilizar los resultados obtenidos para redirigirlos a otro programa. Para el siguiente ejemplo, utilizaremos la herramienta llamada `wc`, que debe contar el número total de resultados obtenidos.

~~~
Juceco@htb[/htb]$ find /etc/ -name *.conf 2>/dev/null | grep systemd | wc -l
~~~

![](https://academy.hackthebox.com/storage/modules/18/find8.png)
___
### RETO

+ ¿Cuántos archivos existen en el sistema que tienen la extensión de archivo ".log"?

`R: 32` 

~~~
htb-student@nixfund:~$ find / -type f -name *.log 2>/dev/null | wc -l
32
~~~
___
+ ¿Cuántos paquetes en total están instalados en el sistema objetivo?

`R: 737`

~~~
htb-student@nixfund:~$ apt list --installed | grep -c "installed"
737
~~~

~~~
htb-student@nixfund:~$ dpkg -l | grep -c '^ii'
737
~~~
___
#### [Anterior (Buscar Archivos y Directorios)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Find-Files-Directories.md)
#### [Siguiente (Filtrar Contenidos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Filter-Contents.md)