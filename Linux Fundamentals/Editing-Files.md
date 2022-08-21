## EDITANDO ARCHIVOS
___
Hay varias formas de editar un archivo. Uno de los editores de texto más comunes para esto es `Vi` y `Vim`. Más raramente, está el editor Nano. Primero nos ocuparemos del editor Nano aquí, ya que es un poco más fácil de entender. Podemos crear un nuevo archivo directamente con el editor Nano especificando el nombre del archivo directamente como primer parámetro. En este caso, creamos un nuevo archivo llamado `notes.txt`.

~~~
Juceco@htb[/htb]$ nano notes.txt
~~~

Ahora deberíamos ver un llamado "`pager`" abierto, y podemos ingresar o insertar libremente cualquier texto. Nuestra shell debería verse así.

~~~
 GNU nano 2.9.3                                    notes.txt                                              

Here we can type everything we want and make our notes.▓


^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line  M-E Redo
~~~

A continuación vemos dos líneas con breves descripciones. El signo de intercalación (`^`) representa nuestra tecla "`[CTRL]`". Por ejemplo, si presionamos `[CTRL + W]`, aparece una línea de "`Search:`" en la parte inferior del editor, donde podemos ingresar la palabra o palabras que estamos buscando. Si ahora buscamos la palabra "`we`" y presionamos `[ENTER]`, el cursor se moverá a la primera palabra que coincida.

Para saltar a la siguiente coincidencia con el cursor, presionamos `[CTRL + W]` nuevamente y confirmamos con `[ENTER]` sin ninguna información adicional.

Ahora podemos guardar el archivo presionando `[CTRL + O]` y confirmar el nombre del archivo con `[ENTER]`.

~~~
GNU nano 2.9.3                                    notes.txt                                              

Here we can type everything we want and make our notes.

File Name to Write: notes.txt▓                                                                           
^G Get Help    M-C Case Sens  M-B Backwards  M-J FullJstify ^W Beg of Par  ^Y First Line  ^P PrevHstory
^C Cancel      M-R Regexp     ^R Replace     ^T Go To Line  ^O End of Par  ^V Last Line   ^N NextHstory
~~~

Después de haber guardado el archivo, podemos salir del editor con `[CTRL + X]`.

**De Vuelta a la SHELL**

Para ver el contenido del archivo, podemos usar el comando `cat`.

~~~
Juceco@htb[/htb]$ cat notes.txt

Here we can type everything we want and make our notes.
~~~

Hay muchos archivos en los sistemas Linux que pueden desempeñar un papel esencial para nosotros como pentesters cuyos derechos no han sido configurados correctamente por los administradores. Dichos archivos pueden incluir el archivo "`/etc/passwd`".
___
### VIM

`Vim` es un editor de código abierto para todo tipo de texto ASCII, al igual que Nano. Es un clon mejorado del `Vi` anterior. Es un editor extremadamente poderoso que se enfoca en lo esencial, es decir, editar texto. Para tareas que van más allá, `Vim` proporciona una interfaz para programas externos, como `grep`, `awk`, `sed`, etc., que pueden manejar sus tareas específicas mucho mejor que una función correspondiente implementada directamente en un editor. Esto hace que el editor sea pequeño y compacto, rápido, potente, flexible y menos propenso a errores.

Vim sigue el principio de Unix: muchos pequeños programas especializados que están bien probados, cuando se combinan y se comunican entre sí, lo que da como resultado un sistema flexible y poderoso.

~~~
Juceco@htb[/htb]$ vim
~~~

~~~
  1 $
~
~                              VIM - Vi IMproved                                
~                                                                               
~                               version 8.0.1453                                
~                           by Bram Moolenaar et al.                            
~           Modified by pkg-vim-maintainers@lists.alioth.debian.org             
~                 Vim is open source and freely distributable                   
~                                                                               
~                           Sponsor Vim development!                            
~                type  :help sponsor<Enter>    for information                  
~                                                                               
~                type  :q<Enter>               to exit                          
~                type  :help<Enter>  or  <F1>  for on-line help                 
~                type  :help version8<Enter>   for version info                 
~                                                                               
                                                                         
                                                                    0,0-1         All
~~~

A diferencia de Nano, `Vim` es un editor modal que puede distinguir entre texto y entrada de comandos. `Vim` ofrece un total de seis modos fundamentales que nos facilitan el trabajo y hacen tan potente a este editor:

|Modo|Descripcion|
|--|--|
|`Normal`|En modo normal, todas las entradas se consideran comandos del editor. Por lo tanto, no hay inserción de los caracteres ingresados en el búfer del editor, como es el caso con la mayoría de los otros editores. Después de iniciar el editor, generalmente estamos en el modo normal.|
|`Insert`|Con algunas excepciones, todos los caracteres ingresados se insertan en el búfer.|
|`Visual`|El modo visual se utiliza para marcar una parte contigua del texto, que se resaltará visualmente. Posicionando el cursor, cambiamos el área seleccionada. El área resaltada se puede editar de varias maneras, como eliminarla, copiarla o reemplazarla.|
|`Command`|Nos permite ingresar comandos de una sola línea en la parte inferior del editor. Esto se puede usar para ordenar, reemplazar secciones de texto o eliminarlas, por ejemplo.|
|`Replace`|En el modo de reemplazo, el texto recién ingresado sobrescribirá los caracteres de texto existentes a menos que no haya más caracteres antiguos en la posición actual del cursor. Luego se agregará el texto recién ingresado.|

Cuando tenemos abierto el editor de Vim, podemos entrar en el modo de comando escribiendo "`:`" y luego escribiendo "`q`" para cerrar Vim.

~~~
  1 $
~
~                              VIM - Vi IMproved                                
~                                                                               
~                               version 8.0.1453                                
~                           by Bram Moolenaar et al.                            
~           Modified by pkg-vim-maintainers@lists.alioth.debian.org             
~                 Vim is open source and freely distributable                   
~                                                                               
~                           Sponsor Vim development!                            
~                type  :help sponsor<Enter>    for information                  
~                                                                               
~                type  :q<Enter>               to exit                          
~                type  :help<Enter>  or  <F1>  for on-line help                 
~                type  :help version8<Enter>   for version info                 
~                                                                               
:q▓
~~~

Vim ofrece una excelente oportunidad llamada `vimtutor` para practicar y familiarizarse con el editor. Puede parecer muy difícil y complicado al principio, pero solo se sentirá así por un corto tiempo. La eficiencia que ganamos con Vim una vez que nos acostumbramos es enorme.

**VIMTUTOR
~~~
Juceco@htb[/htb]$ vimtutor
~~~

~~~
===============================================================================
=    W e l c o m e   t o   t h e   V I M   T u t o r    -    Version 1.7      =
===============================================================================

     Vim is a very powerful editor that has many commands, too many to
     explain in a tutor such as this.  This tutor is designed to describe
     enough of the commands that you will be able to easily use Vim as
     an all-purpose editor.

     The approximate time required to complete the tutor is 25-30 minutes,
     depending upon how much time is spent with experimentation.

     ATTENTION:
     The commands in the lessons will modify the text.  Make a copy of this
     file to practice on (if you started "vimtutor" this is already a copy).

     It is important to remember that this tutor is set up to teach by
     use.  That means that you need to execute the commands to learn them
     properly.  If you only read the text, you will forget the commands!

     Now, make sure that your Caps-Lock key is NOT depressed and press
     the   j   key enough times to move the cursor so that lesson 1.1
     completely fills the screen.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
~~~






___
#### [Anterior (Trabajando con Archivos y Directorios)]()
#### [Siguiente (Buscando Archivos y Directorios)]()