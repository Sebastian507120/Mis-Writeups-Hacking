📓 **Mis Apuntes:** Bandit Nivel 20 → 21

**El Reto:** Conexión Inversa
En este nivel hay un binario especial llamado **Suconnect**. Este programa funciona **al revés** de lo normal:
  1. Yo le digo un puerto
  2. Él se conecta a ese puerto (actúa como Cliente).
  3. Espera que YO le envíe la contraseña actual.
  4. Si es correcta, él me devuelve la contraseña nueva.

El problema es que necesito estar en dos lugares a la vez: ejecutando el programa y esperando la conexión  para pasarle la contraseña.

**Concepto Nuevo:** Arquitectura Cliente-Servidor **Split View**
Aprendí que para depurar o explorar conexiones de red. es vitar ver ambos lados de la comunicación.

- **Terminal 1** `(El servidor/Listener):` Se queda escuchando esperando una llamada.
- **Terminal 2** `(El cliente/Ejecutor):` Ejecuta el programa que realiza la llamada.


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


# 🛠️ Solución Paso a Paso (Método de 2 Terminales)

### PASO 1: Prepara el entorno  (Dividir la pantalla)
Para replicar el método de S4vitar, necesito dos terminales conectadas al mismo tiempo a **Bandit20**.

- **Implementado:** Abrir dos pestañas en mi consola **Kitty** y conectarme por **SSH** en ambas.

### PASO 2: Configurar el "Oído" (Terminal 1)
En la primero terminal (o panel de abajo), uso `netcat` para crear un servidor que escuche en un puerto libre (por ejemplo, el `4646`).

**Comando:** `nc -nlvp 4646`

- **-l:** Listen (Escuchar).
- **-v:** Verbose (Mostrar detalles del la conexión).
- **-p:** Port (Puerto).

**Estado :** La terminal se queda congelada esperando : `Listening on...`


### PASO 3: Ejecutar la llamada (Terminal 2)

En la segunda terminal  (o panel de arriba), ejecuto el binario **SUID** y le digo que se conecte al puerto donde estoy escuchando.

**Comando:** `./suconnect 4646`

- **Acción:** El programa intenta conectarse a mi otra terminal.


### PASO 4: El intercambio Manual (Handshake)
Aquí ocurre la magia en tiempo real:

1. En la **Terminal 1** veo que dice  `Connection received...`
2. El cursor se queda esperando. (Ahí mismo escribo (o pego) manualmente la contraseña actual (la de **bandit20**)). 
3. Presiono Enter.
4. La **Terminal 2** lee mi contraseña, verifica que es correcta y manda la respuesta a la **Terminal 1**.
5. En la **Terminal 1** aparece la nueva contraseña del **nivel 21** que en mi caso fue: `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`

>Recordar que las contraseñas cambian con el tiempo.