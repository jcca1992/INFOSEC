## GESTION DE PERMISOS
___
### VISION GENERAL

En Linux, los permisos se asignan a usuarios y grupos. Cada usuario puede ser miembro de diferentes grupos, y la pertenencia a estos grupos otorga al usuario permisos adicionales específicos. Cada archivo y directorio pertenece a un usuario específico y a un grupo específico. Por lo tanto, los permisos para usuarios y grupos que definieron un archivo también se definen para los respectivos propietarios. Cuando creamos nuevos archivos o directorios, estos pertenecen al grupo al que pertenecemos y a nosotros. Todo el sistema de permisos en los sistemas Linux se basa en el sistema de números octales y, básicamente, hay tres tipos diferentes de permisos que se pueden asignar a un archivo o directorio:
+ (`r`) - Read (Leer)
+ (`w`) - Write (Escribir)
+ (`x`) - Execute (Ejecutar)

Los permisos se pueden configurar para el propietario, el grupo y otros como se presenta en el siguiente ejemplo con sus permisos correspondientes.

~~~
Juceco@htb[/htb]$ ls -l /etc/passwd

- rwx rw- r--   1 root root 1641 May  4 23:42 /etc/passwd
- --- --- ---   |  |    |    |   |__________|
|  |   |   |    |  |    |    |        |_ Date
|  |   |   |    |  |    |    |__________ File Size
|  |   |   |    |  |    |_______________ Group
|  |   |   |    |  |____________________ User
|  |   |   |    |_______________________ Number of hard links
|  |   |   |_ Permission of others (read)
|  |   |_____ Permissions of the group (read, write)
|  |_________ Permissions of the owner (read, write, execute)
|____________ File type (- = File, d = Directory, l = Link, ... )
~~~
___
### CAMBIO DE PERMISOS

Podemos modificar los permisos usando el comando `chmod`, las referencias de grupos de permisos (`u` - propietario, `g` - Grupo, `o` - otros, `a` - Todos los usuarios) y un [`+`] o un [`-`] para agregar eliminar los permisos designados. En el siguiente ejemplo, un usuario crea un nuevo script de shell propiedad de ese usuario, no ejecutable y configurado con permisos de lectura/escritura para todos los usuarios.

~~~
cry0l1t3@htb[/htb]$ ls -l shell

-rwxr-x--x   1 cry0l1t3 htbteam 0 May  4 22:12 shell
~~~

Luego podemos aplicar permisos de `lectura` para todos los usuarios y ver el resultado.

~~~
cry0l1t3@htb[/htb]$ chmod a+r shell && ls -l shell

-rwxr-xr-x   1 cry0l1t3 htbteam 0 May  4 22:12 shell
~~~

También podemos configurar los permisos para que todos los demás usuarios `lean` solo usando la asignación de valor octal.

~~~
cry0l1t3@htb[/htb]$ chmod 754 shell && ls -l shell

-rwxr-xr--   1 cry0l1t3 htbteam 0 May  4 22:12 shell
~~~

Veamos todas las representaciones asociadas a él para comprender mejor cómo se calcula la asignación de permisos.

~~~
Binary Notation:                4 2 1  |  4 2 1  |  4 2 1
----------------------------------------------------------
Binary Representation:          1 1 1  |  1 0 1  |  1 0 0
----------------------------------------------------------
Octal Value:                      7    |    5    |    4
----------------------------------------------------------
Permission Representation:      r w x  |  r - x  |  r - -
~~~

Si sumamos los bits establecidos de la `representación binaria` asignados a los valores de la `notación binaria`, obtenemos el valor octal. `La Representación de permisos` representa los bits establecidos en la `Representación binaria` mediante el uso de los tres caracteres, lo que solo reconoce los permisos establecidos más fácilmente.
___
### CAMBIO DE PROPIEYARIO

Para cambiar el propietario y/o las asignaciones de grupo de un archivo o directorio, podemos usar el comando `chown`. La sintaxis es como la siguiente:

**Sintaxis chown**
~~~
cry0l1t3@htb[/htb]$ chown <user>:<group> <file/directory>
~~~

En este ejemplo, "shell" se puede reemplazar con cualquier archivo o carpeta arbitraria.

~~~
cry0l1t3@htb[/htb]$ chown root:root shell && ls -l shell

-rwxr-xr--   1 root root 0 May  4 22:12 shell
~~~
___
SUID Y GUID

Además de asignar permisos directos de usuario y grupo, también podemos configurar permisos especiales para archivos configurando los bits `Set User ID` (`SUID`) y `Set Group ID` (`GUID`). Estos bits `SUID`/`GUID` permiten, por ejemplo, que los usuarios ejecuten programas con los derechos de otro usuario. Los administradores a menudo usan esto para otorgar a sus usuarios derechos especiales para ciertas aplicaciones o archivos. Se utiliza la letra "`s`" en lugar de una "`x`". Al ejecutar un programa de este tipo, se utiliza el `SUID`/`GUID` del propietario del archivo.

Suele ocurrir que los administradores no están familiarizados con las aplicaciones, pero aun así asignan los bits `SUID`/`GUID`, lo que genera un alto riesgo de seguridad. Dichos programas pueden contener funciones que permitan la ejecución de un shell desde el `pager`, como la aplicación "`journalctl`".

Si el administrador establece el bit `SUID` en "`journalctl`", cualquier usuario con acceso a esta aplicación podría ejecutar un shell como root. Puede encontrar más información sobre esta y otras aplicaciones similares en [GTFObins](https://gtfobins.github.io/gtfobins/journalctl/).
___
#### [Anterior (Filtro de Contenido)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Filter-Contents.md)
#### [Siguiente (Atajos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Shortcuts.md)