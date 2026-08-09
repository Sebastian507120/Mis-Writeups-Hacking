📓 **Mis Apuntes:** Bandit Nivel 22 → 23

**El Reto:** EL Nombre Matemático Igual que en el nivel anterior, había una tarea programada (**ceon job**) ejecutándose en el fondo. pero esta vez, el script no guardaba la contraseña en un archivo con nombre fijo, El script "calculaba" el nombre del archivo usando una fórmula matemática **hash** basada en el nombre del usuario.

**Concepto Nuevo:** Simulación de Comandos y Hashing Aprendí que no siempre puede ejecutar el script original (por que no soy usuario **bandit23**). pero si tengo el código fuente, puedo "copiar la lógica" y ejecutarla yo mismo cambiando las variables manualmente para ver qué resultado daría si yo fuera el otro usuario.

- **MD5** `(md5sum):` Una función que convierte cualquier texto en una huella digital única. Si cambias una letra (incluso una mayúscula), la huell a cambia totalmente.

# 🛠️ Solución Paso a Paso

### PASO 1: Localizar el Cron Job 
Fui a la carpeta de configuración y encontré la tarea para el siguiente nivel.

**Comando:** `cat /etc/cron.d/cronjob_bandit23`

Esto me reveló que el script a analizar era `/usr/bin/cronjob_bandit23.sh`

### PASO 2: Analizar la "Fórmula" 
Leí el contenido del script para entender cómo generaba el nombre del archivo oculto.

**Comando:** `cat /usr/bin/cronjob_bandit23.sh`

El código clave era: `mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)` El script toma la frase `I am user [NOMBRE]` y la convierte en un **hash MD5**.

### PASO 3: Simular la ejecución (La conrreción vital) 
Tenía que calcular cuál sería ese hash cuando el usuario es **bandit23**. 
- **Nota:** Cometí un error inicial usando minúsculas, pero corregí copiando la frase exacta del script.

**Comando de simulación:** `echo I am user bandit23 | md5sum | cut -d ' ' -f 1` 
- **Resultado del Hash:** `8ca319486bfbbc3663ea0fbe81326349`

### PASO 3: Obtener la contraseña 
Sabiendo que ese hash era el nombre del archivo, fui a buscarlo a la carpeta temporal `/tmp/`.

**Comando:** `cat /tmp/8ca319486bfbbc3663ea0fbe81326349`

Al ejecutar el comando anterior encontraremos la contraseña que en mi caso fue: `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`.