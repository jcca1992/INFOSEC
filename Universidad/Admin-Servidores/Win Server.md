## Windows Server

Configuraremos 

___
## ACTIVE DIRECTORY

Active Directory o Directorio Activo son los términos que utiliza Microsoft para referirse a su  implementación de servicio de directorio en una red distribuida de computadores. Utiliza distintos protocolos, principalmente LDAP, DNS, DHCP y Kerberos, basicamente es la gestion de usuarios del servidor de windows

Componente de la estructura logica del Active Directory

+ Objetos
+ Unidad Organizativa
+ Dominios
+ Arboles
+ Bosque

![Diagrama-AD.jpg]
___
![Diagrama-AD-2.jpg]

Objetos: son los componentes basicos de la estructura logica, por ejemplo el usuario, la pc, la impresora

![Ojetos]

Unidad organizativa: Son contenedores, que se usan para organizar objetos con propositos administrativos, por ejemplo, dividir la empresa en departamentos, Contabilidad, Finanzas, Recursos Humanos, entre otro. Podriamos tener administradores para cada Unidad Organizativa.

![Uniad-Organizativa]

Dominios: son colecciones de los objetos administrativos definidos que comparten en una base de datos común del directorio políticas de seguridad y relaciones de confianza con otros dominios. Los dominios son las unidades funcionales clave de la estructura lógica de Active Directory.

Arbol de dominios: Son dominios agrupados en estructuras jerarquicas, cuando se agrega un segundo dominio a un arbol se convierte en hijo del dominio raiz, tambien llamado dominio padre. El dominio hijo a su vez puede tener otros subdominios combinando los nombres, por ejemplo "ventas.empresa.com" "ventas" seria el subdominio o dominio hijo de "empresa.com"

Bosque: Conjunto de uno o varios dominios que comparten una estructura logica, una configuracion de directorio y un catalogo global.

![Bosque]

## Configurar Windows 2012 R2

Una ves iniciado windows server en VirtualBox presionamos Windows+R y buscamos CMD para que nos habra la linea de comandos de windows

![CMD]

Vamos a anotar los siguientes datos

Direccion IPv4: 192.168.0.5 
Mascara de subred: 255.255.255.0
Gateway (puerta de enlace predeterminada): 192.168.0.1
Servidor DNS: 190.160.0.15

para comprobar conexion a internet podemos ejecutar un `ping` a `8.8.8.8` nos deberia arrojar como respuesta lo siguiente

~~~
C:\Users\estudiante>ping 8.8.8.8

Haciendo ping a 8.8.8.8 con 32 bytes de datos:
Respuesta desde 8.8.8.8: bytes=32 tiempo=256msTTL=58
Respuesta desde 8.8.8.8: bytes=32 tiempo=60ms TTL=58
Respuesta desde 8.8.8.8: bytes=32 tiempo=16ms TTL=58
Respuesta desde 8.8.8.8: bytes=32 tiempo=23ms TTL=58

Estadísticas de ping para 8.8.8.8:
Paquetes: enviados = 4, recibidos = 4, perdidos = 0 
    (0% perdidos),
Tiempos aproximados de ida y vuelta en milisegundos:
    Mínimo = 16ms, Máximo = 256ms, Media = 88ms
~~~

Cuando iniciamos sesion en windows server la primera ventana que se abre es la del `Administrador del Servidor`, vamos a presionar en `Administrar` y `Agregar Roles y Caracteristicas`

![AdmServ-Administrar]

En la seccion de `Antes de comenzar` nos muestra un breve resumen de lo que se puede hacer presionamos `Siguiente` 

![AntesDeComenzar]

En Tipo de instalacion dejamos seleccionado `Instalacion basada en caracteristicas o en roles` y presionamos `Siguiente`

![TipoDeInstalacion]

En Seleccion de Servidor dejamos la configuracion como esta y presionamos `Siguiente`

![SeleccionDeServidor]

A continuacion vamos a agregar los roles primero seleccionamos `Servicios de dominio de Active Directory` nos abre otra ventana y presionamos `Agregar Caracteristicas`

![RolesDeServidorAD]

![RolesDeServidorADAgregar]

Despues agregamos el rol de `Servidor DNS` de la misma forma y presionamos `Siguiente`

![RolesDeServidorDNS](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

![RolesDeServidorDNSAgregar](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

Posiblemente nos muestre la siguiente alerta, esto pasa cuando no se ha deshabilitado el DHCP y dejado una IP estatica, presionamos `Continuar` y despues `Siguiente`

![RolesDeServidorAlerta](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

# En las caracteristicas queda pendiente ya que no recuerdo que procede

![Caracteristicas](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

En las ventanas AD DS y Servidor DNS Presionamos siguiente

![AD-DS](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)
![ServidorDNS](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

En la ventana de Confirmacion, Destildamos la opcion de `Reiniciar automaticamente el servidor de destino en caso de ser necesario` y presionamos `Instalar`

![ConfirmacionRoles](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

Despues de instalar presionamos `Cerrar`

![ResultadoAgregarRoles](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

De nuevo en la ventana de Administrador del Servidor, nos aparece una bandera con una alerta amarilla
presionamos la bandera y despues en `Promover este servidor a controlador de dominio`

![BanderaAlerta](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

![ConfiguracionPosterior](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

Nos aparece una ventana para configurar el dominio, seleccionamos `Agregar un nuevo bosque` y escribimos el Nombre del Dominio Raiz que en este caso sera `curso.ciber` y presionamos `Siguiente`

![ConfigDom](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

Nos pide colocar una contraseña que debe tener minimo 8 caracteres, mayusculas, numeros y simbolos especiales, colocaremos para este lab "Passw0rd_2023" y presionamos `Siguiente`

![ConfigDomPass](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

Puede que nos aparezca una alerta, presionamos `Siguiente`

![ConfigDomAlerta](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)


NetBIOS (del inglés, " Network Basic Input/Output System ") es una especificación de interfaz para acceso a servicios de red, es decir, una capa de software desarrollado para enlazar un sistema operativo de red con hardware específico. El nombre de Netbios es un nombre de 16 bytes para un servicio o función de red en una máquina que ejecuta Microsoft Windows Server. Los nombres de Netbios son una forma más amigable de identificar computadoras en una red que los números de red y son utilizados por servicios y aplicaciones habilitadas para NetBios.

En este caso dejamos "CURSO" y presionamos `Siguiente` hasta `Comprobacion de requisitos previos` donde presionaremos `Instalar` y despues se reiniciará

![ConfigDomNetBios](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomNetBios.jpg)

![ConfigDomRutaAcceso](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomRutaAcceso.jpg)

![ConfigDomRevision](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomRevision.jpg)

![ConfigDomComprobacion](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomComprobacion.jpg)

![ConfigDomReinicio](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigDomReinicio.jpg)

## Configurar Active Directory

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD1.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD2.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD3.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD4.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD5.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD6.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD7.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD8.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD9.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD10.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD11.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD12.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD13.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD14.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD15.jpg)

![](https://github.com/jcca1992/INFOSEC/blob/main/Universidad/Admin-Servidores/Imagenes/ConfigAD16.jpg)