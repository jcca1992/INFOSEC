## WINDOWS SECURITY

La seguridad es un tema crítico en los sistemas operativos de Windows. Los sistemas de Windows tienen muchas partes móviles que presentan una vasta superficie de ataque. Debido a las muchas aplicaciones, características y capas de configuración incorporadas, los sistemas de Windows se pueden configurar fácilmente, abriéndolos para atacar incluso si están completamente parcheados.

Tiene muchas características incorporadas que se pueden abusar y ha sufrido una amplia variedad de vulnerabilidades críticas, lo que resulta en hazañas remotas y locales muy utilizadas y muy efectivas.

Microsoft ha mejorado la seguridad de Windows a lo largo de los años. A medida que la interconexión de nuestro mundo continúa expandiéndose y los atacantes se vuelven más sofisticados, Microsoft ha seguido agregando nuevas características que los administradores de sistemas pueden utilizar para endurecer los sistemas y bloquear activamente y detectar intentos de intrusión y mal uso.

Windows sigue ciertos principios de seguridad. Estas son unidades en el sistema que pueden ser autorizadas o autenticadas para una acción particular. Estas unidades incluyen usuarios, computadoras en la red, hilos o procesos. Los principios están diseñados para dificultar que los atacantes o el software malicioso obtengan acceso no autorizado y exploten el sistema de manera no deseada.
___
### IDENTIFICADOR DE SEGURIDAD (SID)

Cada una de las entidades de seguridad del sistema tiene un identificador de seguridad único (SID). El sistema genera automáticamente SIDS. Esto significa que incluso si, por ejemplo, tenemos dos usuarios idénticos en el sistema, Windows puede distinguir los dos y sus derechos en función de su SIDS. Los SIDS son valores de cadena con diferentes longitudes, que se almacenan en la base de datos de seguridad. Estas SID se agregan al token de acceso del usuario para identificar todas las acciones que el usuario está autorizado a tomar.

Un SID consiste en la autoridad del identificador y la ID relativa (RID). En un entorno de dominio `Active Directory` (AD), el SID también incluye el dominio SID.

~~~
PS C:\htb> whoami /user

USER INFORMATION
----------------

User Name           SID
=================== =============================================
ws01\bob S-1-5-21-674899381-4069889467-2080702030-1002
~~~

El SID se descompone en este patrón

~~~
(SID)-(revision level)-(identifier-authority)-(subauthority1)-(subauthority2)-(etc)
~~~

Desglosemos el SID pieza por pieza.

|Numero|Significado|Descripcion|
|S|SID|Identifica la cadena como un SID.|
|1|Nivel de Revision|Hasta la fecha, esto nunca ha cambiado y siempre ha sido `1`.|
|5|Identificador de Autoridad|Una cadena de 48 bits que identifica la autoridad (la computadora o la red) que creó el SID.|
|21|Subautoridad 1| Este es un número variable que identifica la relación o grupo del usuario descrito por el SID a la autoridad que la creó. Nos dice en qué orden esta autoridad creó la cuenta del usuario.|
|674899381-4069889467-2080702030|Subautoridad 2|Nos dice qué computadora (o dominio) creó el número|
|1002|Subautoridad 3|El RID que distingue una cuenta de otra. Nos dice si este usuario es un usuario normal, un invitado, un administrador o parte de algún otro grupo|
___

### ADMINISTRADOR DE CUENTAS DE SEGURIDAD (SAM) Y CONTROL DE ACCESO DE ENTRADAS (ACE)

SAM otorga derechos a una red para ejecutar procesos específicos.

Los derechos de acceso en sí son administrados por el Control de Acceso de Entradas (ACE) en las listas de control de acceso (ACL). Las ACL contienen ACE que definen qué usuarios, grupos o procesos tienen acceso a un archivo o para ejecutar un proceso, por ejemplo

Los permisos para acceder a objetos de seguridad son dados por el descriptor de seguridad, clasificados en dos tipos de ACL: la lista de control de acceso discrecional (DACL) o la lista de control de acceso al sistema (SACL). Cada hilo y proceso iniciado o inicializado por un usuario pasa por un proceso de autorización. Una parte integral de este proceso son los tokens de acceso, validados por la Autoridad de Seguridad local (LSA). Además de SID, estos tokens de acceso contienen otra información relevante para la seguridad. Comprender estas funcionalidades es una parte esencial de aprender a usar y trabajar en torno a estos mecanismos de seguridad durante la fase de escalada de privilegios.
___
### CONTROL DE CUENTAS DE USUARIOS (UAC)

