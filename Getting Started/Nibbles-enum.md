Hay 201 maquinas independientes de varios sistemas operativos y niveles de dificultad disponibles en la plataforma HTB con membresía VIP al momento de escribir esto. Esta membresía incluye un recorrido oficial creado por HTB para cada máquina retirada. También podemos encontrar tutoriales en blogs y videos para la mayoría de las cajas con una búsqueda rápida en Google.

Para nuestros propósitos, repasemos la maquina `Nibbles`, un cuadro de Linux fácil de calificar que muestra tácticas de enumeración comunes, explotación básica de aplicaciones web y una configuración incorrecta relacionada con archivos para escalar privilegios.

![Nibbles](https://academy.hackthebox.com/storage/modules/77/nibbles_card.png)

Veamos primero algunas estadísticas de la máquina:

|Nombre de maquina|Nibbles|
|--|--|
|Creador| mrb3n|
| sistema operativo | Linux |
| Dificultad | Facil |
| User Path | Web |
| Escalada de privilegios | archivos escribibles por todos / Configuración incorrecta de Sudoers |
| Video Ippsec | [https://www.youtube.com/watch?v=s_0GcRGv6Ds](https://www.youtube.com/watch?v=s_0GcRGv6Ds)|
| Tutorial | [https://0xdf.gitlab.io/2018/06/30/htb-nibbles.html](https://0xdf.gitlab.io/2018/06/30/htb-nibbles.html) |

Nuestro primer paso al acercarnos a cualquier máquina es realizar una enumeración básica. Primero, comencemos con lo que sabemos sobre el objetivo. Ya sabemos la dirección IP del objetivo, que es Linux y tiene un vector de ataque relacionado con la web. A esto lo llamamos un enfoque `Gray-Box` porque tenemos cierta información sobre el objetivo. En la plataforma HTB, las 20 máquinas de lanzamiento semanal "activas" se abordan desde una perspectiva de `Black-Box`. Los usuarios reciben la dirección IP y el tipo de sistema operativo de antemano, pero no información adicional sobre el objetivo para formular sus ataques. Esta es la razón por la que la enumeración exhaustiva es fundamental y, a menudo, es un proceso iterativo.

Antes de continuar, demos un paso atrás y veamos los diversos enfoques para las acciones de prueba de penetración. Hay tres tipos principales, `Black-Box`, `Gray_Box` y `White-Box`, y cada uno difiere en el objetivo y el enfoque.

| Enfoque | Descripcion |
| -- | -- |
| `Black-Box` |
| `Gray_Box` |
| `White-Box` |
