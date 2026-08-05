📓 Mis Apuntes: Bandit Nivel 17 → 18
El Reto:
En mi directorio ("home") había dos archivos: "passwords.old" y "passwords.new". El resto decía que la contraseña para el siguiente nivel era "la única linea que habia cambiado" entre el archivo viejo y el nuevo. Como los archivos eranmuy largos, buscar la diferencia a simple vista era imposible.

Concepto Nuevo: Comparación de Archivos (diff)
Aprendí que en Linux no necesito revisar archivos línea por línea con mis ojos. Existe el comando "diff" (diferencia), que pone dos archivos frente a frente y me muestra "solo" lo que es distinto entre ellos.

-Simbolo < (Menor que): Indica líneas que están en el archivo "viejo" (el primero que pongo en el comando).
-Simbolo > (Mayor que): Indica líneas que están en el archivo "nuevo" (el segundo que pongo en el comando).

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


🛠️ Solución Paso a Paso

PASO 1: Verificar los archivos
Primero confirmé que los dos archivos estuvieran ahí.
Comando : "ls -l"
# Veo passwords.old y passwords.new


PASO 2: Ejecutar la comparación
Usé el comando "diff" poniendo primero el archivo original y luego el modificado.
Comando : "diff passwords.old passwords.new"

PASO 3: Interpretar el resultado
El comando me mostró algo así:
< BMIOFK... (contraseña vieja)
---
>x2gL... (contraseña nueva)

Entendí que la línea con el simbolo ">" (la de abajo) correspondía al archivo nuevo (passwords.new).

PASO 4: Capturar la contraseña
copie la cadena de caracteres que estaba señalada por la flecha ">" (la que empieza "x2" o la que sea en tu caso). Esa es la contraseña para entrar al Bandit18

La contraseña en mi caso es : "x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO" 