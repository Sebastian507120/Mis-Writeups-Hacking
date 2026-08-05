📓 Mis Apuntes: Bandit Nivel 21 → 22 (Explorando Cron)

El Reto:
Sabía que un programa se estaba ejecutando automáticamente a intervalos regulares (gracias a Cron). Tenía que investigar la configuración  de esas tareas para descubrir qué comando se estaba  ejecutando y cómo podía aprovecharlo para obtener la contraseña.

Concepto Nuevo: Cron Jobs y Análisis de Scripts

1. "/etc/cron .d/": Es el directorio donde se guardan muchas configuraciones de tareas programadas.
2. Ingenieria Inversa (Básica): En lugare intentar romper el sistema, leí el código  (script) que el administrador creó para entender su lógica y predecir dónde dejaba la información.

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

🛠️ Solución Paso a Paso

PASO 1: Explorar la "Sala de Máquinas" (Cron)
Primero, me dirigí al directorio donde se guardan las tareas programadas y listé el contenido para ver si había algo relacionado con mi nivel
Comandos: 1."cd /etc/cron.d/"
          2."ls -l"

Hallazgo: Encontré un archivo llamado "cronjob_bandit22".

PASO 2: Identificar el Objetivo
Leí el archivo de configuración  del cron para saber qué programa exacto estaba ejecutando.
Comando: "cat cronjob_bandit22"

  -Resultado: La configuración  decía que cada minuto (* * * * *) se ejecutaba el script ubicado en "/usr/bin/cronjob_bandit.sh"

PASO 3: Analizar el Script (Leer el código)
Fui a leer ese script para entender qué hacía internamente con la contraseña.

Comando: "cat /usr/bin/cronjob_bandit22.sh"

  -Análisis del código
    1. "chmod 644 /tmp/t706...": El script le daba permisos de lectura global a un archivo extraño en "/tmp/".
    2. "cat /etc/bandit_pass/bandit22 > /tmp/t706...": El script copiaba la contraseña secreta dentro de ese archivo temporal.

PASO 4: Ejecutar el Robo
Como el script ya había hecho el trabajo de copiar la contraseña a un lugar accesible, solo tuve que ir a leer ese archivo temporal específico que descubrí en el paso anterior

Comando: "cat /tmp/t706lds9S0RqQh9aMcz6ShpAoZKF7fgv"

Resultado: Obtuve la contraseña limpia en pantalla que en mi caso es: "tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q"


~Recuerden que las contraseñas cambian con el tiempo