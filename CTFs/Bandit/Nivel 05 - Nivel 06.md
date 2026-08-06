Y seguimos con los retos del **bandit** 

# PASO 1:
Como siempre hacemos el poderosisimo `ls` para darnos cuenta que contamos con la carpeta **inhere** ingresamos a ella con el comando : `cd inhere` una vez dentro hacemos un `ls` y vemos que hay multiples carpetas aquí es donde se nos complica la cosa. 

# PASO 2: 
Analizamos lo que nos dice la pagina de **OverTheWire** "La contraseña para el siguiente nivel se almacena en un *archivo* en algún lugar debajo del directorio/carpeta **inhere** y tiene las siguientes propiedades"

- **Legible por humanos**
- **1033 bytes de tamaño**
- **No ejecutable**

Teniendo esto en cuenta podemos llegar a la conclusion de debemos de usar el comando `find`  para poder encontrar el archivo del cual nos habla el comando completo seria : `find . -type f -size 1033c ! -executable` 
`find` -> Le dice a linux **Oye, necesito que busques algo...**

`.` -> Significa **Aquí mismo** le dice que busque en la carpeta en la cual nos encontramos y todas las que estan dentro de ella

`type f` -> Le dice que ignore las capetas que solo muestre archivos / ficheros

`size 1033c` -> Filtra por peso 
  `1033` -> Es el numero exacto que pedia el reto 
  `c` -> Significa bytes sin la c linux creeria que hablamos de bloques de disco asi que es importante ponerla.

`!` -> Es el simbolo de negacion, le dice a linux que haga lo contrario de lo que sigue despues del simbolo `!`.

`executable` -> Se refiere a programas o scripts que se pueden **ejecutar/correr** al juntarlo con el simbolo ! se traduce como "Que no sea un programa ejecutable".

Luego de ejecutar el comando nos arrojará la ruta a la cual nos lleva al archivo que cumple con todos los filtros que pusimos y para leer lo que hay dentro y encontrar la contraseña solo debemos de hacerle un `cat` a esa ruta que nos arroja el comando quedaría así :  `cat ./maybehere07/.file2`

Luego de realizar el cat habremos encontrado la contraseña del **bandit6** que en mi caso es `:HWasnPhtq9AVKe0dmk45nxy20cvUa6EG` 