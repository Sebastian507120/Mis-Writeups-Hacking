El reto: Al igual que en el nivel anterior, debia enviar mi contraseña actual a un puerto (30001) para obtener la siguiente. Pero había una trampa: esta vez el servidor usa "cifrado SSL/TLS"

La Lección (Texto Plano vs Cifrado): Aprendí que "nc" (Netcat) no sirve aquí porque envía los datos "denudos" (texto plano). El servidor espera un "apretón de manos" seguro (hanshake) y certificados de seguridad. Si uso "nc", el servidor me cierra la conexión porque no hablo su idioma cifrado.

Necesitaba una herramienta que soporte SSL, y la navaja suiza para esto es OpenSSL.

🛠️ Solución Paso a Paso
PASO 1: Preparar la contraseña Primero, obtuve la contraseña del nivel actual(bandit15) para tenerla lista en el portapapeles
Comando: cat /etc/bandit_pass/bandit15

PASO 2: Conexión Segura con OpenSSL En lugar de "nc", usé el comando "openssl" con el módulo "s_client" (que actúa como un cliente web/seguro).

Sintaxis openssl s_client -connect [host]:[puerto]

Comando: "openssl s_client -connect localhost:30001"

(Nota: "-quiet" es una opción útil para quitar el ruido, pero lo hice sin ella y salió mucha info de certificados).

PASO 3: El intercambio Al ejecutarlo, la pantalla se llenó d einformación técnica sobre certificados y claves de sesión. Esperé a que terminara de cargar y el cursor se detuviera.
Pegué mi contraseña de "bandit15" y presioné "Enter".

PASO 4: Resultado El servidor validó  mi conexión segura y me respondió con la contraseña para el "Nivel 16"
Output: Correct!
[CONTRASEÑA_NIVEL_16]