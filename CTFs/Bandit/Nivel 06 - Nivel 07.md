---
tags:
  - CTF_Bandit
---

La solución es algo parecido al del **bandit6** los pasos son los siguientes: 

# PASO 1 y ÚNICO: 
Analizamos donde aparecimos después de habernos conectado al **bandit6** y vemos que no tenemos nada si hacemos el comando `ls` ni con el comando `ls -l` solo cuando hacemos el `ls -la` es que vemos que aparecen un par de archivos ocultos entonces lo mejor que podemos hacer es empezar a buscar por las propiedades que nos dice la pagina y lo haríamos de la siguiente manera: 

**COMANDO :** `find / -user bandit7 -group bandit6 -size 33c 2>/dev/null`

`find` -> Es el comando que le indica a Linux que vamos a buscar algo 

`/` -> con el / le decimos que busque en todo el disco no solo en una ruta especifica

`user bandit7` -> Es el primero filtro. Buscamos un archivo que pertenezca al usuario llamado **bandit7**

`group bandit6` -> Es nuestro segundo filtro. Buscamos un archivo que pertenezca al grupo **bandit6**

`size 33c` -> Nuestro tercer filtro. El archivo debe pesar  exactamente **33 bytes** (la c al final es para que Linux entienda que nos referimos a bytes)

`2>/dev/null`
`2>` ->Significa "coja el canal de errores"
  `/dev/null` -> Es el agujero negro en Linux todo lo que se mande para allá lo desaparece en nuestro caso queremos desaparecer los errores 
  **traducción:** Todo error de 'permiso denegado' que salga lo mande al agujero negro de Linux para no ver por pantalla ningún mensaje de 'permiso denegado'

Al ejecutar este comando el resultado será una ruta en la cual esta el archivo que encaja con todas las propiedades y se acopla a todos los filtro que pusimos en el `find` a esa ruta que nos arroja toca hacerle un `cat` en mi caso quedaría de la siguiente manera: 

`cat /var/lib/dpkg/info/bandit7.password`

Al hacerle el `cat` a esa ruta nos dejará ver la contraseña del **bandit7** que en mi caso es la siguiente : 
`morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

> ## Recordar que las contraseñas cambian con el tiempo. 