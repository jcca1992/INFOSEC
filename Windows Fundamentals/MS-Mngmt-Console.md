## CONSOLA DE AMINISTRACION DE MICROSOFT (MMC)

La MMC se puede usar para agrupar complementos o herramientas administrativas para administrar hardware, software y componentes de red dentro de un host de Windows. Ha existido desde Windows Server 2000 y se ejecuta en todas las versiones de Windows. También podemos usar MMC para crear herramientas personalizadas y distribuirlas a los usuarios. MMC funciona con el concepto de complementos, lo que permite a los administradores crear una consola personalizada con solo las herramientas administrativas necesarias para administrar varios servicios. Estos complementos se pueden agregar para administrar sistemas locales y remotos.

Podemos abrir MMC simplemente escribiendo `mmc` en el menú Inicio. Cuando abramos MMC por primera vez, estará en blanco.

![](https://academy.hackthebox.com/storage/modules/49/MMC.png)

Desde aquí, podemos navegar hasta File `--> Add or Remove Snap-ins` y comenzar a personalizar nuestra consola administrativa.

![](https://academy.hackthebox.com/storage/modules/49/MMC_add_remove.png)

A medida que comencemos a agregar complementos, se nos preguntará si deseamos agregar el complemento para administrar solo la computadora local o si se usará para administrar otra computadora en la red.

![](https://academy.hackthebox.com/storage/modules/49/MMC_services.png)

Una vez que hayamos terminado de agregar complementos, aparecerán en el lado izquierdo de MMC. Desde aquí, podemos guardar el conjunto de complementos como un archivo `.msc`, para que todos se carguen la próxima vez que abramos `MMC`. De forma predeterminada, se guardan en el directorio `Herramientas administrativas de Windows` en el menú `Inicio`. La próxima vez que abramos `MMC`, podremos optar por cargar cualquiera de las vistas que hayamos creado.

![](https://academy.hackthebox.com/storage/modules/49/saved_msc.png)

