---
tags:
  - CTF_Bandit
---

Bueno ahora nos toca encontrar la contraseña de **bandit4**

# PASO 1:
Vamos a hacer un `ls -l` para ver archivos o directorios tenemos en este caso al hacer un `ls` nos encontramos con la carpeta de nombre **"inhere"**  como bien nos mencionaba **bandit**  en su pagina.

# PASO 2:
Entramos en este directorio con el comando : `cd inhere` y pocedemos a hacer un `ls -l` par ver con que nos encontramos.

# PASO 3:
Al hacer un `ls -l` vemos que no nos devuelve nada ahí es donde debemos ponernos a pensar **¿y que mas puedo hacer?** cuando no aparece nada debemos de buscar los dichosos **"archivos ocultos"**.

# PASO 4 
Para listar archivos ocultos lo hacemos con el comando : `cat . + nombre del archivo` en mi caso seria de la siguiente manera. 

`cat ...Hiding-From-You` (Por que asi se llama el archivo en este momento puede que para cuando lo estés haciendo el nombre sea diferente ).

> ## ✍️ Consejo: 
> 
>  En mi caso a la hora de hacerlo al principio me daba error por que solo puse un punto (.) delante del nombre y este cuando le hice el `ls -la` tenia 3 puntos **(...)**  entonces cuando lo volví a hacer pero con sus respectivos puntos y si me salio la contraseña de **bandit4*  que es :
>  
`2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`




