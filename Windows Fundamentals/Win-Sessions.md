## SESIONES DE WINDOWS

#### INTERACTIVO

Una sesión de inicio de sesión local o interactiva la inicia un usuario que se autentica en un sistema local o de dominio ingresando sus credenciales. Se puede iniciar un inicio de sesión interactivo iniciando sesión directamente en el sistema, solicitando una sesión de inicio de sesión secundaria mediante el comando `runas` a través de la línea de comandos o mediante una conexión de escritorio remoto.

#### NO INTERACTIVO

Las cuentas no interactivas en Windows difieren de las cuentas de usuario estándar, ya que no requieren credenciales de inicio de sesión. Hay 3 tipos de cuentas no interactivas: la cuenta del sistema local, la cuenta de servicio local y la cuenta de servicio de red. El sistema operativo Windows suele utilizar cuentas no interactivas para iniciar automáticamente servicios y aplicaciones sin necesidad de interacción del usuario. Estas cuentas no tienen una contraseña asociada y generalmente se usan para iniciar servicios cuando se inicia el sistema o para ejecutar tareas programadas.

Existen diferencias entre los tres tipos de cuentas

|Cuenta|Descripcion|
|--|--|
|Local System Account|También conocida como la cuenta `NT AUTHORITY\SYSTEM`, esta es la cuenta más poderosa en los sistemas Windows. Se utiliza para una variedad de tareas relacionadas con el sistema operativo, como iniciar servicios de Windows. Esta cuenta es más potente que las cuentas del grupo de administradores locales.|
|Local Service Account|Conocida como la cuenta `NT AUTHORITY\LocalService`, esta es una versión menos privilegiada de la cuenta SYSTEM y tiene privilegios similares a una cuenta de usuario local. Tiene una funcionalidad limitada y puede iniciar algunos servicios.|
|Network Service Account|Esta se conoce como la cuenta `NT AUTHORITY\NetworkService` y es similar a una cuenta de usuario de dominio estándar. Tiene privilegios similares a la cuenta de servicio local en la máquina local. Puede establecer sesiones autenticadas para ciertos servicios de red.|
