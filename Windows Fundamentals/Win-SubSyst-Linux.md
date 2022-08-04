## SUBSISTEMA DE WINDOWS PARA LINUX (WSL)

[WSL](https://docs.microsoft.com/en-us/windows/wsl/) es una característica que permite que los binarios de Linux se ejecuten de forma nativa en Windows 10 y Windows Server 2019. Originalmente estaba destinado a desarrolladores que necesitaban ejecutar `Bash`, `Ruby` y herramientas de línea de comandos nativas de Linux como `sed`, `awk`, `grep`, etc. ., directamente en su estación de trabajo Windows. La segunda versión de WSL, lanzada en mayo de 2019, introdujo un kernel de Linux real que utilizaba un subconjunto de características de `Hyper-V`.

WSL se puede instalar ejecutando el comando PowerShell `Enable-WindowsoptionAlfeature -Online -FeateReName Microsoft-Windows-Subsystem-Linux` como administrador. Una vez que esta función está habilitada, podemos descargar una distribución de Linux de la tienda de Microsoft e instalarla o descargar manualmente la distribución de Linux de nuestra elección y desempaquetarlo e instalarlo desde la línea de comandos.

WSL instala una aplicación llamada `Bash.exe`, que se puede ejecutar simplemente escribiendo `BASH` en una consola de Windows para generar un shell `bash`. Tenemos la apariencia completa de un host de Linux desde este shell, incluida la estructura de directorio de Linux estándar.

~~~
PS C:\htb> ls /

bin dev home lib lLib64 media opt root sbin srv tmp var
boot etc init 1lib32 Libx32 mnt proc run Snap sys usr
~~~

Podemos acceder al volumen C$ y otros volúmenes en el sistema operativo host a través del directorio `mnt`, lo que hace que la transición del host WSL y el sistema operativo de Windows Host sin perfectas. Una vez en este shell bash, podemos interactuar con WSL, ya que interactuaríamos con cualquier sistema operativo basado en Linux: ejecutar comandos, instalar actualizaciones o paquetes, etc.

~~~
PS C:\htb> uname -a

Linux WS01 4.4.0-18362-Microsoft #476-Microsoft Frit Nov 01 16:53:00
PST 2019 x86_64 x86 _64 x86_64 GNU/Linux
~~~