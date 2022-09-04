## LINUX SECURITY
___

Todos los sistemas informáticos tienen un riesgo inherente de intrusión. Algunos presentan más riesgos que otros, como un servidor web con acceso a Internet que aloja múltiples aplicaciones web complejas. Los sistemas Linux también son menos propensos a los virus que afectan a los sistemas operativos Windows y no presentan una superficie de ataque tan grande como los hosts unidos a un dominio de Active Directory. De todos modos, es esencial contar con ciertos fundamentos para asegurar cualquier sistema Linux.

Una de las medidas de seguridad más importantes de los sistemas operativos Linux es mantener actualizados el sistema operativo y los paquetes instalados. Esto se puede lograr con un comando como:

~~~
Juceco@htb[/htb]$ apt update && apt dist-upgrade
~~~

Si las reglas del cortafuegos no están configuradas correctamente a nivel de red, podemos usar el cortafuegos de Linux y/o `iptables` para restringir el tráfico de entrada y salida del host.

Si SSH está abierto en el servidor, la configuración debe realizarse para no permitir el inicio de sesión con contraseña y no permitir que el usuario raíz inicie sesión a través de SSH. También es importante evitar iniciar sesión y administrar el sistema como usuario root siempre que sea posible y administrar adecuadamente el control de acceso. El acceso de los usuarios debe determinarse con base en el principio de privilegio mínimo. Por ejemplo, si un usuario necesita ejecutar un comando como root, ese comando debe especificarse en la configuración de `sudoers` en lugar de otorgarles todos los derechos de sudo. Otro mecanismo de protección común que se puede utilizar es `fail2ban`. Esta herramienta cuenta la cantidad de intentos fallidos de inicio de sesión y, si un usuario ha alcanzado la cantidad máxima, el host que intentó conectarse se manejará como está configurado.

También es importante auditar periódicamente el sistema para asegurarse de que no existan problemas que puedan facilitar la escalada de privilegios, como un kernel desactualizado, problemas de permisos de usuario, archivos world-writable y trabajos cron mal configurados o servicios mal configurados. Muchos administradores se olvidan de la posibilidad de que algunas versiones del kernel deban actualizarse manualmente.

Una opción para bloquear aún más los sistemas Linux es `Security-Enhanced Linux` (`SELinux`) o `AppArmor`. Este es un módulo de seguridad del núcleo que se puede usar para políticas de control de acceso de seguridad. En SELinux, cada proceso, archivo, directorio y objeto del sistema recibe una etiqueta. Las reglas de política se crean para controlar el acceso entre estos procesos y objetos etiquetados y el kernel las aplica. Esto significa que el acceso se puede configurar para controlar qué usuarios y aplicaciones pueden acceder a qué recursos. SELinux proporciona controles de acceso muy granulares, como especificar quién puede agregar a un archivo o moverlo.

Además, existen diferentes aplicaciones y servicios como [Snort](https://www.snort.org/), [chkrootkit](http://www.chkrootkit.org/), [rkhunter](https://packages.debian.org/sid/rkhunter), [Lynis](https://cisofy.com/lynis/) y otros que pueden contribuir a la seguridad de Linux. Además, se deben realizar algunas configuraciones de seguridad, tales como:

+ Eliminar o deshabilitar todos los servicios y software innecesarios
+ Eliminación de todos los servicios que dependen de mecanismos de autenticación no cifrados
+ Asegúrese de que NTP esté habilitado y que Syslog se esté ejecutando
+ Asegúrese de que cada usuario tenga su propia cuenta
+ Hacer cumplir el uso de contraseñas seguras
+ Configure la caducidad de la contraseña y restrinja el uso de contraseñas anteriores
+ Bloqueo de cuentas de usuario después de errores de inicio de sesión
+ Deshabilite todos los binarios SUID/SGID no deseados

Esta lista está incompleta, ya que la seguridad no es un producto sino un proceso. Esto significa que siempre se deben tomar medidas específicas para proteger mejor los sistemas, y depende de los administradores qué tan bien conocen sus sistemas operativos. Cuanto mejor estén familiarizados los administradores con el sistema y cuanto más capacitados estén, mejores y más seguras serán sus precauciones y medidas de seguridad.
___
#### [Anterior (Atajos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Shortcuts.md)