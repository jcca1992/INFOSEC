## ATAJOS
___
Hay muchos atajos que podemos usar para que trabajar con Linux sea más fácil y rápido. Después de que nos hayamos familiarizado con los más importantes y los hayamos convertido en un hábito, nos ahorraremos mucho tipeo. Algunas de ellas incluso nos ayudarán a evitar usar el ratón en el terminal.
___
### AUTOCOMPLETAR

[`TAB`]: inicia el autocompletado. Esto nos sugerirá diferentes opciones basadas en el `STDIN` que proporcionamos. Estas pueden ser sugerencias específicas como directorios en nuestro entorno de trabajo actual, comandos que comienzan con la misma cantidad de caracteres que ya escribimos u opciones.
___
### MOVIMIENTO DEL CURSOR

`[CTRL] + A` - Mueve el cursor al `principio` de la línea actual.

`[CTRL] + E` - Mueve el cursor al `final` de la línea actual.

`[CTRL] + [←]` / `[→]` - Saltar al principio de la palabra actual/anterior.

`[ALT] + B / F` - Saltar hacia atrás/adelante una palabra.
___
### ELIMINAR LINEA ACTUAL

`[CTRL] + U` - Borra todo desde la posición actual del cursor hasta el `comienzo` de la línea.

`[Ctrl] + K` - Borra todo desde la posición actual del cursor hasta el `final` de la línea.

`[Ctrl] + W` - Borra la palabra que precede a la posición del cursor.
___
### PEGAR CONTENIDO BORRADO

`[Ctrl] + Y`: pega el texto o la palabra borrados.
___
### FINALIZAR TAREAS

`[CTRL] + C` - Finaliza la tarea/proceso actual enviando la señal `SIGINT`. Por ejemplo, esto puede ser un análisis que está ejecutando una herramienta. Si estamos viendo el escaneo, podemos detenerlo/matar este proceso usando este atajo. Si bien no está configurado y desarrollado por la herramienta que estamos utilizando. El proceso se cancelará sin pedirnos confirmación.
___

### FIN DE ARCHIVO (EOF)

`[CTRL] + D`: cierra la tubería `STDIN` que también se conoce como Fin de archivo (EOF) o Fin de transmisión.
___
### LIMPIAR TERMINAL

`[CTRL] + L` - Borra la terminal. Una alternativa a este atajo es el comando `borrar` que puedes escribir para borrar nuestro terminal.
___
### PROCESOS EN SEGUNDO PLANO

`[CTRL] + Z` - Suspender el proceso actual enviando la señal `SIGTSTP`.
___
### BUSCAR EN EL HISTORIAL DE COMANDOS

`[CTRL] + R`: busque en el historial de comandos los comandos que escribimos anteriormente que coincidan con nuestros patrones de búsqueda.

`[↑]` / `[↓]` - Ir al comando anterior/siguiente en el historial de comandos.
___
### CAMBIAR ENTRE APLICACIONES

`[ALT] + [TAB]` - Cambiar entre aplicaciones abiertas.
___
### ZOOM

`[CTRL] + [+]` - Acercar.

`[CTRL] + [-]` - Alejar.
___
#### [Anterior (Gestion de permisos)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Perm-Management.md)
#### [Siguiente (Seguridad Linux)](https://github.com/jcca1992/INFOSEC/blob/main/Linux%20Fundamentals/Linux-Security.md)