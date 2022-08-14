## GESTION DE USUARIO
___
La gestión de usuarios es una parte esencial de la administración de Linux. A veces necesitamos crear nuevos usuarios o agregar otros usuarios a grupos específicos. Otra posibilidad es ejecutar comandos como un usuario diferente. Después de todo, no es muy raro que los usuarios de un solo grupo específico tengan permisos para ver o editar archivos o directorios específicos. Esto, a su vez, nos permite recopilar más información localmente en la máquina, lo que puede ser muy importante. Echemos un vistazo al siguiente ejemplo de cómo ejecutar código como un usuario diferente.

Ejecucion como usuario

~~~
Juceco@htb[/htb]$ cat /etc/shadow

cat: /etc/shadow: Permission denied
~~~

Ejecucion como Root

~~~
Juceco@htb[/htb]$ sudo cat /etc/shadow

root:<SNIP>:18395:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
<SNIP>
~~~

Aquí hay una lista que nos ayudará a comprender mejor y manejar la administración de usuarios.

|Comando| Descripcion|
|--|--|
|`sudo`| Ejecutar comando como un usuario diferente|
|`su`|La utilidad `su` solicita las credenciales de usuario adecuadas a través de PAM y cambia a esa ID de usuario (el usuario predeterminado es el superusuario). Luego se ejecuta un shell.|
|`useradd`|Crea un nuevo usuario o actualiza la información predeterminada del nuevo usuario.|
|`userdel`|Elimina una cuenta de usuario y archivos relacionados.|
|`usermod`|Modifica una cuenta de usuario.|
|`addgroup`|Agrega un grupo al sistema.|
|`delgroup`|Elimina un grupo al sistema.|
|`passwd`|Cambia la contraseña de usuario.|

La administración de usuarios es esencial en cualquier sistema operativo, y la mejor manera de familiarizarse con ella es probar los comandos individuales junto con sus diversas opciones.
___
### RETO

+ ¿Qué opción debe configurarse para crear un directorio de inicio para un nuevo usuario usando el comando "useradd"?

`R: -m`

~~~
┌─[juliocesar@PCnotebook]─[~]
└──╼ $ useradd -m
~~~

+ ¿Qué opción se debe configurar para bloquear una cuenta de usuario usando el comando "usermod"? (versión larga de la opción)

`R: --lock`

~~~
┌─[juliocesar@PCnotebook]─[~]
└──╼ $ usermod --lock
~~~

+ ¿Qué opción debe configurarse para ejecutar un comando como un usuario diferente usando el comando "su"? (versión larga de la opción)

`R: --command`
___
#### [Anterior (Informacion del Sistema)]()
#### [Siguiente (Administracion de Paquete)]()