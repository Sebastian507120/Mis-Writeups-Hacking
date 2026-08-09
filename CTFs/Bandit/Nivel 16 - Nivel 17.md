---
tags:
  - CTF_Bandit
---
📓 **Mis Apuntes: Bandit Nivel 16 → 17**

###  El Reto

El nivel me pedía encontrar un puerto específico en el rango `31000-32000` en mi propia máquina (`localhost`). Había muchos puertos abiertos, pero solo uno hablaba SSL y me daría las credenciales; los demás eran servidores eco (repetían lo que yo enviaba). Además, la credencial esta vez no era una contraseña, sino una llave especial.

###  Concepto Nuevo: Port Scanning y Llaves RSA

- **Port Scanning (Nmap):** Aprendí a usar `nmap` no solo para ver qué puertas están abiertas, sino para identificar qué servicio corre en ella (usando la bandera `-sV`). Esto es vital para no probar puerto por puerto manualmente.

- **Llaves Privadas RSA:** Aprendí que el acceso SSH no siempre es con contraseña escrita. Existe un método más seguro usando un archivo de llave privada (`Private Key`). Quien tiene el archivo, tiene la llave para entrar.


## 🛠️ Solución Paso a Paso

# PASO 1: Escaneo inteligente de puertos

En lugar de probar uno por uno usé `nmap` para escanear el rango y detectar versiones.

- **Comando:** `nmap -p 31000-32000 -sV localhost`

**Desglose:**
 - `nmap:` Herramienta estándar para el descubrimiento de redes y auditoría de seguridad.
 - `-p 31000-32000:` Define el rango exacto de puertos a escanear.
	 - Nota: Si omitimos esta bandera, `nmap` por defecto solo escanea los 1000 puertos más comunes (como el **80**, **443**, **22**), lo cual haría que se pasen por alto los puertos altos utilizados en este reto.
- `-sV:` (**Detección de versión**)
	- Sin esta bandera, `nmap` solo indica si el puerto está `Open` (Abierto)
	- Con esta bandera, la herramienta interactúa con el servicio para identificar qué software y versión está corriendo. Esto permite diferenciar entre los servicios que responden con un simple `echo` y el que responde con `ssl/unknown`.
- **`localhost`**: Objetivo (`Target`) que le indica a `nmap` que escanee la propia máquina local (dirección IP `127.0.0.1`).

# PASO 2: El comando de conexión (`openssl`)
Este comando fue nuestro "navegador" para hablar un idioma cifrado que `netcat` (`nc`) no entiende.

- **Comando:** `openssl s_client -connect localhost:31790 -quiet`

**Desglose:**
- **`-openssl`**: La "navaja suiza" de la criptografía. Se usa para crear certificados, claves y probar conexiones.
    
- **`-s_client`**: Es un **sub-comando** específico. Actúa como un cliente genérico de **SSL/TLS**.
    
    - **Analogía:** Es como usar `telnet` o `nc`, pero `s_client` se encarga automáticamente de todo el proceso de negociación de seguridad (el **handshake**) antes de dejarte enviar datos.
        
- **`-connect localhost:31790`**:
    
    - A diferencia de otros comandos que usan espacios (`nc localhost 30000`), `openssl` suele requerir la bandera explícita `-connect` y el formato `HOST:PUERTO` (con dos puntos).
        
- **`-quiet`**:
    
    - Significa "silencioso".
        
    - Cuando `openssl` se conecta, normalmente imprime mucha información técnica sobre el certificado, el tipo de cifrado, la sesión, etc.
        
    - Al usar `-quiet`, ocultamos toda esa "basura" técnica inicial para ver limpia la respuesta del servidor (en este caso, la clave RSA).
        

> **Nota:** Luego de poner el comando `openssl s_client -connect localhost:31790 -quiet` la línea de abajo se quedará parpadeando; ahí es donde debemos de poner la contraseña que usamos para conectarnos al `bandit16`. Cuando la pongamos y demos **enter** nos aparecerá un tipo de párrafo largo, ese será nuestra contraseña para el `bandit17`, pero hace falta hacer algo aún.

# PASO 3: Guardar la "Llave" (Private Key)

Ese bloque de texto que empieza por `-----BEGIN RSA PRIVATE KEY-----` y termina en `-----END RSA PRIVATE KEY-----` es mi credencial. No es una contraseña para escribir, es un archivo para "portar".

1. Copiamos todo el bloque (incluyendo los guiones del principio y del final).
    
2. Creamos un archivo nuevo en mi máquina local (puedo llamarlo `sshkey17.private`) -> `nvim sshkey17.private`
    
3. Pegamos el contenido dentro y lo guardo.

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

# PASO 4: Permisos de seguridad (El paso crítico)
Si intento usar la llave ahora, SSH me rechazará diciendo que el archivo es "demasiado abierto" (`too open`). Por seguridad, una llave privada solo debe poder ser leída por su dueño.

- **Comando:**`chmod 600 sshkey17.private`

**Desglose:**
- **`chmod`**: (_Change Mode_) Comando para cambiar permisos de archivos.
    
- **`600`**: Es el código numérico de permisos:
    
    - **6 (Dueño):** Lectura y escritura (_Read + Write_).
        
    - **0 (Grupo):** Ningún permiso.
        
    - **0 (Otros):** Ningún permiso.
        
- **Traducción:** _"Solo yo puedo leer y editar este archivo; nadie más puede ni verlo"_.

# PASO 5: Conexión Final (Login)

Ahora que tengo la llave guardada y protegida, entro al siguiente nivel indicándole a SSH que use mi archivo en lugar de pedirme contraseña.

- **Comando:** `ssh -i sshkey17.private bandit17@bandit.labs.overthewire.org -p 2220`

**Desglose:**

- **`-i`**: (_Identity file_) Esta es la bandera mágica. Le dice a SSH: _"No me pidas contraseña, usa este archivo de identidad para autenticarme"_.
    
- **`sshkey17.private`**: La ruta al archivo que creé en el Paso 3.
    

> Ya en este paso estaríamos dentro del `bandit17` :)