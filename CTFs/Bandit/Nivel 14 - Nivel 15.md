📓 Mis Apuntes: Bandit Nivel 14 → 15

El Reto: El nivel me pedía  que enviara la contraseña actual al puerto 30.000 de mi propia  máquina (localhost) par que el servidor me devolviera la siguiente.

Concepto Nuevo: Netcat y Puertos Aprendí que los programas pueden "escuchar" en puertas específicas llamadas puertos. En este caso, hay un servicio esperando en la puerta 30.000. Para hablar con él, no uso "ssh" (que es para entrar al sistema), sino una herramienta llamada Netcat (nc), que sirve para enviar y recibir datos crudos a través de la red.

🛠️ Solución Paso a Paso

PASO 1: Recuperar la credencial actual Primero necesitaba tener a mano la contraseña del nivel 14 (la que conseguí en el nivel anterior).
Comando: "cat /etc/bandit_pass/bandit14"            #Copio la contraseña que sale en la pantalla

PASO 2: Conectar el puerto Usé "nc" para tocar la puerta del servicio.
- Sintaxis: nc [destino] [puerto]
Comando: "nc localhost 30000"

PASO 3: El Intercambio Al ejecutar el comando, el cursor se quedó esperando. Pegué la contraseña que copié en el paso 1 y presioné "Enter", Inmediatamente, el servicio me respondió con una línea de texto nueva: "BFM" (La contraseña del nivel 15).

PASO 4: Guardar y Salir Copié la nueva contraseña y cerré la conexión (usando Ctrl + C o simplemente cerrando la sesión).

La contraseña del bandit16 en mi caso fue: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

~Recordar que las contraseñas con el paso del tiempo cambian 