Para encontrar esta Flag seguir los siguientes pasos : 

# PASO 1:
Primero revisamos los permisos del archivo README con el comando `ls -l` al ver los permisos podemos observar que el dueño del archivo es **bandit1** y el grupo asignado es **bandit0** (recordemos que cada usuario tiene un grupo con el mismo nombre del usuario y como nosotros somos **bandit0**  tenemos los permisos del grupo **bandit0**)

# PASO 2:
Analizamos los permisos y deducimos que como pertenecemos al grupo **bandit0** los permisos a los que respondemos es a los del grupo en el archivo que en este caso solo seria `r`  que es leer (Read en ingles)

# PASO 3:  
Como el único permiso que tenemos es el de leer procedemos a hacerle un `cat` al archivo **readme**

Comando : `cat readme` 

Para terminar con la sorpresa de que encontramos la contraseña del usuario **bandit1**  al cual debemos de conectarnos 

**Contraseña de bandit 1 :** 
`ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`   

(La contraseña va cambiando con el tiempo se debe de seguir el paso a paso para conseguir la contraseña actual) 