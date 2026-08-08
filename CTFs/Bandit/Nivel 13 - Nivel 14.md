📓 Mis Apuntes: Bandit Nivel 13 → 14

**El Concepto Nuevo:** Llaves  **SSH**  (SSH Keys) Hasta ahora siempre entraba con contraseña. En este nivel aprendí que también puedo entrar con un archivo de llave `sshkey.private`. Es como tener una tarjeta magnética física: **si tienes el archivo, el servidor te deja pasar sin pedirte clave**.

**El Problema** `(La Trampa):` El nivel me dio la llave privada  y me dijo que me conectara al usuario **bandit14**- Primero intenté desde el mismo servidor `ssh bandit14@localhost -i sshkey.private`

- ❌ **Falló:** El servidor me rechazó diciendo `Connecting from localhost is blocked to conserve resources` Básicamente está prohibido conectarse a uno mismo ahí dentro.


**La Solución:** **Exfiltración de Credenciales** Como no me dejan entrar desde adentro, tuve que robarme la llave para usarla desde afuera (desde mi propia computadora).


# 🛠️ Guía Paso a Paso

### PASO 1: Robar la llave 
(En el servidor de bandit) Entré al nivel 13 y leí el archivo de la llave para copiar su contenido.
**Comando:** `cat sshkey.private`
(Copio todo el bloque de texto desde `-----BEGIN... hasta -----END...`).


### PASO 2: Crear la llave falsa 
( En mi PC/Parrot ) Abrí mi terminal local, creé un archivo nuevo y pegué lo que copié.
**Comando:** `nano clave_bandit14`    
pego el texto, guardo con `Ctrl + o` y salgo con `Ctrl + X`.

### PASO 3: Poner el candado ( Permisos )
SSH es paranoico, Si el archivo de la llave está **abierto** para que cualquiera lo lea, no funciona. Tuve que restringirlo para que solo yo (el dueño) pueda leerlo.
**Comando:** `chmod 600 clave_bandit14`
- **600:** Significa "Yo como dueño puedo leer y escribir para grupos y otros no pueden hacer nada".

### PASO 4: El ataque ( Conexión Remota ) 
Usé la llave robada para entrar directo al **nivel 14** desde mi máquina.
**Comando:** `ssh -i clave_bandit14 bandit14@bandit.labs.overthewire.org -p 2220`.

- **-i:** Indica `identity file` (usa esta llave).

### PASO 5: Obtener la contraseña real Una vez dentro
( Ya como **bandit14** ) fui a buscar la contraseña de texto para no tener que repetir todo este proceso después.
**Comando:** `cat /etc/bandit_pass/bandit14`.


Luego de hacer el `cat` a la ruta de arriba habremos encontrado la contraseña de **bandit14** que en mi caso es : `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`.






































