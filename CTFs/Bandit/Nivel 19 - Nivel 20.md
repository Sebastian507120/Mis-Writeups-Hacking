---
tags:
  - CTF_Bandit
---

📓 **Mis Apuntes:** Bandit Nivel 19 → 20 (Método Shell Interactiva)

**El Reto:**
Necesitaba leer la contraseña en `etc/bandit_pass/bandit20`, pero mi usuario no tenía permisos. En mi carpeta había un ejecutable **SUID** llamado `bandit20-do` que pertenecía al usuario **bandit20**. El objetivo normal era usarlo para leer el archivo, pero yo quería ir mas allá: **convertirme** en el usuario **bandit20**.


**Concepto Nuevo:** `Escalada con SUID y "bash -p"`
Aprendí que si un binario tiene permisos **SUID (s)**, se ejecuta como su dueño. Sin embargo, programas como **bash** tienen una medida de seguridad: si detectan que se están  ejecutando con SUID, "se quitan los poderes automáticamente" para evitar ser usados maliciosamente.

- **Solución:** Usar la bandera `-p` **Privileged** .Esto le dice a Bash: **No me quites los poderes, mantén el modo privilegiado**.


# 🛠️ Solución Paso a Paso

### PASO 1: Identificar el vector de ataque
Hice un `ls -l` y encontré el binario `bandit20-do` con permisos **SUID** `-rwsr-x---` pertenecientes a **bandit20**.

### PASO 2: El intento de "convertirse" en el usuario
En lugar de pedirle que leyera un archivo `cat`, intenté abrir una terminal nueva **bash** usando el binario.

  - **Intento fallido:** `./bandit20-do bash`
- **Resultado:** Bash detectó el **SUID** y me quitó los permisos. Seguía siendo **bandit19**.

### PASO 3: La técnica correcta (-p)
Ejecuté el binario pidiendo una Bash Privilegiada

- **Comando:** `./bandit20-do bash -p`
- **Resultado:** El **promt** cambió **bash-4.4$**. Al ejecutar `whoami`, el sistema me confirmó que ahora yo era **bandit20**.

### PASO 4: Obtener la contraseña
Ya con el control total de una terminal como **bandit20**, pude leer el archivo directamente sin trucos extra.
Comando: `cat /etc/bandit_pass/bandit20`

Al realizar este `cat` me devolvio la contraseña del **bandit20** que en mi caso fue : `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

>Recuerden que las contraseñas todos los niveles de bandit cambian 