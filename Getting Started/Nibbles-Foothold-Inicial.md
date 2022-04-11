Ahora que hemos iniciado sesión en el portal de administración, debemos intentar convertir este acceso en ejecución de código y, en última instancia, obtener acceso de shell inverso al servidor web. Sabemos que un módulo Metasploit probablemente funcionará para esto, pero enumeremos el portal de administración para otras vías de ataque. Mirando un poco alrededor, vemos las siguientes páginas:

| Pagina | Contenido|
| -- | -- |
|`Publish`| hacer una nueva publicación, publicación de video, publicación de citas o nueva página. Podría ser Interesante. |
| `Comments`| no muestra comentarios publicados | 
| `Manage`| Nos permite administrar publicaciones, páginas y categorías. Podemos editar y eliminar categorías, no demasiado interesantes.|
| `Settings` | Desplazarse hacia abajo confirma que la versión vulnerable 4.0.3 está en uso. Hay varias configuraciones disponibles, pero ninguna nos parece valiosa. |
| `Themes` | Esto nos permite instalar un nuevo tema de una lista preseleccionada. |
| `Plugins` | Nos permite configurar, instalar o desinstalar complementos. El complemento `My image` nos permite cargar un archivo de imagen. ¿Se podría abusar de esto para cargar código `PHP` potencialmente? |

Intentar crear una nueva página e incrustar código o cargar archivos no parece ser el camino. Echemos un vistazo a la página de complementos.

![](https://academy.hackthebox.com/storage/modules/77/plugins.png)

Intentemos usar este complemento para cargar un fragmento de código PHP en lugar de una imagen. El siguiente fragmento se puede usar para probar la ejecución del código.