[El control de acceso al usuario (UAC)](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works) es una característica de seguridad en Windows para evitar que el malware ejecute o manipule procesos que podrían dañar la computadora o su contenido. Existe el modo de aprobación de administrador en UAC, que está diseñado para evitar que se instale un software no deseado sin el conocimiento del administrador o para evitar que se realicen cambios en todo el sistema. Seguramente ya ha visto el mensaje de consentimiento si ha instalado un software específico, y su sistema ha pedido confirmación si desea que lo instalen. Dado que la instalación requiere derechos de administrador, aparece una ventana, preguntándole si desea confirmar la instalación. Con un usuario estándar que no tiene derechos para la instalación, la ejecución será denegada o se le solicitará la contraseña del administrador. Este indicador de consentimiento interrumpe la ejecución de scripts o binarios que el malware o los atacantes intentan ejecutar hasta que el usuario ingrese la contraseña o confirme la ejecución. Para comprender cómo funciona UAC, necesitamos saber cómo está estructurado y cómo funciona, y qué desencadena el indicador de consentimiento. El siguiente diagrama, adaptado de la fuente [aquí](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works), ilustra cómo funciona UAC.

![](https://academy.hackthebox.com/storage/modules/49/uacarchitecture1.png)
___
### REGISTRO

El [registro](https://en.wikipedia.org/wiki/Windows_Registry) es una base de datos jerárquica en Windows, crítico para el sistema operativo. Almacena la configuración de bajo nivel para el sistema operativo Windows y las aplicaciones que eligen usarlo. Se divide en datos específicos de la computadora y específicos del usuario. Podemos abrir el editor de registro escribiendo `regedit` desde la línea de comandos o la barra de búsqueda de Windows.

![](https://academy.hackthebox.com/storage/modules/49/regedit.png)

La estructura de árbol consta de carpetas principales (teclas de raíz) en las que se encuentran las subcarpetas (subkeyas) con sus entradas/archivos (valores). Hay 11 tipos diferentes de valores que se pueden ingresar en una subpuesta.

|Valor|Tipo|
|--|--|
|REG_BINARY|Datos binarios en cualquier forma.|
|REG_DWORD|Un número de 32 bits.|
|REG_DWORD_LITTLE_ENDIAN|Un número de 32 bits en formato little-Endian. Windows está diseñado para ejecutarse con arquitecturas de computadoras little-Endian. Por lo tanto, este valor se define como reg_dword en los archivos de encabezado de Windows.|
|REG_DWORD_BIG_ENDIAN|Un número de 32 bits en formato de big-endian. Algunos sistemas UNIX admiten arquitecturas de big-endian.|
|REG_EXPAND_SZ|Una cadena null-terminated que contiene referencias no expandidas a las variables de entorno (por ejemplo, "%path%"). Será una cadena Unicode o ANSI dependiendo de si usa las funciones Unicode o ANSI. Para expandir las referencias variables de entorno, use la función [ExpandenVironmentTrings](https://docs.microsoft.com/en-us/windows/win32/api/processenv/nf-processenv-expandenvironmentstringsa).|
|REG_LINK|Una cadena Unicode null-terminated que contiene la ruta objetivo de un enlace simbólico creado llamando a la función [RegCreateKeyex](https://docs.microsoft.com/en-us/windows/desktop/api/Winreg/nf-winreg-regcreatekeyexa) con REG_OPTION_CREATE_LINK.|
|REG_MULTI_SZ|Una secuencia de cadenas null-terminated, terminada por una cadena vacía (\ 0). El siguiente es un ejemplo: String1 \ 0String2 \ 0String3 \ 0LastString \ 0 \ 0 La primera \ 0 termina la primera cadena, la segunda a la última \ 0 termina la última cadena, y la final \ 0 termina la secuencia. Tenga en cuenta que el terminador final debe tenerse en cuenta en la longitud de la cadena.|
|REG_NONE|Sin tipo de valor definido.|
|REG_QWORD| Un numero de 64 Bit|
|REG_QWORD_LITTLE_ENDIAN|Un número de 64 bits en formato little-Endian. Windows está diseñado para ejecutarse con arquitecturas de computadoras little-Endian. Por lo tanto, este valor se define como reg_qword en los archivos de encabezado de Windows.|
|REG_SZ|Una cadena null-terminated . Esto será una cadena Unicode o ANSI, dependiendo de si usa las funciones Unicode o ANSI.|

>Endianness designa el formato en el que se almacenan los datos de más de un byte en un ordenador. Usando este criterio el sistema `big-endian` adoptado por Motorola entre otros, consiste en representar los bytes en el orden "natural": así el valor hexadecimal 0x4A3B2C1D se codificaría en memoria en la secuencia {4A, 3B, 2C, 1D}. En el sistema `little-endian` adoptado por Intel, entre otros, el mismo valor se codificaría como {1D, 2C, 3B, 4A}, de manera que de este modo se hace más intuitivo el acceso a datos, porque se efectúa fácilmente de manera incremental de menos relevante a más relevante (siempre se opera con incrementos de contador en la memoria), en un paralelismo a "lo importante no es como empiezan las cosas, sino como acaban."

>null-terminated es una cadena de caracteres almacenada como una matriz y termina con un caracter nulo (un caracter con valor 0)