📓 Mis Apuntes: Bandit Nivel 18 → 19

El Reto: La puerta Giratoria
Ya tenía a contraseña correcta para entrar al Nivel18 (la que conseguimos comparando los archivos en el nivel anterior). Sin embargo, al intentar conectarme con SSH, ocurría algo frustrante: el servidor me daba la bienvenida y, "inmediatamente, me expulsaba" con un mensaje de "Byebye!"

Parecía que la contraseña estaba mal, pero no era eso. Era una trampa en la configuración del usuario que ejecutaba una orden de "salir" (exit) apenas yo entraba.

Concepto Nuevo: "Bypassing" la Shell (Saltarse la configuración)
Aprendí que SSH es muy flexible. Normalmente, cuando me conecto, el servidor carga una configuración por defecto (que en este caso tenía trampa). Pero descubrí que puedo usar "banderas" (opciones) en el comando SSH para decirle: "ignora ru configuración normal y ábreme una ventana de comandos básica a la fuerza".


🛠️ Solución Paso a Paso

PASO 1: El Intento Fallido (Diagnostico)
Intenté entrar normalmente y fui expulsado.
  -Síntoma: Veía el banner de bienvenida y luego "Connection to bandit.labs.overthewire.org closed."
  -Conclusión: Necesitaba una forma de quedarme adentro sin que se activara el script de expulsión.

PASO 2: El comando "Mágico" (-t /bin/sh)
Modifiqué mi comando SSH para obligar al sevidor a usar un programa especifico (/bin/sh, que es una consola simple) en lugar de la consola tramposa.

El comando que usé:
"sshpass -p 'x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO' ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh"

Desglose: 
- (-t): Significa "Force TTY" (Forzar terminal). Es como decirle al servidor:"Dame un cursor y un teclado si o si".
- /bin/sh : Es el programa de consola más básico. Al pedirlo directamente, me salté la bienvenida tramposa del servidor.

PASO 3: Ya adentro (La búsqueda)
Al ejecutar el comando anterior , no me apareció el nombre "bandit18@bandit:~$" bonito que siempre aparecia, sino un cursor simple. Pero lo importante: !No nos expulsó esta vez¡

Escribí "ls" y vi que había un archivo llamado "readme".


PASO 4: Leer la contraseña
Como ya tenía el control, solo ruve que leer ese archivo
Comando: "cat readme"

Resultado: El servidor me mostró la contraseña para el Nivel 19 que en mi caso es : "cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8"

Ya con la contraseña solo nos queda loguearnos como lo haciamos en niveles anteriores : "sshpass -p 'cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8' ssh bandit19@bandit.labs.overthewire.org -p 2220"